---
title: "Razor ページの EF コア - 並べ替え、フィルター、ページング - 8 の 3"
author: rick-anderson
description: "このチュートリアルでは、並べ替え、フィルター、およびページング ASP.NET Core および Entity Framework のコアを使用して 1 ページに機能を追加します。"
ms.author: riande
ms.date: 10/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 9c1ee6f8c00f3cd501ea86fbf73f51ae540a010a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>並べ替え、フィルター、ページング、およびグループ化 - EF コア Razor ページ (3/8)

によって[Tom Dykstra](https://github.com/tdykstra)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、および[Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

このチュートリアル、並べ替え、フィルター処理、グループ化、およびページング、機能が追加されます。

次の図は、[完了] ページを示します。 列見出しとは、列の並べ替えにクリック可能なリンクです。 昇順と降順の並べ替え順序の間の切り替えを列見出しを繰り返しクリックするとします。

![インデックス ページの受講者](sort-filter-page/_static/paging.png)

問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)です。

## <a name="add-sorting-to-the-index-page"></a>インデックス ページに並べ替えを追加します。

文字列を追加、 *Students/Index.cshtml.cs* `PageModel`並べ替えのパラメーターを格納します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


更新プログラム、 *Students/Index.cshtml.cs* `OnGetAsync`を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

上記のコードを受け取る、 `sortOrder` URL 内のクエリ文字列からのパラメーターです。 (クエリ文字列を含む) の URL がによって生成された、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

`sortOrder`パラメーターが「名」または「日付」です。 `sortOrder`パラメーターが必要に応じて続く"_desc"降順を指定します。 既定の並べ替え順序は昇順です。

インデックス ページが要求された場合、**受講者**リンクは、クエリ文字列はありません。 受講者は、姓を昇順に表示されます。 既定値 (フォール スルー ケース) が、姓を昇順、`switch`ステートメントです。 ユーザーが列見出しのリンク、適切な`sortOrder`クエリ文字列の値に値を指定します。

`NameSort`および`DateSort`Razor ページで適切なクエリ文字列の値を列見出しのハイパーリンクを構成するために使用します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

次のコードを含む c# [?: 演算子](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

最初の行を指定すると`sortOrder`が null または空で、 `NameSort` "name_desc"に設定されています。 場合`sortOrder`は**いない**null または空である`NameSort`空の文字列に設定されています。

`?: operator`三項演算子とも呼ばれます。

これら 2 つのステートメントでは、ビュー、列見出しのハイパーリンクの次のように設定を有効にします。

| 現在の並べ替え順序 | 名の最後のハイパーリンク | 日付のハイパーリンク |
|:--------------------:|:-------------------:|:--------------:|
| 最後の名前昇順 | descending        | ascending      |
| 最後の名前が降順 | ascending           | ascending      |
| 日付の昇順       | ascending           | descending     |
| 日付 (降順)      | ascending           | ascending      |

メソッドは、並べ替える列を指定するのに LINQ to Entities を使用します。 コードを初期化します、`IQueryable<Student> `スイッチ ステートメントの前にし、switch ステートメントで変更します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-)]

 ときに、`IQueryable`を作成または変更するには、クエリは、データベースに送信されません。 までクエリが実行されない場合は、`IQueryable`オブジェクトをコレクションに変換します。 `IQueryable`などのメソッドを呼び出すことで、コレクションに変換されます`ToListAsync`です。 したがって、`IQueryable`コードの次のステートメントまで実行されない 1 つのクエリ結果。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`列の数が多いと詳細を取得でした。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>学生インデックス ビューに列見出しのハイパーリンクを追加します。

コードに置き換えます*Students/Index.cshtml*、次のようにコードを強調表示されます。

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

上のコードでは以下の操作が行われます。

* ハイパーリンクを追加、`LastName`と`EnrollmentDate`列見出し。
* 内の情報を使用して`NameSort`と`DateSort`現在の並べ替え順序の値を含むハイパーリンクを設定します。

動作を確認する並べ替え。

* アプリの実行を選択して、**受講者**タブです。
* をクリックして**名前**です。
* をクリックして**登録日**です。

コードの理解を深めるを取得します。

* *Student/Index.cshtml.cs*にブレークポイントを設定`switch (sortOrder)`です。
* ウォッチを追加`NameSort`と`DateSort`です。
* *Student/Index.cshtml*にブレークポイントを設定`@Html.DisplayNameFor(model => model.Student[0].LastName)`です。

デバッガー ステップします。

## <a name="add-a-search-box-to-the-students-index-page"></a>受講者インデックス ページに検索ボックスを追加します。

追加する、受講者インデックス ページにフィルター処理します。

* テキスト ボックスと [送信] ボタンは、Razor ページに追加されます。 テキスト ボックスは、最初と最後の名前で検索文字列を指定します。
* テキスト ボックスの値を使用してページ モデルが更新されます。

### <a name="add-filtering-functionality-to-the-index-method"></a>Index メソッドにフィルター処理機能を追加します。

更新プログラム、 *Students/Index.cshtml.cs* `OnGetAsync`を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

上のコードでは以下の操作が行われます。

* 追加、`searchString`パラメーターを`OnGetAsync`メソッドです。 検索する文字列値は、次のセクションで追加されるテキスト ボックスから受信されます。
* LINQ ステートメントに追加、`Where`句。 `Where`句が名または姓を持つ検索文字列が含まれています。 受講者のみを選択します。 検索する値がある場合にのみ LINQ ステートメントを実行します。

メモ: 上記のコードの呼び出しの`Where`メソッドを`IQueryable`オブジェクト、およびフィルターは、サーバーで処理します。 呼び出すことがあります tha アプリの一部のシナリオで、`Where`メモリ内コレクションの拡張メソッドとしてメソッドです。 たとえば、 `_context.Students` EF コアから変更`DbSet`リポジトリをするメソッドを返します、`IEnumerable`コレクション。 結果は、通常同じになりますが、場合によっては異なる場合があります。

.NET Framework の実装など、`Contains`既定では大文字小文字の比較を実行します。 SQL Server で`Contains`大文字小文字の区別は、SQL Server インスタンスの照合順序の設定によって決まります。 SQL Serve が大文字と小文字を既定値です。 `ToUpper`テストを明示的に大文字と小文字を呼び出すことができます。

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

上記のコードは結果ことを確認する大文字と小文字を使用するコードが変更された場合`IEnumerable`です。 ときに`Contains`で呼び出されると、`IEnumerable`コレクション、.NET Core を実装を使用します。 ときに`Contains`で呼び出されると、`IQueryable`オブジェクト、データベースの実装を使用します。 返す、`IEnumerable`リポジトリから大幅なパフォーマンス penality を持つことができます。

1. DB サーバーからは、すべての行が返されます。
1. フィルターは、アプリケーションで返されるすべての行に適用されます。

呼び出し元のパフォーマンスの低下が`ToUpper`です。 `ToUpper`コードは、TSQL の SELECT ステートメントの WHERE 句で関数を追加します。 追加される関数では、オプティマイザーがインデックスを使用できなくなります。 大文字と小文字 SQL がインストールされていることをお勧めを回避するのには`ToUpper`不要であるときに呼び出します。

### <a name="add-a-search-box-to-the-student-index-view"></a>学生インデックス ビューに検索ボックスを追加します。

*Views/Student/Index.cshtml*、作成する次の強調表示されたコードを追加、**検索**ボタンをクリックし、さまざまな chrome です。

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

上記のコードを使用して、 `<form>` [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)検索テキスト ボックスとボタンを追加します。 既定では、`<form>`タグ ヘルパーが POST でフォームにデータを送信します。 Post、HTTP メッセージの本文では、URL ではなく、パラメーターが渡されます。 HTTP GET を使用すると、フォームのデータはクエリ文字列として、URL で渡されます。 クエリ文字列を使用してデータを渡すことにより、URL にブックマークを設定できます。 [W3C ガイドライン](https://www.w3.org/2001/tag/doc/whenToUseGet.html)更新アクションが返されない場合に GET を使用することをお勧めします。

アプリをテストします。

* 選択、**受講者**タブし、検索文字列を入力します。
* 選択**検索**です。

URL に検索文字列が含まれることに注意してください。

```html
http://localhost:5000/Students?SearchString=an
```

ブックマークが、ページの URL に含まれる場合は、ページにブックマークは、および`SearchString`クエリ文字列。 `method="get"`で、`form`はタグを生成するクエリ文字列の原因です。

現時点では、列見出しのソートのリンクを選択すると、フィルター値から、**検索**ボックスは失われます。 失われたフィルターの値は、次のセクションでは固定です。

## <a name="add-paging-functionality-to-the-students-index-page"></a>受講者インデックス ページにページング機能を追加します。

このセクションで、`PaginatedList`ページングをサポートするクラスを作成します。 `PaginatedList`クラス`Skip`と`Take`をテーブルのすべての行を取得する代わりに、サーバー上のデータをフィルター処理するステートメント。 次の図は、ページング ボタンを示しています。

![受講者とページングのリンクのページをインデックスします。](sort-filter-page/_static/paging.png)

プロジェクト フォルダーに作成`PaginatedList.cs`を次のコード。

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

`CreateAsync`前述のコードでメソッドがページ サイズとページ番号を受け取り、適切な適用`Skip`と`Take`ステートメントを`IQueryable`です。 ときに`ToListAsync`で呼び出されると、 `IQueryable`、要求されたページのみを含むリストを返します。 プロパティ`HasPreviousPage`と`HasNextPage`を有効または無効にするために使用**前**と**次**ボタンをページングします。

`CreateAsync`メソッドの使用を作成、`PaginatedList<T>`です。 コンス トラクターを作成できません、`PaginatedList<T>`オブジェクトのコンス トラクターは、非同期コードを実行できません。

## <a name="add-paging-functionality-to-the-index-method"></a>Index メソッドにページング機能を追加します。

*Students/Index.cshtml.cs*の型を更新`Student`から`IList<Student>`に`PaginatedList<Student>`:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

更新プログラム、 *Students/Index.cshtml.cs* `OnGetAsync`を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-)]

上記のコードを追加、現在のページ インデックス`sortOrder`、および`currentFilter`メソッド シグネチャにします。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

すべてのパラメーターは、次のような場合に null。

* ページが呼び出され、**受講者**リンクします。
* ユーザーには、ページングや並べ替えのリンクをクリックしていません。

ページングのリンクがクリックされたときに、ページのインデックス変数には、表示するページ番号が含まれています。

`CurrentSort`現在の並べ替え順序に Razor ページを提供します。 ページングの中に並べ替え順序を保持するページングのリンクでは、現在の並べ替え順序を含める必要があります。

`CurrentFilter`現在のフィルター文字列に Razor ページを提供します。 `CurrentFilter`値。

* ページングの中に、フィルターの設定を維持するために、ページングのリンクで含める必要があります。
* ページが表示されと、テキスト ボックスに復元する必要があります。

ページングの中に検索文字列を変更する場合は、ページが 1 にリセットされます。 ページは、新しいフィルターが、別のデータを表示するために、1 にリセットされるがします。 検索値が入力されている場合と**送信**が選択されています。

* 検索文字列を変更します。
* `searchString`パラメーターが null でないです。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync`メソッドは、ページングをサポートするコレクション型の生徒の 1 つのページを学生クエリを変換します。 その 1 つのページの受講者は、Razor ページに渡されます。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

2 つの疑問符`PaginatedList.CreateAsync`を表す、 [null 合体演算子](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator)です。 Null 合体演算子では、null 許容型の既定値を定義します。 式`(pageIndex ?? 1)`の値を返すことを意味`pageIndex`値がある場合。 場合`pageIndex`しない値を持つ、1 を返します。

## <a name="add-paging-links-to-the-student-razor-page"></a>Razor ページ受講者にページングのリンクを追加します。

内のマークアップを更新*Students/Index.cshtml*です。 変更が強調表示されます。

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-)]

列ヘッダーへのリンクを現在の検索文字列を渡すクエリ文字列を使用して、`OnGetAsync`メソッド内での結果をフィルター処理、ユーザーが並べ替えられるようにします。

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

タグ ヘルパーによってページング ボタンが表示されます。

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

アプリを実行して、受講者のページに移動します。

* ページング動作のことを確認するために、異なる並べ替え順のページングのリンクをクリックします。
* 並べ替えとフィルター処理でページングが正常に動作していることを確認するには、検索文字列を入力し、ページングを試みます。

![受講者とページングのリンクのページをインデックスします。](sort-filter-page/_static/paging.png)

コードの理解を深めるを取得します。

* *Student/Index.cshtml.cs*にブレークポイントを設定`switch (sortOrder)`です。
* ウォッチを追加`NameSort`、 `DateSort`、 `CurrentSort`、および`Model.Student.PageIndex`です。
* *Student/Index.cshtml*にブレークポイントを設定`@Html.DisplayNameFor(model => model.Student[0].LastName)`です。

デバッガー ステップします。

## <a name="update-the-about-page-to-show-student-statistics"></a>学生の統計情報を表示するバージョン情報 ページを更新します。

このステップで*Pages/About.cshtml*更新され、登録日付ごとに登録した数の受講者を表示します。 更新プログラムは、グループ化を使用し、次の手順が含まれています。

* によって使用されるデータのビュー モデル クラスを作成、**に関する**ページ。
* Razor ページについて、ページのモデルを変更します。

### <a name="create-the-view-model"></a>ビュー モデルを作成します。

作成、 *SchoolViewModels*内のフォルダー、*モデル*フォルダーです。

*SchoolViewModels*フォルダーで、追加、 *EnrollmentDateGroup.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>モデルを更新するバージョン情報 ページ

更新プログラム、 *Pages/About.cshtml.cs*を次のコード ファイル。

[!code-csharp[Main](intro/samples/cu/Pages/About.cshtml.cs)]

LINQ ステートメントの登録日で学生エンティティをグループ化、各グループ内のエンティティの数を計算し、結果のコレクションに格納`EnrollmentDateGroup`モデル オブジェクトを表示します。

注: LINQ `group` EF のコアで現在サポートされていないコマンドです。 上記のコードでは、SQL Server から受講者のすべてのレコードが返されます。 `group` Razor ページのアプリで SQL Server ではなく、コマンドを適用します。 この LINQ をサポートする EF コア 2.1`group`演算子、およびグループ化は、SQL Server で発生します。 参照してください[リレーショナル: GroupBy() SQL への変換をサポートして](https://github.com/aspnet/EntityFrameworkCore/issues/2341)です。 [EF コア 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) .NET Core 2.1 でリリースされます。 詳細については、次を参照してください。、 [.NET Core ロードマップ](https://github.com/dotnet/core/blob/master/roadmap.md)です。

### <a name="modify-the-about-razor-page"></a>変更、Razor ページについて

コードで置き換え、 *Views/Home/About.cshtml*を次のコード ファイル。

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

アプリを実行してバージョン情報 ページに移動します。 登録の日付ごとの生徒の数は、テーブルに表示されます。

問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting)です。

![ページについて](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core 2.x ソースのデバッグ](https://github.com/aspnet/Docs/issues/4155)

チュートリアルでは、[次へ]、アプリは移行を使用して、データ モデルを更新します。

>[!div class="step-by-step"]
[前へ](xref:data/ef-rp/crud)
[次へ](xref:data/ef-rp/migrations)
