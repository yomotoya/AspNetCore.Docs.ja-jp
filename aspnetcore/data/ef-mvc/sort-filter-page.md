---
title: ASP.NET Core MVC と EF Core - 並べ替え、フィルター、ページング - 3/10
author: rick-anderson
description: このチュートリアルでは、ASP.NET Core および Entity Framework Core を使用して並べ替え、フィルター、ページング機能をページに追加します。
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 344e3a1806ff21d8ce335b2b407a8a93baf72c1b
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153723"
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a>ASP.NET Core MVC と EF Core - 並べ替え、フィルター、ページング - 3/10

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)を参照してください。

前のチュートリアルでは、Student エンティティの基本的な CRUD 操作用の Web ページのセットを実装しました。 このチュートリアルでは、Students インデックス ページに並べ替え、フィルター、およびページング機能を追加します。 単純なグループ化を実行するページも作成します。

次の図は、作業が終了したときにページがどのように表示されるかを示しています。 列見出しとは、ユーザーがクリックしてその列で並べ替えを行うことができるリンクです。 列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序が切り替えられます。

![Students インデックス ページ](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Students インデックス ページに列の並べ替えリンクを追加する

Students ンデックス ページに並べ替えを追加するには、`Index`Students コントローラーのメソッドを変更し、Students インデックス ビューにコードを追加します。

### <a name="add-sorting-functionality-to-the-index-method"></a>Index メソッドに並べ替え機能を追加する

*StudentsController.cs* で、`Index` メソッドを次のコードで置き換えます。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

このコードは、URL 内の文字列から `sortOrder` パラメーターを受け取ります。 クエリ文字列の値は、ASP.NET Core MVC によってパラメーターとしてアクション メソッドに提供されます。 パラメータは、"Name" または "Date" という文字列で、その後に必要に応じてアンダースコアと降順を指定する文字列 "desc" が続きます。 既定の並べ替え順序は昇順です。

インデックス ページが初めて要求されたときには、クエリ文字列はありません。 受講者は、姓の昇順で表示されます。これは、`switch` ステートメントでフォールスルー ケースによって確立される既定値です。 ユーザーが列見出しハイパーリンクをクリックすると、適切な `sortOrder` 値がクエリ文字列で提供されます。

2 つの `ViewData` 要素 (NameSortParm と DateSortParm) をビューで使用して、適切なクエリ文字列値を持つ列見出しのハイパーリンクを構成します。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

これらは、三項ステートメントです。 最初のステートメントは、`sortOrder` パラメーターが null または空の場合に、NameSortParm を "name_desc" に設定し、それ以外の場合は任意の空の文字列に設定する必要があることを指定します。 これらの 2 つのステートメントを使用して、次のようにビューで列見出しのハイパーリンクの設定することができます。

|  既定の並べ替え順  | 姓のハイパーリンク | 日付のハイパーリンク |
|:--------------------:|:-------------------:|:--------------:|
| 姓の昇順  | descending          | ascending      |
| 姓の降順 | ascending           | ascending      |
| 日付の昇順       | ascending           | descending     |
| 日付の降順      | ascending           | ascending      |

このメソッドは、並べ替える列を指定するのに LINQ to Entities を使用します。 このコードは、switch ステートメントの前に `IQueryable` 変数を作成し、switch ステートメントでそれを変更して、`switch` ステートメントの後に `ToListAsync` を呼び出します。 `IQueryable` 変数を作成および変更するときに、データベースに送信されるクエリはありません。 クエリは、`ToListAsync` などのメソッドを呼び出すことによって `IQueryable` オブジェクトをコレクションに変換するまで実行されません。 そのため、このコードの結果として、`return View` ステートメントはで実行されない 1 つのクエリになります。

このコードは、多数の列によって冗長になる可能性があります。 [このシリーズの最後のチュートリアル](advanced.md#dynamic-linq)では、文字列変数で `OrderBy` 列の名前を渡すことができるコードの記述方法を説明しています。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>列見出しハイパーリンクを Student インデックス ビューに追加する

*Views/Students/Index.cshtml* のコードを次のコードに置き換え、列見出しのハイパーリンクを追加します。 変更された行は強調表示されています。

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

このコードは、`ViewData` プロパティ内の情報を使用して、適切なクエリ文字列使用含むハイパーリンクを設定します。

アプリを実行し、**[Students]** タブを選択して、**[Last Name]** と **[Enrollment Date]** 列見出しをクリックし、並べ替えが機能することを確認します。

![名前順の Students インデックス ページ](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Students インデックス ページに [Search] ボックスを追加する

Students インデックス ページにフィルターを追加するには、テキスト ボックスと送信ボタンをビューに追加し、`Index` メソッドで対応する変更を行います。 テキスト ボックスを使用して、姓と名のフィールドに検索する文字列を入力できます。

### <a name="add-filtering-functionality-to-the-index-method"></a>Index メソッドにフィルター機能を追加する

*StudentsController.cs* で、`Index` メソッドを次のコードで置き換えます (変更部分が強調表示されています)。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

`searchString` パラメーターを `Index` メソッドに追加しました。 インデックス ビューに追加するテキスト ボックスから検索する文字列値を受け取ります。 さらに LINQ ステートメントに where 句を追加し、姓または名に検索文字列を含む受講者のみを選択します。 where 句を追加するステートメントは、検索する値がある場合にのみ、実行されます。

> [!NOTE]
> ここで、`IQueryable` オブジェクトに対して `Where` メソッドを呼び出し、フィルターがサーバーで処理されます。 一部のシナリオでは、`Where` メソッドをメモリ内コレクションの拡張メソッドとして呼び出す場合があります  (たとえば、EF `DbSet` の代わりに `IEnumerable` コレクションを返すリポジトリ メソッドを参照するように参照を `_context.Students` に変更する場合)。結果は、通常同じになりますが、場合によっては異なる場合があります。
>
>たとえば、.NET Framework の `Contains` メソッドの実装は、既定では大文字小文字を区別する比較を実行しますが、SQL Server では、これは SQL Server インスタンスの照合順序の設定によって決まります。 その設定は、既定では大文字小文字を区別しません。 `ToUpper` メソッドを呼び出して明示的に大文字小文字を区別しないテストを作成することもできます: *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*。 これにより、`IQueryable` オブジェクトの代わりに `IEnumerable` コレクションを返すリポジトリを使用するように後でコードを変更した場合でも確実に同じ結果になるようにすることができます  (`IEnumerable` コレクションに対して `Contains` メソッドを呼び出したときには、.NET Framework の実装を取得します。`IQueryable` オブジェクトに対して呼び出したときには、データベース プロバイダーの実装を取得します)。ただし、このソリューションではパフォーマンスが低下します。 `ToUpper` コードは、TSQL SELECT ステートメントの WHERE 句に関数を格納します。 これにより、オプティマイザーはインデックスを使用できなくなります。 ほとんどの場合、SQL は大文字小文字を区別しないようにインストールされているため、大文字小文字を区別するデータストアに移行するまで `ToUpper` コードを避けることをお勧めします。

### <a name="add-a-search-box-to-the-student-index-view"></a>Students インデックス ビューに [Search] ボックスを追加する

*Views/Student/Index.cshtml* で、キャプション、テキスト ボックス、**[Search]** ボタンを作成するために、オープニング テーブル タグの直前に強調表示されたコードを追加します。

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

このコードは、`<form>` [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使用して、検索テキスト ボックスとボタンを追加します。 既定では、`<form>` タグ ヘルパーは、POST を使用してフォーム データを送信します。これは、パラメーターが、クエリ文字列として URL で渡されるのではなく、HTTP メッセージの本文で渡されることを意味します。 HTTP GET を指定すると、フォーム データがクエリ文字列として URL で渡され、ユーザーが URL をブックマークできるようになります。 アクションの結果として更新されない場合、W3C のガイドラインでは、Get の使用が推奨されます。

アプリを実行し、**[Students]** タブを選択して、検索文字列を入力し、[Search] をクリックして、フィルターが機能していることを確認します。

![フィルターを含む Students インデックス ページ](sort-filter-page/_static/filtering.png)

URL に検索文字列が含まれることに注意してください。

```html
http://localhost:5813/Students?SearchString=an
```

このページをブックマークに設定した場合、ブックマークを使用するときに、フィルター処理された一覧が表示されます。 `method="get"` を `form` タグに追加すると、クエリ文字列が生成されます。

この段階で、列見出しのソートのリンクをクリックすると、**[Search]** ボックスに入力したフィルター値が失われます。 次のセクションでこれを修正します。

## <a name="add-paging-functionality-to-the-students-index-page"></a>Students インデックス ページにページング機能を追加する

Students インデックス ページにページングを追加するには、常にテーブルをすべての行を取得する代わりに、`Skip` および `Take` ステートメントを使用してサーバー上でデータをフィルター処理する `PaginatedList` クラスを作成します。 その後で、`Index` メソッドに変更を加え、ページング ボタンを `Index` ビューに追加します。 ページング ボタンを次の図に示します。

![ページング リンクを含む Students インデックス ページ](sort-filter-page/_static/paging.png)

プロジェクト フォルダーで、`PaginatedList.cs` を作成し、テンプレートのコードを次のコードに置き換えます。

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

このコードの `CreateAsync` メソッドは、ページ サイズとページ番号を受け取り、適切な `Skip` および `Take` ステートメントを `IQueryable` に適用します。 `IQueryable` で `ToListAsync` が呼び出されると、要求されたページのみを含むリストを返します。 プロパティ `HasPreviousPage` および `HasNextPage` を使用して、**[Previous]** および **[Next]** ページング ボタンを有効または無効にすることができます。

コンストラクターは非同期コードを実行できないので、コンストラクターの代わりに `CreateAsync` メソッドを使用して `PaginatedList<T>`オブジェクトを作成します。

## <a name="add-paging-functionality-to-the-index-method"></a>Index メソッドにページング機能を追加する

*StudentsController.cs* で、`Index` メソッドを次のコードで置き換えます。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

このコードは、メソッド シグネチャにページ番号パラメーター、現在の並べ替え順序パラメーター、および現在のフィルター パラメーターを追加します。

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

最初にページが表示されるとき、またはユーザーがページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。  ページングのリンクをクリックすると、ページ変数に表示するページ番号が含まれます。

CurrentSort という名前の `ViewData` 要素が、現在の並べ替え順序をビューに提供します。ページング中に同じ並べ替え順序を維持するために、ページングのリンクにこれを含める必要があります。

CurrentFilter という名前の `ViewData` 要素が現在のフィルター文字列をビューに提供します。 ページング中にフィルターの設定を維持するために、ページングのリンクにこの値を含める必要があり、ページが再表示されるときに、この値をテキスト ボックスに復元する必要があります。

ページングの中に検索文字列を変更した場合は、新しいフィルターのために別のデータが表示されるため、ページを 1 にリセットする必要があります。 テキスト ボックスに値を入力して、[Submit] ボタンを押したときに、検索文字列が変更されます。 その場合は、`searchString` パラメーターは null ではありません。

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

`Index` メソッドの最後に、`PaginatedList.CreateAsync` メソッドが、ページングをサポートするコレクション型の受講者の 1 つのページに受講者クエリを変換します。 その 1 つの受講者のページがビューに渡されます。

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` メソッドは、ページ番号を受け取ります。 2 つの疑問符は、null 合体演算子を表します。 null 合体演算子は null 許容型の既定値を定義します。式 `(page ?? 1)` は、値がある場合は `page` の値を返し、`page` が null の場合は 1 を返すことを意味します。

## <a name="add-paging-links-to-the-student-index-view"></a>Student インデックス ビューにページングのリンクを追加する

*Views/Students/Index.cshtml* で、既存のコードを次のコードに置き換えます。 変更が強調表示されています。

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

ページの上部にある `@model` ステートメントは、ビューが `List<T>` オブジェクトの代わりに `PaginatedList<T>` オブジェクトを取得するようになったことを指定します。

列ヘッダーへのリンクは、フィルターの結果内でユーザーが並べ替えられるように、クエリ文字列を使用してコントローラーに現在の検索文字列を渡します。

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

タグ ヘルパーによってページング ボタンが表示されます。

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

アプリを実行して [Students] ページに移動します。

![ページング リンクを含む Students インデックス ページ](sort-filter-page/_static/paging.png)

異なる並べ替え順でページングのリンクをクリックし、ページングが機能することを確認します。 その後で、検索文字列を入力して、ページングをもう一度試し、並べ替えとフィルター処理を使用してもページングが正しく機能することを確認します。

## <a name="create-an-about-page-that-shows-student-statistics"></a>受講者の統計情報を表示する [About] ページを作成する

Contoso 大学の Web サイトの **[About]** ページに、登録日付ごとに登録した受講者の数が表示されます。 これには、グループ化とグループに関する簡単な計算が必要です。 これを実行するためには、次の手順を実行します。

* ビューに渡す必要があるデータのビュー モデル クラスを作成します。

* Home コントローラーで About メソッドを変更します。

* About ビューを変更する

### <a name="create-the-view-model"></a>ビュー モデルを作成する

*SchoolViewModels* フォルダーを *Models* フォルダー内に作成します。

新しいフォルダー内に、*EnrollmentDateGroup.cs* という名前のクラス ファイルを追加し、テンプレート コードを次のコードに置き換えます。

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Home コントローラーを変更する

*HomeController.cs* で、ファイルの先頭に次のステートメントを追加します。

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

クラスの左中かっこの直後に、データベース コンテキストのクラス変数を追加し、ASP.NET Core DI からコンテキストのインスタンスを取得します。

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

`About` メソッドを次のコードで置き換えます。

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ ステートメントは、登録日で受講者エンティティをグループ化し、各グループ内のエンティティの数を計算して、結果を `EnrollmentDateGroup` ビュー モデル オブジェクトのコレクションに格納します。
> [!NOTE] 
> 1.0 のバージョンの Entity Framework Core では、結果セット全体がクライアントに返され、クライアントでグループ化が行われます。 一部のシナリオでは、このためにパフォーマンスの問題が発生する可能性があります。 必ず実稼働時のデータ量でパフォーマンスをテストし、必要な場合、未加工の SQL を使用してサーバーのグループ化を行ってください。 未加工の SQL の使用方法については、[このシリーズの最後のチュートリアル](advanced.md)を参照してください。

### <a name="modify-the-about-view"></a>About ビューを変更する

*Views/Home/About.cshtml* ファイルのコードを次のコードに置き換えます。

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

アプリを実行して [About] ページに移動します。 登録の日付ごとの受講者の数が、テーブルに表示されます。

![About ページ](sort-filter-page/_static/about.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、並べ替え、フィルター処理、ページング、およびグループ化を実行する方法を説明しました。 次のチュートリアルでは、移行を使用して、データ モデルの変更を処理する方法を学習します。

> [!div class="step-by-step"]
> [前へ](crud.md)
> [次へ](migrations.md)  
