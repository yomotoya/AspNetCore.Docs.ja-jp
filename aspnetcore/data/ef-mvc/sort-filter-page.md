---
title: "ASP.NET MVC を持つコアを EF Core - 並べ替え、フィルター、ページング - 10 3"
author: tdykstra
description: "このチュートリアルでは、並べ替え、フィルター、およびページング ASP.NET Core および Entity Framework のコアを使用して 1 ページに機能を追加します。"
keywords: "ASP.NET Core、Entity Framework Core、並べ替え、フィルター、ページング、グループ化"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 59fff4dbf4736f0776aac4072f3f4e2d40119842
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="1a619-104">並べ替え、フィルター、ページング、およびグループ化 - ASP.NET Core MVC のチュートリアル (10 の 3) と EF コア</span><span class="sxs-lookup"><span data-stu-id="1a619-104">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="1a619-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a619-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a619-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1a619-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="1a619-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="1a619-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="1a619-108">前のチュートリアルでは、Student エンティティの基本的な CRUD 操作用の web ページのセットを実装します。</span><span class="sxs-lookup"><span data-stu-id="1a619-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="1a619-109">このチュートリアルでは、受講者インデックス ページに並べ替え、フィルター、およびページング機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-109">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="1a619-110">単純なグループ化を実行するページを作成することもあります。</span><span class="sxs-lookup"><span data-stu-id="1a619-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="1a619-111">次の図は、ページがどのようにしたらを示します。</span><span class="sxs-lookup"><span data-stu-id="1a619-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="1a619-112">列見出しとは、その列で並べ替えを行うユーザーがクリックするリンクです。</span><span class="sxs-lookup"><span data-stu-id="1a619-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="1a619-113">列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序を切り替えます。</span><span class="sxs-lookup"><span data-stu-id="1a619-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![インデックス ページの受講者](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="1a619-115">受講者インデックス ページに列の並べ替えのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-115">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="1a619-116">学生インデックス ページに並べ替えを追加するを変更、`Index`受講者コント ローラーのメソッド学生インデックス ビューにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-116">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="1a619-117">Index メソッドに並べ替え機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-117">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="1a619-118">*StudentsController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="1a619-118">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="1a619-119">このコードを受け取る、 `sortOrder` URL 内のクエリ文字列からのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="1a619-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="1a619-120">クエリ文字列の値は、アクション メソッドにパラメーターとして ASP.NET Core MVC によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-120">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="1a619-121">パラメーターが"Name"または「日付」、必要に応じて後にアンダー スコアと降順の順序を指定するには、"desc"文字列を文字列になります。</span><span class="sxs-lookup"><span data-stu-id="1a619-121">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="1a619-122">既定の並べ替え順序は昇順です。</span><span class="sxs-lookup"><span data-stu-id="1a619-122">The default sort order is ascending.</span></span>

<span data-ttu-id="1a619-123">インデックス ページが要求されると、最初にクエリ文字列はありません。</span><span class="sxs-lookup"><span data-stu-id="1a619-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="1a619-124">受講者は、姓、名、フォール スルー大文字小文字によって設定されるは、既定で昇順に表示される、`switch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="1a619-124">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="1a619-125">ユーザーが列見出し、ハイパーリンク、適切な`sortOrder`クエリ文字列の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="1a619-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="1a619-126">2 つ`ViewData`要素 (NameSortParm および DateSortParm) を使用して、ビューによって適切なクエリ文字列の値を列見出しのハイパーリンクを構成します。</span><span class="sxs-lookup"><span data-stu-id="1a619-126">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="1a619-127">これらは、三項ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="1a619-127">These are ternary statements.</span></span> <span data-ttu-id="1a619-128">最初の 1 つを指定する場合、`sortOrder`パラメーターが null または空で、NameSortParm に設定してください"name_desc"以外の場合は、空の文字列にそれ以外の場合、設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a619-128">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="1a619-129">これら 2 つのステートメントでは、ビュー、列見出しのハイパーリンクの次のように設定を有効にします。</span><span class="sxs-lookup"><span data-stu-id="1a619-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="1a619-130">現在の並べ替え順序</span><span class="sxs-lookup"><span data-stu-id="1a619-130">Current sort order</span></span>  | <span data-ttu-id="1a619-131">名の最後のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="1a619-131">Last Name Hyperlink</span></span> | <span data-ttu-id="1a619-132">日付のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="1a619-132">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="1a619-133">最後の名前昇順</span><span class="sxs-lookup"><span data-stu-id="1a619-133">Last Name ascending</span></span>  | <span data-ttu-id="1a619-134">descending</span><span class="sxs-lookup"><span data-stu-id="1a619-134">descending</span></span>          | <span data-ttu-id="1a619-135">ascending</span><span class="sxs-lookup"><span data-stu-id="1a619-135">ascending</span></span>      |
| <span data-ttu-id="1a619-136">最後の名前が降順</span><span class="sxs-lookup"><span data-stu-id="1a619-136">Last Name descending</span></span> | <span data-ttu-id="1a619-137">ascending</span><span class="sxs-lookup"><span data-stu-id="1a619-137">ascending</span></span>           | <span data-ttu-id="1a619-138">ascending</span><span class="sxs-lookup"><span data-stu-id="1a619-138">ascending</span></span>      |
| <span data-ttu-id="1a619-139">日付の昇順</span><span class="sxs-lookup"><span data-stu-id="1a619-139">Date ascending</span></span>       | <span data-ttu-id="1a619-140">ascending</span><span class="sxs-lookup"><span data-stu-id="1a619-140">ascending</span></span>           | <span data-ttu-id="1a619-141">descending</span><span class="sxs-lookup"><span data-stu-id="1a619-141">descending</span></span>     |
| <span data-ttu-id="1a619-142">日付 (降順)</span><span class="sxs-lookup"><span data-stu-id="1a619-142">Date descending</span></span>      | <span data-ttu-id="1a619-143">ascending</span><span class="sxs-lookup"><span data-stu-id="1a619-143">ascending</span></span>           | <span data-ttu-id="1a619-144">ascending</span><span class="sxs-lookup"><span data-stu-id="1a619-144">ascending</span></span>      |

<span data-ttu-id="1a619-145">メソッドは、並べ替える列を指定するのに LINQ to Entities を使用します。</span><span class="sxs-lookup"><span data-stu-id="1a619-145">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="1a619-146">このコードを作成、 `IQueryable` switch ステートメントの前に変数を呼び出し、switch ステートメントで変更、`ToListAsync`メソッドした後に、`switch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="1a619-146">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="1a619-147">作成および変更するときに`IQueryable`変数、クエリがないデータベースに送信します。</span><span class="sxs-lookup"><span data-stu-id="1a619-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="1a619-148">変換するまでに、クエリが実行されていません、`IQueryable`などのメソッドを呼び出すことで、コレクションにオブジェクト`ToListAsync`です。</span><span class="sxs-lookup"><span data-stu-id="1a619-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="1a619-149">そのため、このコードなりますまで実行されない 1 つのクエリで、`return View`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="1a619-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="1a619-150">このコードは、列の数が多い verbose 取得でした。</span><span class="sxs-lookup"><span data-stu-id="1a619-150">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="1a619-151">[このシリーズの前回のチュートリアル](advanced.md#dynamic-linq)コードの名前を渡すことができますを記述する方法を示します、`OrderBy`文字列変数内の列です。</span><span class="sxs-lookup"><span data-stu-id="1a619-151">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="1a619-152">学生インデックス ビューに列見出しのハイパーリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-152">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="1a619-153">コードに置き換えます*Views/Students/Index.cshtml*、列見出しのハイパーリンクを追加する次のコードにします。</span><span class="sxs-lookup"><span data-stu-id="1a619-153">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="1a619-154">変更された行が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-154">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="1a619-155">このコード内の情報を使用して`ViewData`適切なクエリでハイパーリンクを設定するプロパティの文字列値です。</span><span class="sxs-lookup"><span data-stu-id="1a619-155">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="1a619-156">アプリを実行する、選択、**受講者**タブをクリックし、をクリックして、**姓**と**登録日**を並べ替えることを確認する列見出しが動作します。</span><span class="sxs-lookup"><span data-stu-id="1a619-156">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![受講者名の順にページをインデックスします。](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="1a619-158">受講者インデックス ページに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-158">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="1a619-159">受講者インデックス ページにフィルターを追加するにをビューにテキスト ボックスと [送信] ボタンを追加しで対応する変更を加え、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1a619-159">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="1a619-160">テキスト ボックスを使用すると、姓と名の最後のフィールドで検索する文字列を入力できます。</span><span class="sxs-lookup"><span data-stu-id="1a619-160">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="1a619-161">Index メソッドにフィルター処理機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-161">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="1a619-162">*StudentsController.cs*、置換、`Index`メソッドを次のコード (変更が強調表示されます)。</span><span class="sxs-lookup"><span data-stu-id="1a619-162">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="1a619-163">追加した、`searchString`パラメーターを`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1a619-163">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="1a619-164">インデックス ビューに追加するテキスト ボックスから、検索する文字列値を受信します。</span><span class="sxs-lookup"><span data-stu-id="1a619-164">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="1a619-165">またに追加した LINQ ステートメントの where 句検索文字列を含む文字列名または姓を持つ受講者のみを選択します。</span><span class="sxs-lookup"><span data-stu-id="1a619-165">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="1a619-166">Where を追加するステートメントを検索する値がある場合にのみ、この句を実行します。</span><span class="sxs-lookup"><span data-stu-id="1a619-166">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="1a619-167">ここでの呼び出しには、`Where`メソッドを`IQueryable`オブジェクト、およびフィルターは、サーバーで処理されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-167">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="1a619-168">一部のシナリオでする可能性がありますを呼び出すことの`Where`メモリ内コレクションの拡張メソッドとしてメソッドです。</span><span class="sxs-lookup"><span data-stu-id="1a619-168">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="1a619-169">(たとえばへの参照を変更すると仮定`_context.Students`、EF の代わりに`DbSet`リポジトリ メソッドを表すオブジェクトを参照して、`IEnumerable`コレクション)。結果は、通常同じになりますが、場合によっては異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="1a619-169">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="1a619-170">.NET Framework の実装など、`Contains`メソッドが既定では、大文字小文字の比較を実行しますが、SQL Server では、SQL Server インスタンスの照合順序の設定によって決定これはします。</span><span class="sxs-lookup"><span data-stu-id="1a619-170">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="1a619-171">その設定は、大文字と小文字を既定値です。</span><span class="sxs-lookup"><span data-stu-id="1a619-171">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="1a619-172">呼び出すことも、`ToUpper`を明示的に大文字と小文字、テストを作成するメソッド:*場所 (s = > s.LastName.ToUpper() です。Contains(searchString.ToUpper())*です。</span><span class="sxs-lookup"><span data-stu-id="1a619-172">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="1a619-173">保つ結果、同じを返すリポジトリを使用するには、後でコードを変更する場合、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1a619-173">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="1a619-174">(を呼び出すと、`Contains`メソッドを`IEnumerable`.NET Framework の実装を取得する、コレクション以外のときに呼び出す、`IQueryable`オブジェクト、データベース プロバイダーの実装を取得します)。ただし、このソリューションのパフォーマンスの低下があります。</span><span class="sxs-lookup"><span data-stu-id="1a619-174">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there is a performance penalty for this solution.</span></span> <span data-ttu-id="1a619-175">`ToUpper`コードは、TSQL の SELECT ステートメントの WHERE 句で関数を格納します。</span><span class="sxs-lookup"><span data-stu-id="1a619-175">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="1a619-176">ため、オプティマイザーはインデックスを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="1a619-176">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="1a619-177">大文字と小文字 SQL がインストールされているほとんどの場合、あることをお勧めを回避するのには`ToUpper`大文字小文字を区別データ ストアに移行するまでのコードします。</span><span class="sxs-lookup"><span data-stu-id="1a619-177">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="1a619-178">学生インデックス ビューに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-178">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="1a619-179">*Views/Student/Index.cshtml*、キャプション、テキスト ボックスを作成するためにテーブル タグを開始する直前に強調表示されたコードを追加し、**検索**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a619-179">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="1a619-180">このコードを使用して、 `<form>` [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)検索テキスト ボックスとボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-180">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="1a619-181">既定では、`<form>`タグ ヘルパーが、パラメーターとして渡される HTTP メッセージの本文では、URL ではなくクエリ文字列は、投稿にフォーム データを送信します。</span><span class="sxs-lookup"><span data-stu-id="1a619-181">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="1a619-182">HTTP GET を指定すると、フォームのデータに渡されます URL クエリ文字列としてユーザーの URL をブックマークすることができます。</span><span class="sxs-lookup"><span data-stu-id="1a619-182">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="1a619-183">アクションが、更新プログラムにならない場合に、W3C のガイドラインをお勧めしますが使用するを取得します。</span><span class="sxs-lookup"><span data-stu-id="1a619-183">The W3C guidelines recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="1a619-184">アプリを実行する、選択、**受講者** タブで、検索文字列を入力し、検索フィルターが機能していることを確認する をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a619-184">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![フィルター処理の受講者インデックス ページ](sort-filter-page/_static/filtering.png)

<span data-ttu-id="1a619-186">URL に検索文字列が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1a619-186">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="1a619-187">このページのブックマークを設定した場合に、ブックマークを使用する場合は、フィルター処理された一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-187">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="1a619-188">追加`method="get"`を`form`はタグを生成するクエリ文字列の原因です。</span><span class="sxs-lookup"><span data-stu-id="1a619-188">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="1a619-189">この段階で、列見出しのソートのリンクをクリックすると失われますで指定したフィルター値、**検索**ボックス。</span><span class="sxs-lookup"><span data-stu-id="1a619-189">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="1a619-190">次のセクションで修正します。</span><span class="sxs-lookup"><span data-stu-id="1a619-190">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="1a619-191">受講者インデックス ページにページング機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-191">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="1a619-192">ページングに追加する、受講者インデックス ページを作成、`PaginatedList`を使用してクラス`Skip`と`Take`を常に、テーブルのすべての行を取得する代わりに、サーバー上のデータをフィルター処理するステートメント。</span><span class="sxs-lookup"><span data-stu-id="1a619-192">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="1a619-193">他の変更を加えるされます、`Index`メソッド ページング ボタンを追加し、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="1a619-193">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="1a619-194">次の図は、ページング ボタンを示しています。</span><span class="sxs-lookup"><span data-stu-id="1a619-194">The following illustration shows the paging buttons.</span></span>

![受講者とページングのリンクのページをインデックスします。](sort-filter-page/_static/paging.png)

<span data-ttu-id="1a619-196">プロジェクト フォルダーに作成`PaginatedList.cs`、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1a619-196">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="1a619-197">`CreateAsync`このコードでメソッドがページ サイズとページ番号を受け取り、適切な適用`Skip`と`Take`ステートメントを`IQueryable`です。</span><span class="sxs-lookup"><span data-stu-id="1a619-197">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="1a619-198">ときに`ToListAsync`で呼び出されると、 `IQueryable`、要求されたページのみを含むリストが返されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-198">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="1a619-199">プロパティ`HasPreviousPage`と`HasNextPage`を有効または無効にするために使用できる**前**と**[次へ]**ボタンをページングします。</span><span class="sxs-lookup"><span data-stu-id="1a619-199">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="1a619-200">A`CreateAsync`メソッドを使用するコンス トラクターではなくを作成、`PaginatedList<T>`オブジェクトのコンス トラクターは、非同期コードを実行できないためです。</span><span class="sxs-lookup"><span data-stu-id="1a619-200">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="1a619-201">Index メソッドにページング機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-201">Add paging functionality to the Index method</span></span>

<span data-ttu-id="1a619-202">*StudentsController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="1a619-202">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="1a619-203">このコードは、メソッド シグネチャにページ番号パラメーター、現在の並べ替え順序パラメーター、および現在のフィルター パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-203">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="1a619-204">最初に、ページが表示されたら、またはユーザーが、ページングや並べ替えのリンクをクリックしていない場合、すべてのパラメーターは null になります。</span><span class="sxs-lookup"><span data-stu-id="1a619-204">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="1a619-205">ページングのリンクをクリックすると、ページ変数は、表示するページ番号が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1a619-205">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="1a619-206">`ViewData` CurrentSort をという名前の要素がこのページング中に同じ並べ替え順序を維持するために、ページングのリンクに含める必要がありますので、現在の並べ替え順序でビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="1a619-206">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="1a619-207">`ViewData` CurrentFilter をという名前の要素が現在のフィルター文字列で使用してビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="1a619-207">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="1a619-208">ページング、中に、フィルターの設定を維持するために、ページングのリンクでこの値を含める必要があるし、ページが表示されときに、テキスト ボックスに復元する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1a619-208">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="1a619-209">ページングの中に検索文字列を変更する場合は、新しいフィルターが、別のデータを表示するため、ページを 1 にリセットします。</span><span class="sxs-lookup"><span data-stu-id="1a619-209">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="1a619-210">テキスト ボックスに値を入力し、[送信] ボタンが押された検索文字列が変更されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-210">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="1a619-211">その場合は、`searchString`パラメーターが null ではありません。</span><span class="sxs-lookup"><span data-stu-id="1a619-211">In that case, the `searchString` parameter is not null.</span></span>

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

<span data-ttu-id="1a619-212">最後に、 `Index` 、メソッド、`PaginatedList.CreateAsync`メソッドは、ページングをサポートするコレクション型の生徒の 1 つのページを学生クエリを変換します。</span><span class="sxs-lookup"><span data-stu-id="1a619-212">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="1a619-213">その 1 つのページの受講者は、ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-213">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="1a619-214">`PaginatedList.CreateAsync`メソッドは、ページ数を取得します。</span><span class="sxs-lookup"><span data-stu-id="1a619-214">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="1a619-215">2 つの疑問符は、null 合体演算子を表します。</span><span class="sxs-lookup"><span data-stu-id="1a619-215">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="1a619-216">Null 合体演算子を null 許容型以外の既定値を定義します式`(page ?? 1)`の値を返すことを意味`page`かどうかは、値を持つまたは 1 を返す`page`が null です。</span><span class="sxs-lookup"><span data-stu-id="1a619-216">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="1a619-217">学生インデックス ビューにページングのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="1a619-217">Add paging links to the Student Index view</span></span>

<span data-ttu-id="1a619-218">*Views/Students/Index.cshtml*、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1a619-218">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="1a619-219">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-219">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="1a619-220">`@model`ページの上部にあるステートメントは、ビューが現在を取得するを指定します、`PaginatedList<T>`オブジェクトの代わりに、`List<T>`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1a619-220">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="1a619-221">列ヘッダーへのリンクは、フィルターの結果内で、ユーザーが並べ替えられるように、コント ローラーに現在の検索文字列を渡すクエリ文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="1a619-221">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="1a619-222">タグ ヘルパーによってページング ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-222">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="1a619-223">アプリを実行して、受講者のページに移動します。</span><span class="sxs-lookup"><span data-stu-id="1a619-223">Run the app and go to the Students page.</span></span>

![受講者とページングのリンクのページをインデックスします。](sort-filter-page/_static/paging.png)

<span data-ttu-id="1a619-225">ページング動作のことを確認する異なる並べ替え順のページングのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1a619-225">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="1a619-226">検索文字列を入力し、ページング ページングも正常に動作する並べ替えとフィルター処理のことを確認するには、もう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="1a619-226">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="1a619-227">学生の統計情報を表示するバージョン情報 ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a619-227">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="1a619-228">Contoso 大学の web サイトの**に関する** ページで、登録日付ごとに登録した数の受講者を表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="1a619-228">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="1a619-229">これには、グループにグループ化と簡単な計算が必要です。</span><span class="sxs-lookup"><span data-stu-id="1a619-229">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="1a619-230">これを実現するには、次を行います。</span><span class="sxs-lookup"><span data-stu-id="1a619-230">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="1a619-231">ビューに渡す必要があるデータのビュー モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a619-231">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="1a619-232">Home コント ローラーで、バージョン情報メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="1a619-232">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="1a619-233">バージョン情報の表示を変更します。</span><span class="sxs-lookup"><span data-stu-id="1a619-233">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="1a619-234">ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a619-234">Create the view model</span></span>

<span data-ttu-id="1a619-235">作成、 *SchoolViewModels*内のフォルダー、*モデル*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="1a619-235">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="1a619-236">クラス ファイルを追加、新しいフォルダーに*EnrollmentDateGroup.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1a619-236">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="1a619-237">Home コント ローラーの変更します。</span><span class="sxs-lookup"><span data-stu-id="1a619-237">Modify the Home Controller</span></span>

<span data-ttu-id="1a619-238">*HomeController.cs*次の追加ファイルの上部にあるステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1a619-238">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="1a619-239">クラスの始め中かっこの直後に、データベース コンテキストのクラスの変数を追加し、ASP.NET Core DI からコンテキストのインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="1a619-239">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="1a619-240">`About` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1a619-240">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="1a619-241">LINQ ステートメントの登録日で学生エンティティをグループ化、各グループ内のエンティティの数を計算し、結果のコレクションに格納`EnrollmentDateGroup`モデル オブジェクトを表示します。</span><span class="sxs-lookup"><span data-stu-id="1a619-241">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="1a619-242">1.0 のバージョンの Entity Framework Core では、結果セット全体がクライアントに返され、クライアントでグループ化が行われます。</span><span class="sxs-lookup"><span data-stu-id="1a619-242">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="1a619-243">一部のシナリオでは、パフォーマンスの問題を作成このでした。</span><span class="sxs-lookup"><span data-stu-id="1a619-243">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="1a619-244">必ず、データの実稼働ボリュームとパフォーマンスをテストし、必要な場合をサーバーをグループ化を行うには生の SQL を使用してください。</span><span class="sxs-lookup"><span data-stu-id="1a619-244">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="1a619-245">生の SQL を使用する方法については、次を参照してください。[このシリーズの前回のチュートリアル](advanced.md)です。</span><span class="sxs-lookup"><span data-stu-id="1a619-245">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="1a619-246">変更、ビューについて</span><span class="sxs-lookup"><span data-stu-id="1a619-246">Modify the About View</span></span>

<span data-ttu-id="1a619-247">コードで置き換え、 *Views/Home/About.cshtml*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="1a619-247">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="1a619-248">アプリを実行してバージョン情報 ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="1a619-248">Run the app and go to the About page.</span></span> <span data-ttu-id="1a619-249">登録の日付ごとの生徒の数は、テーブルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="1a619-249">The count of students for each enrollment date is displayed in a table.</span></span>

![ページについて](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="1a619-251">概要</span><span class="sxs-lookup"><span data-stu-id="1a619-251">Summary</span></span>

<span data-ttu-id="1a619-252">このチュートリアルでは、並べ替え、フィルター、ページング、およびグループ化を実行する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="1a619-252">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="1a619-253">次のチュートリアルでは、移行を使用して、データ モデルの変更を処理する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="1a619-253">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1a619-254">[前へ](crud.md)
[次へ](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="1a619-254">[Previous](crud.md)
[Next](migrations.md)</span></span>  
