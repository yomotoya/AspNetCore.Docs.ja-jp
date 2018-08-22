---
uid: web-api/overview/advanced/http-message-handlers
title: ASP.NET Web API の HTTP メッセージ ハンドラー |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 0b0d7b4c543dc4e597c6c472083898f3a8095a83
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823923"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="a03eb-102">ASP.NET Web API の HTTP メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="a03eb-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a03eb-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a03eb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a03eb-104">A*メッセージ ハンドラー*は HTTP 要求を受信し、HTTP 応答を返すクラスです。</span><span class="sxs-lookup"><span data-stu-id="a03eb-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="a03eb-105">メッセージ ハンドラーは、抽象型から派生**HttpMessageHandler**クラス。</span><span class="sxs-lookup"><span data-stu-id="a03eb-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="a03eb-106">通常、一連のメッセージ ハンドラーが連結されます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="a03eb-107">最初のハンドラー HTTP 要求を受信するには、いくつかの処理およびの次のハンドラーへの要求を提供します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="a03eb-108">いくつかの時点では、応答が作成され、チェーンの上位に戻ります。</span><span class="sxs-lookup"><span data-stu-id="a03eb-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="a03eb-109">このパターンと呼ばれます、*委任*ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="a03eb-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="a03eb-110">サーバー側のメッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="a03eb-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="a03eb-111">サーバー側では、Web API パイプラインは、いくつか組み込まれているメッセージのハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="a03eb-112">**HttpServer**ホストから要求を取得します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="a03eb-113">**HttpRoutingDispatcher**ルートに基づいて、要求をディスパッチします。</span><span class="sxs-lookup"><span data-stu-id="a03eb-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="a03eb-114">**HttpControllerDispatcher** Web API コント ローラーに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="a03eb-115">パイプラインにカスタム ハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="a03eb-116">メッセージ ハンドラーは HTTP メッセージ (なくコント ローラー アクション) のレベルで操作するには、横断的関心事に適しています。</span><span class="sxs-lookup"><span data-stu-id="a03eb-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="a03eb-117">たとえば、メッセージ ハンドラーでは次の場合があります。</span><span class="sxs-lookup"><span data-stu-id="a03eb-117">For example, a message handler might:</span></span>

- <span data-ttu-id="a03eb-118">読み取りまたは要求ヘッダーを変更します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-118">Read or modify request headers.</span></span>
- <span data-ttu-id="a03eb-119">応答ヘッダーを応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-119">Add a response header to responses.</span></span>
- <span data-ttu-id="a03eb-120">コント ローラーに到達する前に、要求を検証します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="a03eb-121">この図は、パイプラインに挿入された 2 つのカスタム ハンドラーを示しています。</span><span class="sxs-lookup"><span data-stu-id="a03eb-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="a03eb-122">HttpClient は、クライアント側で、メッセージのハンドラーも使用します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="a03eb-123">詳細については、次を参照してください。 [HttpClient メッセージ ハンドラー](httpclient-message-handlers.md)します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="a03eb-124">カスタム メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="a03eb-124">Custom Message Handlers</span></span>

<span data-ttu-id="a03eb-125">派生するカスタム メッセージ ハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッド。</span><span class="sxs-lookup"><span data-stu-id="a03eb-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="a03eb-126">このメソッドのシグネチャは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a03eb-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="a03eb-127">メソッドには、 **HttpRequestMessage**として入力し、非同期的に返します、 **HttpResponseMessage**します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="a03eb-128">一般的な実装は、次を行います。</span><span class="sxs-lookup"><span data-stu-id="a03eb-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="a03eb-129">要求メッセージを処理します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-129">Process the request message.</span></span>
2. <span data-ttu-id="a03eb-130">呼び出す`base.SendAsync`内部ハンドラーに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="a03eb-131">内部ハンドラーは、応答メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-131">The inner handler returns a response message.</span></span> <span data-ttu-id="a03eb-132">(この手順は、非同期です)。</span><span class="sxs-lookup"><span data-stu-id="a03eb-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="a03eb-133">応答を処理し、呼び出し元に戻すこと。</span><span class="sxs-lookup"><span data-stu-id="a03eb-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="a03eb-134">簡単な例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="a03eb-135">呼び出し`base.SendAsync`は非同期です。</span><span class="sxs-lookup"><span data-stu-id="a03eb-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="a03eb-136">存在する場合、ハンドラーはこの呼び出しの後の作業を使用して、 **await**キーワードを示すようにします。</span><span class="sxs-lookup"><span data-stu-id="a03eb-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="a03eb-137">デリゲート ハンドラーは、内部ハンドラーをスキップすることもおよび応答を直接作成できます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="a03eb-138">ハンドラーが呼び出さずに応答を作成する場合は、委任`base.SendAsync`、要求パイプラインの残りの部分をスキップします。</span><span class="sxs-lookup"><span data-stu-id="a03eb-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="a03eb-139">(エラー応答の作成) 要求を検証するハンドラーの便利なことができます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="a03eb-140">パイプラインにハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="a03eb-141">サーバー側でメッセージのハンドラーを追加するハンドラーを追加、 **HttpConfiguration.MessageHandlers**コレクション。</span><span class="sxs-lookup"><span data-stu-id="a03eb-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="a03eb-142">プロジェクトを作成する、「ASP.NET MVC 4 Web アプリケーション」テンプレートを使用した場合は、この内部を行うことができます、 **WebApiConfig**クラス。</span><span class="sxs-lookup"><span data-stu-id="a03eb-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="a03eb-143">メッセージ ハンドラーは内で出現する順序で呼び出されます**MessageHandlers**コレクション。</span><span class="sxs-lookup"><span data-stu-id="a03eb-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="a03eb-144">ネストされているため、応答メッセージは他の方向に移動します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="a03eb-145">これは最後のハンドラーでは、応答メッセージを取得する 1 つ目があります。</span><span class="sxs-lookup"><span data-stu-id="a03eb-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="a03eb-146">内部のハンドラーを設定する必要があることに注意してください。Web API フレームワークは、メッセージ ハンドラーを自動的に接続します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="a03eb-147">場合[自己ホスト](../older-versions/self-host-a-web-api.md)のインスタンスを作成、 **HttpSelfHostConfiguration**クラスし、ハンドラーを追加、 **MessageHandlers**コレクション。</span><span class="sxs-lookup"><span data-stu-id="a03eb-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="a03eb-148">これでカスタム メッセージ ハンドラーの例をいくつか見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a03eb-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="a03eb-149">例: X-HTTP のメソッドのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="a03eb-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="a03eb-150">X HTTP メソッド オーバーライドは、非標準の HTTP ヘッダーです。</span><span class="sxs-lookup"><span data-stu-id="a03eb-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="a03eb-151">PUT や DELETE など、特定種類の HTTP 要求を送信できないクライアントに設計されています。</span><span class="sxs-lookup"><span data-stu-id="a03eb-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="a03eb-152">代わりに、クライアントは POST 要求を送信し、必要なメソッドに X HTTP メソッド オーバーライドのヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="a03eb-153">例えば:</span><span class="sxs-lookup"><span data-stu-id="a03eb-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="a03eb-154">X HTTP メソッド オーバーライドのサポートを追加するメッセージ ハンドラーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="a03eb-155">**SendAsync**メソッドかどうか、POST 要求は、要求メッセージと X HTTP メソッド オーバーライド ヘッダーが含まれているかどうか、ハンドラーを確認します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="a03eb-156">そうである場合、ヘッダーの値を検証し、要求メソッドを次に変更します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="a03eb-157">最後に、ハンドラーが呼び出す`base.SendAsync`にメッセージを次のハンドラーに渡します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="a03eb-158">要求が達したとき、 **HttpControllerDispatcher**クラス、 **HttpControllerDispatcher**は更新された要求のメソッドに基づく要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="a03eb-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="a03eb-159">例: カスタムの応答ヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="a03eb-160">すべての応答メッセージにカスタム ヘッダーを追加するメッセージ ハンドラーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="a03eb-161">最初に、ハンドラーが呼び出す`base.SendAsync`内部メッセージ ハンドラーに要求を渡す。</span><span class="sxs-lookup"><span data-stu-id="a03eb-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="a03eb-162">内部ハンドラーが応答メッセージを返しますを使用して非同期には、**タスク&lt;T&gt;** オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a03eb-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="a03eb-163">応答メッセージがまでご利用いただけません`base.SendAsync`が非同期的に完了するとします。</span><span class="sxs-lookup"><span data-stu-id="a03eb-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="a03eb-164">この例では、 **await**キーワードを作業を実行後に非同期的に`SendAsync`が完了するとします。</span><span class="sxs-lookup"><span data-stu-id="a03eb-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="a03eb-165">.NET Framework 4.0 を対象とする場合は、使用、**タスク**&lt;T&gt;**します。ContinueWith**メソッド。</span><span class="sxs-lookup"><span data-stu-id="a03eb-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="a03eb-166">例: は、API キーの確認</span><span class="sxs-lookup"><span data-stu-id="a03eb-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="a03eb-167">一部の web サービスでは、クライアントの要求に API キーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="a03eb-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="a03eb-168">次の例では、メッセージ ハンドラーが有効な API キーの要求を確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="a03eb-169">このハンドラーは、URI クエリ文字列内の API キーを検索します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="a03eb-170">(この例では、想定キーが静的な文字列であります。</span><span class="sxs-lookup"><span data-stu-id="a03eb-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="a03eb-171">実際の実装はおそらく使用より複雑な検証します。)クエリ文字列にキーが含まれている場合、ハンドラーは、内部ハンドラーに要求を渡します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="a03eb-172">ハンドラーが状態 403、応答メッセージを作成する要求が有効なキーを持たない場合は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="a03eb-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="a03eb-173">この場合、ハンドラーは呼び出されません`base.SendAsync`、内部ハンドラーが、要求を受け取るようにも、コント ローラーには。</span><span class="sxs-lookup"><span data-stu-id="a03eb-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="a03eb-174">そのため、コント ローラーでは、すべての着信要求が有効な API キーを持っていることを想定できます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="a03eb-175">API キーは、特定のコント ローラー アクションにのみ適用する場合は、メッセージ ハンドラーの代わりにアクション フィルターの使用を検討してください。</span><span class="sxs-lookup"><span data-stu-id="a03eb-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="a03eb-176">アクション フィルターは、URI のルーティングが実行された後に実行します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="a03eb-177">ルート メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="a03eb-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="a03eb-178">ハンドラーで、 **HttpConfiguration.MessageHandlers**コレクションはグローバルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="a03eb-179">または、ルートを定義するときに、特定のルートにメッセージ ハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="a03eb-180">この例では、要求 URI が"Route2"と一致する場合、要求にディスパッチされます`MessageHandler2`します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="a03eb-181">次の図は、これら 2 つのルートのパイプラインを示しています。</span><span class="sxs-lookup"><span data-stu-id="a03eb-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="a03eb-182">注意`MessageHandler2`、既定値の代わり**HttpControllerDispatcher**します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="a03eb-183">この例で`MessageHandler2`応答を作成し、コント ローラーで作業しない"Route2"一致する要求。</span><span class="sxs-lookup"><span data-stu-id="a03eb-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="a03eb-184">これにより、全体の Web API コント ローラーのメカニズムを独自のカスタム エンドポイントに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a03eb-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="a03eb-185">または、ルート別のメッセージ ハンドラーに委任できます**HttpControllerDispatcher**、コント ローラーをそこにディスパッチします。</span><span class="sxs-lookup"><span data-stu-id="a03eb-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="a03eb-186">次のコードでは、このルートを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a03eb-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
