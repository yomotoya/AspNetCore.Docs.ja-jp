---
uid: webhooks/receiving/receivers
title: "ASP.NET Webhook レシーバー |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET Webhook レシーバー"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a>ASP.NET Webhook レシーバー

Webhook の受信は、送信者によって異なります。 サブスクライバーが実際にリッスンしていることを確認する WebHook を登録する追加の手順がある場合があります。 一部の Webhook は、HTTP POST 要求を個別に取得するが、イベント情報への参照が含まれるのみプッシュ、プル モデルを提供します。 多くの場合、セキュリティ モデルは少し異なります。

Microsoft ASP.NET の Webhook の目的は、簡単かつより一貫性のある多くの Webhook の特定のバリエーションを処理する方法を検討する時間を費やすことがなく、API を接続するための両方ようにすることです。

WebHook の受信者を受信し、特定の差出人から Webhook を確認します。 WebHook の受信者は、それぞれは独自の構成の Webhook の任意の数をサポートできます。 たとえば、GitHub WebHook 受信者は、GitHub リポジトリの任意の数から Webhook を受け入れることができます。

## <a name="webhook-receiver-uris"></a>WebHook 受信者 Uri

Microsoft ASP.NET Webhook をインストールすることでは、制約のない数のサービスからの WebHook の要求を受け入れ全般 WebHook コント ローラーを取得します。 要求が到着すると、特定の WebHook 送信者を処理するためにインストールされている適切な受信者を取得します。

このコント ローラーの URI は、サービスに登録する WebHook の URI がありの形式では。

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

セキュリティ上の理由から、WebHook の多数の受信者ことが必要、URI、 *https* URI 場合によってはその必要がありますを含めることも、意図したパーティは、上記の URI に Webhook を送信することができますのみを適用するために使用する追加のクエリ パラメーター.

 *<receiver>* コンポーネントは、受信者の名前。 たとえば*github*または*slack*です。

*{Id}*特定 WebHook 受信側の構成を識別するために使用する省略可能な識別子です。 特定の受信者に N Webhook を登録するために使用できます。 たとえば、次の 3 つの Uri は、次の 3 つの独立した Webhook の登録を使用できます。

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>WebHook 受信機をインストールします。

Microsoft ASP.NET Webhook を使用する Webhook を受信するには、まず、WebHook プロバイダーまたはプロバイダーからの Webhook を受信するための Nuget パッケージをインストールします。 Nuget パッケージの名前は[Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)最後の部分が、サービスがサポートされていることを示します。 次に例を示します。

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Webhook を GitHub から受信するためのサポートを提供し、 [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) ASP によって生成された Webhook を受信するためのサポートを提供します。NET Webhook です。

すぐには、Dropbox、GitHub、MailChimp、PayPal、Pusher、Salesforce、余裕期間、ストライプ、Trello、および WordPress のサポートを見つけることができますが、任意の数の他のプロバイダーをサポートすることです。

## <a name="configuring-a-webhook-receiver"></a>WebHook の受信者を構成します。

WebHook のレシーバーは構成、 [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs)任意の依存関係の挿入のモデルを使用して、インターフェイスとそのインターフェイスの特定の実装を登録できます。 既定の実装が Web.config ファイルで設定することができますか、または、Azure Web Apps を使用する場合を使って設定できるアプリケーションの設定を使用して、 [Azure Portal](https://portal.azure.com/)です。

![Azure アプリの設定](_static/AzureAppSettings.png)

アプリケーション設定キーの形式は次のとおりです。

```
MS_WebHookReceiverSecret_<receiver>
```

値は、一致する値のコンマ区切りのリスト、 *{id}*する Webhook 登録されているなどの値。

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>WebHook 受信者の初期化

WebHook の受信側が登録することで、通常初期化されて、 *WebApiConfig*静的クラス、例を示します。

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```