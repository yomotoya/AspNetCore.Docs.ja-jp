---
uid: web-api/overview/advanced/http-message-handlers
title: "ASP.NET web API HTTP メッセージ ハンドラー |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="1fd4a-102">ASP.NET web API HTTP メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="1fd4a-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="1fd4a-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1fd4a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1fd4a-104">A*メッセージ ハンドラー* HTTP 要求を受信して、HTTP 応答を返すクラスです。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="1fd4a-105">メッセージ ハンドラーは、抽象型から派生**HttpMessageHandler**クラスです。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="1fd4a-106">通常は、一連のメッセージのハンドラーを連結します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="1fd4a-107">最初のハンドラーが HTTP 要求を受信するには、いくつかの処理を行うおよび要求は、次のハンドラーにします。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="1fd4a-108">ある時点で、応答が作成され、チェーンの上に戻ります。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="1fd4a-109">このパターンが呼び出された、*委任*ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="1fd4a-110">サーバー側のメッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="1fd4a-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="1fd4a-111">サーバー側では、Web API パイプラインは、いくつか組み込まれているメッセージのハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="1fd4a-112">**HttpServer**ホストから要求を取得します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="1fd4a-113">**HttpRoutingDispatcher**ルートに基づいて要求をディスパッチします。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="1fd4a-114">**HttpControllerDispatcher** Web API コント ローラーに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="1fd4a-115">パイプラインにカスタム ハンドラーを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="1fd4a-116">メッセージ ハンドラーは、HTTP メッセージ (なくコント ローラーのアクション) のレベルで動作する横断的関心事に適してします。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="1fd4a-117">たとえば、メッセージのハンドラーがあります。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-117">For example, a message handler might:</span></span>

- <span data-ttu-id="1fd4a-118">読み取りまたは要求ヘッダーを変更します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-118">Read or modify request headers.</span></span>
- <span data-ttu-id="1fd4a-119">応答ヘッダーを応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-119">Add a response header to responses.</span></span>
- <span data-ttu-id="1fd4a-120">コント ローラーに到達する前に、要求を検証します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="1fd4a-121">この図は、パイプラインに挿入された 2 つのカスタム ハンドラーを示しています。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="1fd4a-122">クライアント側で HttpClient もメッセージ ハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="1fd4a-123">詳細については、次を参照してください。 [HttpClient メッセージ ハンドラー](httpclient-message-handlers.md)です。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="1fd4a-124">カスタム メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="1fd4a-124">Custom Message Handlers</span></span>

<span data-ttu-id="1fd4a-125">派生するカスタム メッセージのハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="1fd4a-126">このメソッドのシグネチャは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="1fd4a-127">メソッドは、 **HttpRequestMessage**として入力非同期に返す、 **HttpResponseMessage**です。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="1fd4a-128">一般的な実装は次のとおり</span><span class="sxs-lookup"><span data-stu-id="1fd4a-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="1fd4a-129">要求メッセージを処理します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-129">Process the request message.</span></span>
2. <span data-ttu-id="1fd4a-130">呼び出す`base.SendAsync`内部ハンドラーに、要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="1fd4a-131">内部ハンドラーでは、応答メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-131">The inner handler returns a response message.</span></span> <span data-ttu-id="1fd4a-132">(この手順は、非同期です)。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="1fd4a-133">応答を処理し、呼び出し元に返します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="1fd4a-134">簡単な例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="1fd4a-135">呼び出し`base.SendAsync`は非同期です。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="1fd4a-136">場合は、ハンドラーは、この呼び出しの後の作業を使用して、 **await**キーワードで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="1fd4a-137">委任のハンドラーも内部ハンドラーをスキップし、直接応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="1fd4a-138">ハンドラーが呼び出さずに応答を作成する場合は、委任`base.SendAsync`、要求パイプラインの残りの部分をスキップします。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="1fd4a-139">要求を検証します (エラー応答の作成) のハンドラーに役立つことができます。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="1fd4a-140">パイプラインにハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="1fd4a-141">サーバー側でメッセージのハンドラーを追加するハンドラーを追加、 **HttpConfiguration.MessageHandlers**コレクション。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="1fd4a-142">「ASP.NET MVC 4 Web アプリケーション」テンプレートを使用して、プロジェクトを作成した場合は、この内側を行うことができます、 **WebApiConfig**クラス。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="1fd4a-143">メッセージ ハンドラーは記載されているのと同じ順序で呼び出されます**MessageHandlers**コレクション。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="1fd4a-144">ネストされているため、応答メッセージは逆方向に移動します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="1fd4a-145">その最後のハンドラーは、最初に応答メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="1fd4a-146">内部ハンドラー; を設定する必要がないことに注意してください。Web API フレームワークでは、メッセージのハンドラーに自動的に接続します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="1fd4a-147">場合[自己ホスト型](../older-versions/self-host-a-web-api.md)のインスタンスを作成、 **HttpSelfHostConfiguration**クラスし、へハンドラーを追加、 **MessageHandlers**コレクション。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="1fd4a-148">これでカスタム メッセージ ハンドラーの例をいくつか見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="1fd4a-149">例: X-HTTP のメソッドのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="1fd4a-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="1fd4a-150">X HTTP メソッド オーバーライドは、標準 HTTP ヘッダーです。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="1fd4a-151">PUT や DELETE など、特定の HTTP 要求種類を送信できないクライアントに設計されています。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="1fd4a-152">代わりに、クライアントは POST 要求を送信し、目的のメソッドに X HTTP メソッド オーバーライドのヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="1fd4a-153">例:</span><span class="sxs-lookup"><span data-stu-id="1fd4a-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="1fd4a-154">X HTTP メソッド オーバーライドのサポートを追加するメッセージ ハンドラーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="1fd4a-155">**SendAsync**メソッド、ハンドラーは、セルの内容かどうか、要求メッセージが POST 要求では、X HTTP メソッド オーバーライド ヘッダーが含まれているかどうか。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="1fd4a-156">場合は、ヘッダー値を検証し、要求メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="1fd4a-157">最後に、ハンドラーが呼び出され`base.SendAsync`にメッセージを次のハンドラーに渡します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="1fd4a-158">要求に達したとき、 **HttpControllerDispatcher**クラス、 **HttpControllerDispatcher**更新要求のメソッドに基づく要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="1fd4a-159">例: カスタム応答ヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="1fd4a-160">すべての応答メッセージにカスタム ヘッダーを追加するメッセージ ハンドラーを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="1fd4a-161">最初に、ハンドラーを呼び出す`base.SendAsync`内部メッセージ ハンドラーに要求を渡す。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="1fd4a-162">内部ハンドラーが、応答メッセージを返しますが、これは、非同期に実行を使用して、**タスク&lt;T&gt;** オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="1fd4a-163">応答メッセージがまで利用できない`base.SendAsync`非同期的に完了します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="1fd4a-164">この例では、 **await**作業を実行するキーワード後に非同期的に`SendAsync`が完了します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="1fd4a-165">.NET Framework 4.0 を対象とする場合を使用して、**タスク**&lt;T&gt;**です。ContinueWith**メソッド。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="1fd4a-166">例: の API キーの確認</span><span class="sxs-lookup"><span data-stu-id="1fd4a-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="1fd4a-167">一部の web サービスでは、クライアントがその要求に API キーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="1fd4a-168">次の例では、メッセージのハンドラーが有効な API キーの要求を確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="1fd4a-169">このハンドラーは、URI クエリ文字列内の API キーを検索します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="1fd4a-170">(この例では、ものとキーは、静的な文字列であります。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="1fd4a-171">実際の実装はおそらく使用より複雑な検証します。)クエリ文字列にキーが含まれている場合、ハンドラーは要求を内部ハンドラーに渡します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="1fd4a-172">ハンドラーの応答メッセージをステータス 403、作成要求が有効なキーを持たない場合は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="1fd4a-173">この場合、ハンドラーを呼び出しません`base.SendAsync`、内部ハンドラーが決して要求を受け取りようにまたはコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="1fd4a-174">そのため、コント ローラーでは、すべての着信要求が有効な API キーを持っていることを想定できます。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="1fd4a-175">API キーは、特定のコント ローラー アクションにのみ適用する場合は、メッセージ ハンドラーではなくアクション フィルターの使用を検討します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="1fd4a-176">アクション フィルターは、URI のルーティングが実行された後に実行されます。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="1fd4a-177">ルートごとのメッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="1fd4a-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="1fd4a-178">ハンドラー、 **HttpConfiguration.MessageHandlers**コレクションをグローバルに適用します。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="1fd4a-179">または、ルートを定義するときは、特定のルートにメッセージ ハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="1fd4a-180">この例では、要求の URI"Route2"に一致する場合、要求にディスパッチされる`MessageHandler2`です。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="1fd4a-181">次の図は、これら 2 つのルートのパイプラインを示しています。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="1fd4a-182">注意して`MessageHandler2`既定値を置き換えます**HttpControllerDispatcher**です。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="1fd4a-183">この例では`MessageHandler2`応答を作成し、コント ローラーには決して"Route2"一致する要求。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="1fd4a-184">これにより、独自のカスタム エンドポイントを持つ Web API コント ローラーのメカニズムを全体を置換できます。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="1fd4a-185">または、ルート別のメッセージ ハンドラーに委任できます**HttpControllerDispatcher**、コント ローラーを次にディスパッチします。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="1fd4a-186">次のコードは、このルートを構成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="1fd4a-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
