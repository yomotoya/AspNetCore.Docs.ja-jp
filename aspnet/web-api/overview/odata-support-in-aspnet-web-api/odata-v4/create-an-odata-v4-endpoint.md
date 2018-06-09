---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: ASP.NET Web API 2.2 を使用して OData v4 エンドポイントを作成 |Microsoft ドキュメント
author: MikeWasson
description: Open Data Protocol (OData) は、web のデータ アクセス プロトコルです。 OData は、クエリおよび CRUD 操作からのデータ セットを操作する一貫した方法を提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508051"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="47da4-104">ASP.NET Web API 2.2 を使用して OData v4 エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="47da4-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="47da4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="47da4-106">Open Data Protocol (OData) は、web のデータ アクセス プロトコルです。</span><span class="sxs-lookup"><span data-stu-id="47da4-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="47da4-107">OData クエリおよび CRUD 操作からのデータ セットを操作する一貫した方法には (作成、読み取り、更新、および削除) します。</span><span class="sxs-lookup"><span data-stu-id="47da4-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="47da4-108">ASP.NET Web API には、v3 と v4 のプロトコルの両方がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="47da4-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="47da4-109">サイド バイ サイドを実行している v4 エンドポイントを持つこともできます v3 エンドポイントを使用します。</span><span class="sxs-lookup"><span data-stu-id="47da4-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="47da4-110">このチュートリアルでは、CRUD 操作をサポートする OData v4 エンドポイントを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="47da4-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="47da4-111">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="47da4-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="47da4-112">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="47da4-112">Web API 2.2</span></span>
> - <span data-ttu-id="47da4-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="47da4-113">OData v4</span></span>
> - [<span data-ttu-id="47da4-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="47da4-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="47da4-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="47da4-115">Entity Framework 6</span></span>
> - <span data-ttu-id="47da4-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="47da4-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="47da4-117">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="47da4-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="47da4-118">OData バージョン 3 を参照してください。 [OData v3 エンドポイントを作成する](../odata-v3/creating-an-odata-endpoint.md)です。</span><span class="sxs-lookup"><span data-stu-id="47da4-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="47da4-119">Visual Studio プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="47da4-120">Visual Studio から、**ファイル**メニューの **新規** &gt; **プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="47da4-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="47da4-121">展開**インストール** &gt; **テンプレート** &gt; **Visual c#** &gt; **Web**、し、を選択**ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="47da4-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="47da4-122">プロジェクトに名前を&quot;ProductService&quot;です。</span><span class="sxs-lookup"><span data-stu-id="47da4-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="47da4-123">**新しいプロジェクト**ダイアログで、選択、**空**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="47da4-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="47da4-124">&quot;フォルダーを追加し、コア参照しています.&quot;をクリックして**Web API**です。</span><span class="sxs-lookup"><span data-stu-id="47da4-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="47da4-125">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="47da4-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="47da4-126">OData のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="47da4-126">Install the OData Packages</span></span>

<span data-ttu-id="47da4-127">**ツール**メニューの  **NuGet Package Manager** &gt; **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="47da4-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="47da4-128">パッケージ マネージャー コンソール ウィンドウで次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="47da4-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="47da4-129">このコマンドは、最新の OData NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="47da4-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="47da4-130">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="47da4-130">Add a Model Class</span></span>

<span data-ttu-id="47da4-131">A*モデル*アプリケーションにデータ エンティティを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="47da4-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="47da4-132">ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="47da4-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="47da4-133">コンテキスト メニューから選択**追加** &gt; **クラス**です。</span><span class="sxs-lookup"><span data-stu-id="47da4-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="47da4-134">慣例により、モデルのクラスが Models フォルダーに配置されますが、独自のプロジェクトでこの規則に準拠する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="47da4-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="47da4-135">クラスに `Product` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="47da4-135">Name the class `Product`.</span></span> <span data-ttu-id="47da4-136">Product.cs ファイルでは、次のように定型コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="47da4-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="47da4-137">`Id`プロパティがエンティティ キー。</span><span class="sxs-lookup"><span data-stu-id="47da4-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="47da4-138">クライアントは、キーによってエンティティをクエリできます。</span><span class="sxs-lookup"><span data-stu-id="47da4-138">Clients can query entities by key.</span></span> <span data-ttu-id="47da4-139">たとえば、5 の ID を持つ製品を取得するには、URI は`/Products(5)`します。</span><span class="sxs-lookup"><span data-stu-id="47da4-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="47da4-140">`Id`プロパティは、バックエンド データベースの主キーにもなります。</span><span class="sxs-lookup"><span data-stu-id="47da4-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="47da4-141">Entity Framework を有効にします。</span><span class="sxs-lookup"><span data-stu-id="47da4-141">Enable Entity Framework</span></span>

<span data-ttu-id="47da4-142">このチュートリアルで使用されます Entity Framework (EF) Code First バックエンド データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="47da4-143">Web API OData には、EF は不要です。</span><span class="sxs-lookup"><span data-stu-id="47da4-143">Web API OData does not require EF.</span></span> <span data-ttu-id="47da4-144">データベース エンティティをモデルに変換できる任意のデータ アクセス層を使用します。</span><span class="sxs-lookup"><span data-stu-id="47da4-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="47da4-145">最初に、EF の NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="47da4-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="47da4-146">**ツール**メニューの  **NuGet Package Manager** &gt; **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="47da4-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="47da4-147">パッケージ マネージャー コンソール ウィンドウで次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="47da4-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="47da4-148">Web.config ファイルを開き、内の次のセクションを追加、**構成**要素の後に、 **configSections**要素。</span><span class="sxs-lookup"><span data-stu-id="47da4-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="47da4-149">この設定は、LocalDB データベースの接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="47da4-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="47da4-150">このデータベースは、アプリをローカルで実行するときに使用されます。</span><span class="sxs-lookup"><span data-stu-id="47da4-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="47da4-151">という名前のクラスを次に、追加`ProductsContext`Models フォルダーに。</span><span class="sxs-lookup"><span data-stu-id="47da4-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="47da4-152">コンス トラクターで`"name=ProductsContext"`接続文字列の名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="47da4-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="47da4-153">OData エンドポイントを構成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="47da4-154">アプリのファイルを開く\_Start/WebApiConfig.cs です。</span><span class="sxs-lookup"><span data-stu-id="47da4-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="47da4-155">次の追加**を使用して**ステートメント。</span><span class="sxs-lookup"><span data-stu-id="47da4-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="47da4-156">次のコードを追加、**登録**メソッド。</span><span class="sxs-lookup"><span data-stu-id="47da4-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="47da4-157">このコードでは、2 つの処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="47da4-157">This code does two things:</span></span>

- <span data-ttu-id="47da4-158">Entity Data Model (EDM) を作成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="47da4-159">ルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="47da4-159">Adds a route.</span></span>

<span data-ttu-id="47da4-160">EDM は、データの抽象モデルです。</span><span class="sxs-lookup"><span data-stu-id="47da4-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="47da4-161">EDM を使用して、サービス メタデータ ドキュメントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="47da4-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="47da4-162">**ODataConventionModelBuilder**クラスは、既定の名前付け規則を使用して、EDM を作成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="47da4-163">このアプローチには、最小限のコードが必要です。</span><span class="sxs-lookup"><span data-stu-id="47da4-163">This approach requires the least code.</span></span> <span data-ttu-id="47da4-164">EDM より詳細に制御する場合を使えば、**ため**プロパティ、キー、およびナビゲーションのプロパティを明示的に追加することによって、EDM を作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="47da4-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="47da4-165">A*ルート*Web API HTTP 要求をエンドポイントにルーティングする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="47da4-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="47da4-166">OData v4 ルートを作成するには、 **MapODataServiceRoute**拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="47da4-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="47da4-167">アプリケーションに複数の OData エンドポイントがある場合は、ごとに個別のルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="47da4-168">一意のルート名とプレフィックスは、各ルートを提供します。</span><span class="sxs-lookup"><span data-stu-id="47da4-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="47da4-169">OData コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="47da4-169">Add the OData Controller</span></span>

<span data-ttu-id="47da4-170">A*コント ローラー* HTTP 要求を処理するクラスです。</span><span class="sxs-lookup"><span data-stu-id="47da4-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="47da4-171">エンティティごとに、OData サービスで設定したコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="47da4-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="47da4-172">このチュートリアルでは、1 つのコント ローラーを作成、`Product`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="47da4-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="47da4-173">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックして**追加** &gt; **クラス**です。</span><span class="sxs-lookup"><span data-stu-id="47da4-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="47da4-174">クラスに `ProductsController` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="47da4-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="47da4-175">OData v3 の使用は、このチュートリアルのバージョン、**コント ローラーの追加**スキャフォールディングできます。</span><span class="sxs-lookup"><span data-stu-id="47da4-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="47da4-176">現時点では、OData v4 のスキャフォールディングではありません。</span><span class="sxs-lookup"><span data-stu-id="47da4-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="47da4-177">ProductsController.cs で定型コードを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="47da4-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="47da4-178">コント ローラーの使用、 `ProductsContext` EF を使用してデータベースにアクセスするクラス。</span><span class="sxs-lookup"><span data-stu-id="47da4-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="47da4-179">コント ローラーをオーバーライドする通知、 **Dispose**を破棄する方法、 **ProductsContext**です。</span><span class="sxs-lookup"><span data-stu-id="47da4-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="47da4-180">これは、コント ローラーの開始点です。</span><span class="sxs-lookup"><span data-stu-id="47da4-180">This is the starting point for the controller.</span></span> <span data-ttu-id="47da4-181">次に、CRUD 操作のすべてのメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="47da4-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="47da4-182">エンティティ セットのクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="47da4-182">Querying the Entity Set</span></span>

<span data-ttu-id="47da4-183">次のメソッドを追加`ProductsController`です。</span><span class="sxs-lookup"><span data-stu-id="47da4-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="47da4-184">パラメーターなしのバージョンの`Get`メソッド全体の製品のコレクションを返します。</span><span class="sxs-lookup"><span data-stu-id="47da4-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="47da4-185">`Get`メソッドを*キー*パラメーターがキーを使用して、製品を検索 (ここで、`Id`プロパティ)。</span><span class="sxs-lookup"><span data-stu-id="47da4-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="47da4-186">**[EnableQuery]** 属性により、クライアントは、$filter、$sort、$page などのクエリ オプションを使用して、クエリを変更します。</span><span class="sxs-lookup"><span data-stu-id="47da4-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="47da4-187">詳細については、次を参照してください。 [OData クエリ オプションをサポートする](../supporting-odata-query-options.md)です。</span><span class="sxs-lookup"><span data-stu-id="47da4-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="47da4-188">エンティティ セットにエンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="47da4-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="47da4-189">データベースに新しい製品を追加するクライアントを有効にするには、次のメソッドを追加`ProductsController`です。</span><span class="sxs-lookup"><span data-stu-id="47da4-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="47da4-190">エンティティの更新</span><span class="sxs-lookup"><span data-stu-id="47da4-190">Updating an Entity</span></span>

<span data-ttu-id="47da4-191">OData は、PATCH、PUT、エンティティの更新の 2 つの異なるセマンティクスをサポートします。</span><span class="sxs-lookup"><span data-stu-id="47da4-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="47da4-192">修正プログラムでは、部分的な更新を実行します。</span><span class="sxs-lookup"><span data-stu-id="47da4-192">PATCH performs a partial update.</span></span> <span data-ttu-id="47da4-193">クライアントは、更新するプロパティだけを指定します。</span><span class="sxs-lookup"><span data-stu-id="47da4-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="47da4-194">PUT は、エンティティ全体を置換します。</span><span class="sxs-lookup"><span data-stu-id="47da4-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="47da4-195">PUT の欠点は、クライアントがエンティティでは、値を変更しないを含むすべてのプロパティの値を送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="47da4-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="47da4-196">[OData 仕様](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719)修正プログラムが優先されることを示すです。</span><span class="sxs-lookup"><span data-stu-id="47da4-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="47da4-197">いずれの場合は、修正プログラムと PUT の両メソッドのコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="47da4-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="47da4-198">コント ローラーを使用して、修正プログラムの場合、**デルタ&lt;T&gt;** 変更を追跡する型。</span><span class="sxs-lookup"><span data-stu-id="47da4-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="47da4-199">エンティティの削除</span><span class="sxs-lookup"><span data-stu-id="47da4-199">Deleting an Entity</span></span>

<span data-ttu-id="47da4-200">データベースから製品を削除するクライアントを有効にするには、次のメソッドを追加`ProductsController`です。</span><span class="sxs-lookup"><span data-stu-id="47da4-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
