---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 並べ替え、フィルター、および ASP.NET MVC アプリケーション (10 の 3) で、Entity Framework とページング |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 09327b760d9be38d7e004cbcef08cad4eab3a26c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>並べ替え、フィルター、および ASP.NET MVC アプリケーション (10 の 3) で Entity Framework でのページング
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、基本的な CRUD 操作するための web ページのセットを実装`Student`エンティティです。 このチュートリアルでは、並べ替え、フィルター、およびページング機能を追加します、**受講者**インデックス ページです。 単純なグループ化を実行するページも作成します。

次の図は、作業が終了したときにページがどのように表示されるかを示しています。 列見出しとは、ユーザーがクリックしてその列で並べ替えを行うことができるリンクです。 列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序が切り替えられます。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Students インデックス ページに列の並べ替えリンクを追加する

学生インデックス ページに並べ替えを追加するを変更、`Index`のメソッド、`Student`コント ローラーのコードを追加し、`Student`ビューにインデックスをします。

### <a name="add-sorting-functionality-to-the-index-method"></a>並べ替え、インデックス メソッドに機能を追加します。

*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

このコードは、URL 内の文字列から `sortOrder` パラメーターを受け取ります。 クエリ文字列の値は、アクション メソッドにパラメーターとして、ASP.NET MVC によって提供されます。 パラメータは、"Name" または "Date" という文字列で、その後に必要に応じてアンダースコアと降順を指定する文字列 "desc" が続きます。 既定の並べ替え順序は昇順です。

インデックス ページが初めて要求されたときには、クエリ文字列はありません。 受講者がで昇順に表示される`LastName`、フォール スルー大文字小文字によって設定される既定値は、`switch`ステートメントです。 ユーザーが列見出しハイパーリンクをクリックすると、適切な `sortOrder` 値がクエリ文字列で提供されます。

2 つ`ViewBag`変数を使用するビューは、適切なクエリ文字列の値を列見出しのハイパーリンクを構成できます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

これらは、三項ステートメントです。 最初の 1 つを指定する場合、`sortOrder`パラメーターが null または空で、`ViewBag.NameSortParm`に設定する必要があります"名\_desc"、それ以外の空の文字列に設定する必要があります。 これらの 2 つのステートメントを使用して、次のようにビューで列見出しのハイパーリンクの設定することができます。

| 既定の並べ替え順 | 姓のハイパーリンク | 日付のハイパーリンク |
| --- | --- | --- |
| 姓の昇順 | descending | ascending |
| 姓の降順 | ascending | ascending |
| 日付の昇順 | ascending | descending |
| 日付の降順 | ascending | ascending |

メソッドを使用して[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)を並べ替え列を指定します。 コードを作成、 [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)する前に変数、`switch`ステートメントでは、変更で、`switch`ステートメント、および呼び出し、`ToList`メソッドした後に、`switch`ステートメントです。 `IQueryable` 変数を作成および変更するときに、データベースに送信されるクエリはありません。 変換するまでに、クエリが実行されていません、`IQueryable`などのメソッドを呼び出すことで、コレクションにオブジェクト`ToList`です。 そのため、このコードなりますまで実行されない 1 つのクエリで、`return View`ステートメントです。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>列見出しを学生インデックス ビューへのハイパーリンクの追加します。

*Views\Student\Index.cshtml*、置換、`<tr>`と`<th>`の見出し行を強調表示されたコードが要素。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

このコード内の情報を使用して、`ViewBag`適切なクエリでハイパーリンクを設定するプロパティの文字列値です。

ページを実行し、をクリックして、**姓**と**登録日**を並べ替えることを確認する列見出しが動作します。

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

クリックした後、**姓**見出し、受講者が降順で表示される最後の名前。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>受講者インデックス ページに検索ボックスを追加します。

Students インデックス ページにフィルターを追加するには、テキスト ボックスと送信ボタンをビューに追加し、`Index` メソッドで対応する変更を行います。 テキスト ボックスを使用して、姓と名のフィールドに検索する文字列を入力できます。

### <a name="add-filtering-functionality-to-the-index-method"></a>Index メソッドにフィルター処理機能を追加します。

*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード (変更が強調表示されます)。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

`searchString` パラメーターを `Index` メソッドに追加しました。 LINQ ステートメントにも追加している、 `where` clausethat 名または姓を持つ検索文字列が含まれています。 受講者のみを選択します。 インデックス ビューに追加するテキスト ボックスから、検索する文字列値を受信します。ステートメントを追加する、[場所](https://msdn.microsoft.com/library/bb535040.aspx)句は検索対象の値がある場合にのみ実行します。

> [!NOTE]
> 多くの場合は、Entity Framework のエンティティ セットまたはメモリ内コレクションの拡張メソッドとして同じメソッドを呼び出すことができます。 結果は、通常は同じですが、場合によっては異なる場合があります。 .NET Framework の実装など、`Contains`メソッドは、空の文字列を渡しますが、Entity Framework provider for SQL Server Compact 4.0 には、空の文字列には、0 行が返されます。 すべての行を返します。 そのため、例のコードで (配置、`Where`内のステートメント、`if`ステートメント) すべてのバージョンの SQL Server は、同じ結果を取得することを確認します。 また、.NET Framework の実装の`Contains`メソッドは、既定では、大文字小文字の比較を実行しますが、エンティティ フレームワークの SQL Server プロバイダーは、既定では大文字と小文字の比較を実行します。 そのため、呼び出し、`ToUpper`を明示的に大文字と小文字、テストを行うメソッドにより、結果を変更しないことを返す、リポジトリを使用するには、後でコードを変更するときに、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。 (`IEnumerable` コレクションに対して `Contains` メソッドを呼び出したときには、.NET Framework の実装を取得します。`IQueryable` オブジェクトに対して呼び出したときには、データベース プロバイダーの実装を取得します)。


### <a name="add-a-search-box-to-the-student-index-view"></a>Students インデックス ビューに [Search] ボックスを追加する

*Views\Student\Index.cshtml*、開始する直前に強調表示されたコードを追加`table`タグ、キャプションのテキスト ボックスを作成するために、**検索**ボタンをクリックします。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

ページを実行し、検索文字列を入力し をクリックして**検索**フィルタ リングが動作していることを確認します。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

URL には「、」検索文字列このページのブックマークを設定した場合はありません一覧を取得するフィルター選択されたブックマークを使用する場合は意味にはが含まれていないことを確認します。 変更、**検索**チュートリアルの後半で、フィルター条件のクエリ文字列を使用するボタンです。

## <a name="add-paging-to-the-students-index-page"></a>ページングを受講者インデックス ページに追加します。

ページングを受講者インデックス ページに追加するをインストールして起動されます、 **PagedList.Mvc** NuGet パッケージです。 他の変更を加えるされます、`Index`メソッドへのページングのリンクを追加し、`Index`ビュー。 **PagedList.Mvc**多くの優れたページングと ASP.NET MVC 用のパッケージの並べ替えの 1 つであり、ここでを目的として他のオプションより、インデックスの推奨ではなく、例としてのみです。 次の図は、ページングは、リンクを示します。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet パッケージをインストールします。

NuGet **PagedList.Mvc**パッケージを自動的にインストール、 **PagedList**依存関係としてパッケージします。 **PagedList**パッケージのインストール、`PagedList`のコレクションの種類と拡張子メソッド`IQueryable`と`IEnumerable`コレクション。 拡張メソッド内のデータの単一ページを作成する、`PagedList`コレクションのうち、`IQueryable`または`IEnumerable`、および`PagedList`コレクションでいくつかのプロパティとページングを容易にするメソッドを提供します。 **PagedList.Mvc**ページング ボタンを表示するページング ヘルパーをインストールします。

**ツール**メニューの **ライブラリ パッケージ マネージャー**し**Manage NuGet Packages for Solution**です。

**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして、**オンライン** タブの左側にし、検索ボックスに「ページ」を入力します。 表示されている場合、 **PagedList.Mvc**パッケージ、をクリックして**インストール**です。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

**プロジェクトの選択**ボックスで、クリックして**OK**です。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Index メソッドにページング機能を追加します。

*Controllers\StudentController.cs*、追加、`using`のステートメント、`PagedList`名前空間。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

`Index` メソッドを次のコードで置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

このコードを追加、`page`パラメーター、現在の並べ替え順序パラメーターおよびメソッドのシグネチャを次に示すように現在のフィルター パラメーター。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

最初にページが表示されるとき、またはユーザーがページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。 ページング リンクをクリックした場合、`page`変数は、表示するページ番号が格納されます。

`A ViewBag` プロパティは、このページング中に同じ並べ替え順序を維持するために、ページングのリンクに含める必要がありますので、現在の並べ替え順序で使用してビューを提供します。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

別のプロパティ、 `ViewBag.CurrentFilter`、現在のフィルター文字列に、ビューを提供します。 ページング中にフィルターの設定を維持するために、ページングのリンクにこの値を含める必要があり、ページが再表示されるときに、この値をテキスト ボックスに復元する必要があります。 ページングの中に検索文字列を変更した場合は、新しいフィルターのために別のデータが表示されるため、ページを 1 にリセットする必要があります。 テキスト ボックスに値を入力し、[送信] ボタンが押された検索文字列が変更されます。 その場合は、`searchString`パラメーターが null ではありません。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

メソッドの最後に、`ToPagedList`拡張メソッド、受講者を`IQueryable`オブジェクトは、student クエリをページングをサポートするコレクション型の生徒の 1 つのページに変換します。 その 1 つのページの受講者がビューに渡されます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` メソッドは、ページ番号を受け取ります。 2 つの疑問符を表す、 [null 合体演算子](https://msdn.microsoft.com/library/ms173224.aspx)です。 null 合体演算子は null 許容型の既定値を定義します。式 `(page ?? 1)` は、値がある場合は `page` の値を返し、`page` が null の場合は 1 を返すことを意味します。

### <a name="add-paging-links-to-the-student-index-view"></a>学生インデックス ビューにページングのリンクを追加します。

*Views\Student\Index.cshtml*、既存のコードを次のコードに置き換えます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

ページの上部にある `@model` ステートメントは、ビューが `List` オブジェクトの代わりに `PagedList` オブジェクトを取得するようになったことを指定します。

`using`の声明`PagedList.Mvc`ページング ボタンの MVC ヘルパーへのアクセスを提供します。

コードのオーバー ロードを使用して[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)を指定することができる[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)です。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

既定値[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)をパラメーターとして渡される HTTP メッセージの本文では、URL ではなくクエリ文字列は、投稿内容がフォームのデータを送信します。 HTTP GET を指定すると、フォーム データがクエリ文字列として URL で渡され、ユーザーが URL をブックマークできるようになります。 [HTTP GET を使用するための W3C ガイドライン](http://www.w3.org/2001/tag/doc/whenToUseGet.html)アクションが、更新プログラムにならない場合は、GET を使用するように指定します。

テキスト ボックスは、新しいページをクリックすると、現在の検索文字列を表示できるように、現在の検索文字列で初期化されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

列ヘッダーへのリンクは、フィルターの結果内でユーザーが並べ替えられるように、クエリ文字列を使用してコントローラーに現在の検索文字列を渡します。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

現在のページと合計ページ数が表示されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

表示するページがない場合は、「0 の 0 ページ」が表示されます。 (その場合は、ページ番号がページ数より大きいため`Model.PageNumber`1 に設定されてと`Model.PageCount`は 0 です)。

ページング ボタンが表示されます、`PagedListPager`ヘルパー。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager`ヘルパーは、複数の Url を含むおよびスタイル設定、カスタマイズ可能なオプションを提供します。 詳細については、次を参照してください。 [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList)については、GitHub サイトです。

ページを実行します。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

異なる並べ替え順でページングのリンクをクリックし、ページングが機能することを確認します。 その後で、検索文字列を入力して、ページングをもう一度試し、並べ替えとフィルター処理を使用してもページングが正しく機能することを確認します。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>作成する、受講者の統計情報を表示するページについて

Contoso 大学 web サイトのページについて、登録日付ごとにどのように多くの学生が登録に表示されます。 これには、グループ化とグループに関する簡単な計算が必要です。 これを実行するためには、次の手順を実行します。

- ビューに渡す必要があるデータのビュー モデル クラスを作成します。
- 変更、`About`メソッドで、`Home`コント ローラー。
- 変更、`About`ビュー。

### <a name="create-the-view-model"></a>ビュー モデルを作成します。

作成、 *ViewModels*フォルダーです。 クラス ファイルを追加、そのフォルダー内*EnrollmentDateGroup.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Home コントローラーを変更する

*HomeController.cs*、次の追加`using`ファイルの上部にあるステートメント。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

クラスの始め中かっこの直後に、データベース コンテキストのクラスの変数を追加します。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

`About` メソッドを次のコードで置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ ステートメントは、登録日で受講者エンティティをグループ化し、各グループ内のエンティティの数を計算して、結果を `EnrollmentDateGroup` ビュー モデル オブジェクトのコレクションに格納します。

追加、`Dispose`メソッド。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>About ビューを変更する

コードで置き換え、 *Views\Home\About.cshtml*を次のコード ファイル。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

アプリケーションを実行し、をクリックして、**に関する**リンクします。 登録の日付ごとの学生の数が、テーブルに表示されます。

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>省略可能: Windows Azure にアプリを配置します。

これまで、アプリケーションの実行はローカルで IIS Express で開発用コンピューターにします。 使用可能にする他の人に、インターネット経由で使用して、web ホスティング プロバイダーに配置する必要です。 チュートリアルのこの省略可能なセクションの展開に Windows Azure Web サイトへ。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Code First Migrations を使用して、データベースを展開するには

データベースを展開するには、Code First Migrations を使用します。 設定を構成する Visual Studio から展開するために使用する発行プロファイルを作成するときに、というラベルが付いたチェック ボックスをオンします**実行 Code First Migrations (アプリケーション開始時に実行されます)**です。 この設定により、展開プロセスを自動的にアプリケーションを構成する*Web.config* Code First を使用するように、移行先サーバー上のファイル、`MigrateDatabaseToLatestVersion`初期化子のクラスです。

Visual Studio は、展開プロセス中に、データベースのすべてのものを実行してされません。 展開後に初めてアクセスするデータベースと、展開されたアプリケーション、Code First 自動的にデータベースを作成またはデータベース スキーマを最新バージョンに更新します。 アプリケーションには、移行が実装されている場合`Seed`メソッド、メソッドの実行後、データベースが作成されるか、スキーマを更新します。

使用して移行`Seed`メソッドは、テスト データを挿入します。 変更する必要がありますを実稼働環境に配置された場合、`Seed`メソッドが、実稼働データベースに挿入するデータの挿入のみようにします。 たとえば、現在のデータ モデルのことができますが、実際のコース、架空の受講者に、開発用データベースで。 記述することができます、`Seed`を読み込みの両方で、開発し、実稼働環境に展開する前に、架空の受講者をコメントします。 作成して、`Seed`コースのみを読み込むし、アプリケーションの UI を使用して手動でテスト データベースに架空の受講者を入力します。

### <a name="get-a-windows-azure-account"></a>Windows Azure アカウントを取得します。

Windows Azure アカウントが必要です。 既に持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 [Windows Azure 無料評価版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Windows Azure での web サイトおよび SQL データベースを作成します。

Windows Azure の Web サイトは、Windows Azure の他のクライアントと共有されている仮想マシン (Vm) で実行されることを意味する共有ホスティング環境で実行されます。 共有ホスティング環境は、クラウドで作業を開始する低コスト方法です。 その後、web トラフィックが増加すると、アプリケーションを専用の仮想マシンで実行して、ニーズに対応するスケールできます。 複雑なアーキテクチャを必要がある場合は、Windows Azure クラウド サービスに移行することができます。 クラウド サービスは、ニーズに応じて構成できる専用の仮想マシンで実行されます。

Windows Azure SQL データベースとは、SQL Server テクノロジに基づいて構築されたクラウド ベースのリレーショナル データベース サービスです。 ツールと SQL Server で動作するアプリケーションも、SQL Database で動作します。

1. [Windows Azure 管理ポータル](https://manage.windowsazure.com/)をクリックして**Web サイト**をクリックして、左側のタブ**新規**です。

    ![管理ポータルで新しいボタン](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. をクリックして**カスタム作成**です。

    ![管理ポータルでデータベースのリンクを作成します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **新しい Web サイト - カスタム作成**ウィザードが開きます。
3. **新しい Web サイト**ステップのウィザードで文字列を入力してください。、 **URL** 、アプリケーションの一意の URL として使用するボックスです。 完全な URL は、ここに入力したものとテキスト ボックスの横に表示されるサフィックスで構成されます。 図に示す"ConU"が、ため、別の名前を選択する必要が、その URL が実行される可能性があります。

    ![管理ポータルでデータベースのリンクを作成します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. **地域**ドロップダウン リストをするのに近い地域を選択します。 この設定は、データ センターで、web サイトは実行を指定します。
5. **データベース**ドロップダウン リストで、選択**無料の 20 MB SQL データベースを作成する**です。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. **DB 接続文字列名**、入力*SchoolContext*です。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. 右側のボックスの下部にある矢印をクリックします。 ウィザード、**データベース設定**手順です。
8. **名前**ボックスに、入力*ContosoUniversityDB*です。
9. **サーバー**ボックスで、**新しい SQL データベース サーバー**です。 また、以前にサーバーを作成した場合は、ドロップダウン リストからそのサーバーを選択できます。
10. 管理者の入力**ログイン名**と**パスワード**です。 選択した場合は**新しい SQL データベース サーバー**既存の名前とパスワードをここに入力していない、新しい名前とデータベースにアクセスするときに後で使用するようになりました定義しているパスワードを入力しているか。 以前作成したサーバーを選択した場合は、そのサーバーの資格情報を入力します。 このチュートリアルでは、選択することはありません、***詳細***チェック ボックスをオンします。 ***詳細***オプションでは、データベースを設定できます。[照合順序](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)です。
11. 同じ選択**地域**web サイトの選択しました。
12. 完了したらを示すために、ボックスの右下にあるチェック マークをクリックします。   
  
    ![データベースの設定手順は、新しい Web サイトで、ウィザードで作成データベース](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    次の図は、既存の SQL Server およびログインを使用します。   
  
    ![データベースの設定手順は、新しい Web サイトで、ウィザードで作成データベース](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    管理ポータル ページに戻る、Web サイト、および**ステータス**列は、サイトが作成されていることを示しています。 (通常よりも小さい 1 分間)、しばらくしてから、**ステータス**列は、サイトが正常に作成されたことを示しています。 左側にあるナビゲーション バーで、アカウント内にあるサイトの数が表示されます の横に、 **Websites**アイコン、およびデータベースの数が横に表示、 **SQL データベース**アイコン。

## <a name="deploy-the-application-to-windows-azure"></a>Windows Azure にアプリケーションを配置します。

1. Visual Studio でプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**コンテキスト メニューからです。  
  
    ![プロジェクトのコンテキスト メニューを発行します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. **プロファイル**のタブ、 **Web の発行**ウィザード、をクリックして**インポート**です。  
  
    ![発行設定のインポート](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. 既に Visual Studio で、Windows Azure サブスクリプションを追加していない場合は、次の手順を実行します。 次の手順で追加する、サブスクリプション下にあるドロップダウン リストを一覧表示できるように**Windows Azure web サイトからのインポート**web サイトが含まれます。

    a.  **発行プロファイルのインポート**ダイアログ ボックスで、をクリックして**Windows Azure web サイトからのインポート**、順にクリック**追加の Windows Azure サブスクリプション**です。

    ![Windows Azure サブスクリプションを追加します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b.  **Windows Azure サブスクリプションのインポート**ダイアログ ボックスで、をクリックして**サブスクリプション ファイルをダウンロード**です。

    ![サブスクリプション ファイルをダウンロードします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. ブラウザー ウィンドウで、保存、 *.publishsettings*ファイル。

    ![.publishsettings ファイルをダウンロードします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > セキュリティ -、 *publishsettings*ファイルには、Windows Azure のサブスクリプションおよびサービスの管理に使用される (エンコードされていない) 資格情報が含まれています。 このファイルのセキュリティのベスト プラクティスは、ソース ディレクトリの外部一時的に格納する、(たとえば、 *\documents*フォルダー)、インポートが完了した後で削除するとします。 アクセスを取得した悪意のあるユーザー、`.publishsettings`ファイルは、編集、作成、および、Windows Azure サービスを削除します。

    d. **Windows Azure サブスクリプションのインポート**ダイアログ ボックスで、をクリックして**参照**に移動し、 *.publishsettings*ファイル。

    ![sub をダウンロードします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. **[インポート]**をクリックします。

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. **発行プロファイルのインポート**ダイアログ ボックスで、 **Windows Azure web サイトからのインポート**ドロップダウン リストから、web サイトを選択して、をクリックして**OK**です。  
  
    ![発行プロファイルのインポート](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. **接続** タブで、をクリックして**接続の検証**設定が正しいことを確認します。  
  
    ![接続を検証します。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. 緑のチェック マークが横に示すように、接続が検証されると、**接続の検証**ボタンをクリックします。 **[次へ]**をクリックします。  
  
    ![正常に検証された接続](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. 開く、**リモート接続文字列**下にあるドロップダウン リスト**SchoolContext**を作成したデータベースの接続文字列を選択します。
8. 選択**実行 Code First Migrations (アプリケーション開始時に実行されます)**です。
9. オフにして**実行時にこの接続文字列を使用して**の**UserContext (DefaultConnection)**このアプリケーションは、メンバーシップ データベースを使用していないため、します。   
  
    ![[設定] タブ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. **[次へ]**をクリックします。
11. **プレビュー**  タブで、をクリックして**開始プレビュー**です。  
  
    ![[プレビュー] タブで StartPreview ボタン](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    タブには、サーバーにコピーされるファイルの一覧が表示されます。 プレビューを表示する、アプリケーションを発行するため必要はありませんですが便利関数を認識します。 この場合、表示されているファイルの一覧は何もする必要はありません。 このアプリケーションを配置するときに、[次へ] に変更されたファイルだけは、この一覧になります。  
  
    ![StartPreview ファイル出力](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. **[発行]**をクリックします。  
    Visual Studio では、Windows Azure サーバーへのファイルのコピーのプロセスを開始します。
13. **出力**ウィンドウどのような展開アクションを実行した示し、展開の成功した完了を報告します。  
  
    ![展開の成功を報告 [出力] ウィンドウ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. 配置に成功したら、既定のブラウザーは、配置の web サイトの URL を自動的に開きます。  
    作成したアプリケーションは、クラウドで実行されています。 受講者 タブをクリックします。  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

この時点で、 *SchoolContext*データベースが作成された Windows Azure SQL データベースで選択したので、**実行 Code First Migrations (アプリの起動時に実行)**です。 *Web.config*配置済みの web サイト内のファイルが変更されたできるように、 [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初期化子が最初の実行、コードが読み取るか、またはデータベースにデータを書き込みます (選択したときに発生した、**受講者** タブ)。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

展開プロセスは、新しい接続文字列も作成*(SchoolContext\_DatabasePublish*) の Code First Migrations データベース スキーマの更新と、データベースのシード処理に使用します。

![Database_Publish 接続文字列](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection*接続文字列は、メンバーシップ データベースを (このチュートリアルでは使用しない)。 *SchoolContext* ContosoUniversity データベース接続文字列はします。

自分のコンピューター上の Web.config ファイルの配置済みのバージョンを見つけることができます*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*です。展開されたアクセスできる*Web.config* FTP を使用してファイル自体です。 手順については、次を参照してください。 [Visual Studio を使用した ASP.NET Web 展開: コードの更新を展開する](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)です。 開始される指示に従って、"FTP ツールを使用する次の 3 つ必要があります: FTP の URL、ユーザー名とパスワードです"。

> [!NOTE]
> Web アプリは、すべての URL を検索するユーザー データを変更できるようにセキュリティを実装しません。 Web サイトをセキュリティで保護する方法については、次を参照してください。[メンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET MVC アプリケーションを Windows Azure Web サイトに展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 他のユーザーが Windows Azure 管理ポータルを使用して、サイトの使用禁止または**サーバー エクスプ ローラー** Visual studio にサイトを停止します。


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>コードの最初の初期化子

展開に関するセクションで説明しました、 [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初期化子が使用されています。 最初のコードなど、使用できるその他の初期化子を提供[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (既定)、 [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx)と[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)です。 `DropCreateAlways`初期化子は単体テストの条件を設定するため役に立ちます。 独自の初期化子を記述することもでき、アプリケーションからの読み取りまたはデータベースに書き込みます。 するまで待機したくない場合は、明示的に初期化子を呼び出すことができます。 初期化子の包括的な説明については、書籍の第 6 章を参照してください。 [Entity Framework のプログラミング: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman して Rowan Miller です。

## <a name="summary"></a>まとめ

このチュートリアルでは、データ モデルを作成して基本的な CRUD、並べ替え、フィルター、ページング、および機能をグループ化を実装する方法を説明しました。 次のチュートリアルでは、データ モデルを展開してより高度なトピックを見るが始めます。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [次へ](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
