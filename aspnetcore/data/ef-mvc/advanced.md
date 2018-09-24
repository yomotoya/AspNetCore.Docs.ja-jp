---
title: ASP.NET Core MVC と EF Core - 上級 - 10/10
author: rick-anderson
description: このチュートリアルでは、Entity Framework Core を使用するより高度な ASP.NET Core Web アプリを開発する際に、活用できるいくつかのトピックを紹介します。
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/advanced
ms.openlocfilehash: 5cdba79c0b8edd9b865bda8328c86356cbe6a0a2
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010924"
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a><span data-ttu-id="a1147-103">ASP.NET Core MVC と EF Core - 上級 - 10/10</span><span class="sxs-lookup"><span data-stu-id="a1147-103">ASP.NET Core MVC with EF Core - Advanced - 10 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a1147-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a1147-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a1147-105">Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a1147-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="a1147-106">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a1147-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="a1147-107">前のチュートリアルでは、Table-Per-Hierarchy 継承を実装しました。</span><span class="sxs-lookup"><span data-stu-id="a1147-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="a1147-108">このチュートリアルでは、Entity Framework Core を使用するより高度な ASP.NET Core Web アプリケーションを開発する際に、注意すべきいくつかのトピックを紹介します。</span><span class="sxs-lookup"><span data-stu-id="a1147-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="a1147-109">生 SQL クエリ</span><span class="sxs-lookup"><span data-stu-id="a1147-109">Raw SQL Queries</span></span>

<span data-ttu-id="a1147-110">Entity Framework を使用する利点の 1 つは、データを格納する特定のメソッドにコードを過度に接近させなくてもよい点です。</span><span class="sxs-lookup"><span data-stu-id="a1147-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="a1147-111">SQL クエリとコマンドが生成されるため、自分でこれらを記述する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="a1147-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="a1147-112">ただし、例外的なシナリオがあります。手動で作成した特定の SQL クエリを実行する必要がある場合です。</span><span class="sxs-lookup"><span data-stu-id="a1147-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="a1147-113">このようなシナリオでは、Entity Framework Code First API には、SQL コマンドをデータベースに直接渡せるメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a1147-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="a1147-114">EF Core 1.0 には次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="a1147-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="a1147-115">エンティティ型を返すクエリに対して `DbSet.FromSql` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1147-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="a1147-116">返されたオブジェクトは、`DbSet` オブジェクトで想定されている型である必要があります。また、[追跡をオフにしている場合を除き](crud.md#no-tracking-queries)、データベース コンテキストによって自動的に追跡されます。</span><span class="sxs-lookup"><span data-stu-id="a1147-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="a1147-117">非クエリ コマンドに対して `Database.ExecuteSqlCommand` を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1147-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="a1147-118">エンティティではない型を返すクエリを実行する必要がある場合は、ADO.NET と EF で提供されるデータベース接続を使用できます。</span><span class="sxs-lookup"><span data-stu-id="a1147-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="a1147-119">このメソッドを使用してエンティティ型を取得する場合でも、返されるデータはデータベース コンテキストによって追跡されません。</span><span class="sxs-lookup"><span data-stu-id="a1147-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="a1147-120">Web アプリケーションで SQL コマンドを実行する場合は常に、SQL インジェクション攻撃から自身のサイトを保護する対策を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="a1147-121">これを行う 1 つの方法として、パラメーター化されたクエリを使用して、Web ページによって送信された文字列が SQL コマンドとして解釈できないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1147-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="a1147-122">このチュートリアルでは、ユーザー入力をクエリに統合するときに、パラメーター化されたクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1147-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="a1147-123">エンティティを返すクエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="a1147-123">Call a query that returns entities</span></span>

<span data-ttu-id="a1147-124">`DbSet<TEntity>` クラスは、`TEntity` 型のエンティティを返すクエリの実行に使用できるメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="a1147-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="a1147-125">このしくみを確認するため、Department (部門) コントローラーの `Details` メソッド内のコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="a1147-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="a1147-126">*DepartmentsController.cs* の `Details` メソッドで、次の強調表示されたコードに示されているように、部門を取得するコードを `FromSql` メソッドの呼び出しに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a1147-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="a1147-127">新しいコードが正しく動作することを確認するには、**[Departments]\(部門\)** タブを選択し、いずれかの部門の **[Details]\(詳細\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1147-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![部門の詳細](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="a1147-129">その他の型を返すクエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="a1147-129">Call a query that returns other types</span></span>

<span data-ttu-id="a1147-130">以前に、登録日ごとの学生数を示す About ページ用に、学生の統計グリッドを作成しました。</span><span class="sxs-lookup"><span data-stu-id="a1147-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="a1147-131">Students エンティティ セット (`_context.Students`) からデータを取得し、LINQ を使用して結果を `EnrollmentDateGroup` ビュー モデル オブジェクトに投影しました。</span><span class="sxs-lookup"><span data-stu-id="a1147-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="a1147-132">LINQ を使用するのではなく、SQL そのものを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="a1147-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="a1147-133">これを行うには、エンティティ オブジェクト以外のものを返す SQL クエリを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="a1147-134">EF Core 1.0 では、これを行う方法の 1 つとして、ADO.NET コードを記述し、EF からデータベース接続を取得します。</span><span class="sxs-lookup"><span data-stu-id="a1147-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="a1147-135">*HomeController.cs* で、`About` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a1147-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="a1147-136">using ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="a1147-136">Add a using statement:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="a1147-137">アプリを実行して [About] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="a1147-137">Run the app and go to the About page.</span></span> <span data-ttu-id="a1147-138">以前に行ったのと同じデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1147-138">It displays the same data it did before.</span></span>

![About ページ](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="a1147-140">更新クエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="a1147-140">Call an update query</span></span>

<span data-ttu-id="a1147-141">Contoso University の管理者が、すべてのコースの単位数を変更するなどの、データベースでグローバルな変更を実行するとします。</span><span class="sxs-lookup"><span data-stu-id="a1147-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="a1147-142">大学に多くのコースがある場合は、それらすべてをエンティティとして取得し、それらを個別に変更するのは非効率的です。</span><span class="sxs-lookup"><span data-stu-id="a1147-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="a1147-143">このセクションでは、すべてのコースの単位数を変更する係数をユーザーが指定できるようにする Web ページを実装し、SQL UPDATE ステートメントを実行することによって変更を行います。</span><span class="sxs-lookup"><span data-stu-id="a1147-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="a1147-144">Web ページは次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="a1147-144">The web page will look like the following illustration:</span></span>

![Course Credits ページを更新する](advanced/_static/update-credits.png)

<span data-ttu-id="a1147-146">*CoursesContoller.cs* で、HttpGet および HttpPost に UpdateCourseCredits メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="a1147-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="a1147-147">コントローラーが HttpGet 要求を処理するときに、`ViewData["RowsAffected"]` では何も返されず、前の図に示されているように、ビューに空のテキスト ボックスと、[送信] ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1147-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="a1147-148">**[更新]** ボタンをクリックすると、HttpPost メソッドが呼び出され、乗数がテキスト ボックスに入力した値になります。</span><span class="sxs-lookup"><span data-stu-id="a1147-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="a1147-149">このコードは次に、コースを更新し、影響を受けた行の数を`ViewData` のビューに返す SQL を実行します。</span><span class="sxs-lookup"><span data-stu-id="a1147-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="a1147-150">ビューが `RowsAffected` 値を取得すると、更新された行の数を表示します。</span><span class="sxs-lookup"><span data-stu-id="a1147-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="a1147-151">**ソリューション エクスプローラー**で、*Views/Courses* フォルダーを右クリックし、**[追加] > [新しい項目]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1147-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="a1147-152">**[新しい項目の追加]** ダイアログで、左側のウィンドウの **[インストール済み]** の下の **[ASP.NET]** をクリックして、**[MVC ビュー ページ]** をクリックして、新しいビューに *UpdateCourseCredits.cshtml* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="a1147-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="a1147-153">*Views/Courses/UpdateCourseCredits.cshtml* で、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a1147-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="a1147-154">**[Courses]\(コース\)** タブを選択してから、ブラウザーのアドレス バーで URL の末尾に "/UpdateCourseCredits" を追加して (例: `http://localhost:5813/Courses/UpdateCourseCredits`)、`UpdateCourseCredits` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a1147-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="a1147-155">テキスト ボックスに数値を入力します。</span><span class="sxs-lookup"><span data-stu-id="a1147-155">Enter a number in the text box:</span></span>

![Course Credits ページを更新する](advanced/_static/update-credits.png)

<span data-ttu-id="a1147-157">**[更新]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1147-157">Click **Update**.</span></span> <span data-ttu-id="a1147-158">影響を受けた行の数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1147-158">You see the number of rows affected:</span></span>

![Course Credits ページの更新で影響を受けた行](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="a1147-160">**[リストに戻る]** をクリックして、単位数が変更されたコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="a1147-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="a1147-161">実稼働コードでは、更新の結果が常に有効なデータになることが保証される点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="a1147-162">ここに示した簡略化されたコードは、5 より大きい数値になるように単位数を乗算できます </span><span class="sxs-lookup"><span data-stu-id="a1147-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="a1147-163">(`Credits` プロパティには `[Range(0, 5)]` 属性があります)。更新クエリは機能しますが、無効なデータによって、5 以下の単位数を想定していたシステムの他の部分で予期しない結果が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="a1147-164">SQL クエリの詳細については、「[Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql)」 (生 SQL クエリ) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="a1147-165">データベースに送信される SQL を調べる</span><span class="sxs-lookup"><span data-stu-id="a1147-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="a1147-166">データベースに送信される実際の SQL クエリを確認できると役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="a1147-167">ASP.NET Core の組み込みのログ記録機能は、クエリと更新の SQL を含むログを書き込むため、EF Core によって自動的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a1147-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="a1147-168">このセクションでは、SQL ログの例をいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="a1147-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="a1147-169">*StudentsController.cs* を開き、`Details` メソッドで `if (student == null)` ステートメントにブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="a1147-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="a1147-170">デバッグ モードでアプリを実行して、学生の [Details] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="a1147-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="a1147-171">デバッグの出力を示す **[出力]** ウィンドウに移動して、クエリを確認します。</span><span class="sxs-lookup"><span data-stu-id="a1147-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="a1147-172">驚くかもしれませんが、SQL は Person テーブルから最大 2 つの行 (`TOP(2)`) を選択します。</span><span class="sxs-lookup"><span data-stu-id="a1147-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="a1147-173">`SingleOrDefaultAsync` メソッドは、サーバー上の 1 行に解決されません。</span><span class="sxs-lookup"><span data-stu-id="a1147-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="a1147-174">その理由を説明します。</span><span class="sxs-lookup"><span data-stu-id="a1147-174">Here's why:</span></span>

* <span data-ttu-id="a1147-175">クエリが複数の行を返すと、メソッドは null を返します。</span><span class="sxs-lookup"><span data-stu-id="a1147-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="a1147-176">クエリが複数の行を返すかどうかを判断するため、EF はクエリが少なくとも 2 を返すかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="a1147-177">**[出力]** ウィンドウでログ出力を取得するには、デバッグ モードを使用してブレークポイントで停止する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a1147-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="a1147-178">これは単に、出力を見たいポイントでログを停止する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="a1147-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="a1147-179">これを行わないと、ログ記録は続行され、関心がある部分までスクロールで戻る必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="a1147-180">Repository パターンと Unit of Work パターン</span><span class="sxs-lookup"><span data-stu-id="a1147-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="a1147-181">多くの開発者は、Entity Framework で動作するコードをラップするラッパーとして、Repository パターンと Unit of Work パターンを実装するためのコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="a1147-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="a1147-182">これらのパターンは、アプリケーションのデータ アクセス層とビジネス ロジック層の間に抽象化レイヤーを作成するためのものです。</span><span class="sxs-lookup"><span data-stu-id="a1147-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="a1147-183">これらのパターンを実装すると、データ ストアの変更からアプリケーションを隔離でき、自動化された単体テストやテスト駆動開発 (TDD) を円滑化できます。</span><span class="sxs-lookup"><span data-stu-id="a1147-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="a1147-184">ただし、次の複数の理由により、追加のコードを記述してこれらのパターンを実装することが、EF を使用するアプリケーションにとって最善の選択肢ではない場合もあります。</span><span class="sxs-lookup"><span data-stu-id="a1147-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="a1147-185">EF コンテキスト クラス自体が、コードをデータ ストア固有のコードから隔離します。</span><span class="sxs-lookup"><span data-stu-id="a1147-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="a1147-186">EF コンテキスト クラスは、EF を使用して行っているデータベースの更新の unit-of-work クラスとして動作できます。</span><span class="sxs-lookup"><span data-stu-id="a1147-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="a1147-187">リポジトリ コードを記述しなくても、EF には TDD を実装するための機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a1147-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="a1147-188">Repository パターンと Unit of Work パターンを実装する方法については、[このチュートリアル シリーズの Entity Framework 5 バージョン](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="a1147-189">Entity Framework Core は、テストに使用できる In-Memory データベース プロバイダーを実装します。</span><span class="sxs-lookup"><span data-stu-id="a1147-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="a1147-190">詳細については、[InMemory を使ったテスト](/ef/core/miscellaneous/testing/in-memory)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-190">For more information, see [Test with InMemory](/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="a1147-191">変更の自動検出</span><span class="sxs-lookup"><span data-stu-id="a1147-191">Automatic change detection</span></span>

<span data-ttu-id="a1147-192">Entity Framework では、エンティティの現在の値と元の値を比較して、エンティティがどのように変更されたか (およびそれによって、どの更新プログラムをデータベースに送信する必要があるか) を判断します。</span><span class="sxs-lookup"><span data-stu-id="a1147-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="a1147-193">元の値は、エンティティが照会されるかアタッチされるときに格納されます。</span><span class="sxs-lookup"><span data-stu-id="a1147-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="a1147-194">変更の自動検出を行うメソッドには、次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="a1147-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="a1147-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="a1147-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="a1147-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="a1147-196">DbContext.Entry</span></span>

* <span data-ttu-id="a1147-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="a1147-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="a1147-198">多数のエンティティを追跡していて、これらのいずれかのメソッドをループ内で何度も呼び出す場合、`ChangeTracker.AutoDetectChangesEnabled` プロパティを使用して変更の自動検出を一時的にオフにすると、パフォーマンスが大幅に向上する場合があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="a1147-199">例:</span><span class="sxs-lookup"><span data-stu-id="a1147-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="a1147-200">Entity Framework Core のソース コードと開発計画</span><span class="sxs-lookup"><span data-stu-id="a1147-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="a1147-201">Entity Framework Core のソースは、[https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore) にあります。</span><span class="sxs-lookup"><span data-stu-id="a1147-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="a1147-202">EF Core リポジトリには、夜間ビルド、問題追跡、機能仕様、設計ミーティング メモ、および[将来の開発のためのロードマップ](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a1147-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="a1147-203">バグを見つけて報告したり、投稿することができます。</span><span class="sxs-lookup"><span data-stu-id="a1147-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="a1147-204">ソース コードはオープンですが、Entity Framework Core はマイクロソフト製品として完全にサポートされています。</span><span class="sxs-lookup"><span data-stu-id="a1147-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="a1147-205">Microsoft Entity Framework チームは、各リリースの品質を保証するため、受け入れる投稿を管理し、すべてのコード変更をテストしています。</span><span class="sxs-lookup"><span data-stu-id="a1147-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="a1147-206">既存のデータベースからのリバース エンジニアリング</span><span class="sxs-lookup"><span data-stu-id="a1147-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="a1147-207">既存のデータベースからエンティティ クラスを含むデータ モデルをリバース エンジニアリングするには、[scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1147-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="a1147-208">[入門用チュートリアル](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="a1147-209">動的な LINQ を使用して並べ替え選択コードを簡略化する</span><span class="sxs-lookup"><span data-stu-id="a1147-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="a1147-210">[このシリーズの 3 番目のチュートリアル](sort-filter-page.md)では、`switch`ステートメントで列名をハード コーディングすることで、LINQ コードを記述する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="a1147-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="a1147-211">選択する列が 2 つの場合は正常に機能しますが、多数の列がある場合は、コードが冗長になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="a1147-212">この問題を解決するため、`EF.Property` メソッドを使用して、プロパティの名前を文字列として指定できます。</span><span class="sxs-lookup"><span data-stu-id="a1147-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="a1147-213">この方法を試すには、`StudentsController` の `Index` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a1147-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="a1147-214">次の手順</span><span class="sxs-lookup"><span data-stu-id="a1147-214">Next steps</span></span>

<span data-ttu-id="a1147-215">これで、ASP.NET Core MVC アプリケーションでの Entity Framework Core の使用に関するチュートリアル シリーズは終了です。</span><span class="sxs-lookup"><span data-stu-id="a1147-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET Core MVC application.</span></span>

<span data-ttu-id="a1147-216">EF Core の詳細については、「[Entity Framework Core 概要](https://docs.microsoft.com/ef/core)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="a1147-217">書籍「[Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action)」も利用できます。</span><span class="sxs-lookup"><span data-stu-id="a1147-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="a1147-218">Web アプリの展開方法については、「[ASP.NET Core のホストと展開](xref:host-and-deploy/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="a1147-219">認証および承認などの、ASP.NET Core MVC に関連するその他のトピックについては、「[ASP.NET Core](xref:index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](xref:index).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="a1147-220">謝辞</span><span class="sxs-lookup"><span data-stu-id="a1147-220">Acknowledgments</span></span>

<span data-ttu-id="a1147-221">チュートリアルを執筆してくださった、Tom Dykstra と Rick Anderson (twitter @RickAndMSFT)。</span><span class="sxs-lookup"><span data-stu-id="a1147-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="a1147-222">コードの確認をサポートし、チュートリアル用のコードの記述中に発生した問題のデバッグを支援してくれた、Rowan Miller、Diego Vega、およびその他の Entity Framework チームのメンバー。</span><span class="sxs-lookup"><span data-stu-id="a1147-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="a1147-223">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="a1147-223">Common errors</span></span>

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="a1147-224">ContosoUniversity.dll が別のプロセスによって使用されている</span><span class="sxs-lookup"><span data-stu-id="a1147-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="a1147-225">エラー メッセージ</span><span class="sxs-lookup"><span data-stu-id="a1147-225">Error message:</span></span>

> <span data-ttu-id="a1147-226">'...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' を書き込み用に開けません -- '別のプロセスで使用されているため、プロセスはファイル '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' にアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a1147-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="a1147-227">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="a1147-227">Solution:</span></span>

<span data-ttu-id="a1147-228">IIS express でサイトを停止します。</span><span class="sxs-lookup"><span data-stu-id="a1147-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="a1147-229">Windows システム トレイに戻り、IIS Express を見つけてそのアイコンを右クリックし、Contoso University サイトを選択し、**[サイトの停止]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1147-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="a1147-230">Up メソッドと Down メソッドでコードを使用せずに移行がスキャフォールディングされる</span><span class="sxs-lookup"><span data-stu-id="a1147-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="a1147-231">考えられる原因:</span><span class="sxs-lookup"><span data-stu-id="a1147-231">Possible cause:</span></span>

<span data-ttu-id="a1147-232">EF CLI コマンドは、コード ファイルを自動的に閉じて保存しません。</span><span class="sxs-lookup"><span data-stu-id="a1147-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="a1147-233">`migrations add` コマンドを実行するときに未保存の変更があると、EF は変更を検出しません。</span><span class="sxs-lookup"><span data-stu-id="a1147-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="a1147-234">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="a1147-234">Solution:</span></span>

<span data-ttu-id="a1147-235">`migrations remove` コマンドを実行して、コードの変更を保存し、`migrations add` コマンドを再実行します。</span><span class="sxs-lookup"><span data-stu-id="a1147-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="a1147-236">データベースの更新中のエラー</span><span class="sxs-lookup"><span data-stu-id="a1147-236">Errors while running database update</span></span>

<span data-ttu-id="a1147-237">データが存在するデータベースでスキーマの変更を行っているときに、他のエラーが発生する場合があります。</span><span class="sxs-lookup"><span data-stu-id="a1147-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="a1147-238">解決できない移行エラーが発生した場合は、接続文字列のデータベース名を変更するか、データベースを削除できます。</span><span class="sxs-lookup"><span data-stu-id="a1147-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="a1147-239">新しいデータベースには移行するデータが存在しないため、update-database コマンドがエラーなしで完了する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="a1147-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="a1147-240">最も簡単な方法は、*appsettings.json* でデータベースの名前を変更することです。</span><span class="sxs-lookup"><span data-stu-id="a1147-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="a1147-241">次に `database update` を実行したときに、新しいデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a1147-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="a1147-242">SSOX でデータベースを削除するには、そのデータベースを右クリックして、**[削除]** をクリックしてから、**[データベースの削除]** ダイアログ ボックスで **[既存の接続を閉じる]**、**[OK]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="a1147-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="a1147-243">CLI を使用してデータベースを削除するには、`database drop` CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a1147-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="a1147-244">SQL Server インスタンスの位置を特定しているときのエラー</span><span class="sxs-lookup"><span data-stu-id="a1147-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="a1147-245">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="a1147-245">Error Message:</span></span>

> <span data-ttu-id="a1147-246">SQL Server への接続を確立しているときに、ネットワーク関連またはインスタンス固有のエラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="a1147-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="a1147-247">サーバーが見つからないかアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="a1147-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="a1147-248">インスタンス名が正しいこと、および SQL Server がリモート接続を許可するように構成されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="a1147-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="a1147-249">(プロバイダー: SQL ネットワーク インターフェイス、エラー: 26 - 指定されたサーバーまたはインスタンスの位置を特定しているときにエラーが発生しました)。</span><span class="sxs-lookup"><span data-stu-id="a1147-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="a1147-250">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="a1147-250">Solution:</span></span>

<span data-ttu-id="a1147-251">接続文字列を確認します。</span><span class="sxs-lookup"><span data-stu-id="a1147-251">Check the connection string.</span></span> <span data-ttu-id="a1147-252">データベース ファイルを手動で削除した場合は、構築文字列でデータベースの名前を変更して、新しいデータベースで最初からやり直します。</span><span class="sxs-lookup"><span data-stu-id="a1147-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="a1147-253">前へ</span><span class="sxs-lookup"><span data-stu-id="a1147-253">Previous</span></span>](inheritance.md)
