---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework でページング |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 10/08/2018
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9fabb5a90af715d4e96ff79b43bfff5a4600ac08
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912775"
---
# <a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="12386-103">並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework でのページング</span><span class="sxs-lookup"><span data-stu-id="12386-103">Sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application</span></span>

<span data-ttu-id="12386-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="12386-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="12386-105">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="12386-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> <span data-ttu-id="12386-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="12386-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio.</span></span> <span data-ttu-id="12386-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="12386-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="12386-108">前のチュートリアルでの基本的な CRUD 操作の web ページのセットを実装した`Student`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="12386-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="12386-109">このチュートリアルでは、並べ替え、フィルター処理、およびページング機能を追加します、**学生**インデックス ページです。</span><span class="sxs-lookup"><span data-stu-id="12386-109">In this tutorial you'll add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="12386-110">単純なグループ化を実行するページも作成します。</span><span class="sxs-lookup"><span data-stu-id="12386-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="12386-111">次の図は、作業が終了したときにページがどのように表示されるかを示しています。</span><span class="sxs-lookup"><span data-stu-id="12386-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="12386-112">列見出しとは、ユーザーがクリックしてその列で並べ替えを行うことができるリンクです。</span><span class="sxs-lookup"><span data-stu-id="12386-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="12386-113">列見出しを繰り返しクリックすると、昇順と降順の並べ替え順序が切り替えられます。</span><span class="sxs-lookup"><span data-stu-id="12386-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="12386-115">Students インデックス ページに列の並べ替えリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-115">Add column sort links to the Students index page</span></span>

<span data-ttu-id="12386-116">Student インデックス ページに並べ替えを追加、変更します、`Index`のメソッド、`Student`コント ローラーのコードを追加し、`Student`ビューにインデックスします。</span><span class="sxs-lookup"><span data-stu-id="12386-116">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="12386-117">Index メソッドに並べ替え機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-117">Add sorting functionality to the Index method</span></span>

- <span data-ttu-id="12386-118">*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="12386-118">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="12386-119">このコードは、URL 内の文字列から `sortOrder` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="12386-119">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="12386-120">クエリ文字列の値は、アクション メソッドのパラメーターとして、ASP.NET MVC によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="12386-120">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="12386-121">パラメーターは、必要に応じて後にアンダー スコアと降順を指定するには、"desc"文字列は"Name"または「日付」、文字列です。</span><span class="sxs-lookup"><span data-stu-id="12386-121">The parameter is a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="12386-122">既定の並べ替え順序は昇順です。</span><span class="sxs-lookup"><span data-stu-id="12386-122">The default sort order is ascending.</span></span>

<span data-ttu-id="12386-123">インデックス ページが初めて要求されたときには、クエリ文字列はありません。</span><span class="sxs-lookup"><span data-stu-id="12386-123">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="12386-124">受講者がで昇順に表示される`LastName`でフォールスルー ケースによって確立されると、既定値は、`switch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="12386-124">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="12386-125">ユーザーが列見出しハイパーリンクをクリックすると、適切な `sortOrder` 値がクエリ文字列で提供されます。</span><span class="sxs-lookup"><span data-stu-id="12386-125">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="12386-126">2 つ`ViewBag`変数を使用するビューは、適切なクエリ文字列の値を持つ列見出しのハイパーリンクを構成できます。</span><span class="sxs-lookup"><span data-stu-id="12386-126">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="12386-127">これらは、三項ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="12386-127">These are ternary statements.</span></span> <span data-ttu-id="12386-128">1 つ目の指定された場合、`sortOrder`パラメーターが null または空の場合、`ViewBag.NameSortParm`に設定する必要があります"名\_desc"、それ以外の空の文字列に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="12386-128">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="12386-129">これらの 2 つのステートメントを使用して、次のようにビューで列見出しのハイパーリンクの設定することができます。</span><span class="sxs-lookup"><span data-stu-id="12386-129">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="12386-130">既定の並べ替え順</span><span class="sxs-lookup"><span data-stu-id="12386-130">Current sort order</span></span> | <span data-ttu-id="12386-131">姓のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="12386-131">Last Name Hyperlink</span></span> | <span data-ttu-id="12386-132">日付のハイパーリンク</span><span class="sxs-lookup"><span data-stu-id="12386-132">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="12386-133">姓の昇順</span><span class="sxs-lookup"><span data-stu-id="12386-133">Last Name ascending</span></span> | <span data-ttu-id="12386-134">descending</span><span class="sxs-lookup"><span data-stu-id="12386-134">descending</span></span> | <span data-ttu-id="12386-135">ascending</span><span class="sxs-lookup"><span data-stu-id="12386-135">ascending</span></span> |
| <span data-ttu-id="12386-136">姓の降順</span><span class="sxs-lookup"><span data-stu-id="12386-136">Last Name descending</span></span> | <span data-ttu-id="12386-137">ascending</span><span class="sxs-lookup"><span data-stu-id="12386-137">ascending</span></span> | <span data-ttu-id="12386-138">ascending</span><span class="sxs-lookup"><span data-stu-id="12386-138">ascending</span></span> |
| <span data-ttu-id="12386-139">日付の昇順</span><span class="sxs-lookup"><span data-stu-id="12386-139">Date ascending</span></span> | <span data-ttu-id="12386-140">ascending</span><span class="sxs-lookup"><span data-stu-id="12386-140">ascending</span></span> | <span data-ttu-id="12386-141">descending</span><span class="sxs-lookup"><span data-stu-id="12386-141">descending</span></span> |
| <span data-ttu-id="12386-142">日付の降順</span><span class="sxs-lookup"><span data-stu-id="12386-142">Date descending</span></span> | <span data-ttu-id="12386-143">ascending</span><span class="sxs-lookup"><span data-stu-id="12386-143">ascending</span></span> | <span data-ttu-id="12386-144">ascending</span><span class="sxs-lookup"><span data-stu-id="12386-144">ascending</span></span> |

<span data-ttu-id="12386-145">メソッドを使用して[LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)を並べ替え列を指定します。</span><span class="sxs-lookup"><span data-stu-id="12386-145">The method uses [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) to specify the column to sort by.</span></span> <span data-ttu-id="12386-146">コードを作成、<xref:System.Linq.IQueryable%601>する前に変数、`switch`ステートメントでは、変更で、`switch`ステートメント、および呼び出し、`ToList`メソッドの後、`switch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="12386-146">The code creates an <xref:System.Linq.IQueryable%601> variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="12386-147">`IQueryable` 変数を作成および変更するときに、データベースに送信されるクエリはありません。</span><span class="sxs-lookup"><span data-stu-id="12386-147">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="12386-148">変換するまで、クエリが実行されない、`IQueryable`などのメソッドを呼び出すことによって、コレクションにオブジェクト`ToList`します。</span><span class="sxs-lookup"><span data-stu-id="12386-148">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="12386-149">そのため、このコード結果まで実行されない 1 つのクエリ、`return View`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="12386-149">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="12386-150">各並べ替え順序の異なる LINQ ステートメントを記述する代わりに、LINQ ステートメントを動的に作成できます。</span><span class="sxs-lookup"><span data-stu-id="12386-150">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="12386-151">動的な LINQ の詳細については、次を参照してください。[動的 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)します。</span><span class="sxs-lookup"><span data-stu-id="12386-151">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="12386-152">列見出しのハイパーリンクを Student インデックス ビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-152">Add column heading hyperlinks to the Student index view</span></span>

1. <span data-ttu-id="12386-153">*Views\Student\Index.cshtml*、置換、`<tr>`と`<th>`見出し行を強調表示されたコードの要素。</span><span class="sxs-lookup"><span data-stu-id="12386-153">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   <span data-ttu-id="12386-154">このコードは、情報を使用して、`ViewBag`適切なクエリを含むハイパーリンクを設定するプロパティの文字列値。</span><span class="sxs-lookup"><span data-stu-id="12386-154">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

2. <span data-ttu-id="12386-155">ページを実行し、をクリックして、**姓**と**加入契約日**その並べ替えを確認する列見出しに動作します。</span><span class="sxs-lookup"><span data-stu-id="12386-155">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

   ![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

   <span data-ttu-id="12386-157">クリックした後、**姓**見出しで、学生は最後の名前の降順に表示されます。</span><span class="sxs-lookup"><span data-stu-id="12386-157">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

   ![Web ブラウザーで student インデックス ビュー](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="12386-159">Students インデックス ページに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-159">Add a search box to the Students index page</span></span>

<span data-ttu-id="12386-160">Students インデックス ページにフィルターを追加するにが、ビューにテキスト ボックスと送信ボタンを追加しで対応する変更を行い、`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="12386-160">To add filtering to the Students index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="12386-161">テキスト ボックスを使用して、姓と名の最後のフィールドで検索する文字列を入力できます。</span><span class="sxs-lookup"><span data-stu-id="12386-161">The text box lets you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="12386-162">Index メソッドにフィルター機能を追加する</span><span class="sxs-lookup"><span data-stu-id="12386-162">Add filtering functionality to the Index method</span></span>

- <span data-ttu-id="12386-163">*Controllers\StudentController.cs*、置換、`Index`メソッドを次のコード (変更が強調表示されます)。</span><span class="sxs-lookup"><span data-stu-id="12386-163">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="12386-164">コードを追加、`searchString`パラメーターを`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="12386-164">The code adds a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="12386-165">インデックス ビューに追加するテキスト ボックスから検索する文字列値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="12386-165">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="12386-166">さらに、`where`名または姓を持つは、検索文字列を含む受講者のみを選択する LINQ ステートメントの句。</span><span class="sxs-lookup"><span data-stu-id="12386-166">It also adds a `where` clause to the LINQ statement that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="12386-167">ステートメントを追加する、<xref:System.Linq.Queryable.Where%2A>句を検索する値がある場合にのみ実行します。</span><span class="sxs-lookup"><span data-stu-id="12386-167">The statement that adds the <xref:System.Linq.Queryable.Where%2A> clause executes only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="12386-168">多くの場合は、Entity Framework のエンティティ セットまたはメモリ内コレクションの拡張メソッドとして同じメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="12386-168">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="12386-169">結果は、通常は同じが、場合によっては異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="12386-169">The results are normally the same but in some cases may be different.</span></span>
>
> <span data-ttu-id="12386-170">.NET Framework の実装など、`Contains`メソッドが、空の文字列を渡しますが、Entity Framework provider for SQL Server Compact 4.0 は、空の文字列には、ゼロ行を返すときに、すべての行を返します。</span><span class="sxs-lookup"><span data-stu-id="12386-170">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="12386-171">例のコードではそのため (配置すること、`Where`内のステートメント、`if`ステートメント) は、すべてのバージョンの SQL Server と同じ結果を取得することを確認します。</span><span class="sxs-lookup"><span data-stu-id="12386-171">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="12386-172">.NET Framework の実装も、`Contains`メソッドは、既定では、大文字小文字の比較を実行しますが、Entity Framework の SQL Server プロバイダーは、既定では大文字の比較を実行します。</span><span class="sxs-lookup"><span data-stu-id="12386-172">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="12386-173">そのため、呼び出し、`ToUpper`メソッド、テストを明示的に大文字をにより返されます、リポジトリを使用するには、後でコードを変更すると、結果は変化しません、`IEnumerable`コレクションの代わりに、`IQueryable`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="12386-173">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="12386-174">(`IEnumerable` コレクションに対して `Contains` メソッドを呼び出したときには、.NET Framework の実装を取得します。`IQueryable` オブジェクトに対して呼び出したときには、データベース プロバイダーの実装を取得します)。</span><span class="sxs-lookup"><span data-stu-id="12386-174">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
>
> <span data-ttu-id="12386-175">Null 処理が異なる別のデータベース プロバイダーまたは使用する場合も場合があります、`IQueryable`を使用する場合は、オブジェクトを比較する`IEnumerable`コレクション。</span><span class="sxs-lookup"><span data-stu-id="12386-175">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="12386-176">たとえば、一部のシナリオで、`Where`などの条件`table.Column != 0`を持つ列が返されない可能性があります`null`値として。</span><span class="sxs-lookup"><span data-stu-id="12386-176">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="12386-177">詳細については、次を参照してください。 ['where' 句で null 変数が正しく処理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)します。</span><span class="sxs-lookup"><span data-stu-id="12386-177">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="12386-178">Student インデックス ビューに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-178">Add a search box to the Student index view</span></span>

1. <span data-ttu-id="12386-179">*Views\Student\Index.cshtml*、開始する直前に強調表示されたコードを追加`table`キャプション、テキスト ボックスを作成するにはタグと**検索**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="12386-179">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. <span data-ttu-id="12386-180">ページを実行し、検索文字列を入力し をクリックして**検索**フィルターが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="12386-180">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

   ![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

   <span data-ttu-id="12386-182">URL に「、」検索文字列、これをこのページにブックマークを設定した場合はありません一覧を取得するフィルター選択されたブックマークを使用する場合は意味が含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="12386-182">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="12386-183">これは適用されますも、列の並べ替えリンク リスト全体を並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="12386-183">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="12386-184">変更を**検索**フィルター条件のチュートリアルの後半でクエリ文字列を使用するボタン。</span><span class="sxs-lookup"><span data-stu-id="12386-184">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging-to-the-students-index-page"></a><span data-ttu-id="12386-185">Students インデックス ページにページングを追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-185">Add paging to the Students index page</span></span>

<span data-ttu-id="12386-186">Students インデックス ページには、ページングを追加するをインストールして起動します、 **PagedList.Mvc** NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="12386-186">To add paging to the Students index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="12386-187">その後で追加の変更、`Index`メソッドへのページングのリンクを追加し、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="12386-187">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="12386-188">**PagedList.Mvc**優れた多くのページングと ASP.NET MVC でのパッケージの並べ替えの 1 つとここでその使用のためのものとしてその他のオプションの上の推奨事項ではなく、例としてのみです。</span><span class="sxs-lookup"><span data-stu-id="12386-188">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span> <span data-ttu-id="12386-189">次の図は、ページングのリンクを示します。</span><span class="sxs-lookup"><span data-stu-id="12386-189">The following illustration shows the paging links.</span></span>

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="12386-191">PagedList.MVC NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="12386-191">Install the PagedList.MVC NuGet package</span></span>

<span data-ttu-id="12386-192">NuGet **PagedList.Mvc**パッケージが自動的にインストール、 **PagedList**依存関係としてパッケージします。</span><span class="sxs-lookup"><span data-stu-id="12386-192">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="12386-193">**PagedList**パッケージのインストール、`PagedList`のコレクションの種類と拡張子メソッド`IQueryable`と`IEnumerable`コレクション。</span><span class="sxs-lookup"><span data-stu-id="12386-193">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="12386-194">拡張メソッドが 1 つのデータのページを作成、`PagedList`コレクションのうち、`IQueryable`または`IEnumerable`、および`PagedList`いくつかのプロパティとメソッドをページングを容易にするコレクションを提供します。</span><span class="sxs-lookup"><span data-stu-id="12386-194">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="12386-195">**PagedList.Mvc**パッケージは、ページング ボタンを表示するページング ヘルパーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="12386-195">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

1. <span data-ttu-id="12386-196">**ツール**メニューの  **NuGet パッケージ マネージャー**し**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="12386-196">From the **Tools** menu, select **NuGet Package Manager** and then **Package Manager Console**.</span></span>

2. <span data-ttu-id="12386-197">**パッケージ マネージャー コンソール**ウィンドウで、ことを確認、**パッケージ ソース**は**nuget.org**と**既定のプロジェクト**は**ContosoUniversity**、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="12386-197">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

   ```text
   Install-Package PagedList.Mvc
   ```

   ![PagedList.Mvc をインストールします。](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

3. <span data-ttu-id="12386-199">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="12386-199">Build the project.</span></span>

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="12386-200">Index メソッドにページング機能を追加する</span><span class="sxs-lookup"><span data-stu-id="12386-200">Add paging functionality to the Index method</span></span>

1. <span data-ttu-id="12386-201">*Controllers\StudentController.cs*、追加、`using`のステートメント、`PagedList`名前空間。</span><span class="sxs-lookup"><span data-stu-id="12386-201">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. <span data-ttu-id="12386-202">`Index` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="12386-202">Replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   <span data-ttu-id="12386-203">このコードを追加、`page`パラメーター、現在の並べ替え順序パラメーター、およびメソッド シグネチャに、現在のフィルター パラメーター。</span><span class="sxs-lookup"><span data-stu-id="12386-203">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   <span data-ttu-id="12386-204">初めてページが表示されます、またはユーザーには、ページングまたは並べ替えのリンクをクリックしていない、すべてのパラメーターが null です。</span><span class="sxs-lookup"><span data-stu-id="12386-204">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters are null.</span></span> <span data-ttu-id="12386-205">ページングのリンクがクリックされた場合、`page`変数には表示するページ番号が含まれています。</span><span class="sxs-lookup"><span data-stu-id="12386-205">If a paging link is clicked, the `page` variable contains the page number to display.</span></span>

   <span data-ttu-id="12386-206">A`ViewBag`このページングの中に同じ並べ替え順序を維持するために、ページング リンクに含める必要があるために、プロパティは、ビューで、現在の並べ替え順序を提供します。</span><span class="sxs-lookup"><span data-stu-id="12386-206">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   <span data-ttu-id="12386-207">別のプロパティ、 `ViewBag.CurrentFilter`、現在のフィルター文字列でビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="12386-207">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="12386-208">ページング中にフィルターの設定を維持するために、ページングのリンクにこの値を含める必要があり、ページが再表示されるときに、この値をテキスト ボックスに復元する必要があります。</span><span class="sxs-lookup"><span data-stu-id="12386-208">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="12386-209">ページングの中に検索文字列を変更した場合は、新しいフィルターのために別のデータが表示されるため、ページを 1 にリセットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="12386-209">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="12386-210">テキスト ボックスに値を入力し、[送信] ボタンが押されたときに、検索文字列が変更されます。</span><span class="sxs-lookup"><span data-stu-id="12386-210">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="12386-211">その場合は、`searchString`パラメーターが null ではありません。</span><span class="sxs-lookup"><span data-stu-id="12386-211">In that case, the `searchString` parameter is not null.</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   <span data-ttu-id="12386-212">メソッドの最後に、`ToPagedList`拡張メソッド、受講者を`IQueryable`オブジェクトがページングをサポートするコレクション型の受講者の 1 つのページに学生クエリを変換します。</span><span class="sxs-lookup"><span data-stu-id="12386-212">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="12386-213">その学生の 1 つのページは、ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="12386-213">That single page of students is then passed to the view:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   <span data-ttu-id="12386-214">`ToPagedList` メソッドは、ページ番号を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="12386-214">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="12386-215">2 つの疑問符を表す、 [null 合体演算子](/dotnet/csharp/language-reference/operators/null-coalescing-operator)します。</span><span class="sxs-lookup"><span data-stu-id="12386-215">The two question marks represent the [null-coalescing operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span></span> <span data-ttu-id="12386-216">null 合体演算子は null 許容型の既定値を定義します。式 `(page ?? 1)` は、値がある場合は `page` の値を返し、`page` が null の場合は 1 を返すことを意味します。</span><span class="sxs-lookup"><span data-stu-id="12386-216">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="12386-217">Student インデックス ビューにページングのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-217">Add paging links to the Student index view</span></span>

1. <span data-ttu-id="12386-218">*Views\Student\Index.cshtml*、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="12386-218">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="12386-219">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="12386-219">The changes are highlighted.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   <span data-ttu-id="12386-220">ページの上部にある `@model` ステートメントは、ビューが `List` オブジェクトの代わりに `PagedList` オブジェクトを取得するようになったことを指定します。</span><span class="sxs-lookup"><span data-stu-id="12386-220">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

   <span data-ttu-id="12386-221">`using`ステートメント`PagedList.Mvc`ページング ボタンの MVC ヘルパーにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="12386-221">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

   <span data-ttu-id="12386-222">コードのオーバー ロードを使用して[BeginForm](/previous-versions/aspnet/dd492719(v=vs.108))することを指定する、 [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100))します。</span><span class="sxs-lookup"><span data-stu-id="12386-222">The code uses an overload of [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) that allows it to specify [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   <span data-ttu-id="12386-223">既定の[BeginForm](/previous-versions/aspnet/dd492719(v=vs.108))フォーム データをパラメーターとして渡されると、URL ではなく HTTP メッセージの本文にクエリ文字列は、投稿を送信します。</span><span class="sxs-lookup"><span data-stu-id="12386-223">The default [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="12386-224">HTTP GET を指定すると、フォーム データがクエリ文字列として URL で渡され、ユーザーが URL をブックマークできるようになります。</span><span class="sxs-lookup"><span data-stu-id="12386-224">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="12386-225">[HTTP GET を使用するための W3C のガイドライン](http://www.w3.org/2001/tag/doc/whenToUseGet.html)アクションは結果として更新しない場合は、GET を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="12386-225">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

   <span data-ttu-id="12386-226">テキスト ボックスは、新しいページをクリックすると、現在の検索文字列を表示できるように、現在の検索文字列で初期化されます。</span><span class="sxs-lookup"><span data-stu-id="12386-226">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   <span data-ttu-id="12386-227">列ヘッダーへのリンクは、フィルターの結果内でユーザーが並べ替えられるように、クエリ文字列を使用してコントローラーに現在の検索文字列を渡します。</span><span class="sxs-lookup"><span data-stu-id="12386-227">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   <span data-ttu-id="12386-228">ページの現在のページとの合計数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="12386-228">The current page and total number of pages are displayed.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   <span data-ttu-id="12386-229">表示するページがない場合は、「の 0 ページ 0」が表示されます。</span><span class="sxs-lookup"><span data-stu-id="12386-229">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="12386-230">(その場合、ページ番号は、ページ数より大きいため、 `Model.PageNumber` 1 に設定されてと`Model.PageCount`は 0 です)。</span><span class="sxs-lookup"><span data-stu-id="12386-230">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

   <span data-ttu-id="12386-231">によってページング ボタンが表示されます、`PagedListPager`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="12386-231">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   <span data-ttu-id="12386-232">`PagedListPager`ヘルパーはさまざまな Url を含むおよびスタイル設定、カスタマイズ可能なオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="12386-232">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="12386-233">詳細については、次を参照してください。 [TroyGoode/PagedList](https://github.com/TroyGoode/PagedList) 、GitHub サイトでします。</span><span class="sxs-lookup"><span data-stu-id="12386-233">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

2. <span data-ttu-id="12386-234">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="12386-234">Run the page.</span></span>

   ![ページングを含む students インデックス ページ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

   <span data-ttu-id="12386-236">異なる並べ替え順でページングのリンクをクリックし、ページングが機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="12386-236">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="12386-237">その後で、検索文字列を入力して、ページングをもう一度試し、並べ替えとフィルター処理を使用してもページングが正しく機能することを確認します。</span><span class="sxs-lookup"><span data-stu-id="12386-237">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

   ![フィルター テキストの検索含む students インデックス ページ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="12386-239">受講者の統計情報を表示する [About] ページを作成する</span><span class="sxs-lookup"><span data-stu-id="12386-239">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="12386-240">Contoso University web サイトのページについて、登録日付ごとに登録した受講者の数を表示します。</span><span class="sxs-lookup"><span data-stu-id="12386-240">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="12386-241">これには、グループ化とグループに関する簡単な計算が必要です。</span><span class="sxs-lookup"><span data-stu-id="12386-241">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="12386-242">これを実行するためには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="12386-242">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="12386-243">ビューに渡す必要があるデータのビュー モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="12386-243">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="12386-244">変更、`About`メソッドで、`Home`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="12386-244">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="12386-245">変更、`About`ビュー。</span><span class="sxs-lookup"><span data-stu-id="12386-245">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="12386-246">ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="12386-246">Create the View Model</span></span>

<span data-ttu-id="12386-247">作成、 *ViewModels*プロジェクト フォルダー内のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="12386-247">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="12386-248">クラス ファイルを追加、そのフォルダー内*EnrollmentDateGroup.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="12386-248">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="12386-249">Home コントローラーを変更する</span><span class="sxs-lookup"><span data-stu-id="12386-249">Modify the Home Controller</span></span>

1. <span data-ttu-id="12386-250">*HomeController.cs*、次の追加`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="12386-250">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. <span data-ttu-id="12386-251">クラスの中かっこの直後に、データベース コンテキストのクラスの変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="12386-251">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. <span data-ttu-id="12386-252">`About` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="12386-252">Replace the `About` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   <span data-ttu-id="12386-253">LINQ ステートメントは、登録日で受講者エンティティをグループ化し、各グループ内のエンティティの数を計算して、結果を `EnrollmentDateGroup` ビュー モデル オブジェクトのコレクションに格納します。</span><span class="sxs-lookup"><span data-stu-id="12386-253">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

4. <span data-ttu-id="12386-254">追加、`Dispose`メソッド。</span><span class="sxs-lookup"><span data-stu-id="12386-254">Add a `Dispose` method:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="12386-255">About ビューを変更する</span><span class="sxs-lookup"><span data-stu-id="12386-255">Modify the About View</span></span>

1. <span data-ttu-id="12386-256">コードに置き換えます、 *Views\Home\About.cshtml*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="12386-256">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. <span data-ttu-id="12386-257">アプリを実行し、をクリックして、**について**リンク。</span><span class="sxs-lookup"><span data-stu-id="12386-257">Run the app and click the **About** link.</span></span>

   <span data-ttu-id="12386-258">テーブルに、登録日付ごとの生徒の数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="12386-258">The count of students for each enrollment date displays in a table.</span></span>

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="12386-260">まとめ</span><span class="sxs-lookup"><span data-stu-id="12386-260">Summary</span></span>

<span data-ttu-id="12386-261">このチュートリアルでは、データ モデルを作成して、基本的な CRUD、並べ替え、フィルター、ページング、および機能をグループ化を実装する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="12386-261">In this tutorial you've seen how to create a data model and implement basic CRUD, sorting, filtering, paging, and grouping functionality.</span></span> <span data-ttu-id="12386-262">次のチュートリアルでは、データ モデルを展開して、詳細なトピックについて確認します。</span><span class="sxs-lookup"><span data-stu-id="12386-262">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span>

<span data-ttu-id="12386-263">このチュートリアルの立った方法で改善できましたフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="12386-263">Please leave feedback on how you liked this tutorial and what we could improve.</span></span>

<span data-ttu-id="12386-264">その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="12386-264">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="12386-265">[前へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [次へ](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="12386-265">[Previous](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Next](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
