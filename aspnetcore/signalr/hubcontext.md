---
title: SignalR HubContext
author: tdykstra
description: ハブの外部からのクライアントに通知を送信するための ASP.NET Core SignalR HubContext サービスを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 2d7d37b655bf7dbb71b321919314bbb8bef8db17
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2018
ms.locfileid: "44339980"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="cdf2e-103">ハブの外部からのメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-103">Send messages from outside a hub</span></span>

<span data-ttu-id="cdf2e-104">によって[Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="cdf2e-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="cdf2e-105">SignalR ハブは、SignalR のサーバーに接続しているクライアントにメッセージを送信するための中核となる抽象化です。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="cdf2e-106">アプリを使用して、その他の場所からメッセージを送信することも、`IHubContext`サービス。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="cdf2e-107">この記事は、SignalR にアクセスする方法を説明します`IHubContext`ハブ外からのクライアントに通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="cdf2e-108">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="cdf2e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="cdf2e-109">インスタンスを取得します。 `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="cdf2e-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="cdf2e-110">ASP.NET Core signalr でのインスタンスにアクセスすることができます`IHubContext`依存関係の挿入を使用しています。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="cdf2e-111">インスタンスを挿入できる`IHubContext`コント ローラー、ミドルウェア、またはその他の DI サービスにします。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="cdf2e-112">インスタンスを使用して、クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="cdf2e-113">一方、ASP.NET 4.x GlobalHost にアクセスできるようにするために使用する SignalR、`IHubContext`します。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="cdf2e-114">ASP.NET Core は、このグローバル シングルトンの必要性を削除する依存関係挿入フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="cdf2e-115">インスタンスを挿入する`IHubContext`コント ローラー</span><span class="sxs-lookup"><span data-stu-id="cdf2e-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="cdf2e-116">インスタンスを挿入できる`IHubContext`コンス トラクターに追加して、コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="cdf2e-117">現在のインスタンスへのアクセスで`IHubContext`、ハブ自体で必要がある場合は、ハブ メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="cdf2e-118">インスタンスを取得`IHubContext`ミドルウェア内で</span><span class="sxs-lookup"><span data-stu-id="cdf2e-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="cdf2e-119">アクセス、`IHubContext`ミドルウェア パイプライン内で次のようにします。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="cdf2e-120">ハブ メソッドの外部から呼び出されるときに、`Hub`クラスは、呼び出しに関連付けられている呼び出し元はありません。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="cdf2e-121">そのためへのアクセスはありません、 `ConnectionId`、 `Caller`、および`Others`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="cdf2e-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="cdf2e-122">関連資料</span><span class="sxs-lookup"><span data-stu-id="cdf2e-122">Related resources</span></span>

* [<span data-ttu-id="cdf2e-123">開始するには</span><span class="sxs-lookup"><span data-stu-id="cdf2e-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="cdf2e-124">ハブ</span><span class="sxs-lookup"><span data-stu-id="cdf2e-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="cdf2e-125">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="cdf2e-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
