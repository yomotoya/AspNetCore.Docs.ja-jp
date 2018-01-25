---
uid: webhooks/receiving/handlers
title: "ASP.NET Webhook ハンドラー |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET の Webhook の要求を処理する方法。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 12acae0883c12698a8f9c2150623ba792303e7ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="5d265-103">ASP.NET Webhook ハンドラー</span><span class="sxs-lookup"><span data-stu-id="5d265-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="5d265-104">Webhook の要求の WebHook の受信側で検証した後、ユーザー コードで処理する準備ができてです。</span><span class="sxs-lookup"><span data-stu-id="5d265-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="5d265-105">ここでは*ハンドラー*取得します。</span><span class="sxs-lookup"><span data-stu-id="5d265-105">This is where *handlers* come in.</span></span> <span data-ttu-id="5d265-106">派生してハンドラー、 [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)インターフェイスが、通常は、 [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)クラス、インターフェイスから直接派生する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="5d265-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="5d265-107">WebHook 要求は、1 つまたは複数のハンドラーで処理できます。</span><span class="sxs-lookup"><span data-stu-id="5d265-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="5d265-108">ハンドラーは、それぞれに基づく順序で呼び出されます*順序*移動を小さい方から高い順序 (1 ~ 100 を指定する推奨) 単純な整数は、場所のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="5d265-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![WebHook ハンドラー順序のプロパティのダイアグラム](_static/Handlers.png)

<span data-ttu-id="5d265-110">ハンドラーを設定できます必要に応じて、*応答*WebHookHandlerContext それにより、処理を停止し、WebHook を HTTP 応答として送信する応答のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="5d265-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="5d265-111">上記の例でハンドラー C 呼び出されませんが、応答を設定し、B よりも、高い次数があるためです。</span><span class="sxs-lookup"><span data-stu-id="5d265-111">In the case above, Handler C won’t get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="5d265-112">応答の設定は通常のみ Webhook の応答が送信元の API に情報を伝達できます。</span><span class="sxs-lookup"><span data-stu-id="5d265-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="5d265-113">たとえば、チャネル、WebHook が送信元に戻す応答の送信先 Slack Webhook の場合です。</span><span class="sxs-lookup"><span data-stu-id="5d265-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="5d265-114">ハンドラーは、だけをその特定の受信者から Webhook を受信する場合、受信者のプロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="5d265-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="5d265-115">受信者が設定されていない場合は、それらのすべての呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5d265-115">If they don’t set the receiver they are called for all of them.</span></span>

<span data-ttu-id="5d265-116">応答の他の一般的な用途の 1 つを使用して、 *410 Gone* WebHook が不要になったがアクティブであり、これ以上の要求が送信することなしを示すために応答します。</span><span class="sxs-lookup"><span data-stu-id="5d265-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="5d265-117">既定では、ハンドラーがすべての WebHook 受信者が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5d265-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="5d265-118">ただし場合、*受信者*プロパティがハンドラーの名前に設定し、そのハンドラーがその受信者から WebHook 要求を受信のみです。</span><span class="sxs-lookup"><span data-stu-id="5d265-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="5d265-119">WebHook の処理</span><span class="sxs-lookup"><span data-stu-id="5d265-119">Processing a WebHook</span></span>

<span data-ttu-id="5d265-120">ハンドラーが呼び出されると、取得、 [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) WebHook 要求に関する情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="5d265-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="5d265-121">データ、通常、HTTP 要求本文はから利用可能な*データ*プロパティです。</span><span class="sxs-lookup"><span data-stu-id="5d265-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="5d265-122">データの種類は通常 JSON または HTML フォームのデータですが、必要な場合より具体的な種類にキャストすることは。</span><span class="sxs-lookup"><span data-stu-id="5d265-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="5d265-123">ASP.NET Webhook によって生成されたカスタムの Webhook を型にキャストするなど、 [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs)次のようにします。</span><span class="sxs-lookup"><span data-stu-id="5d265-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

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

  ## <a name="queued-processing"></a><span data-ttu-id="5d265-124">キューに置かれた処理</span><span class="sxs-lookup"><span data-stu-id="5d265-124">Queued Processing</span></span>

<span data-ttu-id="5d265-125">秒の少数の応答が生成されない場合は、ほとんどの WebHook センダは、WebHook を再送信します。</span><span class="sxs-lookup"><span data-stu-id="5d265-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="5d265-126">つまり、ハンドラーがもう一度呼び出されることのない順序でタイム フレーム内での処理を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d265-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="5d265-127">処理時間がかかります、または個別に処理が向上する場合、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)など、キューに WebHook の要求を送信するために使用できる[Azure ストレージ キュー](https://msdn.microsoft.com/library/azure/dd179353.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="5d265-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="5d265-128">概要、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)実装はここで説明します。</span><span class="sxs-lookup"><span data-stu-id="5d265-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

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
