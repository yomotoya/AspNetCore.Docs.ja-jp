---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API でルーティング |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="4045c-102">ASP.NET Web API でのルーティング</span><span class="sxs-lookup"><span data-stu-id="4045c-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="4045c-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4045c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4045c-104">この記事では、ASP.NET Web API がコント ローラーに HTTP 要求をルーティングする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4045c-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="4045c-105">ASP.NET MVC について理解する場合は、Web API ルーティングは、MVC ルーティングするためによく似ています。</span><span class="sxs-lookup"><span data-stu-id="4045c-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="4045c-106">主な違いは、Web API メソッドを使用して、HTTP、URI パスではなく、操作を選択します。</span><span class="sxs-lookup"><span data-stu-id="4045c-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="4045c-107">また、Web API での MVC スタイルのルーティングを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="4045c-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="4045c-108">この記事は、ASP.NET MVC の知識を想定していません。</span><span class="sxs-lookup"><span data-stu-id="4045c-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="4045c-109">ルーティング テーブル</span><span class="sxs-lookup"><span data-stu-id="4045c-109">Routing Tables</span></span>

<span data-ttu-id="4045c-110">ASP.NET Web API で、*コント ローラー* HTTP 要求を処理するクラスです。</span><span class="sxs-lookup"><span data-stu-id="4045c-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="4045c-111">コント ローラーのパブリック メソッドが呼び出されます*アクション メソッド*または単に*アクション*です。</span><span class="sxs-lookup"><span data-stu-id="4045c-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="4045c-112">Web API フレームワークでは、要求を受信すると、アクションに要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="4045c-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="4045c-113">フレームワークを使用して呼び出すアクションを確認するのには*ルーティング テーブル*です。</span><span class="sxs-lookup"><span data-stu-id="4045c-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="4045c-114">Web API の Visual Studio プロジェクト テンプレートには、既定のルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="4045c-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="4045c-115">このルートがアプリに配置されます WebApiConfig.cs ファイルで定義されている\_開始ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="4045c-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="4045c-116">詳細については、 **WebApiConfig**クラスを参照してください[ASP.NET Web API の構成](../advanced/configuring-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="4045c-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="4045c-117">自己 Web API をホストしている場合に直接ルーティング テーブルを設定する必要があります、 **HttpSelfHostConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4045c-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="4045c-118">詳細については、次を参照してください。 [Web API の自己ホスト](../older-versions/self-host-a-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="4045c-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="4045c-119">ルーティング テーブル内の各エントリが含まれています、*ルート テンプレート*です。</span><span class="sxs-lookup"><span data-stu-id="4045c-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="4045c-120">Web API の既定のルート テンプレートは&quot;api/{controller}/{id}&quot;です。</span><span class="sxs-lookup"><span data-stu-id="4045c-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="4045c-121">このテンプレートで&quot;api&quot;は、リテラルのパス セグメント、および {controller} と {id} プレース ホルダー変数します。</span><span class="sxs-lookup"><span data-stu-id="4045c-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="4045c-122">Web API フレームワークでは、HTTP 要求を受信、ルーティング テーブルにルート テンプレートのいずれかに対して URI と照合しようとします。</span><span class="sxs-lookup"><span data-stu-id="4045c-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="4045c-123">ルートが一致しない場合、クライアントは、404 エラーを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="4045c-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="4045c-124">たとえば、次の Uri は、既定のルートを一致します。</span><span class="sxs-lookup"><span data-stu-id="4045c-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="4045c-125">/api および連絡先</span><span class="sxs-lookup"><span data-stu-id="4045c-125">/api/contacts</span></span>
- <span data-ttu-id="4045c-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="4045c-126">/api/contacts/1</span></span>
- <span data-ttu-id="4045c-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="4045c-127">/api/products/gizmo1</span></span>

<span data-ttu-id="4045c-128">ただし、次の URI が一致しないがないため、 &quot;api&quot;セグメント。</span><span class="sxs-lookup"><span data-stu-id="4045c-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="4045c-129">/連絡先/1</span><span class="sxs-lookup"><span data-stu-id="4045c-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="4045c-130">ルートに"api"を使用する理由は、ASP.NET MVC ルーティングとの衝突を避けるためにです。</span><span class="sxs-lookup"><span data-stu-id="4045c-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="4045c-131">そうすることができます&quot;連絡先/&quot; 、MVC コント ローラーに移動し、 &quot;api/連絡先&quot;Web API コント ローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="4045c-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="4045c-132">もちろん、この規則がたくない場合は、既定のルート テーブルを変更できます。</span><span class="sxs-lookup"><span data-stu-id="4045c-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="4045c-133">一致するルートが見つかると、Web API は、コント ローラーとアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="4045c-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="4045c-134">Web API を追加するコント ローラーを検索するには、&quot;コント ローラー&quot;の値に、 *{controller}* 変数。</span><span class="sxs-lookup"><span data-stu-id="4045c-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="4045c-135">アクションを検索するには、Web API HTTP メソッドと調べ操作の対応する HTTP メソッド名で始まる名前を検索します。</span><span class="sxs-lookup"><span data-stu-id="4045c-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="4045c-136">たとえば、GET 要求で Web API 検索で始まるアクション&quot;を取得しています.&quot;など&quot;GetContact&quot;または&quot;GetAllContacts&quot;です。</span><span class="sxs-lookup"><span data-stu-id="4045c-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="4045c-137">この規則は、取得、POST、PUT、および削除メソッドにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="4045c-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="4045c-138">その他の HTTP メソッドを有効にするには、コント ローラーの属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="4045c-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="4045c-139">その例が後で表示されます。</span><span class="sxs-lookup"><span data-stu-id="4045c-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="4045c-140">ルート テンプレートでは、その他のプレース ホルダー変数など *{id},* アクション パラメーターにマップされます。</span><span class="sxs-lookup"><span data-stu-id="4045c-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="4045c-141">例を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="4045c-141">Let's look at an example.</span></span> <span data-ttu-id="4045c-142">次のコント ローラーを定義するとします。</span><span class="sxs-lookup"><span data-stu-id="4045c-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="4045c-143">一部可能な HTTP 要求、ごとに呼び出されるアクションを次に示します。</span><span class="sxs-lookup"><span data-stu-id="4045c-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="4045c-144">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="4045c-144">HTTP Method</span></span> | <span data-ttu-id="4045c-145">URI のパス</span><span class="sxs-lookup"><span data-stu-id="4045c-145">URI Path</span></span> | <span data-ttu-id="4045c-146">操作</span><span class="sxs-lookup"><span data-stu-id="4045c-146">Action</span></span> | <span data-ttu-id="4045c-147">パラメーター</span><span class="sxs-lookup"><span data-stu-id="4045c-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4045c-148">GET</span><span class="sxs-lookup"><span data-stu-id="4045c-148">GET</span></span> | <span data-ttu-id="4045c-149">api/製品</span><span class="sxs-lookup"><span data-stu-id="4045c-149">api/products</span></span> | <span data-ttu-id="4045c-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="4045c-150">GetAllProducts</span></span> | <span data-ttu-id="4045c-151">*(なし)*</span><span class="sxs-lookup"><span data-stu-id="4045c-151">*(none)*</span></span> |
| <span data-ttu-id="4045c-152">GET</span><span class="sxs-lookup"><span data-stu-id="4045c-152">GET</span></span> | <span data-ttu-id="4045c-153">api/製品/4</span><span class="sxs-lookup"><span data-stu-id="4045c-153">api/products/4</span></span> | <span data-ttu-id="4045c-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="4045c-154">GetProductById</span></span> | <span data-ttu-id="4045c-155">4</span><span class="sxs-lookup"><span data-stu-id="4045c-155">4</span></span> |
| <span data-ttu-id="4045c-156">Del</span><span class="sxs-lookup"><span data-stu-id="4045c-156">DELETE</span></span> | <span data-ttu-id="4045c-157">api/製品/4</span><span class="sxs-lookup"><span data-stu-id="4045c-157">api/products/4</span></span> | <span data-ttu-id="4045c-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="4045c-158">DeleteProduct</span></span> | <span data-ttu-id="4045c-159">4</span><span class="sxs-lookup"><span data-stu-id="4045c-159">4</span></span> |
| <span data-ttu-id="4045c-160">投稿</span><span class="sxs-lookup"><span data-stu-id="4045c-160">POST</span></span> | <span data-ttu-id="4045c-161">api/製品</span><span class="sxs-lookup"><span data-stu-id="4045c-161">api/products</span></span> | <span data-ttu-id="4045c-162">*(一致なし)*</span><span class="sxs-lookup"><span data-stu-id="4045c-162">*(no match)*</span></span> |  |

<span data-ttu-id="4045c-163">注意して、 *{id}* 、URI のセグメントは存在する場合にマップされて、 *id*アクションのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="4045c-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="4045c-164">この例では、コント ローラーを持つ 2 つの GET メソッドを定義、 *id*パラメーター、パラメーターなしのいずれか。</span><span class="sxs-lookup"><span data-stu-id="4045c-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="4045c-165">また、注意してください、コント ローラーが定義されていないため、POST 要求に失敗する、 &quot;Post しています.&quot;メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4045c-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="4045c-166">ルーティングのバリエーション</span><span class="sxs-lookup"><span data-stu-id="4045c-166">Routing Variations</span></span>

<span data-ttu-id="4045c-167">前のセクションでは、ASP.NET Web api ルーティングの基本的なメカニズムについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4045c-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="4045c-168">このセクションでは、いくつかのバリエーションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4045c-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="4045c-169">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="4045c-169">HTTP Methods</span></span>

<span data-ttu-id="4045c-170">HTTP メソッドの名前付け規則を使用する代わりにことが明示的に指定するアクションの HTTP メソッドをアクション メソッドを修飾することによって、 **HttpGet**、 **HttpPut**、 **HttpPost**、または**HttpDelete**属性。</span><span class="sxs-lookup"><span data-stu-id="4045c-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="4045c-171">次の例では、FindProduct メソッドは、GET 要求にマップされます。</span><span class="sxs-lookup"><span data-stu-id="4045c-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="4045c-172">アクションに対して複数の HTTP メソッドを許可する、または GET、PUT、POST、および DELETE 以外の HTTP メソッドを使用して、 **AcceptVerbs**属性は、HTTP メソッドの一覧です。</span><span class="sxs-lookup"><span data-stu-id="4045c-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="4045c-173">アクション名をルーティング</span><span class="sxs-lookup"><span data-stu-id="4045c-173">Routing by Action Name</span></span>

<span data-ttu-id="4045c-174">Web API では、既定のルーティング テンプレートで、HTTP メソッドを使用して、操作を選択します。</span><span class="sxs-lookup"><span data-stu-id="4045c-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="4045c-175">ただし、URI でアクション名が含まれているルートを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="4045c-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="4045c-176">このルート テンプレートで、 *{controller}* パラメーター名、コント ローラー アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="4045c-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="4045c-177">ルーティングのこのスタイルでは、許可される HTTP メソッドを指定するのに属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="4045c-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="4045c-178">たとえば、コント ローラーに次のメソッドがあるとします。</span><span class="sxs-lookup"><span data-stu-id="4045c-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="4045c-179">この場合、「api/製品/詳細/1」の GET 要求は、Details メソッドにマップします。</span><span class="sxs-lookup"><span data-stu-id="4045c-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="4045c-180">このスタイルのルーティングは ASP.NET MVC に似ており、RPC スタイルの API の適切な場合があります。</span><span class="sxs-lookup"><span data-stu-id="4045c-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="4045c-181">使用して、アクション名をオーバーライドすることができます、 **ActionName**属性。</span><span class="sxs-lookup"><span data-stu-id="4045c-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="4045c-182">次の例では、2 つのアクションにマップされる&quot;api/製品/縮小版/*id*です。GET をでいずれか、他の POST をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="4045c-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="4045c-183">アクション以外</span><span class="sxs-lookup"><span data-stu-id="4045c-183">Non-Actions</span></span>

<span data-ttu-id="4045c-184">メソッドを防ぐため、アクションとして呼び出されてを使用して、 **NonAction**属性。</span><span class="sxs-lookup"><span data-stu-id="4045c-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="4045c-185">これは、通知 framework メソッドは、アクションではない場合でも、ルーティングの規則はそれ以外の場合と一致します。</span><span class="sxs-lookup"><span data-stu-id="4045c-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="4045c-186">関連項目</span><span class="sxs-lookup"><span data-stu-id="4045c-186">Further Reading</span></span>

<span data-ttu-id="4045c-187">このトピックでは、ルーティングの概要ビューが用意されています。</span><span class="sxs-lookup"><span data-stu-id="4045c-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="4045c-188">詳細については、次を参照してください。[ルーティングとアクションの選択](routing-and-action-selection.md)、を正確におよび方法について説明フレームワークをルート URI と一致する、コント ローラーを選択に呼び出すアクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="4045c-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
