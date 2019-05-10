---
title: C core から ASP.NET Core への移行の gRPC サービス
author: juntaoluo
description: ASP.NET Core スタック上で実行する既存の C core gRPC ベース アプリを移動する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895239"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="fb469-103">C core から ASP.NET Core への移行の gRPC サービス</span><span class="sxs-lookup"><span data-stu-id="fb469-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="fb469-104">作成者: [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="fb469-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="fb469-105">基になるスタックの実装により、すべての機能がの間で同じ方法で機能[C コア ベース gRPC](https://grpc.io/blog/grpc-stacks)アプリおよび ASP.NET Core ベースのアプリ。</span><span class="sxs-lookup"><span data-stu-id="fb469-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="fb469-106">このドキュメントには、2 つのスタック間を移行するための主な違いが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="fb469-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="fb469-107">gRPC サービス実装の有効期間</span><span class="sxs-lookup"><span data-stu-id="fb469-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="fb469-108">ASP.NET Core スタック、gRPC、既定ではで作成されたサービス、[有効期間がスコープ](xref:fundamentals/dependency-injection#service-lifetimes)します。</span><span class="sxs-lookup"><span data-stu-id="fb469-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="fb469-109">サービスに既定では、gRPC C コアがこれに対し、バインド、[シングルトン有効期間](xref:fundamentals/dependency-injection#service-lifetimes)します。</span><span class="sxs-lookup"><span data-stu-id="fb469-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="fb469-110">有効期間がスコープには、有効期間のスコープを持つその他のサービスを解決するサービス実装ができるようにします。</span><span class="sxs-lookup"><span data-stu-id="fb469-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="fb469-111">たとえば、有効期間がスコープを解決できるも`DBContext`コンス トラクターの挿入を通じて DI コンテナーから。</span><span class="sxs-lookup"><span data-stu-id="fb469-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="fb469-112">スコープを持つ有効期間を使用します。</span><span class="sxs-lookup"><span data-stu-id="fb469-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="fb469-113">要求ごとに、サービス実装の新しいインスタンスが構築されます。</span><span class="sxs-lookup"><span data-stu-id="fb469-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="fb469-114">実装の種類のインスタンス メンバーを使用して要求間で状態を共有することはできません。</span><span class="sxs-lookup"><span data-stu-id="fb469-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="fb469-115">DI コンテナー内のシングルトン サービスで共有状態を格納することを見込んでです。</span><span class="sxs-lookup"><span data-stu-id="fb469-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="fb469-116">ストアドの共有状態は、gRPC のサービス実装のコンス トラクターで解決されます。</span><span class="sxs-lookup"><span data-stu-id="fb469-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="fb469-117">サービスの有効期間の詳細については、次を参照してください。<xref:fundamentals/dependency-injection#service-lifetimes>します。</span><span class="sxs-lookup"><span data-stu-id="fb469-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="fb469-118">シングルトン サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="fb469-118">Add a singleton service</span></span>

<span data-ttu-id="fb469-119">GRPC C core の実装から ASP.NET Core への移行を容易にするには、サービスの実装からのサービスの有効期間を変更することスコープ シングルトンです。</span><span class="sxs-lookup"><span data-stu-id="fb469-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="fb469-120">これは、DI コンテナーへのサービス実装のインスタンスの追加が含まれます。</span><span class="sxs-lookup"><span data-stu-id="fb469-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="fb469-121">ただし、シングルトン有効期間を持つサービスの実装では、コンス トラクターの挿入を通じてスコープを持つサービスを解決することはありません。</span><span class="sxs-lookup"><span data-stu-id="fb469-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="fb469-122">GRPC サービス オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="fb469-122">Configure gRPC services options</span></span>

<span data-ttu-id="fb469-123">C コア ベースのアプリなどの設定で`grpc.max_receive_message_length`と`grpc.max_send_message_length`で構成された`ChannelOption`とき[サーバー インスタンスを構築する](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)します。</span><span class="sxs-lookup"><span data-stu-id="fb469-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="fb469-124">ASP.NET Core で gRPC を使用した構成を提供します、`GrpcServiceOptions`型。</span><span class="sxs-lookup"><span data-stu-id="fb469-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="fb469-125">たとえば、gRPC サービスの受信メッセージの最大サイズを使用して構成できる`AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="fb469-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="fb469-126">構成の詳細については、次を参照してください。<xref:grpc/configuration>します。</span><span class="sxs-lookup"><span data-stu-id="fb469-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="fb469-127">ログの記録</span><span class="sxs-lookup"><span data-stu-id="fb469-127">Logging</span></span>

<span data-ttu-id="fb469-128">C コア ベースのアプリの依存、`GrpcEnvironment`に[ロガーを構成する](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)デバッグのためです。</span><span class="sxs-lookup"><span data-stu-id="fb469-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="fb469-129">ASP.NET Core スタックを通じてこの機能を提供する、[ログ API](xref:fundamentals/logging/index)します。</span><span class="sxs-lookup"><span data-stu-id="fb469-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="fb469-130">たとえば、コンス トラクターの挿入を使用して、gRPC サービスにロガーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="fb469-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="fb469-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fb469-131">HTTPS</span></span>

<span data-ttu-id="fb469-132">C コア ベースのアプリからの HTTPS を構成する、 [Server.Ports プロパティ](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)します。</span><span class="sxs-lookup"><span data-stu-id="fb469-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="fb469-133">同様の概念は、ASP.NET Core でのサーバーの構成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="fb469-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="fb469-134">たとえば、Kestrel を使用して[エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)この機能のためです。</span><span class="sxs-lookup"><span data-stu-id="fb469-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="fb469-135">インターセプターとミドルウェア</span><span class="sxs-lookup"><span data-stu-id="fb469-135">Interceptors and Middleware</span></span>

<span data-ttu-id="fb469-136">ASP.NET Core[ミドルウェア](xref:fundamentals/middleware/index)gRPC の C コア ベースのアプリケーションでインターセプターと比較すると同様の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="fb469-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="fb469-137">ミドルウェアとインターセプターは概念的には、同じ両方は gRPC 要求を処理するパイプラインの構築に使用されるためです。</span><span class="sxs-lookup"><span data-stu-id="fb469-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="fb469-138">どちらは、パイプライン内の次のコンポーネントの前後に実行される処理を許可します。</span><span class="sxs-lookup"><span data-stu-id="fb469-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="fb469-139">インターセプターの抽象化を使用して、gRPC レイヤーを操作中にただし、ASP.NET Core のミドルウェアが、基になる http/2 メッセージに対して動作する、 [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)します。</span><span class="sxs-lookup"><span data-stu-id="fb469-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb469-140">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="fb469-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
