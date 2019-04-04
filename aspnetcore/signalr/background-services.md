---
title: ホストの ASP.NET Core SignalR のバック グラウンド サービス
author: bradygaster
description: .NET Core BackgroundService クラスから SignalR クライアントにメッセージを送信する方法について説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739671"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="55d59-103">ホストの ASP.NET Core SignalR のバック グラウンド サービス</span><span class="sxs-lookup"><span data-stu-id="55d59-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="55d59-104">によって[Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="55d59-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="55d59-105">この記事では、ガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="55d59-105">This article provides guidance for:</span></span>

* <span data-ttu-id="55d59-106">ASP.NET Core でホストされているバック グラウンド ワーカー プロセスを使用して、SignalR ハブをホストします。</span><span class="sxs-lookup"><span data-stu-id="55d59-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="55d59-107">.NET Core 内からクライアントにメッセージを送信接続されている[BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)します。</span><span class="sxs-lookup"><span data-stu-id="55d59-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="55d59-108">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="55d59-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="55d59-109">スタートアップ中に SignalR を接続します。</span><span class="sxs-lookup"><span data-stu-id="55d59-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="55d59-110">バック グラウンド ワーカー プロセスのコンテキストで ASP.NET Core SignalR ハブをホストしているは、ASP.NET Core web アプリでのハブをホストするいると同じです。</span><span class="sxs-lookup"><span data-stu-id="55d59-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="55d59-111">`Startup.ConfigureServices`メソッドを呼び出す`services.AddSignalR`SignalR をサポートするために、ASP.NET Core の依存関係挿入 (DI) 層に必要なサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="55d59-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="55d59-112">`Startup.Configure`、`UseSignalR`メソッドが呼び出されたに ASP.NET Core の要求パイプラインで Hub のエンドポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="55d59-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="55d59-113">上記の例では、`ClockHub`クラスが実装する、`Hub<T>`厳密に型指定されたハブを作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="55d59-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="55d59-114">`ClockHub`で構成されている、`Startup`クラスは、エンドポイントで要求に応答する`/hubs/clock`します。</span><span class="sxs-lookup"><span data-stu-id="55d59-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="55d59-115">厳密に型指定されたハブの詳細については、[for ASP.NET Core SignalR のハブを使用して、](xref:signalr/hubs#strongly-typed-hubs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="55d59-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="55d59-116">この機能に限定されません、[ハブ\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1)クラス。</span><span class="sxs-lookup"><span data-stu-id="55d59-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="55d59-117">任意のクラスから継承する[ハブ](xref:Microsoft.AspNetCore.SignalR.Hub)など[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)も機能します。</span><span class="sxs-lookup"><span data-stu-id="55d59-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="55d59-118">厳密に型指定によって使用されるインターフェイス`ClockHub`は、`IClock`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="55d59-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="55d59-119">バック グラウンド サービスから SignalR ハブを呼び出す</span><span class="sxs-lookup"><span data-stu-id="55d59-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="55d59-120">起動中に、`Worker`クラス、`BackgroundService`を使用してワイヤード (有線) は`AddHostedService`。</span><span class="sxs-lookup"><span data-stu-id="55d59-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="55d59-121">SignalR もワイヤード (有線) の中にあるため、`Startup`フェーズ、によって表される各ハブの各ハブに接続されている ASP.NET Core の HTTP 要求パイプライン内の個々 のエンドポイントで、`IHubContext<T>`サーバー。</span><span class="sxs-lookup"><span data-stu-id="55d59-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="55d59-122">ASP.NET Core の使用する DI 機能、他のクラスのようなホストのレイヤーによってインスタンス化`BackgroundService`クラス、MVC コント ローラー クラスまたは Razor ページのモデルのインスタンスをそのまま使用してサーバー側のハブへの参照を取得できます`IHubContext<ClockHub, IClock>`構築時にします。</span><span class="sxs-lookup"><span data-stu-id="55d59-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="55d59-123">として、`ExecuteAsync`メソッドがバック グラウンド サービスに繰り返し呼び出されると、サーバーの現在の日付と時刻はクライアントに送信、接続を使用して、`ClockHub`します。</span><span class="sxs-lookup"><span data-stu-id="55d59-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="55d59-124">バック グラウンド サービス使用の SignalR イベントに対応します。</span><span class="sxs-lookup"><span data-stu-id="55d59-124">React to SignalR events with background services</span></span>

<span data-ttu-id="55d59-125">SignalR または .NET デスクトップ アプリが実行できるは、JavaScript クライアントを使用してシングル ページ アプリなどを使用して、使用して、 <xref:signalr/dotnet-client>、`BackgroundService`または`IHostedService`実装を使用して SignalR ハブに接続し、イベントに応答することもできます。</span><span class="sxs-lookup"><span data-stu-id="55d59-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="55d59-126">`ClockHubClient`両方を実装するクラス、`IClock`インターフェイスと`IHostedService`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="55d59-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="55d59-127">この方法の中に有線できます`Startup`を継続的に実行し、サーバーからハブのイベントに応答します。</span><span class="sxs-lookup"><span data-stu-id="55d59-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="55d59-128">初期化中に、`ClockHubClient`のインスタンスを作成、`HubConnection`そのに接続して、`IClock.ShowTime`のハブのハンドラーとしてメソッド`ShowTime`イベント。</span><span class="sxs-lookup"><span data-stu-id="55d59-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="55d59-129">`IHostedService.StartAsync` 、実装、`HubConnection`が非同期的に開始します。</span><span class="sxs-lookup"><span data-stu-id="55d59-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="55d59-130">中に、`IHostedService.StopAsync`メソッド、`HubConnection`の非同期的に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="55d59-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="55d59-131">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="55d59-131">Additional resources</span></span>

* [<span data-ttu-id="55d59-132">開始するには</span><span class="sxs-lookup"><span data-stu-id="55d59-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="55d59-133">ハブ</span><span class="sxs-lookup"><span data-stu-id="55d59-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="55d59-134">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="55d59-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="55d59-135">厳密に型指定されたハブ</span><span class="sxs-lookup"><span data-stu-id="55d59-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
