---
title: SignalR HubContext
author: rachelappel
description: ハブの外部からのクライアントに通知を送信するための ASP.NET Core SignalR HubContext サービスを使用する方法を説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726082"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="a53f6-103">ハブの外部からのメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="a53f6-103">Send messages from outside a hub</span></span>

<span data-ttu-id="a53f6-104">によって[Mikael メンギストゥ](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="a53f6-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="a53f6-105">SignalR ハブは、SignalR のサーバーに接続しているクライアントにメッセージを送信するための中核となる抽象型です。</span><span class="sxs-lookup"><span data-stu-id="a53f6-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="a53f6-106">アプリを使用して、他の場所からメッセージを送信することも、`IHubContext`サービス。</span><span class="sxs-lookup"><span data-stu-id="a53f6-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="a53f6-107">この記事は、SignalR にアクセスする方法を説明します`IHubContext`ハブ外からのクライアントに通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="a53f6-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="a53f6-108">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a53f6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="a53f6-109">インスタンスを取得します。 `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="a53f6-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="a53f6-110">ASP.NET Core SignalR でのインスタンスにアクセスすることができます`IHubContext`依存関係の挿入を使用しています。</span><span class="sxs-lookup"><span data-stu-id="a53f6-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="a53f6-111">インスタンスを挿入する`IHubContext`コント ローラー、ミドルウェア、またはその他の DI サービスにします。</span><span class="sxs-lookup"><span data-stu-id="a53f6-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="a53f6-112">インスタンスを使用して、クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="a53f6-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="a53f6-113">ASP.NET SignalR GlobalHost へのアクセスを提供するために使用される一方、`IHubContext`です。</span><span class="sxs-lookup"><span data-stu-id="a53f6-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="a53f6-114">ASP.NET Core では、このグローバル シングルトンの必要性を削除する依存関係インジェクション フレームワークがあります。</span><span class="sxs-lookup"><span data-stu-id="a53f6-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="a53f6-115">インスタンスを挿入`IHubContext`コント ローラー</span><span class="sxs-lookup"><span data-stu-id="a53f6-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="a53f6-116">インスタンスを挿入する`IHubContext`コンス トラクターに追加することでコント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="a53f6-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="a53f6-117">インスタンスにアクセスできるようになりました、 `IHubContext`、自体のハブ必要がある場合は、ハブ メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a53f6-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="a53f6-118">インスタンスを取得`IHubContext`ミドルウェア内で</span><span class="sxs-lookup"><span data-stu-id="a53f6-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="a53f6-119">アクセス、`IHubContext`ミドルウェア パイプライン内で次のようにします。</span><span class="sxs-lookup"><span data-stu-id="a53f6-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="a53f6-120">ハブ メソッドの外部から呼び出されるときに、`Hub`クラスは、呼び出しに関連付けられている呼び出し元はありません。</span><span class="sxs-lookup"><span data-stu-id="a53f6-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="a53f6-121">したがってへのアクセスはありません、 `ConnectionId`、 `Caller`、および`Others`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="a53f6-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="a53f6-122">関連資料</span><span class="sxs-lookup"><span data-stu-id="a53f6-122">Related resources</span></span>

* [<span data-ttu-id="a53f6-123">開始するには</span><span class="sxs-lookup"><span data-stu-id="a53f6-123">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="a53f6-124">ハブ</span><span class="sxs-lookup"><span data-stu-id="a53f6-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a53f6-125">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="a53f6-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
