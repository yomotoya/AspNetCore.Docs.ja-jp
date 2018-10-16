---
title: SignalR HubContext
author: tdykstra
description: ハブの外部からのクライアントに通知を送信するための ASP.NET Core SignalR HubContext サービスを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: bb07a3b5c6e153092635fa4e1283619777865a53
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325355"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="f2277-103">ハブの外部からのメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="f2277-103">Send messages from outside a hub</span></span>

<span data-ttu-id="f2277-104">によって[Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="f2277-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="f2277-105">SignalR ハブは、SignalR のサーバーに接続しているクライアントにメッセージを送信するための中核となる抽象化です。</span><span class="sxs-lookup"><span data-stu-id="f2277-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="f2277-106">アプリを使用して、その他の場所からメッセージを送信することも、`IHubContext`サービス。</span><span class="sxs-lookup"><span data-stu-id="f2277-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="f2277-107">この記事は、SignalR にアクセスする方法を説明します`IHubContext`ハブ外からのクライアントに通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="f2277-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="f2277-108">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="f2277-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="f2277-109">IHubContext のインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="f2277-109">Get an instance of IHubContext</span></span>

<span data-ttu-id="f2277-110">ASP.NET Core signalr でのインスタンスにアクセスすることができます`IHubContext`依存関係の挿入を使用しています。</span><span class="sxs-lookup"><span data-stu-id="f2277-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="f2277-111">インスタンスを挿入できる`IHubContext`コント ローラー、ミドルウェア、またはその他の DI サービスにします。</span><span class="sxs-lookup"><span data-stu-id="f2277-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="f2277-112">インスタンスを使用して、クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="f2277-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="f2277-113">一方、ASP.NET 4.x GlobalHost にアクセスできるようにするために使用する SignalR、`IHubContext`します。</span><span class="sxs-lookup"><span data-stu-id="f2277-113">This differs from ASP.NET 4.x SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="f2277-114">ASP.NET Core は、このグローバル シングルトンの必要性を削除する依存関係挿入フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="f2277-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="f2277-115">IHubContext のコント ローラーのインスタンスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="f2277-115">Inject an instance of IHubContext in a controller</span></span>

<span data-ttu-id="f2277-116">インスタンスを挿入できる`IHubContext`コンス トラクターに追加して、コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="f2277-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="f2277-117">現在のインスタンスへのアクセスで`IHubContext`、ハブ自体で必要がある場合は、ハブ メソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="f2277-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="f2277-118">IHubContext のミドルウェア内でインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="f2277-118">Get an instance of IHubContext in middleware</span></span>

<span data-ttu-id="f2277-119">アクセス、`IHubContext`ミドルウェア パイプライン内で次のようにします。</span><span class="sxs-lookup"><span data-stu-id="f2277-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="f2277-120">ハブ メソッドの外部から呼び出されるときに、`Hub`クラスは、呼び出しに関連付けられている呼び出し元はありません。</span><span class="sxs-lookup"><span data-stu-id="f2277-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="f2277-121">そのためへのアクセスはありません、 `ConnectionId`、 `Caller`、および`Others`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f2277-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="f2277-122">関連資料</span><span class="sxs-lookup"><span data-stu-id="f2277-122">Related resources</span></span>

* [<span data-ttu-id="f2277-123">開始するには</span><span class="sxs-lookup"><span data-stu-id="f2277-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="f2277-124">ハブ</span><span class="sxs-lookup"><span data-stu-id="f2277-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f2277-125">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="f2277-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
