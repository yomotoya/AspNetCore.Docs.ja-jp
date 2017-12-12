---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "並べ替え、フィルター、および Entity Framework、ASP.NET MVC アプリケーションでのページング |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8d11bf47f8c43040ef30d7132f0bb756748dbacd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>並べ替え、フィルター、および Entity Framework、ASP.NET MVC アプリケーションでのページング
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。


前のチュートリアルでは、基本的な CRUD 操作するための web ページのセットを実装`Student`エンティティです。 このチュートリアルでは、並べ替え、フィルター、およびページング機能を追加します、**受講者**インデックス ページです。 単純なグループ化を実行するページを作成することもあります。

次の図は、ページがどのようにしたらを示します。 列見出しとは、その列で並べ替えを行うユーザーがクリックするリンクです。 列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序を切り替えます。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>受講者インデックス ページに列の並べ替えのリンクを追加します。

学生インデックス ページに並べ替えを追加するを変更、`Index`のメソッド、`Student`コント ローラーのコードを追加し、`Student`ビューにインデックスをします。

### <a name="add-sorting-functionality-to-the-index-method"></a>並べ替え、インデックス メソッドに機能を追加します。

*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

このコードを受け取る、 `sortOrder` URL 内のクエリ文字列からのパラメーターです。 クエリ文字列の値は、アクション メソッドにパラメーターとして、ASP.NET MVC によって提供されます。 パラメーターが"Name"または「日付」、必要に応じて後にアンダー スコアと降順の順序を指定するには、"desc"文字列を文字列になります。 既定の並べ替え順序は昇順です。

インデックス ページが要求されると、最初にクエリ文字列はありません。 受講者がで昇順に表示される`LastName`、フォール スルー大文字小文字によって設定される既定値は、`switch`ステートメントです。 ユーザーが列見出し、ハイパーリンク、適切な`sortOrder`クエリ文字列の値を指定します。

2 つ`ViewBag`変数を使用するビューは、適切なクエリ文字列の値を列見出しのハイパーリンクを構成できます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

これらは、三項ステートメントです。 最初の 1 つを指定する場合、`sortOrder`パラメーターが null または空で、`ViewBag.NameSortParm`に設定する必要があります"名\_desc"、それ以外の空の文字列に設定する必要があります。 これら 2 つのステートメントでは、ビュー、列見出しのハイパーリンクの次のように設定を有効にします。

| 現在の並べ替え順序 | 名の最後のハイパーリンク | 日付のハイパーリンク |
| --- | --- | --- |
| 最後の名前昇順 | descending | ascending |
| 最後の名前が降順 | ascending | ascending |
| 日付の昇順 | ascending | descending |
| 日付 (降順) | ascending | ascending |

メソッドを使用して[LINQ to Entities](https://msdn.microsoft.com/en-us/library/bb386964.aspx)を並べ替え列を指定します。 コードを作成、 [IQueryable](https://msdn.microsoft.com/en-us/library/bb351562.aspx)する前に変数、`switch`ステートメントでは、変更で、`switch`ステートメント、および呼び出し、`ToList`メソッドした後に、`switch`ステートメントです。 作成および変更するときに`IQueryable`変数、クエリがないデータベースに送信します。 変換するまでに、クエリが実行されていません、`IQueryable`などのメソッドを呼び出すことで、コレクションにオブジェクト`ToList`です。 そのため、このコードなりますまで実行されない 1 つのクエリで、`return View`ステートメントです。

それぞれの並べ替え順序のさまざまな LINQ ステートメントを記述する代わりに、LINQ ステートメントを動的に作成できます。 動的な LINQ の概要については、次を参照してください。[動的 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)です。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>列見出しを学生インデックス ビューへのハイパーリンクの追加します。

*Views\Student\Index.cshtml*、置換、`<tr>`と`<th>`の見出し行を強調表示されたコードが要素。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

このコード内の情報を使用して、`ViewBag`適切なクエリでハイパーリンクを設定するプロパティの文字列値です。

ページを実行し、をクリックして、**姓**と**登録日**を並べ替えることを確認する列見出しが動作します。

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

クリックした後、**姓**見出し、受講者が降順で表示される最後の名前。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>受講者インデックス ページに検索ボックスを追加します。

受講者インデックス ページにフィルターを追加するにをビューにテキスト ボックスと [送信] ボタンを追加しで対応する変更を加え、`Index`メソッドです。 テキスト ボックスを使用すると、姓と名の最後のフィールドで検索する文字列を入力できます。

### <a name="add-filtering-functionality-to-the-index-method"></a>Index メソッドにフィルター処理機能を追加します。

*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード (変更が強調表示されます)。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

追加した、`searchString`パラメーターを`Index`メソッドです。 インデックス ビューに追加するテキスト ボックスから、検索する文字列値を受信します。 また、LINQ ステートメントに追加した、`where`名または姓を持つ検索文字列が含まれています。 受講者のみが選択した句。 ステートメントを追加する、[場所](https://msdn.microsoft.com/en-us/library/bb535040.aspx)句は検索対象の値がある場合にのみ実行します。

> [!NOTE]
> 多くの場合は、Entity Framework のエンティティ セットまたはメモリ内コレクションの拡張メソッドとして同じメソッドを呼び出すことができます。 結果は、通常は同じですが、場合によっては異なる場合があります。
> 
> .NET Framework の実装など、`Contains`メソッドは、空の文字列を渡しますが、Entity Framework provider for SQL Server Compact 4.0 には、空の文字列には、0 行が返されます。 すべての行を返します。 そのため、例のコードで (配置、`Where`内のステートメント、`if`ステートメント) すべてのバージョンの SQL Server は、同じ結果を取得することを確認します。 また、.NET Framework の実装の`Contains`メソッドは、既定では、大文字小文字の比較を実行しますが、エンティティ フレームワークの SQL Server プロバイダーは、既定では大文字と小文字の比較を実行します。 そのため、呼び出し、`ToUpper`を明示的に大文字と小文字、テストを行うメソッドにより、結果を変更しないことを返す、リポジトリを使用するには、後でコードを変更するときに、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。 (を呼び出すと、`Contains`メソッドを`IEnumerable`.NET Framework の実装を取得する、コレクション以外のときに呼び出す、`IQueryable`オブジェクト、データベース プロバイダーの実装を取得します)。
> 
> Null の処理が異なる別のデータベース プロバイダーまたは使用する場合も可能性があります、`IQueryable`オブジェクトと比較して使用すると、`IEnumerable`コレクション。 たとえば、一部のシナリオで、`Where`などの条件`table.Column != 0`を持つ列が返されない可能性があります`null`値として。 詳細については、次を参照してください。 ['where' 句で null の変数が正しく処理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)です。


### <a name="add-a-search-box-to-the-student-index-view"></a>学生インデックス ビューに検索ボックスを追加します。

*Views\Student\Index.cshtml*、開始する直前に強調表示されたコードを追加`table`タグ、キャプションのテキスト ボックスを作成するために、**検索**ボタンをクリックします。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

ページを実行し、検索文字列を入力し をクリックして**検索**フィルタ リングが動作していることを確認します。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

URL には「、」検索文字列このページのブックマークを設定した場合はありません一覧を取得するフィルター選択されたブックマークを使用する場合は意味にはが含まれていないことを確認します。 これに適用されますも列の並べ替えリンク全体の一覧が並べ替えられます。 変更、**検索**チュートリアルの後半で、フィルター条件のクエリ文字列を使用するボタンです。

## <a name="add-paging-to-the-students-index-page"></a>ページングを受講者インデックス ページに追加します。

ページングを受講者インデックス ページに追加するをインストールして起動されます、 **PagedList.Mvc** NuGet パッケージです。 他の変更を加えるされます、`Index`メソッドへのページングのリンクを追加し、`Index`ビュー。 **PagedList.Mvc**多くの優れたページングと ASP.NET MVC 用のパッケージの並べ替えの 1 つであり、ここでを目的として他のオプションより、インデックスの推奨ではなく、例としてのみです。 次の図は、ページングは、リンクを示します。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet パッケージをインストールします。

NuGet **PagedList.Mvc**パッケージを自動的にインストール、 **PagedList**依存関係としてパッケージします。 **PagedList**パッケージのインストール、`PagedList`のコレクションの種類と拡張子メソッド`IQueryable`と`IEnumerable`コレクション。 拡張メソッド内のデータの単一ページを作成する、`PagedList`コレクションのうち、`IQueryable`または`IEnumerable`、および`PagedList`コレクションでいくつかのプロパティとページングを容易にするメソッドを提供します。 **PagedList.Mvc**ページング ボタンを表示するページング ヘルパーをインストールします。

**ツール**メニューの **ライブラリ パッケージ マネージャー**し**Package Manager Console**です。

**パッケージ マネージャー コンソール** ウィンドウを確認してください、**パッケージ ソース**は**nuget.org**と**既定のプロジェクト**は、**ContosoUniversity**、次のコマンドを入力します。

`Install-Package PagedList.Mvc`

![PagedList.Mvc をインストールします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

プロジェクトをビルドします。 

### <a name="add-paging-functionality-to-the-index-method"></a>Index メソッドにページング機能を追加します。

*Controllers\StudentController.cs*、追加、`using`のステートメント、`PagedList`名前空間。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

`Index` メソッドを次のコードで置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

このコードを追加、`page`パラメーター、現在の並べ替え順序パラメーターおよびメソッドのシグネチャに現在のフィルター パラメーター。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

最初に、ページが表示されたら、またはユーザーが、ページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。 ページング リンクをクリックした場合、`page`変数は、表示するページ番号が格納されます。

A`ViewBag`プロパティは、このページング中に同じ並べ替え順序を維持するために、ページングのリンクに含める必要がありますので、現在の並べ替え順序で使用してビューを提供します。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

別のプロパティ、 `ViewBag.CurrentFilter`、現在のフィルター文字列に、ビューを提供します。 ページング、中に、フィルターの設定を維持するために、ページングのリンクでこの値を含める必要があるし、ページが表示されときに、テキスト ボックスに復元する必要があります。 ページングの中に検索文字列を変更する場合は、新しいフィルターが、別のデータを表示するため、ページを 1 にリセットします。 テキスト ボックスに値を入力し、[送信] ボタンが押された検索文字列が変更されます。 その場合は、`searchString`パラメーターが null ではありません。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

メソッドの最後に、`ToPagedList`拡張メソッド、受講者を`IQueryable`オブジェクトは、student クエリをページングをサポートするコレクション型の生徒の 1 つのページに変換します。 その 1 つのページの受講者がビューに渡されます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList`メソッドは、ページ数を取得します。 2 つの疑問符を表す、 [null 合体演算子](https://msdn.microsoft.com/en-us/library/ms173224.aspx)です。 Null 合体演算子を null 許容型以外の既定値を定義します式`(page ?? 1)`の値を返すことを意味`page`かどうかは、値を持つまたは 1 を返す`page`が null です。

### <a name="add-paging-links-to-the-student-index-view"></a>学生インデックス ビューにページングのリンクを追加します。

*Views\Student\Index.cshtml*、既存のコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

`@model`ページの上部にあるステートメントは、ビューが現在を取得するを指定します、`PagedList`オブジェクトの代わりに、`List`オブジェクト。

`using`の声明`PagedList.Mvc`ページング ボタンの MVC ヘルパーへのアクセスを提供します。

コードのオーバー ロードを使用して[BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)を指定することができる[FormMethod.Get](https://msdn.microsoft.com/en-us/library/system.web.mvc.formmethod(v=vs.100).aspx/css)です。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

既定値[BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)をパラメーターとして渡される HTTP メッセージの本文では、URL ではなくクエリ文字列は、投稿内容がフォームのデータを送信します。 HTTP GET を指定すると、フォームのデータに渡されます URL クエリ文字列としてユーザーの URL をブックマークすることができます。 [HTTP GET を使用するための W3C ガイドライン](http://www.w3.org/2001/tag/doc/whenToUseGet.html)アクションが、更新プログラムにならない場合に GET を使用することをお勧めします。

テキスト ボックスは、新しいページをクリックすると、現在の検索文字列を表示できるように、現在の検索文字列で初期化されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

列ヘッダーへのリンクは、フィルターの結果内で、ユーザーが並べ替えられるように、コント ローラーに現在の検索文字列を渡すクエリ文字列を使用します。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

現在のページと合計ページ数が表示されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

表示するページがない場合は、「0 の 0 ページ」が表示されます。 (その場合は、ページ番号がページ数より大きいため`Model.PageNumber`1 に設定されてと`Model.PageCount`は 0 です)。

ページング ボタンが表示されます、`PagedListPager`ヘルパー。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager`ヘルパーは、複数の Url を含むおよびスタイル設定、カスタマイズ可能なオプションを提供します。 詳細については、次を参照してください。 [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList)については、GitHub サイトです。

ページを実行します。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

ページング動作のことを確認する異なる並べ替え順のページングのリンクをクリックします。 検索文字列を入力し、ページング ページングも正常に動作する並べ替えとフィルター処理のことを確認するには、もう一度やり直してください。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>作成する、受講者の統計情報を表示するページについて

Contoso 大学 web サイトのページについて、登録日付ごとにどのように多くの学生が登録に表示されます。 これには、グループにグループ化と簡単な計算が必要です。 これを実現するには、次を行います。

- ビューに渡す必要があるデータのビュー モデル クラスを作成します。
- 変更、`About`メソッドで、`Home`コント ローラー。
- 変更、`About`ビュー。

### <a name="create-the-view-model"></a>ビュー モデルを作成します。

作成、 *ViewModels*プロジェクト フォルダー内のフォルダーです。 クラス ファイルを追加、そのフォルダー内*EnrollmentDateGroup.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Home コント ローラーの変更します。

*HomeController.cs*、次の追加`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

クラスの始め中かっこの直後に、データベース コンテキストのクラスの変数を追加します。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

`About` メソッドを次のコードで置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ ステートメントの登録日で学生エンティティをグループ化、各グループ内のエンティティの数を計算し、結果のコレクションに格納`EnrollmentDateGroup`モデル オブジェクトを表示します。

追加、`Dispose`メソッド。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>変更、ビューについて

コードで置き換え、 *Views\Home\About.cshtml*を次のコード ファイル。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

アプリケーションを実行し、をクリックして、**に関する**リンクします。 登録の日付ごとの生徒の数は、テーブルに表示されます。

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>概要

このチュートリアルでは、データ モデルを作成して基本的な CRUD、並べ替え、フィルター、ページング、および機能をグループ化を実装する方法を説明しました。 次のチュートリアルでは、データ モデルを展開してより高度なトピックを見るが始めます。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[前へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[次へ](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
