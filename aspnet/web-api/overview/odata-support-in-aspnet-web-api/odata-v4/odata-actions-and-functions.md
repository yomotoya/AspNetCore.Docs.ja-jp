---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: アクションと ASP.NET Web API 2.2 を使用して OData v4 の関数 |Microsoft Docs
author: MikeWasson
description: Odata では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 このチュートリアルでは方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: c78a9acabcb346b33a4fe53aa0d18ef67ddcaf33
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371964"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="acb39-104">アクションと ASP.NET Web API 2.2 を使用して OData v4 の関数</span><span class="sxs-lookup"><span data-stu-id="acb39-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="acb39-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="acb39-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="acb39-106">Odata では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="acb39-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="acb39-107">このチュートリアルでは、アクションと関数を Web API 2.2 を使用して、OData v4 エンドポイントを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="acb39-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="acb39-108">チュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="acb39-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="acb39-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="acb39-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="acb39-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="acb39-110">Web API 2.2</span></span>
> - <span data-ttu-id="acb39-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="acb39-111">OData v4</span></span>
> - [<span data-ttu-id="acb39-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="acb39-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="acb39-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="acb39-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="acb39-114">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="acb39-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="acb39-115">OData バージョン 3 では、次を参照してください。[の ASP.NET Web API 2 OData アクション](../odata-v3/odata-actions.md)します。</span><span class="sxs-lookup"><span data-stu-id="acb39-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="acb39-116">間の差*アクション*と*関数*アクションは、副作用を持つことができます、関数がないことです。</span><span class="sxs-lookup"><span data-stu-id="acb39-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="acb39-117">アクションと関数の両方では、データを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="acb39-117">Both actions and functions can return data.</span></span> <span data-ttu-id="acb39-118">アクションの使用は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="acb39-118">Some uses for actions include:</span></span>

- <span data-ttu-id="acb39-119">複雑なトランザクション。</span><span class="sxs-lookup"><span data-stu-id="acb39-119">Complex transactions.</span></span>
- <span data-ttu-id="acb39-120">一度に複数のエンティティを操作します。</span><span class="sxs-lookup"><span data-stu-id="acb39-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="acb39-121">エンティティの特定のプロパティにのみ更新できるようにします。</span><span class="sxs-lookup"><span data-stu-id="acb39-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="acb39-122">エンティティではないデータを送信します。</span><span class="sxs-lookup"><span data-stu-id="acb39-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="acb39-123">関数は、エンティティまたはコレクションに直接対応しない情報を返す場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="acb39-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="acb39-124">操作 (または関数) は、1 つのエンティティまたはコレクションを対象ことができます。</span><span class="sxs-lookup"><span data-stu-id="acb39-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="acb39-125">これは、OData の用語では、*バインド*します。</span><span class="sxs-lookup"><span data-stu-id="acb39-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="acb39-126">こともできます&quot;バインドされていない&quot;アクション/関数、サービスで静的な操作として呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="acb39-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="acb39-127">例: アクションの追加</span><span class="sxs-lookup"><span data-stu-id="acb39-127">Example: Adding an Action</span></span>

<span data-ttu-id="acb39-128">製品を評価するアクションを定義してみましょう。</span><span class="sxs-lookup"><span data-stu-id="acb39-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="acb39-129">このチュートリアルでは、チュートリアルを[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="acb39-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="acb39-130">最初に、追加、`ProductRating`モデルを評価を表します。</span><span class="sxs-lookup"><span data-stu-id="acb39-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="acb39-131">追加も、 **DbSet**を`ProductsContext`クラス、EF はデータベースの評価テーブルを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="acb39-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="acb39-132">EDM にアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-132">Add the Action to the EDM</span></span>

<span data-ttu-id="acb39-133">WebApiConfig.cs で、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="acb39-134">**EntityTypeConfiguration.Action**メソッドは、entity data model (EDM) にアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="acb39-135">**パラメーター**メソッドは、アクションの型指定されたパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="acb39-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="acb39-136">また、このコードは、EDM の名前空間を設定します。</span><span class="sxs-lookup"><span data-stu-id="acb39-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="acb39-137">名前空間には、アクションの URI には、アクションの完全修飾名が含まれているためにが重要です。</span><span class="sxs-lookup"><span data-stu-id="acb39-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="acb39-138">一般的な IIS 構成でこの URL にドットがエラー 404 を返す IIS が発生します。</span><span class="sxs-lookup"><span data-stu-id="acb39-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="acb39-139">これは、Web.Config ファイルに次のセクションを追加することで解決できます。</span><span class="sxs-lookup"><span data-stu-id="acb39-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="acb39-140">アクションのコント ローラー メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="acb39-141">有効にする、&quot;レート&quot;アクションでは、次のメソッドを追加`ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="acb39-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="acb39-142">メソッド名が、アクションの名前と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="acb39-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="acb39-143">**[HttpPost]** 属性は、メソッドが HTTP POST メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="acb39-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="acb39-144">アクションを呼び出すクライアントは、次のような HTTP POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="acb39-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="acb39-145">&quot;レート&quot;アクションの URI は、エンティティの URI に追加されたアクションの完全修飾名、製品のインスタンスへのアクションがバインドされます。</span><span class="sxs-lookup"><span data-stu-id="acb39-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="acb39-146">(に EDM 名前空間を設定することを思い出してください&quot;ProductService&quot;でアクションの完全修飾名があるため、 &quot;ProductService.Rate&quot;)。</span><span class="sxs-lookup"><span data-stu-id="acb39-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="acb39-147">要求の本文には、JSON ペイロードとしてのアクション パラメーターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="acb39-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="acb39-148">Web API に JSON ペイロードを自動的に変換する、 **ODataActionParameters**オブジェクトで、パラメーター値のディクショナリのみです。</span><span class="sxs-lookup"><span data-stu-id="acb39-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="acb39-149">コント ローラー メソッドでパラメーターにアクセスするのにには、このディクショナリを使用します。</span><span class="sxs-lookup"><span data-stu-id="acb39-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="acb39-150">クライアントが正しくないアクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は false です。</span><span class="sxs-lookup"><span data-stu-id="acb39-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="acb39-151">コント ローラー メソッドでは、このフラグをチェックし、場合は、エラーを返します**IsValid**は false です。</span><span class="sxs-lookup"><span data-stu-id="acb39-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="acb39-152">例: 関数の追加</span><span class="sxs-lookup"><span data-stu-id="acb39-152">Example: Adding a Function</span></span>

<span data-ttu-id="acb39-153">これで、最も高価な製品が返す OData 関数を追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="acb39-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="acb39-154">ようにする前に、最初の手順は、EDM に関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="acb39-155">WebApiConfig.cs では、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="acb39-156">この場合、関数は、個々 の製品のインスタンスではなく、製品のコレクションにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="acb39-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="acb39-157">クライアントは、GET 要求を送信することによって、関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="acb39-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="acb39-158">この関数のコント ローラー メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="acb39-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="acb39-159">メソッド名に関数名と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="acb39-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="acb39-160">**[HttpGet]** 属性は、メソッドが HTTP GET メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="acb39-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="acb39-161">HTTP 応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="acb39-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="acb39-162">例: バインドされていない、関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="acb39-163">前の例では、コレクションにバインドされている関数をしました。</span><span class="sxs-lookup"><span data-stu-id="acb39-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="acb39-164">この次の例で作成します、*バインドされていない*関数。</span><span class="sxs-lookup"><span data-stu-id="acb39-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="acb39-165">バインドされていない関数は、サービスで静的な操作として呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="acb39-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="acb39-166">この例では、関数では、特定の郵便番号の税抜きを返します。</span><span class="sxs-lookup"><span data-stu-id="acb39-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="acb39-167">WebApiConfig ファイルでは、EDM に関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="acb39-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="acb39-168">呼び出すことに注意してください**関数**上で直接、**に**エンティティ型またはコレクションの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="acb39-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="acb39-169">これにより、関数がバインドであるモデル ビルダー。</span><span class="sxs-lookup"><span data-stu-id="acb39-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="acb39-170">関数を実装するコント ローラー メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="acb39-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="acb39-171">Web API コント ローラーでこのメソッドを配置する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="acb39-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="acb39-172">配置する`ProductsController`、または別のコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="acb39-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="acb39-173">**[ODataRoute]** 属性は、関数の URI テンプレートを定義します。</span><span class="sxs-lookup"><span data-stu-id="acb39-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="acb39-174">クライアント要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="acb39-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="acb39-175">HTTP 応答:</span><span class="sxs-lookup"><span data-stu-id="acb39-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
