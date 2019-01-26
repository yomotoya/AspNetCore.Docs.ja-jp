---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 'チュートリアル: MVC 5 Web アプリの高度な EF のシナリオについて説明します'
description: このチュートリアルでは、Entity Framework Code First を使用する ASP.NET web アプリケーションの開発の基本機能を越えて移動するときの注意すべきに役立ついくつかのトピックについて紹介します。
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d0208c8890467ec6044d807aeee7c7ae02e18790
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54890004"
---
# <a name="tutorial-learn-about-advanced-ef-scenarios-for-an-mvc-5-web-app"></a><span data-ttu-id="746e9-103">チュートリアル: MVC 5 Web アプリの高度な EF のシナリオについて説明します</span><span class="sxs-lookup"><span data-stu-id="746e9-103">Tutorial: Learn about advanced EF Scenarios for an MVC 5 Web app</span></span>

<span data-ttu-id="746e9-104">前のチュートリアルでは、table-per-hierarchy 継承を実装しました。</span><span class="sxs-lookup"><span data-stu-id="746e9-104">In the previous tutorial you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="746e9-105">このチュートリアルでは、Entity Framework Code First を使用する ASP.NET web アプリケーションの開発の基本機能を越えて移動するときの注意すべきに役立ついくつかのトピックについて紹介します。</span><span class="sxs-lookup"><span data-stu-id="746e9-105">This tutorial includes introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span> <span data-ttu-id="746e9-106">最初のいくつかのセクションは、コードについて説明した手順を持ち、詳細については、リソースへのリンクの後に簡単な紹介をいくつかのトピックを紹介する Visual Studio を使用して、次のセクションのタスクを完了します。</span><span class="sxs-lookup"><span data-stu-id="746e9-106">The first few sections have step-by-step instructions that walk you through the code and using Visual Studio to complete tasks The sections that follow introduce several topics with brief introductions followed by links to resources for more information.</span></span>

<span data-ttu-id="746e9-107">これらのトピックのほとんどは、既に作成したページを操作します。</span><span class="sxs-lookup"><span data-stu-id="746e9-107">For most of these topics, you'll work with pages that you already created.</span></span> <span data-ttu-id="746e9-108">生 SQL を使用して一括更新プログラムを実行するには、データベース内のすべてのコースの単位数を更新する新しいページを作成します。</span><span class="sxs-lookup"><span data-stu-id="746e9-108">To use raw SQL to do bulk updates you'll create a new page that updates the number of credits of all courses in the database:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<span data-ttu-id="746e9-110">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="746e9-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="746e9-111">生 SQL クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="746e9-111">Perform raw SQL queries</span></span>
> * <span data-ttu-id="746e9-112">追跡なしのクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="746e9-112">Perform no-tracking queries</span></span>
> * <span data-ttu-id="746e9-113">SQL を調べるクエリ データベースに送信されます</span><span class="sxs-lookup"><span data-stu-id="746e9-113">Examine SQL queries sent to database</span></span>

<span data-ttu-id="746e9-114">についても説明します。</span><span class="sxs-lookup"><span data-stu-id="746e9-114">You also learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="746e9-115">抽象化レイヤーを作成します。</span><span class="sxs-lookup"><span data-stu-id="746e9-115">Creating an abstraction layer</span></span>
> * <span data-ttu-id="746e9-116">プロキシ クラス</span><span class="sxs-lookup"><span data-stu-id="746e9-116">Proxy classes</span></span>
> * <span data-ttu-id="746e9-117">変更の自動検出</span><span class="sxs-lookup"><span data-stu-id="746e9-117">Automatic change detection</span></span>
> * <span data-ttu-id="746e9-118">自動検証</span><span class="sxs-lookup"><span data-stu-id="746e9-118">Automatic validation</span></span>
> * <span data-ttu-id="746e9-119">Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="746e9-119">Entity Framework Power Tools</span></span>
> * <span data-ttu-id="746e9-120">Entity Framework のソース コード</span><span class="sxs-lookup"><span data-stu-id="746e9-120">Entity Framework source code</span></span>

## <a name="prerequisite"></a><span data-ttu-id="746e9-121">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="746e9-121">Prerequisite</span></span>

* [<span data-ttu-id="746e9-122">継承の実装</span><span class="sxs-lookup"><span data-stu-id="746e9-122">Implementing Inheritance</span></span>](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="perform-raw-sql-queries"></a><span data-ttu-id="746e9-123">生 SQL クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="746e9-123">Perform raw SQL queries</span></span>

<span data-ttu-id="746e9-124">Entity Framework Code First API をデータベースに直接 SQL コマンドを渡すことができるようにするメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="746e9-124">The Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="746e9-125">次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="746e9-125">You have the following options:</span></span>

- <span data-ttu-id="746e9-126">使用して、 [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx)エンティティ型を返すクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="746e9-126">Use the [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) method for queries that return entity types.</span></span> <span data-ttu-id="746e9-127">によって予期される型で返されたオブジェクトが必要があります、`DbSet`オブジェクト、およびそれらが自動的に追跡データベース コンテキストによって追跡をオフにする場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="746e9-127">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you turn tracking off.</span></span> <span data-ttu-id="746e9-128">(に関する次のセクションを参照してください、 [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx)メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="746e9-128">(See the following section about the [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) method.)</span></span>
- <span data-ttu-id="746e9-129">使用して、 [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx)エンティティではない型を返すクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="746e9-129">Use the [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) method for queries that return types that aren't entities.</span></span> <span data-ttu-id="746e9-130">このメソッドを使用してエンティティ型を取得する場合でも、返されるデータはデータベース コンテキストによって追跡されません。</span><span class="sxs-lookup"><span data-stu-id="746e9-130">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>
- <span data-ttu-id="746e9-131">使用して、 [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx)非クエリ コマンド。</span><span class="sxs-lookup"><span data-stu-id="746e9-131">Use the [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) for non-query commands.</span></span>

<span data-ttu-id="746e9-132">Entity Framework を使用する利点の 1 つは、データを格納する特定のメソッドにコードを過度に接近させなくてもよい点です。</span><span class="sxs-lookup"><span data-stu-id="746e9-132">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="746e9-133">SQL クエリとコマンドが生成されるため、自分でこれらを記述する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="746e9-133">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="746e9-134">手動で作成した特定の SQL クエリを実行する必要がある場合に例外的なシナリオがあるし、これらのメソッドにより、このような例外を処理することが可能です。</span><span class="sxs-lookup"><span data-stu-id="746e9-134">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created, and these methods make it possible for you to handle such exceptions.</span></span>

<span data-ttu-id="746e9-135">Web アプリケーションで SQL コマンドを実行する場合は常に、SQL インジェクション攻撃から自身のサイトを保護する対策を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="746e9-135">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="746e9-136">これを行う 1 つの方法として、パラメーター化されたクエリを使用して、Web ページによって送信された文字列が SQL コマンドとして解釈できないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="746e9-136">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="746e9-137">このチュートリアルでは、ユーザー入力をクエリに統合するときに、パラメーター化されたクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="746e9-137">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

### <a name="calling-a-query-that-returns-entities"></a><span data-ttu-id="746e9-138">クエリを呼び出すには、エンティティが返されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-138">Calling a Query that Returns Entities</span></span>

<span data-ttu-id="746e9-139">[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx)クラス型のエンティティを返すクエリの実行に使用できるメソッドを提供`TEntity`します。</span><span class="sxs-lookup"><span data-stu-id="746e9-139">The [DbSet&lt;TEntity&gt;](https://msdn.microsoft.com/library/gg696460.aspx) class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="746e9-140">しくみを確認するコードを変更します、`Details`のメソッド、`Department`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="746e9-140">To see how this works you'll change the code in the `Details` method of the `Department` controller.</span></span>

<span data-ttu-id="746e9-141">*DepartmentController.cs*の`Details`メソッド、置換、`db.Departments.FindAsync`メソッドの呼び出しで、`db.Departments.SqlQuery`メソッドの呼び出し、次の強調表示されたコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="746e9-141">In *DepartmentController.cs*, in the `Details` method, replace the `db.Departments.FindAsync` method call with a `db.Departments.SqlQuery` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

<span data-ttu-id="746e9-142">新しいコードが正しく動作することを確認するには、**[Departments]\(部門\)** タブを選択し、いずれかの部門の **[Details]\(詳細\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="746e9-142">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span> <span data-ttu-id="746e9-143">期待どおりにすべてのデータ表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="746e9-143">Make sure all of the data displays as expected.</span></span>

### <a name="calling-a-query-that-returns-other-types-of-objects"></a><span data-ttu-id="746e9-144">クエリを呼び出すには、その他の種類のオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-144">Calling a Query that Returns Other Types of Objects</span></span>

<span data-ttu-id="746e9-145">以前に、登録日ごとの学生数を示す About ページ用に、学生の統計グリッドを作成しました。</span><span class="sxs-lookup"><span data-stu-id="746e9-145">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="746e9-146">コードでは、これを*HomeController.cs* LINQ を使用します。</span><span class="sxs-lookup"><span data-stu-id="746e9-146">The code that does this in *HomeController.cs* uses LINQ:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

<span data-ttu-id="746e9-147">LINQ を使用するのではなく、SQL で直接このデータを取得するコードを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="746e9-147">Suppose you want to write the code that retrieves this data directly in SQL rather than using LINQ.</span></span> <span data-ttu-id="746e9-148">エンティティ オブジェクト以外のものを返すクエリを実行する必要があることには、ことを意味する必要があるを使用して、 [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx)メソッド。</span><span class="sxs-lookup"><span data-stu-id="746e9-148">To do that you need to run a query that returns something other than entity objects, which means you need to use the [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) method.</span></span>

<span data-ttu-id="746e9-149">*HomeController.cs*での LINQ ステートメントを置き換えます、`About`メソッドを次の強調表示されたコードに示すように、SQL ステートメント。</span><span class="sxs-lookup"><span data-stu-id="746e9-149">In *HomeController.cs*, replace the LINQ statement in the `About` method with a SQL statement, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

<span data-ttu-id="746e9-150">[About] ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="746e9-150">Run the About page.</span></span> <span data-ttu-id="746e9-151">先ほどと同じデータを表示することを確認します。</span><span class="sxs-lookup"><span data-stu-id="746e9-151">Verify that it displays the same data it did before.</span></span>

### <a name="calling-an-update-query"></a><span data-ttu-id="746e9-152">更新クエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="746e9-152">Calling an Update Query</span></span>

<span data-ttu-id="746e9-153">Contoso University の管理者は、すべてのコースの単位数を変更するなど、データベースで一括変更を実行することができるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="746e9-153">Suppose Contoso University administrators want to be able to perform bulk changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="746e9-154">大学に多くのコースがある場合は、それらすべてをエンティティとして取得し、それらを個別に変更するのは非効率的です。</span><span class="sxs-lookup"><span data-stu-id="746e9-154">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="746e9-155">このセクションでは、ユーザーは、すべてのコースの単位数を変更する係数を指定できる web ページを実装し、SQL を実行することによって、変更を行います`UPDATE`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="746e9-155">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL `UPDATE` statement.</span></span> 

<span data-ttu-id="746e9-156">*CourseContoller.cs*、追加`UpdateCourseCredits`メソッド`HttpGet`と`HttpPost`:</span><span class="sxs-lookup"><span data-stu-id="746e9-156">In *CourseContoller.cs*, add `UpdateCourseCredits` methods for `HttpGet` and `HttpPost`:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

<span data-ttu-id="746e9-157">コント ローラーを処理すると、`HttpGet`要求、何も返されないで、`ViewBag.RowsAffected`変数、および、ビューは空のテキスト ボックスと送信ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-157">When the controller processes an `HttpGet` request, nothing is returned in the `ViewBag.RowsAffected` variable, and the view displays an empty text box and a submit button.</span></span>

<span data-ttu-id="746e9-158">ときに、 **Update**ボタンがクリックされた、`HttpPost`メソッドが呼び出されると、および`multiplier`テキスト ボックスに入力した値を持ちます。</span><span class="sxs-lookup"><span data-stu-id="746e9-158">When the **Update** button is clicked, the `HttpPost` method is called, and `multiplier` has the value entered in the text box.</span></span> <span data-ttu-id="746e9-159">コードのコースを更新し、ビューに影響を受けた行の数を返します SQL を実行し、`ViewBag.RowsAffected`変数。</span><span class="sxs-lookup"><span data-stu-id="746e9-159">The code then executes the SQL that updates courses and returns the number of affected rows to the view in the `ViewBag.RowsAffected` variable.</span></span> <span data-ttu-id="746e9-160">ビューは、その変数に値を取得、ときに、テキスト ボックスではなく更新された行の数を表示し、送信ボタン。</span><span class="sxs-lookup"><span data-stu-id="746e9-160">When the view gets a value in that variable, it displays the number of rows updated instead of the text box and submit button.</span></span>

<span data-ttu-id="746e9-161">*CourseController.cs*のいずれかを右クリックし、`UpdateCourseCredits`メソッド、およびクリック**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="746e9-161">In *CourseController.cs*, right-click one of the `UpdateCourseCredits` methods, and then click **Add View**.</span></span> <span data-ttu-id="746e9-162">**ビューの追加**ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-162">The **Add View** dialog appears.</span></span> <span data-ttu-id="746e9-163">既定値と選択のままに**追加**します。</span><span class="sxs-lookup"><span data-stu-id="746e9-163">Leave the defaults and select **Add**.</span></span>

<span data-ttu-id="746e9-164">*Views\Course\UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="746e9-164">In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

<span data-ttu-id="746e9-165">**[Courses]\(コース\)** タブを選択してから、ブラウザーのアドレス バーで URL の末尾に "/UpdateCourseCredits" を追加して (例: `http://localhost:50205/Course/UpdateCourseCredits`)、`UpdateCourseCredits` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="746e9-165">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:50205/Course/UpdateCourseCredits`).</span></span> <span data-ttu-id="746e9-166">テキスト ボックスに数値を入力します。</span><span class="sxs-lookup"><span data-stu-id="746e9-166">Enter a number in the text box:</span></span>

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<span data-ttu-id="746e9-168">**[更新]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="746e9-168">Click **Update**.</span></span> <span data-ttu-id="746e9-169">影響を受ける行の数を表示します。</span><span class="sxs-lookup"><span data-stu-id="746e9-169">You see the number of rows affected.</span></span>

<span data-ttu-id="746e9-170">**[リストに戻る]** をクリックして、単位数が変更されたコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="746e9-170">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="746e9-171">生 SQL クエリの詳細については、次を参照してください。[生 SQL クエリ](https://msdn.microsoft.com/data/jj592907)msdn です。</span><span class="sxs-lookup"><span data-stu-id="746e9-171">For more information about raw SQL queries, see [Raw SQL Queries](https://msdn.microsoft.com/data/jj592907) on MSDN.</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="746e9-172">追跡なしのクエリ</span><span class="sxs-lookup"><span data-stu-id="746e9-172">No-tracking queries</span></span>

<span data-ttu-id="746e9-173">データベース コンテキストは、テーブルの行を取得してそれらを表すエンティティ オブジェクトを作成するとき、既定では、メモリ内のエンティティがデータベース内のデータと同期されているかどうかを追跡します。</span><span class="sxs-lookup"><span data-stu-id="746e9-173">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="746e9-174">メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使われます。</span><span class="sxs-lookup"><span data-stu-id="746e9-174">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="746e9-175">Web アプリケーションでは、一般にコンテキスト インスタンスの存続期間は短く (要求ごとに新しいインスタンスが作成されて破棄されます)、通常、エンティティを読み取るコンテキストはエンティティが再び使われる前に破棄されるので、多くの場合、このようなキャッシュは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="746e9-175">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="746e9-176">使用してメモリ内のエンティティ オブジェクトの追跡を無効にすることができます、 [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)メソッド。</span><span class="sxs-lookup"><span data-stu-id="746e9-176">You can disable tracking of entity objects in memory by using the [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) method.</span></span> <span data-ttu-id="746e9-177">追跡を無効にした方がよい一般的なシナリオを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="746e9-177">Typical scenarios in which you might want to do that include the following:</span></span>

- <span data-ttu-id="746e9-178">クエリでは、このような膨大な量の追跡をオフにするとパフォーマンスを向上させる著しくをデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="746e9-178">A query retrieves such a large volume of data that turning off tracking might noticeably enhance performance.</span></span>
- <span data-ttu-id="746e9-179">それを更新するには、エンティティをアタッチしたいが、異なる目的で以前に同じエンティティを取得します。</span><span class="sxs-lookup"><span data-stu-id="746e9-179">You want to attach an entity in order to update it, but you earlier retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="746e9-180">エンティティはデータベース コンテキストによって既に追跡されているため、変更するエンティティをアタッチできません。</span><span class="sxs-lookup"><span data-stu-id="746e9-180">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="746e9-181">この状況に対処する 1 つの方法は使用する、`AsNoTracking`オプションは、以前のクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="746e9-181">One way to handle this situation is to use the `AsNoTracking` option with the earlier query.</span></span>

<span data-ttu-id="746e9-182">使用する方法を示す例については、 [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)メソッドを参照してください[このチュートリアルの以前のバージョン](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="746e9-182">For an example that demonstrates how to use the [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) method, see [the earlier version of this tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md).</span></span> <span data-ttu-id="746e9-183">チュートリアルのこのバージョンは、必要がないために、編集の方法でモデル バインダーに作成されたエンティティでの Modified フラグを設定しない`AsNoTracking`します。</span><span class="sxs-lookup"><span data-stu-id="746e9-183">This version of the tutorial doesn't set the Modified flag on a model-binder-created entity in the Edit method, so it doesn't need `AsNoTracking`.</span></span>

## <a name="examine-sql-sent-to-database"></a><span data-ttu-id="746e9-184">データベースに送信される SQL を確認します。</span><span class="sxs-lookup"><span data-stu-id="746e9-184">Examine SQL sent to database</span></span>

<span data-ttu-id="746e9-185">データベースに送信される実際の SQL クエリを確認できると役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="746e9-185">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="746e9-186">前のチュートリアルでは、インターセプターのコードで方法を説明しましたこれでインターセプターのコードを記述せずに行う方法をいくつかを確認します。</span><span class="sxs-lookup"><span data-stu-id="746e9-186">In an earlier tutorial you saw how to do that in interceptor code; now you'll see some ways to do it without writing interceptor code.</span></span> <span data-ttu-id="746e9-187">これを試すには、単純なクエリを見てをこのような一括読み込み、フィルター処理、および並べ替えオプションを追加するときにどう見ています。</span><span class="sxs-lookup"><span data-stu-id="746e9-187">To try this out, you'll look at a simple query and then look at what happens to it as you add options such eager loading, filtering, and sorting.</span></span>

<span data-ttu-id="746e9-188">*コント ローラー/CourseController*、置換、`Index`メソッドを一時的に一括読み込みを停止するには、次のコード。</span><span class="sxs-lookup"><span data-stu-id="746e9-188">In *Controllers/CourseController*, replace the `Index` method with the following code, in order to temporarily stop eager loading:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

<span data-ttu-id="746e9-189">今すぐにブレークポイントを設定、`return`ステートメント (その行にカーソルを f9 キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="746e9-189">Now set a breakpoint on the `return` statement (F9 with the cursor on that line).</span></span> <span data-ttu-id="746e9-190">キーを押して**F5**プロジェクトをデバッグ モードで実行して、コースのインデックス ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="746e9-190">Press **F5** to run the project in debug mode, and select the Course Index page.</span></span> <span data-ttu-id="746e9-191">コードでは、ブレークポイントに達すると、確認、`sql`変数。</span><span class="sxs-lookup"><span data-stu-id="746e9-191">When the code reaches the breakpoint, examine the `sql` variable.</span></span> <span data-ttu-id="746e9-192">SQL Server に送信されるクエリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-192">You see the query that's sent to SQL Server.</span></span> <span data-ttu-id="746e9-193">単純な`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="746e9-193">It's a simple `Select` statement.</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

<span data-ttu-id="746e9-194">クエリでは、虫眼鏡をクリックして、**テキスト ビジュアライザー**します。</span><span class="sxs-lookup"><span data-stu-id="746e9-194">Click the magnifying glass to see the query in the **Text Visualizer**.</span></span>

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

<span data-ttu-id="746e9-195">今すぐできるように、ユーザーは、特定の部署のフィルター処理できます Courses/index ページにドロップダウン リストを追加します。</span><span class="sxs-lookup"><span data-stu-id="746e9-195">Now you'll add a drop-down list to the Courses Index page so that users can filter for a particular department.</span></span> <span data-ttu-id="746e9-196">コース タイトルで並べ替えられるし、一括読み込みを指定します、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="746e9-196">You'll sort the courses by title, and you'll specify eager loading for the `Department` navigation property.</span></span>

<span data-ttu-id="746e9-197">*CourseController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="746e9-197">In *CourseController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

<span data-ttu-id="746e9-198">ブレークポイントの復元、`return`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="746e9-198">Restore the breakpoint on the `return` statement.</span></span>

<span data-ttu-id="746e9-199">メソッドのドロップダウン リストの選択した値を受け取る、`SelectedDepartment`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="746e9-199">The method receives the selected value of the drop-down list in the `SelectedDepartment` parameter.</span></span> <span data-ttu-id="746e9-200">何も選択されている場合、このパラメーターは null になります。</span><span class="sxs-lookup"><span data-stu-id="746e9-200">If nothing is selected, this parameter will be null.</span></span>

<span data-ttu-id="746e9-201">A`SelectList`ドロップダウン リストのすべての部門を含むコレクションが、ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-201">A `SelectList` collection containing all departments is passed to the view for the drop-down list.</span></span> <span data-ttu-id="746e9-202">渡されるパラメーター、`SelectList`コンス トラクターは、値フィールドの名前、テキスト フィールド名、および選択した項目を指定します。</span><span class="sxs-lookup"><span data-stu-id="746e9-202">The parameters passed to the `SelectList` constructor specify the value field name, the text field name, and the selected item.</span></span>

<span data-ttu-id="746e9-203">`Get`のメソッド、`Course`リポジトリ、コードは、フィルター式、並べ替え順序、およびの一括読み込みを指定、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="746e9-203">For the `Get` method of the `Course` repository, the code specifies a filter expression, a sort order, and eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="746e9-204">フィルター式は常に返します`true`かどうかは、ドロップダウン リストで何も選択が (つまり、`SelectedDepartment`が null)。</span><span class="sxs-lookup"><span data-stu-id="746e9-204">The filter expression always returns `true` if nothing is selected in the drop-down list (that is, `SelectedDepartment` is null).</span></span>

<span data-ttu-id="746e9-205">*Views\Course\Index.cshtml*、開始する直前に`table`タグは、ドロップダウン リストと送信ボタンを作成する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="746e9-205">In *Views\Course\Index.cshtml*, immediately before the opening `table` tag, add the following code to create the drop-down list and a submit button:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

<span data-ttu-id="746e9-206">まだブレークポイント設定、コースのインデックス ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="746e9-206">With the breakpoint still set, run the Course Index page.</span></span> <span data-ttu-id="746e9-207">最初に、コードが、ブレークポイントをヒット回を続け、ブラウザーでページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-207">Continue through the first times that the code hits a breakpoint, so that the page is displayed in the browser.</span></span> <span data-ttu-id="746e9-208">ドロップダウン リストから、部門を選択し、クリックして**フィルター**します。</span><span class="sxs-lookup"><span data-stu-id="746e9-208">Select a department from the drop-down list and click **Filter**.</span></span>

<span data-ttu-id="746e9-209">この時間、最初のブレークポイントは、ドロップダウン リストの部門のクエリになります。</span><span class="sxs-lookup"><span data-stu-id="746e9-209">This time the first breakpoint will be for the departments query for the drop-down list.</span></span> <span data-ttu-id="746e9-210">スキップして、表示、`query`変数次に、コード、ブレークポイントに到達内容を表示するには、`Course`クエリのようになりますようになりました。</span><span class="sxs-lookup"><span data-stu-id="746e9-210">Skip that and view the `query` variable the next time the code reaches the breakpoint in order to see what the `Course` query now looks like.</span></span> <span data-ttu-id="746e9-211">次のようなものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-211">You'll see something like the following:</span></span>

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

<span data-ttu-id="746e9-212">クエリを参照してください、`JOIN`読み込みクエリ`Department`データと共に、`Course`データ、および含まれている、`WHERE`句。</span><span class="sxs-lookup"><span data-stu-id="746e9-212">You can see that the query is now a `JOIN` query that loads `Department` data along with the `Course` data, and that it includes a `WHERE` clause.</span></span>

<span data-ttu-id="746e9-213">削除、`var sql = courses.ToString()`行。</span><span class="sxs-lookup"><span data-stu-id="746e9-213">Remove the `var sql = courses.ToString()` line.</span></span>

## <a name="create-an-abstraction-layer"></a><span data-ttu-id="746e9-214">抽象化レイヤーを作成します。</span><span class="sxs-lookup"><span data-stu-id="746e9-214">Create an abstraction layer</span></span>

<span data-ttu-id="746e9-215">多くの開発者は、Entity Framework で動作するコードをラップするラッパーとして、Repository パターンと Unit of Work パターンを実装するためのコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="746e9-215">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="746e9-216">これらのパターンは、アプリケーションのデータ アクセス層とビジネス ロジック層の間に抽象化レイヤーを作成するためのものです。</span><span class="sxs-lookup"><span data-stu-id="746e9-216">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="746e9-217">これらのパターンを実装すると、データ ストアの変更からアプリケーションを隔離でき、自動化された単体テストやテスト駆動開発 (TDD) を円滑化できます。</span><span class="sxs-lookup"><span data-stu-id="746e9-217">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="746e9-218">ただし、これらのパターンを実装する追加コードの記述は常にいくつかの理由、EF を使用するアプリケーションに最適な選択肢。</span><span class="sxs-lookup"><span data-stu-id="746e9-218">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

- <span data-ttu-id="746e9-219">EF コンテキスト クラス自体が、コードをデータ ストア固有のコードから隔離します。</span><span class="sxs-lookup"><span data-stu-id="746e9-219">The EF context class itself insulates your code from data-store-specific code.</span></span>
- <span data-ttu-id="746e9-220">EF コンテキスト クラスは、EF を使用して行っているデータベースの更新の unit-of-work クラスとして動作できます。</span><span class="sxs-lookup"><span data-stu-id="746e9-220">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>
- <span data-ttu-id="746e9-221">Entity Framework 6 で導入された機能を簡単にリポジトリのコードを記述することがなく、TDD を実装します。</span><span class="sxs-lookup"><span data-stu-id="746e9-221">Features introduced in Entity Framework 6 make it easier to implement TDD without writing repository code.</span></span>

<span data-ttu-id="746e9-222">リポジトリと unit of work パターンを実装する方法の詳細については、次を参照してください。[このチュートリアル シリーズの Entity Framework 5 バージョン](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="746e9-222">For more information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="746e9-223">Entity Framework 6 で TDD を実装する方法については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="746e9-223">For information about ways to implement TDD in Entity Framework 6, see the following resources:</span></span>

- [<span data-ttu-id="746e9-224">EF6 がより簡単にモックの作成の DbSets を使用</span><span class="sxs-lookup"><span data-stu-id="746e9-224">How EF6 Enables Mocking DbSets more easily</span></span>](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [<span data-ttu-id="746e9-225">モック作成フレームワークとテスト</span><span class="sxs-lookup"><span data-stu-id="746e9-225">Testing with a mocking framework</span></span>](https://msdn.microsoft.com/data/dn314429)
- [<span data-ttu-id="746e9-226">独自のテスト代替によるテスト</span><span class="sxs-lookup"><span data-stu-id="746e9-226">Testing with your own test doubles</span></span>](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>

## <a name="proxy-classes"></a><span data-ttu-id="746e9-227">プロキシ クラス</span><span class="sxs-lookup"><span data-stu-id="746e9-227">Proxy classes</span></span>

<span data-ttu-id="746e9-228">Entity Framework では、(たとえば、クエリを実行するときに) エンティティ インスタンスを作成するときは、動的に生成されたエンティティのプロキシとして機能する派生型のインスタンスとしてそのを多くの場合、作成します。</span><span class="sxs-lookup"><span data-stu-id="746e9-228">When the Entity Framework creates entity instances (for example, when you execute a query), it often creates them as instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="746e9-229">たとえば、次の 2 つのデバッガー イメージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="746e9-229">For example, see the following two debugger images.</span></span> <span data-ttu-id="746e9-230">最初のイメージで、`student`変数は、予想される`Student`エンティティをインスタンス化した直後に入力します。</span><span class="sxs-lookup"><span data-stu-id="746e9-230">In the first image, you see that the `student` variable is the expected `Student` type immediately after you instantiate the entity.</span></span> <span data-ttu-id="746e9-231">2 番目のイメージを student エンティティをデータベースから読み取る EF を使用した後、プロキシ クラスを参照します。</span><span class="sxs-lookup"><span data-stu-id="746e9-231">In the second image, after EF has been used to read a student entity from the database, you see the proxy class.</span></span>

![プロキシ クラスの前に](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![プロキシ クラスの後](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

<span data-ttu-id="746e9-234">このプロキシ クラスは、プロパティにアクセスするときにアクションを自動的に実行するためのフックを挿入するエンティティの一部の仮想プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="746e9-234">This proxy class overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="746e9-235">このメカニズムが使用する 1 つの関数では、遅延読み込みです。</span><span class="sxs-lookup"><span data-stu-id="746e9-235">One function this mechanism is used for is lazy loading.</span></span>

<span data-ttu-id="746e9-236">ほとんどの場合、このプロキシの使用を意識する必要はありませんが、例外があります。</span><span class="sxs-lookup"><span data-stu-id="746e9-236">Most of the time you don't need to be aware of this use of proxies, but there are exceptions:</span></span>

- <span data-ttu-id="746e9-237">一部のシナリオでは、Entity Framework がプロキシのインスタンスを作成するを防ぐためにする場合があります。</span><span class="sxs-lookup"><span data-stu-id="746e9-237">In some scenarios you might want to prevent the Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="746e9-238">たとえば、エンティティをシリアル化しているときに一般的にする POCO クラス、プロキシ クラスではありません。</span><span class="sxs-lookup"><span data-stu-id="746e9-238">For example, when you're serializing entities you generally want the POCO classes, not the proxy classes.</span></span> <span data-ttu-id="746e9-239">シリアル化の問題を回避する方法の 1 つは、ように、エンティティ オブジェクトではなく、データ転送オブジェクト (Dto) のシリアル化には、 [Entity Framework で Web API を使用して](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="746e9-239">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects, as shown in the [Using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) tutorial.</span></span> <span data-ttu-id="746e9-240">別の方法が、[プロキシの作成を無効にする](https://msdn.microsoft.com/data/jj592886.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="746e9-240">Another way is to [disable proxy creation](https://msdn.microsoft.com/data/jj592886.aspx).</span></span>
- <span data-ttu-id="746e9-241">インスタンス化すると、エンティティ クラスを使用して、`new`オペレーターは、プロキシ インスタンスを取得しません。</span><span class="sxs-lookup"><span data-stu-id="746e9-241">When you instantiate an entity class using the `new` operator, you don't get a proxy instance.</span></span> <span data-ttu-id="746e9-242">これは、遅延読み込みと自動変更の追跡などの機能を取得しないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="746e9-242">This means you don't get functionality such as lazy loading and automatic change tracking.</span></span> <span data-ttu-id="746e9-243">これは通常では;一般的に不要な lazy loading、データベースにない新しいエンティティを作成するためと、変更の追跡としてエンティティを明示的にマークしている場合は通常不要`Added`します。</span><span class="sxs-lookup"><span data-stu-id="746e9-243">This is typically okay; you generally don't need lazy loading, because you're creating a new entity that isn't in the database, and you generally don't need change tracking if you're explicitly marking the entity as `Added`.</span></span> <span data-ttu-id="746e9-244">ただし、遅延読み込みする必要は、変更の追跡が必要な場合は、インスタンスを作成できます新しいエンティティを使用してプロキシを持つ、[作成](https://msdn.microsoft.com/library/gg679504.aspx)のメソッド、`DbSet`クラス。</span><span class="sxs-lookup"><span data-stu-id="746e9-244">However, if you do need lazy loading and you need change tracking, you can create new entity instances with proxies using the [Create](https://msdn.microsoft.com/library/gg679504.aspx) method of the `DbSet` class.</span></span>
- <span data-ttu-id="746e9-245">プロキシ型から実際のエンティティ型を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="746e9-245">You might want to get an actual entity type from a proxy type.</span></span> <span data-ttu-id="746e9-246">使用することができます、 [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx)のメソッド、`ObjectContext`プロキシ型のインスタンスの実際のエンティティ型を取得するクラス。</span><span class="sxs-lookup"><span data-stu-id="746e9-246">You can use the [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) method of the `ObjectContext` class to get the actual entity type of a proxy type instance.</span></span>

<span data-ttu-id="746e9-247">詳細については、次を参照してください。 [Proxies の操作](https://msdn.microsoft.com/data/JJ592886.aspx)msdn です。</span><span class="sxs-lookup"><span data-stu-id="746e9-247">For more information, see [Working with Proxies](https://msdn.microsoft.com/data/JJ592886.aspx) on MSDN.</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="746e9-248">変更の自動検出</span><span class="sxs-lookup"><span data-stu-id="746e9-248">Automatic change detection</span></span>

<span data-ttu-id="746e9-249">Entity Framework では、エンティティの現在の値と元の値を比較して、エンティティがどのように変更されたか (およびそれによって、どの更新プログラムをデータベースに送信する必要があるか) を判断します。</span><span class="sxs-lookup"><span data-stu-id="746e9-249">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="746e9-250">元の値は、エンティティが照会されるかアタッチされるときに格納されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-250">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="746e9-251">変更の自動検出を行うメソッドには、次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="746e9-251">Some of the methods that cause automatic change detection are the following:</span></span>

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

<span data-ttu-id="746e9-252">多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、一時的に変更の自動検出を使用して無効にすることで大幅なパフォーマンス向上を取得可能性があります、 [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="746e9-252">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) property.</span></span> <span data-ttu-id="746e9-253">詳細については、次を参照してください。[変更を自動的に検出](https://msdn.microsoft.com/data/jj556205)msdn です。</span><span class="sxs-lookup"><span data-stu-id="746e9-253">For more information, see [Automatically Detecting Changes](https://msdn.microsoft.com/data/jj556205) on MSDN.</span></span>

## <a name="automatic-validation"></a><span data-ttu-id="746e9-254">自動検証</span><span class="sxs-lookup"><span data-stu-id="746e9-254">Automatic validation</span></span>

<span data-ttu-id="746e9-255">呼び出すと、`SaveChanges`メソッド、既定では、Entity Framework データを検証、変更されたすべてのエンティティのすべてのプロパティで、データベースを更新する前にします。</span><span class="sxs-lookup"><span data-stu-id="746e9-255">When you call the `SaveChanges` method, by default the Entity Framework validates the data in all properties of all changed entities before updating the database.</span></span> <span data-ttu-id="746e9-256">多数のエンティティを更新したら、既に検証したデータは、この作業は必要ありませんし保存するプロセスを行うことができます、変更は一時的に検証を無効にすることで時間が。</span><span class="sxs-lookup"><span data-stu-id="746e9-256">If you've updated a large number of entities and you've already validated the data, this work is unnecessary and you could make the process of saving the changes take less time by temporarily turning off validation.</span></span> <span data-ttu-id="746e9-257">使用して行うことができます、 [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="746e9-257">You can do that using the [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) property.</span></span> <span data-ttu-id="746e9-258">詳細については、次を参照してください。[検証](https://msdn.microsoft.com/data/gg193959)msdn です。</span><span class="sxs-lookup"><span data-stu-id="746e9-258">For more information, see [Validation](https://msdn.microsoft.com/data/gg193959) on MSDN.</span></span>

## <a name="entity-framework-power-tools"></a><span data-ttu-id="746e9-259">Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="746e9-259">Entity Framework Power Tools</span></span>

<span data-ttu-id="746e9-260">[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition)データ モデルのダイアグラムの作成に使用された Visual Studio アドインは、このチュートリアルで示します。</span><span class="sxs-lookup"><span data-stu-id="746e9-260">[Entity Framework Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) is a Visual Studio add-in that was used to create the data model diagrams shown in these tutorials.</span></span> <span data-ttu-id="746e9-261">ツールでは、Code First とデータベースを使用できるように、既存のデータベース内のテーブルに基づくエンティティ クラスを生成するなどの他の関数も実行できます。</span><span class="sxs-lookup"><span data-stu-id="746e9-261">The tools can also do other function such as generate entity classes based on the tables in an existing database so that you can use the database with Code First.</span></span> <span data-ttu-id="746e9-262">ツールをインストールすると、コンテキスト メニューに追加のオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="746e9-262">After you install the tools, some additional options appear in context menus.</span></span> <span data-ttu-id="746e9-263">たとえば、右クリックすると、コンテキスト クラスを**ソリューション エクスプ ローラー**、表示と**Entity Framework**オプション。</span><span class="sxs-lookup"><span data-stu-id="746e9-263">For example, when you right-click your context class in **Solution Explorer**, you see and **Entity Framework** option.</span></span> <span data-ttu-id="746e9-264">これにより、図を生成します。</span><span class="sxs-lookup"><span data-stu-id="746e9-264">This gives you the ability to generate a diagram.</span></span> <span data-ttu-id="746e9-265">Code First を使用する場合、図では、データ モデルを変更することはできませんを理解しやすいように内点を移動することができます。</span><span class="sxs-lookup"><span data-stu-id="746e9-265">When you're using Code First you can't change the data model in the diagram, but you can move things around to make it easier to understand.</span></span>

![EF のダイアグラム](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

## <a name="entity-framework-source-code"></a><span data-ttu-id="746e9-267">Entity Framework のソース コード</span><span class="sxs-lookup"><span data-stu-id="746e9-267">Entity Framework source code</span></span>

<span data-ttu-id="746e9-268">Entity Framework 6 のソース コードは[GitHub](https://github.com/aspnet/EntityFramework6)します。</span><span class="sxs-lookup"><span data-stu-id="746e9-268">The source code for Entity Framework 6 is available at [GitHub](https://github.com/aspnet/EntityFramework6).</span></span> <span data-ttu-id="746e9-269">バグを探し、EF のソース コードを独自の機能強化を投稿することができます。</span><span class="sxs-lookup"><span data-stu-id="746e9-269">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="746e9-270">ソース コードはオープンですが、Entity Framework は、Microsoft 製品がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="746e9-270">Although the source code is open, Entity Framework is fully supported as a Microsoft product.</span></span> <span data-ttu-id="746e9-271">Microsoft Entity Framework チームは、各リリースの品質を保証するため、受け入れる投稿を管理し、すべてのコード変更をテストしています。</span><span class="sxs-lookup"><span data-stu-id="746e9-271">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="746e9-272">謝辞</span><span class="sxs-lookup"><span data-stu-id="746e9-272">Acknowledgments</span></span>

- <span data-ttu-id="746e9-273">Tom Dykstra は、このチュートリアルの元のバージョンを記述した、EF 5 更新プログラムを共同執筆し、EF 6 の更新プログラムを記述します。</span><span class="sxs-lookup"><span data-stu-id="746e9-273">Tom Dykstra wrote the original version of this tutorial, co-authored the EF 5 update, and wrote the EF 6 update.</span></span> <span data-ttu-id="746e9-274">Tom は、Microsoft Web プラットフォームとツール コンテンツ チームのシニア プログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="746e9-274">Tom is a senior programming writer on the Microsoft Web Platform and Tools Content Team.</span></span>
- <span data-ttu-id="746e9-275">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) EF 5 と MVC 4 のチュートリアルを更新する作業の大半を EF 6 の更新プログラムを共同で作成します。</span><span class="sxs-lookup"><span data-stu-id="746e9-275">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) did most of the work updating the tutorial for EF 5 and MVC 4 and co-authored the EF 6 update.</span></span> <span data-ttu-id="746e9-276">Rick は、Azure と MVC に注目して Microsoft のシニア プログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="746e9-276">Rick is a senior programming writer for Microsoft focusing on Azure and MVC.</span></span>
- <span data-ttu-id="746e9-277">[Rowan Miller](http://www.romiller.com)と Entity Framework チームの他のメンバーがコード レビューを支援し、EF 5 と 6 の EF のチュートリアルを更新していた私たちの中に発生した移行に関する多くの問題のデバッグに協力します。</span><span class="sxs-lookup"><span data-stu-id="746e9-277">[Rowan Miller](http://www.romiller.com) and other members of the Entity Framework team assisted with code reviews and helped debug many issues with migrations that arose while we were updating the tutorial for EF 5 and EF 6.</span></span>

## <a name="troubleshoot-common-errors"></a><span data-ttu-id="746e9-278">一般的なエラーをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="746e9-278">Troubleshoot common errors</span></span>

### <a name="cannot-createshadow-copy"></a><span data-ttu-id="746e9-279">コピーを作成/シャドウことはできません。</span><span class="sxs-lookup"><span data-stu-id="746e9-279">Cannot create/shadow copy</span></span>

<span data-ttu-id="746e9-280">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="746e9-280">Error Message:</span></span>

> <span data-ttu-id="746e9-281">コピーを作成/シャドウできません '&lt;filename&gt;' そのファイルが既に存在します。</span><span class="sxs-lookup"><span data-stu-id="746e9-281">Cannot create/shadow copy '&lt;filename&gt;' when that file already exists.</span></span>

<span data-ttu-id="746e9-282">ソリューション</span><span class="sxs-lookup"><span data-stu-id="746e9-282">Solution</span></span>

<span data-ttu-id="746e9-283">数秒待ってからページを更新します。</span><span class="sxs-lookup"><span data-stu-id="746e9-283">Wait a few seconds and refresh the page.</span></span>

### <a name="update-database-not-recognized"></a><span data-ttu-id="746e9-284">データベースの更新認識されません。</span><span class="sxs-lookup"><span data-stu-id="746e9-284">Update-Database not recognized</span></span>

<span data-ttu-id="746e9-285">エラー メッセージ (から、 `Update-Database` PMC でコマンド)。</span><span class="sxs-lookup"><span data-stu-id="746e9-285">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="746e9-286">'データベースの更新' という用語はコマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前として認識されません。</span><span class="sxs-lookup"><span data-stu-id="746e9-286">The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="746e9-287">名前のスペルを確認するか、パスが正しいことを確認し、もう一度お試しパスが含まれている場合。</span><span class="sxs-lookup"><span data-stu-id="746e9-287">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>

<span data-ttu-id="746e9-288">ソリューション</span><span class="sxs-lookup"><span data-stu-id="746e9-288">Solution</span></span>

<span data-ttu-id="746e9-289">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="746e9-289">Exit Visual Studio.</span></span> <span data-ttu-id="746e9-290">プロジェクトを再度開いてもう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="746e9-290">Reopen project and try again.</span></span>

### <a name="validation-failed"></a><span data-ttu-id="746e9-291">検証に失敗しました</span><span class="sxs-lookup"><span data-stu-id="746e9-291">Validation failed</span></span>

<span data-ttu-id="746e9-292">エラー メッセージ (から、 `Update-Database` PMC でコマンド)。</span><span class="sxs-lookup"><span data-stu-id="746e9-292">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="746e9-293">1 つまたは複数のエンティティの検証に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="746e9-293">Validation failed for one or more entities.</span></span> <span data-ttu-id="746e9-294">詳細については、'EntityValidationErrors' プロパティを参照してください。</span><span class="sxs-lookup"><span data-stu-id="746e9-294">See 'EntityValidationErrors' property for more details.</span></span>

<span data-ttu-id="746e9-295">ソリューション</span><span class="sxs-lookup"><span data-stu-id="746e9-295">Solution</span></span>

<span data-ttu-id="746e9-296">この問題の原因の 1 つは、検証エラー時に、`Seed`メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="746e9-296">One cause of this problem is validation errors when the `Seed` method runs.</span></span> <span data-ttu-id="746e9-297">参照してください[シード処理中、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)デバッグに関するヒント、`Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="746e9-297">See [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) for tips on debugging the `Seed` method.</span></span>

### <a name="http-50019-error"></a><span data-ttu-id="746e9-298">HTTP 500.19 エラー</span><span class="sxs-lookup"><span data-stu-id="746e9-298">HTTP 500.19 error</span></span>

<span data-ttu-id="746e9-299">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="746e9-299">Error Message:</span></span>

> <span data-ttu-id="746e9-300">HTTP エラー 500.19 - 内部サーバー エラー、要求されたページは、ページの関連する構成データが無効であるために、アクセスできません。</span><span class="sxs-lookup"><span data-stu-id="746e9-300">HTTP Error 500.19 - Internal Server Error The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

<span data-ttu-id="746e9-301">ソリューション</span><span class="sxs-lookup"><span data-stu-id="746e9-301">Solution</span></span>

<span data-ttu-id="746e9-302">このエラーを取得する方法の 1 つは、同じポート番号を使用してそれらの各ソリューションの複数のコピーです。</span><span class="sxs-lookup"><span data-stu-id="746e9-302">One way you can get this error is from having multiple copies of the solution, each of them using the same port number.</span></span> <span data-ttu-id="746e9-303">この問題の解決には、Visual Studio のすべてのインスタンスを終了し、プロジェクトでの作業を再起動する通常します。</span><span class="sxs-lookup"><span data-stu-id="746e9-303">You can usually solve this problem by exiting all instances of Visual Studio, then restarting the project you're working on.</span></span> <span data-ttu-id="746e9-304">うまく行かない場合は、ポート番号を変更してみてください。</span><span class="sxs-lookup"><span data-stu-id="746e9-304">If that doesn't work, try changing the port number.</span></span> <span data-ttu-id="746e9-305">プロジェクト ファイルを右クリックし、し、[プロパティ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="746e9-305">Right click on the project file and then click properties.</span></span> <span data-ttu-id="746e9-306">選択、 **Web**タブをされているポート番号を変更し、**プロジェクト Url**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="746e9-306">Select the **Web** tab and then change the port number in the **Project Url** text box.</span></span>

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="746e9-307">SQL Server インスタンスの位置を特定しているときのエラー</span><span class="sxs-lookup"><span data-stu-id="746e9-307">Error locating SQL Server instance</span></span>

<span data-ttu-id="746e9-308">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="746e9-308">Error Message:</span></span>

> <span data-ttu-id="746e9-309">SQL Server への接続を確立しているときに、ネットワーク関連またはインスタンス固有のエラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="746e9-309">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="746e9-310">サーバーが見つからないかアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="746e9-310">The server was not found or was not accessible.</span></span> <span data-ttu-id="746e9-311">インスタンス名が正しいこと、および SQL Server がリモート接続を許可するように構成されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="746e9-311">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="746e9-312">(プロバイダー:SQL ネットワーク インターフェイス、エラー:26 - 指定されたサーバーまたはインスタンスの位置を特定しているときにエラーが発生しました)</span><span class="sxs-lookup"><span data-stu-id="746e9-312">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="746e9-313">ソリューション</span><span class="sxs-lookup"><span data-stu-id="746e9-313">Solution</span></span>

<span data-ttu-id="746e9-314">接続文字列を確認します。</span><span class="sxs-lookup"><span data-stu-id="746e9-314">Check the connection string.</span></span> <span data-ttu-id="746e9-315">データベースを手動で削除した場合は、構築文字列でデータベースの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="746e9-315">If you have manually deleted the database, change the name of the database in the construction string.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="746e9-316">コードを取得する</span><span class="sxs-lookup"><span data-stu-id="746e9-316">Get the code</span></span>

[<span data-ttu-id="746e9-317">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="746e9-317">Download Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="746e9-318">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="746e9-318">Additional resources</span></span>

 <span data-ttu-id="746e9-319">Entity Framework を使用してデータを操作する方法の詳細については、次を参照してください。、 [msdn ドキュメント ページを EF](https://msdn.microsoft.com/data/ee712907)と[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="746e9-319">For more information about how to work with data using the Entity Framework, see the [EF documentation page on MSDN](https://msdn.microsoft.com/data/ee712907) and [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="746e9-320">これを構築した後、web アプリケーションをデプロイする方法の詳細については、次を参照してください。 [ASP.NET Web 展開 - 推奨リソース](../../../../whitepapers/aspnet-web-deployment-content-map.md)、MSDN ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="746e9-320">For more information about how to deploy your web application after you've built it, see [ASP.NET Web Deployment - Recommended Resources](../../../../whitepapers/aspnet-web-deployment-content-map.md) in the MSDN Library.</span></span>

<span data-ttu-id="746e9-321">については、認証および承認など、MVC に関連するその他のトピックを参照してください、 [ASP.NET MVC - 推奨リソース](../recommended-resources-for-mvc.md)します。</span><span class="sxs-lookup"><span data-stu-id="746e9-321">For information about other topics related to MVC, such as authentication and authorization, see the [ASP.NET MVC - Recommended Resources](../recommended-resources-for-mvc.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="746e9-322">次の手順</span><span class="sxs-lookup"><span data-stu-id="746e9-322">Next steps</span></span>

<span data-ttu-id="746e9-323">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="746e9-323">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="746e9-324">生 SQL クエリの実行</span><span class="sxs-lookup"><span data-stu-id="746e9-324">Performed raw SQL queries</span></span>
> * <span data-ttu-id="746e9-325">追跡なしのクエリの実行</span><span class="sxs-lookup"><span data-stu-id="746e9-325">Performed no-tracking queries</span></span>
> * <span data-ttu-id="746e9-326">データベースに送信する SQL クエリの検査</span><span class="sxs-lookup"><span data-stu-id="746e9-326">Examined SQL queries sent to the database</span></span>

<span data-ttu-id="746e9-327">詳細についても学習しました。</span><span class="sxs-lookup"><span data-stu-id="746e9-327">You also learned about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="746e9-328">抽象化レイヤーを作成します。</span><span class="sxs-lookup"><span data-stu-id="746e9-328">Creating an abstraction layer</span></span>
> * <span data-ttu-id="746e9-329">プロキシ クラス</span><span class="sxs-lookup"><span data-stu-id="746e9-329">Proxy classes</span></span>
> * <span data-ttu-id="746e9-330">変更の自動検出</span><span class="sxs-lookup"><span data-stu-id="746e9-330">Automatic change detection</span></span>
> * <span data-ttu-id="746e9-331">自動検証</span><span class="sxs-lookup"><span data-stu-id="746e9-331">Automatic validation</span></span>
> * <span data-ttu-id="746e9-332">Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="746e9-332">Entity Framework Power Tools</span></span>
> * <span data-ttu-id="746e9-333">Entity Framework のソース コード</span><span class="sxs-lookup"><span data-stu-id="746e9-333">Entity Framework source code</span></span>

<span data-ttu-id="746e9-334">これは、この一連の ASP.NET MVC アプリケーションで Entity Framework の使用に関するチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="746e9-334">This completes this series of tutorials on using the Entity Framework in an ASP.NET MVC application.</span></span> <span data-ttu-id="746e9-335">EF Database First について学習する場合は、DB の最初のチュートリアル シリーズを参照してください。</span><span class="sxs-lookup"><span data-stu-id="746e9-335">If you want to learn about EF Database First, see the DB First tutorial series.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="746e9-336">Entity Framework データベース ファースト</span><span class="sxs-lookup"><span data-stu-id="746e9-336">Entity Framework Database First</span></span>](../database-first-development/setting-up-database.md)