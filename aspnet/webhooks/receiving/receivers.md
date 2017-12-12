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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="21fd6-103">ASP.NET Webhook レシーバー</span><span class="sxs-lookup"><span data-stu-id="21fd6-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="21fd6-104">Webhook の受信は、送信者によって異なります。</span><span class="sxs-lookup"><span data-stu-id="21fd6-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="21fd6-105">サブスクライバーが実際にリッスンしていることを確認する WebHook を登録する追加の手順がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="21fd6-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="21fd6-106">一部の Webhook は、HTTP POST 要求を個別に取得するが、イベント情報への参照が含まれるのみプッシュ、プル モデルを提供します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="21fd6-107">多くの場合、セキュリティ モデルは少し異なります。</span><span class="sxs-lookup"><span data-stu-id="21fd6-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="21fd6-108">Microsoft ASP.NET の Webhook の目的は、簡単かつより一貫性のある多くの Webhook の特定のバリエーションを処理する方法を検討する時間を費やすことがなく、API を接続するための両方ようにすることです。</span><span class="sxs-lookup"><span data-stu-id="21fd6-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="21fd6-109">WebHook の受信者を受信し、特定の差出人から Webhook を確認します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="21fd6-110">WebHook の受信者は、それぞれは独自の構成の Webhook の任意の数をサポートできます。</span><span class="sxs-lookup"><span data-stu-id="21fd6-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="21fd6-111">たとえば、GitHub WebHook 受信者は、GitHub リポジトリの任意の数から Webhook を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="21fd6-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="21fd6-112">WebHook 受信者 Uri</span><span class="sxs-lookup"><span data-stu-id="21fd6-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="21fd6-113">Microsoft ASP.NET Webhook をインストールすることでは、制約のない数のサービスからの WebHook の要求を受け入れ全般 WebHook コント ローラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="21fd6-114">要求が到着すると、特定の WebHook 送信者を処理するためにインストールされている適切な受信者を取得します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="21fd6-115">このコント ローラーの URI は、サービスに登録する WebHook の URI がありの形式では。</span><span class="sxs-lookup"><span data-stu-id="21fd6-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="21fd6-116">セキュリティ上の理由から、WebHook の多数の受信者ことが必要、URI、 *https* URI 場合によってはその必要がありますを含めることも、意図したパーティは、上記の URI に Webhook を送信することができますのみを適用するために使用する追加のクエリ パラメーター.</span><span class="sxs-lookup"><span data-stu-id="21fd6-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="21fd6-117"> *<receiver>* コンポーネントは、受信者の名前。 たとえば*github*または*slack*です。</span><span class="sxs-lookup"><span data-stu-id="21fd6-117">The *<receiver>* component is the name of the receiver, for example *github* or *slack*.</span></span>

<span data-ttu-id="21fd6-118">*{Id}*特定 WebHook 受信側の構成を識別するために使用する省略可能な識別子です。</span><span class="sxs-lookup"><span data-stu-id="21fd6-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="21fd6-119">特定の受信者に N Webhook を登録するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="21fd6-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="21fd6-120">たとえば、次の 3 つの Uri は、次の 3 つの独立した Webhook の登録を使用できます。</span><span class="sxs-lookup"><span data-stu-id="21fd6-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="21fd6-121">WebHook 受信機をインストールします。</span><span class="sxs-lookup"><span data-stu-id="21fd6-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="21fd6-122">Microsoft ASP.NET Webhook を使用する Webhook を受信するには、まず、WebHook プロバイダーまたはプロバイダーからの Webhook を受信するための Nuget パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="21fd6-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="21fd6-123">Nuget パッケージの名前は[Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)最後の部分が、サービスがサポートされていることを示します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="21fd6-124">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-124">For example</span></span>

<span data-ttu-id="21fd6-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) Webhook を GitHub から受信するためのサポートを提供し、 [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) ASP によって生成された Webhook を受信するためのサポートを提供します。NET Webhook です。</span><span class="sxs-lookup"><span data-stu-id="21fd6-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="21fd6-126">すぐには、Dropbox、GitHub、MailChimp、PayPal、Pusher、Salesforce、余裕期間、ストライプ、Trello、および WordPress のサポートを見つけることができますが、任意の数の他のプロバイダーをサポートすることです。</span><span class="sxs-lookup"><span data-stu-id="21fd6-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="21fd6-127">WebHook の受信者を構成します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="21fd6-128">WebHook のレシーバーは構成、 [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs)任意の依存関係の挿入のモデルを使用して、インターフェイスとそのインターフェイスの特定の実装を登録できます。</span><span class="sxs-lookup"><span data-stu-id="21fd6-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="21fd6-129">既定の実装が Web.config ファイルで設定することができますか、または、Azure Web Apps を使用する場合を使って設定できるアプリケーションの設定を使用して、 [Azure Portal](https://portal.azure.com/)です。</span><span class="sxs-lookup"><span data-stu-id="21fd6-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Azure アプリの設定](_static/AzureAppSettings.png)

<span data-ttu-id="21fd6-131">アプリケーション設定キーの形式は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="21fd6-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="21fd6-132">値は、一致する値のコンマ区切りのリスト、 *{id}*する Webhook 登録されているなどの値。</span><span class="sxs-lookup"><span data-stu-id="21fd6-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="21fd6-133">WebHook 受信者の初期化</span><span class="sxs-lookup"><span data-stu-id="21fd6-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="21fd6-134">WebHook の受信側が登録することで、通常初期化されて、 *WebApiConfig*静的クラス、例を示します。</span><span class="sxs-lookup"><span data-stu-id="21fd6-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
