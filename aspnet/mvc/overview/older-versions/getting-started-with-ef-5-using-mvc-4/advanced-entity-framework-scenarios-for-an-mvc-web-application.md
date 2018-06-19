---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: エンティティ フレームワークのシナリオを高度な MVC Web アプリケーション (10 10) の |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 277503b65d9b75a9d3cc05538d5327f9367f45e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876710"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a><span data-ttu-id="c1c57-103">MVC Web アプリケーション (10 10) の高度なエンティティ フレームワークのシナリオ</span><span class="sxs-lookup"><span data-stu-id="c1c57-103">Advanced Entity Framework Scenarios for an MVC Web Application (10 of 10)</span></span>
====================
<span data-ttu-id="c1c57-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c1c57-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="c1c57-105">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="c1c57-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="c1c57-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="c1c57-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c1c57-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="c1c57-108">一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="c1c57-109">解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="c1c57-110">一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="c1c57-111">一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="c1c57-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="c1c57-112">前のチュートリアルでは、リポジトリと作業パターンの単位を実装します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-112">In the previous tutorial you implemented the repository and unit of work patterns.</span></span> <span data-ttu-id="c1c57-113">このチュートリアルでは、次のトピックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-113">This tutorial covers the following topics:</span></span>

- <span data-ttu-id="c1c57-114">生の SQL クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-114">Performing raw SQL queries.</span></span>
- <span data-ttu-id="c1c57-115">No 追跡クエリを実行しています。</span><span class="sxs-lookup"><span data-stu-id="c1c57-115">Performing no-tracking queries.</span></span>
- <span data-ttu-id="c1c57-116">クエリの調査は、データベースに送信します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-116">Examining queries sent to the database.</span></span>
- <span data-ttu-id="c1c57-117">プロキシ クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-117">Working with proxy classes.</span></span>
- <span data-ttu-id="c1c57-118">変更の自動検出を無効にします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-118">Disabling automatic detection of changes.</span></span>
- <span data-ttu-id="c1c57-119">変更を保存するときに検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-119">Disabling validation when saving changes.</span></span>
- [<span data-ttu-id="c1c57-120">エラーと回避策</span><span class="sxs-lookup"><span data-stu-id="c1c57-120">Errors and Work Arounds</span></span>](#errors)

<span data-ttu-id="c1c57-121">これらのトピックのほとんどは、既に作成しているページを操作します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-121">For most of these topics, you'll work with pages that you already created.</span></span> <span data-ttu-id="c1c57-122">一括更新を実行する生の SQL を使用するには、データベース内のすべてのコースのクレジットの数を更新する新しいページを作成します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-122">To use raw SQL to do bulk updates you'll create a new page that updates the number of credits of all courses in the database:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<span data-ttu-id="c1c57-124">なしの追跡クエリを使用して、部門の編集 ページに新しい検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-124">And to use a no-tracking query you'll add new validation logic to the Department Edit page:</span></span>

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a><span data-ttu-id="c1c57-126">生の SQL クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-126">Performing Raw SQL Queries</span></span>

<span data-ttu-id="c1c57-127">Entity Framework コードの最初の API には、データベースに直接 SQL コマンドを渡すことができるようにするメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c1c57-127">The Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="c1c57-128">次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-128">You have the following options:</span></span>

- <span data-ttu-id="c1c57-129">エンティティ型を返すクエリに対して `DbSet.SqlQuery` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-129">Use the `DbSet.SqlQuery` method for queries that return entity types.</span></span> <span data-ttu-id="c1c57-130">返されたオブジェクトで想定されている型でなければなりません、`DbSet`オブジェクト、およびそれらが自動的に追跡データベースのコンテキストによって追跡を無効にする場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-130">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you turn tracking off.</span></span> <span data-ttu-id="c1c57-131">(は、次のセクションを参照してください、`AsNoTracking`メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="c1c57-131">(See the following section about the `AsNoTracking` method.)</span></span>
- <span data-ttu-id="c1c57-132">使用して、`Database.SqlQuery`をエンティティがない型を返すクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="c1c57-132">Use the `Database.SqlQuery` method for queries that return types that aren't entities.</span></span> <span data-ttu-id="c1c57-133">このメソッドを使用してエンティティ型を取得する場合でも、返されるデータはデータベース コンテキストによって追跡されません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-133">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>
- <span data-ttu-id="c1c57-134">使用して、 [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非クエリ コマンド。</span><span class="sxs-lookup"><span data-stu-id="c1c57-134">Use the [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) for non-query commands.</span></span>

<span data-ttu-id="c1c57-135">Entity Framework を使用する利点の 1 つは、データを格納する特定のメソッドにコードを過度に接近させなくてもよい点です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-135">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="c1c57-136">SQL クエリとコマンドが生成されるため、自分でこれらを記述する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-136">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="c1c57-137">例外的なシナリオは、手動で作成した特定の SQL クエリを実行する必要がある場合とこれらのメソッドのできるようにすると、このような例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-137">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created, and these methods make it possible for you to handle such exceptions.</span></span>

<span data-ttu-id="c1c57-138">Web アプリケーションで SQL コマンドを実行する場合は常に、SQL インジェクション攻撃から自身のサイトを保護する対策を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-138">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="c1c57-139">これを行う 1 つの方法として、パラメーター化されたクエリを使用して、Web ページによって送信された文字列が SQL コマンドとして解釈できないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-139">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="c1c57-140">このチュートリアルでは、ユーザー入力をクエリに統合するときに、パラメーター化されたクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-140">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

### <a name="calling-a-query-that-returns-entities"></a><span data-ttu-id="c1c57-141">クエリを呼び出すには、エンティティが返されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-141">Calling a Query that Returns Entities</span></span>

<span data-ttu-id="c1c57-142">場合を考えます、`GenericRepository`追加フィルターおよび並べ替え、その他の方法では、派生クラスを作成することを必要とせずの柔軟性を提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="c1c57-142">Suppose you want the `GenericRepository` class to provide additional filtering and sorting flexibility without requiring that you create a derived class with additional methods.</span></span> <span data-ttu-id="c1c57-143">実現するための 1 つの方法は、SQL クエリを受け取るメソッドを追加することです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-143">One way to achieve that would be to add a method that accepts a SQL query.</span></span> <span data-ttu-id="c1c57-144">でしたしするを指定する任意の種類のフィルター処理や並べ替えするコント ローラーなど、`Where`結合またはサブクエリに依存する句。</span><span class="sxs-lookup"><span data-stu-id="c1c57-144">You could then specify any kind of filtering or sorting you want in the controller, such as a `Where` clause that depends on a joins or a subquery.</span></span> <span data-ttu-id="c1c57-145">このセクションでは、このようなメソッドを実装する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-145">In this section you'll see how to implement such a method.</span></span>

<span data-ttu-id="c1c57-146">作成、`GetWithRawSql`メソッドに次のコードを追加することによって*GenericRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="c1c57-146">Create the `GetWithRawSql` method by adding the following code to *GenericRepository.cs*:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

<span data-ttu-id="c1c57-147">*CourseController.cs*から新しいメソッドを呼び出す、`Details`メソッドを次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-147">In *CourseController.cs*, call the new method from the `Details` method, as shown in the following example:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

<span data-ttu-id="c1c57-148">ここで使用しても、`GetByID`メソッドを使用している、`GetWithRawSql`ことを確認するメソッド、`GetWithRawSQL`メソッドの動作です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-148">In this case you could have used the `GetByID` method, but you're using the `GetWithRawSql` method to verify that the `GetWithRawSQL` method works.</span></span>

<span data-ttu-id="c1c57-149">選択が動作をクエリすることを確認する詳細ページを実行 (選択、**コース** タブし**詳細**コースの 1 つの)。</span><span class="sxs-lookup"><span data-stu-id="c1c57-149">Run the Details page to verify that the select query works (select the **Course** tab and then **Details** for one course).</span></span>

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a><span data-ttu-id="c1c57-151">クエリを呼び出すには、その他の種類のオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-151">Calling a Query that Returns Other Types of Objects</span></span>

<span data-ttu-id="c1c57-152">以前に、登録日ごとの学生数を示す About ページ用に、学生の統計グリッドを作成しました。</span><span class="sxs-lookup"><span data-stu-id="c1c57-152">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="c1c57-153">これを行うコード*HomeController.cs* LINQ を使用します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-153">The code that does this in *HomeController.cs* uses LINQ:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

<span data-ttu-id="c1c57-154">LINQ を使用するのではなく、SQL の直接このデータを取得するコードを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-154">Suppose you want to write the code that retrieves this data directly in SQL rather than using LINQ.</span></span> <span data-ttu-id="c1c57-155">エンティティ オブジェクト以外のものを返すクエリを実行する必要があることを意味する必要がありますを使用して、`Database.SqlQuery`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-155">To do that you need to run a query that returns something other than entity objects, which means you need to use the `Database.SqlQuery` method.</span></span>

<span data-ttu-id="c1c57-156">*HomeController.cs*での LINQ ステートメントは、置換、`About`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="c1c57-156">In *HomeController.cs*, replace the LINQ statement in the `About` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

<span data-ttu-id="c1c57-157">バージョン情報 ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-157">Run the About page.</span></span> <span data-ttu-id="c1c57-158">以前に行ったのと同じデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-158">It displays the same data it did before.</span></span>

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a><span data-ttu-id="c1c57-160">更新クエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="c1c57-160">Calling an Update Query</span></span>

<span data-ttu-id="c1c57-161">Contoso 大学管理者がすべてコースのクレジットの数の変更などのデータベースで一括変更を実行することができるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-161">Suppose Contoso University administrators want to be able to perform bulk changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="c1c57-162">大学に多くのコースがある場合は、それらすべてをエンティティとして取得し、それらを個別に変更するのは非効率的です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-162">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="c1c57-163">Web ページをすべてのコースのクレジットの数を変更する要素を指定することができますを実装するこのセクションの内容と SQL を実行することによって、変更を行うを`UPDATE`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-163">In this section you'll implement a web page that allows the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL `UPDATE` statement.</span></span> <span data-ttu-id="c1c57-164">Web ページは次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-164">The web page will look like the following illustration:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

<span data-ttu-id="c1c57-166">前のチュートリアルでの読み取りし、更新を汎用的なリポジトリを使用して`Course`内のエンティティ、`Course`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="c1c57-166">In the previous tutorial you used the generic repository to read and update `Course` entities in the `Course` controller.</span></span> <span data-ttu-id="c1c57-167">この一括更新操作では、汎用的なリポジトリにない新しいリポジトリ メソッドを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-167">For this bulk update operation, you need to create a new repository method that isn't in the generic repository.</span></span> <span data-ttu-id="c1c57-168">そのためには作成専用`CourseRepository`から派生したクラス、`GenericRepository`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-168">To do that, you'll create a dedicated `CourseRepository` class that derives from the `GenericRepository` class.</span></span>

<span data-ttu-id="c1c57-169">*DAL*フォルダー作成*CourseRepository.cs*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-169">In the *DAL* folder, create *CourseRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

<span data-ttu-id="c1c57-170">*UnitOfWork.cs*、変更、`Course`リポジトリの種類から`GenericRepository<Course>`に `CourseRepository:`</span><span class="sxs-lookup"><span data-stu-id="c1c57-170">In *UnitOfWork.cs*, change the `Course` repository type from `GenericRepository<Course>` to `CourseRepository:`</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

<span data-ttu-id="c1c57-171">*CourseContoller.cs*、追加、`UpdateCourseCredits`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c1c57-171">In *CourseContoller.cs*, add an `UpdateCourseCredits` method:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

<span data-ttu-id="c1c57-172">このメソッドは、両方の使用は`HttpGet`と`HttpPost`です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-172">This method will be used for both `HttpGet` and `HttpPost`.</span></span> <span data-ttu-id="c1c57-173">ときに、 `HttpGet` `UpdateCourseCredits`メソッドを実行する、`multiplier`変数が null でないと、表示は、その前の図に示すように空のテキスト ボックスと [送信] ボタン、表示されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-173">When the `HttpGet` `UpdateCourseCredits` method runs, the `multiplier` variable will be null and the view will display an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="c1c57-174">ときに、**更新**ボタンがクリックされると`HttpPost`メソッドを実行する`multiplier`テキスト ボックスに入力した値になります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-174">When the **Update** button is clicked and the `HttpPost` method runs, `multiplier` will have the value entered in the text box.</span></span> <span data-ttu-id="c1c57-175">コード リポジトリを呼び出して、`UpdateCourseCredits`の数を返すメソッドの影響を受ける行、およびでその値が格納されている、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c1c57-175">The code then calls the repository `UpdateCourseCredits` method, which returns the number of affected rows, and that value is stored in the `ViewBag` object.</span></span> <span data-ttu-id="c1c57-176">ビューが影響を受ける行の数を受信すると、`ViewBag`オブジェクトのテキスト ボックスの代わりにその番号が表示され、次の図に示すように、ボタンを送信します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-176">When the view receives the number of affected rows in the `ViewBag` object, it displays that number instead of the text box and submit button, as shown in the following illustration:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

<span data-ttu-id="c1c57-178">ビューを作成、 *Views\Course*更新コース クレジット ページ用のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="c1c57-178">Create a view in the *Views\Course* folder for the Update Course Credits page:</span></span>

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

<span data-ttu-id="c1c57-180">*Views\Course\UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-180">In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

<span data-ttu-id="c1c57-181">**[Courses]\(コース\)** タブを選択してから、ブラウザーのアドレス バーで URL の末尾に "/UpdateCourseCredits" を追加して (例: `http://localhost:50205/Course/UpdateCourseCredits`)、`UpdateCourseCredits` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-181">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:50205/Course/UpdateCourseCredits`).</span></span> <span data-ttu-id="c1c57-182">テキスト ボックスに数値を入力します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-182">Enter a number in the text box:</span></span>

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

<span data-ttu-id="c1c57-184">**[更新]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-184">Click **Update**.</span></span> <span data-ttu-id="c1c57-185">影響を受けた行の数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-185">You see the number of rows affected:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

<span data-ttu-id="c1c57-187">**[リストに戻る]** をクリックして、単位数が変更されたコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-187">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

<span data-ttu-id="c1c57-189">生の SQL クエリの詳細については、次を参照してください。[生の SQL クエリ](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)Entity Framework チームのブログです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-189">For more information about raw SQL queries, see [Raw SQL Queries](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) on the Entity Framework team blog.</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="c1c57-190">No 追跡クエリ</span><span class="sxs-lookup"><span data-stu-id="c1c57-190">No-Tracking Queries</span></span>

<span data-ttu-id="c1c57-191">データベース コンテキストは、データベースの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定では、追跡データベース内にメモリ内のエンティティが同期されているかどうか。</span><span class="sxs-lookup"><span data-stu-id="c1c57-191">When a database context retrieves database rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="c1c57-192">メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使われます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-192">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="c1c57-193">Web アプリケーションでは、一般にコンテキスト インスタンスの存続期間は短く (要求ごとに新しいインスタンスが作成されて破棄されます)、通常、エンティティを読み取るコンテキストはエンティティが再び使われる前に破棄されるので、多くの場合、このようなキャッシュは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-193">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="c1c57-194">コンテキストが使用して、クエリのエンティティ オブジェクトを追跡するかどうかを指定することができます、`AsNoTracking`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-194">You can specify whether the context tracks entity objects for a query by using the `AsNoTracking` method.</span></span> <span data-ttu-id="c1c57-195">追跡を無効にした方がよい一般的なシナリオを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-195">Typical scenarios in which you might want to do that include the following:</span></span>

- <span data-ttu-id="c1c57-196">クエリでは、大量の追跡を無効にするとパフォーマンスを向上させる著しくするデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-196">The query retrieves such a large volume of data that turning off tracking might noticeably enhance performance.</span></span>
- <span data-ttu-id="c1c57-197">前のさまざまな目的は同じエンティティを取得したが、更新するためにエンティティをアタッチする場合します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-197">You want to attach an entity in order to update it, but you earlier retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="c1c57-198">エンティティはデータベース コンテキストによって既に追跡されているため、変更するエンティティをアタッチできません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-198">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="c1c57-199">これを防ぐ方法の 1 つを使用して、`AsNoTracking`オプションは、以前のクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-199">One way to prevent this from happening is to use the `AsNoTracking` option with the earlier query.</span></span>

<span data-ttu-id="c1c57-200">このセクションの内容を 2 番目のこれらのシナリオを示しています。 ビジネス ロジックを実装します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-200">In this section you'll implement business logic that illustrates the second of these scenarios.</span></span> <span data-ttu-id="c1c57-201">具体的には、インストラクターは 1 つ以上の部門の管理者であることを示すビジネス ルールを適用します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-201">Specifically, you'll enforce a business rule that says that an instructor can't be the administrator of more than one department.</span></span>

<span data-ttu-id="c1c57-202">*DepartmentController.cs*から呼び出すことのできる新しいメソッドを追加、`Edit`と`Create`ない 2 つの部門に同じ管理者が持っていることを確認する方法。</span><span class="sxs-lookup"><span data-stu-id="c1c57-202">In *DepartmentController.cs*, add a new method that you can call from the `Edit` and `Create` methods to make sure that no two departments have the same administrator:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

<span data-ttu-id="c1c57-203">コードを追加、`try`のブロック、 `HttpPost` `Edit`検証エラーがない場合は、この新しいメソッドを呼び出すメソッド。</span><span class="sxs-lookup"><span data-stu-id="c1c57-203">Add code in the `try` block of the `HttpPost` `Edit` method to call this new method if there are no validation errors.</span></span> <span data-ttu-id="c1c57-204">`try`ブロックが次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-204">The `try` block now looks like the following example:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

<span data-ttu-id="c1c57-205">部門の編集 ページを実行し、インストラクターは既に別の部門の管理者ユーザーに部門の管理者を変更しようとします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-205">Run the Department Edit page and try to change a department's administrator to an instructor who is already the administrator of a different department.</span></span> <span data-ttu-id="c1c57-206">予期されるエラー メッセージを表示されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-206">You get the expected error message:</span></span>

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

<span data-ttu-id="c1c57-208">これにより実行部門の編集ページをもう一度この時刻の変更、**予算**量。</span><span class="sxs-lookup"><span data-stu-id="c1c57-208">Now run the Department Edit page again and this time change the **Budget** amount.</span></span> <span data-ttu-id="c1c57-209">クリックすると、**保存**、エラー ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c1c57-209">When you click **Save**, you see an error page:</span></span>

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

<span data-ttu-id="c1c57-211">例外のエラー メッセージは"`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`"これには、次の一連のイベントの原因が発生しました。</span><span class="sxs-lookup"><span data-stu-id="c1c57-211">The exception error message is "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" This happened because of the following sequence of events:</span></span>

- <span data-ttu-id="c1c57-212">`Edit`メソッドの呼び出し、`ValidateOneAdministratorAssignmentPerInstructor`メソッドで、管理者は、Kim Abercrombie のすべての部門を取得します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-212">The `Edit` method calls the `ValidateOneAdministratorAssignmentPerInstructor` method, which retrieves all departments that have Kim Abercrombie as their administrator.</span></span> <span data-ttu-id="c1c57-213">読み取られる英語部門を原因となったとします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-213">That causes the English department to be read.</span></span> <span data-ttu-id="c1c57-214">編集中、部門があるために、エラーは報告されません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-214">Because that's the department being edited, no error is reported.</span></span> <span data-ttu-id="c1c57-215">この読み取り操作の結果としてただし、データベースから読み取られた英語部門 エンティティは、今すぐによって追跡されているデータベース コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="c1c57-215">As a result of this read operation, however, the English department entity that was read from the database is now being tracked by the database context.</span></span>
- <span data-ttu-id="c1c57-216">`Edit`メソッド設定しようとする、`Modified`英語のフラグ、MVC モデル バインダーで、department エンティティが作成されますが、失敗すると、コンテキストが既に英語部門のエンティティを追跡するためです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-216">The `Edit` method tries to set the `Modified` flag on the English department entity created by the MVC model binder, but that fails because the context is already tracking an entity for the English department.</span></span>

<span data-ttu-id="c1c57-217">この問題を解決では、コンテキストが検証クエリによって取得されるメモリ内の部門エンティティを追跡するを防止します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-217">One solution to this problem is to keep the context from tracking in-memory department entities retrieved by the validation query.</span></span> <span data-ttu-id="c1c57-218">欠点は、このエンティティが更新されないため、これを行うまたはメモリにキャッシュされていることからメリットを得られる方法でもう一度読み取るにはありません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-218">There's no disadvantage to doing this, because you won't be updating this entity or reading it again in a way that would benefit from it being cached in memory.</span></span>

<span data-ttu-id="c1c57-219">*DepartmentController.cs*で、`ValidateOneAdministratorAssignmentPerInstructor`メソッドを次に示すようには、追跡を指定しません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-219">In *DepartmentController.cs*, in the `ValidateOneAdministratorAssignmentPerInstructor` method, specify no tracking, as shown in the following:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

<span data-ttu-id="c1c57-220">編集しようとするを繰り返して、**予算**部門の量。</span><span class="sxs-lookup"><span data-stu-id="c1c57-220">Repeat your attempt to edit the **Budget** amount of a department.</span></span> <span data-ttu-id="c1c57-221">今度は、操作が成功すると、し、修正された予算の値を表示、部門インデックス ページに期待どおりに、サイトを返します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-221">This time the operation is successful, and the site returns as expected to the Departments Index page, showing the revised budget value.</span></span>

## <a name="examining-queries-sent-to-the-database"></a><span data-ttu-id="c1c57-222">データベースに送信されるクエリを確認します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-222">Examining Queries Sent to the Database</span></span>

<span data-ttu-id="c1c57-223">データベースに送信される実際の SQL クエリを確認できると役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-223">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="c1c57-224">これを行うには、デバッガーでクエリ変数を確認またはクエリの呼び出す`ToString`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-224">To do this, you can examine a query variable in the debugger or call the query's `ToString` method.</span></span> <span data-ttu-id="c1c57-225">これを試すをするには、単純なクエリを見てし、このような一括読み込み、フィルター、および並べ替えオプションを追加するように動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-225">To try this out, you'll look at a simple query and then look at what happens to it as you add options such eager loading, filtering, and sorting.</span></span>

<span data-ttu-id="c1c57-226">*コント ローラー/CourseController*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="c1c57-226">In *Controllers/CourseController*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

<span data-ttu-id="c1c57-227">今すぐにブレークポイントを設定*GenericRepository.cs*上、`return query.ToList();`と`return orderBy(query).ToList();`のステートメント、`Get`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-227">Now set a breakpoint in *GenericRepository.cs* on the `return query.ToList();` and the `return orderBy(query).ToList();` statements of the `Get` method.</span></span> <span data-ttu-id="c1c57-228">プロジェクトをデバッグ モードで実行し、コース インデックス ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-228">Run the project in debug mode and select the Course Index page.</span></span> <span data-ttu-id="c1c57-229">コードでは、ブレークポイントに達すると、確認、`query`変数。</span><span class="sxs-lookup"><span data-stu-id="c1c57-229">When the code reaches the breakpoint, examine the `query` variable.</span></span> <span data-ttu-id="c1c57-230">SQL Server に送信されるクエリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-230">You see the query that's sent to SQL Server.</span></span> <span data-ttu-id="c1c57-231">単純な`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="c1c57-231">It's a simple `Select` statement:</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

<span data-ttu-id="c1c57-232">クエリは、Visual Studio でのデバッグ ウィンドウに表示するには長すぎます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-232">Queries can be too long to display in the debugging windows in Visual Studio.</span></span> <span data-ttu-id="c1c57-233">クエリ全体を表示するには、変数の値をコピーしてテキスト エディターに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-233">To see the entire query, you can copy the variable value and paste it into a text editor:</span></span>

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

<span data-ttu-id="c1c57-235">今すぐできるように、ユーザーが特定の部署のフィルター処理できます、コース インデックス ページにドロップダウン リストを追加します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-235">Now you'll add a drop-down list to the Course Index page so that users can filter for a particular department.</span></span> <span data-ttu-id="c1c57-236">タイトル、コースが並べ替えられるしの一括読み込みを指定します、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1c57-236">You'll sort the courses by title, and you'll specify eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="c1c57-237">*CourseController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="c1c57-237">In *CourseController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

<span data-ttu-id="c1c57-238">このメソッドは、ドロップダウン リストの選択した値、`SelectedDepartment`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="c1c57-238">The method receives the selected value of the drop-down list in the `SelectedDepartment` parameter.</span></span> <span data-ttu-id="c1c57-239">何も選択すると、このパラメーターは null になります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-239">If nothing is selected, this parameter will be null.</span></span>

<span data-ttu-id="c1c57-240">A`SelectList`ドロップ ダウン リスト ビューにすべての部門を含むコレクションが渡されました。</span><span class="sxs-lookup"><span data-stu-id="c1c57-240">A `SelectList` collection containing all departments is passed to the view for the drop-down list.</span></span> <span data-ttu-id="c1c57-241">渡されるパラメーター、`SelectList`コンス トラクターは、値フィールドの名前、テキスト フィールド名、および選択した項目を指定します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-241">The parameters passed to the `SelectList` constructor specify the value field name, the text field name, and the selected item.</span></span>

<span data-ttu-id="c1c57-242">`Get`のメソッド、`Course`リポジトリ、コードを指定するフィルター式、並べ替え順、および一括読み込みの`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1c57-242">For the `Get` method of the `Course` repository, the code specifies a filter expression, a sort order, and eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="c1c57-243">常に、フィルター式を返す`true`かどうか何も選択ドロップダウン リストで (つまり、`SelectedDepartment`が null です)。</span><span class="sxs-lookup"><span data-stu-id="c1c57-243">The filter expression always returns `true` if nothing is selected in the drop-down list (that is, `SelectedDepartment` is null).</span></span>

<span data-ttu-id="c1c57-244">*Views\Course\Index.cshtml*、開始する直前に`table`タグで、ドロップダウン リストと送信 ボタンを作成する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-244">In *Views\Course\Index.cshtml*, immediately before the opening `table` tag, add the following code to create the drop-down list and a submit button:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

<span data-ttu-id="c1c57-245">設定されているブレークポイントが、`GenericRepository`クラス、コース インデックス ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-245">With the breakpoints still set in the `GenericRepository` class, run the Course Index page.</span></span> <span data-ttu-id="c1c57-246">コードが、ブレークポイントをヒットの最初の 2 回に従い、ページがブラウザーに表示されないようにします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-246">Continue through the first two times that the code hits a breakpoint, so that the page is displayed in the browser.</span></span> <span data-ttu-id="c1c57-247">ドロップダウン リストから、部署を選択し、クリックして**フィルター**:</span><span class="sxs-lookup"><span data-stu-id="c1c57-247">Select a department from the drop-down list and click **Filter**:</span></span>

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

<span data-ttu-id="c1c57-249">今度は、最初のブレークポイントは、ドロップダウン リストの部門クエリのされます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-249">This time the first breakpoint will be for the departments query for the drop-down list.</span></span> <span data-ttu-id="c1c57-250">スキップして、表示、`query`変数、次回、コードのブレークポイントに達した新機能を確認するために、`Course`ようこれでクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-250">Skip that and view the `query` variable the next time the code reaches the breakpoint in order to see what the `Course` query now looks like.</span></span> <span data-ttu-id="c1c57-251">次のようなものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-251">You'll see something like the following:</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

<span data-ttu-id="c1c57-252">クエリを参照してください、`JOIN`読み込むクエリの`Department`データと共に、`Course`データ、およびが含まれている、`WHERE`句。</span><span class="sxs-lookup"><span data-stu-id="c1c57-252">You can see that the query is now a `JOIN` query that loads `Department` data along with the `Course` data, and that it includes a `WHERE` clause.</span></span>

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a><span data-ttu-id="c1c57-253">プロキシ クラスの扱い</span><span class="sxs-lookup"><span data-stu-id="c1c57-253">Working with Proxy Classes</span></span>

<span data-ttu-id="c1c57-254">Entity Framework では、エンティティのインスタンス (たとえば、クエリを実行する場合) を作成するときは、エンティティのプロキシとして機能する、動的に生成される派生型のインスタンスとしてそれを多くの場合を作成します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-254">When the Entity Framework creates entity instances (for example, when you execute a query), it often creates them as instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="c1c57-255">このプロキシは、プロパティにアクセスする場合、自動的にアクションを実行するためのフックを挿入するエンティティの一部の仮想プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-255">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="c1c57-256">たとえば、このメカニズムはリレーションシップの遅延読み込みをサポートするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-256">For example, this mechanism is used to support lazy loading of relationships.</span></span>

<span data-ttu-id="c1c57-257">ほとんどの場合このプロキシの使用について注意する必要ありませんは例外があります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-257">Most of the time you don't need to be aware of this use of proxies, but there are exceptions:</span></span>

- <span data-ttu-id="c1c57-258">一部のシナリオでは、Entity Framework がプロキシのインスタンスを作成しないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-258">In some scenarios you might want to prevent the Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="c1c57-259">たとえば、プロキシではないインスタンスをシリアル化する場合がありますプロキシ インスタンスをシリアル化するよりも効率的です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-259">For example, serializing non-proxy instances might be more efficient than serializing proxy instances.</span></span>
- <span data-ttu-id="c1c57-260">ときにインスタンス化する、エンティティ クラスを使用して、`new`演算子、プロキシ インスタンスを取得しません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-260">When you instantiate an entity class using the `new` operator, you don't get a proxy instance.</span></span> <span data-ttu-id="c1c57-261">これは、遅延読み込みと自動変更の追跡などの機能を取得しないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-261">This means you don't get functionality such as lazy loading and automatic change tracking.</span></span> <span data-ttu-id="c1c57-262">これは、通常さてです。通常必要はありません、遅延読み込み時、データベースにない新しいエンティティを作成しているため、通常必要し、しない変更の追跡とエンティティを明示的にマークしている場合`Added`です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-262">This is typically okay; you generally don't need lazy loading, because you're creating a new entity that isn't in the database, and you generally don't need change tracking if you're explicitly marking the entity as `Added`.</span></span> <span data-ttu-id="c1c57-263">ただし、遅延読み込みする必要は、変更の追跡が必要な場合は、インスタンスを作成できます新しいエンティティを使用してプロキシ、`Create`のメソッド、`DbSet`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-263">However, if you do need lazy loading and you need change tracking, you can create new entity instances with proxies using the `Create` method of the `DbSet` class.</span></span>
- <span data-ttu-id="c1c57-264">プロキシの種類から実際のエンティティ型を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-264">You might want to get an actual entity type from a proxy type.</span></span> <span data-ttu-id="c1c57-265">使用することができます、`GetObjectType`のメソッド、`ObjectContext`プロキシ型のインスタンスの実際のエンティティ型を取得するクラス。</span><span class="sxs-lookup"><span data-stu-id="c1c57-265">You can use the `GetObjectType` method of the `ObjectContext` class to get the actual entity type of a proxy type instance.</span></span>

<span data-ttu-id="c1c57-266">詳細については、次を参照してください。[プロキシ扱う](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx)Entity Framework チームのブログです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-266">For more information, see [Working with Proxies](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) on the Entity Framework team blog.</span></span>

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="c1c57-267">変更の自動検出を無効にします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-267">Disabling Automatic Detection of Changes</span></span>

<span data-ttu-id="c1c57-268">Entity Framework では、エンティティの現在の値と元の値を比較して、エンティティがどのように変更されたか (およびそれによって、どの更新プログラムをデータベースに送信する必要があるか) を判断します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-268">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="c1c57-269">元の値は、エンティティがクエリを実行したり、接続されているときに格納されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-269">The original values are stored when the entity was queried or attached.</span></span> <span data-ttu-id="c1c57-270">変更の自動検出を行うメソッドには、次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="c1c57-270">Some of the methods that cause automatic change detection are the following:</span></span>

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

<span data-ttu-id="c1c57-271">多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、自動変更の検出を使用して機能をオフに一時的に大幅なパフォーマンス向上をする可能性があります、 [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)プロパティです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-271">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) property.</span></span> <span data-ttu-id="c1c57-272">詳細については、次を参照してください。[変更を自動的に検出する](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-272">For more information, see [Automatically Detecting Changes](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).</span></span>

## <a name="disabling-validation-when-saving-changes"></a><span data-ttu-id="c1c57-273">変更を保存するときに検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-273">Disabling Validation When Saving Changes</span></span>

<span data-ttu-id="c1c57-274">呼び出すと、`SaveChanges`メソッド、既定では、Entity Framework データを検証、すべての変更されたエンティティのすべてのプロパティで、データベースを更新する前にします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-274">When you call the `SaveChanges` method, by default the Entity Framework validates the data in all properties of all changed entities before updating the database.</span></span> <span data-ttu-id="c1c57-275">多数のエンティティを更新したし、して既に検証した、データをこの作業は必要と保存の処理を行うことが、変更は、一時的に検証を無効にすることによって時間を短縮します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-275">If you've updated a large number of entities and you've already validated the data, this work is unnecessary and you could make the process of saving the changes take less time by temporarily turning off validation.</span></span> <span data-ttu-id="c1c57-276">使用して行うことができます、 [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)プロパティです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-276">You can do that using the [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) property.</span></span> <span data-ttu-id="c1c57-277">詳細については、次を参照してください。[検証](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-277">For more information, see [Validation](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="c1c57-278">まとめ</span><span class="sxs-lookup"><span data-stu-id="c1c57-278">Summary</span></span>

<span data-ttu-id="c1c57-279">これは、この一連の ASP.NET MVC アプリケーションで Entity Framework を使用してチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-279">This completes this series of tutorials on using the Entity Framework in an ASP.NET MVC application.</span></span> <span data-ttu-id="c1c57-280">その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-280">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="c1c57-281">構築した後、web アプリケーションを展開する方法の詳細については、次を参照してください。 [ASP.NET 展開のコンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)、MSDN ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-281">For more information about how to deploy your web application after you've built it, see [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx) in the MSDN Library.</span></span>

<span data-ttu-id="c1c57-282">については、認証および承認など、MVC に関連するその他のトピックを参照してください、[推奨の MVC リソース](../../getting-started/recommended-resources-for-mvc.md)です。</span><span class="sxs-lookup"><span data-stu-id="c1c57-282">For information about other topics related to MVC, such as authentication and authorization, see the [MVC Recommended Resources](../../getting-started/recommended-resources-for-mvc.md).</span></span>

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a><span data-ttu-id="c1c57-283">謝辞</span><span class="sxs-lookup"><span data-stu-id="c1c57-283">Acknowledgments</span></span>

- <span data-ttu-id="c1c57-284">Tom Dykstra はこのチュートリアルの元のバージョンを作成、Microsoft Web プラットフォームとツール コンテンツ チームのシニア プログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-284">Tom Dykstra wrote the original version of this tutorial and is a senior programming writer on the Microsoft Web Platform and Tools Content Team.</span></span>
- <span data-ttu-id="c1c57-285">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) このチュートリアルを共同で作成し、5 の EF と MVC 4 の更新作業の大半をします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-285">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) co-authored this tutorial and did most of the work updating it for EF 5 and MVC 4.</span></span> <span data-ttu-id="c1c57-286">Rick は、Microsoft Azure と MVC に焦点を当てたのスキルの限られたプログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-286">Rick is a senior programming writer for Microsoft focusing on Azure and MVC.</span></span>
- <span data-ttu-id="c1c57-287">[Rowan Miller](http://www.romiller.com)および Entity Framework チームの他のメンバー コード レビューを支援して EF 5 のチュートリアルが更新されたことをお中に発生し、移行に関する多くの問題のデバッグを支援します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-287">[Rowan Miller](http://www.romiller.com) and other members of the Entity Framework team assisted with code reviews and helped debug many issues with migrations that arose while we were updating the tutorial for EF 5.</span></span>

## <a name="vb"></a><span data-ttu-id="c1c57-288">VB</span><span class="sxs-lookup"><span data-stu-id="c1c57-288">VB</span></span>

<span data-ttu-id="c1c57-289">このチュートリアルがもともと生成されるとき、おダウンロードが完了したプロジェクトの c# および VB の両方のバージョンが用意されています。</span><span class="sxs-lookup"><span data-stu-id="c1c57-289">When the tutorial was originally produced, we provided both C# and VB versions of the completed download project.</span></span> <span data-ttu-id="c1c57-290">この更新プログラムでは、c# ダウンロード可能なプロジェクトの作業を開始する任意の場所が時間の制限事項と作業を行っていないを vb です。 の他の優先順位によりシリーズでは、容易にできるように各章を提供します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-290">With this update we are providing a C# downloadable project for each chapter to make it easier to get started anywhere in the series, but due to time limitations and other priorities we did not do that for VB.</span></span> <span data-ttu-id="c1c57-291">これらのチュートリアルを使用して、VB プロジェクトをビルドして可能な項目を共有する他のユーザー、お知らせします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-291">If you build a VB project using these tutorials and would be willing to share that with others, please let us know.</span></span>

<a id="errors"></a>

## <a name="errors-and-workarounds"></a><span data-ttu-id="c1c57-292">エラーと回避策</span><span class="sxs-lookup"><span data-stu-id="c1c57-292">Errors and Workarounds</span></span>

### <a name="cannot-createshadow-copy"></a><span data-ttu-id="c1c57-293">コピーを作成またはシャドウことはできません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-293">Cannot create/shadow copy</span></span>

<span data-ttu-id="c1c57-294">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="c1c57-294">Error Message:</span></span>

<span data-ttu-id="c1c57-295">*できません作成、またはシャドウ コピー 'DotNetOpenAuth.OpenId' そのファイルが既に存在する場合。*</span><span class="sxs-lookup"><span data-stu-id="c1c57-295">*Cannot create/shadow copy 'DotNetOpenAuth.OpenId' when that file already exists.*</span></span>

<span data-ttu-id="c1c57-296">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="c1c57-296">Solution:</span></span>

<span data-ttu-id="c1c57-297">数秒待ってから、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-297">Wait a few seconds and refresh the page.</span></span>

### <a name="update-database-not-recognized"></a><span data-ttu-id="c1c57-298">データベースの更新認識されません。</span><span class="sxs-lookup"><span data-stu-id="c1c57-298">Update-Database not recognized</span></span>

<span data-ttu-id="c1c57-299">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="c1c57-299">Error Message:</span></span>

<span data-ttu-id="c1c57-300">*用語 'データベースの更新' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前としては認識されません。名のスペルを確認するかパスが含まれている場合、パスが正しいことを確認し、もう一度やり直してください。*(から、 *`Update-Database`* PMC コマンドです)。</span><span class="sxs-lookup"><span data-stu-id="c1c57-300">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*(From the *`Update-Database`* command in the PMC.)</span></span>

<span data-ttu-id="c1c57-301">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="c1c57-301">Solution:</span></span>

<span data-ttu-id="c1c57-302">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-302">Exit Visual Studio.</span></span> <span data-ttu-id="c1c57-303">プロジェクトを開き直すし、もう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="c1c57-303">Reopen project and try again.</span></span>

### <a name="validation-failed"></a><span data-ttu-id="c1c57-304">検証に失敗しました</span><span class="sxs-lookup"><span data-stu-id="c1c57-304">Validation failed</span></span>

<span data-ttu-id="c1c57-305">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="c1c57-305">Error Message:</span></span>

<span data-ttu-id="c1c57-306">*1 つまたは複数のエンティティの検証に失敗しました。詳細については 'EntityValidationErrors' プロパティを参照してください。*</span><span class="sxs-lookup"><span data-stu-id="c1c57-306">*Validation failed for one or more entities. See 'EntityValidationErrors' property for more details.*</span></span> <span data-ttu-id="c1c57-307">(から、 *`Update-Database`* PMC コマンドです)。</span><span class="sxs-lookup"><span data-stu-id="c1c57-307">(From the *`Update-Database`* command in the PMC.)</span></span>

<span data-ttu-id="c1c57-308">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="c1c57-308">Solution:</span></span>

<span data-ttu-id="c1c57-309">この問題の原因の 1 つは、検証エラー時に、`Seed`メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="c1c57-309">One cause of this problem is validation errors when the `Seed` method runs.</span></span> <span data-ttu-id="c1c57-310">参照してください[シード処理、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)のデバッグに関するヒントについては、`Seed`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-310">See [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) for tips on debugging the `Seed` method.</span></span>

### <a name="http-50019-error"></a><span data-ttu-id="c1c57-311">HTTP 500.19 エラー</span><span class="sxs-lookup"><span data-stu-id="c1c57-311">HTTP 500.19 error</span></span>

<span data-ttu-id="c1c57-312">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="c1c57-312">Error Message:</span></span>

<span data-ttu-id="c1c57-313">*HTTP エラー 500.19 - 内部サーバー エラー、要求されたページはページの関連する構成データが正しくないために、アクセスできません。*</span><span class="sxs-lookup"><span data-stu-id="c1c57-313">*HTTP Error 500.19 - Internal Server Error The requested page cannot be accessed because the related configuration data for the page is invalid.*</span></span>

<span data-ttu-id="c1c57-314">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="c1c57-314">Solution:</span></span>

<span data-ttu-id="c1c57-315">このエラーを取得する方法の 1 つは同じポート番号を使用してそれらの各ソリューションの複数のコピーです。</span><span class="sxs-lookup"><span data-stu-id="c1c57-315">One way you can get this error is from having multiple copies of the solution, each of them using the same port number.</span></span> <span data-ttu-id="c1c57-316">通常、この問題を解決するには、Visual Studio のすべてのインスタンスを終了し、プロジェクトで作業中の再起動します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-316">You can usually solve this problem by exiting all instances of Visual Studio, then restarting the project your working on.</span></span> <span data-ttu-id="c1c57-317">それでもうまくいかない場合は、ポート番号を変更してください。</span><span class="sxs-lookup"><span data-stu-id="c1c57-317">If that doesn't work, try changing the port number.</span></span> <span data-ttu-id="c1c57-318">プロジェクト ファイルを右クリックし、[プロパティ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c1c57-318">Right click on the project file and then click properties.</span></span> <span data-ttu-id="c1c57-319">選択、 **Web**  タブをされているポート番号を変更、**プロジェクト Url**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="c1c57-319">Select the **Web** tab and then change the port number in the **Project Url** text box.</span></span>

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="c1c57-320">SQL Server インスタンスの位置を特定しているときのエラー</span><span class="sxs-lookup"><span data-stu-id="c1c57-320">Error locating SQL Server instance</span></span>

<span data-ttu-id="c1c57-321">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="c1c57-321">Error Message:</span></span>

<span data-ttu-id="c1c57-322">*SQL Server への接続を確立中にネットワーク関連またはインスタンス固有のエラーが発生しました。サーバーが見つからないかアクセスできません。インスタンス名が正しいことと、リモート接続を許可する SQL Server が構成されていることを確認します。(プロバイダー: SQL Network Interfaces、エラー: 26 - エラーの検索のサーバー/インスタンスの指定)*</span><span class="sxs-lookup"><span data-stu-id="c1c57-322">*A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)*</span></span>

<span data-ttu-id="c1c57-323">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="c1c57-323">Solution:</span></span>

<span data-ttu-id="c1c57-324">接続文字列を確認します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-324">Check connection string.</span></span> <span data-ttu-id="c1c57-325">データベースを手動で削除する場合は、構築文字列でデータベースの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="c1c57-325">If you have manually deleted the database, change the name of the database in the construction string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c1c57-326">[前へ](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [次へ](building-the-ef5-mvc4-chapter-downloads.md)</span><span class="sxs-lookup"><span data-stu-id="c1c57-326">[Previous](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[Next](building-the-ef5-mvc4-chapter-downloads.md)</span></span>
