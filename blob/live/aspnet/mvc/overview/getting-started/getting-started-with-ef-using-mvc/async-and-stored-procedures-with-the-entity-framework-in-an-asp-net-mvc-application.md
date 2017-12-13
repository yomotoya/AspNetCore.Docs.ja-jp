---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Async および Entity Framework、ASP.NET MVC アプリケーションでのストアド プロシージャ |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5b4904037838441942ea266ce71d735642d0a717
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="40204-103">Async および Entity Framework、ASP.NET MVC アプリケーションでのストアド プロシージャ</span><span class="sxs-lookup"><span data-stu-id="40204-103">Async and Stored Procedures with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="40204-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="40204-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="40204-105">[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="40204-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="40204-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="40204-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="40204-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="40204-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="40204-108">前のチュートリアルでは、読み取りし、同期プログラミング モデルを使用してデータを更新する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="40204-108">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="40204-109">このチュートリアルでは、非同期プログラミング モデルを実装する方法について参照してください。</span><span class="sxs-lookup"><span data-stu-id="40204-109">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="40204-110">非同期コードには、サーバーのリソースを効率的に使用可能になったためにでパフォーマンスが向上するアプリケーションが役立ちます。</span><span class="sxs-lookup"><span data-stu-id="40204-110">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="40204-111">このチュートリアルでは、insert、update、および削除操作、エンティティのストアド プロシージャを使用する方法も表示されます。</span><span class="sxs-lookup"><span data-stu-id="40204-111">In this tutorial you'll also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="40204-112">最後に、と共に展開した最初の時刻以降に実装しているデータベースの変更のすべての Azure にアプリケーションを再展開します。</span><span class="sxs-lookup"><span data-stu-id="40204-112">Finally, you'll redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="40204-113">次の図を使って作業するページの一部を表示します。</span><span class="sxs-lookup"><span data-stu-id="40204-113">The following illustrations show some of the pages that you'll work with.</span></span>

![部門ページ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![部門を作成します。](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a><span data-ttu-id="40204-116">非同期コードの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="40204-116">Why bother with asynchronous code</span></span>

<span data-ttu-id="40204-117">Web サーバーは、使用可能なスレッド数を限定を持ち、負荷が高い状況でのすべての利用可能なスレッドがありますで使用します。</span><span class="sxs-lookup"><span data-stu-id="40204-117">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="40204-118">そのような場合は、サーバーは、スレッドが解放されるまで新しい要求を処理できません。</span><span class="sxs-lookup"><span data-stu-id="40204-118">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="40204-119">同期コードはの I/O 完了を待機しているため、実際には、作業の実行されない中に多数のスレッド関連付ける可能性があります。</span><span class="sxs-lookup"><span data-stu-id="40204-119">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="40204-120">非同期コードは、プロセスが完了するには I/O の待機している場合、他の要求を処理するために使用するサーバー用に、スレッドが解放されます。</span><span class="sxs-lookup"><span data-stu-id="40204-120">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="40204-121">その結果、非同期のコードより効率的に使用し、遅延なしのより多くのトラフィックを処理するサーバーを有効にするサーバーのリソースが有効です。</span><span class="sxs-lookup"><span data-stu-id="40204-121">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="40204-122">.NET の以前のバージョンで作成および非同期コードのテストが複雑で、エラーが発生しやすく、デバッグが困難にします。</span><span class="sxs-lookup"><span data-stu-id="40204-122">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="40204-123">.NET 4.5、記述、テスト、および非同期コードのデバッグはずっと簡単に理由がない限り、非同期コードを記述する、一般にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="40204-123">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="40204-124">非同期のコードは少量のオーバーヘッドを導入がトラフィックが少ない場合は、パフォーマンスに影響はごくわずかであり、中に大量のトラフィックの場合、潜在的なパフォーマンスが向上大きくします。</span><span class="sxs-lookup"><span data-stu-id="40204-124">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="40204-125">非同期プログラミングの詳細については、次を参照してください。[呼び出しがブロックされないようにする非同期のサポートを使用する .NET 4.5 の](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)します。</span><span class="sxs-lookup"><span data-stu-id="40204-125">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-the-department-controller"></a><span data-ttu-id="40204-126">部署コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="40204-126">Create the Department controller</span></span>

<span data-ttu-id="40204-127">部門コント ローラーが以前コント ローラーで、この時間以外で実行したのと同様の選択を作成、**を使用して非同期コント ローラー**チェック ボックスを操作します。</span><span class="sxs-lookup"><span data-stu-id="40204-127">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller** actions check box.</span></span>

![部門コント ローラーのスキャフォールディング](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="40204-129">次の点には同期コードに追加された内容を表示する、`Index`メソッドを非同期になります。</span><span class="sxs-lookup"><span data-stu-id="40204-129">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="40204-130">非同期的に実行する Entity Framework データベース クエリを有効にする 4 つの変更が適用されました。</span><span class="sxs-lookup"><span data-stu-id="40204-130">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="40204-131">メソッドが付いて、`async`キーワード、メソッド本体の各部分のコールバックを生成して自動的に作成するコンパイラに指示する、`Task<ActionResult>`返されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="40204-131">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="40204-132">戻り値の型がから変更された`ActionResult`に`Task<ActionResult>`です。</span><span class="sxs-lookup"><span data-stu-id="40204-132">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="40204-133">`Task<T>`型が型の結果で進行中の作業を表す`T`です。</span><span class="sxs-lookup"><span data-stu-id="40204-133">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="40204-134">`await`キーワードは、web サービスの呼び出しに適用されました。</span><span class="sxs-lookup"><span data-stu-id="40204-134">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="40204-135">コンパイラは、このキーワードを見て、バック グラウンドでそのメソッドに分割 2 つの部分です。</span><span class="sxs-lookup"><span data-stu-id="40204-135">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="40204-136">最初の部分は、非同期的に開始される操作を終了します。</span><span class="sxs-lookup"><span data-stu-id="40204-136">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="40204-137">2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。</span><span class="sxs-lookup"><span data-stu-id="40204-137">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="40204-138">非同期バージョンの`ToList`拡張メソッドが呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="40204-138">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="40204-139">理由は、`departments.ToList`ステートメントを変更しない場合は、`departments = db.Departments`ステートメントか。</span><span class="sxs-lookup"><span data-stu-id="40204-139">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="40204-140">クエリまたはコマンドのデータベースに送信されるステートメントだけが非同期的に実行されることです。</span><span class="sxs-lookup"><span data-stu-id="40204-140">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="40204-141">`departments = db.Departments`ステートメントがクエリを設定するが、まで、クエリが実行されない、`ToList`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="40204-141">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="40204-142">したがって、のみ、`ToList`メソッドが非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="40204-142">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="40204-143">`Details`メソッドおよび`HttpGet``Edit`と`Delete`メソッド、`Find`メソッドは非同期的に実行を取得するメソッドになるように、データベースに送信されるクエリが発生しました。</span><span class="sxs-lookup"><span data-stu-id="40204-143">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="40204-144">`Create`、 `HttpPost Edit`、および`DeleteConfirmed`メソッドでは、`SaveChanges`メソッドの呼び出しを実行するコマンドの原因となるなどのステートメントではない`db.Departments.Add(department)`のみ変更するメモリ内のエンティティが発生します。</span><span class="sxs-lookup"><span data-stu-id="40204-144">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="40204-145">開いている*Views\Department\Index.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="40204-145">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="40204-146">管理者名を右に移動このコードは、部門をインデックスからタイトルを変更し、管理者の完全な名前を提供します。</span><span class="sxs-lookup"><span data-stu-id="40204-146">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="40204-147">作成、削除、詳細については、ビューの編集、およびのキャプションを変更、`InstructorID`フィールドを"Administrator"コース ビューで、部門名 フィールドを"Department"に変更したのと同様にします。</span><span class="sxs-lookup"><span data-stu-id="40204-147">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="40204-148">作成および編集には、ビューは、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="40204-148">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="40204-149">削除と詳細ビューで、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="40204-149">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="40204-150">アプリケーションを実行し、**部門**タブです。</span><span class="sxs-lookup"><span data-stu-id="40204-150">Run the application, and click the **Departments** tab.</span></span>

![部門ページ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="40204-152">すべてが他のコント ローラーのものと同じ機能が、このコント ローラーで、すべての SQL クエリは非同期に実行します。</span><span class="sxs-lookup"><span data-stu-id="40204-152">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="40204-153">Entity framework 非同期プログラミングを使用する場合の注意すべき点がいくつか:</span><span class="sxs-lookup"><span data-stu-id="40204-153">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="40204-154">非同期コードは、スレッド セーフではありません。</span><span class="sxs-lookup"><span data-stu-id="40204-154">The async code is not thread safe.</span></span> <span data-ttu-id="40204-155">つまり、つまり、しないでください、同じコンテキスト インスタンスを使用して並列に複数の操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="40204-155">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="40204-156">非同期コードのパフォーマンスの利点を活用、任意のライブラリのパッケージにあるかどうかを確認する場合、(ページングなど) を使用している、データベースに送信されるクエリを Entity Framework メソッドを呼び出す場合にも非同期を使用します。</span><span class="sxs-lookup"><span data-stu-id="40204-156">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a><span data-ttu-id="40204-157">挿入、更新、および削除のストアド プロシージャを使用します。</span><span class="sxs-lookup"><span data-stu-id="40204-157">Use stored procedures for inserting, updating, and deleting</span></span>

<span data-ttu-id="40204-158">一部の開発者と Dba は、データベースへのアクセスのストアド プロシージャの使用を優先します。</span><span class="sxs-lookup"><span data-stu-id="40204-158">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="40204-159">以前のバージョンの Entity Framework によってストアド プロシージャを使用してデータを取得できます[生 SQL クエリを実行する](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)、更新操作のためのストアド プロシージャを使用する EF を指示することはできませんが、します。</span><span class="sxs-lookup"><span data-stu-id="40204-159">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="40204-160">EF 6 ストアド プロシージャを使用して Code First の構成が容易になります。</span><span class="sxs-lookup"><span data-stu-id="40204-160">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="40204-161">*DAL\SchoolContext.cs*、強調表示されたコードを追加、`OnModelCreating`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="40204-161">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="40204-162">このコードでは、Entity Framework を指示挿入、使用するストアド プロシージャに、更新、および delete 操作を`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="40204-162">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="40204-163">パッケージの管理コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40204-163">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="40204-164">開いている*移行\&lt; タイムスタンプ&gt;\_DepartmentSP.cs*でコードを確認する、`Up`を作成するメソッドの挿入、更新、および Delete ストアド プロシージャ。</span><span class="sxs-lookup"><span data-stu-id="40204-164">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- <span data-ttu-id="40204-165">パッケージの管理コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="40204-165">In Package Manage Console, enter the following command:</span></span>

    `update-database`
- <span data-ttu-id="40204-166">アプリケーションをデバッグ モードで実行 をクリックして、**部門** タブをクリックして**新規作成**です。</span><span class="sxs-lookup"><span data-stu-id="40204-166">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
- <span data-ttu-id="40204-167">新しい部門の場合は、データを入力し、クリックして**作成**です。</span><span class="sxs-lookup"><span data-stu-id="40204-167">Enter data for a new department, and then click **Create**.</span></span>

    ![部門を作成します。](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- <span data-ttu-id="40204-169">Visual Studio でのログを確認、**出力**を新しい部門行を挿入するストアド プロシージャが使用されたことを表示するウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="40204-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

    ![部門挿入 SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="40204-171">コードは、最初に格納されている既定のプロシージャ名を作成します。</span><span class="sxs-lookup"><span data-stu-id="40204-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="40204-172">既存のデータベースを使用している場合は、データベースで既に定義されているストアド プロシージャを使用するためにストアド プロシージャ名をカスタマイズする必要があります。</span><span class="sxs-lookup"><span data-stu-id="40204-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="40204-173">実行する方法については、次を参照してください。 [Entity Framework コード最初挿入/更新/削除ストアド プロシージャ](https://msdn.microsoft.com/en-us/data/dn468673)です。</span><span class="sxs-lookup"><span data-stu-id="40204-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/en-us/data/dn468673).</span></span>

<span data-ttu-id="40204-174">ストアド プロシージャは生成された新機能をカスタマイズする場合は、移行のためスキャフォールディング コードを編集することができます`Up`ストアド プロシージャを作成するメソッド。</span><span class="sxs-lookup"><span data-stu-id="40204-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="40204-175">このようにして、変更が反映されるたびに移行が実行され、展開後に、実稼働環境で移行を自動的に実行すると、実稼働データベースに適用されます。</span><span class="sxs-lookup"><span data-stu-id="40204-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="40204-176">前の移行で作成された既存のストアド プロシージャを変更する場合は、空白の移行を生成する Add-migration コマンドを使用してし、を呼び出すコードを手動で作成、 [AlterStoredProcedure](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)メソッド.</span><span class="sxs-lookup"><span data-stu-id="40204-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="40204-177">Azure への配置します。</span><span class="sxs-lookup"><span data-stu-id="40204-177">Deploy to Azure</span></span>

<span data-ttu-id="40204-178">このセクションでは、省略可能な完了している必要があります**Azure へのアプリの展開**」の「、[移行と配置](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズ チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="40204-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="40204-179">ローカル プロジェクトでデータベースを削除することによって解決移行エラーがある場合は、このセクションをスキップします。</span><span class="sxs-lookup"><span data-stu-id="40204-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="40204-180">Visual Studio でプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**コンテキスト メニューからです。</span><span class="sxs-lookup"><span data-stu-id="40204-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="40204-181">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40204-181">Click **Publish**.</span></span>

    <span data-ttu-id="40204-182">Visual Studio は、azure アプリケーションを展開し、アプリケーションを Azure で実行されている、既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="40204-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="40204-183">アプリケーションをテストすることを確認して動作します。</span><span class="sxs-lookup"><span data-stu-id="40204-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="40204-184">初めてページを実行するデータベースにアクセスする場合は、Entity Framework 実行のすべての移行の`Up`データベースが現在のデータ モデルに最新の状態に必要なメソッドです。</span><span class="sxs-lookup"><span data-stu-id="40204-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="40204-185">これですべての前回のこのチュートリアルで追加した部門ページなど、展開した後に追加した web ページを使用できます。</span><span class="sxs-lookup"><span data-stu-id="40204-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="summary"></a><span data-ttu-id="40204-186">概要</span><span class="sxs-lookup"><span data-stu-id="40204-186">Summary</span></span>

<span data-ttu-id="40204-187">このチュートリアルでは、非同期的に実行されるコードを記述して、サーバーの効率性を向上させる方法と用のストアド プロシージャを使用する方法は、挿入、更新、および削除操作を確認しました。</span><span class="sxs-lookup"><span data-stu-id="40204-187">In this tutorial you saw how to improve server efficiency by writing code that executes asynchronously, and how to use stored procedures for insert, update, and delete operations.</span></span> <span data-ttu-id="40204-188">次のチュートリアルでは、複数のユーザーが同時に同じレコードを編集しようとデータ損失を回避する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="40204-188">In the next tutorial, you'll see how to prevent data loss when multiple users try to edit the same record at the same time.</span></span>

<span data-ttu-id="40204-189">その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="40204-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="40204-190">[前へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="40204-190">[Previous](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
