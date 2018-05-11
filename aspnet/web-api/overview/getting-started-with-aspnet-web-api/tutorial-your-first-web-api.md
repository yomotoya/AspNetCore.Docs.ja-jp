---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: ASP.NET Web API 2 (C#) を開始します。
author: MikeWasson
description: HTTP は、web ページを提供しているだけではなくです。 サービスとデータを公開する Api を構築するための強力なプラットフォームです。 HTTP は、シンプルで柔軟なので、および ubiq には.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: d881563cdb6449aada444ef0528061581113a925
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="c4c77-105">ASP.NET Web API 2 (C#) を開始します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-105">Get Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="c4c77-106">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4c77-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c4c77-107">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="c4c77-108">HTTP は、web ページを提供しているだけではなくです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="c4c77-109">HTTP は、サービスとデータを公開する Api を構築するための強力なプラットフォームではもです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="c4c77-110">HTTP、シンプルかつ柔軟なので、ユビキタスです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="c4c77-111">ほとんどのプラットフォームの考えることができますには、HTTP サービスがクライアント、ブラウザー、モバイル デバイスは、従来のデスクトップ アプリケーションなどの広範な範囲にアクセスできるように HTTP ライブラリがします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="c4c77-112">ASP.NET Web API は、web Api、.NET Framework 上に構築するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="c4c77-113">このチュートリアルでは ASP.NET Web API を使用して web API 製品の一覧を表すオブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c4c77-114">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="c4c77-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="c4c77-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c4c77-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="c4c77-116">Web API 2</span><span class="sxs-lookup"><span data-stu-id="c4c77-116">Web API 2</span></span>

<span data-ttu-id="c4c77-117">参照してください[ASP.NET Core と Visual Studio for Windows で web API を作成する](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api)のこのチュートリアルの最新バージョンです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="c4c77-118">Web API プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-118">Create a Web API Project</span></span>

<span data-ttu-id="c4c77-119">このチュートリアルでは ASP.NET Web API を使用して web API 製品の一覧を表すオブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="c4c77-120">フロント エンドの web ページでは、jQuery を使用して、結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="c4c77-121">Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="c4c77-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="c4c77-122">またはから、**ファイル**メニューの **新規**し**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c4c77-123">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="c4c77-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c4c77-124">**Visual c#** **Web**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c4c77-125">プロジェクト テンプレートの一覧で選択**ASP.NET Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="c4c77-126">プロジェクト"ProductsApp"の名前し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="c4c77-127">**新しい ASP.NET プロジェクト**ダイアログで、選択、**空**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="c4c77-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="c4c77-128">&quot;フォルダーを追加し、参照用のコア&quot;、確認**Web API**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="c4c77-129">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="c4c77-130">使用して Web API プロジェクトを作成することも、 &quot;Web API&quot;テンプレート。</span><span class="sxs-lookup"><span data-stu-id="c4c77-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="c4c77-131">Web API テンプレートでは、ASP.NET MVC を使用して、API のヘルプ ページを提供します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="c4c77-132">MVC せず Web API を表示するために、このチュートリアルでは、空のテンプレートを使っています。</span><span class="sxs-lookup"><span data-stu-id="c4c77-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="c4c77-133">一般に、Web API を使用して ASP.NET MVC を知る必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c4c77-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="c4c77-134">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-134">Adding a Model</span></span>

<span data-ttu-id="c4c77-135">*モデル*は、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="c4c77-136">ASP.NET Web API では、JSON、XML、または他のいくつかの形式をモデルを自動的にシリアル化でき、HTTP 応答メッセージの本文にシリアル化されたデータを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="c4c77-137">クライアントは、シリアル化形式を読み取ることができます、限り、オブジェクトを逆シリアル化できます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="c4c77-138">ほとんどのクライアントには、XML または JSON を解析できます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="c4c77-139">さらに、クライアントは、HTTP 要求メッセージに Accept ヘッダーを設定して、どの形式を指定できます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="c4c77-140">製品を表す単純なモデルの作成を始めます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="c4c77-141">ソリューション エクスプ ローラーが表示されない場合にクリックして、**ビュー**メニュー**ソリューション エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="c4c77-142">ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="c4c77-143">コンテキスト メニューから選択**追加**を選択し、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="c4c77-144">クラスの名前を付けます&quot;製品&quot;です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="c4c77-145">次のプロパティを追加、`Product`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="c4c77-146">コントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="c4c77-146">Adding a Controller</span></span>

<span data-ttu-id="c4c77-147">Web API では、*コント ローラー* HTTP 要求を処理するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="c4c77-148">ID で指定された 1 つの製品または製品の一覧のいずれかを返すことができるコント ローラーを追加します</span><span class="sxs-lookup"><span data-stu-id="c4c77-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="c4c77-149">ASP.NET MVC を使用している場合、既に精通コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="c4c77-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="c4c77-150">Web API コント ローラーは、MVC コント ローラーに似ていますが、継承、 **ApiController**クラスの代わりに、**コント ローラー**クラスです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="c4c77-151">**ソリューション エクスプ ローラー**、コント ローラーのフォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="c4c77-152">選択**追加**し、**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="c4c77-153">**追加 Scaffold**ダイアログで、 **Web API コント ローラー - 空**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="c4c77-154">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="c4c77-155">**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー &quot;ProductsController&quot;です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="c4c77-156">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="c4c77-157">スキャフォールディングでは、コント ローラーのフォルダーに ProductsController.cs をという名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="c4c77-158">コント ローラーをという名前のフォルダーに、コント ローラーを配置する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c4c77-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="c4c77-159">フォルダー名は、ソース ファイルを整理する便利な手段だけです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="c4c77-160">このファイルがまだ開いていないを開くには、ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="c4c77-161">次のようにこのファイル内のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="c4c77-162">単純な例から始めてに製品は、コント ローラー クラス内の固定長の配列に格納されます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="c4c77-163">もちろん、実際のアプリケーションではデータベースを照会するかを使用して他の外部データ ソース。</span><span class="sxs-lookup"><span data-stu-id="c4c77-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="c4c77-164">コント ローラーには、製品を返す 2 つのメソッドが定義されています。</span><span class="sxs-lookup"><span data-stu-id="c4c77-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="c4c77-165">`GetAllProducts`メソッドとして製品の全体の一覧を返します、 **IEnumerable&lt;製品&gt;** 型です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="c4c77-166">`GetProduct`メソッドは、ID によって 1 つの製品を検索</span><span class="sxs-lookup"><span data-stu-id="c4c77-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="c4c77-167">これで完了です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-167">That's it!</span></span> <span data-ttu-id="c4c77-168">作業用の web API があります。</span><span class="sxs-lookup"><span data-stu-id="c4c77-168">You have a working web API.</span></span> <span data-ttu-id="c4c77-169">コント ローラーの各メソッドは、1 つまたは複数の Uri に対応しています。</span><span class="sxs-lookup"><span data-stu-id="c4c77-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="c4c77-170">コント ローラー メソッド</span><span class="sxs-lookup"><span data-stu-id="c4c77-170">Controller Method</span></span> | <span data-ttu-id="c4c77-171">URI</span><span class="sxs-lookup"><span data-stu-id="c4c77-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="c4c77-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="c4c77-172">GetAllProducts</span></span> | <span data-ttu-id="c4c77-173">/api/products</span><span class="sxs-lookup"><span data-stu-id="c4c77-173">/api/products</span></span> |
| <span data-ttu-id="c4c77-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="c4c77-174">GetProduct</span></span> | <span data-ttu-id="c4c77-175">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c4c77-175">/api/products/*id*</span></span> |

<span data-ttu-id="c4c77-176">`GetProduct` 、メソッド、 *id* URI には、プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="c4c77-177">たとえば、5 の ID を持つ製品を取得するには、URI は`api/products/5`します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="c4c77-178">コント ローラーのメソッドに、Web API が HTTP 要求をルーティングする方法の詳細については、次を参照してください。 [ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="c4c77-179">Javascript と jQuery Web API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="c4c77-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="c4c77-180">このセクションでは、web API を呼び出す AJAX を使用する HTML ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="c4c77-181">JQuery AJAX 呼び出しを行うと、結果を含むページを更新するも使用されます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="c4c77-182">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加**選択してから、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="c4c77-183">**新しい項目の追加**ダイアログで、選択、 **Web**ノードの下**Visual c#**、クリックして、 **HTML ページ**項目。</span><span class="sxs-lookup"><span data-stu-id="c4c77-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="c4c77-184">ページの名前を付けます&quot;index.html&quot;です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="c4c77-185">次のように、このファイル内のすべてを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="c4c77-186">JQuery を取得するいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="c4c77-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="c4c77-187">この例では使用して、 [Microsoft Ajax CDN](../../../ajax/cdn/overview.md)です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="c4c77-188">ダウンロードすることも[ http://jquery.com/ ](http://jquery.com/)、および ASP.NET の「Web API」プロジェクト テンプレートにには、jQuery にもが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c4c77-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="c4c77-189">製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-189">Getting a List of Products</span></span>

<span data-ttu-id="c4c77-190">製品の一覧を取得するには、HTTP GET 要求を送信&quot;/api 製品&quot;です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="c4c77-191">JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/)関数が AJAX 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="c4c77-192">応答には、JSON オブジェクトの配列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c4c77-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="c4c77-193">`done`関数は、要求が成功した場合に呼び出されるコールバックを指定します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="c4c77-194">コールバックでは、製品情報を使用して DOM を更新します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="c4c77-195">ID によって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-195">Getting a Product By ID</span></span>

<span data-ttu-id="c4c77-196">ID によって製品を取得するには、送信に HTTP GET 要求を&quot;/api 製品/*id*&quot;ここで、 *id*製品 ID です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="c4c77-197">まだ呼び`getJSON`AJAX 要求がこの時間を送信する要求 URI で ID を挿入しました。</span><span class="sxs-lookup"><span data-stu-id="c4c77-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="c4c77-198">この要求からの応答は、1 つの製品の JSON 表現です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="c4c77-199">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="c4c77-199">Running the Application</span></span>

<span data-ttu-id="c4c77-200">F5 キーを押して、アプリケーションのデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="c4c77-201">Web ページは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c4c77-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="c4c77-202">ID によって製品を取得し、ID を入力して検索 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="c4c77-203">無効な ID を入力すると、サーバーは HTTP エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="c4c77-204">F12 キーを使用して、HTTP 要求と応答を表示するには</span><span class="sxs-lookup"><span data-stu-id="c4c77-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="c4c77-205">HTTP サービスを使用する場合は、HTTP 要求を表示、メッセージを要求する場合に役立ちますができます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="c4c77-206">これは、Internet Explorer 9 で F12 開発者ツールを使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="c4c77-207">Internet Explorer 9 を押して、 **F12**を開くには、ツールです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="c4c77-208">をクリックして、**ネットワーク**タブとキーを押して**キャプチャ開始**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="c4c77-209">これで、キーを押して、web ページに戻ってください**f5 キーを押して**web ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="c4c77-210">Internet Explorer では、ブラウザーと web サーバー間の HTTP トラフィックをキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="c4c77-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="c4c77-211">概要ビューは、ページのすべてのネットワーク トラフィックを示しています。</span><span class="sxs-lookup"><span data-stu-id="c4c77-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="c4c77-212">相対 URI のエントリが見つかりません"api 製品//"です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="c4c77-213">このエントリを選択し、をクリックして**詳細ビューに移動して**です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="c4c77-214">詳細ビューでは、要求と応答ヘッダーと本文を表示するタブがあります。</span><span class="sxs-lookup"><span data-stu-id="c4c77-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="c4c77-215">たとえばをクリックする、**要求ヘッダー**  タブで、クライアントが要求した確認できます&quot;アプリケーション/json&quot; Accept ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="c4c77-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="c4c77-216">応答本文 タブをクリックすると、製品の一覧が JSON にシリアル化される方法が確認できます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="c4c77-217">その他のブラウザーでは、同様の機能があります。</span><span class="sxs-lookup"><span data-stu-id="c4c77-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="c4c77-218">他の便利なツールが[Fiddler](http://www.fiddler2.com/fiddler2/)、web デバッグ プロキシです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="c4c77-219">使用できる Fiddler、HTTP トラフィックを表示して、HTTP 要求の作成にもため、要求で HTTP ヘッダーを完全に制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="c4c77-220">Azure で実行されているこのアプリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c4c77-220">See this App Running on Azure</span></span>

<span data-ttu-id="c4c77-221">ライブ web アプリとして実行している完成したサイトを参照してもよろしいですか。</span><span class="sxs-lookup"><span data-stu-id="c4c77-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="c4c77-222">Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。</span><span class="sxs-lookup"><span data-stu-id="c4c77-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="c4c77-223">Azure アカウントを Azure にこのソリューションを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4c77-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="c4c77-224">アカウントがない場合は、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="c4c77-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="c4c77-225">[Azure アカウントを無料で開いて](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)のクレジットを取得する有料の Azure サービスを試すことができますを使用して使用後もアカウントを保持する最大の使用は、Azure のサービスを解放します。</span><span class="sxs-lookup"><span data-stu-id="c4c77-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="c4c77-226">[MSDN サブスクリプション会員の特典を有効に](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)が、MSDN サブスクリプションでは、クレジット毎月 Azure の有料のサービスに使用することができます。</span><span class="sxs-lookup"><span data-stu-id="c4c77-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4c77-227">次の手順</span><span class="sxs-lookup"><span data-stu-id="c4c77-227">Next Steps</span></span>

- <span data-ttu-id="c4c77-228">POST、PUT、および DELETE の操作をサポートし、データベースに書き込みます HTTP サービスのより完全な例を参照してください。 [Entity Framework 6 の Web API 2 を使用して](../data/using-web-api-with-entity-framework/part-1.md)です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="c4c77-229">HTTP サービスの上位の流動性および応答 web アプリケーションの作成に関する詳細は、次を参照してください。 [ASP.NET Single Page Application](../../../single-page-application/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="c4c77-230">Azure App Service に、Visual Studio web プロジェクトを展開する方法については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。</span><span class="sxs-lookup"><span data-stu-id="c4c77-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
