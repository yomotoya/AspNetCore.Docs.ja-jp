---
title: SignalR HubContext
author: rachelappel
description: ハブの外部からのクライアントに通知を送信するための ASP.NET Core SignalR HubContext サービスを使用する方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277762"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="3045c-103">ハブの外部からのメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="3045c-103">Send messages from outside a hub</span></span>

<span data-ttu-id="3045c-104">によって[Mikael メンギストゥ](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="3045c-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="3045c-105">SignalR ハブは、SignalR のサーバーに接続しているクライアントにメッセージを送信するための中核となる抽象型です。</span><span class="sxs-lookup"><span data-stu-id="3045c-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="3045c-106">アプリを使用して、他の場所からメッセージを送信することも、`IHubContext`サービス。</span><span class="sxs-lookup"><span data-stu-id="3045c-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="3045c-107">この記事は、SignalR にアクセスする方法を説明します`IHubContext`ハブ外からのクライアントに通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="3045c-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="3045c-108">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3045c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="3045c-109">インスタンスを取得します。 `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="3045c-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="3045c-110">ASP.NET Core SignalR でのインスタンスにアクセスすることができます`IHubContext`依存関係の挿入を使用しています。</span><span class="sxs-lookup"><span data-stu-id="3045c-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="3045c-111">インスタンスを挿入する`IHubContext`コント ローラー、ミドルウェア、またはその他の DI サービスにします。</span><span class="sxs-lookup"><span data-stu-id="3045c-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="3045c-112">インスタンスを使用して、クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="3045c-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="3045c-113">ASP.NET SignalR GlobalHost へのアクセスを提供するために使用される一方、`IHubContext`です。</span><span class="sxs-lookup"><span data-stu-id="3045c-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="3045c-114">ASP.NET Core では、このグローバル シングルトンの必要性を削除する依存関係インジェクション フレームワークがあります。</span><span class="sxs-lookup"><span data-stu-id="3045c-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="3045c-115">インスタンスを挿入`IHubContext`コント ローラー</span><span class="sxs-lookup"><span data-stu-id="3045c-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="3045c-116">インスタンスを挿入する`IHubContext`コンス トラクターに追加することでコント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="3045c-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="3045c-117">インスタンスにアクセスできるようになりました、 `IHubContext`、自体のハブ必要がある場合は、ハブ メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="3045c-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="3045c-118">インスタンスを取得`IHubContext`ミドルウェア内で</span><span class="sxs-lookup"><span data-stu-id="3045c-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="3045c-119">アクセス、`IHubContext`ミドルウェア パイプライン内で次のようにします。</span><span class="sxs-lookup"><span data-stu-id="3045c-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="3045c-120">ハブ メソッドの外部から呼び出されるときに、`Hub`クラスは、呼び出しに関連付けられている呼び出し元はありません。</span><span class="sxs-lookup"><span data-stu-id="3045c-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="3045c-121">したがってへのアクセスはありません、 `ConnectionId`、 `Caller`、および`Others`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="3045c-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3045c-122">関連資料</span><span class="sxs-lookup"><span data-stu-id="3045c-122">Related resources</span></span>

* [<span data-ttu-id="3045c-123">開始するには</span><span class="sxs-lookup"><span data-stu-id="3045c-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="3045c-124">ハブ</span><span class="sxs-lookup"><span data-stu-id="3045c-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3045c-125">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="3045c-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
