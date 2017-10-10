---
title: "SP.NET Core で Razor ページにファイルをアップロードする"
author: guardrex
description: "Razor ページにファイルをアップロードする方法を説明します。"
keywords: "ASP.NET Core,Razor,Razor ページ,IFormFile,ファイル アップロード,fileupload"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 3c5841f8c623f09530b60cc9997281dcb8e3c4f6
ms.sourcegitcommit: 94b7e0f95b92c98b182a93d2b3dc0287e5f97976
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>SP.NET Core で Razor ページにファイルをアップロードする

作成者: [Luke Latham](https://github.com/guardrex)

このセクションで、Razor ページにファイルをアップロードする例を示します。

このチュートリアルの [Razor ページ動画サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)では、小さなファイルのアップロードに最適な、単純なモデル バインディングを利用してファイルをアップロードします。 大きなファイルをストリーミングする方法については、「[Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming)」 (ストリーミングでの大きいファイルのアップロード) を参照してください。

以下の手順では、映画スケジュール ファイルのアップロード機能をサンプル アプリに追加します。 映画スケジュールを表すのが `Schedule` クラスです。 このクラスには、2 つのバージョンのスケジュールが含まれています。 1 つのバージョンは顧客に提供される `PublicSchedule` です。 もう 1 つのバージョンは社員が利用する `PrivateSchedule` です。 いずれも別個のファイルとしてアップロードされます。 このチュートリアルでは、POST が 1 つのページからサーバーに 2 つのファイルをアップロードします。

## <a name="add-a-helper-method-to-upload-files"></a>ファイルをアップロードするヘルパー メソッドを追加する

アップロード済みのスケジュール ファイルを処理するコード重複を回避するために、静的なヘルパー メソッドを先に追加します。 アプリで *Utilities* フォルダーを作成し、次のコンテンツを含む *FileHelpers.cs* ファイルを追加します。 ヘルパー メソッドの `ProcessFormFile` は [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) と [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) を受け取り、ファイルのサイズとコンテンツが含まれる文字列を返します。 コンテンツの種類と長さが確認されます。 ファイルが検証チェックに合格しなければ、エラーが `ModelState` に追加されます。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a>Schedule クラスを追加する

*Models* フォルダーを右クリックします。 **[追加]**、**[クラス]** の順に選択します。 クラスに **Schedule** という名前を付けて、次のプロパティを追加します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

このクラスは `Display` 属性と `DisplayFormat` 属性を使用します。この 2 つの属性は、スケジュール データがレンダリングされるとき、わかりやすい名前を生成し、書式を設定します。

## <a name="update-the-moviecontext"></a>MovieContext を更新する

このスケジュールは、`MovieContext` (*Models/MovieContext.cs*) で `DbSet` を指定します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Schedule テーブルをデータベースに追加する

**[ツール]**、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択し、パッケージ マネージャー コンソール (PMC) を起動します。

![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

PMC で、次のコマンドを実行します。 コマンドによって `Schedule` テーブルがデータベースに追加されます。

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-fileupload-class"></a>FileUpload クラスを追加する

次に、`FileUpload` クラスを追加します。これはスケジュール データを取得するページにバインドされています。 *Models* フォルダーを右クリックします。 **[追加]**、**[クラス]** の順に選択します。 クラスに **FileUpload** という名前を付けて、次のプロパティを追加します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

このクラスには、スケジュールのタイトルのプロパティと 2 つあるスケジュールのバージョンのそれぞれのプロパティが含まれます。 3 つのプロパティはすべて必須であり、タイトルの長さは 3 ～ 60 文字にする必要があります。

## <a name="add-a-file-upload-razor-page"></a>ファイル アップロード Razor ページを追加する

*Pages* フォルダーで、*Schedules* フォルダーを作成します。 *Schedules* フォルダーで、*Index.cshtml* という名前のページを作成します。次のコンテンツでスケジュールをアップロードするためのページです。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

各フォーム グループには、**\<label>** が含まれます。これは各クラス プロパティの名前を表示します。 `FileUpload` モデルの `Display` 属性は、ラベルの表示値を与えます。 たとえば、`UploadPublicSchedule` プロパティの表示名は `[Display(Name="Public Schedule")]` で設定されます。そのため、フォームがレンダリングされると、ラベルに "Public Schedule" と表示されます。

各フォーム グループには、検証 **\<span>** が含まれます。 ユーザーの入力が `FileUpload` クラスに設定されているプロパティ属性の要求を持たさない場合、あるいは `ProcessFormFile` メソッド ファイルの検証チェックで不合格になった場合、モデルは検証に失敗します。 モデルが検証に失敗すると、ヒントが表示されます。 たとえば、`Title` プロパティであれば、`[Required]` や `[StringLength(60, MinimumLength = 3)]` という注釈が表示されます。 ユーザーがタイトルを入力しないと、値が必須であるというメッセージが表示されます。 ユーザーが 3 文字より少ないか、60 文字より多い値を入力した場合、値の長さが正しくないというメッセージが表示されます。 コンテンツのないファイルが指定された場合、ファイルが空であるというメッセージが表示されます。

## <a name="add-the-code-behind-file"></a>分離コード ファイルを追加する

分離コード ファイル (*Index.cshtml.cs*) を *Schedules* フォルダーに追加します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

ページ モデル (*Index.cshtml.cs* の `IndexModel`) は `FileUpload` クラスをバインドします。

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

このモデルはまた、スケジュールの一覧 (`IList<Schedule>`) を利用し、ページのデータベースに保存されているスケジュールを表示します。

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

ページが `OnGetAsync` で読み込まれると、データベースから `Schedules` が入力され、読み込まれたスケジュールの HTML テーブルが生成されます。

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

フォームがサーバーに投稿されると、`ModelState` がチェックされます。 無効な場合、`Schedule` が再ビルドされます。ページがレンダリングされ、ページ検証に失敗した理由を伝える検証メッセージが表示されます。 有効な場合、`FileUpload` プロパティが *OnPostAsync* で使用され、バージョンが 2 つあるスケジュール ファイルがアップロードされ、データを保存するための新しい `Schedule` オブジェクトが作成されます。 スケジュールがデータベースに保存されます。

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>ファイル アップロード Razor ページをリンクする

*_Layout.cshtml* を開き、ファイル アップロード ページにアクセスするためのリンクをナビゲーション バーに追加します。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>スケジュール削除を確定するためのページを追加する

ユーザーがスケジュールを削除するボタンをクリックしたとき、削除を取り消す機会をユーザーに与えます。 削除確定ページ (*Delete.cshtml*) を *Schedules* フォルダーに追加します。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

分離コード ファイル (*Delete.cshtml.cs*) は、要求のルート データの `id` によって特定される 1 つのスケジュールを読み込みます。 *Delete.cshtml.cs* ファイルを *Schedules* フォルダーに追加します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

`OnPostAsync` メソッドは、その `id` によるスケジュール削除を処理します。

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

スケジュールが削除されると、`RedirectToPage` はユーザーをスケジュール *Index.cshtml* ページに戻します。

## <a name="the-working-schedules-razor-page"></a>Schedules Razor ページの実際の動作

ページが読み込まれると、スケジュール タイトルのラベルと情報、パブリック スケジュール、プライベート スケジュール、送信ボタンがレンダリングされます。

![初回読み込み時の Schedules Razor ページ。検証エラーや空のフィールドに関する表示はまだありません。](uploading-files/_static/browser1.png)

フィールドに何も入力せずに **[アップロード]** ボタンを選択すると、モデルの `[Required]` 属性に違反することになります。 `ModelState` が無効です。 検証エラー メッセージが表示されます。

![各入力コントロールの隣に検証エラー メッセージが表示されます。](uploading-files/_static/browser2.png)

**[タイトル]** フィールドに 2 つの文字を入力します。 タイトルは 3 ～ 60 文字にする必要があるという検証メッセージが表示されます。

![タイトルの検証メッセージが変わりました](uploading-files/_static/browser3.png)

1 つまたは複数のスケジュールをアップロードすると、**[Loaded Schedules]\(読み込まれたスケジュール\)** セクションに、読み込まれたスケジュールがレンダリングされます。

![読み込まれたスケジュールのテーブル。各スケジュールのタイトル、アップロード日付 (UTC)、パブリック バージョン ファイルのサイズ、プライベート バージョン ファイルのサイズ](uploading-files/_static/browser4.png)

この **[削除]** リンクをクリックすると、削除確定ビューが表示されます。削除を確定するか、取り消す機会が与えられます。

## <a name="troubleshooting"></a>トラブルシューティング

`IFormFile` アップロードの問題を解決する方法については、「[ASP.NET でのファイルのアップロード](xref:mvc/models/file-uploads#troubleshooting)」の「トラブルシューティング」セクションを参照してください。

このたびは、この Razor ページの紹介を最後までお読みいただきありがとうございました。 コメントを残していただければ幸いです。 このチュートリアルの後は、「[Getting started with MVC and EF Core](xref:data/ef-mvc/intro)」 (MVC と EF Core の概要) にお進みいただくことが推奨されます。

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core でのファイルのアップロード](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[前: 検証](xref:tutorials/razor-pages/validation)
