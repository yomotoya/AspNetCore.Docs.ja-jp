---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "ASP.NET Web API 1 での CRUD 操作の有効化 |Microsoft ドキュメント"
author: MikeWasson
description: "このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスでの CRUD 操作をサポートする方法を示します。 ソフトウェアのバージョンがチュートリアルの Visual Studio 2012 Web AP で使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="73e41-104">ASP.NET Web API 1 での CRUD 操作を有効にします。</span><span class="sxs-lookup"><span data-stu-id="73e41-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="73e41-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="73e41-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="73e41-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="73e41-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="73e41-107">このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスでの CRUD 操作をサポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="73e41-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="73e41-108">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="73e41-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="73e41-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="73e41-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="73e41-110">Web API 1 (も Web API 2 で機能します)</span><span class="sxs-lookup"><span data-stu-id="73e41-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="73e41-111">CRUD&quot;作成、読み取り、更新、および Delete、&quot;は 4 つの基本的なデータベース操作します。</span><span class="sxs-lookup"><span data-stu-id="73e41-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="73e41-112">多くの HTTP サービスでは、REST または REST のような Api を介して CRUD 操作もモデルします。</span><span class="sxs-lookup"><span data-stu-id="73e41-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="73e41-113">このチュートリアルでは、非常に単純な web 製品の一覧を管理する API を構築します。</span><span class="sxs-lookup"><span data-stu-id="73e41-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="73e41-114">各製品は、名前、価格、およびカテゴリが含まれます (など&quot;toys&quot;または&quot;ハードウェア&quot;)、さらに、製品の id。</span><span class="sxs-lookup"><span data-stu-id="73e41-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="73e41-115">製品の API は、次のメソッドに公開します。</span><span class="sxs-lookup"><span data-stu-id="73e41-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="73e41-116">操作</span><span class="sxs-lookup"><span data-stu-id="73e41-116">Action</span></span> | <span data-ttu-id="73e41-117">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="73e41-117">HTTP method</span></span> | <span data-ttu-id="73e41-118">相対 URI</span><span class="sxs-lookup"><span data-stu-id="73e41-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73e41-119">すべての製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="73e41-119">Get a list of all products</span></span> | <span data-ttu-id="73e41-120">GET</span><span class="sxs-lookup"><span data-stu-id="73e41-120">GET</span></span> | <span data-ttu-id="73e41-121">製品/api</span><span class="sxs-lookup"><span data-stu-id="73e41-121">/api/products</span></span> |
| <span data-ttu-id="73e41-122">ID の製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="73e41-122">Get a product by ID</span></span> | <span data-ttu-id="73e41-123">GET</span><span class="sxs-lookup"><span data-stu-id="73e41-123">GET</span></span> | <span data-ttu-id="73e41-124">製品が/api/*id*</span><span class="sxs-lookup"><span data-stu-id="73e41-124">/api/products/*id*</span></span> |
| <span data-ttu-id="73e41-125">カテゴリによって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="73e41-125">Get a product by category</span></span> | <span data-ttu-id="73e41-126">GET</span><span class="sxs-lookup"><span data-stu-id="73e41-126">GET</span></span> | <span data-ttu-id="73e41-127">/api 製品ですか? カテゴリ =*カテゴリ*</span><span class="sxs-lookup"><span data-stu-id="73e41-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="73e41-128">新しい製品を作成します。</span><span class="sxs-lookup"><span data-stu-id="73e41-128">Create a new product</span></span> | <span data-ttu-id="73e41-129">投稿</span><span class="sxs-lookup"><span data-stu-id="73e41-129">POST</span></span> | <span data-ttu-id="73e41-130">製品/api</span><span class="sxs-lookup"><span data-stu-id="73e41-130">/api/products</span></span> |
| <span data-ttu-id="73e41-131">製品を更新します。</span><span class="sxs-lookup"><span data-stu-id="73e41-131">Update a product</span></span> | <span data-ttu-id="73e41-132">PUT</span><span class="sxs-lookup"><span data-stu-id="73e41-132">PUT</span></span> | <span data-ttu-id="73e41-133">製品が/api/*id*</span><span class="sxs-lookup"><span data-stu-id="73e41-133">/api/products/*id*</span></span> |
| <span data-ttu-id="73e41-134">製品を削除します。</span><span class="sxs-lookup"><span data-stu-id="73e41-134">Delete a product</span></span> | <span data-ttu-id="73e41-135">Del</span><span class="sxs-lookup"><span data-stu-id="73e41-135">DELETE</span></span> | <span data-ttu-id="73e41-136">製品が/api/*id*</span><span class="sxs-lookup"><span data-stu-id="73e41-136">/api/products/*id*</span></span> |

<span data-ttu-id="73e41-137">パスに製品 ID を含む Uri の一部に注意してください。</span><span class="sxs-lookup"><span data-stu-id="73e41-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="73e41-138">たとえば、製品 ID が 28 を取得するクライアント GET 要求を送信`http://hostname/api/products/28`です。</span><span class="sxs-lookup"><span data-stu-id="73e41-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="73e41-139">リソース</span><span class="sxs-lookup"><span data-stu-id="73e41-139">Resources</span></span>

<span data-ttu-id="73e41-140">製品の API は、2 種類のリソースの Uri を定義します。</span><span class="sxs-lookup"><span data-stu-id="73e41-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="73e41-141">リソース</span><span class="sxs-lookup"><span data-stu-id="73e41-141">Resource</span></span> | <span data-ttu-id="73e41-142">URI</span><span class="sxs-lookup"><span data-stu-id="73e41-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="73e41-143">すべての製品の一覧。</span><span class="sxs-lookup"><span data-stu-id="73e41-143">The list of all the products.</span></span> | <span data-ttu-id="73e41-144">製品/api</span><span class="sxs-lookup"><span data-stu-id="73e41-144">/api/products</span></span> |
| <span data-ttu-id="73e41-145">個々 の製品です。</span><span class="sxs-lookup"><span data-stu-id="73e41-145">An individual product.</span></span> | <span data-ttu-id="73e41-146">製品が/api/*id*</span><span class="sxs-lookup"><span data-stu-id="73e41-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="73e41-147">メソッド</span><span class="sxs-lookup"><span data-stu-id="73e41-147">Methods</span></span>

<span data-ttu-id="73e41-148">4 つの主要な HTTP メソッド (GET、PUT、POST、および削除) は、CRUD 操作にマップすることができます。</span><span class="sxs-lookup"><span data-stu-id="73e41-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="73e41-149">GET では、指定された URI にあるリソースの表現を取得します。</span><span class="sxs-lookup"><span data-stu-id="73e41-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="73e41-150">サーバーの副作用が GET はありません。</span><span class="sxs-lookup"><span data-stu-id="73e41-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="73e41-151">PUT は、指定された URI にリソースを更新します。</span><span class="sxs-lookup"><span data-stu-id="73e41-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="73e41-152">PUT こともできますを指定された URI で新しいリソースを作成する場合は、サーバーにより、クライアントが新しい Uri を指定します。</span><span class="sxs-lookup"><span data-stu-id="73e41-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="73e41-153">このチュートリアルでは、API では PUT による作成はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="73e41-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="73e41-154">投稿では、新しいリソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="73e41-154">POST creates a new resource.</span></span> <span data-ttu-id="73e41-155">サーバーは、新しいオブジェクトの URI が割り当てられ、応答メッセージの一部としてこの URI を返します。</span><span class="sxs-lookup"><span data-stu-id="73e41-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="73e41-156">指定された URI にリソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="73e41-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="73e41-157">注: PUT メソッドでは、全体の product エンティティを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="73e41-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="73e41-158">つまり、クライアントは、更新された製品の完全な表現を送信すると想定されます。</span><span class="sxs-lookup"><span data-stu-id="73e41-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="73e41-159">部分的な更新をサポートする場合は、PATCH メソッドをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="73e41-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="73e41-160">このチュートリアルでは、修正プログラムが実装されていません。</span><span class="sxs-lookup"><span data-stu-id="73e41-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="73e41-161">新しい Web API プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="73e41-161">Create a New Web API Project</span></span>

<span data-ttu-id="73e41-162">Visual Studio を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="73e41-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="73e41-163">またはから、**ファイル**メニューの **新規**し**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="73e41-164">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。</span><span class="sxs-lookup"><span data-stu-id="73e41-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="73e41-165">**Visual c#** **Web**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="73e41-166">プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="73e41-167">プロジェクトに名前を&quot;ProductStore&quot;  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="73e41-168">**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、 **Web API**  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="73e41-169">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e41-169">Adding a Model</span></span>

<span data-ttu-id="73e41-170">*モデル*は、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="73e41-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="73e41-171">ASP.NET Web API では、モデルとして厳密に型指定された CLR オブジェクトを使用することができ、自動的にシリアル化する XML または JSON をクライアントのします。</span><span class="sxs-lookup"><span data-stu-id="73e41-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="73e41-172">ProductStore API データで構成されている製品、という名前の新しいクラスを作成するように`Product`です。</span><span class="sxs-lookup"><span data-stu-id="73e41-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="73e41-173">ソリューション エクスプ ローラーが表示されない場合にクリックして、**ビュー**メニュー**ソリューション エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="73e41-174">ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="73e41-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="73e41-175">コンテキスト meny から選択**追加**選択してから、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="73e41-176">クラスの名前を付けます&quot;製品&quot;です。</span><span class="sxs-lookup"><span data-stu-id="73e41-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="73e41-177">次のプロパティを追加、`Product`クラスです。</span><span class="sxs-lookup"><span data-stu-id="73e41-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="73e41-178">リポジトリの追加</span><span class="sxs-lookup"><span data-stu-id="73e41-178">Adding a Repository</span></span>

<span data-ttu-id="73e41-179">製品のコレクションを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="73e41-179">We need to store a collection of products.</span></span> <span data-ttu-id="73e41-180">サービス実装からコレクションを分割することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="73e41-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="73e41-181">ようにして、バッキング ストア サービス クラスを書き直すことがなく変更できます。</span><span class="sxs-lookup"><span data-stu-id="73e41-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="73e41-182">この型のデザインが呼び出される、*リポジトリ*パターン。</span><span class="sxs-lookup"><span data-stu-id="73e41-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="73e41-183">リポジトリの汎用インターフェイスを定義することで開始します。</span><span class="sxs-lookup"><span data-stu-id="73e41-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="73e41-184">ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="73e41-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="73e41-185">選択**追加**選択してから、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="73e41-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="73e41-186">**テンプレート**ペインで、**インストールされたテンプレート**(C#) ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="73e41-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="73e41-187">C# の場合、で **コード**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-187">Under C#, select **Code**.</span></span> <span data-ttu-id="73e41-188">コード テンプレートの一覧で選択**インターフェイス**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="73e41-189">インターフェイスの名前&quot;IProductRepository&quot;です。</span><span class="sxs-lookup"><span data-stu-id="73e41-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="73e41-190">次の実装を追加します。</span><span class="sxs-lookup"><span data-stu-id="73e41-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="73e41-191">という名前のモデル フォルダーを別のクラスを今すぐ追加&quot;ProductRepository です。&quot;このクラスが `IProductRespository` インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="73e41-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="73e41-192">次の実装を追加します。</span><span class="sxs-lookup"><span data-stu-id="73e41-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="73e41-193">リポジトリでは、ローカル メモリの一覧が維持されます。</span><span class="sxs-lookup"><span data-stu-id="73e41-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="73e41-194">チュートリアルについては、[ok] をこれが実際のアプリケーションでは、データを格納する外部で、データベースであるか、またはクラウド ストレージにします。</span><span class="sxs-lookup"><span data-stu-id="73e41-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="73e41-195">リポジトリ パターンが容易にできるように実装を後で変更します。</span><span class="sxs-lookup"><span data-stu-id="73e41-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="73e41-196">Web API コント ローラーの追加</span><span class="sxs-lookup"><span data-stu-id="73e41-196">Adding a Web API Controller</span></span>

<span data-ttu-id="73e41-197">ASP.NET MVC を使用する場合、し、既に慣れているコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="73e41-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="73e41-198">ASP.NET Web API で、*コント ローラー*クライアントから HTTP 要求を処理するクラスです。</span><span class="sxs-lookup"><span data-stu-id="73e41-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="73e41-199">プロジェクトが作成されたときに、新しいプロジェクト ウィザードによって次の 2 つのコント ローラーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="73e41-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="73e41-200">これらを参照するには、ソリューション エクスプ ローラーで Controllers フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="73e41-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="73e41-201">テンプレートを使用するとは、従来の ASP.NET MVC コント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="73e41-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="73e41-202">サイトの HTML ページの要求を処理して、web API に直接関連しません。</span><span class="sxs-lookup"><span data-stu-id="73e41-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="73e41-203">ValuesController は、例 WebAPI コント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="73e41-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="73e41-204">ソリューション エクスプ ローラーでファイルを右クリックして、ValuesController を削除してください**を削除します。**</span><span class="sxs-lookup"><span data-stu-id="73e41-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="73e41-205">今すぐよう、新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e41-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="73e41-206">**ソリューション エクスプ ローラー**を右クリックし、コント ローラーのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="73e41-206">In **Solution Explorer**, right-click the the Controllers folder.</span></span> <span data-ttu-id="73e41-207">選択**追加**し、**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="73e41-208">**コント ローラーの追加**ウィザードで、名前、コント ローラー &quot;ProductsController&quot;です。</span><span class="sxs-lookup"><span data-stu-id="73e41-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="73e41-209">**テンプレート**ドロップダウン リストで、**空の API コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="73e41-210">次に、 **[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="73e41-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="73e41-211">コント ローラーをという名前のフォルダーに、コントを配置する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="73e41-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="73e41-212">フォルダー名が重要です。ソース ファイルを整理する便利な方法では単純にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="73e41-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="73e41-213">**コント ローラーの追加**ProductsController.cs Controllers フォルダーをという名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="73e41-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="73e41-214">このファイルがまだ開いていないを開くには、ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="73e41-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="73e41-215">次の追加**を使用して**ステートメント。</span><span class="sxs-lookup"><span data-stu-id="73e41-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="73e41-216">格納するフィールドを追加、 **IProductRepository**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="73e41-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="73e41-217">呼び出す`new ProductRepository()`コント ローラーには最善の設計では、コント ローラーの特定の実装を結び付けるため`IProductRepository`です。</span><span class="sxs-lookup"><span data-stu-id="73e41-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="73e41-218">適切な方法では、次を参照してください。 [Web API の依存関係競合回避モジュールを使用して](../advanced/dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="73e41-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="73e41-219">リソースの取得</span><span class="sxs-lookup"><span data-stu-id="73e41-219">Getting a Resource</span></span>

<span data-ttu-id="73e41-220">ProductStore API では、いくつかは公開&quot;読み取り&quot;HTTP GET メソッドとアクション。</span><span class="sxs-lookup"><span data-stu-id="73e41-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="73e41-221">各アクション メソッドに対応させるには、`ProductsController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="73e41-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="73e41-222">操作</span><span class="sxs-lookup"><span data-stu-id="73e41-222">Action</span></span> | <span data-ttu-id="73e41-223">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="73e41-223">HTTP method</span></span> | <span data-ttu-id="73e41-224">相対 URI</span><span class="sxs-lookup"><span data-stu-id="73e41-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73e41-225">すべての製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="73e41-225">Get a list of all products</span></span> | <span data-ttu-id="73e41-226">GET</span><span class="sxs-lookup"><span data-stu-id="73e41-226">GET</span></span> | <span data-ttu-id="73e41-227">製品/api</span><span class="sxs-lookup"><span data-stu-id="73e41-227">/api/products</span></span> |
| <span data-ttu-id="73e41-228">ID の製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="73e41-228">Get a product by ID</span></span> | <span data-ttu-id="73e41-229">GET</span><span class="sxs-lookup"><span data-stu-id="73e41-229">GET</span></span> | <span data-ttu-id="73e41-230">製品が/api/*id*</span><span class="sxs-lookup"><span data-stu-id="73e41-230">/api/products/*id*</span></span> |
| <span data-ttu-id="73e41-231">カテゴリによって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="73e41-231">Get a product by category</span></span> | <span data-ttu-id="73e41-232">GET</span><span class="sxs-lookup"><span data-stu-id="73e41-232">GET</span></span> | <span data-ttu-id="73e41-233">/api 製品ですか? カテゴリ =*カテゴリ*</span><span class="sxs-lookup"><span data-stu-id="73e41-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="73e41-234">すべての製品の一覧を取得するには、このメソッドは追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="73e41-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="73e41-235">メソッドの名前が始まる&quot;取得&quot;慣例 GET 要求にマップするため、します。</span><span class="sxs-lookup"><span data-stu-id="73e41-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="73e41-236">また、メソッドにパラメーターがあるないためにマップを含まない URI、  *&quot;id&quot;* パス内のセグメント。</span><span class="sxs-lookup"><span data-stu-id="73e41-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="73e41-237">ID によって製品を入手するには、このメソッドは追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="73e41-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="73e41-238">このメソッドの名前からも始まります&quot;取得&quot;、メソッドがという名前のパラメーターが、 *id*です。このパラメーターにマップされて、 &quot;id&quot; URI パスのセグメント。</span><span class="sxs-lookup"><span data-stu-id="73e41-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="73e41-239">ASP.NET Web API フレームワークが、ID を正しいデータ型に自動的に変換 (**int**) のパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="73e41-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="73e41-240">GetProduct メソッド型の例外をスロー **HttpResponseException**場合*id*が無効です。</span><span class="sxs-lookup"><span data-stu-id="73e41-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="73e41-241">この例外は、フレームワークによって 404 (Not Found) エラーに変換されます。</span><span class="sxs-lookup"><span data-stu-id="73e41-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="73e41-242">最後に、カテゴリによって製品を検索するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e41-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="73e41-243">要求 URI にクエリ文字列がある場合は、Web API では、コント ローラーのメソッドのパラメーターにクエリ パラメーターを照合します。</span><span class="sxs-lookup"><span data-stu-id="73e41-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="73e41-244">そのため、フォームの URI"api/製品ですか? カテゴリ =*カテゴリ*"このメソッドにマップされます。</span><span class="sxs-lookup"><span data-stu-id="73e41-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="73e41-245">リソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="73e41-245">Creating a Resource</span></span>

<span data-ttu-id="73e41-246">次に、追加する方法、`ProductsController`新しい製品を作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="73e41-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="73e41-247">メソッドの単純な実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="73e41-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="73e41-248">このメソッドの 2 つの点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="73e41-248">Note two things about this method:</span></span>

- <span data-ttu-id="73e41-249">メソッドの名前が始まる&quot;Post しています.&quot;.</span><span class="sxs-lookup"><span data-stu-id="73e41-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="73e41-250">新しい製品を作成するには、クライアントは、HTTP POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="73e41-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="73e41-251">メソッドは、製品の種類のパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="73e41-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="73e41-252">Web API では、複合型を持つパラメーターが要求本文から逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="73e41-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="73e41-253">したがって、XML または JSON のいずれかの形式で、製品オブジェクトのシリアル化された表現を送信するクライアントを見込んでいます。</span><span class="sxs-lookup"><span data-stu-id="73e41-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="73e41-254">この実装は機能しますが、完全なものはありません。</span><span class="sxs-lookup"><span data-stu-id="73e41-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="73e41-255">理想的には、HTTP 応答、以下を含むましょう。</span><span class="sxs-lookup"><span data-stu-id="73e41-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="73e41-256">**応答コード:**既定では、Web API フレームワークは 200 (OK) に応答のステータス コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="73e41-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="73e41-257">ただし、http/1.1 プロトコルに従って、POST 要求は、リソースの作成時に発生すると、サーバーという応答が 201 (Created) の状態。</span><span class="sxs-lookup"><span data-stu-id="73e41-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="73e41-258">**場所:**応答の Location ヘッダーの新しいリソースの URI を含める必要があります、サーバーは、リソースを作成したときにします。</span><span class="sxs-lookup"><span data-stu-id="73e41-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="73e41-259">ASP.NET Web API 簡単を HTTP 応答メッセージを操作できます。</span><span class="sxs-lookup"><span data-stu-id="73e41-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="73e41-260">強化された実装を次に示します。</span><span class="sxs-lookup"><span data-stu-id="73e41-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="73e41-261">メソッドの戻り値の型は、今すぐ通知**HttpResponseMessage**です。</span><span class="sxs-lookup"><span data-stu-id="73e41-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="73e41-262">返すことによって、 **HttpResponseMessage**製品の代わりには、ステータス コードと Location ヘッダーを含む、HTTP 応答メッセージの詳細を制御できます。</span><span class="sxs-lookup"><span data-stu-id="73e41-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="73e41-263">**CreateResponse**メソッドを作成、 **HttpResponseMessage**と本文に Product オブジェクトのシリアル化された表現を自動的に書き込み fo 応答メッセージ。</span><span class="sxs-lookup"><span data-stu-id="73e41-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="73e41-264">この例では検証されません、`Product`です。</span><span class="sxs-lookup"><span data-stu-id="73e41-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="73e41-265">モデルの検証方法については、次を参照してください。 [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="73e41-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="73e41-266">リソースの更新</span><span class="sxs-lookup"><span data-stu-id="73e41-266">Updating a Resource</span></span>

<span data-ttu-id="73e41-267">Put の製品を更新することは簡単です。</span><span class="sxs-lookup"><span data-stu-id="73e41-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="73e41-268">メソッドの名前が始まる&quot;を配置しています.&quot;ので、Web API が PUT 要求に一致します。</span><span class="sxs-lookup"><span data-stu-id="73e41-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="73e41-269">メソッドは、2 つのパラメーター、製品 ID と製品の更新を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="73e41-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="73e41-270">*Id*パラメーターが URI パスから取得され、*製品*パラメーターが要求本文から逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="73e41-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="73e41-271">既定では、ASP.NET Web API フレームワークは、ルートからの単純なパラメーターの型と、要求本文の複合型はクリックします。</span><span class="sxs-lookup"><span data-stu-id="73e41-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="73e41-272">リソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="73e41-272">Deleting a Resource</span></span>

<span data-ttu-id="73e41-273">Resourse を削除するには、「削除...」メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="73e41-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="73e41-274">ステータスを記述するエンティティ本体を持つステータス 200 (OK) を返すことができます削除要求が成功すると、ステータス 202 (Accepted) 場合は、削除が保留中です。または、204 (No Content) エンティティ本文なしでの状態。</span><span class="sxs-lookup"><span data-stu-id="73e41-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="73e41-275">ここで、`DeleteProduct`メソッドには、`void`型を返すには、ため、ASP.NET Web API に自動的に変換このステータス コード 204 (No Content)。</span><span class="sxs-lookup"><span data-stu-id="73e41-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
