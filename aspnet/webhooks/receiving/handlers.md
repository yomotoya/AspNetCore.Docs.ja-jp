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
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET Webhook ハンドラー

Webhook の要求の WebHook の受信側で検証した後、ユーザー コードで処理する準備ができてです。 ここでは*ハンドラー*取得します。 派生してハンドラー、 [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)インターフェイスが、通常は、 [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)クラス、インターフェイスから直接派生する代わりにします。

WebHook 要求は、1 つまたは複数のハンドラーで処理できます。 ハンドラーは、それぞれに基づく順序で呼び出されます*順序*移動を小さい方から高い順序 (1 ~ 100 を指定する推奨) 単純な整数は、場所のプロパティ。

![WebHook ハンドラー順序のプロパティのダイアグラム](_static/Handlers.png)

ハンドラーを設定できます必要に応じて、*応答*WebHookHandlerContext それにより、処理を停止し、WebHook を HTTP 応答として送信する応答のプロパティです。 上記の例でハンドラー C 呼び出されませんが、応答を設定し、B よりも、高い次数があるためです。

応答の設定は通常のみ Webhook の応答が送信元の API に情報を伝達できます。 たとえば、チャネル、WebHook が送信元に戻す応答の送信先 Slack Webhook の場合です。 ハンドラーは、だけをその特定の受信者から Webhook を受信する場合、受信者のプロパティを設定できます。 受信者が設定されていない場合は、それらのすべての呼び出されます。

応答の他の一般的な用途の 1 つを使用して、 *410 Gone* WebHook が不要になったがアクティブであり、これ以上の要求が送信することなしを示すために応答します。

既定では、ハンドラーがすべての WebHook 受信者が呼び出されます。 ただし場合、*受信者*プロパティがハンドラーの名前に設定し、そのハンドラーがその受信者から WebHook 要求を受信のみです。

## <a name="processing-a-webhook"></a>WebHook の処理

ハンドラーが呼び出されると、取得、 [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) WebHook 要求に関する情報を格納します。 データ、通常、HTTP 要求本文はから利用可能な*データ*プロパティです。

データの種類は通常 JSON または HTML フォームのデータですが、必要な場合より具体的な種類にキャストすることは。 ASP.NET Webhook によって生成されたカスタムの Webhook を型にキャストするなど、 [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs)次のようにします。

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

  ## <a name="queued-processing"></a>キューに置かれた処理

秒の少数の応答が生成されない場合は、ほとんどの WebHook センダは、WebHook を再送信します。 つまり、ハンドラーがもう一度呼び出されることのない順序でタイム フレーム内での処理を完了する必要があります。

処理時間がかかります、または個別に処理が向上する場合、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)など、キューに WebHook の要求を送信するために使用できる[Azure ストレージ キュー](https://msdn.microsoft.com/library/azure/dd179353.aspx)です。

概要、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)実装はここで説明します。

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
