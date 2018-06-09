---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: アクションと ASP.NET Web API 2.2 を使用して OData v4 に関数 |Microsoft ドキュメント
author: MikeWasson
description: OData では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 このチュートリアルで説明する方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508231"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="1ddda-104">アクションと ASP.NET Web API 2.2 を使用して OData v4 に関数</span><span class="sxs-lookup"><span data-stu-id="1ddda-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="1ddda-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1ddda-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="1ddda-106">OData では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="1ddda-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="1ddda-107">このチュートリアルでは、Web API 2.2 を使用して、OData v4 エンドポイントをアクションと関数を追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="1ddda-108">このチュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用する ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="1ddda-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1ddda-109">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="1ddda-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1ddda-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="1ddda-110">Web API 2.2</span></span>
> - <span data-ttu-id="1ddda-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="1ddda-111">OData v4</span></span>
> - [<span data-ttu-id="1ddda-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="1ddda-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="1ddda-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="1ddda-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="1ddda-114">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="1ddda-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="1ddda-115">OData バージョン 3 では、次を参照してください。 [ASP.NET Web API 2 OData アクション](../odata-v3/odata-actions.md)です。</span><span class="sxs-lookup"><span data-stu-id="1ddda-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="1ddda-116">違い*アクション*と*関数*は関数は、アクションは、副作用があることができます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="1ddda-117">両方のアクションと関数は、データを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-117">Both actions and functions can return data.</span></span> <span data-ttu-id="1ddda-118">いくつかの操作の使用は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1ddda-118">Some uses for actions include:</span></span>

- <span data-ttu-id="1ddda-119">複雑なトランザクションです。</span><span class="sxs-lookup"><span data-stu-id="1ddda-119">Complex transactions.</span></span>
- <span data-ttu-id="1ddda-120">一度に複数のエンティティを操作します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="1ddda-121">エンティティの特定のプロパティにのみ更新を許可します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="1ddda-122">エンティティではないデータを送信しています。</span><span class="sxs-lookup"><span data-stu-id="1ddda-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="1ddda-123">関数は、エンティティまたはコレクションに直接対応していない情報を返すに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="1ddda-124">操作 (または関数) は、1 つのエンティティまたはコレクションを対象ことができます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="1ddda-125">これは、OData の用語で、*バインディング*です。</span><span class="sxs-lookup"><span data-stu-id="1ddda-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="1ddda-126">こともできます&quot;バインドされていない&quot;アクション/関数、サービス上の静的な操作としてと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="1ddda-127">例: アクションの追加</span><span class="sxs-lookup"><span data-stu-id="1ddda-127">Example: Adding an Action</span></span>

<span data-ttu-id="1ddda-128">製品を評価するアクションを定義してみましょう。</span><span class="sxs-lookup"><span data-stu-id="1ddda-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="1ddda-129">このチュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用する ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="1ddda-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="1ddda-130">最初に、追加、`ProductRating`モデルを評価を表します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="1ddda-131">追加も、 **DbSet**を`ProductsContext`クラス、EF はデータベースに評価テーブルを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1ddda-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="1ddda-132">EDM にアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-132">Add the Action to the EDM</span></span>

<span data-ttu-id="1ddda-133">WebApiConfig.cs では、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="1ddda-134">**EntityTypeConfiguration.Action**メソッドは、entity data model (EDM) にアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="1ddda-135">**パラメーター**メソッドは、アクションの型指定されたパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="1ddda-136">また、このコードは、EDM の名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="1ddda-137">名前空間が重要なは、アクションの URI には、アクションの完全修飾名が含まれているため。</span><span class="sxs-lookup"><span data-stu-id="1ddda-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="1ddda-138">一般的な IIS の構成では、この URL にドットと、IIS はエラー 404 が返されます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="1ddda-139">これは、Web.Config ファイルに次のセクションを追加することで解決できます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="1ddda-140">アクションのコント ローラーのメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="1ddda-141">有効にする、&quot;レート&quot;アクションには、次のメソッドを追加`ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="1ddda-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="1ddda-142">メソッド名が、アクションの名前と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1ddda-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="1ddda-143">**[HttpPost]** 属性は、メソッドが HTTP POST メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="1ddda-144">アクションを呼び出すには、クライアントは、次のように HTTP POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="1ddda-145">&quot;レート&quot;のため、操作の URI は、エンティティの URI に追加操作の完全修飾名、製品のインスタンスにアクションがバインドされます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="1ddda-146">(には、EDM 名前空間を設定おことに注意してください&quot;ProductService&quot;、アクションの完全修飾名が、 &quot;ProductService.Rate&quot;)。</span><span class="sxs-lookup"><span data-stu-id="1ddda-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="1ddda-147">要求の本文には、JSON ペイロードとしてのアクション パラメーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1ddda-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="1ddda-148">Web API では、JSON ペイロードを自動的に変換、 **ODataActionParameters**オブジェクトで、パラメーター値のディクショナリだけです。</span><span class="sxs-lookup"><span data-stu-id="1ddda-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="1ddda-149">コント ローラー メソッドでパラメーターにアクセスするのにには、このディクショナリを使用します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="1ddda-150">クライアントで間違ったアクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は false。</span><span class="sxs-lookup"><span data-stu-id="1ddda-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="1ddda-151">コント ローラー メソッドでは、このフラグをチェックし、エラーを返します**IsValid**は false。</span><span class="sxs-lookup"><span data-stu-id="1ddda-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="1ddda-152">例: 関数の追加</span><span class="sxs-lookup"><span data-stu-id="1ddda-152">Example: Adding a Function</span></span>

<span data-ttu-id="1ddda-153">これで最も高い製品を返す OData 関数を追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="1ddda-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="1ddda-154">ようにする前に、最初の手順は、EDM に関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="1ddda-155">WebApiConfig.cs では、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="1ddda-156">この場合、関数は、個々 の製品のインスタンスではなく製品コレクションにバインドされています。</span><span class="sxs-lookup"><span data-stu-id="1ddda-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="1ddda-157">クライアントは、GET 要求を送信することによって、関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="1ddda-158">この関数のコント ローラーのメソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="1ddda-159">メソッド名が、関数名と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1ddda-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="1ddda-160">**[HttpGet]** 属性は、メソッドが HTTP GET メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="1ddda-161">HTTP 応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="1ddda-162">例: バインドされていない関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="1ddda-163">前の例では、コレクションにバインドされた関数はでした。</span><span class="sxs-lookup"><span data-stu-id="1ddda-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="1ddda-164">次の例を作成して、*バインドされていない*関数。</span><span class="sxs-lookup"><span data-stu-id="1ddda-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="1ddda-165">バインドされていない関数は、サービス上の静的な操作として呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1ddda-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="1ddda-166">この例では、関数では、特定の郵便番号の売上税を返します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="1ddda-167">WebApiConfig ファイルでは、EDM に関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="1ddda-168">呼び出していることを確認**関数**上で直接、**ため**エンティティ型またはコレクションの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="1ddda-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="1ddda-169">これは、ビルダーに指示、モデル、関数はバインドされていることはありません。</span><span class="sxs-lookup"><span data-stu-id="1ddda-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="1ddda-170">関数を実装するコント ローラーのメソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="1ddda-171">どの Web API コント ローラーでこのメソッドを配置する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="1ddda-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="1ddda-172">内に配置できなかった`ProductsController`、または別のコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="1ddda-173">**[ODataRoute]** 属性は、関数の URI テンプレートを定義します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="1ddda-174">クライアント要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1ddda-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="1ddda-175">HTTP 応答:</span><span class="sxs-lookup"><span data-stu-id="1ddda-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
