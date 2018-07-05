---
uid: webhooks/receiving/handlers
title: ASP.NET Webhook ハンドラー |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook の要求を処理する方法。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.openlocfilehash: 7e45a97ac9d61b2d046984e5ede3be158b741b7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372700"
---
# <a name="aspnet-webhooks-handlers"></a>ASP.NET Webhook ハンドラー

WebHook レシーバーによって Webhook 要求が検証されると、ユーザー コードで処理する準備が。 これは、ような場合*ハンドラー*取得します。 派生してハンドラー、 [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)インターフェイスが、通常は、 [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs)クラス、インターフェイスから直接派生する代わりにします。

1 つまたは複数のハンドラーによって WebHook 要求を処理できます。 ハンドラーは、それぞれに基づく順序で呼び出されます*順序*と最低から最高順序が (1 から 100 の間に推奨) 単純な整数をプロパティ。

![WebHook ハンドラー順序のプロパティのダイアグラム](_static/Handlers.png)

ハンドラーを設定できます必要に応じて、*応答*WebHookHandlerContext それにより、処理を停止し、WebHook に対する HTTP 応答として送信する応答のプロパティ。 上記の例では、ハンドラーの C を B と B セットの応答よりも、高い次数があるために呼び出されますはありません。

応答の設定は通常のみ関連 Webhook の応答が、元の API に情報を伝達できます。 たとえば、チャネルからの WebHook の入手先に、応答の送信先 Slack Webhook を使用した場合です。 のみをその特定の受信側から Webhook を受信する場合、ハンドラーは受信側のプロパティを設定できます。 受信側が設定されていない場合はそれらすべてに対して呼び出されます。

応答の他の一般的な用途の 1 つは、使用する、 *410 Gone* WebHook が不要になったがアクティブになっており、それ以降の要求を送信しないことを示す応答。

既定ですべての WebHook レシーバーによってハンドラーが呼び出されます。 ただし場合、*受信者*プロパティがハンドラーの名前に設定し、そのハンドラーがそのレシーバーから WebHook 要求を受信のみです。

## <a name="processing-a-webhook"></a>WebHook の処理

ハンドラーが呼び出されると、取得、 [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) WebHook 要求に関する情報を格納します。 データ、通常、HTTP 要求本文は、*データ*プロパティ。

データの型は通常 JSON または HTML フォーム データでは、ですが、必要な場合より具体的な型にキャストすることができます。 ASP.NET Webhook によって生成されたカスタム Webhook を型にキャストするなど、 [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs)次のようにします。

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

秒の少数の応答が生成されない場合は、ほとんどの WebHook の送信者は、WebHook を再送信します。 つまり、ハンドラーがもう一度呼び出すことのない順序でタイム フレーム内での処理を完了する必要があります。

場合は、処理時間はかかりますか個別に処理が適しています、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)など、キューに WebHook 要求を送信するために使用できる[Azure Storage キュー](https://msdn.microsoft.com/library/azure/dd179353.aspx)します。

概要、 [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)実装はここで提供されます。

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
