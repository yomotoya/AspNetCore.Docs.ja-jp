---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Web API 2 OData v3 エンドポイントの作成 |Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) は、web 用のデータ アクセス プロトコルです。 OData では、データを構造体、データのクエリをおよびデータを操作する方法を統一を提供しています.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 654f697c8d095d45ba31e2808c52f9ad24b606c8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832378"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="bea48-104">Web API 2 OData v3 エンドポイントの作成</span><span class="sxs-lookup"><span data-stu-id="bea48-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="bea48-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bea48-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bea48-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="bea48-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="bea48-107">[Open Data Protocol](http://www.odata.org/) (OData) は、web のデータ アクセス プロトコル。</span><span class="sxs-lookup"><span data-stu-id="bea48-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="bea48-108">OData データ構造体、データを照会および CRUD 操作を通じてデータ セットを操作する一貫した方法を提供します (作成、読み取り、更新、および削除) します。</span><span class="sxs-lookup"><span data-stu-id="bea48-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="bea48-109">OData では、AtomPub (XML) と JSON 形式の両方をサポートします。</span><span class="sxs-lookup"><span data-stu-id="bea48-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="bea48-110">OData では、データに関するメタデータを公開する方法も定義します。</span><span class="sxs-lookup"><span data-stu-id="bea48-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="bea48-111">クライアントは、型情報と、データ セットの関係を検出するのにメタデータを使用できます。</span><span class="sxs-lookup"><span data-stu-id="bea48-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="bea48-112">ASP.NET Web API では、簡単にデータ セットの OData エンドポイントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="bea48-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="bea48-113">OData 操作正確にエンドポイントがサポートを制御できます。</span><span class="sxs-lookup"><span data-stu-id="bea48-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="bea48-114">非 OData エンドポイントと共に、複数の OData エンドポイントをホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="bea48-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="bea48-115">完全に制御、データ モデル、バック エンドのビジネス ロジック、およびデータ層があります。</span><span class="sxs-lookup"><span data-stu-id="bea48-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bea48-116">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="bea48-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="bea48-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bea48-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="bea48-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="bea48-118">Web API 2</span></span>
> - <span data-ttu-id="bea48-119">OData バージョン 3</span><span class="sxs-lookup"><span data-stu-id="bea48-119">OData Version 3</span></span>
> - <span data-ttu-id="bea48-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="bea48-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="bea48-121">Fiddler の Web デバッグ プロキシ (省略可能)</span><span class="sxs-lookup"><span data-stu-id="bea48-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="bea48-122">Web API OData のサポートが追加された[ASP.NET および Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650)します。</span><span class="sxs-lookup"><span data-stu-id="bea48-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="bea48-123">ただし、このチュートリアルでは、Visual Studio 2013 で追加されたスキャフォールディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="bea48-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="bea48-124">このチュートリアルでは、クライアントを照会できる単純な OData エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="bea48-125">C# クライアント エンドポイントも作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="bea48-126">このチュートリアルを完了すると、次の一連のチュートリアルは、エンティティ関係、アクションを含む、多くの機能を追加する方法を示すし、$展開/$ を選択します。</span><span class="sxs-lookup"><span data-stu-id="bea48-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="bea48-127">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="bea48-128">エンティティ モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="bea48-129">OData コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="bea48-130">EDM およびルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="bea48-131">(省略可能) データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="bea48-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="bea48-132">OData エンドポイントの探索</span><span class="sxs-lookup"><span data-stu-id="bea48-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="bea48-133">OData シリアル化の形式</span><span class="sxs-lookup"><span data-stu-id="bea48-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="bea48-134">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="bea48-135">このチュートリアルでは、基本的な CRUD 操作をサポートする OData エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="bea48-136">エンドポイントでは、製品の一覧の 1 つのリソースを公開します。</span><span class="sxs-lookup"><span data-stu-id="bea48-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="bea48-137">以降のチュートリアルより多くの機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="bea48-138">Visual Studio を起動し、選択**新しいプロジェクト**スタート ページから。</span><span class="sxs-lookup"><span data-stu-id="bea48-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="bea48-139">またはから、**ファイル**メニューの **新規**し**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="bea48-140">**テンプレート**ペインで、**インストールされたテンプレート**Visual c# ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="bea48-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="bea48-141">**Visual c#**、 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="bea48-142">選択 **、ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="bea48-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="bea48-143">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="bea48-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="bea48-144">&quot;フォルダーを追加し、コアを参照しています.&quot;、確認**Web API**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="bea48-145">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bea48-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="bea48-146">エンティティ モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-146">Add an Entity Model</span></span>

<span data-ttu-id="bea48-147">*モデル*は、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="bea48-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="bea48-148">このチュートリアルでは、製品を表すモデルが必要です。</span><span class="sxs-lookup"><span data-stu-id="bea48-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="bea48-149">モデルは、OData エンティティ型に対応します。</span><span class="sxs-lookup"><span data-stu-id="bea48-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="bea48-150">ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="bea48-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="bea48-151">コンテキスト メニューでは、次のように選択します。**追加**選び**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="bea48-152">**新規追加**ダイアログの項目、クラスの名前を付けて&quot;製品&quot;します。</span><span class="sxs-lookup"><span data-stu-id="bea48-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="bea48-153">慣例により、モデル クラスは、Models フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="bea48-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="bea48-154">独自のプロジェクトでこの規則に従う必要はありませんが、このチュートリアルを使用します。</span><span class="sxs-lookup"><span data-stu-id="bea48-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="bea48-155">Product.cs ファイルでは、次のクラス定義を追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="bea48-156">ID プロパティは、エンティティ キーになります。</span><span class="sxs-lookup"><span data-stu-id="bea48-156">The ID property will be the entity key.</span></span> <span data-ttu-id="bea48-157">クライアントは ID によって製品を照会できます。</span><span class="sxs-lookup"><span data-stu-id="bea48-157">Clients can query products by ID.</span></span> <span data-ttu-id="bea48-158">このフィールドは、バックエンド データベースの主キーもなります。</span><span class="sxs-lookup"><span data-stu-id="bea48-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="bea48-159">これでプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="bea48-159">Build the project now.</span></span> <span data-ttu-id="bea48-160">次の手順では、リフレクションを使用して、製品の種類を検索するいくつかの Visual Studio スキャフォールディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="bea48-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="bea48-161">OData コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-161">Add an OData Controller</span></span>

<span data-ttu-id="bea48-162">A*コント ローラー*は HTTP 要求を処理するクラスです。</span><span class="sxs-lookup"><span data-stu-id="bea48-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="bea48-163">各エンティティの OData サービスのセットを別のコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="bea48-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="bea48-164">このチュートリアルでは、単一のコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="bea48-165">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="bea48-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="bea48-166">選択**追加**選び**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="bea48-167">**スキャフォールディングの追加**ダイアログ ボックスで、 &quot;Web API 2 OData コント ローラー アクション、Entity Framework を使用して&quot;します。</span><span class="sxs-lookup"><span data-stu-id="bea48-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="bea48-168">**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー"ProductsController"。</span><span class="sxs-lookup"><span data-stu-id="bea48-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="bea48-169">選択、&quot;非同期コント ローラー アクションを使用して、&quot;チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="bea48-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="bea48-170">**モデル**ドロップダウン リストで、Product クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="bea48-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="bea48-171">をクリックして、**新しいデータ コンテキスト.** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bea48-171">Click the **New data context...** button.</span></span> <span data-ttu-id="bea48-172">データ コンテキスト型の既定の名前のままにし、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="bea48-173">コント ローラーを追加するコント ローラーの追加ダイアログ ボックスで [追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bea48-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="bea48-174">注: いるというエラー メッセージを取得するかどうかは&quot;型を取得中にエラーが発生しました.&quot;、Product クラスを追加した後、Visual Studio プロジェクトをビルドしたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="bea48-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="bea48-175">スキャフォールディングでは、リフレクションを使用して、クラスを検索します。</span><span class="sxs-lookup"><span data-stu-id="bea48-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="bea48-176">スキャフォールディングは、プロジェクトに 2 つのコード ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="bea48-177">Products.cs では、OData エンドポイントを実装する Web API コント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="bea48-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="bea48-178">ProductServiceContext.cs は、Entity Framework を使用して、基になるデータベースを照会するメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="bea48-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="bea48-179">EDM およびルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-179">Add the EDM and Route</span></span>

<span data-ttu-id="bea48-180">ソリューション エクスプ ローラーでアプリケーションを拡張して\_フォルダーを起動し、WebApiConfig.cs という名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="bea48-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="bea48-181">このクラスは、Web API の構成コードを保持します。</span><span class="sxs-lookup"><span data-stu-id="bea48-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="bea48-182">このコードを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bea48-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="bea48-183">このコードは 2 つの処理を行います。</span><span class="sxs-lookup"><span data-stu-id="bea48-183">This code does two things:</span></span>

- <span data-ttu-id="bea48-184">OData エンドポイントのエンティティ データ モデル (EDM) を作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="bea48-185">エンドポイントのルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="bea48-186">EDM は、データの抽象モデルです。</span><span class="sxs-lookup"><span data-stu-id="bea48-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="bea48-187">EDM は、メタデータ ドキュメントを作成し、サービスの Uri の定義に使用されます。</span><span class="sxs-lookup"><span data-stu-id="bea48-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="bea48-188">**ODataConventionModelBuilder** EDM 既定の名前付け規則のセットを使用して EDM を作成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="bea48-189">このアプローチでは、最小限のコードが必要です。</span><span class="sxs-lookup"><span data-stu-id="bea48-189">This approach requires the least code.</span></span> <span data-ttu-id="bea48-190">使用することができます、EDM より詳細に制御する場合、**に**プロパティ、キー、およびナビゲーション プロパティを明示的に追加することで、EDM を作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="bea48-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="bea48-191">**EntitySet**メソッドは、EDM に設定エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="bea48-192">文字列「製品」は、エンティティ セットの名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="bea48-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="bea48-193">コント ローラーの名前は、エンティティ セットの名前と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bea48-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="bea48-194">エンティティ セットでこのチュートリアルでは、"Products"の名前は、コント ローラーの名前は`ProductsController`します。</span><span class="sxs-lookup"><span data-stu-id="bea48-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="bea48-195">エンティティ セットの"ProductSet"という名前を付けた場合は、名前、コント ローラー`ProductSetController`します。</span><span class="sxs-lookup"><span data-stu-id="bea48-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="bea48-196">エンドポイントは複数のエンティティ セットであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="bea48-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="bea48-197">呼び出す**EntitySet&lt;T&gt;** エンティティごとに設定すると、および対応するコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="bea48-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="bea48-198">**MapODataRoute**メソッドは、OData エンドポイントのルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="bea48-199">最初のパラメーターは、ルートのフレンドリ名です。</span><span class="sxs-lookup"><span data-stu-id="bea48-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="bea48-200">サービスのクライアントでは、この名前は表示されません。</span><span class="sxs-lookup"><span data-stu-id="bea48-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="bea48-201">2 番目のパラメーターは、エンドポイントの URI プレフィックスです。</span><span class="sxs-lookup"><span data-stu-id="bea48-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="bea48-202">このコードを指定するには、Products エンティティ セットの URI は http://<em>hostname</em>  /odata/製品です。</span><span class="sxs-lookup"><span data-stu-id="bea48-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="bea48-203">アプリケーションでは、1 つ以上の OData エンドポイントを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="bea48-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="bea48-204">各エンドポイントでは、呼び出す<strong>MapODataRoute</strong>一意のルート名と一意の URI プレフィックスを入力します。</span><span class="sxs-lookup"><span data-stu-id="bea48-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="bea48-205">(省略可能) データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="bea48-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="bea48-206">この手順では、何らかのテスト データでデータベースをシードするのに Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="bea48-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="bea48-207">この手順は省略可能でも、すぐ OData エンドポイントをテストできます。</span><span class="sxs-lookup"><span data-stu-id="bea48-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="bea48-208">**ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bea48-209">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="bea48-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="bea48-210">これには、移行とという Configuration.cs コード ファイルをという名前のフォルダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="bea48-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="bea48-211">このファイルを開き、次のコードを追加、`Configuration.Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="bea48-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="bea48-212">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="bea48-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="bea48-213">これらのコマンドは、データベースを作成し、そのコードを実行するコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="bea48-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="bea48-214">OData エンドポイントの探索</span><span class="sxs-lookup"><span data-stu-id="bea48-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="bea48-215">このセクションで使用します、 [Fiddler Web デバッグ プロキシ](http://www.fiddler2.com)エンドポイントに要求を送信し、応答メッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="bea48-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="bea48-216">これは、OData エンドポイントの機能を理解するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="bea48-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="bea48-217">Visual Studio で f5 キーを押してデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="bea48-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="bea48-218">既定では、Visual Studio がブラウザーを開いて`http://localhost:*port*`ここで、*ポート*はプロジェクトの設定で構成されているポート番号です。</span><span class="sxs-lookup"><span data-stu-id="bea48-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="bea48-219">プロジェクトの設定でポート番号を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="bea48-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="bea48-220">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="bea48-221">[プロパティ] ウィンドウで次のように選択します。 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="bea48-222">ポート番号を入力**プロジェクト Url**します。</span><span class="sxs-lookup"><span data-stu-id="bea48-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="bea48-223">サービス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="bea48-223">Service Document</span></span>

<span data-ttu-id="bea48-224">*サービス ドキュメント*OData エンドポイントのエンティティ セットの一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bea48-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="bea48-225">サービス ドキュメントを取得するには、ルート サービスの URI に GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="bea48-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="bea48-226">次の URI を入力して、Fiddler を使用して、 **Composer**  タブ:`http://localhost:port/odata/`ここで、*ポート*のポート番号です。</span><span class="sxs-lookup"><span data-stu-id="bea48-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="bea48-227">をクリックして、 **Execute**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bea48-227">Click the **Execute** button.</span></span> <span data-ttu-id="bea48-228">Fiddler では、アプリケーションに HTTP GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="bea48-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="bea48-229">Web セッションの一覧で応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="bea48-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="bea48-230">すべてが動作している場合、状態コードが 200 になります。</span><span class="sxs-lookup"><span data-stu-id="bea48-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="bea48-231">[インスペクター] タブで、応答メッセージの詳細を表示する Web セッションの一覧で、応答をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="bea48-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="bea48-232">生の HTTP 応答メッセージは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="bea48-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="bea48-233">既定では、Web API は、AtomPub 形式でサービス ドキュメントを返します。</span><span class="sxs-lookup"><span data-stu-id="bea48-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="bea48-234">JSON を要求するには、HTTP 要求に次のヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="bea48-235">これで、HTTP 応答には、JSON ペイロードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bea48-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="bea48-236">サービス メタデータ ドキュメント</span><span class="sxs-lookup"><span data-stu-id="bea48-236">Service Metadata Document</span></span>

<span data-ttu-id="bea48-237">*サービス メタデータ ドキュメント*概念スキーマ定義言語 (CSDL) と呼ばれる XML 言語を使用して、サービスのデータ モデルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="bea48-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="bea48-238">メタデータ ドキュメントでは、サービスでは、データの構造を示していて、クライアント コードを生成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="bea48-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="bea48-239">メタデータ ドキュメントを取得する GET 要求を送信`http://localhost:port/odata/$metadata`します。</span><span class="sxs-lookup"><span data-stu-id="bea48-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="bea48-240">このチュートリアルで示すようにエンドポイントのメタデータを次に示します。</span><span class="sxs-lookup"><span data-stu-id="bea48-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="bea48-241">エンティティ セット</span><span class="sxs-lookup"><span data-stu-id="bea48-241">Entity Set</span></span>

<span data-ttu-id="bea48-242">Products エンティティ セットを取得する GET 要求を送信`http://localhost:port/odata/Products`します。</span><span class="sxs-lookup"><span data-stu-id="bea48-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="bea48-243">エンティティ</span><span class="sxs-lookup"><span data-stu-id="bea48-243">Entity</span></span>

<span data-ttu-id="bea48-244">個別の製品を取得する GET 要求を送信`http://localhost:port/odata/Products(1)`ここで、「1」は製品の id。</span><span class="sxs-lookup"><span data-stu-id="bea48-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="bea48-245">OData シリアル化の形式</span><span class="sxs-lookup"><span data-stu-id="bea48-245">OData Serialization Formats</span></span>

<span data-ttu-id="bea48-246">OData は、いくつかのシリアル化形式をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="bea48-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="bea48-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="bea48-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="bea48-248">JSON (OData v3 で導入) の"light"</span><span class="sxs-lookup"><span data-stu-id="bea48-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="bea48-249">JSON の"verbose"(OData v2)</span><span class="sxs-lookup"><span data-stu-id="bea48-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="bea48-250">既定では、Web API は、AtomPubJSON"light"の形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="bea48-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="bea48-251">AtomPub 形式を取得するには、「アプリケーション フィードおよび atom + xml」に Accept ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="bea48-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="bea48-252">応答本文の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="bea48-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="bea48-253">Atom 形式の 1 つの明らかなメリットを確認できます。 JSON light よりもはるかに詳細になります。</span><span class="sxs-lookup"><span data-stu-id="bea48-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="bea48-254">ただし、AtomPub を認識するクライアントがある場合、クライアントがその形式を JSON に対するに優先する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bea48-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="bea48-255">次に、同じエンティティの JSON の軽量バージョンを示します。</span><span class="sxs-lookup"><span data-stu-id="bea48-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="bea48-256">JSON light 形式は、OData プロトコルのバージョン 3 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="bea48-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="bea48-257">旧バージョンと互換性のため、クライアントは、古い"verbose"の JSON 形式を要求できます。</span><span class="sxs-lookup"><span data-stu-id="bea48-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="bea48-258">詳細な JSON を要求に Accept ヘッダーを設定します。`application/json;odata=verbose`します。</span><span class="sxs-lookup"><span data-stu-id="bea48-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="bea48-259">詳細なバージョンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="bea48-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="bea48-260">この形式は、セッション全体にかなりのオーバーヘッドを追加できる応答の本体でより多くのメタデータを伝達します。</span><span class="sxs-lookup"><span data-stu-id="bea48-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="bea48-261">また、"d"をという名前のプロパティで、オブジェクトをラップすることによって、レベルの間接参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="bea48-262">次の手順</span><span class="sxs-lookup"><span data-stu-id="bea48-262">Next Steps</span></span>

- [<span data-ttu-id="bea48-263">エンティティのリレーションシップを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="bea48-264">OData アクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="bea48-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="bea48-265">.NET クライアントから OData サービスを呼び出す</span><span class="sxs-lookup"><span data-stu-id="bea48-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
