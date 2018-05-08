---
title: ASP.NET Core MVC と EF Core - 並べ替え、フィルター、ページング - 3/10
author: tdykstra
description: このチュートリアルでは、ASP.NET Core および Entity Framework Core を使用して並べ替え、フィルター、ページング機能をページに追加します。
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: d4fe6386318210a751d1248c87299d414ab563a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a><span data-ttu-id="6d119-103">ASP.NET Core MVC と EF Core - 並べ替え、フィルター、ページング - 3/10</span><span class="sxs-lookup"><span data-stu-id="6d119-103">ASP.NET Core MVC with EF Core - Sort, Filter, Paging - 3 of 10</span></span>

<span data-ttu-id="6d119-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6d119-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6d119-105">Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6d119-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="6d119-106">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d119-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="6d119-107">前のチュートリアルでは、Student エンティティの基本的な CRUD 操作用の Web ページのセットを実装しました。</span><span class="sxs-lookup"><span data-stu-id="6d119-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="6d119-108">このチュートリアルでは、Students インデックス ページに並べ替え、フィルター、およびページング機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="6d119-109">単純なグループ化を実行するページも作成します。</span><span class="sxs-lookup"><span data-stu-id="6d119-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="6d119-110">次の図は、作業が終了したときにページがどのように表示されるかを示しています。</span><span class="sxs-lookup"><span data-stu-id="6d119-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="6d119-111">列見出しとは、ユーザーがクリックしてその列で並べ替えを行うことができるリンクです。</span><span class="sxs-lookup"><span data-stu-id="6d119-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="6d119-112">列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序が切り替えられます。</span><span class="sxs-lookup"><span data-stu-id="6d119-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students インデックス ページ](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="6d119-114">Students インデックス ページに列の並べ替えリンクを追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="6d119-115">Students ンデックス ページに並べ替えを追加するには、`Index`Students コントローラーのメソッドを変更し、Students インデックス ビューにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="6d119-116">Index メソッドに並べ替え機能を追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="6d119-117">*StudentsController.cs* で、`Index` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6d119-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="6d119-118">このコードは、URL 内の文字列から `sortOrder` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="6d119-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="6d119-119">クエリ文字列の値は、ASP.NET Core MVC によってパラメーターとしてアクション メソッドに提供されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="6d119-120">パラメータは、"Name" または "Date" という文字列で、その後に必要に応じてアンダースコアと降順を指定する文字列 "desc" が続きます。</span><span class="sxs-lookup"><span data-stu-id="6d119-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="6d119-121">既定の並べ替え順序は昇順です。</span><span class="sxs-lookup"><span data-stu-id="6d119-121">The default sort order is ascending.</span></span>

<span data-ttu-id="6d119-122">インデックス ページが初めて要求されたときには、クエリ文字列はありません。</span><span class="sxs-lookup"><span data-stu-id="6d119-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="6d119-123">受講者は、姓の昇順で表示されます。これは、`switch` ステートメントでフォールスルー ケースによって確立される既定値です。</span><span class="sxs-lookup"><span data-stu-id="6d119-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="6d119-124">ユーザーが列見出しハイパーリンクをクリックすると、適切な `sortOrder` 値がクエリ文字列で提供されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="6d119-125">2 つの `ViewData` 要素 (NameSortParm と DateSortParm) をビューで使用して、適切なクエリ文字列値を持つ列見出しのハイパーリンクを構成します。</span><span class="sxs-lookup"><span data-stu-id="6d119-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="6d119-126">これらは、三項ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="6d119-126">These are ternary statements.</span></span> <span data-ttu-id="6d119-127">最初のステートメントは、`sortOrder` パラメーターが null または空の場合に、NameSortParm を "name_desc" に設定し、それ以外の場合は任意の空の文字列に設定する必要があることを指定します。</span><span class="sxs-lookup"><span data-stu-id="6d119-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="6d119-128">これらの 2 つのステートメントを使用して、次のようにビューで列見出しのハイパーリンクの設定することができます。</span><span class="sxs-lookup"><span data-stu-id="6d119-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="6d119-129">既定の並べ替え順</span><span class="sxs-lookup"><span data-stu-id="6d119-129">Current sort order</span></span>  | <span data-ttu-id="6d119-130">姓のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="6d119-130">Last Name Hyperlink</span></span> | <span data-ttu-id="6d119-131">日付のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="6d119-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="6d119-132">姓の昇順</span><span class="sxs-lookup"><span data-stu-id="6d119-132">Last Name ascending</span></span>  | <span data-ttu-id="6d119-133">descending</span><span class="sxs-lookup"><span data-stu-id="6d119-133">descending</span></span>          | <span data-ttu-id="6d119-134">ascending</span><span class="sxs-lookup"><span data-stu-id="6d119-134">ascending</span></span>      |
| <span data-ttu-id="6d119-135">姓の降順</span><span class="sxs-lookup"><span data-stu-id="6d119-135">Last Name descending</span></span> | <span data-ttu-id="6d119-136">ascending</span><span class="sxs-lookup"><span data-stu-id="6d119-136">ascending</span></span>           | <span data-ttu-id="6d119-137">ascending</span><span class="sxs-lookup"><span data-stu-id="6d119-137">ascending</span></span>      |
| <span data-ttu-id="6d119-138">日付の昇順</span><span class="sxs-lookup"><span data-stu-id="6d119-138">Date ascending</span></span>       | <span data-ttu-id="6d119-139">ascending</span><span class="sxs-lookup"><span data-stu-id="6d119-139">ascending</span></span>           | <span data-ttu-id="6d119-140">descending</span><span class="sxs-lookup"><span data-stu-id="6d119-140">descending</span></span>     |
| <span data-ttu-id="6d119-141">日付の降順</span><span class="sxs-lookup"><span data-stu-id="6d119-141">Date descending</span></span>      | <span data-ttu-id="6d119-142">ascending</span><span class="sxs-lookup"><span data-stu-id="6d119-142">ascending</span></span>           | <span data-ttu-id="6d119-143">ascending</span><span class="sxs-lookup"><span data-stu-id="6d119-143">ascending</span></span>      |

<span data-ttu-id="6d119-144">このメソッドは、並べ替える列を指定するのに LINQ to Entities を使用します。</span><span class="sxs-lookup"><span data-stu-id="6d119-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="6d119-145">このコードは、switch ステートメントの前に `IQueryable` 変数を作成し、switch ステートメントでそれを変更して、`switch` ステートメントの後に `ToListAsync` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="6d119-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="6d119-146">`IQueryable` 変数を作成および変更するときに、データベースに送信されるクエリはありません。</span><span class="sxs-lookup"><span data-stu-id="6d119-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="6d119-147">クエリは、`ToListAsync` などのメソッドを呼び出すことによって `IQueryable` オブジェクトをコレクションに変換するまで実行されません。</span><span class="sxs-lookup"><span data-stu-id="6d119-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="6d119-148">そのため、このコードの結果として、`return View` ステートメントはで実行されない 1 つのクエリになります。</span><span class="sxs-lookup"><span data-stu-id="6d119-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="6d119-149">このコードは、多数の列によって冗長になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6d119-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="6d119-150">[このシリーズの最後のチュートリアル](advanced.md#dynamic-linq)では、文字列変数で `OrderBy` 列の名前を渡すことができるコードの記述方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="6d119-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="6d119-151">列見出しハイパーリンクを Student インデックス ビューに追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="6d119-152">*Views/Students/Index.cshtml* のコードを次のコードに置き換え、列見出しのハイパーリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="6d119-153">変更された行は強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="6d119-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="6d119-154">このコードは、`ViewData` プロパティ内の情報を使用して、適切なクエリ文字列使用含むハイパーリンクを設定します。</span><span class="sxs-lookup"><span data-stu-id="6d119-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="6d119-155">アプリを実行し、**[Students]** タブを選択して、**[Last Name]** と **[Enrollment Date]** 列見出しをクリックし、並べ替えが機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="6d119-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![名前順の Students インデックス ページ](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="6d119-157">Students インデックス ページに [Search] ボックスを追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="6d119-158">Students インデックス ページにフィルターを追加するには、テキスト ボックスと送信ボタンをビューに追加し、`Index` メソッドで対応する変更を行います。</span><span class="sxs-lookup"><span data-stu-id="6d119-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="6d119-159">テキスト ボックスを使用して、姓と名のフィールドに検索する文字列を入力できます。</span><span class="sxs-lookup"><span data-stu-id="6d119-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="6d119-160">Index メソッドにフィルター機能を追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="6d119-161">*StudentsController.cs* で、`Index` メソッドを次のコードで置き換えます (変更部分が強調表示されています)。</span><span class="sxs-lookup"><span data-stu-id="6d119-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="6d119-162">`searchString` パラメーターを `Index` メソッドに追加しました。</span><span class="sxs-lookup"><span data-stu-id="6d119-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="6d119-163">インデックス ビューに追加するテキスト ボックスから検索する文字列値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="6d119-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="6d119-164">さらに LINQ ステートメントに where 句を追加し、姓または名に検索文字列を含む受講者のみを選択します。</span><span class="sxs-lookup"><span data-stu-id="6d119-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="6d119-165">where 句を追加するステートメントは、検索する値がある場合にのみ、実行されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="6d119-166">ここで、`IQueryable` オブジェクトに対して `Where` メソッドを呼び出し、フィルターがサーバーで処理されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="6d119-167">一部のシナリオでは、`Where` メソッドをメモリ内コレクションの拡張メソッドとして呼び出す場合があります </span><span class="sxs-lookup"><span data-stu-id="6d119-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="6d119-168">(たとえば、EF `DbSet` の代わりに `IEnumerable` コレクションを返すリポジトリ メソッドを参照するように参照を `_context.Students` に変更する場合)。結果は、通常同じになりますが、場合によっては異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="6d119-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="6d119-169">たとえば、.NET Framework の `Contains` メソッドの実装は、既定では大文字小文字を区別する比較を実行しますが、SQL Server では、これは SQL Server インスタンスの照合順序の設定によって決まります。</span><span class="sxs-lookup"><span data-stu-id="6d119-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="6d119-170">その設定は、既定では大文字小文字を区別しません。</span><span class="sxs-lookup"><span data-stu-id="6d119-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="6d119-171">`ToUpper` メソッドを呼び出して明示的に大文字小文字を区別しないテストを作成することもできます: *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*。</span><span class="sxs-lookup"><span data-stu-id="6d119-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="6d119-172">これにより、`IQueryable` オブジェクトの代わりに `IEnumerable` コレクションを返すリポジトリを使用するように後でコードを変更した場合でも確実に同じ結果になるようにすることができます </span><span class="sxs-lookup"><span data-stu-id="6d119-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="6d119-173">(`IEnumerable` コレクションに対して `Contains` メソッドを呼び出したときには、.NET Framework の実装を取得します。`IQueryable` オブジェクトに対して呼び出したときには、データベース プロバイダーの実装を取得します)。ただし、このソリューションではパフォーマンスが低下します。</span><span class="sxs-lookup"><span data-stu-id="6d119-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="6d119-174">`ToUpper` コードは、TSQL SELECT ステートメントの WHERE 句に関数を格納します。</span><span class="sxs-lookup"><span data-stu-id="6d119-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="6d119-175">これにより、オプティマイザーはインデックスを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="6d119-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="6d119-176">ほとんどの場合、SQL は大文字小文字を区別しないようにインストールされているため、大文字小文字を区別するデータストアに移行するまで `ToUpper` コードを避けることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6d119-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="6d119-177">Students インデックス ビューに [Search] ボックスを追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="6d119-178">*Views/Student/Index.cshtml* で、キャプション、テキスト ボックス、**[Search]** ボタンを作成するために、オープニング テーブル タグの直前に強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="6d119-179">このコードは、`<form>` [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使用して、検索テキスト ボックスとボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="6d119-180">既定では、`<form>` タグ ヘルパーは、POST を使用してフォーム データを送信します。これは、パラメーターが、クエリ文字列として URL で渡されるのではなく、HTTP メッセージの本文で渡されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="6d119-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="6d119-181">HTTP GET を指定すると、フォーム データがクエリ文字列として URL で渡され、ユーザーが URL をブックマークできるようになります。</span><span class="sxs-lookup"><span data-stu-id="6d119-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="6d119-182">アクションの結果として更新されない場合、W3C のガイドラインでは、Get の使用が推奨されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="6d119-183">アプリを実行し、**[Students]** タブを選択して、検索文字列を入力し、[Search] をクリックして、フィルターが機能していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6d119-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![フィルターを含む Students インデックス ページ](sort-filter-page/_static/filtering.png)

<span data-ttu-id="6d119-185">URL に検索文字列が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6d119-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="6d119-186">このページをブックマークに設定した場合、ブックマークを使用するときに、フィルター処理された一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="6d119-187">`method="get"` を `form` タグに追加すると、クエリ文字列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="6d119-188">この段階で、列見出しのソートのリンクをクリックすると、**[Search]** ボックスに入力したフィルター値が失われます。</span><span class="sxs-lookup"><span data-stu-id="6d119-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="6d119-189">次のセクションでこれを修正します。</span><span class="sxs-lookup"><span data-stu-id="6d119-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="6d119-190">Students インデックス ページにページング機能を追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="6d119-191">Students インデックス ページにページングを追加するには、常にテーブルをすべての行を取得する代わりに、`Skip` および `Take` ステートメントを使用してサーバー上でデータをフィルター処理する `PaginatedList` クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d119-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="6d119-192">その後で、`Index` メソッドに変更を加え、ページング ボタンを `Index` ビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="6d119-193">ページング ボタンを次の図に示します。</span><span class="sxs-lookup"><span data-stu-id="6d119-193">The following illustration shows the paging buttons.</span></span>

![ページング リンクを含む Students インデックス ページ](sort-filter-page/_static/paging.png)

<span data-ttu-id="6d119-195">プロジェクト フォルダーで、`PaginatedList.cs` を作成し、テンプレートのコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6d119-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="6d119-196">このコードの `CreateAsync` メソッドは、ページ サイズとページ番号を受け取り、適切な `Skip` および `Take` ステートメントを `IQueryable` に適用します。</span><span class="sxs-lookup"><span data-stu-id="6d119-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="6d119-197">`IQueryable` で `ToListAsync` が呼び出されると、要求されたページのみを含むリストを返します。</span><span class="sxs-lookup"><span data-stu-id="6d119-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="6d119-198">プロパティ `HasPreviousPage` および `HasNextPage` を使用して、**[Previous]** および **[Next]** ページング ボタンを有効または無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="6d119-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="6d119-199">コンストラクターは非同期コードを実行できないので、コンストラクターの代わりに `CreateAsync` メソッドを使用して `PaginatedList<T>`オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d119-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="6d119-200">Index メソッドにページング機能を追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="6d119-201">*StudentsController.cs* で、`Index` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6d119-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="6d119-202">このコードは、メソッド シグネチャにページ番号パラメーター、現在の並べ替え順序パラメーター、および現在のフィルター パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="6d119-203">最初にページが表示されるとき、またはユーザーがページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。</span><span class="sxs-lookup"><span data-stu-id="6d119-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="6d119-204">ページングのリンクをクリックすると、ページ変数に表示するページ番号が含まれます。</span><span class="sxs-lookup"><span data-stu-id="6d119-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="6d119-205">CurrentSort という名前の `ViewData` 要素が、現在の並べ替え順序をビューに提供します。ページング中に同じ並べ替え順序を維持するために、ページングのリンクにこれを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d119-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="6d119-206">CurrentFilter という名前の `ViewData` 要素が現在のフィルター文字列をビューに提供します。</span><span class="sxs-lookup"><span data-stu-id="6d119-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="6d119-207">ページング中にフィルターの設定を維持するために、ページングのリンクにこの値を含める必要があり、ページが再表示されるときに、この値をテキスト ボックスに復元する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d119-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="6d119-208">ページングの中に検索文字列を変更した場合は、新しいフィルターのために別のデータが表示されるため、ページを 1 にリセットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d119-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="6d119-209">テキスト ボックスに値を入力して、[Submit] ボタンを押したときに、検索文字列が変更されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="6d119-210">その場合は、`searchString` パラメーターは null ではありません。</span><span class="sxs-lookup"><span data-stu-id="6d119-210">In that case, the `searchString` parameter isn't null.</span></span>

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

<span data-ttu-id="6d119-211">`Index` メソッドの最後に、`PaginatedList.CreateAsync` メソッドが、ページングをサポートするコレクション型の受講者の 1 つのページに受講者クエリを変換します。</span><span class="sxs-lookup"><span data-stu-id="6d119-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="6d119-212">その 1 つの受講者のページがビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="6d119-213">`PaginatedList.CreateAsync` メソッドは、ページ番号を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="6d119-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="6d119-214">2 つの疑問符は、null 合体演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="6d119-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="6d119-215">null 合体演算子は null 許容型の既定値を定義します。式 `(page ?? 1)` は、値がある場合は `page` の値を返し、`page` が null の場合は 1 を返すことを意味します。</span><span class="sxs-lookup"><span data-stu-id="6d119-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="6d119-216">Student インデックス ビューにページングのリンクを追加する</span><span class="sxs-lookup"><span data-stu-id="6d119-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="6d119-217">*Views/Students/Index.cshtml* で、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6d119-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="6d119-218">変更が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="6d119-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="6d119-219">ページの上部にある `@model` ステートメントは、ビューが `List<T>` オブジェクトの代わりに `PaginatedList<T>` オブジェクトを取得するようになったことを指定します。</span><span class="sxs-lookup"><span data-stu-id="6d119-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="6d119-220">列ヘッダーへのリンクは、フィルターの結果内でユーザーが並べ替えられるように、クエリ文字列を使用してコントローラーに現在の検索文字列を渡します。</span><span class="sxs-lookup"><span data-stu-id="6d119-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="6d119-221">タグ ヘルパーによってページング ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="6d119-222">アプリを実行して [Students] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="6d119-222">Run the app and go to the Students page.</span></span>

![ページング リンクを含む Students インデックス ページ](sort-filter-page/_static/paging.png)

<span data-ttu-id="6d119-224">異なる並べ替え順でページングのリンクをクリックし、ページングが機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="6d119-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="6d119-225">その後で、検索文字列を入力して、ページングをもう一度試し、並べ替えとフィルター処理を使用してもページングが正しく機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="6d119-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="6d119-226">受講者の統計情報を表示する [About] ページを作成する</span><span class="sxs-lookup"><span data-stu-id="6d119-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="6d119-227">Contoso 大学の Web サイトの **[About]** ページに、登録日付ごとに登録した受講者の数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="6d119-228">これには、グループ化とグループに関する簡単な計算が必要です。</span><span class="sxs-lookup"><span data-stu-id="6d119-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="6d119-229">これを実行するためには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="6d119-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="6d119-230">ビューに渡す必要があるデータのビュー モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d119-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="6d119-231">Home コントローラーで About メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="6d119-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="6d119-232">About ビューを変更する</span><span class="sxs-lookup"><span data-stu-id="6d119-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="6d119-233">ビュー モデルを作成する</span><span class="sxs-lookup"><span data-stu-id="6d119-233">Create the view model</span></span>

<span data-ttu-id="6d119-234">*SchoolViewModels* フォルダーを *Models* フォルダー内に作成します。</span><span class="sxs-lookup"><span data-stu-id="6d119-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="6d119-235">新しいフォルダー内に、*EnrollmentDateGroup.cs* という名前のクラス ファイルを追加し、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6d119-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="6d119-236">Home コントローラーを変更する</span><span class="sxs-lookup"><span data-stu-id="6d119-236">Modify the Home Controller</span></span>

<span data-ttu-id="6d119-237">*HomeController.cs* で、ファイルの先頭に次のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d119-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="6d119-238">クラスの左中かっこの直後に、データベース コンテキストのクラス変数を追加し、ASP.NET Core DI からコンテキストのインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="6d119-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="6d119-239">`About` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6d119-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="6d119-240">LINQ ステートメントは、登録日で受講者エンティティをグループ化し、各グループ内のエンティティの数を計算して、結果を `EnrollmentDateGroup` ビュー モデル オブジェクトのコレクションに格納します。</span><span class="sxs-lookup"><span data-stu-id="6d119-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="6d119-241">1.0 のバージョンの Entity Framework Core では、結果セット全体がクライアントに返され、クライアントでグループ化が行われます。</span><span class="sxs-lookup"><span data-stu-id="6d119-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="6d119-242">一部のシナリオでは、このためにパフォーマンスの問題が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6d119-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="6d119-243">必ず実稼働時のデータ量でパフォーマンスをテストし、必要な場合、未加工の SQL を使用してサーバーのグループ化を行ってください。</span><span class="sxs-lookup"><span data-stu-id="6d119-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="6d119-244">未加工の SQL の使用方法については、[このシリーズの最後のチュートリアル](advanced.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6d119-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="6d119-245">About ビューを変更する</span><span class="sxs-lookup"><span data-stu-id="6d119-245">Modify the About View</span></span>

<span data-ttu-id="6d119-246">*Views/Home/About.cshtml* ファイルのコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6d119-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="6d119-247">アプリを実行して [About] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="6d119-247">Run the app and go to the About page.</span></span> <span data-ttu-id="6d119-248">登録の日付ごとの受講者の数が、テーブルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="6d119-248">The count of students for each enrollment date is displayed in a table.</span></span>

![About ページ](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="6d119-250">まとめ</span><span class="sxs-lookup"><span data-stu-id="6d119-250">Summary</span></span>

<span data-ttu-id="6d119-251">このチュートリアルでは、並べ替え、フィルター処理、ページング、およびグループ化を実行する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="6d119-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="6d119-252">次のチュートリアルでは、移行を使用して、データ モデルの変更を処理する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6d119-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6d119-253">[前へ](crud.md)
> [次へ](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="6d119-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
