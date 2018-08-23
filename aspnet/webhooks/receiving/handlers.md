---
uid: webhooks/receiving/handlers
title: ASP.NET Webhook ハンドラー |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook の要求を処理する方法。
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835155"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="4de30-103">ASP.NET Webhook ハンドラー</span><span class="sxs-lookup"><span data-stu-id="4de30-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="4de30-104">WebHook レシーバーによって Webhook 要求が検証されると、ユーザー コードで処理する準備が。</span><span class="sxs-lookup"><span data-stu-id="4de30-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="4de30-105">これは、ような場合*ハンドラー*取得します。</span><span class="sxs-lookup"><span data-stu-id="4de30-105">This is where *handlers* come in.</span></span> <span data-ttu-id="4de30-106">派生してハンドラー、 [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)インターフェイスが、通常は、 [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)クラス、インターフェイスから直接派生する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="4de30-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="4de30-107">1 つまたは複数のハンドラーによって WebHook 要求を処理できます。</span><span class="sxs-lookup"><span data-stu-id="4de30-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="4de30-108">ハンドラーは、それぞれに基づく順序で呼び出されます*順序*と最低から最高順序が (1 から 100 の間に推奨) 単純な整数をプロパティ。</span><span class="sxs-lookup"><span data-stu-id="4de30-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![WebHook ハンドラー順序のプロパティのダイアグラム](_static/Handlers.png)

<span data-ttu-id="4de30-110">ハンドラーを設定できます必要に応じて、*応答*WebHookHandlerContext それにより、処理を停止し、WebHook に対する HTTP 応答として送信する応答のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="4de30-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="4de30-111">上記の例では、ハンドラーの C を B と B セットの応答よりも、高い次数があるために呼び出されますはありません。</span><span class="sxs-lookup"><span data-stu-id="4de30-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="4de30-112">応答の設定は通常のみ関連 Webhook の応答が、元の API に情報を伝達できます。</span><span class="sxs-lookup"><span data-stu-id="4de30-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="4de30-113">たとえば、チャネルからの WebHook の入手先に、応答の送信先 Slack Webhook を使用した場合です。</span><span class="sxs-lookup"><span data-stu-id="4de30-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="4de30-114">のみをその特定の受信側から Webhook を受信する場合、ハンドラーは受信側のプロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="4de30-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="4de30-115">受信側が設定されていない場合はそれらすべてに対して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4de30-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="4de30-116">応答の他の一般的な用途の 1 つは、使用する、 *410 Gone* WebHook が不要になったがアクティブになっており、それ以降の要求を送信しないことを示す応答。</span><span class="sxs-lookup"><span data-stu-id="4de30-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="4de30-117">既定ですべての WebHook レシーバーによってハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4de30-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="4de30-118">ただし場合、*受信者*プロパティがハンドラーの名前に設定し、そのハンドラーがそのレシーバーから WebHook 要求を受信のみです。</span><span class="sxs-lookup"><span data-stu-id="4de30-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="4de30-119">WebHook の処理</span><span class="sxs-lookup"><span data-stu-id="4de30-119">Processing a WebHook</span></span>

<span data-ttu-id="4de30-120">ハンドラーが呼び出されると、取得、 [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) WebHook 要求に関する情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="4de30-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="4de30-121">データ、通常、HTTP 要求本文は、*データ*プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4de30-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="4de30-122">データの型は通常 JSON または HTML フォーム データでは、ですが、必要な場合より具体的な型にキャストすることができます。</span><span class="sxs-lookup"><span data-stu-id="4de30-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="4de30-123">ASP.NET Webhook によって生成されたカスタム Webhook を型にキャストするなど、 [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs)次のようにします。</span><span class="sxs-lookup"><span data-stu-id="4de30-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="4de30-124">キューに置かれた処理</span><span class="sxs-lookup"><span data-stu-id="4de30-124">Queued Processing</span></span>

<span data-ttu-id="4de30-125">秒の少数の応答が生成されない場合は、ほとんどの WebHook の送信者は、WebHook を再送信します。</span><span class="sxs-lookup"><span data-stu-id="4de30-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="4de30-126">つまり、ハンドラーがもう一度呼び出すことのない順序でタイム フレーム内での処理を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4de30-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="4de30-127">場合は、処理時間はかかりますか個別に処理が適しています、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)など、キューに WebHook 要求を送信するために使用できる[Azure Storage キュー](https://msdn.microsoft.com/library/azure/dd179353.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4de30-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="4de30-128">概要、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)実装はここで提供されます。</span><span class="sxs-lookup"><span data-stu-id="4de30-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
