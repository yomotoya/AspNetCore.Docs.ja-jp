---
uid: webhooks/receiving/receivers
title: ASP.NET Webhook レシーバー |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook レシーバー
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.openlocfilehash: be1f7dcef60642231af9c593eb7329ede7c2094f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374744"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="fa939-103">ASP.NET Webhook レシーバー</span><span class="sxs-lookup"><span data-stu-id="fa939-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="fa939-104">Webhook を受信は、送信者に依存します。</span><span class="sxs-lookup"><span data-stu-id="fa939-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="fa939-105">サブスクライバーが実際にリッスンしていることを確認する WebHook を登録する追加の手順がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="fa939-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="fa939-106">いくつかの Webhook では、HTTP POST 要求には個別に取得するが、イベント情報への参照が含まれてのみプッシュ/プル モデルを提供します。</span><span class="sxs-lookup"><span data-stu-id="fa939-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="fa939-107">多くの場合、セキュリティ モデルが大幅に異なります。</span><span class="sxs-lookup"><span data-stu-id="fa939-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="fa939-108">Microsoft ASP.NET Webhook の目的は、単純なより一貫性のある多くの Webhook の特定のバリエーションを処理する方法を見極める時間をかけることがなく、API を構成するようにすることです。</span><span class="sxs-lookup"><span data-stu-id="fa939-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="fa939-109">WebHook レシーバーは、受け入れ、特定の差出人からの Webhook の検証を担当します。</span><span class="sxs-lookup"><span data-stu-id="fa939-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="fa939-110">WebHook レシーバーは、任意の数の Webhook は、それぞれに独自の構成をサポートできます。</span><span class="sxs-lookup"><span data-stu-id="fa939-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="fa939-111">たとえば、GitHub の WebHook レシーバーは、任意の数の GitHub リポジトリからの Webhook を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fa939-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="fa939-112">WebHook レシーバーの Uri</span><span class="sxs-lookup"><span data-stu-id="fa939-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="fa939-113">Microsoft ASP.NET Webhook をインストールすることで拡張可能なさまざまなサービスからの WebHook 要求を受け取る一般的な WebHook コント ローラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="fa939-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="fa939-114">要求が到着すると、特定の WebHook 送信者を処理するためにインストールされている適切な受信者を取得します。</span><span class="sxs-lookup"><span data-stu-id="fa939-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="fa939-115">このコント ローラーの URI は、サービスに登録する、WebHook URI し、の形式は。</span><span class="sxs-lookup"><span data-stu-id="fa939-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="fa939-116">セキュリティ上の理由から、多くの WebHook レシーバーことが要求 URI、 *https* URI によっても含める必要があります、意図したパーティは上記の URI に Webhook を送信することができますのみを適用するために使用する追加のクエリ パラメーターと.</span><span class="sxs-lookup"><span data-stu-id="fa939-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="fa939-117"><em> <receiver> </em>コンポーネントは、受信者の名前、たとえば<em>github</em>または<em>slack</em>します。</span><span class="sxs-lookup"><span data-stu-id="fa939-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="fa939-118">*{Id}* 特定の WebHook レシーバー構成を識別するために使用できる省略可能な識別子です。</span><span class="sxs-lookup"><span data-stu-id="fa939-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="fa939-119">特定の受信側に n 個の Webhook を登録するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fa939-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="fa939-120">たとえば、次の 3 つの独立した Webhook を登録する次の 3 つの Uri を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fa939-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="fa939-121">WebHook レシーバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fa939-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="fa939-122">Microsoft ASP.NET Webhook を使用して Webhook を受信するには、最初の WebHook プロバイダーまたはプロバイダーからの Webhook を受信するための Nuget パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fa939-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="fa939-123">Nuget パッケージの名前は[Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)最後の部分が、サービスがサポートされていることを示します。</span><span class="sxs-lookup"><span data-stu-id="fa939-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="fa939-124">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fa939-124">For example</span></span>

<span data-ttu-id="fa939-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="fa939-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="fa939-126">すぐには、Dropbox、GitHub、MailChimp、PayPal、Pusher、Salesforce、Slack、ストライプ、Trello、および WordPress のサポートを見つけることができますが、他のプロバイダーの任意の数をサポートするために得ます。</span><span class="sxs-lookup"><span data-stu-id="fa939-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="fa939-127">WebHook レシーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="fa939-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="fa939-128">を介して WebHook レシーバーが構成されている、 [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs)任意の依存関係の挿入のモデルを使用して、インターフェイスとそのインターフェイスの特定の実装を登録できます。</span><span class="sxs-lookup"><span data-stu-id="fa939-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="fa939-129">既定の実装は、Web.config ファイルで設定できますか、または、Azure Web Apps を使用する場合は、使用して設定できるアプリケーションの設定を使用して、 [Azure Portal](https://portal.azure.com/)します。</span><span class="sxs-lookup"><span data-stu-id="fa939-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Azure アプリの設定](_static/AzureAppSettings.png)

<span data-ttu-id="fa939-131">アプリケーション設定キーの形式は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fa939-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="fa939-132">値が一致する値のコンマ区切りの一覧、 *{id}* を Webhook 登録されているなどの値。</span><span class="sxs-lookup"><span data-stu-id="fa939-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="fa939-133">WebHook レシーバーの初期化</span><span class="sxs-lookup"><span data-stu-id="fa939-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="fa939-134">登録することにより、通常の WebHook レシーバーが初期化されます、 *WebApiConfig*などの静的クラスします。</span><span class="sxs-lookup"><span data-stu-id="fa939-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
