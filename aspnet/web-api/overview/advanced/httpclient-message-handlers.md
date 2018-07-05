---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API の HttpClient メッセージ ハンドラー |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831373"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="fc8b1-102">ASP.NET Web API の HttpClient メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="fc8b1-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="fc8b1-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fc8b1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fc8b1-104">A*メッセージ ハンドラー*は HTTP 要求を受信し、HTTP 応答を返すクラスです。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="fc8b1-105">通常、一連のメッセージ ハンドラーが連結されます。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="fc8b1-106">最初のハンドラー HTTP 要求を受信するには、いくつかの処理およびの次のハンドラーへの要求を提供します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="fc8b1-107">いくつかの時点では、応答が作成され、チェーンの上位に戻ります。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="fc8b1-108">このパターンと呼ばれます、*委任*ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="fc8b1-109">クライアント側で、 **HttpClient**クラスはメッセージ ハンドラーを使用して要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="fc8b1-110">既定のハンドラーは**HttpClientHandler**、ネットワーク経由で要求を送信して、サーバーからの応答を取得します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="fc8b1-111">クライアント パイプラインにカスタム メッセージ ハンドラーを挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="fc8b1-112">また、ASP.NET Web API は、サーバー側でメッセージのハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="fc8b1-113">詳細については、次を参照してください。 [HTTP メッセージ ハンドラー](http-message-handlers.md)します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="fc8b1-114">カスタム メッセージ ハンドラー</span><span class="sxs-lookup"><span data-stu-id="fc8b1-114">Custom Message Handlers</span></span>

<span data-ttu-id="fc8b1-115">派生するカスタム メッセージ ハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッド。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="fc8b1-116">メソッド シグネチャを次に示します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="fc8b1-117">メソッドには、 **HttpRequestMessage**として入力し、非同期的に返します、 **HttpResponseMessage**します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="fc8b1-118">一般的な実装は、次を行います。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="fc8b1-119">要求メッセージを処理します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-119">Process the request message.</span></span>
2. <span data-ttu-id="fc8b1-120">呼び出す`base.SendAsync`内部ハンドラーに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="fc8b1-121">内部ハンドラーは、応答メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-121">The inner handler returns a response message.</span></span> <span data-ttu-id="fc8b1-122">(この手順は、非同期です)。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="fc8b1-123">応答を処理し、呼び出し元に戻すこと。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="fc8b1-124">次の例では、送信要求にカスタム ヘッダーを追加するメッセージ ハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="fc8b1-125">呼び出し`base.SendAsync`は非同期です。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="fc8b1-126">存在する場合、ハンドラーはこの呼び出しの後の作業を使用して、 **await**キーワードをメソッドの完了後に実行を再開します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="fc8b1-127">次の例では、コードのエラー ログに記録するハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="fc8b1-128">ログ自体が非常に興味深いではありませんが、ハンドラー内で応答を取得する方法の例に示します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="fc8b1-129">クライアントのパイプラインへのメッセージ ハンドラーの追加</span><span class="sxs-lookup"><span data-stu-id="fc8b1-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="fc8b1-130">カスタム ハンドラーを追加する**HttpClient**を使用して、 **HttpClientFactory.Create**メソッド。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="fc8b1-131">メッセージ ハンドラーは順番に渡す、**作成**メソッド。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="fc8b1-132">ハンドラーは入れ子になったため、応答メッセージは、他の方向に移動します。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="fc8b1-133">これは最後のハンドラーでは、応答メッセージを取得する 1 つ目があります。</span><span class="sxs-lookup"><span data-stu-id="fc8b1-133">That is, the last handler is the first to get the response message.</span></span>
