---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Entity Framework 6 のシナリオを高度な MVC 5 Web アプリケーション (12/12) の |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3d6cc52f7fa3089f30f1a6bbd76593f1eca95009
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a><span data-ttu-id="ee696-103">MVC 5 Web アプリケーション (12/12) の高度な Entity Framework 6 のシナリオ</span><span class="sxs-lookup"><span data-stu-id="ee696-103">Advanced Entity Framework 6 Scenarios for an MVC 5 Web Application (12 of 12)</span></span>
====================
<span data-ttu-id="ee696-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ee696-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ee696-105">[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="ee696-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="ee696-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ee696-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="ee696-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="ee696-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="ee696-108">前のチュートリアルでは、テーブルの階層あたりの継承を実装します。</span><span class="sxs-lookup"><span data-stu-id="ee696-108">In the previous tutorial you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="ee696-109">このチュートリアルでは、Entity Framework Code First を使用する ASP.NET web アプリケーションの開発の基礎を超えたときに認識する便利ないくつかのトピックについて紹介します。</span><span class="sxs-lookup"><span data-stu-id="ee696-109">This tutorial includes introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET web applications that use Entity Framework Code First.</span></span> <span data-ttu-id="ee696-110">詳細な手順を進めていきますコードと、次のトピックの Visual Studio を使用します。</span><span class="sxs-lookup"><span data-stu-id="ee696-110">Step-by-step instructions walk you through the code and using Visual Studio for the following topics:</span></span>

- [<span data-ttu-id="ee696-111">生の SQL クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee696-111">Performing raw SQL queries</span></span>](#rawsql)
- [<span data-ttu-id="ee696-112">No の追跡クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee696-112">Performing no-tracking queries</span></span>](#notracking)
- [<span data-ttu-id="ee696-113">データベースに送信する SQL の確認</span><span class="sxs-lookup"><span data-stu-id="ee696-113">Examining SQL sent to the database</span></span>](#sql)

<span data-ttu-id="ee696-114">このチュートリアルでは、簡単な概要については次の詳細については、リソースをリンクでいくつかのトピックが導入されています。</span><span class="sxs-lookup"><span data-stu-id="ee696-114">The tutorial introduces several topics with brief introductions followed by links to resources for more information:</span></span>

- [<span data-ttu-id="ee696-115">リポジトリと作業パターンの単位</span><span class="sxs-lookup"><span data-stu-id="ee696-115">Repository and unit of work patterns</span></span>](#repo)
- [<span data-ttu-id="ee696-116">プロキシ クラス</span><span class="sxs-lookup"><span data-stu-id="ee696-116">Proxy classes</span></span>](#proxies)
- [<span data-ttu-id="ee696-117">自動の変更の検出</span><span class="sxs-lookup"><span data-stu-id="ee696-117">Automatic change detection</span></span>](#changedetection)
- [<span data-ttu-id="ee696-118">自動検証</span><span class="sxs-lookup"><span data-stu-id="ee696-118">Automatic validation</span></span>](#validation)
- [<span data-ttu-id="ee696-119">Visual Studio の EF ツール</span><span class="sxs-lookup"><span data-stu-id="ee696-119">EF tools for Visual Studio</span></span>](#tools)
- [<span data-ttu-id="ee696-120">Entity Framework のソース コード</span><span class="sxs-lookup"><span data-stu-id="ee696-120">Entity Framework source code</span></span>](#source)

<span data-ttu-id="ee696-121">このチュートリアルでは、次のセクションも含まれます。</span><span class="sxs-lookup"><span data-stu-id="ee696-121">The tutorial also includes the following sections:</span></span>

- [<span data-ttu-id="ee696-122">まとめ</span><span class="sxs-lookup"><span data-stu-id="ee696-122">Summary</span></span>](#summary)
- [<span data-ttu-id="ee696-123">受信確認</span><span class="sxs-lookup"><span data-stu-id="ee696-123">Acknowledgments</span></span>](#acknowledgments)
- [<span data-ttu-id="ee696-124">VB に関する注意事項</span><span class="sxs-lookup"><span data-stu-id="ee696-124">A note about VB</span></span>](#vb)
- [<span data-ttu-id="ee696-125">一般的なエラーと解決方法またはそれらの回避策</span><span class="sxs-lookup"><span data-stu-id="ee696-125">Common errors, and solutions or workarounds for them</span></span>](#errors)

<span data-ttu-id="ee696-126">これらのトピックのほとんどは、既に作成しているページを操作します。</span><span class="sxs-lookup"><span data-stu-id="ee696-126">For most of these topics, you'll work with pages that you already created.</span></span> <span data-ttu-id="ee696-127">一括更新を実行する生の SQL を使用するには、データベース内のすべてのコースのクレジットの数を更新する新しいページを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee696-127">To use raw SQL to do bulk updates you'll create a new page that updates the number of credits of all courses in the database:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a><span data-ttu-id="ee696-129">生の SQL クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee696-129">Performing Raw SQL Queries</span></span>

<span data-ttu-id="ee696-130">Entity Framework コードの最初の API には、データベースに直接 SQL コマンドを渡すことができるようにするメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ee696-130">The Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="ee696-131">次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="ee696-131">You have the following options:</span></span>

- <span data-ttu-id="ee696-132">使用して、 [DbSet.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.sqlquery.aspx)をエンティティ型を返すクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee696-132">Use the [DbSet.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.sqlquery.aspx) method for queries that return entity types.</span></span> <span data-ttu-id="ee696-133">返されたオブジェクトで想定されている型でなければなりません、`DbSet`オブジェクト、およびそれらが自動的に追跡データベースのコンテキストによって追跡を無効にする場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="ee696-133">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you turn tracking off.</span></span> <span data-ttu-id="ee696-134">(は、次のセクションを参照してください、 [AsNoTracking](https://msdn.microsoft.com/en-us/library/system.data.entity.dbextensions.asnotracking.aspx)メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="ee696-134">(See the following section about the [AsNoTracking](https://msdn.microsoft.com/en-us/library/system.data.entity.dbextensions.asnotracking.aspx) method.)</span></span>
- <span data-ttu-id="ee696-135">使用して、 [Database.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.database.sqlquery.aspx)をエンティティがない型を返すクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee696-135">Use the [Database.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.database.sqlquery.aspx) method for queries that return types that aren't entities.</span></span> <span data-ttu-id="ee696-136">返されるデータは、エンティティの種類を取得するこのメソッドを使用する場合でもデータベース コンテキストによって追跡されていません。</span><span class="sxs-lookup"><span data-stu-id="ee696-136">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>
- <span data-ttu-id="ee696-137">使用して、 [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456.aspx)非クエリ コマンド。</span><span class="sxs-lookup"><span data-stu-id="ee696-137">Use the [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456.aspx) for non-query commands.</span></span>

<span data-ttu-id="ee696-138">Entity Framework を使用する利点の 1 つは、コードのデータを格納する特定のメソッドを過度に結び付ける済む点です。</span><span class="sxs-lookup"><span data-stu-id="ee696-138">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="ee696-139">これは、SQL クエリとコマンドの生成も自分で記述することから解放することで実行します。</span><span class="sxs-lookup"><span data-stu-id="ee696-139">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="ee696-140">例外的なシナリオは、手動で作成した特定の SQL クエリを実行する必要がある場合とこれらのメソッドのできるようにすると、このような例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="ee696-140">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created, and these methods make it possible for you to handle such exceptions.</span></span>

<span data-ttu-id="ee696-141">常に true web アプリケーションで SQL コマンドを実行するときに、としては、SQL インジェクション攻撃からサイトを保護する対策を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee696-141">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="ee696-142">行うには 1 つの方法では、パラメーター化クエリを使用して、SQL コマンドとして web ページによって送信される文字列を解釈できないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="ee696-142">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="ee696-143">このチュートリアルでは、ユーザー入力をクエリに統合するとき、パラメーター化クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee696-143">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

### <a name="calling-a-query-that-returns-entities"></a><span data-ttu-id="ee696-144">クエリを呼び出すには、エンティティが返されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-144">Calling a Query that Returns Entities</span></span>

<span data-ttu-id="ee696-145">[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/en-us/library/gg696460.aspx)クラス型のエンティティを返すクエリの実行に使用できるメソッドを提供`TEntity`です。</span><span class="sxs-lookup"><span data-stu-id="ee696-145">The [DbSet&lt;TEntity&gt;](https://msdn.microsoft.com/en-us/library/gg696460.aspx) class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="ee696-146">内のコードを変更するしくみを表示する、`Details`のメソッド、`Department`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="ee696-146">To see how this works you'll change the code in the `Details` method of the `Department` controller.</span></span>

<span data-ttu-id="ee696-147">*DepartmentController.cs*で、`Details`メソッド、置換、`db.Departments.FindAsync`メソッド呼び出し、`db.Departments.SqlQuery`メソッドの呼び出し、次の強調表示されたコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="ee696-147">In *DepartmentController.cs*, in the `Details` method, replace the `db.Departments.FindAsync` method call with a `db.Departments.SqlQuery` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

<span data-ttu-id="ee696-148">新しいコードが正しく動作することを確認するには、次のように選択します。、**部門**タブし、**詳細**部門のいずれか。</span><span class="sxs-lookup"><span data-stu-id="ee696-148">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![部門の詳細](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a><span data-ttu-id="ee696-150">クエリを呼び出すには、その他の種類のオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-150">Calling a Query that Returns Other Types of Objects</span></span>

<span data-ttu-id="ee696-151">以前登録日付ごとの生徒の数が映っているバージョン情報 ページの受講者統計グリッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee696-151">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="ee696-152">これを行うコード*HomeController.cs* LINQ を使用します。</span><span class="sxs-lookup"><span data-stu-id="ee696-152">The code that does this in *HomeController.cs* uses LINQ:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

<span data-ttu-id="ee696-153">LINQ を使用するのではなく、SQL の直接このデータを取得するコードを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="ee696-153">Suppose you want to write the code that retrieves this data directly in SQL rather than using LINQ.</span></span> <span data-ttu-id="ee696-154">エンティティ オブジェクト以外のものを返すクエリを実行する必要があることを意味する必要がありますを使用して、 [Database.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.database.sqlquery(v=VS.103).aspx)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ee696-154">To do that you need to run a query that returns something other than entity objects, which means you need to use the [Database.SqlQuery](https://msdn.microsoft.com/en-us/library/system.data.entity.database.sqlquery(v=VS.103).aspx) method.</span></span>

<span data-ttu-id="ee696-155">*HomeController.cs*での LINQ ステートメントは、置換、 `About` SQL ステートメントには、次の強調表示されたコードに示すように、メソッド。</span><span class="sxs-lookup"><span data-stu-id="ee696-155">In *HomeController.cs*, replace the LINQ statement in the `About` method with a SQL statement, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

<span data-ttu-id="ee696-156">バージョン情報 ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee696-156">Run the About page.</span></span> <span data-ttu-id="ee696-157">以前と同じ、同じデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-157">It displays the same data it did before.</span></span>

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a><span data-ttu-id="ee696-159">更新クエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="ee696-159">Calling an Update Query</span></span>

<span data-ttu-id="ee696-160">Contoso 大学管理者がすべてコースのクレジットの数の変更などのデータベースで一括変更を実行することができるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="ee696-160">Suppose Contoso University administrators want to be able to perform bulk changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="ee696-161">かかる大学のコースの数が多い場合は、できなくなるそれらすべてのエンティティとして取得し、それらを個別に変更が効率的です。</span><span class="sxs-lookup"><span data-stu-id="ee696-161">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="ee696-162">ユーザーが、すべてのコースのクレジットの数を変更する係数を指定できるようにする web ページを実装するこのセクションの内容と SQL を実行することによって、変更を加えるを`UPDATE`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="ee696-162">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL `UPDATE` statement.</span></span> <span data-ttu-id="ee696-163">Web ページは、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="ee696-163">The web page will look like the following illustration:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

<span data-ttu-id="ee696-165">*CourseContoller.cs*、追加`UpdateCourseCredits`メソッド`HttpGet`と`HttpPost`:</span><span class="sxs-lookup"><span data-stu-id="ee696-165">In *CourseContoller.cs*, add `UpdateCourseCredits` methods for `HttpGet` and `HttpPost`:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

<span data-ttu-id="ee696-166">コント ローラーを処理すると、`HttpGet`要求、何も返されないで、`ViewBag.RowsAffected`変数、およびビューが表示されます、空のテキスト ボックスと [送信] ボタン、前の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="ee696-166">When the controller processes an `HttpGet` request, nothing is returned in the `ViewBag.RowsAffected` variable, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="ee696-167">ときに、**更新**ボタンをクリックして、`HttpPost`メソッドを呼び出すと`multiplier`テキスト ボックスに入力した値を持ちます。</span><span class="sxs-lookup"><span data-stu-id="ee696-167">When the **Update** button is clicked, the `HttpPost` method is called, and `multiplier` has the value entered in the text box.</span></span> <span data-ttu-id="ee696-168">コードの更新を記録するコースのビューに影響を受けた行の数を返します SQL を実行し、`ViewBag.RowsAffected`変数。</span><span class="sxs-lookup"><span data-stu-id="ee696-168">The code then executes the SQL that updates courses and returns the number of affected rows to the view in the `ViewBag.RowsAffected` variable.</span></span> <span data-ttu-id="ee696-169">ビューは、その値を取得、変数がテキスト ボックスの代わりに更新された行の数を表示され、次の図に示すように、ボタンを送信します。</span><span class="sxs-lookup"><span data-stu-id="ee696-169">When the view gets a value in that variable, it displays the number of rows updated instead of the text box and submit button, as shown in the following illustration:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

<span data-ttu-id="ee696-171">*CourseController.cs*のいずれかを右クリックし、`UpdateCourseCredits`メソッド、およびクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="ee696-171">In *CourseController.cs*, right-click one of the `UpdateCourseCredits` methods, and then click **Add View**.</span></span>

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

<span data-ttu-id="ee696-173">*Views\Course\UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ee696-173">In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

<span data-ttu-id="ee696-174">実行、`UpdateCourseCredits`メソッドを選択して、**コース** タブで、追加し、"/UpdateCourseCredits"ブラウザーのアドレス バーの URL の末尾に (たとえば: `http://localhost:50205/Course/UpdateCourseCredits`)。</span><span class="sxs-lookup"><span data-stu-id="ee696-174">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:50205/Course/UpdateCourseCredits`).</span></span> <span data-ttu-id="ee696-175">テキスト ボックスに数値を入力します。</span><span class="sxs-lookup"><span data-stu-id="ee696-175">Enter a number in the text box:</span></span>

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

<span data-ttu-id="ee696-177">**[更新]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee696-177">Click **Update**.</span></span> <span data-ttu-id="ee696-178">影響を受ける行の数が参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-178">You see the number of rows affected:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

<span data-ttu-id="ee696-180">をクリックして**一覧に戻る**コースのクレジットの改訂された数との一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="ee696-180">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

<span data-ttu-id="ee696-182">生の SQL クエリの詳細については、次を参照してください。[生の SQL クエリ](https://msdn.microsoft.com/en-us/data/jj592907)msdn です。</span><span class="sxs-lookup"><span data-stu-id="ee696-182">For more information about raw SQL queries, see [Raw SQL Queries](https://msdn.microsoft.com/en-us/data/jj592907) on MSDN.</span></span>

<a id="notracking"></a>
## <a name="no-tracking-queries"></a><span data-ttu-id="ee696-183">No 追跡クエリ</span><span class="sxs-lookup"><span data-stu-id="ee696-183">No-Tracking Queries</span></span>

<span data-ttu-id="ee696-184">データベース コンテキストは、テーブルの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定では、追跡データベース内にメモリ内のエンティティが同期されているかどうか。</span><span class="sxs-lookup"><span data-stu-id="ee696-184">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="ee696-185">メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使用します。</span><span class="sxs-lookup"><span data-stu-id="ee696-185">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="ee696-186">このキャッシュではありません多くの場合、web アプリケーションで必要なコンテキストをインスタンス化は通常短時間 (新しい 1 つが作成され、破棄要求ごとに) とコンテキストを読み取るそのエンティティが再度使用される前に、通常、エンティティが破棄されています。</span><span class="sxs-lookup"><span data-stu-id="ee696-186">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="ee696-187">使用してメモリ内のエンティティ オブジェクトの追跡を無効にすることができます、 [AsNoTracking](https://msdn.microsoft.com/en-us/library/gg679352(v=vs.103).aspx)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ee696-187">You can disable tracking of entity objects in memory by using the [AsNoTracking](https://msdn.microsoft.com/en-us/library/gg679352(v=vs.103).aspx) method.</span></span> <span data-ttu-id="ee696-188">実行する一般的なシナリオを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="ee696-188">Typical scenarios in which you might want to do that include the following:</span></span>

- <span data-ttu-id="ee696-189">クエリでは、大量の追跡を無効にするとパフォーマンスを向上させる著しくするデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="ee696-189">A query retrieves such a large volume of data that turning off tracking might noticeably enhance performance.</span></span>
- <span data-ttu-id="ee696-190">前のさまざまな目的は同じエンティティを取得したが、更新するためにエンティティをアタッチする場合します。</span><span class="sxs-lookup"><span data-stu-id="ee696-190">You want to attach an entity in order to update it, but you earlier retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="ee696-191">エンティティは、データベースのコンテキストによって既に追跡されているが、ために、変更するエンティティをアタッチできません。</span><span class="sxs-lookup"><span data-stu-id="ee696-191">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="ee696-192">この状況に対処する方法の 1 つを使用して、`AsNoTracking`オプションは、以前のクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="ee696-192">One way to handle this situation is to use the `AsNoTracking` option with the earlier query.</span></span>

<span data-ttu-id="ee696-193">使用する方法を示す例については、 [AsNoTracking](https://msdn.microsoft.com/en-us/library/gg679352(v=vs.103).aspx)メソッドを参照してください[このチュートリアルの以前のバージョン](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="ee696-193">For an example that demonstrates how to use the [AsNoTracking](https://msdn.microsoft.com/en-us/library/gg679352(v=vs.103).aspx) method, see [the earlier version of this tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md).</span></span> <span data-ttu-id="ee696-194">チュートリアルのこのバージョンは、必要があるために、メソッドでは、編集、モデル バインダーに作成されたエンティティでの Modified フラグを設定しない`AsNoTracking`です。</span><span class="sxs-lookup"><span data-stu-id="ee696-194">This version of the tutorial doesn't set the Modified flag on a model-binder-created entity in the Edit method, so it doesn't need `AsNoTracking`.</span></span>

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a><span data-ttu-id="ee696-195">データベースに送信する SQL の確認</span><span class="sxs-lookup"><span data-stu-id="ee696-195">Examining SQL sent to the database</span></span>

<span data-ttu-id="ee696-196">データベースに送信される実際の SQL クエリを表示できるようにすると役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="ee696-196">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="ee696-197">以前のチュートリアルでは、インターセプターのコードで操作する方法を説明今すぐをインターセプター コードを書かずに行うにはいくつかの方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-197">In an earlier tutorial you saw how to do that in interceptor code; now you'll see some ways to do it without writing interceptor code.</span></span> <span data-ttu-id="ee696-198">これを試すをするには、単純なクエリを見てし、このような一括読み込み、フィルター、および並べ替えオプションを追加するように動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="ee696-198">To try this out, you'll look at a simple query and then look at what happens to it as you add options such eager loading, filtering, and sorting.</span></span>

<span data-ttu-id="ee696-199">*コント ローラー/CourseController*、置換、`Index`メソッドを一時的に一括読み込みを停止するのには、次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee696-199">In *Controllers/CourseController*, replace the `Index` method with the following code, in order to temporarily stop eager loading:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

<span data-ttu-id="ee696-200">今すぐにブレークポイントを設定、`return`ステートメント (その行にカーソルを f9 キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="ee696-200">Now set a breakpoint on the `return` statement (F9 with the cursor on that line).</span></span> <span data-ttu-id="ee696-201">F5 キーを押して、プロジェクトをデバッグ モードで実行し、コース インデックス ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="ee696-201">Press F5 to run the project in debug mode, and select the Course Index page.</span></span> <span data-ttu-id="ee696-202">コードでは、ブレークポイントに達すると、確認、`sql`変数。</span><span class="sxs-lookup"><span data-stu-id="ee696-202">When the code reaches the breakpoint, examine the `sql` variable.</span></span> <span data-ttu-id="ee696-203">SQL Server に送信されるクエリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-203">You see the query that's sent to SQL Server.</span></span> <span data-ttu-id="ee696-204">単純な`Select`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="ee696-204">It's a simple `Select` statement.</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

<span data-ttu-id="ee696-205">拡大鏡クラス内のクエリを表示する をクリックして、**テキスト ビジュアライザー**です。</span><span class="sxs-lookup"><span data-stu-id="ee696-205">Click the magnifying class to see the query in the **Text Visualizer**.</span></span>

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

<span data-ttu-id="ee696-206">今すぐできるように、ユーザーが特定の部署のフィルター処理できます、コース インデックス ページにドロップダウン リストを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee696-206">Now you'll add a drop-down list to the Courses Index page so that users can filter for a particular department.</span></span> <span data-ttu-id="ee696-207">タイトル、コースが並べ替えられるしの一括読み込みを指定します、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee696-207">You'll sort the courses by title, and you'll specify eager loading for the `Department` navigation property.</span></span>

<span data-ttu-id="ee696-208">*CourseController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="ee696-208">In *CourseController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

<span data-ttu-id="ee696-209">ブレークポイントの復元、`return`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="ee696-209">Restore the breakpoint on the `return` statement.</span></span>

<span data-ttu-id="ee696-210">このメソッドは、ドロップダウン リストの選択した値、`SelectedDepartment`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ee696-210">The method receives the selected value of the drop-down list in the `SelectedDepartment` parameter.</span></span> <span data-ttu-id="ee696-211">何も選択すると、このパラメーターは null になります。</span><span class="sxs-lookup"><span data-stu-id="ee696-211">If nothing is selected, this parameter will be null.</span></span>

<span data-ttu-id="ee696-212">A`SelectList`ドロップ ダウン リスト ビューにすべての部門を含むコレクションが渡されました。</span><span class="sxs-lookup"><span data-stu-id="ee696-212">A `SelectList` collection containing all departments is passed to the view for the drop-down list.</span></span> <span data-ttu-id="ee696-213">渡されるパラメーター、`SelectList`コンス トラクターは、値フィールドの名前、テキスト フィールド名、および選択した項目を指定します。</span><span class="sxs-lookup"><span data-stu-id="ee696-213">The parameters passed to the `SelectList` constructor specify the value field name, the text field name, and the selected item.</span></span>

<span data-ttu-id="ee696-214">`Get`のメソッド、`Course`リポジトリ、コードを指定するフィルター式、並べ替え順、および一括読み込みの`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ee696-214">For the `Get` method of the `Course` repository, the code specifies a filter expression, a sort order, and eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="ee696-215">常に、フィルター式を返す`true`かどうか何も選択ドロップダウン リストで (つまり、`SelectedDepartment`が null です)。</span><span class="sxs-lookup"><span data-stu-id="ee696-215">The filter expression always returns `true` if nothing is selected in the drop-down list (that is, `SelectedDepartment` is null).</span></span>

<span data-ttu-id="ee696-216">*Views\Course\Index.cshtml*、開始する直前に`table`タグで、ドロップダウン リストと送信 ボタンを作成する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ee696-216">In *Views\Course\Index.cshtml*, immediately before the opening `table` tag, add the following code to create the drop-down list and a submit button:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

<span data-ttu-id="ee696-217">まだブレークポイントを設定、コース インデックス ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee696-217">With the breakpoint still set, run the Course Index page.</span></span> <span data-ttu-id="ee696-218">コードが、ブレークポイントをヒットする最初の時間に従い、ページがブラウザーに表示されないようにします。</span><span class="sxs-lookup"><span data-stu-id="ee696-218">Continue through the first times that the code hits a breakpoint, so that the page is displayed in the browser.</span></span> <span data-ttu-id="ee696-219">ドロップダウン リストから、部署を選択し、クリックして**フィルター**:</span><span class="sxs-lookup"><span data-stu-id="ee696-219">Select a department from the drop-down list and click **Filter**:</span></span>

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

<span data-ttu-id="ee696-221">今度は、最初のブレークポイントは、ドロップダウン リストの部門クエリのされます。</span><span class="sxs-lookup"><span data-stu-id="ee696-221">This time the first breakpoint will be for the departments query for the drop-down list.</span></span> <span data-ttu-id="ee696-222">スキップして、表示、`query`変数、次回、コードのブレークポイントに達した新機能を確認するために、`Course`ようこれでクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee696-222">Skip that and view the `query` variable the next time the code reaches the breakpoint in order to see what the `Course` query now looks like.</span></span> <span data-ttu-id="ee696-223">次のようなものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-223">You'll see something like the following:</span></span>

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

<span data-ttu-id="ee696-224">クエリを参照してください、`JOIN`読み込むクエリの`Department`データと共に、`Course`データ、およびが含まれている、`WHERE`句。</span><span class="sxs-lookup"><span data-stu-id="ee696-224">You can see that the query is now a `JOIN` query that loads `Department` data along with the `Course` data, and that it includes a `WHERE` clause.</span></span>

<span data-ttu-id="ee696-225">削除、`var sql = courses.ToString()`行です。</span><span class="sxs-lookup"><span data-stu-id="ee696-225">Remove the `var sql = courses.ToString()` line.</span></span>

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="ee696-226">リポジトリと作業パターンの単位</span><span class="sxs-lookup"><span data-stu-id="ee696-226">Repository and unit of work patterns</span></span>

<span data-ttu-id="ee696-227">多くの開発者は、Entity Framework で動作するコードをラップするラッパーとしてリポジトリと作業パターンの単位を実装するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="ee696-227">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="ee696-228">これらのパターンは、データ アクセス層とアプリケーションのビジネス ロジック層の間で抽象化レイヤーを作成するためのものです。</span><span class="sxs-lookup"><span data-stu-id="ee696-228">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="ee696-229">これらのパターンを実装して、変更によって、データ ストアにアプリケーションを分離できます、自動化された単体テストまたはテスト駆動開発 (TDD) に容易にすることができます。</span><span class="sxs-lookup"><span data-stu-id="ee696-229">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="ee696-230">ただし、これらのパターンを実装する追加のコードの記述は常にいくつかの理由、EF を使用するアプリケーションに最適な選択肢。</span><span class="sxs-lookup"><span data-stu-id="ee696-230">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

- <span data-ttu-id="ee696-231">EF コンテキスト クラス自体は、データ ストア固有のコードからコードを分離します。</span><span class="sxs-lookup"><span data-stu-id="ee696-231">The EF context class itself insulates your code from data-store-specific code.</span></span>
- <span data-ttu-id="ee696-232">EF コンテキスト クラスは、データベースの作業単位クラス更新 EF を使用してを実行することで動作できます。</span><span class="sxs-lookup"><span data-stu-id="ee696-232">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>
- <span data-ttu-id="ee696-233">Entity Framework 6 で導入された機能をやすくリポジトリ コードを記述することがなく TDD を実装します。</span><span class="sxs-lookup"><span data-stu-id="ee696-233">Features introduced in Entity Framework 6 make it easier to implement TDD without writing repository code.</span></span>

<span data-ttu-id="ee696-234">リポジトリと作業パターンの単位を実装する方法の詳細については、次を参照してください。[このチュートリアル シリーズの Entity Framework 5 バージョン](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="ee696-234">For more information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="ee696-235">Entity Framework 6 で TDD を実装する方法については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-235">For information about ways to implement TDD in Entity Framework 6, see the following resources:</span></span>

- [<span data-ttu-id="ee696-236">EF6 実現するしくみついて Mocking DbSets より簡単に</span><span class="sxs-lookup"><span data-stu-id="ee696-236">How EF6 Enables Mocking DbSets more easily</span></span>](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [<span data-ttu-id="ee696-237">モック フレームワークとテスト</span><span class="sxs-lookup"><span data-stu-id="ee696-237">Testing with a mocking framework</span></span>](https://msdn.microsoft.com/en-us/data/dn314429)
- [<span data-ttu-id="ee696-238">独自のテスト代替でテストします。</span><span class="sxs-lookup"><span data-stu-id="ee696-238">Testing with your own test doubles</span></span>](https://msdn.microsoft.com/en-us/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a><span data-ttu-id="ee696-239">プロキシ クラス</span><span class="sxs-lookup"><span data-stu-id="ee696-239">Proxy classes</span></span>

<span data-ttu-id="ee696-240">Entity Framework では、エンティティのインスタンス (たとえば、クエリを実行する場合) を作成するときは、エンティティのプロキシとして機能する、動的に生成される派生型のインスタンスとしてそれを多くの場合を作成します。</span><span class="sxs-lookup"><span data-stu-id="ee696-240">When the Entity Framework creates entity instances (for example, when you execute a query), it often creates them as instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="ee696-241">たとえば、次の 2 つのデバッガー イメージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-241">For example, see the following two debugger images.</span></span> <span data-ttu-id="ee696-242">最初の図で表示される、`student`変数が、予期された`Student`エンティティのインスタンスを作成した後すぐを入力します。</span><span class="sxs-lookup"><span data-stu-id="ee696-242">In the first image, you see that the `student` variable is the expected `Student` type immediately after you instantiate the entity.</span></span> <span data-ttu-id="ee696-243">2 番目のイメージで EF 学生エンティティをデータベースから読み取りに使用した後、プロキシ クラスが参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-243">In the second image, after EF has been used to read a student entity from the database, you see the proxy class.</span></span>

![プロキシ クラスの前に](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![プロキシ クラスの後](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

<span data-ttu-id="ee696-246">このプロキシ クラスは、プロパティにアクセスする場合、自動的にアクションを実行するためのフックを挿入するエンティティの一部の仮想プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ee696-246">This proxy class overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="ee696-247">このメカニズムが使用される 1 つの関数は、遅延読み込みです。</span><span class="sxs-lookup"><span data-stu-id="ee696-247">One function this mechanism is used for is lazy loading.</span></span>

<span data-ttu-id="ee696-248">ほとんどの場合このプロキシの使用について注意する必要ありませんは例外があります。</span><span class="sxs-lookup"><span data-stu-id="ee696-248">Most of the time you don't need to be aware of this use of proxies, but there are exceptions:</span></span>

- <span data-ttu-id="ee696-249">一部のシナリオでは、Entity Framework がプロキシのインスタンスを作成しないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="ee696-249">In some scenarios you might want to prevent the Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="ee696-250">たとえば、エンティティをシリアル化しているときに通常するプロキシ クラスではありません、POCO クラスです。</span><span class="sxs-lookup"><span data-stu-id="ee696-250">For example, when you're serializing entities you generally want the POCO classes, not the proxy classes.</span></span> <span data-ttu-id="ee696-251">シリアル化の問題を回避する方法の 1 つが、エンティティ オブジェクトの代わりにデータ転送オブジェクト (Dto) をシリアル化のように、 [Entity Framework と Web API を使用して](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="ee696-251">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects, as shown in the [Using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) tutorial.</span></span> <span data-ttu-id="ee696-252">別の方法は、する[プロキシの作成を無効にする](https://msdn.microsoft.com/en-US/data/jj592886.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="ee696-252">Another way is to [disable proxy creation](https://msdn.microsoft.com/en-US/data/jj592886.aspx).</span></span>
- <span data-ttu-id="ee696-253">ときにインスタンス化する、エンティティ クラスを使用して、`new`演算子、プロキシ インスタンスを取得しません。</span><span class="sxs-lookup"><span data-stu-id="ee696-253">When you instantiate an entity class using the `new` operator, you don't get a proxy instance.</span></span> <span data-ttu-id="ee696-254">これは、遅延読み込みと自動変更の追跡などの機能を取得しないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="ee696-254">This means you don't get functionality such as lazy loading and automatic change tracking.</span></span> <span data-ttu-id="ee696-255">これは、通常さてです。通常必要はありません、遅延読み込み時、データベースにない新しいエンティティを作成しているため、通常必要し、しない変更の追跡とエンティティを明示的にマークしている場合`Added`です。</span><span class="sxs-lookup"><span data-stu-id="ee696-255">This is typically okay; you generally don't need lazy loading, because you're creating a new entity that isn't in the database, and you generally don't need change tracking if you're explicitly marking the entity as `Added`.</span></span> <span data-ttu-id="ee696-256">ただし、遅延読み込みする必要は、変更の追跡が必要な場合は、インスタンスを作成できます新しいエンティティを使用してプロキシ、[作成](https://msdn.microsoft.com/en-us/library/gg679504.aspx)のメソッド、`DbSet`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ee696-256">However, if you do need lazy loading and you need change tracking, you can create new entity instances with proxies using the [Create](https://msdn.microsoft.com/en-us/library/gg679504.aspx) method of the `DbSet` class.</span></span>
- <span data-ttu-id="ee696-257">プロキシの種類から実際のエンティティ型を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ee696-257">You might want to get an actual entity type from a proxy type.</span></span> <span data-ttu-id="ee696-258">使用することができます、 [GetObjectType](https://msdn.microsoft.com/en-us/library/system.data.objects.objectcontext.getobjecttype.aspx)のメソッド、`ObjectContext`プロキシ型のインスタンスの実際のエンティティ型を取得するクラス。</span><span class="sxs-lookup"><span data-stu-id="ee696-258">You can use the [GetObjectType](https://msdn.microsoft.com/en-us/library/system.data.objects.objectcontext.getobjecttype.aspx) method of the `ObjectContext` class to get the actual entity type of a proxy type instance.</span></span>

<span data-ttu-id="ee696-259">詳細については、次を参照してください。[プロキシ扱う](https://msdn.microsoft.com/en-us/data/JJ592886.aspx)msdn です。</span><span class="sxs-lookup"><span data-stu-id="ee696-259">For more information, see [Working with Proxies](https://msdn.microsoft.com/en-us/data/JJ592886.aspx) on MSDN.</span></span>

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a><span data-ttu-id="ee696-260">自動の変更の検出</span><span class="sxs-lookup"><span data-stu-id="ee696-260">Automatic change detection</span></span>

<span data-ttu-id="ee696-261">Entity Framework では、元の値を持つエンティティの現在の値を比較することによってエンティティがどのように変更されたか (およびしたがって、更新プログラムがデータベースに送信する必要があります) を決定します。</span><span class="sxs-lookup"><span data-stu-id="ee696-261">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="ee696-262">元の値は、エンティティがクエリを実行したり、接続されているときに格納されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-262">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="ee696-263">自動の変更の検出が発生する方法の一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ee696-263">Some of the methods that cause automatic change detection are the following:</span></span>

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

<span data-ttu-id="ee696-264">多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、自動変更の検出を使用して機能をオフに一時的に大幅なパフォーマンス向上をする可能性があります、 [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx)プロパティです。</span><span class="sxs-lookup"><span data-stu-id="ee696-264">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) property.</span></span> <span data-ttu-id="ee696-265">詳細については、次を参照してください。[変更を自動的に検出する](https://msdn.microsoft.com/en-us/data/jj556205)msdn です。</span><span class="sxs-lookup"><span data-stu-id="ee696-265">For more information, see [Automatically Detecting Changes](https://msdn.microsoft.com/en-us/data/jj556205) on MSDN.</span></span>

<a id="validation"></a>
## <a name="automatic-validation"></a><span data-ttu-id="ee696-266">自動検証</span><span class="sxs-lookup"><span data-stu-id="ee696-266">Automatic validation</span></span>

<span data-ttu-id="ee696-267">呼び出すと、`SaveChanges`メソッド、既定では、Entity Framework データを検証、すべての変更されたエンティティのすべてのプロパティで、データベースを更新する前にします。</span><span class="sxs-lookup"><span data-stu-id="ee696-267">When you call the `SaveChanges` method, by default the Entity Framework validates the data in all properties of all changed entities before updating the database.</span></span> <span data-ttu-id="ee696-268">多数のエンティティを更新したし、して既に検証した、データをこの作業は必要と保存の処理を行うことが、変更は、一時的に検証を無効にすることによって時間を短縮します。</span><span class="sxs-lookup"><span data-stu-id="ee696-268">If you've updated a large number of entities and you've already validated the data, this work is unnecessary and you could make the process of saving the changes take less time by temporarily turning off validation.</span></span> <span data-ttu-id="ee696-269">使用して行うことができます、 [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx)プロパティです。</span><span class="sxs-lookup"><span data-stu-id="ee696-269">You can do that using the [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) property.</span></span> <span data-ttu-id="ee696-270">詳細については、次を参照してください。[検証](https://msdn.microsoft.com/en-us/data/gg193959)msdn です。</span><span class="sxs-lookup"><span data-stu-id="ee696-270">For more information, see [Validation](https://msdn.microsoft.com/en-us/data/gg193959) on MSDN.</span></span>

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a><span data-ttu-id="ee696-271">Entity Framework のパワー ツール</span><span class="sxs-lookup"><span data-stu-id="ee696-271">Entity Framework Power Tools</span></span>

<span data-ttu-id="ee696-272">[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)データ モデル図を作成するために使用した Visual Studio アドインがこれらのチュートリアルで表示します。</span><span class="sxs-lookup"><span data-stu-id="ee696-272">[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) is a Visual Studio add-in that was used to create the data model diagrams shown in these tutorials.</span></span> <span data-ttu-id="ee696-273">ツールには、Code First でデータベースを使用できるように、既存のデータベース内のテーブルに基づくエンティティ クラスを生成するなどその他の関数も実行できます。</span><span class="sxs-lookup"><span data-stu-id="ee696-273">The tools can also do other function such as generate entity classes based on the tables in an existing database so that you can use the database with Code First.</span></span> <span data-ttu-id="ee696-274">ツールをインストールすると、コンテキスト メニューに追加のオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-274">After you install the tools, some additional options appear in context menus.</span></span> <span data-ttu-id="ee696-275">たとえば、右クリックすると、コンテキスト クラスを**ソリューション エクスプ ローラー**図を生成するオプションを取得します。</span><span class="sxs-lookup"><span data-stu-id="ee696-275">For example, when you right-click your context class in **Solution Explorer**, you get an option to generate a diagram.</span></span> <span data-ttu-id="ee696-276">Code First を使用している場合、ダイアグラムで、データ モデルを変更することはできませんが、理解しやすいようにことを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="ee696-276">When you're using Code First you can't change the data model in the diagram, but you can move things around to make it easier to understand.</span></span>

![コンテキスト メニューで EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF ダイアグラム](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a><span data-ttu-id="ee696-279">Entity Framework のソース コード</span><span class="sxs-lookup"><span data-stu-id="ee696-279">Entity Framework source code</span></span>

<span data-ttu-id="ee696-280">Entity Framework 6 のソース コードは[GitHub](https://github.com/aspnet/EntityFramework6)です。</span><span class="sxs-lookup"><span data-stu-id="ee696-280">The source code for Entity Framework 6 is available at [GitHub](https://github.com/aspnet/EntityFramework6).</span></span> <span data-ttu-id="ee696-281">バグ、ファイルを作成でき、EF ソース コードに独自の機能強化に協力することができます。</span><span class="sxs-lookup"><span data-stu-id="ee696-281">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="ee696-282">ソース コードは開いているが、Entity Framework が Microsoft の製品としてサポートされます。</span><span class="sxs-lookup"><span data-stu-id="ee696-282">Although the source code is open, Entity Framework is fully supported as a Microsoft product.</span></span> <span data-ttu-id="ee696-283">Microsoft Entity Framework チームは、コントロール コントリビューションの受け入れを保持し、各リリースの品質を保証するすべてのコード変更をテストします。</span><span class="sxs-lookup"><span data-stu-id="ee696-283">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

<a id="summary"></a>
## <a name="summary"></a><span data-ttu-id="ee696-284">概要</span><span class="sxs-lookup"><span data-stu-id="ee696-284">Summary</span></span>

<span data-ttu-id="ee696-285">これは、この一連の ASP.NET MVC アプリケーションで Entity Framework を使用してチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="ee696-285">This completes this series of tutorials on using the Entity Framework in an ASP.NET MVC application.</span></span> <span data-ttu-id="ee696-286">Entity Framework を使用してデータを操作する方法の詳細については、次を参照してください。、 [MSDN のドキュメント ページを EF](https://msdn.microsoft.com/en-us/data/ee712907)と[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="ee696-286">For more information about how to work with data using the Entity Framework, see the [EF documentation page on MSDN](https://msdn.microsoft.com/en-us/data/ee712907) and [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="ee696-287">構築した後、web アプリケーションを展開する方法の詳細については、次を参照してください。 [ASP.NET Web 展開 - リソースのことをお勧め](../../../../whitepapers/aspnet-web-deployment-content-map.md)、MSDN ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="ee696-287">For more information about how to deploy your web application after you've built it, see [ASP.NET Web Deployment - Recommended Resources](../../../../whitepapers/aspnet-web-deployment-content-map.md) in the MSDN Library.</span></span>

<span data-ttu-id="ee696-288">については、認証および承認など、MVC に関連するその他のトピックを参照してください、 [ASP.NET MVC のリソースをお勧め](../recommended-resources-for-mvc.md)です。</span><span class="sxs-lookup"><span data-stu-id="ee696-288">For information about other topics related to MVC, such as authentication and authorization, see the [ASP.NET MVC - Recommended Resources](../recommended-resources-for-mvc.md).</span></span>

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a><span data-ttu-id="ee696-289">受信確認</span><span class="sxs-lookup"><span data-stu-id="ee696-289">Acknowledgments</span></span>

- <span data-ttu-id="ee696-290">Tom Dykstra このチュートリアルの元のバージョンを作成するには、併置 EF 5 更新プログラムを作成および EF 6 更新プログラムを記述します。</span><span class="sxs-lookup"><span data-stu-id="ee696-290">Tom Dykstra wrote the original version of this tutorial, co-authored the EF 5 update, and wrote the EF 6 update.</span></span> <span data-ttu-id="ee696-291">Tom は、Microsoft Web プラットフォームとツール コンテンツ チームのシニア プログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="ee696-291">Tom is a senior programming writer on the Microsoft Web Platform and Tools Content Team.</span></span>
- <span data-ttu-id="ee696-292">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 5 の EF と MVC 4 のチュートリアルを更新して作業の大半と併置 EF 6 更新プログラムを作成します。</span><span class="sxs-lookup"><span data-stu-id="ee696-292">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) did most of the work updating the tutorial for EF 5 and MVC 4 and co-authored the EF 6 update.</span></span> <span data-ttu-id="ee696-293">Rick は、Microsoft Azure と MVC に焦点を当てたのスキルの限られたプログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="ee696-293">Rick is a senior programming writer for Microsoft focusing on Azure and MVC.</span></span>
- <span data-ttu-id="ee696-294">[Rowan Miller](http://www.romiller.com)および Entity Framework チームの他のメンバー コード レビューを支援して EF 5 および 6 の EF のチュートリアルが更新されたことをお中に発生し、移行に関する多くの問題のデバッグを支援します。</span><span class="sxs-lookup"><span data-stu-id="ee696-294">[Rowan Miller](http://www.romiller.com) and other members of the Entity Framework team assisted with code reviews and helped debug many issues with migrations that arose while we were updating the tutorial for EF 5 and EF 6.</span></span>

<a id="vb"></a>
## <a name="vb"></a><span data-ttu-id="ee696-295">VB</span><span class="sxs-lookup"><span data-stu-id="ee696-295">VB</span></span>

<span data-ttu-id="ee696-296">EF 4.1 は、このチュートリアルは生成された最初と、おダウンロードが完了したプロジェクトの c# および VB の両方のバージョンが用意されています。</span><span class="sxs-lookup"><span data-stu-id="ee696-296">When the tutorial was originally produced for EF 4.1, we provided both C# and VB versions of the completed download project.</span></span> <span data-ttu-id="ee696-297">時間の制限とその他の優先順位によりお作業を行っていないをこのバージョンのです。</span><span class="sxs-lookup"><span data-stu-id="ee696-297">Due to time limitations and other priorities we have not done that for this version.</span></span> <span data-ttu-id="ee696-298">これらのチュートリアルを使用して、VB プロジェクトをビルドして可能な項目を共有する他のユーザー、お知らせします。</span><span class="sxs-lookup"><span data-stu-id="ee696-298">If you build a VB project using these tutorials and would be willing to share that with others, please let us know.</span></span>

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a><span data-ttu-id="ee696-299">一般的なエラーと解決方法またはそれらの回避策</span><span class="sxs-lookup"><span data-stu-id="ee696-299">Common errors, and solutions or workarounds for them</span></span>

### <a name="cannot-createshadow-copy"></a><span data-ttu-id="ee696-300">コピーを作成またはシャドウことはできません。</span><span class="sxs-lookup"><span data-stu-id="ee696-300">Cannot create/shadow copy</span></span>

<span data-ttu-id="ee696-301">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="ee696-301">Error Message:</span></span>

> <span data-ttu-id="ee696-302">コピーを作成またはシャドウできません '&lt;filename&gt;' そのファイルが既に存在する場合。</span><span class="sxs-lookup"><span data-stu-id="ee696-302">Cannot create/shadow copy '&lt;filename&gt;' when that file already exists.</span></span>


<span data-ttu-id="ee696-303">ソリューション</span><span class="sxs-lookup"><span data-stu-id="ee696-303">Solution</span></span>

<span data-ttu-id="ee696-304">数秒待ってから、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="ee696-304">Wait a few seconds and refresh the page.</span></span>

### <a name="update-database-not-recognized"></a><span data-ttu-id="ee696-305">データベースの更新認識されません。</span><span class="sxs-lookup"><span data-stu-id="ee696-305">Update-Database not recognized</span></span>

<span data-ttu-id="ee696-306">エラー メッセージ (から、 `Update-Database` PMC コマンド)。</span><span class="sxs-lookup"><span data-stu-id="ee696-306">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="ee696-307">用語 'データベースの更新' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前としては認識されません。</span><span class="sxs-lookup"><span data-stu-id="ee696-307">The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="ee696-308">名のスペルを確認するかパスが含まれている場合、パスが正しいことを確認し、もう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-308">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>


<span data-ttu-id="ee696-309">ソリューション</span><span class="sxs-lookup"><span data-stu-id="ee696-309">Solution</span></span>

<span data-ttu-id="ee696-310">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="ee696-310">Exit Visual Studio.</span></span> <span data-ttu-id="ee696-311">プロジェクトを開き直すし、もう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-311">Reopen project and try again.</span></span>

### <a name="validation-failed"></a><span data-ttu-id="ee696-312">検証に失敗しました</span><span class="sxs-lookup"><span data-stu-id="ee696-312">Validation failed</span></span>

<span data-ttu-id="ee696-313">エラー メッセージ (から、 `Update-Database` PMC コマンド)。</span><span class="sxs-lookup"><span data-stu-id="ee696-313">Error Message (from the `Update-Database` command in the PMC):</span></span>

> <span data-ttu-id="ee696-314">1 つまたは複数のエンティティの検証に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="ee696-314">Validation failed for one or more entities.</span></span> <span data-ttu-id="ee696-315">詳細については 'EntityValidationErrors' プロパティを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-315">See 'EntityValidationErrors' property for more details.</span></span>


<span data-ttu-id="ee696-316">ソリューション</span><span class="sxs-lookup"><span data-stu-id="ee696-316">Solution</span></span>

<span data-ttu-id="ee696-317">この問題の原因の 1 つは、検証エラー時に、`Seed`メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="ee696-317">One cause of this problem is validation errors when the `Seed` method runs.</span></span> <span data-ttu-id="ee696-318">参照してください[シード処理、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)のデバッグに関するヒントについては、`Seed`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ee696-318">See [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) for tips on debugging the `Seed` method.</span></span>

### <a name="http-50019-error"></a><span data-ttu-id="ee696-319">HTTP 500.19 エラー</span><span class="sxs-lookup"><span data-stu-id="ee696-319">HTTP 500.19 error</span></span>

<span data-ttu-id="ee696-320">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="ee696-320">Error Message:</span></span>

> <span data-ttu-id="ee696-321">HTTP エラー 500.19 - 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="ee696-321">HTTP Error 500.19 - Internal Server Error</span></span>  
> <span data-ttu-id="ee696-322">ページの関連する構成データが正しくないため、要求されたページにアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="ee696-322">The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>


<span data-ttu-id="ee696-323">ソリューション</span><span class="sxs-lookup"><span data-stu-id="ee696-323">Solution</span></span>

<span data-ttu-id="ee696-324">このエラーを取得する方法の 1 つは同じポート番号を使用してそれらの各ソリューションの複数のコピーです。</span><span class="sxs-lookup"><span data-stu-id="ee696-324">One way you can get this error is from having multiple copies of the solution, each of them using the same port number.</span></span> <span data-ttu-id="ee696-325">通常、この問題を解決には、Visual Studio のすべてのインスタンスを終了しで作業するプロジェクトを再起動します。</span><span class="sxs-lookup"><span data-stu-id="ee696-325">You can usually solve this problem by exiting all instances of Visual Studio, then restarting the project you're working on.</span></span> <span data-ttu-id="ee696-326">それでもうまくいかない場合は、ポート番号を変更してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-326">If that doesn't work, try changing the port number.</span></span> <span data-ttu-id="ee696-327">プロジェクト ファイルを右クリックし、[プロパティ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ee696-327">Right click on the project file and then click properties.</span></span> <span data-ttu-id="ee696-328">選択、 **Web**  タブをされているポート番号を変更、**プロジェクト Url**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="ee696-328">Select the **Web** tab and then change the port number in the **Project Url** text box.</span></span>

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="ee696-329">SQL Server インスタンスの検索エラー</span><span class="sxs-lookup"><span data-stu-id="ee696-329">Error locating SQL Server instance</span></span>

<span data-ttu-id="ee696-330">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="ee696-330">Error Message:</span></span>

> <span data-ttu-id="ee696-331">SQL Server への接続を確立中にネットワーク関連またはインスタンス固有のエラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="ee696-331">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="ee696-332">サーバーが見つからないかアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="ee696-332">The server was not found or was not accessible.</span></span> <span data-ttu-id="ee696-333">インスタンス名が正しいこと、および SQL Server がリモート接続を許可するように構成されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="ee696-333">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="ee696-334">(プロバイダー: SQL Network Interfaces、エラー: 26 - エラーの検索のサーバー/インスタンスの指定)</span><span class="sxs-lookup"><span data-stu-id="ee696-334">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>


<span data-ttu-id="ee696-335">ソリューション</span><span class="sxs-lookup"><span data-stu-id="ee696-335">Solution</span></span>

<span data-ttu-id="ee696-336">接続文字列を確認します。</span><span class="sxs-lookup"><span data-stu-id="ee696-336">Check the connection string.</span></span> <span data-ttu-id="ee696-337">データベースを手動で削除する場合は、構築文字列でデータベースの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="ee696-337">If you have manually deleted the database, change the name of the database in the construction string.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ee696-338">前へ</span><span class="sxs-lookup"><span data-stu-id="ee696-338">Previous</span></span>](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
