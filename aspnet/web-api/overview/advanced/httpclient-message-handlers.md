---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "ASP.NET Web API で HttpClient メッセージ ハンドラー |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="ad676-102">ASP.NET Web API で HttpClient メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="ad676-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ad676-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ad676-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ad676-104">A*メッセージ ハンドラー* HTTP 要求を受信して、HTTP 応答を返すクラスです。</span><span class="sxs-lookup"><span data-stu-id="ad676-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="ad676-105">通常は、一連のメッセージのハンドラーを連結します。</span><span class="sxs-lookup"><span data-stu-id="ad676-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="ad676-106">最初のハンドラーが HTTP 要求を受信するには、いくつかの処理を行うおよび要求は、次のハンドラーにします。</span><span class="sxs-lookup"><span data-stu-id="ad676-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="ad676-107">ある時点で、応答が作成され、チェーンの上に戻ります。</span><span class="sxs-lookup"><span data-stu-id="ad676-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="ad676-108">このパターンが呼び出された、*委任*ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="ad676-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="ad676-109">クライアント側で、 **HttpClient**クラスは、要求を処理するメッセージのハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ad676-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="ad676-110">既定のハンドラーは**HttpClientHandler**、ネットワーク経由で要求を送信し、サーバーから応答を取得します。</span><span class="sxs-lookup"><span data-stu-id="ad676-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="ad676-111">カスタム メッセージのハンドラーは、クライアントのパイプラインに挿入できます。</span><span class="sxs-lookup"><span data-stu-id="ad676-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="ad676-112">また、ASP.NET Web API は、サーバー側のメッセージ ハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ad676-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="ad676-113">詳細については、次を参照してください。 [HTTP メッセージ ハンドラー](http-message-handlers.md)です。</span><span class="sxs-lookup"><span data-stu-id="ad676-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="ad676-114">カスタム メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="ad676-114">Custom Message Handlers</span></span>

<span data-ttu-id="ad676-115">派生するカスタム メッセージのハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ad676-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="ad676-116">メソッドのシグネチャを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ad676-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="ad676-117">メソッドは、 **HttpRequestMessage**として入力非同期に返す、 **HttpResponseMessage**です。</span><span class="sxs-lookup"><span data-stu-id="ad676-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="ad676-118">一般的な実装は次のとおり</span><span class="sxs-lookup"><span data-stu-id="ad676-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="ad676-119">要求メッセージを処理します。</span><span class="sxs-lookup"><span data-stu-id="ad676-119">Process the request message.</span></span>
2. <span data-ttu-id="ad676-120">呼び出す`base.SendAsync`内部ハンドラーに、要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="ad676-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="ad676-121">内部ハンドラーでは、応答メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="ad676-121">The inner handler returns a response message.</span></span> <span data-ttu-id="ad676-122">(この手順は、非同期です)。</span><span class="sxs-lookup"><span data-stu-id="ad676-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="ad676-123">応答を処理し、呼び出し元に返します。</span><span class="sxs-lookup"><span data-stu-id="ad676-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="ad676-124">次の例では、送信要求にカスタム ヘッダーを追加するメッセージ ハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="ad676-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="ad676-125">呼び出し`base.SendAsync`は非同期です。</span><span class="sxs-lookup"><span data-stu-id="ad676-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="ad676-126">場合は、ハンドラーは、この呼び出しの後の作業を使用して、 **await**メソッドの完了後に実行を再開するキーワードです。</span><span class="sxs-lookup"><span data-stu-id="ad676-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="ad676-127">次の例は、エラー コードをログに記録するハンドラーを示しています。</span><span class="sxs-lookup"><span data-stu-id="ad676-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="ad676-128">自体のログが非常に興味深いではありませんが、例では、応答ハンドラー内では、取得する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ad676-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="ad676-129">クライアントのパイプラインにメッセージのハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ad676-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="ad676-130">カスタム ハンドラーを追加する**HttpClient**を使用して、 **HttpClientFactory.Create**メソッド。</span><span class="sxs-lookup"><span data-stu-id="ad676-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="ad676-131">メッセージ ハンドラーに渡したりする順序で呼び出されます、**作成**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ad676-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="ad676-132">ハンドラーが入れ子になっているため、応答メッセージは逆方向に移動します。</span><span class="sxs-lookup"><span data-stu-id="ad676-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="ad676-133">その最後のハンドラーは、最初に応答メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="ad676-133">That is, the last handler is the first to get the response message.</span></span>
