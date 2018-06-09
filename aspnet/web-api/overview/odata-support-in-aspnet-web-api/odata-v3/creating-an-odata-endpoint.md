---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Web API 2 OData v3 エンドポイントの作成 |Microsoft ドキュメント
author: MikeWasson
description: Open Data Protocol (OData) は、web のデータ アクセス プロトコルです。 OData は、データ構造体、データをクエリおよびデータを操作する一貫した方法を提供します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874630"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="2a810-104">Web API 2 OData v3 エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="2a810-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2a810-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2a810-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="2a810-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="2a810-107">[Open Data Protocol](http://www.odata.org/) (OData) は、web のデータ アクセス プロトコル。</span><span class="sxs-lookup"><span data-stu-id="2a810-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="2a810-108">OData にはデータを構造体、データに対するクエリおよび CRUD 操作を通じてデータ セットを操作する方法を統一 (作成、読み取り、更新、および削除) します。</span><span class="sxs-lookup"><span data-stu-id="2a810-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="2a810-109">OData は、AtomPub (XML) と JSON 形式の両方をサポートします。</span><span class="sxs-lookup"><span data-stu-id="2a810-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="2a810-110">OData では、データに関するメタデータを公開する方法も定義します。</span><span class="sxs-lookup"><span data-stu-id="2a810-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="2a810-111">クライアントは、型情報と、データ セットの関係を検出するのにメタデータを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2a810-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="2a810-112">ASP.NET Web API では、簡単にデータ セットの OData エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="2a810-113">エンドポイントをサポートしている OData 操作だけを制御できます。</span><span class="sxs-lookup"><span data-stu-id="2a810-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="2a810-114">非 OData エンドポイントと共に、複数の OData エンドポイントをホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="2a810-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="2a810-115">データ モデル、バックエンド ビジネス ロジック、およびデータ層を完全に制御があります。</span><span class="sxs-lookup"><span data-stu-id="2a810-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2a810-116">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="2a810-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="2a810-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2a810-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="2a810-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="2a810-118">Web API 2</span></span>
> - <span data-ttu-id="2a810-119">OData バージョン 3</span><span class="sxs-lookup"><span data-stu-id="2a810-119">OData Version 3</span></span>
> - <span data-ttu-id="2a810-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2a810-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="2a810-121">Fiddler Web デバッグ プロキシ (省略可能)</span><span class="sxs-lookup"><span data-stu-id="2a810-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="2a810-122">Web API OData のサポートが追加された[ASP.NET および Web ツール 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650)です。</span><span class="sxs-lookup"><span data-stu-id="2a810-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="2a810-123">ただし、このチュートリアルでは、Visual Studio 2013 で追加されたスキャフォールディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="2a810-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="2a810-124">このチュートリアルでは、クライアントが照会できる単純な OData エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="2a810-125">エンドポイントの c# クライアントも作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="2a810-126">このチュートリアルを完了すると、チュートリアルの次のセットが、アクション、エンティティ リレーションシップを含む、多くの機能を追加する方法を表示し、$展開/$ を選択します。</span><span class="sxs-lookup"><span data-stu-id="2a810-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="2a810-127">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="2a810-128">エンティティ モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="2a810-129">OData コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="2a810-130">EDM およびルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="2a810-131">(省略可能) のデータベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="2a810-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="2a810-132">OData エンドポイントの探索</span><span class="sxs-lookup"><span data-stu-id="2a810-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="2a810-133">OData のシリアル化形式</span><span class="sxs-lookup"><span data-stu-id="2a810-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="2a810-134">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="2a810-135">このチュートリアルでは、基本的な CRUD 操作をサポートする OData エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="2a810-136">エンドポイントは、1 つのリソース、製品のリストを公開します。</span><span class="sxs-lookup"><span data-stu-id="2a810-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="2a810-137">後のチュートリアルより多くの機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="2a810-138">Visual Studio を起動し、選択**新しいプロジェクト**スタート ページからです。</span><span class="sxs-lookup"><span data-stu-id="2a810-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="2a810-139">またはから、**ファイル**メニューの **新規**し**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2a810-140">**テンプレート**ペインで、**インストールされたテンプレート**[Visual c#] ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="2a810-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="2a810-141">**Visual c#** **Web**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="2a810-142">選択**ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="2a810-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="2a810-143">**新しい ASP.NET プロジェクト**ダイアログで、選択、**空**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="2a810-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="2a810-144">&quot;フォルダーを追加し、コアを参照しています.&quot;、確認**Web API**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="2a810-145">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a810-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="2a810-146">エンティティ モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-146">Add an Entity Model</span></span>

<span data-ttu-id="2a810-147">*モデル*は、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="2a810-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="2a810-148">このチュートリアルでは、製品を表すモデルが必要です。</span><span class="sxs-lookup"><span data-stu-id="2a810-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="2a810-149">モデルは、OData エンティティ型に対応しています。</span><span class="sxs-lookup"><span data-stu-id="2a810-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="2a810-150">ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="2a810-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="2a810-151">コンテキスト メニューから選択**追加**を選択し、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="2a810-152">**新規追加**ダイアログの項目に、クラスを名前&quot;製品&quot;です。</span><span class="sxs-lookup"><span data-stu-id="2a810-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="2a810-153">慣例により、モデルのクラスは、Models フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="2a810-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="2a810-154">独自のプロジェクトでこの規則に従う必要はありませんが、このチュートリアルで使用します。</span><span class="sxs-lookup"><span data-stu-id="2a810-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="2a810-155">Product.cs ファイルでは、次のクラス定義を追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="2a810-156">ID プロパティは、エンティティ キーになります。</span><span class="sxs-lookup"><span data-stu-id="2a810-156">The ID property will be the entity key.</span></span> <span data-ttu-id="2a810-157">クライアントは ID によって製品を照会できます。</span><span class="sxs-lookup"><span data-stu-id="2a810-157">Clients can query products by ID.</span></span> <span data-ttu-id="2a810-158">このフィールドは、バックエンド データベースの主キーもなります。</span><span class="sxs-lookup"><span data-stu-id="2a810-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="2a810-159">今すぐプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="2a810-159">Build the project now.</span></span> <span data-ttu-id="2a810-160">次の手順では、リフレクションを使用して、製品の種類を検索するいくつかの Visual Studio スキャフォールディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="2a810-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="2a810-161">OData コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-161">Add an OData Controller</span></span>

<span data-ttu-id="2a810-162">A*コント ローラー* HTTP 要求を処理するクラスです。</span><span class="sxs-lookup"><span data-stu-id="2a810-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="2a810-163">各エンティティに設定する OData サービスで別のコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="2a810-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="2a810-164">このチュートリアルでは単一のコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="2a810-165">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="2a810-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="2a810-166">選択**追加**し、**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="2a810-167">**追加 Scaffold**ダイアログで、 &quot;Web API 2 OData コント ローラー アクション、Entity Framework を使用して&quot;です。</span><span class="sxs-lookup"><span data-stu-id="2a810-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="2a810-168">**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー"ProductsController"です。</span><span class="sxs-lookup"><span data-stu-id="2a810-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="2a810-169">選択、&quot;非同期コント ローラー アクションを使用して&quot;チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="2a810-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="2a810-170">**モデル**ドロップダウン リストで、Product クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="2a810-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="2a810-171">クリックして、**新しいデータ コンテキストしています.** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a810-171">Click the **New data context...** button.</span></span> <span data-ttu-id="2a810-172">データ コンテキスト型の既定の名前のままにし、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="2a810-173">コント ローラーを追加するコント ローラーの追加ダイアログ ボックスで [追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a810-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="2a810-174">注: エラー メッセージが表示することを知らせる&quot;型を取得中にエラーが発生しました.&quot;、Product クラスを追加した後は、Visual Studio プロジェクトをビルドすることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="2a810-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="2a810-175">スキャフォールディングでは、リフレクションを使用して、クラスを検索します。</span><span class="sxs-lookup"><span data-stu-id="2a810-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="2a810-176">スキャフォールディングでは、プロジェクトに 2 つのコード ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="2a810-177">Products.cs では、OData エンドポイントを実装する Web API コント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="2a810-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="2a810-178">ProductServiceContext.cs は、Entity Framework を使用して、基になるデータベースをクエリするメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="2a810-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="2a810-179">EDM およびルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-179">Add the EDM and Route</span></span>

<span data-ttu-id="2a810-180">ソリューション エクスプ ローラーで、アプリを展開して\_フォルダーを起動し、WebApiConfig.cs をという名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="2a810-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="2a810-181">このクラスは、Web API の構成コードを保持します。</span><span class="sxs-lookup"><span data-stu-id="2a810-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="2a810-182">このコードを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2a810-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="2a810-183">このコードでは、2 つの処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="2a810-183">This code does two things:</span></span>

- <span data-ttu-id="2a810-184">OData エンドポイントのエンティティ データ モデル (EDM) を作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="2a810-185">エンドポイントのルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="2a810-186">EDM は、データの抽象モデルです。</span><span class="sxs-lookup"><span data-stu-id="2a810-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="2a810-187">EDM は、メタデータ ドキュメントを作成し、サービスの Uri を定義に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2a810-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="2a810-188">**ODataConventionModelBuilder** EDM 既定の名前付け規則のセットを使用して、EDM を作成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="2a810-189">このアプローチには、最小限のコードが必要です。</span><span class="sxs-lookup"><span data-stu-id="2a810-189">This approach requires the least code.</span></span> <span data-ttu-id="2a810-190">EDM より詳細に制御する場合を使えば、**ため**プロパティ、キー、およびナビゲーションのプロパティを明示的に追加することによって、EDM を作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="2a810-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="2a810-191">**EntitySet**メソッドは、EDM に設定エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="2a810-192">文字列"Products"では、エンティティ セットの名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="2a810-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="2a810-193">コント ローラーの名前は、エンティティ セットの名前と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a810-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="2a810-194">エンティティ セットでこのチュートリアルでは、"Products など"の名前は、コント ローラーの名前は`ProductsController`します。</span><span class="sxs-lookup"><span data-stu-id="2a810-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="2a810-195">エンティティ セット"ProductSet"という名前を付けた場合、コント ローラーの名前を付けますが`ProductSetController`です。</span><span class="sxs-lookup"><span data-stu-id="2a810-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="2a810-196">エンドポイントが複数のエンティティ セットを持てることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2a810-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="2a810-197">呼び出す**EntitySet&lt;T&gt;** の各エンティティ セットにし、対応するコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="2a810-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="2a810-198">**MapODataRoute**メソッドは、OData エンドポイントのルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="2a810-199">最初のパラメーターは、ルートのフレンドリ名です。</span><span class="sxs-lookup"><span data-stu-id="2a810-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="2a810-200">サービスのクライアントでは、この名前は表示されません。</span><span class="sxs-lookup"><span data-stu-id="2a810-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="2a810-201">2 番目のパラメーターは、エンドポイントの URI プレフィックスです。</span><span class="sxs-lookup"><span data-stu-id="2a810-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="2a810-202">このコードを指定するには、Products エンティティ セットの URI は http://<em>hostname</em>odata/製品です。</span><span class="sxs-lookup"><span data-stu-id="2a810-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="2a810-203">アプリケーションでは、1 つ以上の OData エンドポイントを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="2a810-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="2a810-204">各エンドポイントでは、呼び出す<strong>MapODataRoute</strong>固有のルート名と一意の URI プレフィックスを入力します。</span><span class="sxs-lookup"><span data-stu-id="2a810-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="2a810-205">(省略可能) のデータベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="2a810-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="2a810-206">この手順では、Entity Framework を使用して、テスト データの一部で、データベースのシードです。</span><span class="sxs-lookup"><span data-stu-id="2a810-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="2a810-207">この手順は省略可能ながすぐに、OData エンドポイントをテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="2a810-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="2a810-208">**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="2a810-209">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2a810-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="2a810-210">これは、移行とされる Configuration.cs をという名前のコード ファイルをという名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="2a810-211">このファイルを開き、次のコードを追加、`Configuration.Seed`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="2a810-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="2a810-212">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2a810-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="2a810-213">これらのコマンドは、データベースを作成し、そのコードを実行するコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="2a810-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="2a810-214">OData エンドポイントの探索</span><span class="sxs-lookup"><span data-stu-id="2a810-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="2a810-215">使用して、このセクションで、 [Fiddler Web デバッグ プロキシ](http://www.fiddler2.com)をエンドポイントに要求を送信し、応答メッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="2a810-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="2a810-216">これは、OData エンドポイントの機能を理解するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2a810-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="2a810-217">Visual Studio で f5 キーを押してデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="2a810-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="2a810-218">既定では、Visual Studio がブラウザーを開いて`http://localhost:*port*`ここで、*ポート*プロジェクト設定で構成されたポート番号です。</span><span class="sxs-lookup"><span data-stu-id="2a810-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="2a810-219">プロジェクト設定でポート番号を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="2a810-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="2a810-220">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="2a810-221">[プロパティ] ウィンドウで選択**Web**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="2a810-222">ポート番号を入力**プロジェクト Url**です。</span><span class="sxs-lookup"><span data-stu-id="2a810-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="2a810-223">サービス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2a810-223">Service Document</span></span>

<span data-ttu-id="2a810-224">*サービス ドキュメント*OData エンドポイントのエンティティ セットの一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2a810-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="2a810-225">サービス ドキュメントを取得するには、サービスのルート URI に GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="2a810-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="2a810-226">次の URI を入力して、Fiddler を使用して、 **Composer**  タブ:`http://localhost:port/odata/`ここで、*ポート*のポート番号です。</span><span class="sxs-lookup"><span data-stu-id="2a810-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="2a810-227">クリックして、 **Execute**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a810-227">Click the **Execute** button.</span></span> <span data-ttu-id="2a810-228">Fiddler は、アプリケーションに HTTP GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="2a810-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="2a810-229">Web セッションの一覧で、応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2a810-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="2a810-230">すべてが動作している場合、状態コードは 200 になります。</span><span class="sxs-lookup"><span data-stu-id="2a810-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="2a810-231">インスペクター タブで、応答メッセージの詳細を表示する Web セッションの一覧で、応答をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a810-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="2a810-232">生の HTTP 応答メッセージは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="2a810-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="2a810-233">既定では、Web API は、AtomPub 形式でサービス ドキュメントを返します。</span><span class="sxs-lookup"><span data-stu-id="2a810-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="2a810-234">JSON を要求するには、HTTP 要求に次のヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="2a810-235">これで、HTTP 応答には、JSON ペイロードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2a810-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="2a810-236">サービス メタデータ ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2a810-236">Service Metadata Document</span></span>

<span data-ttu-id="2a810-237">*サービス メタデータ ドキュメント*概念スキーマ定義言語 (CSDL) と呼ばれる XML 言語を使用して、サービスのデータ モデルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="2a810-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="2a810-238">メタデータ ドキュメントでは、サービスでは、データの構造を示していて、クライアント コードを生成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="2a810-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="2a810-239">メタデータ ドキュメントを取得する GET 要求を送信`http://localhost:port/odata/$metadata`です。</span><span class="sxs-lookup"><span data-stu-id="2a810-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="2a810-240">このチュートリアルで示すようにエンドポイントのメタデータを次に示します。</span><span class="sxs-lookup"><span data-stu-id="2a810-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="2a810-241">エンティティ セット</span><span class="sxs-lookup"><span data-stu-id="2a810-241">Entity Set</span></span>

<span data-ttu-id="2a810-242">Products エンティティ セットを取得する GET 要求を送信`http://localhost:port/odata/Products`です。</span><span class="sxs-lookup"><span data-stu-id="2a810-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="2a810-243">エンティティ</span><span class="sxs-lookup"><span data-stu-id="2a810-243">Entity</span></span>

<span data-ttu-id="2a810-244">個々 の製品を入手するに GET 要求を送信`http://localhost:port/odata/Products(1)`ここで、「1」は、製品の id です。</span><span class="sxs-lookup"><span data-stu-id="2a810-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="2a810-245">OData のシリアル化形式</span><span class="sxs-lookup"><span data-stu-id="2a810-245">OData Serialization Formats</span></span>

<span data-ttu-id="2a810-246">OData には、いくつかのシリアル化形式がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2a810-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="2a810-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="2a810-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="2a810-248">JSON"light"(OData v3 で導入されました)</span><span class="sxs-lookup"><span data-stu-id="2a810-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="2a810-249">JSON"verbose"(OData v2)</span><span class="sxs-lookup"><span data-stu-id="2a810-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="2a810-250">既定では、Web API は、AtomPubJSON"light"形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="2a810-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="2a810-251">AtomPub 形式を取得するには、「アプリケーションおよび atom + xml」に Accept ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="2a810-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="2a810-252">応答本文の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2a810-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="2a810-253">Atom 形式の 1 つの明らかな欠点を確認できます: JSON light よりもずっと詳細なであります。</span><span class="sxs-lookup"><span data-stu-id="2a810-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="2a810-254">ただし、AtomPub を理解しているクライアントがあれば、クライアント方がよい形式で JSON 上。</span><span class="sxs-lookup"><span data-stu-id="2a810-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="2a810-255">次に、同じエンティティの JSON の軽量バージョンを示します。</span><span class="sxs-lookup"><span data-stu-id="2a810-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="2a810-256">JSON light 形式は、OData プロトコルの version 3 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="2a810-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="2a810-257">旧バージョンとの互換性のため、クライアントは、"verbose"古い JSON 形式を要求できます。</span><span class="sxs-lookup"><span data-stu-id="2a810-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="2a810-258">詳細な JSON を要求する Accept ヘッダーを設定`application/json;odata=verbose`です。</span><span class="sxs-lookup"><span data-stu-id="2a810-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="2a810-259">詳細なバージョンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="2a810-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="2a810-260">この形式は、セッション全体にかなりのオーバーヘッドを追加できる応答本文でより多くのメタデータを伝達します。</span><span class="sxs-lookup"><span data-stu-id="2a810-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="2a810-261">また、"d"をという名前のプロパティで、オブジェクトをラップすることによって、レベルの間接参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a810-262">次の手順</span><span class="sxs-lookup"><span data-stu-id="2a810-262">Next Steps</span></span>

- [<span data-ttu-id="2a810-263">エンティティのリレーションシップを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="2a810-264">OData アクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="2a810-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="2a810-265">.NET クライアントから OData サービスを呼び出す</span><span class="sxs-lookup"><span data-stu-id="2a810-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
