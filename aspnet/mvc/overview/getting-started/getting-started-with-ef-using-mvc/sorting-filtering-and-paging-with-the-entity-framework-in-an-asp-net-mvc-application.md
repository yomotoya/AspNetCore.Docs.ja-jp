---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework でページング |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 3cd7d5e7ea97dd4defa5e609de70beda7dfccf77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375413"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework でのページング
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


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

各並べ替え順序の異なる LINQ ステートメントを記述する代わりに、LINQ ステートメントを動的に作成できます。 動的な LINQ の詳細については、次を参照してください。[動的 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)します。

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

`searchString` パラメーターを `Index` メソッドに追加しました。 インデックス ビューに追加するテキスト ボックスから検索する文字列値を受け取ります。 LINQ ステートメントに追加することも、`where`名または姓を持つ検索文字列が含まれています。 学生のみを選択する句。 ステートメントを追加する、[場所](https://msdn.microsoft.com/library/bb535040.aspx)句は、検索する値がある場合にのみ実行されます。

> [!NOTE]
> 多くの場合は、Entity Framework のエンティティ セットまたはメモリ内コレクションの拡張メソッドとして同じメソッドを呼び出すことができます。 結果は、通常は同じが、場合によっては異なる場合があります。
> 
> .NET Framework の実装など、`Contains`メソッドが、空の文字列を渡しますが、Entity Framework provider for SQL Server Compact 4.0 は、空の文字列には、ゼロ行を返すときに、すべての行を返します。 例のコードではそのため (配置すること、`Where`内のステートメント、`if`ステートメント) は、すべてのバージョンの SQL Server と同じ結果を取得することを確認します。 .NET Framework の実装も、`Contains`メソッドは、既定では、大文字小文字の比較を実行しますが、Entity Framework の SQL Server プロバイダーは、既定では大文字の比較を実行します。 そのため、呼び出し、`ToUpper`メソッド、テストを明示的に大文字をにより返されます、リポジトリを使用するには、後でコードを変更すると、結果は変化しません、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。 (`IEnumerable` コレクションに対して `Contains` メソッドを呼び出したときには、.NET Framework の実装を取得します。`IQueryable` オブジェクトに対して呼び出したときには、データベース プロバイダーの実装を取得します)。
> 
> Null 処理が異なる別のデータベース プロバイダーまたは使用する場合も場合があります、`IQueryable`を使用する場合は、オブジェクトを比較する`IEnumerable`コレクション。 たとえば、一部のシナリオで、`Where`などの条件`table.Column != 0`を持つ列が返されない可能性があります`null`値として。 詳細については、次を参照してください。 ['where' 句で null 変数が正しく処理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)します。


### <a name="add-a-search-box-to-the-student-index-view"></a>Students インデックス ビューに [Search] ボックスを追加する

*Views\Student\Index.cshtml*、開始する直前に強調表示されたコードを追加`table`キャプション、テキスト ボックスを作成するにはタグと**検索**ボタンをクリックします。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

ページを実行し、検索文字列を入力し をクリックして**検索**フィルターが動作していることを確認します。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

URL に「、」検索文字列、これをこのページにブックマークを設定した場合はありません一覧を取得するフィルター選択されたブックマークを使用する場合は意味が含まれていないことを確認します。 これは適用されますも、列の並べ替えリンク リスト全体を並べ替えられます。 変更を**検索**フィルター条件のチュートリアルの後半でクエリ文字列を使用するボタン。

## <a name="add-paging-to-the-students-index-page"></a>Students インデックス ページにページングを追加します。

Students インデックス ページには、ページングを追加するをインストールして起動します、 **PagedList.Mvc** NuGet パッケージ。 その後で追加の変更、`Index`メソッドへのページングのリンクを追加し、`Index`ビュー。 **PagedList.Mvc**優れた多くのページングと ASP.NET MVC でのパッケージの並べ替えの 1 つとここでその使用のためのものとしてその他のオプションの上の推奨事項ではなく、例としてのみです。 次の図は、ページングのリンクを示します。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>PagedList.MVC NuGet パッケージをインストールします。

NuGet **PagedList.Mvc**パッケージが自動的にインストール、 **PagedList**依存関係としてパッケージします。 **PagedList**パッケージのインストール、`PagedList`のコレクションの種類と拡張子メソッド`IQueryable`と`IEnumerable`コレクション。 拡張メソッドが 1 つのデータのページを作成、`PagedList`コレクションのうち、`IQueryable`または`IEnumerable`、および`PagedList`いくつかのプロパティとメソッドをページングを容易にするコレクションを提供します。 **PagedList.Mvc**パッケージは、ページング ボタンを表示するページング ヘルパーをインストールします。

**ツール**メニューの **ライブラリ パッケージ マネージャー**し**パッケージ マネージャー コンソール**します。

アプリを実行し、をクリックして、**について**リンク。

`Install-Package PagedList.Mvc`

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

プロジェクトをビルドします。 

### <a name="add-paging-functionality-to-the-index-method"></a>このチュートリアルでは、データ モデルを作成して、基本的な CRUD、並べ替え、フィルター、ページング、および機能をグループ化を実装する方法を説明しました。

次のチュートリアルでは、データ モデルを展開してより高度なトピックを見てが始めます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

`Index` メソッドを次のコードで置き換えます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

このチュートリアルの立った方法で改善できましたフィードバックを送信してください。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

最初にページが表示されるとき、またはユーザーがページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。 また、新しいトピックを要求することもできます。`page`表示 Me How With Codeします。

その他の Entity Framework リソースへのリンクが記載`ViewBag`ASP.NET データ アクセス - 推奨リソースします。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

別のプロパティ、 `ViewBag.CurrentFilter`、現在のフィルター文字列でビューを提供します。 ページング中にフィルターの設定を維持するために、ページングのリンクにこの値を含める必要があり、ページが再表示されるときに、この値をテキスト ボックスに復元する必要があります。 ページングの中に検索文字列を変更した場合は、新しいフィルターのために別のデータが表示されるため、ページを 1 にリセットする必要があります。 テキスト ボックスに値を入力し、[送信] ボタンが押されたときに、検索文字列が変更されます。 その場合は、`searchString`パラメーターが null ではありません。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

メソッドの最後に、`ToPagedList`拡張メソッド、受講者を`IQueryable`オブジェクトがページングをサポートするコレクション型の受講者の 1 つのページに学生クエリを変換します。 その学生の 1 つのページは、ビューに渡されます。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` メソッドは、ページ番号を受け取ります。 2 つの疑問符を表す、 [null 合体演算子](https://msdn.microsoft.com/library/ms173224.aspx)します。 null 合体演算子は null 許容型の既定値を定義します。式 `(page ?? 1)` は、値がある場合は `page` の値を返し、`page` が null の場合は 1 を返すことを意味します。

### <a name="add-paging-links-to-the-student-index-view"></a>Student インデックス ビューにページングのリンクを追加します。

*Views\Student\Index.cshtml*、既存のコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

ページの上部にある `@model` ステートメントは、ビューが `List` オブジェクトの代わりに `PagedList` オブジェクトを取得するようになったことを指定します。

`using`ステートメント`PagedList.Mvc`ページング ボタンの MVC ヘルパーにアクセスします。

コードのオーバー ロードを使用して[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)することを指定する、 [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)します。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

既定の[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)フォーム データをパラメーターとして渡されると、URL ではなく HTTP メッセージの本文にクエリ文字列は、投稿を送信します。 HTTP GET を指定すると、フォーム データがクエリ文字列として URL で渡され、ユーザーが URL をブックマークできるようになります。 [HTTP GET を使用するための W3C のガイドライン](http://www.w3.org/2001/tag/doc/whenToUseGet.html)アクションは結果として更新しない場合は、GET を使用することをお勧めします。

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

異なる並べ替え順でページングのリンクをクリックし、ページングが機能することを確認します。 その後で、検索文字列を入力して、ページングをもう一度試し、並べ替えとフィルター処理を使用してもページングが正しく機能することを確認します。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>作成、学生の統計情報を表示するページについて

Contoso University web サイトのページについて、登録日付ごとに登録した受講者の数を表示します。 これには、グループ化とグループに関する簡単な計算が必要です。 これを実行するためには、次の手順を実行します。

- ビューに渡す必要があるデータのビュー モデル クラスを作成します。
- 変更、`About`メソッドで、`Home`コント ローラー。
- 変更、`About`ビュー。

### <a name="create-the-view-model"></a>ビュー モデルを作成します。

作成、 *ViewModels*プロジェクト フォルダー内のフォルダー。 クラス ファイルを追加、そのフォルダー内*EnrollmentDateGroup.cs*テンプレート コードを次のコードに置き換えます。

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、データ モデルを作成して、基本的な CRUD、並べ替え、フィルター、ページング、および機能をグループ化を実装する方法を説明しました。 次のチュートリアルでは、データ モデルを展開してより高度なトピックを見てが始めます。

このチュートリアルの立った方法で改善できましたフィードバックを送信してください。 また、新しいトピックを要求することもできます。[表示 Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。

その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [次へ](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
