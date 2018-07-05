---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: MVC Web アプリケーション (10/10) 用の Entity Framework シナリオの詳細 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: e4e0a754163ad6b44ca02678fe6a0407e71ec3e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398198"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a><span data-ttu-id="83a27-103">MVC Web アプリケーション (10/10) 用の高度な Entity Framework シナリオ</span><span class="sxs-lookup"><span data-stu-id="83a27-103">Advanced Entity Framework Scenarios for an MVC Web Application (10 of 10)</span></span>
====================
<span data-ttu-id="83a27-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="83a27-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="83a27-105">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="83a27-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="83a27-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="83a27-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="83a27-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="83a27-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="83a27-108">チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。</span><span class="sxs-lookup"><span data-stu-id="83a27-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="83a27-109">を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="83a27-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="83a27-110">問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。</span><span class="sxs-lookup"><span data-stu-id="83a27-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="83a27-111">一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="83a27-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="83a27-112">前のチュートリアルでは、リポジトリと unit of work パターンを実装します。</span><span class="sxs-lookup"><span data-stu-id="83a27-112">In the previous tutorial you implemented the repository and unit of work patterns.</span></span> <span data-ttu-id="83a27-113">このチュートリアルでは、次のトピックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="83a27-113">This tutorial covers the following topics:</span></span>

- <span data-ttu-id="83a27-114">生 SQL クエリを実行しています。</span><span class="sxs-lookup"><span data-stu-id="83a27-114">Performing raw SQL queries.</span></span>
- <span data-ttu-id="83a27-115">追跡なしのクエリを実行しています。</span><span class="sxs-lookup"><span data-stu-id="83a27-115">Performing no-tracking queries.</span></span>
- <span data-ttu-id="83a27-116">クエリの調査は、データベースに送信します。</span><span class="sxs-lookup"><span data-stu-id="83a27-116">Examining queries sent to the database.</span></span>
- <span data-ttu-id="83a27-117">プロキシ クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="83a27-117">Working with proxy classes.</span></span>
- <span data-ttu-id="83a27-118">変更の自動検出を無効にします。</span><span class="sxs-lookup"><span data-stu-id="83a27-118">Disabling automatic detection of changes.</span></span>
- <span data-ttu-id="83a27-119">変更を保存するときに検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="83a27-119">Disabling validation when saving changes.</span></span>
- [<span data-ttu-id="83a27-120">エラーと回避策</span><span class="sxs-lookup"><span data-stu-id="83a27-120">Errors and Work Arounds</span></span>](#errors)

<span data-ttu-id="83a27-121">これらのトピックのほとんどは、既に作成したページを操作します。</span><span class="sxs-lookup"><span data-stu-id="83a27-121">For most of these topics, you'll work with pages that you already created.</span></span> <span data-ttu-id="83a27-122">生 SQL を使用して一括更新プログラムを実行するには、データベース内のすべてのコースの単位数を更新する新しいページを作成します。</span><span class="sxs-lookup"><span data-stu-id="83a27-122">To use raw SQL to do bulk updates you'll create a new page that updates the number of credits of all courses in the database:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<span data-ttu-id="83a27-124">追跡なしのクエリを使用して、Department Edit ページに新しい検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="83a27-124">And to use a no-tracking query you'll add new validation logic to the Department Edit page:</span></span>

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a><span data-ttu-id="83a27-126">生 SQL クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="83a27-126">Performing Raw SQL Queries</span></span>

<span data-ttu-id="83a27-127">Entity Framework Code First API をデータベースに直接 SQL コマンドを渡すことができるようにするメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="83a27-127">The Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="83a27-128">次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="83a27-128">You have the following options:</span></span>

- <span data-ttu-id="83a27-129">エンティティ型を返すクエリに対して `DbSet.SqlQuery` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="83a27-129">Use the `DbSet.SqlQuery` method for queries that return entity types.</span></span> <span data-ttu-id="83a27-130">によって予期される型で返されたオブジェクトが必要があります、`DbSet`オブジェクト、およびそれらが自動的に追跡データベース コンテキストによって追跡をオフにする場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="83a27-130">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you turn tracking off.</span></span> <span data-ttu-id="83a27-131">(に関する次のセクションを参照してください、`AsNoTracking`メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="83a27-131">(See the following section about the `AsNoTracking` method.)</span></span>
- <span data-ttu-id="83a27-132">使用して、`Database.SqlQuery`エンティティではない型を返すクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-132">Use the `Database.SqlQuery` method for queries that return types that aren't entities.</span></span> <span data-ttu-id="83a27-133">このメソッドを使用してエンティティ型を取得する場合でも、返されるデータはデータベース コンテキストによって追跡されません。</span><span class="sxs-lookup"><span data-stu-id="83a27-133">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>
- <span data-ttu-id="83a27-134">使用して、 [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非クエリ コマンド。</span><span class="sxs-lookup"><span data-stu-id="83a27-134">Use the [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) for non-query commands.</span></span>

<span data-ttu-id="83a27-135">Entity Framework を使用する利点の 1 つは、データを格納する特定のメソッドにコードを過度に接近させなくてもよい点です。</span><span class="sxs-lookup"><span data-stu-id="83a27-135">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="83a27-136">SQL クエリとコマンドが生成されるため、自分でこれらを記述する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="83a27-136">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="83a27-137">手動で作成した特定の SQL クエリを実行する必要がある場合に例外的なシナリオがあるし、これらのメソッドにより、このような例外を処理することが可能です。</span><span class="sxs-lookup"><span data-stu-id="83a27-137">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created, and these methods make it possible for you to handle such exceptions.</span></span>

<span data-ttu-id="83a27-138">Web アプリケーションで SQL コマンドを実行する場合は常に、SQL インジェクション攻撃から自身のサイトを保護する対策を講じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="83a27-138">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="83a27-139">これを行う 1 つの方法として、パラメーター化されたクエリを使用して、Web ページによって送信された文字列が SQL コマンドとして解釈できないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="83a27-139">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="83a27-140">このチュートリアルでは、ユーザー入力をクエリに統合するときに、パラメーター化されたクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="83a27-140">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

### <a name="calling-a-query-that-returns-entities"></a><span data-ttu-id="83a27-141">クエリを呼び出すには、エンティティが返されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-141">Calling a Query that Returns Entities</span></span>

<span data-ttu-id="83a27-142">`GenericRepository`追加のフィルター処理および他のメソッドは、派生クラスを作成することを必要とせずに柔軟性を並べ替えクラス。</span><span class="sxs-lookup"><span data-stu-id="83a27-142">Suppose you want the `GenericRepository` class to provide additional filtering and sorting flexibility without requiring that you create a derived class with additional methods.</span></span> <span data-ttu-id="83a27-143">これを実現する方法の 1 つは、SQL クエリを受け取るメソッドを追加することです。</span><span class="sxs-lookup"><span data-stu-id="83a27-143">One way to achieve that would be to add a method that accepts a SQL query.</span></span> <span data-ttu-id="83a27-144">あらゆる種類のフィルター処理または並べ替えなどのコント ローラーなを指定できます、`Where`結合やサブクエリに依存する句。</span><span class="sxs-lookup"><span data-stu-id="83a27-144">You could then specify any kind of filtering or sorting you want in the controller, such as a `Where` clause that depends on a joins or a subquery.</span></span> <span data-ttu-id="83a27-145">このセクションでは、このようなメソッドを実装する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="83a27-145">In this section you'll see how to implement such a method.</span></span>

<span data-ttu-id="83a27-146">作成、`GetWithRawSql`メソッドに次のコードを追加することで*GenericRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="83a27-146">Create the `GetWithRawSql` method by adding the following code to *GenericRepository.cs*:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

<span data-ttu-id="83a27-147">*CourseController.cs*から新しいメソッドを呼び出し、`Details`メソッドを次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="83a27-147">In *CourseController.cs*, call the new method from the `Details` method, as shown in the following example:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

<span data-ttu-id="83a27-148">ここで使用しても、`GetByID`メソッドを使用している、`GetWithRawSql`ことを確認するメソッド、`GetWithRawSQL`方法でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="83a27-148">In this case you could have used the `GetByID` method, but you're using the `GetWithRawSql` method to verify that the `GetWithRawSQL` method works.</span></span>

<span data-ttu-id="83a27-149">Select クエリのしくみを確認する詳細ページを実行 (選択、**コース** タブをクリックし**詳細**の 1 つのコース)。</span><span class="sxs-lookup"><span data-stu-id="83a27-149">Run the Details page to verify that the select query works (select the **Course** tab and then **Details** for one course).</span></span>

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a><span data-ttu-id="83a27-151">クエリを呼び出すには、その他の種類のオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-151">Calling a Query that Returns Other Types of Objects</span></span>

<span data-ttu-id="83a27-152">以前に、登録日ごとの学生数を示す About ページ用に、学生の統計グリッドを作成しました。</span><span class="sxs-lookup"><span data-stu-id="83a27-152">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="83a27-153">コードでは、これを*HomeController.cs* LINQ を使用します。</span><span class="sxs-lookup"><span data-stu-id="83a27-153">The code that does this in *HomeController.cs* uses LINQ:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

<span data-ttu-id="83a27-154">LINQ を使用するのではなく、SQL で直接このデータを取得するコードを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="83a27-154">Suppose you want to write the code that retrieves this data directly in SQL rather than using LINQ.</span></span> <span data-ttu-id="83a27-155">エンティティ オブジェクト以外のものを返すクエリを実行する必要があることには、ことを意味する必要があるを使用して、`Database.SqlQuery`メソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-155">To do that you need to run a query that returns something other than entity objects, which means you need to use the `Database.SqlQuery` method.</span></span>

<span data-ttu-id="83a27-156">*HomeController.cs*での LINQ ステートメントを置き換える、`About`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="83a27-156">In *HomeController.cs*, replace the LINQ statement in the `About` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

<span data-ttu-id="83a27-157">[About] ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="83a27-157">Run the About page.</span></span> <span data-ttu-id="83a27-158">以前に行ったのと同じデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-158">It displays the same data it did before.</span></span>

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a><span data-ttu-id="83a27-160">更新クエリを呼び出す</span><span class="sxs-lookup"><span data-stu-id="83a27-160">Calling an Update Query</span></span>

<span data-ttu-id="83a27-161">Contoso University の管理者は、すべてのコースの単位数を変更するなど、データベースで一括変更を実行することができるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="83a27-161">Suppose Contoso University administrators want to be able to perform bulk changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="83a27-162">大学に多くのコースがある場合は、それらすべてをエンティティとして取得し、それらを個別に変更するのは非効率的です。</span><span class="sxs-lookup"><span data-stu-id="83a27-162">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="83a27-163">このセクションでは、すべてのコースの単位数を変更する係数を指定するユーザーを許可する web ページを実装し、SQL を実行することによって、変更を行います`UPDATE`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="83a27-163">In this section you'll implement a web page that allows the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL `UPDATE` statement.</span></span> <span data-ttu-id="83a27-164">Web ページは次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="83a27-164">The web page will look like the following illustration:</span></span>

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

<span data-ttu-id="83a27-166">前のチュートリアルでの読み取りし、更新を汎用的なリポジトリを使用して`Course`内のエンティティ、`Course`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="83a27-166">In the previous tutorial you used the generic repository to read and update `Course` entities in the `Course` controller.</span></span> <span data-ttu-id="83a27-167">この一括更新操作では、汎用的なリポジトリに含まれていない新しいリポジトリ メソッドを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="83a27-167">For this bulk update operation, you need to create a new repository method that isn't in the generic repository.</span></span> <span data-ttu-id="83a27-168">そのために作成します専用`CourseRepository`から派生したクラス、`GenericRepository`クラス。</span><span class="sxs-lookup"><span data-stu-id="83a27-168">To do that, you'll create a dedicated `CourseRepository` class that derives from the `GenericRepository` class.</span></span>

<span data-ttu-id="83a27-169">*DAL*フォルダー作成*CourseRepository.cs*既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="83a27-169">In the *DAL* folder, create *CourseRepository.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

<span data-ttu-id="83a27-170">*UnitOfWork.cs*、変更、`Course`リポジトリの種類から`GenericRepository<Course>`に `CourseRepository:`</span><span class="sxs-lookup"><span data-stu-id="83a27-170">In *UnitOfWork.cs*, change the `Course` repository type from `GenericRepository<Course>` to `CourseRepository:`</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

<span data-ttu-id="83a27-171">*CourseContoller.cs*、追加、`UpdateCourseCredits`メソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-171">In *CourseContoller.cs*, add an `UpdateCourseCredits` method:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

<span data-ttu-id="83a27-172">このメソッドは、両方の使用は`HttpGet`と`HttpPost`します。</span><span class="sxs-lookup"><span data-stu-id="83a27-172">This method will be used for both `HttpGet` and `HttpPost`.</span></span> <span data-ttu-id="83a27-173">ときに、 `HttpGet` `UpdateCourseCredits`メソッドを実行、`multiplier`変数が null にして、ビューは空のテキスト ボックスと送信ボタンの場合は、前の図に示すように、表示されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-173">When the `HttpGet` `UpdateCourseCredits` method runs, the `multiplier` variable will be null and the view will display an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="83a27-174">ときに、**更新**ボタンがクリックされると、`HttpPost`メソッドを実行`multiplier`テキスト ボックスに入力した値になります。</span><span class="sxs-lookup"><span data-stu-id="83a27-174">When the **Update** button is clicked and the `HttpPost` method runs, `multiplier` will have the value entered in the text box.</span></span> <span data-ttu-id="83a27-175">コードを呼び出してリポジトリ`UpdateCourseCredits`の数を返すメソッドには、行が影響を受けるしでその値が格納されている、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="83a27-175">The code then calls the repository `UpdateCourseCredits` method, which returns the number of affected rows, and that value is stored in the `ViewBag` object.</span></span> <span data-ttu-id="83a27-176">ビューが影響を受ける行の数を受信すると、`ViewBag`オブジェクト、テキスト ボックスではなく、その数を表示し、次の図に示すように、ボタンを送信します。</span><span class="sxs-lookup"><span data-stu-id="83a27-176">When the view receives the number of affected rows in the `ViewBag` object, it displays that number instead of the text box and submit button, as shown in the following illustration:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

<span data-ttu-id="83a27-178">ビューを作成、 *Views\Course*更新 Course Credits ページのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="83a27-178">Create a view in the *Views\Course* folder for the Update Course Credits page:</span></span>

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

<span data-ttu-id="83a27-180">*Views\Course\UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="83a27-180">In *Views\Course\UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

<span data-ttu-id="83a27-181">**[Courses]\(コース\)** タブを選択してから、ブラウザーのアドレス バーで URL の末尾に "/UpdateCourseCredits" を追加して (例: `http://localhost:50205/Course/UpdateCourseCredits`)、`UpdateCourseCredits` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="83a27-181">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:50205/Course/UpdateCourseCredits`).</span></span> <span data-ttu-id="83a27-182">テキスト ボックスに数値を入力します。</span><span class="sxs-lookup"><span data-stu-id="83a27-182">Enter a number in the text box:</span></span>

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

<span data-ttu-id="83a27-184">**[更新]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="83a27-184">Click **Update**.</span></span> <span data-ttu-id="83a27-185">影響を受けた行の数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-185">You see the number of rows affected:</span></span>

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

<span data-ttu-id="83a27-187">**[リストに戻る]** をクリックして、単位数が変更されたコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="83a27-187">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

<span data-ttu-id="83a27-189">生 SQL クエリの詳細については、次を参照してください。[生 SQL クエリ](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)Entity Framework チームのブログ。</span><span class="sxs-lookup"><span data-stu-id="83a27-189">For more information about raw SQL queries, see [Raw SQL Queries](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) on the Entity Framework team blog.</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="83a27-190">追跡なしのクエリ</span><span class="sxs-lookup"><span data-stu-id="83a27-190">No-Tracking Queries</span></span>

<span data-ttu-id="83a27-191">データベース コンテキストでは、データベースの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定では、追跡データベースをメモリ内のエンティティが同期されているかどうか。</span><span class="sxs-lookup"><span data-stu-id="83a27-191">When a database context retrieves database rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="83a27-192">メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使われます。</span><span class="sxs-lookup"><span data-stu-id="83a27-192">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="83a27-193">Web アプリケーションでは、一般にコンテキスト インスタンスの存続期間は短く (要求ごとに新しいインスタンスが作成されて破棄されます)、通常、エンティティを読み取るコンテキストはエンティティが再び使われる前に破棄されるので、多くの場合、このようなキャッシュは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="83a27-193">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="83a27-194">コンテキストが使用してクエリのエンティティ オブジェクトを追跡するかどうかを指定することができます、`AsNoTracking`メソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-194">You can specify whether the context tracks entity objects for a query by using the `AsNoTracking` method.</span></span> <span data-ttu-id="83a27-195">追跡を無効にした方がよい一般的なシナリオを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="83a27-195">Typical scenarios in which you might want to do that include the following:</span></span>

- <span data-ttu-id="83a27-196">クエリでは、このような膨大な量の追跡を無効にするとパフォーマンスを向上させる著しくするデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="83a27-196">The query retrieves such a large volume of data that turning off tracking might noticeably enhance performance.</span></span>
- <span data-ttu-id="83a27-197">それを更新するには、エンティティをアタッチしたいが、異なる目的で以前に同じエンティティを取得します。</span><span class="sxs-lookup"><span data-stu-id="83a27-197">You want to attach an entity in order to update it, but you earlier retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="83a27-198">エンティティはデータベース コンテキストによって既に追跡されているため、変更するエンティティをアタッチできません。</span><span class="sxs-lookup"><span data-stu-id="83a27-198">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="83a27-199">これを防ぐ方法の 1 つが使用するには、`AsNoTracking`オプションは、以前のクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="83a27-199">One way to prevent this from happening is to use the `AsNoTracking` option with the earlier query.</span></span>

<span data-ttu-id="83a27-200">このセクションでは、2 番目のこれらのシナリオを示しています。 ビジネス ロジックを実装します。</span><span class="sxs-lookup"><span data-stu-id="83a27-200">In this section you'll implement business logic that illustrates the second of these scenarios.</span></span> <span data-ttu-id="83a27-201">具体的には、講師は 1 つ以上の部門の管理者であることを示すビジネス ルールを適用します。</span><span class="sxs-lookup"><span data-stu-id="83a27-201">Specifically, you'll enforce a business rule that says that an instructor can't be the administrator of more than one department.</span></span>

<span data-ttu-id="83a27-202">*DepartmentController.cs*から呼び出すことのできる新しいメソッドを追加、`Edit`と`Create`メソッドを 2 つの部門がありませんが、同じ管理者であること。</span><span class="sxs-lookup"><span data-stu-id="83a27-202">In *DepartmentController.cs*, add a new method that you can call from the `Edit` and `Create` methods to make sure that no two departments have the same administrator:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

<span data-ttu-id="83a27-203">内のコードを追加、`try`のブロック、 `HttpPost` `Edit`検証エラーがない場合、この新しいメソッドを呼び出すメソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-203">Add code in the `try` block of the `HttpPost` `Edit` method to call this new method if there are no validation errors.</span></span> <span data-ttu-id="83a27-204">`try`ブロックでは、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="83a27-204">The `try` block now looks like the following example:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

<span data-ttu-id="83a27-205">Department Edit ページを実行し、インストラクターは既に別の部門の管理者ユーザーに部門の管理者を変更しようとします。</span><span class="sxs-lookup"><span data-stu-id="83a27-205">Run the Department Edit page and try to change a department's administrator to an instructor who is already the administrator of a different department.</span></span> <span data-ttu-id="83a27-206">予期されるエラー メッセージを表示されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-206">You get the expected error message:</span></span>

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

<span data-ttu-id="83a27-208">これにより実行 Department Edit ページをもう一度この時刻の変更、**予算**量。</span><span class="sxs-lookup"><span data-stu-id="83a27-208">Now run the Department Edit page again and this time change the **Budget** amount.</span></span> <span data-ttu-id="83a27-209">クリックすると**保存**、エラー ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="83a27-209">When you click **Save**, you see an error page:</span></span>

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

<span data-ttu-id="83a27-211">例外のエラー メッセージは"`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`"これにより次の一連のイベントが発生しました。</span><span class="sxs-lookup"><span data-stu-id="83a27-211">The exception error message is "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" This happened because of the following sequence of events:</span></span>

- <span data-ttu-id="83a27-212">`Edit`メソッドの呼び出し、`ValidateOneAdministratorAssignmentPerInstructor`メソッドで、管理者として Kim Abercrombie のすべての部門を取得します。</span><span class="sxs-lookup"><span data-stu-id="83a27-212">The `Edit` method calls the `ValidateOneAdministratorAssignmentPerInstructor` method, which retrieves all departments that have Kim Abercrombie as their administrator.</span></span> <span data-ttu-id="83a27-213">読み取る English 部署を原因となったとします。</span><span class="sxs-lookup"><span data-stu-id="83a27-213">That causes the English department to be read.</span></span> <span data-ttu-id="83a27-214">編集中、部門があるため、エラーは報告されません。</span><span class="sxs-lookup"><span data-stu-id="83a27-214">Because that's the department being edited, no error is reported.</span></span> <span data-ttu-id="83a27-215">この読み取り操作では、結果としてただし、データベースから読み取られた English 部署のエンティティは今すぐコンテキストによって追跡されて、データベース。</span><span class="sxs-lookup"><span data-stu-id="83a27-215">As a result of this read operation, however, the English department entity that was read from the database is now being tracked by the database context.</span></span>
- <span data-ttu-id="83a27-216">`Edit`メソッドを設定しようとする、`Modified`英語版のフラグ、MVC モデル バインダーによって作成された department エンティティがコンテキストが既に、English 部署のエンティティを追跡するために失敗します。</span><span class="sxs-lookup"><span data-stu-id="83a27-216">The `Edit` method tries to set the `Modified` flag on the English department entity created by the MVC model binder, but that fails because the context is already tracking an entity for the English department.</span></span>

<span data-ttu-id="83a27-217">この問題を 1 つのソリューションでは、検証クエリによって取得されたメモリ内の部署エンティティの追跡からコンテキストを保持します。</span><span class="sxs-lookup"><span data-stu-id="83a27-217">One solution to this problem is to keep the context from tracking in-memory department entities retrieved by the validation query.</span></span> <span data-ttu-id="83a27-218">このエンティティを更新しないため、これを行うか、メモリにキャッシュされていることからメリットを得られる方法でもう一度読み取る欠点はありません。</span><span class="sxs-lookup"><span data-stu-id="83a27-218">There's no disadvantage to doing this, because you won't be updating this entity or reading it again in a way that would benefit from it being cached in memory.</span></span>

<span data-ttu-id="83a27-219">*DepartmentController.cs*の`ValidateOneAdministratorAssignmentPerInstructor`メソッドを次に示すようには、追跡、指定しません。</span><span class="sxs-lookup"><span data-stu-id="83a27-219">In *DepartmentController.cs*, in the `ValidateOneAdministratorAssignmentPerInstructor` method, specify no tracking, as shown in the following:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

<span data-ttu-id="83a27-220">編集しようとするを繰り返して、**予算**部門の量。</span><span class="sxs-lookup"><span data-stu-id="83a27-220">Repeat your attempt to edit the **Budget** amount of a department.</span></span> <span data-ttu-id="83a27-221">この時間、操作が成功すると修正された予算の値を表示、Departments Index ページに期待どおりに、サイトが返されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-221">This time the operation is successful, and the site returns as expected to the Departments Index page, showing the revised budget value.</span></span>

## <a name="examining-queries-sent-to-the-database"></a><span data-ttu-id="83a27-222">データベースに送信されるクエリの調査</span><span class="sxs-lookup"><span data-stu-id="83a27-222">Examining Queries Sent to the Database</span></span>

<span data-ttu-id="83a27-223">データベースに送信される実際の SQL クエリを確認できると役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="83a27-223">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="83a27-224">これを行うには、デバッガーでのクエリ変数を確認またはクエリの呼び出す`ToString`メソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-224">To do this, you can examine a query variable in the debugger or call the query's `ToString` method.</span></span> <span data-ttu-id="83a27-225">これを試すには、単純なクエリを見てをこのような一括読み込み、フィルター処理、および並べ替えオプションを追加するときにどう見ています。</span><span class="sxs-lookup"><span data-stu-id="83a27-225">To try this out, you'll look at a simple query and then look at what happens to it as you add options such eager loading, filtering, and sorting.</span></span>

<span data-ttu-id="83a27-226">*コント ローラー/CourseController*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="83a27-226">In *Controllers/CourseController*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

<span data-ttu-id="83a27-227">ブレークポイントを設定するようになりました*GenericRepository.cs*上、`return query.ToList();`と`return orderBy(query).ToList();`のステートメント、`Get`メソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-227">Now set a breakpoint in *GenericRepository.cs* on the `return query.ToList();` and the `return orderBy(query).ToList();` statements of the `Get` method.</span></span> <span data-ttu-id="83a27-228">プロジェクトをデバッグ モードで実行し、コースのインデックス ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="83a27-228">Run the project in debug mode and select the Course Index page.</span></span> <span data-ttu-id="83a27-229">コードでは、ブレークポイントに達すると、確認、`query`変数。</span><span class="sxs-lookup"><span data-stu-id="83a27-229">When the code reaches the breakpoint, examine the `query` variable.</span></span> <span data-ttu-id="83a27-230">SQL Server に送信されるクエリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-230">You see the query that's sent to SQL Server.</span></span> <span data-ttu-id="83a27-231">単純な`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="83a27-231">It's a simple `Select` statement:</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

<span data-ttu-id="83a27-232">クエリは、Visual Studio でのデバッグ ウィンドウに表示するには長すぎます。</span><span class="sxs-lookup"><span data-stu-id="83a27-232">Queries can be too long to display in the debugging windows in Visual Studio.</span></span> <span data-ttu-id="83a27-233">クエリ全体を表示するは、変数の値をコピーし、テキスト エディターに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="83a27-233">To see the entire query, you can copy the variable value and paste it into a text editor:</span></span>

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

<span data-ttu-id="83a27-235">今すぐできるように、ユーザーは、特定の部署のフィルター処理できます、コースのインデックス ページにドロップダウン リストを追加します。</span><span class="sxs-lookup"><span data-stu-id="83a27-235">Now you'll add a drop-down list to the Course Index page so that users can filter for a particular department.</span></span> <span data-ttu-id="83a27-236">コース タイトルで並べ替えられるし、一括読み込みを指定します、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="83a27-236">You'll sort the courses by title, and you'll specify eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="83a27-237">*CourseController.cs*、置換、`Index`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="83a27-237">In *CourseController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

<span data-ttu-id="83a27-238">メソッドのドロップダウン リストの選択した値を受け取る、`SelectedDepartment`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="83a27-238">The method receives the selected value of the drop-down list in the `SelectedDepartment` parameter.</span></span> <span data-ttu-id="83a27-239">何も選択されている場合、このパラメーターは null になります。</span><span class="sxs-lookup"><span data-stu-id="83a27-239">If nothing is selected, this parameter will be null.</span></span>

<span data-ttu-id="83a27-240">A`SelectList`ドロップダウン リストのすべての部門を含むコレクションが、ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-240">A `SelectList` collection containing all departments is passed to the view for the drop-down list.</span></span> <span data-ttu-id="83a27-241">渡されるパラメーター、`SelectList`コンス トラクターは、値フィールドの名前、テキスト フィールド名、および選択した項目を指定します。</span><span class="sxs-lookup"><span data-stu-id="83a27-241">The parameters passed to the `SelectList` constructor specify the value field name, the text field name, and the selected item.</span></span>

<span data-ttu-id="83a27-242">`Get`のメソッド、`Course`リポジトリ、コードは、フィルター式、並べ替え順序、およびの一括読み込みを指定、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="83a27-242">For the `Get` method of the `Course` repository, the code specifies a filter expression, a sort order, and eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="83a27-243">フィルター式は常に返します`true`かどうかは、ドロップダウン リストで何も選択が (つまり、`SelectedDepartment`が null)。</span><span class="sxs-lookup"><span data-stu-id="83a27-243">The filter expression always returns `true` if nothing is selected in the drop-down list (that is, `SelectedDepartment` is null).</span></span>

<span data-ttu-id="83a27-244">*Views\Course\Index.cshtml*、開始する直前に`table`タグは、ドロップダウン リストと送信ボタンを作成する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="83a27-244">In *Views\Course\Index.cshtml*, immediately before the opening `table` tag, add the following code to create the drop-down list and a submit button:</span></span>

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

<span data-ttu-id="83a27-245">まだ設定されたブレークポイントと、`GenericRepository`クラス、コースのインデックス ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="83a27-245">With the breakpoints still set in the `GenericRepository` class, run the Course Index page.</span></span> <span data-ttu-id="83a27-246">ページがブラウザーに表示されるよう、コードが、ブレークポイントをヒットする最初の 2 回の手順を続行します。</span><span class="sxs-lookup"><span data-stu-id="83a27-246">Continue through the first two times that the code hits a breakpoint, so that the page is displayed in the browser.</span></span> <span data-ttu-id="83a27-247">ドロップダウン リストから、部門を選択し、クリックして**フィルター**:</span><span class="sxs-lookup"><span data-stu-id="83a27-247">Select a department from the drop-down list and click **Filter**:</span></span>

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

<span data-ttu-id="83a27-249">この時間、最初のブレークポイントは、ドロップダウン リストの部門のクエリになります。</span><span class="sxs-lookup"><span data-stu-id="83a27-249">This time the first breakpoint will be for the departments query for the drop-down list.</span></span> <span data-ttu-id="83a27-250">スキップして、表示、`query`変数次に、コード、ブレークポイントに到達内容を表示するには、`Course`クエリのようになりますようになりました。</span><span class="sxs-lookup"><span data-stu-id="83a27-250">Skip that and view the `query` variable the next time the code reaches the breakpoint in order to see what the `Course` query now looks like.</span></span> <span data-ttu-id="83a27-251">次のようなものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-251">You'll see something like the following:</span></span>

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

<span data-ttu-id="83a27-252">クエリを参照してください、`JOIN`読み込みクエリ`Department`データと共に、`Course`データ、および含まれている、`WHERE`句。</span><span class="sxs-lookup"><span data-stu-id="83a27-252">You can see that the query is now a `JOIN` query that loads `Department` data along with the `Course` data, and that it includes a `WHERE` clause.</span></span>

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a><span data-ttu-id="83a27-253">プロキシ クラスの使用</span><span class="sxs-lookup"><span data-stu-id="83a27-253">Working with Proxy Classes</span></span>

<span data-ttu-id="83a27-254">Entity Framework では、(たとえば、クエリを実行するときに) エンティティ インスタンスを作成するときは、動的に生成されたエンティティのプロキシとして機能する派生型のインスタンスとしてそのを多くの場合、作成します。</span><span class="sxs-lookup"><span data-stu-id="83a27-254">When the Entity Framework creates entity instances (for example, when you execute a query), it often creates them as instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="83a27-255">このプロキシは、プロパティにアクセスするときにアクションを自動的に実行するためのフックを挿入するエンティティの一部の仮想プロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="83a27-255">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="83a27-256">たとえば、このメカニズムは、リレーションシップの遅延読み込みをサポートに使用されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-256">For example, this mechanism is used to support lazy loading of relationships.</span></span>

<span data-ttu-id="83a27-257">ほとんどの場合、このプロキシの使用を意識する必要はありませんが、例外があります。</span><span class="sxs-lookup"><span data-stu-id="83a27-257">Most of the time you don't need to be aware of this use of proxies, but there are exceptions:</span></span>

- <span data-ttu-id="83a27-258">一部のシナリオでは、Entity Framework がプロキシのインスタンスを作成するを防ぐためにする場合があります。</span><span class="sxs-lookup"><span data-stu-id="83a27-258">In some scenarios you might want to prevent the Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="83a27-259">たとえば、プロキシではないインスタンスをシリアル化がプロキシ インスタンスをシリアル化するよりも効率的あります。</span><span class="sxs-lookup"><span data-stu-id="83a27-259">For example, serializing non-proxy instances might be more efficient than serializing proxy instances.</span></span>
- <span data-ttu-id="83a27-260">インスタンス化すると、エンティティ クラスを使用して、`new`オペレーターは、プロキシ インスタンスを取得しません。</span><span class="sxs-lookup"><span data-stu-id="83a27-260">When you instantiate an entity class using the `new` operator, you don't get a proxy instance.</span></span> <span data-ttu-id="83a27-261">これは、遅延読み込みと自動変更の追跡などの機能を取得しないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="83a27-261">This means you don't get functionality such as lazy loading and automatic change tracking.</span></span> <span data-ttu-id="83a27-262">これは通常では;一般的に不要な lazy loading、データベースにない新しいエンティティを作成するためと、変更の追跡としてエンティティを明示的にマークしている場合は通常不要`Added`します。</span><span class="sxs-lookup"><span data-stu-id="83a27-262">This is typically okay; you generally don't need lazy loading, because you're creating a new entity that isn't in the database, and you generally don't need change tracking if you're explicitly marking the entity as `Added`.</span></span> <span data-ttu-id="83a27-263">ただし、遅延読み込みする必要は、変更の追跡が必要な場合は、インスタンスを作成できます新しいエンティティを使用してプロキシを持つ、`Create`のメソッド、`DbSet`クラス。</span><span class="sxs-lookup"><span data-stu-id="83a27-263">However, if you do need lazy loading and you need change tracking, you can create new entity instances with proxies using the `Create` method of the `DbSet` class.</span></span>
- <span data-ttu-id="83a27-264">プロキシ型から実際のエンティティ型を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="83a27-264">You might want to get an actual entity type from a proxy type.</span></span> <span data-ttu-id="83a27-265">使用することができます、`GetObjectType`のメソッド、`ObjectContext`プロキシ型のインスタンスの実際のエンティティ型を取得するクラス。</span><span class="sxs-lookup"><span data-stu-id="83a27-265">You can use the `GetObjectType` method of the `ObjectContext` class to get the actual entity type of a proxy type instance.</span></span>

<span data-ttu-id="83a27-266">詳細については、次を参照してください。 [Proxies の操作](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx)Entity Framework チームのブログ。</span><span class="sxs-lookup"><span data-stu-id="83a27-266">For more information, see [Working with Proxies](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) on the Entity Framework team blog.</span></span>

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="83a27-267">変更の自動検出を無効にします。</span><span class="sxs-lookup"><span data-stu-id="83a27-267">Disabling Automatic Detection of Changes</span></span>

<span data-ttu-id="83a27-268">Entity Framework では、エンティティの現在の値と元の値を比較して、エンティティがどのように変更されたか (およびそれによって、どの更新プログラムをデータベースに送信する必要があるか) を判断します。</span><span class="sxs-lookup"><span data-stu-id="83a27-268">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="83a27-269">エンティティのクエリを実行またはアタッチしたときに、元の値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="83a27-269">The original values are stored when the entity was queried or attached.</span></span> <span data-ttu-id="83a27-270">変更の自動検出を行うメソッドには、次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="83a27-270">Some of the methods that cause automatic change detection are the following:</span></span>

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

<span data-ttu-id="83a27-271">多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、一時的に変更の自動検出を使用して無効にすることで大幅なパフォーマンス向上を取得可能性があります、 [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="83a27-271">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) property.</span></span> <span data-ttu-id="83a27-272">詳細については、次を参照してください。[変更を自動的に検出](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="83a27-272">For more information, see [Automatically Detecting Changes](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).</span></span>

## <a name="disabling-validation-when-saving-changes"></a><span data-ttu-id="83a27-273">変更を保存するときに検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="83a27-273">Disabling Validation When Saving Changes</span></span>

<span data-ttu-id="83a27-274">呼び出すと、`SaveChanges`メソッド、既定では、Entity Framework データを検証、変更されたすべてのエンティティのすべてのプロパティで、データベースを更新する前にします。</span><span class="sxs-lookup"><span data-stu-id="83a27-274">When you call the `SaveChanges` method, by default the Entity Framework validates the data in all properties of all changed entities before updating the database.</span></span> <span data-ttu-id="83a27-275">多数のエンティティを更新したら、既に検証したデータは、この作業は必要ありませんし保存するプロセスを行うことができます、変更は一時的に検証を無効にすることで時間が。</span><span class="sxs-lookup"><span data-stu-id="83a27-275">If you've updated a large number of entities and you've already validated the data, this work is unnecessary and you could make the process of saving the changes take less time by temporarily turning off validation.</span></span> <span data-ttu-id="83a27-276">使用して行うことができます、 [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="83a27-276">You can do that using the [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) property.</span></span> <span data-ttu-id="83a27-277">詳細については、次を参照してください。[検証](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="83a27-277">For more information, see [Validation](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="83a27-278">まとめ</span><span class="sxs-lookup"><span data-stu-id="83a27-278">Summary</span></span>

<span data-ttu-id="83a27-279">これは、この一連の ASP.NET MVC アプリケーションで Entity Framework の使用に関するチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="83a27-279">This completes this series of tutorials on using the Entity Framework in an ASP.NET MVC application.</span></span> <span data-ttu-id="83a27-280">その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="83a27-280">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

<span data-ttu-id="83a27-281">これを構築した後、web アプリケーションをデプロイする方法の詳細については、次を参照してください。 [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)、MSDN ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="83a27-281">For more information about how to deploy your web application after you've built it, see [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx) in the MSDN Library.</span></span>

<span data-ttu-id="83a27-282">については、認証および承認など、MVC に関連するその他のトピックを参照してください、 [MVC 推奨リソース](../../getting-started/recommended-resources-for-mvc.md)します。</span><span class="sxs-lookup"><span data-stu-id="83a27-282">For information about other topics related to MVC, such as authentication and authorization, see the [MVC Recommended Resources](../../getting-started/recommended-resources-for-mvc.md).</span></span>

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a><span data-ttu-id="83a27-283">謝辞</span><span class="sxs-lookup"><span data-stu-id="83a27-283">Acknowledgments</span></span>

- <span data-ttu-id="83a27-284">Tom Dykstra はこのチュートリアルの元のバージョンを記述した、Microsoft Web プラットフォームとツール コンテンツ チームのシニア プログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="83a27-284">Tom Dykstra wrote the original version of this tutorial and is a senior programming writer on the Microsoft Web Platform and Tools Content Team.</span></span>
- <span data-ttu-id="83a27-285">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) このチュートリアルを共同執筆し、EF 5 と MVC 4 の更新作業のほとんどをでした。</span><span class="sxs-lookup"><span data-stu-id="83a27-285">[Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT)) co-authored this tutorial and did most of the work updating it for EF 5 and MVC 4.</span></span> <span data-ttu-id="83a27-286">Rick は、Azure と MVC に注目して Microsoft のシニア プログラミング ライターです。</span><span class="sxs-lookup"><span data-stu-id="83a27-286">Rick is a senior programming writer for Microsoft focusing on Azure and MVC.</span></span>
- <span data-ttu-id="83a27-287">[Rowan Miller](http://www.romiller.com)と Entity Framework チームの他のメンバーは、コード レビューを支援し、EF 5 のチュートリアルを更新していた私たちの中に発生した移行に関する多くの問題のデバッグに協力します。</span><span class="sxs-lookup"><span data-stu-id="83a27-287">[Rowan Miller](http://www.romiller.com) and other members of the Entity Framework team assisted with code reviews and helped debug many issues with migrations that arose while we were updating the tutorial for EF 5.</span></span>

## <a name="vb"></a><span data-ttu-id="83a27-288">VB</span><span class="sxs-lookup"><span data-stu-id="83a27-288">VB</span></span>

<span data-ttu-id="83a27-289">チュートリアルが生成された最初のダウンロードが完了したプロジェクトの c# および VB の両方のバージョンが用意されていますが。</span><span class="sxs-lookup"><span data-stu-id="83a27-289">When the tutorial was originally produced, we provided both C# and VB versions of the completed download project.</span></span> <span data-ttu-id="83a27-290">この更新により、時間の制限と実行していないを VB 向けの優先順位が、シリーズでは、任意の場所を開始するが容易に章ごとに c# ダウンロード可能なプロジェクトを提供しています</span><span class="sxs-lookup"><span data-stu-id="83a27-290">With this update we are providing a C# downloadable project for each chapter to make it easier to get started anywhere in the series, but due to time limitations and other priorities we did not do that for VB.</span></span> <span data-ttu-id="83a27-291">これらのチュートリアルを使用した VB プロジェクトをビルドして可能な項目を他のユーザーと共有、ご連絡ください。</span><span class="sxs-lookup"><span data-stu-id="83a27-291">If you build a VB project using these tutorials and would be willing to share that with others, please let us know.</span></span>

<a id="errors"></a>

## <a name="errors-and-workarounds"></a><span data-ttu-id="83a27-292">エラーと回避策</span><span class="sxs-lookup"><span data-stu-id="83a27-292">Errors and Workarounds</span></span>

### <a name="cannot-createshadow-copy"></a><span data-ttu-id="83a27-293">コピーを作成/シャドウことはできません。</span><span class="sxs-lookup"><span data-stu-id="83a27-293">Cannot create/shadow copy</span></span>

<span data-ttu-id="83a27-294">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="83a27-294">Error Message:</span></span>

<span data-ttu-id="83a27-295">*できませんを作成/シャドウ コピー 'DotNetOpenAuth.OpenId' そのファイルが既に存在する場合。*</span><span class="sxs-lookup"><span data-stu-id="83a27-295">*Cannot create/shadow copy 'DotNetOpenAuth.OpenId' when that file already exists.*</span></span>

<span data-ttu-id="83a27-296">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="83a27-296">Solution:</span></span>

<span data-ttu-id="83a27-297">数秒待ってからページを更新します。</span><span class="sxs-lookup"><span data-stu-id="83a27-297">Wait a few seconds and refresh the page.</span></span>

### <a name="update-database-not-recognized"></a><span data-ttu-id="83a27-298">データベースの更新認識されません。</span><span class="sxs-lookup"><span data-stu-id="83a27-298">Update-Database not recognized</span></span>

<span data-ttu-id="83a27-299">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="83a27-299">Error Message:</span></span>

<span data-ttu-id="83a27-300">*'データベースの更新' という用語はコマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前として認識されません。名前のスペルを確認するか、パスが正しいことを確認し、もう一度お試しパスが含まれている場合。*(から、 *`Update-Database`* PMC でコマンド)。</span><span class="sxs-lookup"><span data-stu-id="83a27-300">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*(From the *`Update-Database`* command in the PMC.)</span></span>

<span data-ttu-id="83a27-301">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="83a27-301">Solution:</span></span>

<span data-ttu-id="83a27-302">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="83a27-302">Exit Visual Studio.</span></span> <span data-ttu-id="83a27-303">プロジェクトを再度開いてもう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="83a27-303">Reopen project and try again.</span></span>

### <a name="validation-failed"></a><span data-ttu-id="83a27-304">検証に失敗しました</span><span class="sxs-lookup"><span data-stu-id="83a27-304">Validation failed</span></span>

<span data-ttu-id="83a27-305">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="83a27-305">Error Message:</span></span>

<span data-ttu-id="83a27-306">*1 つまたは複数のエンティティの検証に失敗しました。詳細については、'EntityValidationErrors' プロパティを参照してください。*</span><span class="sxs-lookup"><span data-stu-id="83a27-306">*Validation failed for one or more entities. See 'EntityValidationErrors' property for more details.*</span></span> <span data-ttu-id="83a27-307">(から、 *`Update-Database`* PMC でコマンド)。</span><span class="sxs-lookup"><span data-stu-id="83a27-307">(From the *`Update-Database`* command in the PMC.)</span></span>

<span data-ttu-id="83a27-308">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="83a27-308">Solution:</span></span>

<span data-ttu-id="83a27-309">この問題の原因の 1 つは、検証エラー時に、`Seed`メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="83a27-309">One cause of this problem is validation errors when the `Seed` method runs.</span></span> <span data-ttu-id="83a27-310">参照してください[シード処理中、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)デバッグに関するヒント、`Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="83a27-310">See [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) for tips on debugging the `Seed` method.</span></span>

### <a name="http-50019-error"></a><span data-ttu-id="83a27-311">HTTP 500.19 エラー</span><span class="sxs-lookup"><span data-stu-id="83a27-311">HTTP 500.19 error</span></span>

<span data-ttu-id="83a27-312">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="83a27-312">Error Message:</span></span>

<span data-ttu-id="83a27-313">*HTTP エラー 500.19 - 内部サーバー エラー、要求されたページは、ページの関連する構成データが無効であるために、アクセスできません。*</span><span class="sxs-lookup"><span data-stu-id="83a27-313">*HTTP Error 500.19 - Internal Server Error The requested page cannot be accessed because the related configuration data for the page is invalid.*</span></span>

<span data-ttu-id="83a27-314">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="83a27-314">Solution:</span></span>

<span data-ttu-id="83a27-315">このエラーを取得する方法の 1 つは、同じポート番号を使用してそれらの各ソリューションの複数のコピーです。</span><span class="sxs-lookup"><span data-stu-id="83a27-315">One way you can get this error is from having multiple copies of the solution, each of them using the same port number.</span></span> <span data-ttu-id="83a27-316">通常には、Visual Studio のすべてのインスタンスを終了し、プロジェクトで作業を再起動するこの問題を解決できます。</span><span class="sxs-lookup"><span data-stu-id="83a27-316">You can usually solve this problem by exiting all instances of Visual Studio, then restarting the project your working on.</span></span> <span data-ttu-id="83a27-317">うまく行かない場合は、ポート番号を変更してみてください。</span><span class="sxs-lookup"><span data-stu-id="83a27-317">If that doesn't work, try changing the port number.</span></span> <span data-ttu-id="83a27-318">プロジェクト ファイルを右クリックし、し、[プロパティ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="83a27-318">Right click on the project file and then click properties.</span></span> <span data-ttu-id="83a27-319">選択、 **Web**タブをされているポート番号を変更し、**プロジェクト Url**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="83a27-319">Select the **Web** tab and then change the port number in the **Project Url** text box.</span></span>

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="83a27-320">SQL Server インスタンスの位置を特定しているときのエラー</span><span class="sxs-lookup"><span data-stu-id="83a27-320">Error locating SQL Server instance</span></span>

<span data-ttu-id="83a27-321">エラー メッセージ:</span><span class="sxs-lookup"><span data-stu-id="83a27-321">Error Message:</span></span>

<span data-ttu-id="83a27-322">*SQL Server への接続を確立中にネットワーク関連またはインスタンス固有のエラーが発生しました。サーバーが見つからないかアクセスできません。インスタンス名が正しいことと、リモート接続を許可する SQL Server が構成されていることを確認します。(プロバイダー: SQL Network Interfaces、エラー: 26 - エラーの検索のサーバー/インスタンスの指定)*</span><span class="sxs-lookup"><span data-stu-id="83a27-322">*A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)*</span></span>

<span data-ttu-id="83a27-323">解決方法 : </span><span class="sxs-lookup"><span data-stu-id="83a27-323">Solution:</span></span>

<span data-ttu-id="83a27-324">接続文字列を確認します。</span><span class="sxs-lookup"><span data-stu-id="83a27-324">Check connection string.</span></span> <span data-ttu-id="83a27-325">データベースを手動で削除した場合は、構築文字列でデータベースの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="83a27-325">If you have manually deleted the database, change the name of the database in the construction string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="83a27-326">[前へ](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [次へ](building-the-ef5-mvc4-chapter-downloads.md)</span><span class="sxs-lookup"><span data-stu-id="83a27-326">[Previous](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[Next](building-the-ef5-mvc4-chapter-downloads.md)</span></span>
