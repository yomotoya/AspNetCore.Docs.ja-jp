---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: ルーティングと ASP.NET Web API の操作の選択 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043419"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="663fd-102">ルーティングと ASP.NET Web API の操作の選択</span><span class="sxs-lookup"><span data-stu-id="663fd-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="663fd-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="663fd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="663fd-104">この記事では、ASP.NET Web API がコント ローラー上の特定のアクションに HTTP 要求をルーティングする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="663fd-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="663fd-105">ルーティングの大まかな概要については、次を参照してください。 [ASP.NET Web API でルーティング](routing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="663fd-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="663fd-106">この記事は、ルーティング処理の詳細を検索します。</span><span class="sxs-lookup"><span data-stu-id="663fd-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="663fd-107">Web API プロジェクトを作成すると、いくつかの要求を取得しない検索期待どおりのルーティング、できればこの記事が役立ちます。</span><span class="sxs-lookup"><span data-stu-id="663fd-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="663fd-108">ルーティングと、次の 3 つの主要なフェーズがあります。</span><span class="sxs-lookup"><span data-stu-id="663fd-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="663fd-109">ルート テンプレートを URI が一致します。</span><span class="sxs-lookup"><span data-stu-id="663fd-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="663fd-110">コント ローラーを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-110">Selecting a controller.</span></span>
3. <span data-ttu-id="663fd-111">アクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-111">Selecting an action.</span></span>

<span data-ttu-id="663fd-112">独自のカスタム ビヘイビアーの使用、プロセスの一部を置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="663fd-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="663fd-113">この記事では、既定の動作を説明します。</span><span class="sxs-lookup"><span data-stu-id="663fd-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="663fd-114">最後に、動作をカスタマイズできる場所には注意してください。</span><span class="sxs-lookup"><span data-stu-id="663fd-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="663fd-115">ルート テンプレート</span><span class="sxs-lookup"><span data-stu-id="663fd-115">Route Templates</span></span>

<span data-ttu-id="663fd-116">ルート テンプレートのような URI パスが、中かっこで示されているプレース ホルダーの値を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="663fd-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="663fd-117">ルートを作成するときに、プレース ホルダーの一部またはすべての既定値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="663fd-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="663fd-118">URI のセグメントがプレース ホルダーを照合する方法を制限する制約を指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="663fd-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="663fd-119">フレームワークでは、テンプレートへの URI パスのセグメントを照合します。</span><span class="sxs-lookup"><span data-stu-id="663fd-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="663fd-120">テンプレート内のリテラルは、正確に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="663fd-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="663fd-121">プレース ホルダーは制約を指定していない限り、任意の値と一致します。</span><span class="sxs-lookup"><span data-stu-id="663fd-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="663fd-122">フレームワークでは、URI、ホスト名またはクエリのパラメーターなどの他の部分が一致しません。</span><span class="sxs-lookup"><span data-stu-id="663fd-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="663fd-123">フレームワークは、URI に一致するルート テーブルの最初のルートを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="663fd-124">2 つの特殊なプレース ホルダーがある:"{controller}"と"{action}"。</span><span class="sxs-lookup"><span data-stu-id="663fd-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="663fd-125">"{controller}"は、コント ローラーの名前を提供します。</span><span class="sxs-lookup"><span data-stu-id="663fd-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="663fd-126">"{action}"は、アクションの名前を提供します。</span><span class="sxs-lookup"><span data-stu-id="663fd-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="663fd-127">Web API では、通常の規則は、"{action}"を省略する場合です。</span><span class="sxs-lookup"><span data-stu-id="663fd-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="663fd-128">既定値</span><span class="sxs-lookup"><span data-stu-id="663fd-128">Defaults</span></span>

<span data-ttu-id="663fd-129">既定値を指定する場合、ルートはこれらのセグメントが欠落している URI に一致します。</span><span class="sxs-lookup"><span data-stu-id="663fd-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="663fd-130">例:</span><span class="sxs-lookup"><span data-stu-id="663fd-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="663fd-131">URI"`http://localhost/api/products`"このルートと一致します。</span><span class="sxs-lookup"><span data-stu-id="663fd-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="663fd-132">「{カテゴリ}」セグメントには、"all"の既定値が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="663fd-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="663fd-133">ルート ディクショナリ</span><span class="sxs-lookup"><span data-stu-id="663fd-133">Route Dictionary</span></span>

<span data-ttu-id="663fd-134">フレームワークでは、URI の一致が見つかると、各プレース ホルダーの値を格納するディクショナリを作成します。</span><span class="sxs-lookup"><span data-stu-id="663fd-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="663fd-135">キーは、中かっこを含まない、プレース ホルダー名です。</span><span class="sxs-lookup"><span data-stu-id="663fd-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="663fd-136">値は、URI のパスとは、既定値から取得されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="663fd-137">ディクショナリが格納されている、 **IHttpRouteData**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="663fd-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="663fd-138">このルートの一致フェーズ中に、特別な"{controller}"と"{action}"プレース ホルダーは、他のプレース ホルダーと同じように扱われます。</span><span class="sxs-lookup"><span data-stu-id="663fd-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="663fd-139">単にその他の値を使用してディクショナリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="663fd-140">既定値は、特殊な値を持つことができます**RouteParameter.Optional**です。</span><span class="sxs-lookup"><span data-stu-id="663fd-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="663fd-141">プレース ホルダーには、この値が割り当てられているを取得する場合、値はルート ディクショナリに追加されません。</span><span class="sxs-lookup"><span data-stu-id="663fd-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="663fd-142">例:</span><span class="sxs-lookup"><span data-stu-id="663fd-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="663fd-143">"Api/products"などの URI パスに対してルート ディクショナリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="663fd-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="663fd-144">コント ローラー:"products など"</span><span class="sxs-lookup"><span data-stu-id="663fd-144">controller: "products"</span></span>
- <span data-ttu-id="663fd-145">カテゴリ:"all"</span><span class="sxs-lookup"><span data-stu-id="663fd-145">category: "all"</span></span>

<span data-ttu-id="663fd-146">「Api/製品/toys/123」、ただし、ルート ディクショナリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="663fd-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="663fd-147">コント ローラー:"products など"</span><span class="sxs-lookup"><span data-stu-id="663fd-147">controller: "products"</span></span>
- <span data-ttu-id="663fd-148">category: "toys"</span><span class="sxs-lookup"><span data-stu-id="663fd-148">category: "toys"</span></span>
- <span data-ttu-id="663fd-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="663fd-149">id: "123"</span></span>

<span data-ttu-id="663fd-150">既定値は、ルート テンプレートで任意の場所に表示されていない値を含めることができますも。</span><span class="sxs-lookup"><span data-stu-id="663fd-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="663fd-151">一致する場合は、その値がディクショナリに格納します。</span><span class="sxs-lookup"><span data-stu-id="663fd-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="663fd-152">例:</span><span class="sxs-lookup"><span data-stu-id="663fd-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="663fd-153">URI のパスが「api/ルート/8」の場合は、2 つの値がディクショナリに含まれます。</span><span class="sxs-lookup"><span data-stu-id="663fd-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="663fd-154">コント ローラー:"customers"</span><span class="sxs-lookup"><span data-stu-id="663fd-154">controller: "customers"</span></span>
- <span data-ttu-id="663fd-155">id: "8"</span><span class="sxs-lookup"><span data-stu-id="663fd-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="663fd-156">コント ローラーを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-156">Selecting a Controller</span></span>

<span data-ttu-id="663fd-157">コント ローラーの選択はによって処理される、 **IHttpControllerSelector.SelectController**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="663fd-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="663fd-158">このメソッドは、 **HttpRequestMessage**インスタンスを返す、 **HttpControllerDescriptor**です。</span><span class="sxs-lookup"><span data-stu-id="663fd-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="663fd-159">によって既定の実装が提供される、 **DefaultHttpControllerSelector**クラスです。</span><span class="sxs-lookup"><span data-stu-id="663fd-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="663fd-160">このクラスは、単純なアルゴリズムを使用します。</span><span class="sxs-lookup"><span data-stu-id="663fd-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="663fd-161">キー"controller"のルート ディクショナリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="663fd-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="663fd-162">このキーの値を取得し、文字列、コント ローラーの種類名を取得するには、"Controller"を追加します。</span><span class="sxs-lookup"><span data-stu-id="663fd-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="663fd-163">Web API コント ローラーではこの型の名前を探します。</span><span class="sxs-lookup"><span data-stu-id="663fd-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="663fd-164">たとえば、ルート ディクショナリに"controller"キー/値ペアが含まれている場合 ="products"など、コント ローラーの種類が"ProductsController"します。</span><span class="sxs-lookup"><span data-stu-id="663fd-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="663fd-165">一致する型がない、または複数の一致がある場合、フレームワークは、クライアントにエラーを返します。</span><span class="sxs-lookup"><span data-stu-id="663fd-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="663fd-166">手順 3. の**DefaultHttpControllerSelector**を使用して、 **IHttpControllerTypeResolver** Web API コント ローラーの種類の一覧を取得するインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="663fd-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="663fd-167">既定の実装**IHttpControllerTypeResolver** (a) 実装するすべてのパブリック クラスを返します**IHttpController**、(b) は抽象化、されず (c)"Controller"で終わる名前であります。</span><span class="sxs-lookup"><span data-stu-id="663fd-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="663fd-168">操作の選択</span><span class="sxs-lookup"><span data-stu-id="663fd-168">Action Selection</span></span>

<span data-ttu-id="663fd-169">コント ローラーを選択すると、フレームワークは呼び出すことによって、操作を選択、 **IHttpActionSelector.SelectAction**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="663fd-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="663fd-170">このメソッドは、 **HttpControllerContext**を返します、 **HttpActionDescriptor**です。</span><span class="sxs-lookup"><span data-stu-id="663fd-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="663fd-171">によって既定の実装が提供される、 **ApiControllerActionSelector**クラスです。</span><span class="sxs-lookup"><span data-stu-id="663fd-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="663fd-172">アクションを選択するには、次を検索します。</span><span class="sxs-lookup"><span data-stu-id="663fd-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="663fd-173">要求の HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="663fd-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="663fd-174">ルート テンプレートで、存在する場合、「{アクション}」プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="663fd-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="663fd-175">コント ローラーのアクションのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="663fd-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="663fd-176">選択アルゴリズムを見る前に、コント ローラー アクションのいくつかの点を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="663fd-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="663fd-177">**コント ローラーのメソッドは、「アクション」と見なされますか。**</span><span class="sxs-lookup"><span data-stu-id="663fd-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="663fd-178">アクションを選択すると、フレームワークのみではパブリック インスタンス メソッド、コント ローラーでします。</span><span class="sxs-lookup"><span data-stu-id="663fd-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="663fd-179">また、除外[「特別な名前」](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname)メソッド (コンス トラクター、イベント、演算子のオーバー ロードなど)、およびから継承されたメソッド、 **ApiController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="663fd-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="663fd-180">**HTTP メソッド。**</span><span class="sxs-lookup"><span data-stu-id="663fd-180">**HTTP Methods.**</span></span> <span data-ttu-id="663fd-181">のみ、フレームワークには、次のように、要求の HTTP メソッドと一致するアクションが選択されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="663fd-182">属性を持つ HTTP メソッドを指定できます: **AcceptVerbs**、 **HttpDelete**、 **HttpGet**、 **HttpHead**、 **HttpOptions**、 **HttpPatch**、 **HttpPost**、または**HttpPut**です。</span><span class="sxs-lookup"><span data-stu-id="663fd-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="663fd-183">それ以外の場合、コント ローラーのメソッドの名前は、"Get"、"Post"、"Put"、"Delete"、"Head"、「オプション」または「修正」で始まる場合、規則によって、アクションがサポートする HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="663fd-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="663fd-184">上記のいずれの場合、メソッドは POST をサポートします。</span><span class="sxs-lookup"><span data-stu-id="663fd-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="663fd-185">**パラメーター バインディング。**</span><span class="sxs-lookup"><span data-stu-id="663fd-185">**Parameter Bindings.**</span></span> <span data-ttu-id="663fd-186">パラメーターのバインドは、Web API が、パラメーターの値を作成する方法です。</span><span class="sxs-lookup"><span data-stu-id="663fd-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="663fd-187">パラメーター バインディングの既定の規則を次に示します。</span><span class="sxs-lookup"><span data-stu-id="663fd-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="663fd-188">単純型は、URI から取得されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="663fd-189">複合型は、要求本文から取得されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="663fd-190">単純型には、すべてが含まれている、 [.NET Framework のプリミティブ型](https://msdn.microsoft.com/library/system.type.isprimitive)、plus **DateTime**、 **Decimal**、 **Guid**、**文字列**、および**TimeSpan**です。</span><span class="sxs-lookup"><span data-stu-id="663fd-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="663fd-191">アクションごとに最大で 1 つのパラメーターは、要求本文を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="663fd-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="663fd-192">既定のバインディング規則を上書きすることは。</span><span class="sxs-lookup"><span data-stu-id="663fd-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="663fd-193">参照してください[フードの下部 WebAPI パラメーター バインド](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="663fd-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="663fd-194">その背景には、アクションの選択アルゴリズムを次に示します。</span><span class="sxs-lookup"><span data-stu-id="663fd-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="663fd-195">HTTP 要求メソッドに一致するコント ローラーのすべてのアクションの一覧を作成します。</span><span class="sxs-lookup"><span data-stu-id="663fd-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="663fd-196">ルート ディクショナリに、"action"エントリがある場合は、名前を持つが、この値と一致しませんアクションを削除します。</span><span class="sxs-lookup"><span data-stu-id="663fd-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="663fd-197">次のように、uri、アクション パラメーターを一致するように再試行してください。</span><span class="sxs-lookup"><span data-stu-id="663fd-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="663fd-198">各アクションの単純型の場合は、バインディングが、URI からパラメーターを取得するパラメーターの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="663fd-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="663fd-199">省略可能なパラメーターを除外します。</span><span class="sxs-lookup"><span data-stu-id="663fd-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="663fd-200">この一覧から、ルート ディクショナリまたは URI クエリ文字列内に各パラメーターの名前の一致を見つけようとします。</span><span class="sxs-lookup"><span data-stu-id="663fd-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="663fd-201">一致は、大文字と小文字パラメーターの順序に依存しません。</span><span class="sxs-lookup"><span data-stu-id="663fd-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="663fd-202">リスト内のすべてのパラメーターが URI で一致するものがアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="663fd-203">複数のアクションがこれらの条件を満たしている場合は、ほとんどのパラメーターと一致すると、1 つを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="663fd-204">使って操作を無視する、 **[NonAction]** 属性。</span><span class="sxs-lookup"><span data-stu-id="663fd-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="663fd-205">手順 3 では最も難しい可能性があります。</span><span class="sxs-lookup"><span data-stu-id="663fd-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="663fd-206">基本的な考え方は、パラメーターは、URI、要求本文からまたはカスタム バインドからはその値を取得できます。</span><span class="sxs-lookup"><span data-stu-id="663fd-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="663fd-207">URI に由来するパラメーターの場合、URI が (ルート ディクショナリ) 経由でパスまたはクエリ文字列にそのパラメーターの値には実際に含まれていることを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="663fd-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="663fd-208">たとえば、次の操作があるとします。</span><span class="sxs-lookup"><span data-stu-id="663fd-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="663fd-209">*Id*パラメーターの URI にバインドします。</span><span class="sxs-lookup"><span data-stu-id="663fd-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="663fd-210">したがって、この操作では、"id", ルート ディクショナリまたはクエリ文字列内の値を含む URI が一致だけことができます。</span><span class="sxs-lookup"><span data-stu-id="663fd-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="663fd-211">省略可能なパラメーターは例外をためには省略可能です。</span><span class="sxs-lookup"><span data-stu-id="663fd-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="663fd-212">省略可能なパラメーターでは [ok]、バインドは、URI から値を取得できない場合です。</span><span class="sxs-lookup"><span data-stu-id="663fd-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="663fd-213">複合型では、例外を別の理由。</span><span class="sxs-lookup"><span data-stu-id="663fd-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="663fd-214">複合型は、カスタム バインディングから URI にのみバインドできます。</span><span class="sxs-lookup"><span data-stu-id="663fd-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="663fd-215">その場合は、フレームワーク前もってわかりませんパラメーターは、特定の URI にバインドされているかどうか。</span><span class="sxs-lookup"><span data-stu-id="663fd-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="663fd-216">を調べるにバインディングを呼び出すことはできます。</span><span class="sxs-lookup"><span data-stu-id="663fd-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="663fd-217">選択アルゴリズムの目的では、任意のバインディングを呼び出す前に、静的な記述からアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="663fd-218">そのため、複合型は、一致のアルゴリズムから除外されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="663fd-219">アクションを選択すると、すべてのパラメーター バインドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="663fd-220">概要:</span><span class="sxs-lookup"><span data-stu-id="663fd-220">Summary:</span></span>

- <span data-ttu-id="663fd-221">アクションは、要求の HTTP メソッドと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="663fd-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="663fd-222">アクション名は、存在する場合ルート ディクショナリで"action"エントリを一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="663fd-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="663fd-223">アクションのすべてのパラメーターの場合は、パラメーターは、URI から取得し、パラメーター名必要がありますが見つかりませんルート ディクショナリまたは URI クエリ文字列。</span><span class="sxs-lookup"><span data-stu-id="663fd-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="663fd-224">(省略可能なパラメーターと複合型のパラメーターは除外されます。)</span><span class="sxs-lookup"><span data-stu-id="663fd-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="663fd-225">パラメーター数が最も一致するように再試行してください。</span><span class="sxs-lookup"><span data-stu-id="663fd-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="663fd-226">最適なパラメーターなしでメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="663fd-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="663fd-227">拡張の例</span><span class="sxs-lookup"><span data-stu-id="663fd-227">Extended Example</span></span>

<span data-ttu-id="663fd-228">ルート:</span><span class="sxs-lookup"><span data-stu-id="663fd-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="663fd-229">コントローラー:</span><span class="sxs-lookup"><span data-stu-id="663fd-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="663fd-230">HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="663fd-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="663fd-231">ルートの照合</span><span class="sxs-lookup"><span data-stu-id="663fd-231">Route Matching</span></span>

<span data-ttu-id="663fd-232">URI では、"DefaultApi"をという名前のルートと一致します。</span><span class="sxs-lookup"><span data-stu-id="663fd-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="663fd-233">ルート ディクショナリには、次のエントリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="663fd-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="663fd-234">コント ローラー:"products など"</span><span class="sxs-lookup"><span data-stu-id="663fd-234">controller: "products"</span></span>
- <span data-ttu-id="663fd-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="663fd-235">id: "1"</span></span>

<span data-ttu-id="663fd-236">ルート ディクショナリに、クエリ文字列パラメーター、「バージョン」と「詳細」が含まれていませんが、これらも操作の選択時に考慮されます。</span><span class="sxs-lookup"><span data-stu-id="663fd-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="663fd-237">コント ローラーの選択</span><span class="sxs-lookup"><span data-stu-id="663fd-237">Controller Selection</span></span>

<span data-ttu-id="663fd-238">コント ローラーの種類には、"controller"のエントリ ルート ディクショナリから、`ProductsController`です。</span><span class="sxs-lookup"><span data-stu-id="663fd-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="663fd-239">操作の選択</span><span class="sxs-lookup"><span data-stu-id="663fd-239">Action Selection</span></span>

<span data-ttu-id="663fd-240">HTTP 要求は、GET 要求です。</span><span class="sxs-lookup"><span data-stu-id="663fd-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="663fd-241">GET をサポートするコント ローラー アクションは`GetAll`、 `GetById`、および`FindProductsByName`です。</span><span class="sxs-lookup"><span data-stu-id="663fd-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="663fd-242">ルート ディクショナリは、"action"のエントリを含まないため、操作名と一致する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="663fd-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="663fd-243">次に、しようと、アクションのパラメーター名と一致する GET 操作だけを見ています。</span><span class="sxs-lookup"><span data-stu-id="663fd-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="663fd-244">アクション</span><span class="sxs-lookup"><span data-stu-id="663fd-244">Action</span></span> | <span data-ttu-id="663fd-245">一致するパラメーター</span><span class="sxs-lookup"><span data-stu-id="663fd-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="663fd-246">none</span><span class="sxs-lookup"><span data-stu-id="663fd-246">none</span></span> |
| `GetById` | <span data-ttu-id="663fd-247">"id"</span><span class="sxs-lookup"><span data-stu-id="663fd-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="663fd-248">"name"</span><span class="sxs-lookup"><span data-stu-id="663fd-248">"name"</span></span> |

<span data-ttu-id="663fd-249">注意して、*バージョン*のパラメーター`GetById`とは見なされません、省略可能なパラメーターであるためです。</span><span class="sxs-lookup"><span data-stu-id="663fd-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="663fd-250">`GetAll`普通のメソッドに一致します。</span><span class="sxs-lookup"><span data-stu-id="663fd-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="663fd-251">`GetById`メソッドも一致すると、ルート ディクショナリには、"id"が含まれているためです。</span><span class="sxs-lookup"><span data-stu-id="663fd-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="663fd-252">`FindProductsByName`メソッドと一致しません。</span><span class="sxs-lookup"><span data-stu-id="663fd-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="663fd-253">`GetById`のパラメーターなしではなく、1 つのパラメーターと一致するので、メソッドが wins`GetAll`です。</span><span class="sxs-lookup"><span data-stu-id="663fd-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="663fd-254">このメソッドは、次のパラメーター値。</span><span class="sxs-lookup"><span data-stu-id="663fd-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="663fd-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="663fd-255">*id* = 1</span></span>
- <span data-ttu-id="663fd-256">*バージョン*1.5 を =</span><span class="sxs-lookup"><span data-stu-id="663fd-256">*version* = 1.5</span></span>

<span data-ttu-id="663fd-257">たとえことに注意して*バージョン*は使用されませんでした選択アルゴリズムのパラメーターの値は URI クエリ文字列から取得します。</span><span class="sxs-lookup"><span data-stu-id="663fd-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="663fd-258">拡張ポイント</span><span class="sxs-lookup"><span data-stu-id="663fd-258">Extension Points</span></span>

<span data-ttu-id="663fd-259">Web API は、ルーティング プロセスの一部の拡張ポイントを提供します。</span><span class="sxs-lookup"><span data-stu-id="663fd-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="663fd-260">Interface</span><span class="sxs-lookup"><span data-stu-id="663fd-260">Interface</span></span> | <span data-ttu-id="663fd-261">説明</span><span class="sxs-lookup"><span data-stu-id="663fd-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="663fd-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="663fd-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="663fd-263">コント ローラーを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-263">Selects the controller.</span></span> |
| <span data-ttu-id="663fd-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="663fd-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="663fd-265">コント ローラーの種類の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="663fd-265">Gets the list of controller types.</span></span> <span data-ttu-id="663fd-266">**DefaultHttpControllerSelector**がこの一覧からコント ローラーの種類を選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="663fd-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="663fd-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="663fd-268">プロジェクト アセンブリの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="663fd-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="663fd-269">**IHttpControllerTypeResolver**インターフェイスでは、この一覧を使用して、コント ローラーの種類を検索します。</span><span class="sxs-lookup"><span data-stu-id="663fd-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="663fd-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="663fd-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="663fd-271">コント ローラーの新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="663fd-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="663fd-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="663fd-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="663fd-273">アクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="663fd-273">Selects the action.</span></span> |
| <span data-ttu-id="663fd-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="663fd-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="663fd-275">アクションを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="663fd-275">Invokes the action.</span></span> |

<span data-ttu-id="663fd-276">これらのインターフェイスのいずれにも、独自の実装を提供するには、使用、 **Services**コレクションに、 **HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="663fd-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
