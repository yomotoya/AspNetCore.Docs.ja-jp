---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリでの EF で非同期とストアド プロシージャを使用します。'
description: このチュートリアルでは、非同期プログラミング モデルを実装して、ストアド プロシージャを使用する方法を説明する方法を参照してください。
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0896664174bc2fee65b73ecf256d994f2abacc0a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444364"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="a28a9-103">チュートリアル: ASP.NET MVC アプリでの EF で非同期とストアド プロシージャを使用します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="a28a9-104">前のチュートリアルでは、読み取りし、同期プログラミング モデルを使用してデータを更新する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="a28a9-105">このチュートリアルでは、非同期プログラミング モデルを実装する方法について参照してください。</span><span class="sxs-lookup"><span data-stu-id="a28a9-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="a28a9-106">非同期コードは、アプリケーション サーバーのリソースを効率的に使用できるのでパフォーマンスが向上に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="a28a9-107">このチュートリアルでは、insert、update、および削除の各エンティティに対して操作をストアド プロシージャを使用する方法も参照します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="a28a9-108">最後に、初めてデプロイした後を実装しているデータベースの変更はすべてと共に、Azure にアプリケーションを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="a28a9-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="a28a9-109">以下の図は、使用するページの一部を示しています。</span><span class="sxs-lookup"><span data-stu-id="a28a9-109">The following illustrations show some of the pages that you'll work with.</span></span>

![部門のページ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![学科を作成します。](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="a28a9-112">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a28a9-113">非同期コードについて説明します</span><span class="sxs-lookup"><span data-stu-id="a28a9-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="a28a9-114">部門のコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-114">Create a Department controller</span></span>
> * <span data-ttu-id="a28a9-115">ストアド プロシージャを使用します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-115">Use stored procedures</span></span>
> * <span data-ttu-id="a28a9-116">Azure に配置する</span><span class="sxs-lookup"><span data-stu-id="a28a9-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a28a9-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a28a9-117">Prerequisites</span></span>

* [<span data-ttu-id="a28a9-118">関連データの更新</span><span class="sxs-lookup"><span data-stu-id="a28a9-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="a28a9-119">非同期コードを使用する理由</span><span class="sxs-lookup"><span data-stu-id="a28a9-119">Why use asynchronous code</span></span>

<span data-ttu-id="a28a9-120">Web サーバーでは、利用できるスレッド数に限りがあります。負荷が高い状況では、利用できるスレッドが全部使われる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a28a9-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="a28a9-121">その場合、スレッドが解放されるまでサーバーは新しい要求を処理できません。</span><span class="sxs-lookup"><span data-stu-id="a28a9-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="a28a9-122">同期コードの場合、たくさんのスレッドが関連付けられていても、I/O の完了を待っているため、実際には何の作業も行っていないということがあります。</span><span class="sxs-lookup"><span data-stu-id="a28a9-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="a28a9-123">非同期コードの場合、あるプロセスが I/O の完了を待っているとき、そのスレッドは解放され、サーバーによって他の要求の処理に利用できます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="a28a9-124">結果として、非同期コードより効率的に使用すると、遅延なしの多くのトラフィックを処理するために、サーバーが有効になっているサーバーのリソースができます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="a28a9-125">.NET の以前のバージョンで非同期コードのテストの記述とは、複雑なエラーが発生しやすく、デバッグが困難にします。</span><span class="sxs-lookup"><span data-stu-id="a28a9-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="a28a9-126">.NET 4.5 での書き込み、テスト、および非同期コードのデバッグはずっと簡単にしないようにする理由がない限り非同期コードを記述する、一般にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a28a9-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="a28a9-127">非同期のコードで少量のオーバーヘッド、挿入されたが、パフォーマンスに影響はごくわずかであり、中にトラフィックが多い場合のトラフィックが少ない場合、潜在的なパフォーマンスの向上は実体です。</span><span class="sxs-lookup"><span data-stu-id="a28a9-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="a28a9-128">非同期プログラミングの詳細については、次を参照してください。[ブロッキング呼び出しを回避するために .NET 4.5 の非同期サポート](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async)します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="a28a9-129">部門のコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-129">Create Department controller</span></span>

<span data-ttu-id="a28a9-130">この時間を除く、以前のコント ローラーを実行した同じ方法を選択部門コント ローラーを作成、**非同期コント ローラー アクションを使用して、** チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="a28a9-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="a28a9-131">次の点には、同期コードに追加された内容を表示する、`Index`メソッドを非同期になります。</span><span class="sxs-lookup"><span data-stu-id="a28a9-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="a28a9-132">非同期的に実行する Entity Framework データベース クエリを有効にする 4 つの変更が適用されました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="a28a9-133">メソッドが付いて、`async`キーワード、メソッド本体の一部にコールバックを生成して、自動的に作成するコンパイラに指示する、`Task<ActionResult>`返されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a28a9-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="a28a9-134">戻り値の型がから変更された`ActionResult`に`Task<ActionResult>`します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="a28a9-135">`Task<T>`型が型の結果で進行中の作業を表す`T`します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="a28a9-136">`await`キーワードは、web サービス呼び出しに適用されました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="a28a9-137">コンパイラでは、このキーワードを見て、バック グラウンドでそのメソッドに分割 2 つの部分。</span><span class="sxs-lookup"><span data-stu-id="a28a9-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="a28a9-138">最初の部分は、非同期的に開始される操作を終了します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="a28a9-139">2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="a28a9-140">非同期バージョン、`ToList`拡張機能メソッドが呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="a28a9-141">理由は、`departments.ToList`ステートメントを変更しない場合は、`departments = db.Departments`ステートメントか?</span><span class="sxs-lookup"><span data-stu-id="a28a9-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="a28a9-142">理由は、クエリやコマンドをデータベースに送信するステートメントのみが非同期的に実行されることです。</span><span class="sxs-lookup"><span data-stu-id="a28a9-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="a28a9-143">`departments = db.Departments`ステートメントがクエリの設定が、まで、クエリが実行されない、`ToList`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="a28a9-144">そのため、のみ、`ToList`メソッドは非同期的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="a28a9-145">`Details`メソッドと`HttpGet``Edit`と`Delete`メソッド、`Find`メソッドは非同期的に実行されるメソッドがあるため、データベースに送信されるクエリが発生しました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="a28a9-146">`Create`、 `HttpPost Edit`、および`DeleteConfirmed`メソッドは、`SaveChanges`コマンドを実行するメソッドの呼び出しなどのステートメントではない`db.Departments.Add(department)`のみ変更するメモリ内のエンティティが発生します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="a28a9-147">開いている*Views\Department\Index.cshtml*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="a28a9-148">管理者名を右側に移動の部門にこのコードがインデックスからタイトルを変更し、管理者の完全名を提供します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="a28a9-149">作成、削除、詳細、およびビューの編集のキャプションを変更、`InstructorID`フィールドを「管理者」Course ビューで、部門名 フィールドを"Department"に変更して同じようにします。</span><span class="sxs-lookup"><span data-stu-id="a28a9-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="a28a9-150">Create と Edit のビューは次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="a28a9-151">Details と Delete ビューでは、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="a28a9-152">アプリケーションを実行し、**部門**タブ。</span><span class="sxs-lookup"><span data-stu-id="a28a9-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="a28a9-153">すべて他のコント ローラーの場合と同様に動作しますが、このコント ローラーのすべての SQL クエリに非同期で実行します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="a28a9-154">非同期プログラミングを Entity Framework を使用しているときの注意すべき点がいくつか:</span><span class="sxs-lookup"><span data-stu-id="a28a9-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="a28a9-155">非同期コードはスレッド セーフではありません。</span><span class="sxs-lookup"><span data-stu-id="a28a9-155">The async code is not thread safe.</span></span> <span data-ttu-id="a28a9-156">つまり、つまり、しようとしないでください、同じコンテキスト インスタンスを使用して並列で複数の操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="a28a9-157">非同期コードのパフォーマンス上の利点を最大限に活用する場合、(ページングなどのために) ライブラリ パッケージを利用しているのであれば、それがクエリをデータベースに送信させる Entity Framework メソッドを呼び出す場合、非同期を利用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a28a9-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="a28a9-158">ストアド プロシージャを使用します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-158">Use stored procedures</span></span>

<span data-ttu-id="a28a9-159">一部の開発者と Dba は、データベースへのアクセスのストアド プロシージャを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="a28a9-160">以前のバージョンの Entity Framework でストアド プロシージャを使用してデータを取得できます[生 SQL クエリを実行する](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)、更新操作のストアド プロシージャを使用して EF に指示することはできませんが、します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="a28a9-161">EF 6 Code First ストアド プロシージャを使用して構成が容易になります。</span><span class="sxs-lookup"><span data-stu-id="a28a9-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="a28a9-162">*DAL\SchoolContext.cs*、強調表示されたコードを追加、`OnModelCreating`メソッド。</span><span class="sxs-lookup"><span data-stu-id="a28a9-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="a28a9-163">このコードでは、Entity Framework を指示挿入、使用してストアド プロシージャに、更新、および delete 操作を`Department`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="a28a9-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="a28a9-164">パッケージの管理コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="a28a9-165">開いている*移行\&lt; タイムスタンプ&gt;\_DepartmentSP.cs*でコードを表示する、`Up`を作成するメソッドの挿入、更新、および Delete ストアド プロシージャ。</span><span class="sxs-lookup"><span data-stu-id="a28a9-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="a28a9-166">パッケージの管理コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="a28a9-167">アプリケーションをデバッグ モードで実行をクリックして、**部門**タブをクリックし、をクリックし、**新規作成**です。</span><span class="sxs-lookup"><span data-stu-id="a28a9-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="a28a9-168">新しい部署のデータを入力し、クリックして**作成**です。</span><span class="sxs-lookup"><span data-stu-id="a28a9-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="a28a9-169">ログを見て、Visual Studio で、**出力**ウィンドウを新しい Department 行を挿入するストアド プロシージャが使用されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![部門 Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="a28a9-171">コードは、最初に格納されている既定のプロシージャ名を作成します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="a28a9-172">既存のデータベースを使用している場合は、データベースで既に定義されているストアド プロシージャを使用するには、ストアド プロシージャの名前をカスタマイズする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a28a9-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="a28a9-173">その方法については、次を参照してください。[エンティティ フレームワーク コード最初挿入/更新/削除ストアド プロシージャ](https://msdn.microsoft.com/data/dn468673)します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="a28a9-174">ストアド プロシージャは生成されたものをカスタマイズする場合は、移行のためのスキャフォールディングされたコードを編集できます`Up`ストアド プロシージャを作成するメソッド。</span><span class="sxs-lookup"><span data-stu-id="a28a9-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="a28a9-175">これにより変更が反映されますたびに移行が実行され、デプロイ後に運用環境で移行を自動的に実行すると、実稼働データベースに適用されます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="a28a9-176">以前の移行で作成された既存のストアド プロシージャを変更する場合は、Add-migration コマンドを使用して、空の移行を生成して呼び出すコードを手動で作成、 [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx)メソッド.</span><span class="sxs-lookup"><span data-stu-id="a28a9-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="a28a9-177">Azure に配置する</span><span class="sxs-lookup"><span data-stu-id="a28a9-177">Deploy to Azure</span></span>

<span data-ttu-id="a28a9-178">このセクションでは、省略可能なが完了する必要があります**アプリを Azure にデプロイ**セクション、 [Migrations と展開](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズのチュートリアル。</span><span class="sxs-lookup"><span data-stu-id="a28a9-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="a28a9-179">ローカル プロジェクトでデータベースを削除することによって解決された移行エラーがある場合は、このセクションをスキップします。</span><span class="sxs-lookup"><span data-stu-id="a28a9-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="a28a9-180">Visual Studio でのプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択と**発行**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="a28a9-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="a28a9-181">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a28a9-181">Click **Publish**.</span></span>

    <span data-ttu-id="a28a9-182">Visual Studio を Azure にアプリケーションを配置して、アプリケーションを Azure で実行されている、既定のブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="a28a9-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="a28a9-183">アプリケーションをテストして確認することが動作します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="a28a9-184">初めてページを実行するデータベースにアクセスする、Entity Framework で実行されるすべての移行`Up`データベースの現在のデータ モデルで最新の状態に必要なメソッドです。</span><span class="sxs-lookup"><span data-stu-id="a28a9-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="a28a9-185">このチュートリアルで追加した部門ページなど、デプロイした最終時刻以降に追加する web ページのすべてを使用することができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="a28a9-186">コードを取得する</span><span class="sxs-lookup"><span data-stu-id="a28a9-186">Get the code</span></span>

[<span data-ttu-id="a28a9-187">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a28a9-187">Download the Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="a28a9-188">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a28a9-188">Additional resources</span></span>

<span data-ttu-id="a28a9-189">その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="a28a9-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a28a9-190">次の手順</span><span class="sxs-lookup"><span data-stu-id="a28a9-190">Next steps</span></span>

<span data-ttu-id="a28a9-191">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="a28a9-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a28a9-192">非同期コードについて説明しました</span><span class="sxs-lookup"><span data-stu-id="a28a9-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="a28a9-193">部門のコント ローラーの作成</span><span class="sxs-lookup"><span data-stu-id="a28a9-193">Created a Department controller</span></span>
> * <span data-ttu-id="a28a9-194">ストアド プロシージャの使用</span><span class="sxs-lookup"><span data-stu-id="a28a9-194">Used stored procedures</span></span>
> * <span data-ttu-id="a28a9-195">Azure にデプロイ</span><span class="sxs-lookup"><span data-stu-id="a28a9-195">Deployed to Azure</span></span>

<span data-ttu-id="a28a9-196">複数のユーザーが同時に同じエンティティを更新するときの競合を処理する方法については、次の記事に進んでください。</span><span class="sxs-lookup"><span data-stu-id="a28a9-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="a28a9-197">同時実行の処理</span><span class="sxs-lookup"><span data-stu-id="a28a9-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
