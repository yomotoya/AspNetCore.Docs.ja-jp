---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API でのルーティング |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207304"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="c8486-102">ASP.NET Web API でのルーティング</span><span class="sxs-lookup"><span data-stu-id="c8486-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="c8486-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c8486-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c8486-104">この記事では、ASP.NET Web API がコント ローラーに HTTP 要求をルーティングする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c8486-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="c8486-105">ASP.NET MVC について理解する場合は、Web API のルーティングは、MVC ルーティングによく似ています。</span><span class="sxs-lookup"><span data-stu-id="c8486-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="c8486-106">主な違いは、Web API が URI パスではなく、HTTP 動詞を使用して、アクションを選択することです。</span><span class="sxs-lookup"><span data-stu-id="c8486-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="c8486-107">MVC スタイルの Web API のルーティングを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="c8486-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="c8486-108">この記事では、ASP.NET MVC の知識を前提としてはいません。</span><span class="sxs-lookup"><span data-stu-id="c8486-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="c8486-109">ルーティング テーブル</span><span class="sxs-lookup"><span data-stu-id="c8486-109">Routing Tables</span></span>

<span data-ttu-id="c8486-110">ASP.NET Web api で、*コント ローラー*は HTTP 要求を処理するクラスです。</span><span class="sxs-lookup"><span data-stu-id="c8486-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="c8486-111">コント ローラーのパブリック メソッドが呼び出されます*アクション メソッド*または単に*アクション*します。</span><span class="sxs-lookup"><span data-stu-id="c8486-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="c8486-112">Web API フレームワークは、要求を受信すると、アクションに要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="c8486-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="c8486-113">呼び出すアクションを確認するのには、framework は、*ルーティング テーブル*します。</span><span class="sxs-lookup"><span data-stu-id="c8486-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="c8486-114">Web API 用の Visual Studio プロジェクト テンプレートは、既定のルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="c8486-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="c8486-115">このルートが定義されている、 *WebApiConfig.cs*で配置されているファイル、*アプリ\_開始*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="c8486-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="c8486-116">詳細については、`WebApiConfig`クラスを参照してください[ASP.NET Web API の構成](../advanced/configuring-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="c8486-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="c8486-117">Web API を自己ホストする場合は、上で直接ルーティング テーブルを設定する必要があります、`HttpSelfHostConfiguration`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c8486-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="c8486-118">詳細については、次を参照してください。 [Web API を自己ホスト](../older-versions/self-host-a-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="c8486-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="c8486-119">ルーティング テーブル各エントリが含まれる、*ルート テンプレート*します。</span><span class="sxs-lookup"><span data-stu-id="c8486-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="c8486-120">Web API の既定のルート テンプレートは&quot;api/{controller}/{id}&quot;します。</span><span class="sxs-lookup"><span data-stu-id="c8486-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="c8486-121">このテンプレートで&quot;api&quot;はリテラルのパス セグメント、および {controller} と {id} はプレース ホルダー変数です。</span><span class="sxs-lookup"><span data-stu-id="c8486-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="c8486-122">Web API フレームワークでは、HTTP 要求を受信、ルーティング テーブルにルート テンプレートのいずれかに対して URI と照合しようとします。</span><span class="sxs-lookup"><span data-stu-id="c8486-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="c8486-123">ルートが一致しない場合、クライアントは、404 エラーを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="c8486-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="c8486-124">たとえば、次の Uri では、既定のルートと一致します。</span><span class="sxs-lookup"><span data-stu-id="c8486-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="c8486-125">api/連絡先</span><span class="sxs-lookup"><span data-stu-id="c8486-125">/api/contacts</span></span>
- <span data-ttu-id="c8486-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="c8486-126">/api/contacts/1</span></span>
- <span data-ttu-id="c8486-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="c8486-127">/api/products/gizmo1</span></span>

<span data-ttu-id="c8486-128">ただし、次の URI が一致しないがないため、 &quot;api&quot;セグメント。</span><span class="sxs-lookup"><span data-stu-id="c8486-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="c8486-129">/連絡先/1</span><span class="sxs-lookup"><span data-stu-id="c8486-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="c8486-130">ルートで"api"を使用する理由は、ASP.NET MVC ルーティングとの衝突を避けるためです。</span><span class="sxs-lookup"><span data-stu-id="c8486-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="c8486-131">そうすることがあることができます&quot;連絡先/&quot; 、MVC コント ローラーに移動し、 &quot;api/連絡先&quot;Web API コント ローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="c8486-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="c8486-132">もちろん、この規則に満足できない場合は、既定のルーティング テーブルを変更できます。</span><span class="sxs-lookup"><span data-stu-id="c8486-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="c8486-133">一致するルートが見つかったら、Web API コント ローラーとアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="c8486-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="c8486-134">Web API を追加、コント ローラーを検索する&quot;コント ローラー&quot;の値に、 *{controller}* 変数。</span><span class="sxs-lookup"><span data-stu-id="c8486-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="c8486-135">アクションを検索するには、Web API は、HTTP 動詞を見てし、その HTTP 動詞名で始まる名前、アクションを探します。</span><span class="sxs-lookup"><span data-stu-id="c8486-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="c8486-136">たとえば、GET 要求で Web API を探しますアクションが付いた&quot;取得&quot;など&quot;GetContact&quot;または&quot;GetAllContacts&quot;します。</span><span class="sxs-lookup"><span data-stu-id="c8486-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="c8486-137">この規則は、取得、POST、PUT、DELETE、HEAD、オプション、および PATCH の動詞にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="c8486-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="c8486-138">属性をコント ローラーを使用して、他の HTTP 動詞を有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="c8486-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="c8486-139">後で例を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="c8486-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="c8486-140">ルート テンプレートでは、その他のプレース ホルダー変数など *{id},* アクション パラメーターにマップされます。</span><span class="sxs-lookup"><span data-stu-id="c8486-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="c8486-141">例を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="c8486-141">Let's look at an example.</span></span> <span data-ttu-id="c8486-142">次のコント ローラーを定義するとします。</span><span class="sxs-lookup"><span data-stu-id="c8486-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="c8486-143">いくつか考えられる HTTP 要求、ごとに呼び出されるアクションと共に以下に示します。</span><span class="sxs-lookup"><span data-stu-id="c8486-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="c8486-144">HTTP 動詞</span><span class="sxs-lookup"><span data-stu-id="c8486-144">HTTP Verb</span></span> | <span data-ttu-id="c8486-145">URI のパス</span><span class="sxs-lookup"><span data-stu-id="c8486-145">URI Path</span></span> | <span data-ttu-id="c8486-146">アクション</span><span class="sxs-lookup"><span data-stu-id="c8486-146">Action</span></span> | <span data-ttu-id="c8486-147">パラメーター</span><span class="sxs-lookup"><span data-stu-id="c8486-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c8486-148">GET</span><span class="sxs-lookup"><span data-stu-id="c8486-148">GET</span></span> | <span data-ttu-id="c8486-149">api/製品</span><span class="sxs-lookup"><span data-stu-id="c8486-149">api/products</span></span> | <span data-ttu-id="c8486-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="c8486-150">GetAllProducts</span></span> | <span data-ttu-id="c8486-151">*(なし)*</span><span class="sxs-lookup"><span data-stu-id="c8486-151">*(none)*</span></span> |
| <span data-ttu-id="c8486-152">GET</span><span class="sxs-lookup"><span data-stu-id="c8486-152">GET</span></span> | <span data-ttu-id="c8486-153">api/製品/4</span><span class="sxs-lookup"><span data-stu-id="c8486-153">api/products/4</span></span> | <span data-ttu-id="c8486-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="c8486-154">GetProductById</span></span> | <span data-ttu-id="c8486-155">4</span><span class="sxs-lookup"><span data-stu-id="c8486-155">4</span></span> |
| <span data-ttu-id="c8486-156">Del</span><span class="sxs-lookup"><span data-stu-id="c8486-156">DELETE</span></span> | <span data-ttu-id="c8486-157">api/製品/4</span><span class="sxs-lookup"><span data-stu-id="c8486-157">api/products/4</span></span> | <span data-ttu-id="c8486-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="c8486-158">DeleteProduct</span></span> | <span data-ttu-id="c8486-159">4</span><span class="sxs-lookup"><span data-stu-id="c8486-159">4</span></span> |
| <span data-ttu-id="c8486-160">POST</span><span class="sxs-lookup"><span data-stu-id="c8486-160">POST</span></span> | <span data-ttu-id="c8486-161">api/製品</span><span class="sxs-lookup"><span data-stu-id="c8486-161">api/products</span></span> | <span data-ttu-id="c8486-162">*(一致なし)*</span><span class="sxs-lookup"><span data-stu-id="c8486-162">*(no match)*</span></span> |  |

<span data-ttu-id="c8486-163">なお、 *{id}* 、URI のセグメントは存在する場合にマップされます、 *id*アクションのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="c8486-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="c8486-164">この例では、コント ローラーが 1、2 つの GET メソッドを定義します。、 *id*パラメーター、パラメーターなしのもう 1 つ。</span><span class="sxs-lookup"><span data-stu-id="c8486-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="c8486-165">また、コント ローラーを一切定義しませんので、POST 要求が失敗する注、&quot;投稿しています.&quot;メソッド。</span><span class="sxs-lookup"><span data-stu-id="c8486-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="c8486-166">ルーティングのバリエーション</span><span class="sxs-lookup"><span data-stu-id="c8486-166">Routing Variations</span></span>

<span data-ttu-id="c8486-167">前のセクションでは、ASP.NET Web API の基本的なルーティング メカニズムについて説明します。</span><span class="sxs-lookup"><span data-stu-id="c8486-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="c8486-168">このセクションでは、いくつかのバリエーションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="c8486-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="c8486-169">HTTP 動詞</span><span class="sxs-lookup"><span data-stu-id="c8486-169">HTTP verbs</span></span>

<span data-ttu-id="c8486-170">HTTP 動詞の名前付け規則を使用する代わりに、次の属性のいずれかのアクション メソッドを修飾することによって、アクションの HTTP 動詞を明示的に指定できます。</span><span class="sxs-lookup"><span data-stu-id="c8486-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="c8486-171">次の例では、`FindProduct`メソッドが GET 要求にマップされます。</span><span class="sxs-lookup"><span data-stu-id="c8486-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="c8486-172">アクション、複数の HTTP 動詞を許可するまたは以外、GET、PUT、POST、DELETE、HEAD、オプション、および PATCH HTTP 動詞を許可するのを使用して、`[AcceptVerbs]`属性には、HTTP 動詞のリストを取得します。</span><span class="sxs-lookup"><span data-stu-id="c8486-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="c8486-173">アクション名によるルーティング</span><span class="sxs-lookup"><span data-stu-id="c8486-173">Routing by Action Name</span></span>

<span data-ttu-id="c8486-174">既定のルーティング テンプレートでは、Web API は HTTP 動詞を使用して、アクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="c8486-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="c8486-175">ただし、アクション名が URI に含まれている場所のルートを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="c8486-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="c8486-176">このルート テンプレートで、 *{action}* パラメーター名、コント ローラー アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="c8486-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="c8486-177">このスタイルのルーティングでは、属性を使用して、使用できる HTTP 動詞を指定します。</span><span class="sxs-lookup"><span data-stu-id="c8486-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="c8486-178">たとえば、コント ローラーに次のメソッドがあるとします。</span><span class="sxs-lookup"><span data-stu-id="c8486-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="c8486-179">「Api/製品/詳細/1」の GET 要求をここにマップ、`Details`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c8486-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="c8486-180">このスタイルのルーティングは、ASP.NET MVC でのような RPC スタイルの API の適切な場合があります。</span><span class="sxs-lookup"><span data-stu-id="c8486-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="c8486-181">アクション名を使用してオーバーライドすることができます、`[ActionName]`属性。</span><span class="sxs-lookup"><span data-stu-id="c8486-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="c8486-182">次の例では、2 つのアクションにマップされる&quot;api/製品/サムネイル/*id*します。GET をサポートしている 1 つと、その他の投稿をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="c8486-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="c8486-183">非アクション</span><span class="sxs-lookup"><span data-stu-id="c8486-183">Non-Actions</span></span>

<span data-ttu-id="c8486-184">メソッドがアクションとして呼び出されることを防ぐために使用して、`[NonAction]`属性。</span><span class="sxs-lookup"><span data-stu-id="c8486-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="c8486-185">これに通知フレームワーク メソッドは、アクションではない場合でも、ルーティングの規則はそれ以外の場合と一致します。</span><span class="sxs-lookup"><span data-stu-id="c8486-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="c8486-186">関連項目</span><span class="sxs-lookup"><span data-stu-id="c8486-186">Further Reading</span></span>

<span data-ttu-id="c8486-187">このトピックでは、ルーティングの概要が提供されています。</span><span class="sxs-lookup"><span data-stu-id="c8486-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="c8486-188">詳細については、次を参照してください。[ルーティングとアクションの選択](routing-and-action-selection.md)、が正確におよび方法について説明フレームワーク ルートへの URI と一致する、コント ローラーを選択します。 次に呼び出すアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="c8486-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
