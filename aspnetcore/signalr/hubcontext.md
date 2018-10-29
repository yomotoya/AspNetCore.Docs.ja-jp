---
title: SignalR HubContext
author: tdykstra
description: ハブの外部からのクライアントに通知を送信するための ASP.NET Core SignalR HubContext サービスを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 8be888e1f7b16d65ebbaa24b618e84fca029d80b
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207954"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="578f0-103">ハブの外部からのメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="578f0-103">Send messages from outside a hub</span></span>

<span data-ttu-id="578f0-104">によって[Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="578f0-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="578f0-105">SignalR ハブは、SignalR のサーバーに接続しているクライアントにメッセージを送信するための中核となる抽象化です。</span><span class="sxs-lookup"><span data-stu-id="578f0-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="578f0-106">アプリを使用して、その他の場所からメッセージを送信することも、`IHubContext`サービス。</span><span class="sxs-lookup"><span data-stu-id="578f0-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="578f0-107">この記事は、SignalR にアクセスする方法を説明します`IHubContext`ハブ外からのクライアントに通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="578f0-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="578f0-108">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="578f0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="578f0-109">IHubContext のインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="578f0-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="578f0-110">ASP.NET Core signalr でのインスタンスにアクセスすることができます`IHubContext`依存関係の挿入を使用しています。</span><span class="sxs-lookup"><span data-stu-id="578f0-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="578f0-111">インスタンスを挿入できる`IHubContext`コント ローラー、ミドルウェア、またはその他の DI サービスにします。</span><span class="sxs-lookup"><span data-stu-id="578f0-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="578f0-112">インスタンスを使用して、クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="578f0-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="578f0-113">一方、ASP.NET 4.x GlobalHost にアクセスできるようにするために使用する SignalR、`IHubContext`します。</span><span class="sxs-lookup"><span data-stu-id="578f0-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="578f0-114">ASP.NET Core は、このグローバル シングルトンの必要性を削除する依存関係挿入フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="578f0-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="578f0-115">IHubContext のコント ローラーのインスタンスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="578f0-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="578f0-116">インスタンスを挿入できる`IHubContext`コンス トラクターに追加して、コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="578f0-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="578f0-117">現在のインスタンスへのアクセスで`IHubContext`、ハブ自体で必要がある場合は、ハブ メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="578f0-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="578f0-118">IHubContext のミドルウェア内でインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="578f0-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="578f0-119">アクセス、`IHubContext`ミドルウェア パイプライン内で次のようにします。</span><span class="sxs-lookup"><span data-stu-id="578f0-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="578f0-120">ハブ メソッドの外部から呼び出されるときに、`Hub`クラスは、呼び出しに関連付けられている呼び出し元はありません。</span><span class="sxs-lookup"><span data-stu-id="578f0-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="578f0-121">そのためへのアクセスはありません、 `ConnectionId`、 `Caller`、および`Others`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="578f0-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="578f0-122">関連資料</span><span class="sxs-lookup"><span data-stu-id="578f0-122">Related resources</span></span>

* [<span data-ttu-id="578f0-123">開始するには</span><span class="sxs-lookup"><span data-stu-id="578f0-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="578f0-124">ハブ</span><span class="sxs-lookup"><span data-stu-id="578f0-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="578f0-125">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="578f0-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
