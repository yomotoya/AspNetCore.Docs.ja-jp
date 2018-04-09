---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'パート 3: 作成、管理コント ローラー |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="8ec80-102">パート 3: 作成、管理コント ローラー</span><span class="sxs-lookup"><span data-stu-id="8ec80-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="8ec80-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8ec80-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8ec80-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="8ec80-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="8ec80-105">管理コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-105">Add an Admin Controller</span></span>

<span data-ttu-id="8ec80-106">このセクションでは、CRUD をサポートする Web API コント ローラーが追加されます (作成、読み取り、更新、および削除) の製品に操作します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="8ec80-107">コント ローラーでは、Entity Framework をデータベース層との通信に使用します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="8ec80-108">管理者のみがこのコント ローラーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8ec80-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="8ec80-109">顧客は、製品を別のコント ローラーからアクセスします。</span><span class="sxs-lookup"><span data-stu-id="8ec80-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="8ec80-110">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="8ec80-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="8ec80-111">選択**追加**し**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="8ec80-112">**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー`AdminController`です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="8ec80-113">**テンプレート** &quot;Entity Framework を使用して、読み取り/書き込みアクションがある API コント ローラー&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="8ec80-114">**モデル クラス**、"Product (ProductStore.Models)"を選択します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="8ec80-115">**データ コンテキスト**"&lt;新しいデータ コンテキスト&gt;"です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="8ec80-116">場合、**モデル クラス**ドロップダウンはすべてのモデル クラスを表示しないため、プロジェクトをコンパイルするかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="8ec80-117">Entity Framework は、ため、コンパイルされたアセンブリが必要があります、リフレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="8ec80-118">選択すると"&lt;新しいデータ コンテキスト&gt;"が開き、**新しいデータ コンテキスト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="8ec80-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="8ec80-119">データ コンテキストの名前を付けます`ProductStore.Models.OrdersContext`です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="8ec80-120">をクリックして**OK**を消去する、**新しいデータ コンテキスト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="8ec80-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="8ec80-121">**コント ローラーの追加**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="8ec80-122">プロジェクトに追加された新機能が次に示します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="8ec80-123">という名前のクラス`OrdersContext`から派生した**DbContext**です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="8ec80-124">このクラスは、POCO モデルとデータベース間接着剤を提供します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="8ec80-125">という名前の Web API コント ローラー`AdminController`です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="8ec80-126">このコント ローラーでの CRUD 操作をサポートしている`Product`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="8ec80-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="8ec80-127">使用して、 `OrdersContext` Entity Framework と通信するクラス。</span><span class="sxs-lookup"><span data-stu-id="8ec80-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="8ec80-128">Web.config ファイルに新しいデータベース接続文字列。</span><span class="sxs-lookup"><span data-stu-id="8ec80-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="8ec80-129">OrdersContext.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8ec80-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="8ec80-130">コンス トラクターが、データベース接続文字列の名前を指定することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8ec80-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="8ec80-131">この名前は、Web.config に追加された接続文字列を参照します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="8ec80-132">`OrdersContext` クラスに次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="8ec80-133">A **DbSet**問い合わせることができるエンティティのセットを表します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="8ec80-134">ここでは、完全な一覧で、`OrdersContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="8ec80-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="8ec80-135">`AdminController`クラスは、基本的な CRUD 機能を実装する 5 つのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="8ec80-136">各メソッドは、クライアントが呼び出すことのできる URI に対応しています。</span><span class="sxs-lookup"><span data-stu-id="8ec80-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="8ec80-137">コント ローラー メソッド</span><span class="sxs-lookup"><span data-stu-id="8ec80-137">Controller Method</span></span> | <span data-ttu-id="8ec80-138">説明</span><span class="sxs-lookup"><span data-stu-id="8ec80-138">Description</span></span> | <span data-ttu-id="8ec80-139">URI</span><span class="sxs-lookup"><span data-stu-id="8ec80-139">URI</span></span> | <span data-ttu-id="8ec80-140">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="8ec80-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8ec80-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="8ec80-141">GetProducts</span></span> | <span data-ttu-id="8ec80-142">すべての製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-142">Gets all products.</span></span> | <span data-ttu-id="8ec80-143">api/products</span><span class="sxs-lookup"><span data-stu-id="8ec80-143">api/products</span></span> | <span data-ttu-id="8ec80-144">GET</span><span class="sxs-lookup"><span data-stu-id="8ec80-144">GET</span></span> |
| <span data-ttu-id="8ec80-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="8ec80-145">GetProduct</span></span> | <span data-ttu-id="8ec80-146">ID によって製品を検索します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-146">Finds a product by ID.</span></span> | <span data-ttu-id="8ec80-147">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8ec80-147">api/products/*id*</span></span> | <span data-ttu-id="8ec80-148">GET</span><span class="sxs-lookup"><span data-stu-id="8ec80-148">GET</span></span> |
| <span data-ttu-id="8ec80-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="8ec80-149">PutProduct</span></span> | <span data-ttu-id="8ec80-150">製品を更新します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-150">Updates a product.</span></span> | <span data-ttu-id="8ec80-151">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8ec80-151">api/products/*id*</span></span> | <span data-ttu-id="8ec80-152">PUT</span><span class="sxs-lookup"><span data-stu-id="8ec80-152">PUT</span></span> |
| <span data-ttu-id="8ec80-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="8ec80-153">PostProduct</span></span> | <span data-ttu-id="8ec80-154">新しい製品を作成します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-154">Creates a new product.</span></span> | <span data-ttu-id="8ec80-155">api/products</span><span class="sxs-lookup"><span data-stu-id="8ec80-155">api/products</span></span> | <span data-ttu-id="8ec80-156">POST</span><span class="sxs-lookup"><span data-stu-id="8ec80-156">POST</span></span> |
| <span data-ttu-id="8ec80-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="8ec80-157">DeleteProduct</span></span> | <span data-ttu-id="8ec80-158">製品を削除します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-158">Deletes a product.</span></span> | <span data-ttu-id="8ec80-159">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="8ec80-159">api/products/*id*</span></span> | <span data-ttu-id="8ec80-160">Del</span><span class="sxs-lookup"><span data-stu-id="8ec80-160">DELETE</span></span> |

<span data-ttu-id="8ec80-161">各メソッドの呼び出しに`OrdersContext`データベースを照会します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="8ec80-162">(PUT、POST、および DELETE) のコレクションを変更するメソッドを呼び出す`db.SaveChanges`データベースに変更を保持します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="8ec80-163">コント ローラーは HTTP 要求ごとに作成され、メソッドから返される前に、変更を永続化する必要があるため、その後破棄します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="8ec80-164">データベース初期化子を追加します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-164">Add a Database Initializer</span></span>

<span data-ttu-id="8ec80-165">Entity Framework では、スタートアップ時に、データベースを設定し、モデルが変更されるたびに自動的にデータベースを再作成できる便利な機能があります。</span><span class="sxs-lookup"><span data-stu-id="8ec80-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="8ec80-166">この機能は、モデルを変更した場合でも、テスト データが常にあるため、開発では便利です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="8ec80-167">ソリューション エクスプ ローラーで、Models フォルダーを右クリックし、という名前の新しいクラスを作成する`OrdersContextInitializer`です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="8ec80-168">次の実装に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="8ec80-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="8ec80-169">継承することによって、 **DropCreateDatabaseIfModelChanges**クラスを指示することに Entity Framework モデル クラスは変更されるたびに、データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="8ec80-170">Entity Framework を作成 (または再作成)、データベース呼び出し、**シード**テーブルに挿入する方法です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="8ec80-171">使用して、**シード**メソッドをいくつかの例の製品と使用例注文を追加します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="8ec80-172">この機能は、テストの多くが使用しない、 **DropCreateDatabaseIfModelChanges**モデル クラスを変更した場合、データが失われる可能性がありますので、実稼働環境でクラスです。</span><span class="sxs-lookup"><span data-stu-id="8ec80-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="8ec80-173">次に、Global.asax を開くし、次のコードを追加、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="8ec80-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="8ec80-174">コント ローラーに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-174">Send a Request to the Controller</span></span>

<span data-ttu-id="8ec80-175">この時点で、すべてのクライアント コードでは、まだ作成されていないが、web API、web ブラウザーや、HTTP デバッグを使用してツールなどを呼び出すことができます[Fiddler](http://www.fiddler2.com/fiddler2/)です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="8ec80-176">Visual Studio で f5 キーを押してデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="8ec80-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="8ec80-177">Web ブラウザーで開かれます`http://localhost:*portnum*/`ここで、*させる*いくつかのポート番号です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="8ec80-178">HTTP 要求を送信"`http://localhost:*portnum*/api/admin`です。</span><span class="sxs-lookup"><span data-stu-id="8ec80-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="8ec80-179">最初の要求は、Entify フレームワークを作成し、データベースのシードする必要があるためにを完了に時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="8ec80-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="8ec80-180">応答には、以下のように、次の必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ec80-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="8ec80-181">[前へ](using-web-api-with-entity-framework-part-2.md)
> [次へ](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="8ec80-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
