---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 並べ替え、フィルター処理、および ASP.NET MVC アプリケーション (3/10) で Entity Framework でページング |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8bea3d4bc19a5a47240abeb2cc015116814a8fdf
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911820"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>並べ替え、フィルター処理、および ASP.NET MVC アプリケーション (3/10) で Entity Framework でのページング
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでの基本的な CRUD 操作の web ページのセットを実装した`Student`エンティティ。 このチュートリアルでは、並べ替え、フィルター処理、およびページング機能を追加します、**学生**インデックス ページです。 単純なグループ化を実行するページも作成します。

次の図は、作業が終了したときにページがどのように表示されるかを示しています。 列見出しとは、ユーザーがクリックしてその列で並べ替えを行うことができるリンクです。 列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序が切り替えられます。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Students インデックス ページに列の並べ替えリンクを追加する

Student インデックス ページに並べ替えを追加、変更します、`Index`のメソッド、`Student`コント ローラーのコードを追加し、`Student`ビューにインデックスします。

### <a name="add-sorting-functionality-to-the-index-method"></a>並べ替えは Index メソッドに機能を追加します。

*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

このコードは、URL 内の文字列から `sortOrder` パラメーターを受け取ります。 クエリ文字列の値は、アクション メソッドのパラメーターとして、ASP.NET MVC によって提供されます。 パラメータは、"Name" または "Date" という文字列で、その後に必要に応じてアンダースコアと降順を指定する文字列 "desc" が続きます。 既定の並べ替え順序は昇順です。

インデックス ページが初めて要求されたときには、クエリ文字列はありません。 受講者がで昇順に表示される`LastName`でフォールスルー ケースによって確立されると、既定値は、`switch`ステートメント。 ユーザーが列見出しハイパーリンクをクリックすると、適切な `sortOrder` 値がクエリ文字列で提供されます。

2 つ`ViewBag`変数を使用するビューは、適切なクエリ文字列の値を持つ列見出しのハイパーリンクを構成できます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

これらは、三項ステートメントです。 1 つ目の指定された場合、`sortOrder`パラメーターが null または空の場合、`ViewBag.NameSortParm`に設定する必要があります"名\_desc"、それ以外の空の文字列に設定する必要があります。 これらの 2 つのステートメントを使用して、次のようにビューで列見出しのハイパーリンクの設定することができます。

| 既定の並べ替え順 | 姓のハイパーリンク | 日付のハイパーリンク |
| --- | --- | --- |
| 姓の昇順 | descending | ascending |
| 姓の降順 | ascending | ascending |
| 日付の昇順 | ascending | descending |
| 日付の降順 | ascending | ascending |

メソッドを使用して[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)を並べ替え列を指定します。 コードを作成、 [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)する前に変数、`switch`ステートメントでは、変更で、`switch`ステートメント、および呼び出し、`ToList`メソッドの後、`switch`ステートメント。 `IQueryable` 変数を作成および変更するときに、データベースに送信されるクエリはありません。 変換するまで、クエリが実行されない、`IQueryable`などのメソッドを呼び出すことによって、コレクションにオブジェクト`ToList`します。 そのため、このコード結果まで実行されない 1 つのクエリ、`return View`ステートメント。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>列見出しを Student インデックス ビューへのハイパーリンクの追加します。

*Views\Student\Index.cshtml*、置換、`<tr>`と`<th>`見出し行を強調表示されたコードの要素。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

このコードは、情報を使用して、`ViewBag`適切なクエリを含むハイパーリンクを設定するプロパティの文字列値。

ページを実行し、をクリックして、**姓**と**加入契約日**その並べ替えを確認する列見出しに動作します。

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

クリックした後、**姓**見出しで、学生は最後の名前の降順に表示されます。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Students インデックス ページに検索ボックスを追加します。

Students インデックス ページにフィルターを追加するには、テキスト ボックスと送信ボタンをビューに追加し、`Index` メソッドで対応する変更を行います。 テキスト ボックスを使用して、姓と名のフィールドに検索する文字列を入力できます。

### <a name="add-filtering-functionality-to-the-index-method"></a>Index メソッドにフィルター処理機能を追加します。

*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード (変更が強調表示されます)。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

`searchString` パラメーターを `Index` メソッドに追加しました。 また、LINQ ステートメントに追加した、 `where` clausethat 名または姓を持つは、検索文字列を含む受講者のみを選択します。 インデックス ビューに追加するテキスト ボックスから検索する文字列値を受信します。ステートメントを追加する、[場所](https://msdn.microsoft.com/library/bb535040.aspx)句は、検索する値がある場合にのみ実行されます。

> [!NOTE]
> 多くの場合は、Entity Framework のエンティティ セットまたはメモリ内コレクションの拡張メソッドとして同じメソッドを呼び出すことができます。 結果は、通常は同じが、場合によっては異なる場合があります。 .NET Framework の実装など、`Contains`メソッドが、空の文字列を渡しますが、Entity Framework provider for SQL Server Compact 4.0 は、空の文字列には、ゼロ行を返すときに、すべての行を返します。 例のコードではそのため (配置すること、`Where`内のステートメント、`if`ステートメント) は、すべてのバージョンの SQL Server と同じ結果を取得することを確認します。 .NET Framework の実装も、`Contains`メソッドは、既定では、大文字小文字の比較を実行しますが、Entity Framework の SQL Server プロバイダーは、既定では大文字の比較を実行します。 そのため、呼び出し、`ToUpper`メソッド、テストを明示的に大文字をにより返されます、リポジトリを使用するには、後でコードを変更すると、結果は変化しません、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。 (`IEnumerable` コレクションに対して `Contains` メソッドを呼び出したときには、.NET Framework の実装を取得します。`IQueryable` オブジェクトに対して呼び出したときには、データベース プロバイダーの実装を取得します)。


### <a name="add-a-search-box-to-the-student-index-view"></a>Students インデックス ビューに [Search] ボックスを追加する

*Views\Student\Index.cshtml*、開始する直前に強調表示されたコードを追加`table`キャプション、テキスト ボックスを作成するにはタグと**検索**ボタンをクリックします。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

ページを実行し、検索文字列を入力し をクリックして**検索**フィルターが動作していることを確認します。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

URL に「、」検索文字列、これをこのページにブックマークを設定した場合はありません一覧を取得するフィルター選択されたブックマークを使用する場合は意味が含まれていないことを確認します。 変更を**検索**フィルター条件のチュートリアルの後半でクエリ文字列を使用するボタン。

## <a name="add-paging-to-the-students-index-page"></a>Students インデックス ページにページングを追加します。

Students インデックス ページには、ページングを追加するをインストールして起動します、 **PagedList.Mvc** NuGet パッケージ。 その後で追加の変更、`Index`メソッドへのページングのリンクを追加し、`Index`ビュー。 **PagedList.Mvc**優れた多くのページングと ASP.NET MVC でのパッケージの並べ替えの 1 つとここでその使用のためのものとしてその他のオプションの上の推奨事項ではなく、例としてのみです。 次の図は、ページングのリンクを示します。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet パッケージをインストールします。

NuGet **PagedList.Mvc**パッケージが自動的にインストール、 **PagedList**依存関係としてパッケージします。 **PagedList**パッケージのインストール、`PagedList`のコレクションの種類と拡張子メソッド`IQueryable`と`IEnumerable`コレクション。 拡張メソッドが 1 つのデータのページを作成、`PagedList`コレクションのうち、`IQueryable`または`IEnumerable`、および`PagedList`いくつかのプロパティとメソッドをページングを容易にするコレクションを提供します。 **PagedList.Mvc**パッケージは、ページング ボタンを表示するページング ヘルパーをインストールします。

**ツール**メニューの  **NuGet パッケージ マネージャー**し**ソリューションの NuGet パッケージの管理**します。

**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして、**オンライン**左側のタブし、検索ボックスに「ページ」を入力します。 表示された場合、 **PagedList.Mvc**パッケージで、**インストール**します。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

**プロジェクトの選択**ボックスで、 **OK**します。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Index メソッドにページング機能を追加します。

*Controllers\StudentController.cs*、追加、`using`のステートメント、`PagedList`名前空間。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

`Index` メソッドを次のコードで置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

このコードを追加、`page`パラメーター、現在の並べ替え順序パラメーター、およびメソッドのシグネチャを次に示すように、現在のフィルター パラメーター。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

最初にページが表示されるとき、またはユーザーがページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。 ページングのリンクがクリックされた場合、`page`変数を表示するページ番号が含まれます。

`A ViewBag` プロパティは、このページングの中に同じ並べ替え順序を維持するために、ページング リンクに含める必要があるため、ビューで、現在の並べ替え順序を提供します。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

別のプロパティ、 `ViewBag.CurrentFilter`、現在のフィルター文字列でビューを提供します。 ページング中にフィルターの設定を維持するために、ページングのリンクにこの値を含める必要があり、ページが再表示されるときに、この値をテキスト ボックスに復元する必要があります。 ページングの中に検索文字列を変更した場合は、新しいフィルターのために別のデータが表示されるため、ページを 1 にリセットする必要があります。 テキスト ボックスに値を入力し、[送信] ボタンが押されたときに、検索文字列が変更されます。 その場合は、`searchString`パラメーターが null ではありません。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

メソッドの最後に、`ToPagedList`拡張メソッド、受講者を`IQueryable`オブジェクトがページングをサポートするコレクション型の受講者の 1 つのページに学生クエリを変換します。 その学生の 1 つのページは、ビューに渡されます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` メソッドは、ページ番号を受け取ります。 2 つの疑問符を表す、 [null 合体演算子](https://msdn.microsoft.com/library/ms173224.aspx)します。 null 合体演算子は null 許容型の既定値を定義します。式 `(page ?? 1)` は、値がある場合は `page` の値を返し、`page` が null の場合は 1 を返すことを意味します。

### <a name="add-paging-links-to-the-student-index-view"></a>Student インデックス ビューにページングのリンクを追加します。

*Views\Student\Index.cshtml*、既存のコードを次のコードに置き換えます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

ページの上部にある `@model` ステートメントは、ビューが `List` オブジェクトの代わりに `PagedList` オブジェクトを取得するようになったことを指定します。

`using`ステートメント`PagedList.Mvc`ページング ボタンの MVC ヘルパーにアクセスします。

コードのオーバー ロードを使用して[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)することを指定する、 [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)します。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

既定の[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)フォーム データをパラメーターとして渡されると、URL ではなく HTTP メッセージの本文にクエリ文字列は、投稿を送信します。 HTTP GET を指定すると、フォーム データがクエリ文字列として URL で渡され、ユーザーが URL をブックマークできるようになります。 [HTTP GET を使用するための W3C のガイドライン](http://www.w3.org/2001/tag/doc/whenToUseGet.html)アクションは結果として更新しない場合は、GET を使用するように指定します。

テキスト ボックスは、新しいページをクリックすると、現在の検索文字列を表示できるように、現在の検索文字列で初期化されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

列ヘッダーへのリンクは、フィルターの結果内でユーザーが並べ替えられるように、クエリ文字列を使用してコントローラーに現在の検索文字列を渡します。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

ページの現在のページとの合計数が表示されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

表示するページがない場合は、「の 0 ページ 0」が表示されます。 (その場合、ページ番号は、ページ数より大きいため、 `Model.PageNumber` 1 に設定されてと`Model.PageCount`は 0 です)。

によってページング ボタンが表示されます、`PagedListPager`ヘルパー。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager`ヘルパーはさまざまな Url を含むおよびスタイル設定、カスタマイズ可能なオプションを提供します。 詳細については、次を参照してください。 [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) 、GitHub サイトでします。

ページを実行します。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

異なる並べ替え順でページングのリンクをクリックし、ページングが機能することを確認します。 その後で、検索文字列を入力して、ページングをもう一度試し、並べ替えとフィルター処理を使用してもページングが正しく機能することを確認します。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>作成、学生の統計情報を表示するページについて

Contoso University web サイトのページについて、登録日付ごとに登録した受講者の数を表示します。 これには、グループ化とグループに関する簡単な計算が必要です。 これを実行するためには、次の手順を実行します。

- ビューに渡す必要があるデータのビュー モデル クラスを作成します。
- 変更、`About`メソッドで、`Home`コント ローラー。
- 変更、`About`ビュー。

### <a name="create-the-view-model"></a>ビュー モデルを作成します。

作成、 *ViewModels*フォルダー。 クラス ファイルを追加、そのフォルダー内*EnrollmentDateGroup.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Home コントローラーを変更する

*HomeController.cs*、次の追加`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

クラスの中かっこの直後に、データベース コンテキストのクラスの変数を追加します。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

`About` メソッドを次のコードで置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ ステートメントは、登録日で受講者エンティティをグループ化し、各グループ内のエンティティの数を計算して、結果を `EnrollmentDateGroup` ビュー モデル オブジェクトのコレクションに格納します。

追加、`Dispose`メソッド。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>About ビューを変更する

コードに置き換えます、 *Views\Home\About.cshtml*を次のコード ファイル。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

アプリを実行し、をクリックして、**について**リンク。 登録の日付ごとの学生の数が、テーブルに表示されます。

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>省略可能: Windows Azure へのアプリをデプロイします。

これまでに、アプリケーションを実行したローカル IIS Express で開発用コンピューターにします。 他のユーザーに、インターネット経由で使用できるように、web ホスティング プロバイダーにデプロイがあります。 このチュートリアルのオプションのセクションで展開に Windows Azure の Web サイトに。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Code First Migrations を使用して、データベースを展開するには

データベースを展開するには、Code First Migrations を使用します。 Visual Studio から展開の設定を構成するために使用する発行プロファイルを作成するときに、というラベルの付いたチェック ボックスをオンします**実行 Code First Migrations (アプリケーションの起動時に実行)** します。 この設定により、自動的にアプリケーションを構成する展開プロセス*Web.config* Code First を使用するように、移行先サーバー上のファイル、`MigrateDatabaseToLatestVersion`初期化子クラス。

Visual Studio は行いませんデータベースで何か、展開プロセス中にします。 デプロイされたアプリケーションでは、デプロイ後に最初に、データベースにアクセス、Code First 自動的にデータベースを作成またはデータベース スキーマを最新バージョンに更新します。 アプリケーションは、移行を実装している場合`Seed`メソッド、メソッドの実行後、データベースが作成されるか、スキーマを更新します。

使用して移行`Seed`メソッドは、テスト データを挿入します。 運用環境にデプロイする、変更する必要があります、`Seed`メソッドを実稼働データベースに挿入するデータの挿入のみであるためです。 たとえば、現在のデータ モデルで開発用データベースに実際のコースが架空の受講者がある可能性があります。 記述することができます、`Seed`メソッドは負荷が開発では、両方とも、実稼働環境に展開する前に、架空の受講者をし、コメントをします。 したり、作成することができます、`Seed`のみのコースを読み込むし、アプリケーションの UI を使用して手動でテスト データベースで架空の受講者を入力します。

### <a name="get-a-windows-azure-account"></a>Windows Azure アカウントを取得します。

Windows Azure アカウントが必要があります。 1 つをいない場合は、ほんの数分で無料試用版アカウントを作成できます。 詳細については、次を参照してください。 [Windows Azure の無料試用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Windows Azure での web サイトと SQL database を作成します。

Windows Azure の Web サイトは、Windows Azure の他のクライアントと共有されている仮想マシン (Vm) で実行されることを意味する共有ホスティング環境で実行されます。 共有ホスティング環境は、クラウドで開始する低コスト方法です。 後で、web トラフィックが増加すると、アプリケーションは専用 Vm 上で実行して、ニーズを満たすためにスケールできます。 複雑なアーキテクチャが必要な場合、Windows Azure のクラウド サービスに移行することができます。 クラウド サービスは、必要に応じて構成できる専用の Vm で実行します。

Windows Azure SQL Database とは、SQL Server テクノロジに基づいて構築されたクラウド ベースのリレーショナル データベース サービスです。 ツールおよび SQL Server で動作するアプリケーションは、SQL database でも動作します。

1. [Windows Azure 管理ポータル](https://manage.windowsazure.com/)、 をクリックして**Websites**で左側のタブをクリック**新規**します。

    ![管理ポータルで新しいボタン](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. クリックして**カスタム作成**です。

    ![管理ポータルでデータベースのリンクを作成します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **新しい Web サイト - カスタム作成**ウィザードが開きます。
3. **新しい Web サイト**手順と、ウィザードの文字列を入力、 **URL**ボックス、アプリケーションの一意の URL として使用します。 完全な URL は、ここで入力とテキスト ボックスの横に表示されるサフィックスで構成されます。 図に、"ConU"を示しますが、別のアカウントを選択する必要がありますので、その URL が実行される可能性があります。

    ![管理ポータルでデータベースのリンクを作成します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. **リージョン**ドロップダウン リストで、近くのリージョンを選択します。 この設定では、web サイトが実行されるデータ センターを指定します。
5. **データベース**ドロップダウン リストで、選択**無料の 20 MB SQL database を作成する**します。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. **DB 接続文字列名**、入力*SchoolContext*します。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. ボックスの下部にある右を指している矢印をクリックします。 ウィザード、**データベース設定**手順。
8. **名前**ボックスに、入力*ContosoUniversityDB*します。
9. **Server**ボックスで、**新しい SQL Database server**します。 または、以前にサーバーを作成する場合は、ドロップダウン リストからそのサーバーを選択できます。
10. 管理者が入力**ログイン名**と**パスワード**します。 選択した場合**新しい SQL Database server**既存の名前とパスワードでは入力は、データベースにアクセスするときに後で使用するパスワードを今すぐを定義して新しい名前を入力してください。 以前に作成したサーバーを選択した場合は、そのサーバーの資格情報を入力します。 このチュートリアルでは、選択したしません、***詳細***チェック ボックスをオンします。 ***詳細***オプションを使用するデータベースを設定する[照合順序](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)します。
11. 同じ選択**リージョン**で選択した web サイト。
12. 完了することを示すために、ボックスの右下にあるチェック マークをクリックします。   
  
    ![データベースの設定手順は、新しい Web サイトで-データベースとともに作成ウィザード](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    次の図は、既存の SQL Server とログインを使用します。   
  
    ![データベースの設定手順は、新しい Web サイトで-データベースとともに作成ウィザード](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    管理ポータル Web サイトのページに戻り、**状態**列は、サイトが作成されていることを示しています。 (通常、1 分未満)、しばらくしてから、**状態**列は、サイトが正常に作成されたことを示しています。 自分のアカウントで所有するサイトの数を横に表示、左側のナビゲーション バーで、 **Websites**アイコン、およびデータベースの数が横に表示されます、 **SQL データベース**アイコン。

## <a name="deploy-the-application-to-windows-azure"></a>Windows Azure へのアプリケーションをデプロイします。

1. Visual Studio でのプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択と**発行**コンテキスト メニュー。  
  
    ![プロジェクトのコンテキスト メニューを発行します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. **プロファイル**のタブ、 **Web の発行**ウィザード、をクリックして**インポート**します。  
  
    ![発行設定のインポート](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. 以前、Windows Azure サブスクリプションを Visual Studio に追加していない場合は、次の手順を実行します。 これらの手順で追加、サブスクリプション下で、ドロップダウン リストを一覧表示するため**Windows Azure の web サイトからのインポート**web サイトが含まれます。

    a.  **発行プロファイルのインポート**ダイアログ ボックスで、をクリックして**Windows Azure の web サイトからのインポート**、順にクリックします**Windows Azure サブスクリプションの追加**します。

    ![Windows Azure サブスクリプションを追加します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b.  **Windows Azure サブスクリプションのインポート**ダイアログ ボックスで、をクリックして**サブスクリプション ファイルのダウンロード**します。

    ![サブスクリプション ファイルのダウンロード](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. ブラウザー ウィンドウでは、保存、 *.publishsettings*ファイル。

    ![.publishsettings ファイルをダウンロードします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > セキュリティ -、 *publishsettings*ファイルには、Windows Azure サブスクリプションとサービスの管理に使用される (エンコードされていない) 資格情報が含まれています。 このファイルのセキュリティのベスト プラクティスは、ソース ディレクトリの外に一時的に保存するため (たとえば、 *libraries \documents*フォルダー)、してから、インポートが完了したら削除します。 アクセスした悪意のあるユーザー、`.publishsettings`ファイルは、編集、作成、および Windows Azure のサービスを削除します。

    d. **Windows Azure サブスクリプションのインポート**ダイアログ ボックスで、をクリックして**参照**に移動し、 *.publishsettings*ファイル。

    ![サブスクリプションをダウンロードします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. **[インポート]** をクリックします。

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. **発行プロファイルのインポート**ダイアログ ボックスで、 **Windows Azure の web サイトからのインポート**ボックスの一覧から web サイトを選択し、クリックして**OK**します。  
  
    ![発行プロファイルのインポート](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. **接続**] タブで [**接続の検証**設定が正しいことを確認します。  
  
    ![接続を検証します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. 横に緑色のチェック マークが表示、接続が検証されると、**接続の検証**ボタンをクリックします。 **[次へ]** をクリックします。  
  
    ![検証が成功した接続](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. 開く、**リモート接続文字列**下のドロップダウン リスト**SchoolContext**作成したデータベースの接続文字列を選択します。
8. 選択**実行 Code First Migrations (アプリケーションの起動時に実行)** します。
9. オフに**実行時にこの接続文字列を使用して**の**UserContext (DefaultConnection)**、このアプリケーションは、メンバーシップ データベースを使用していないためです。   
  
    ![[設定] タブ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. **[次へ]** をクリックします。
11. **プレビュー** ] タブで [**プレビューの開始**します。  
  
    ![[プレビュー] タブの [開始] ボタン](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    タブには、サーバーにコピーされるファイルの一覧が表示されます。 プレビューを表示するアプリケーションを発行する必要はありませんが、便利な機能に注意してください。 この場合、表示されるファイルの一覧に何もする必要はありません。 次に、このアプリケーションをデプロイするとき、変更されたファイルのみがこの一覧になります。  
  
    ![StartPreview ファイルの出力](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. **[発行]** をクリックします。  
    Visual Studio では、Windows Azure サーバーにファイルをコピーするプロセスを開始します。
13. **出力**ウィンドウが実行されたどのようなデプロイ操作を示しています。 され、展開が正常に完了を報告します。  
  
    ![展開の成功を示す出力ウィンドウ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. デプロイが成功、既定のブラウザーは自動的にデプロイされた web サイトの URL を開きます。  
    作成したアプリケーションは、クラウドで実行されているようになりました。 受講者 タブをクリックします。  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

この時点で、 *SchoolContext*を選択したので、Windows Azure SQL Database でデータベースが作成されている**実行 Code First Migrations (アプリの起動時に実行)** します。 *Web.config*デプロイされた web サイトのファイルが変更されているように、 [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初期化子は、最初に、コードの読み取りまたはデータベースにデータを書き込みを実行 (選択したときに発生、**学生** タブ)。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

展開プロセスは、新しい接続文字列を作成することも *(SchoolContext\_DatabasePublish*)、データベース スキーマを更新して、データベースをシード処理を使用する Code First Migrations に対する。

![Database_Publish 接続文字列](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection* (これは、このチュートリアルでは使用しません)、メンバーシップ データベースの接続文字列です。 *SchoolContext* ContosoUniversity データベースの接続文字列です。

デプロイされたバージョンので、自分のコンピューター上の Web.config ファイルを検索する*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*します。デプロイされているアクセスできる*Web.config* FTP を使用してファイル自体。 手順については、次を参照してください。 [Visual Studio を使用して ASP.NET Web 展開: コードの更新を展開する](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)します。 始まる手順"、FTP ツールを使用するには、するには、次の 3 つが必要です: FTP の URL、ユーザー名、およびパスワード。"。

> [!NOTE]
> Web アプリの URL を見つけた他のユーザー データを変更できるように、セキュリティを実装しません。 Web サイトをセキュリティで保護する方法の詳細については、次を参照してください。[メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC アプリを Windows Azure の Web サイトにデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 他の人が、Windows Azure 管理ポータルを使用して、サイトを使用するようにまたは**サーバー エクスプ ローラー** Visual Studio で、サイトを停止します。


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>コードの最初の初期化子

展開に関するセクションで説明しました、 [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初期化子が使用されています。 最初のコードなど、使用できるその他の初期化子を提供[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (既定)、 [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx)と[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)します。 `DropCreateAlways`初期化子は、単体テストの条件の設定に役立ちます。 独自の初期化子を記述することもでき、明示的に、アプリケーションから読み取るか、データベースに書き込むまで待機するたくない場合、初期化子を呼び出すことができます。 初期化子の包括的な説明については、この書籍の第 6 章を参照してください。 [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do)を Julie Lerman が Rowan Miller が。

## <a name="summary"></a>まとめ

このチュートリアルでは、データ モデルを作成して、基本的な CRUD、並べ替え、フィルター、ページング、および機能をグループ化を実装する方法を説明しました。 次のチュートリアルでは、データ モデルを展開してより高度なトピックを見てが始めます。

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [次へ](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
