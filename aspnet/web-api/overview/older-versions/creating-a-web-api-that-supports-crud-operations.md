---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: ASP.NET Web API 1 で CRUD 操作の有効化 |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスで CRUD 操作をサポートする方法を示します。 チュートリアルの Visual Studio 2012 Web AP で使用されるソフトウェアのバージョン.
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: ba061b26b8527e447f25f6046057542a54f989a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826314"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="2f976-104">ASP.NET Web API 1 で CRUD 操作を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2f976-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="2f976-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2f976-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2f976-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="2f976-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="2f976-107">このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスで CRUD 操作をサポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2f976-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2f976-108">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="2f976-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2f976-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2f976-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="2f976-110">Web API 1 (Web API 2 での動作も)</span><span class="sxs-lookup"><span data-stu-id="2f976-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="2f976-111">CRUD の略&quot;作成、読み取り、更新、および削除、&quot; 4 つの基本的なデータベース操作であります。</span><span class="sxs-lookup"><span data-stu-id="2f976-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="2f976-112">多くの HTTP サービスでは、REST api または REST のような Api を介して、CRUD 操作もモデルします。</span><span class="sxs-lookup"><span data-stu-id="2f976-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="2f976-113">このチュートリアルでは、非常に単純な web 製品の一覧を管理する API を構築します。</span><span class="sxs-lookup"><span data-stu-id="2f976-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="2f976-114">各製品は、名前、価格、およびカテゴリが含まれます (など&quot;toys&quot;または&quot;ハードウェア&quot;)、さらに製品の id。</span><span class="sxs-lookup"><span data-stu-id="2f976-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="2f976-115">製品の API は、次のメソッドに公開します。</span><span class="sxs-lookup"><span data-stu-id="2f976-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="2f976-116">アクション</span><span class="sxs-lookup"><span data-stu-id="2f976-116">Action</span></span> | <span data-ttu-id="2f976-117">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="2f976-117">HTTP method</span></span> | <span data-ttu-id="2f976-118">相対 URI</span><span class="sxs-lookup"><span data-stu-id="2f976-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f976-119">すべての製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="2f976-119">Get a list of all products</span></span> | <span data-ttu-id="2f976-120">GET</span><span class="sxs-lookup"><span data-stu-id="2f976-120">GET</span></span> | <span data-ttu-id="2f976-121">/api/products</span><span class="sxs-lookup"><span data-stu-id="2f976-121">/api/products</span></span> |
| <span data-ttu-id="2f976-122">ID によって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="2f976-122">Get a product by ID</span></span> | <span data-ttu-id="2f976-123">GET</span><span class="sxs-lookup"><span data-stu-id="2f976-123">GET</span></span> | <span data-ttu-id="2f976-124">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2f976-124">/api/products/*id*</span></span> |
| <span data-ttu-id="2f976-125">カテゴリによって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="2f976-125">Get a product by category</span></span> | <span data-ttu-id="2f976-126">GET</span><span class="sxs-lookup"><span data-stu-id="2f976-126">GET</span></span> | <span data-ttu-id="2f976-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="2f976-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="2f976-128">新しい成果物を作成します。</span><span class="sxs-lookup"><span data-stu-id="2f976-128">Create a new product</span></span> | <span data-ttu-id="2f976-129">POST</span><span class="sxs-lookup"><span data-stu-id="2f976-129">POST</span></span> | <span data-ttu-id="2f976-130">/api/products</span><span class="sxs-lookup"><span data-stu-id="2f976-130">/api/products</span></span> |
| <span data-ttu-id="2f976-131">製品を更新します。</span><span class="sxs-lookup"><span data-stu-id="2f976-131">Update a product</span></span> | <span data-ttu-id="2f976-132">PUT</span><span class="sxs-lookup"><span data-stu-id="2f976-132">PUT</span></span> | <span data-ttu-id="2f976-133">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2f976-133">/api/products/*id*</span></span> |
| <span data-ttu-id="2f976-134">製品を削除します。</span><span class="sxs-lookup"><span data-stu-id="2f976-134">Delete a product</span></span> | <span data-ttu-id="2f976-135">Del</span><span class="sxs-lookup"><span data-stu-id="2f976-135">DELETE</span></span> | <span data-ttu-id="2f976-136">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2f976-136">/api/products/*id*</span></span> |

<span data-ttu-id="2f976-137">パスに製品 ID を含む Uri の一部に注目してください。</span><span class="sxs-lookup"><span data-stu-id="2f976-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="2f976-138">たとえば、製品 ID が 28 を取得するクライアント GET 要求を送信`http://hostname/api/products/28`します。</span><span class="sxs-lookup"><span data-stu-id="2f976-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="2f976-139">リソース</span><span class="sxs-lookup"><span data-stu-id="2f976-139">Resources</span></span>

<span data-ttu-id="2f976-140">製品の API では、2 つのリソースの種類の Uri を定義します。</span><span class="sxs-lookup"><span data-stu-id="2f976-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="2f976-141">リソース</span><span class="sxs-lookup"><span data-stu-id="2f976-141">Resource</span></span> | <span data-ttu-id="2f976-142">URI</span><span class="sxs-lookup"><span data-stu-id="2f976-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="2f976-143">すべての製品の一覧。</span><span class="sxs-lookup"><span data-stu-id="2f976-143">The list of all the products.</span></span> | <span data-ttu-id="2f976-144">/api/products</span><span class="sxs-lookup"><span data-stu-id="2f976-144">/api/products</span></span> |
| <span data-ttu-id="2f976-145">個別の製品です。</span><span class="sxs-lookup"><span data-stu-id="2f976-145">An individual product.</span></span> | <span data-ttu-id="2f976-146">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2f976-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="2f976-147">メソッド</span><span class="sxs-lookup"><span data-stu-id="2f976-147">Methods</span></span>

<span data-ttu-id="2f976-148">4 つの主な HTTP メソッド (GET、PUT、POST、および DELETE) は、CRUD 操作にマップすることができます。</span><span class="sxs-lookup"><span data-stu-id="2f976-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="2f976-149">取得した指定した URI のリソースの表現を取得します。</span><span class="sxs-lookup"><span data-stu-id="2f976-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="2f976-150">サーバーの副作用が GET 必要なしです。</span><span class="sxs-lookup"><span data-stu-id="2f976-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="2f976-151">PUT は、指定された URI にリソースを更新します。</span><span class="sxs-lookup"><span data-stu-id="2f976-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="2f976-152">PUT こともできます、指定した URI で新しいリソースを作成する場合は、サーバーにより、クライアントが新しい Uri を指定します。</span><span class="sxs-lookup"><span data-stu-id="2f976-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="2f976-153">このチュートリアルでは、API では PUT の作成はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="2f976-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="2f976-154">POST は、新しいリソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2f976-154">POST creates a new resource.</span></span> <span data-ttu-id="2f976-155">サーバーは、新しいオブジェクトの URI を割り当てるし、応答メッセージの一部としてこの URI を返します。</span><span class="sxs-lookup"><span data-stu-id="2f976-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="2f976-156">指定された URI にリソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="2f976-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="2f976-157">注: PUT メソッドには、製品全体のエンティティが置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="2f976-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="2f976-158">つまり、更新された製品の完全な表現を送信するクライアントが必要です。</span><span class="sxs-lookup"><span data-stu-id="2f976-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="2f976-159">部分的な更新をサポートする場合は、PATCH メソッドをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2f976-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="2f976-160">このチュートリアルは、修正プログラムを実装していません。</span><span class="sxs-lookup"><span data-stu-id="2f976-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="2f976-161">新しい Web API プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2f976-161">Create a New Web API Project</span></span>

<span data-ttu-id="2f976-162">Visual Studio を使用して起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="2f976-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2f976-163">またはから、**ファイル**メニューの **新規**し**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2f976-164">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="2f976-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2f976-165">**Visual c#**、 **Web**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="2f976-166">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="2f976-167">プロジェクトに名前を&quot;ProductStore&quot;  をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="2f976-168">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、 **Web API**  をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="2f976-169">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="2f976-169">Adding a Model</span></span>

<span data-ttu-id="2f976-170">*モデル*は、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="2f976-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="2f976-171">ASP.NET Web api では、モデルとして厳密に型指定された CLR オブジェクトを使用して、自動的にシリアル化されます XML または JSON をクライアントの。</span><span class="sxs-lookup"><span data-stu-id="2f976-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="2f976-172">データで構成の製品のためという名前の新しいクラスを作成します ProductStore api`Product`します。</span><span class="sxs-lookup"><span data-stu-id="2f976-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="2f976-173">ソリューション エクスプ ローラーが表示されない場合は、クリックして、**ビュー**メニュー選択し、**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="2f976-174">ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2f976-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="2f976-175">コンテキスト メニューから次のように選択します。**追加**を選択し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="2f976-176">クラスの名前&quot;製品&quot;します。</span><span class="sxs-lookup"><span data-stu-id="2f976-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="2f976-177">次のプロパティを追加、`Product`クラス。</span><span class="sxs-lookup"><span data-stu-id="2f976-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="2f976-178">リポジトリを追加</span><span class="sxs-lookup"><span data-stu-id="2f976-178">Adding a Repository</span></span>

<span data-ttu-id="2f976-179">製品のコレクションを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2f976-179">We need to store a collection of products.</span></span> <span data-ttu-id="2f976-180">サービス実装からコレクションを分割することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2f976-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="2f976-181">これにより、サービス クラスを書き直すことがなく、バッキング ストアを変更できます。</span><span class="sxs-lookup"><span data-stu-id="2f976-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="2f976-182">この型のデザインが呼び出される、*リポジトリ*パターン。</span><span class="sxs-lookup"><span data-stu-id="2f976-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="2f976-183">まず、リポジトリのジェネリック インターフェイスを定義します。</span><span class="sxs-lookup"><span data-stu-id="2f976-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="2f976-184">ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2f976-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="2f976-185">選択**追加**を選択し、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="2f976-186">**テンプレート**ペインで、**インストールされたテンプレート**c# ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="2f976-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="2f976-187">C# の場合は、選択**コード**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-187">Under C#, select **Code**.</span></span> <span data-ttu-id="2f976-188">コード テンプレートの一覧で選択**インターフェイス**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="2f976-189">インターフェイスの名前&quot;IProductRepository&quot;します。</span><span class="sxs-lookup"><span data-stu-id="2f976-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="2f976-190">次の実装を追加します。</span><span class="sxs-lookup"><span data-stu-id="2f976-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="2f976-191">という名前の Models フォルダーに別のクラスを今すぐ追加&quot;ProductRepository します。&quot;このクラスが `IProductRespository` インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="2f976-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="2f976-192">次の実装を追加します。</span><span class="sxs-lookup"><span data-stu-id="2f976-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="2f976-193">リポジトリは、ローカル メモリ内のリストを保持します。</span><span class="sxs-lookup"><span data-stu-id="2f976-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="2f976-194">チュートリアルについては、[ok] になりますが、実際のアプリケーションでは、データを外部から、データベースであるかを格納する、またはクラウド ストレージ。</span><span class="sxs-lookup"><span data-stu-id="2f976-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="2f976-195">リポジトリ パターンが容易にできるように実装を後で変更します。</span><span class="sxs-lookup"><span data-stu-id="2f976-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="2f976-196">Web API コント ローラーの追加</span><span class="sxs-lookup"><span data-stu-id="2f976-196">Adding a Web API Controller</span></span>

<span data-ttu-id="2f976-197">ASP.NET MVC を使用する場合、し、既に慣れてコント ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2f976-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="2f976-198">ASP.NET Web api で、*コント ローラー*は、クライアントから HTTP 要求を処理するクラスです。</span><span class="sxs-lookup"><span data-stu-id="2f976-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="2f976-199">プロジェクト作成時に、新しいプロジェクト ウィザードは 2 つのコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2f976-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="2f976-200">これらを参照するには、ソリューション エクスプ ローラーで Controllers フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="2f976-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="2f976-201">HomeController は、従来の ASP.NET MVC コント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="2f976-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="2f976-202">サイトの HTML ページの提供を担当し、web API に直接関連しません。</span><span class="sxs-lookup"><span data-stu-id="2f976-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="2f976-203">ValuesController では、例の WebAPI コント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="2f976-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="2f976-204">ソリューション エクスプ ローラーでファイルを右クリックして、ValuesController を削除してください**を削除します。**</span><span class="sxs-lookup"><span data-stu-id="2f976-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="2f976-205">今すぐよう、新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2f976-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="2f976-206">**ソリューション エクスプ ローラー**、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="2f976-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="2f976-207">選択**追加**選び**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="2f976-208">**コント ローラーの追加**ウィザードで、名前、コント ローラー &quot;ProductsController&quot;します。</span><span class="sxs-lookup"><span data-stu-id="2f976-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="2f976-209">**テンプレート**ドロップダウン リストで、**空の API コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="2f976-210">次に、 **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2f976-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="2f976-211">コント ローラーをという名前のフォルダーに、contollers を格納する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2f976-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="2f976-212">フォルダー名は重要です。ソース ファイルを整理する便利な方法では単純にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2f976-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="2f976-213">**コント ローラーの追加**ProductsController.cs Controllers フォルダーをという名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="2f976-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="2f976-214">このファイルがまだ開いていない場合、は、開くファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="2f976-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="2f976-215">次の追加**を使用して**ステートメント。</span><span class="sxs-lookup"><span data-stu-id="2f976-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="2f976-216">保持するフィールドを追加、 **IProductRepository**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2f976-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="2f976-217">呼び出す`new ProductRepository()`コント ローラーでないにとって最善の設計では、コント ローラーの特定の実装に結び付けるため`IProductRepository`します。</span><span class="sxs-lookup"><span data-stu-id="2f976-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="2f976-218">優れたアプローチでは、次を参照してください。 [Web API の依存関係競合回避モジュールを使用して](../advanced/dependency-injection.md)します。</span><span class="sxs-lookup"><span data-stu-id="2f976-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="2f976-219">リソースの取得</span><span class="sxs-lookup"><span data-stu-id="2f976-219">Getting a Resource</span></span>

<span data-ttu-id="2f976-220">ProductStore API をいくつか公開&quot;読み取り&quot;の HTTP GET メソッドの操作。</span><span class="sxs-lookup"><span data-stu-id="2f976-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="2f976-221">各アクションのメソッドに対応させるには、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="2f976-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="2f976-222">アクション</span><span class="sxs-lookup"><span data-stu-id="2f976-222">Action</span></span> | <span data-ttu-id="2f976-223">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="2f976-223">HTTP method</span></span> | <span data-ttu-id="2f976-224">相対 URI</span><span class="sxs-lookup"><span data-stu-id="2f976-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f976-225">すべての製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="2f976-225">Get a list of all products</span></span> | <span data-ttu-id="2f976-226">GET</span><span class="sxs-lookup"><span data-stu-id="2f976-226">GET</span></span> | <span data-ttu-id="2f976-227">/api/products</span><span class="sxs-lookup"><span data-stu-id="2f976-227">/api/products</span></span> |
| <span data-ttu-id="2f976-228">ID によって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="2f976-228">Get a product by ID</span></span> | <span data-ttu-id="2f976-229">GET</span><span class="sxs-lookup"><span data-stu-id="2f976-229">GET</span></span> | <span data-ttu-id="2f976-230">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2f976-230">/api/products/*id*</span></span> |
| <span data-ttu-id="2f976-231">カテゴリによって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="2f976-231">Get a product by category</span></span> | <span data-ttu-id="2f976-232">GET</span><span class="sxs-lookup"><span data-stu-id="2f976-232">GET</span></span> | <span data-ttu-id="2f976-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="2f976-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="2f976-234">すべての製品の一覧を取得するには、このメソッドを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="2f976-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="2f976-235">メソッドの名前で始まる&quot;取得&quot;慣例 GET 要求にマップされるよう、します。</span><span class="sxs-lookup"><span data-stu-id="2f976-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="2f976-236">また、メソッドがパラメーターを持たないため、それを URI にマップが含まれていない、 *&quot;id&quot;* パス セグメント。</span><span class="sxs-lookup"><span data-stu-id="2f976-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="2f976-237">ID によって製品を取得するには、このメソッドを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="2f976-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="2f976-238">このメソッドの名前からも始まります&quot;取得&quot;、という名前のパラメーターがメソッド*id*します。このパラメーターにマップされます、 &quot;id&quot; URI パスのセグメント。</span><span class="sxs-lookup"><span data-stu-id="2f976-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="2f976-239">ASP.NET Web API フレームワークは、ID を適切なデータ型に自動的に変換 (**int**) パラメーター。</span><span class="sxs-lookup"><span data-stu-id="2f976-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="2f976-240">GetProduct メソッド型の例外をスロー **HttpResponseException**場合*id*が無効です。</span><span class="sxs-lookup"><span data-stu-id="2f976-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="2f976-241">この例外は、フレームワークによって 404 (Not Found) エラーに変換されます。</span><span class="sxs-lookup"><span data-stu-id="2f976-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="2f976-242">最後に、カテゴリによって製品を検索するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="2f976-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="2f976-243">要求 URI にクエリ文字列がある場合は、Web API は、コント ローラー メソッドのパラメーターにクエリ パラメーターに一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="2f976-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="2f976-244">フォームの URI ではそのため、"api/製品ですか? カテゴリ =*カテゴリ*"このメソッドにマップされます。</span><span class="sxs-lookup"><span data-stu-id="2f976-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="2f976-245">リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2f976-245">Creating a Resource</span></span>

<span data-ttu-id="2f976-246">次に、メソッドを追加します、`ProductsController`新しい製品を作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="2f976-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="2f976-247">メソッドの単純な実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2f976-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="2f976-248">この方法に関する 2 つのことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2f976-248">Note two things about this method:</span></span>

- <span data-ttu-id="2f976-249">メソッドの名前で始まる&quot;投稿しています.&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f976-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="2f976-250">新しい製品を作成するには、クライアントは、HTTP POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="2f976-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="2f976-251">メソッドは、製品の種類のパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="2f976-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="2f976-252">Web api では、複合型を持つパラメーターでは、要求本文から逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="2f976-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="2f976-253">そのため、クライアントが XML または JSON 形式で、product オブジェクトのシリアル化された表現を送信する予定です。</span><span class="sxs-lookup"><span data-stu-id="2f976-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="2f976-254">この実装は機能しますが、完全なものはありません。</span><span class="sxs-lookup"><span data-stu-id="2f976-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="2f976-255">理想的には、次のように HTTP 応答を今回は。</span><span class="sxs-lookup"><span data-stu-id="2f976-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="2f976-256">**応答コード:** 既定では、Web API フレームワークは 200 (OK) 応答ステータス コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="2f976-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="2f976-257">ただし、http/1.1 プロトコルに従って POST 要求の結果、リソースの作成時に、サーバーという応答がステータス 201 (Created)。</span><span class="sxs-lookup"><span data-stu-id="2f976-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="2f976-258">**場所:** 応答の Location ヘッダーで、新しいリソースの URI を含める必要があります、サーバーは、リソースを作成したときにします。</span><span class="sxs-lookup"><span data-stu-id="2f976-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="2f976-259">ASP.NET Web API を簡単に HTTP 応答メッセージを操作します。</span><span class="sxs-lookup"><span data-stu-id="2f976-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="2f976-260">強化された実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2f976-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="2f976-261">メソッドの戻り値の型は今すぐ**HttpResponseMessage**します。</span><span class="sxs-lookup"><span data-stu-id="2f976-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="2f976-262">返すことによって、 **HttpResponseMessage**製品ではなく、状態コードと Location ヘッダーを含む、HTTP 応答メッセージの詳細を制御することができます。</span><span class="sxs-lookup"><span data-stu-id="2f976-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="2f976-263">**CreateResponse**メソッドを作成、 **HttpResponseMessage**本文に自動的に Product オブジェクトのシリアル化された表現を書き込みます fo 応答メッセージ。</span><span class="sxs-lookup"><span data-stu-id="2f976-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="2f976-264">この例では検証されません、`Product`します。</span><span class="sxs-lookup"><span data-stu-id="2f976-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="2f976-265">モデルの検証については、次を参照してください。 [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="2f976-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="2f976-266">リソースの更新</span><span class="sxs-lookup"><span data-stu-id="2f976-266">Updating a Resource</span></span>

<span data-ttu-id="2f976-267">Put の製品を更新することは簡単です。</span><span class="sxs-lookup"><span data-stu-id="2f976-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="2f976-268">メソッドの名前で始まる&quot;を配置しています.&quot;ので、Web API の PUT 要求に一致します。</span><span class="sxs-lookup"><span data-stu-id="2f976-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="2f976-269">メソッドは、2 つのパラメーター、製品 ID、および製品の更新を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="2f976-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="2f976-270">*Id*パラメーターは、URI のパスから取得し、*製品*パラメーターが要求本文から逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="2f976-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="2f976-271">既定では、ASP.NET Web API フレームワークは、ルートから単純なパラメーターの型と、要求本文から複合型を使用します。</span><span class="sxs-lookup"><span data-stu-id="2f976-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="2f976-272">リソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="2f976-272">Deleting a Resource</span></span>

<span data-ttu-id="2f976-273">Resourse を削除するには、[削除] メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="2f976-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="2f976-274">ステータスを記述するエンティティ本文を含むステータス 200 (OK) を返すことができます、DELETE 要求が成功すると、ステータス 202 (Accepted) 場合は、削除が保留中です。または 204 (No Content) エンティティ本文なしでのステータス。</span><span class="sxs-lookup"><span data-stu-id="2f976-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="2f976-275">ここで、`DeleteProduct`メソッドには、`void`型を返すため、ASP.NET Web API に自動的に変換このステータス コード 204 (No Content)。</span><span class="sxs-lookup"><span data-stu-id="2f976-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
