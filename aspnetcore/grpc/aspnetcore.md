---
title: ASP.NET Core を使用した gRPC サービス
author: juntaoluo
description: ASP.NET Core で gRPC のサービスを記述する場合は、基本的な概念を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 387c3134efc04bec740fc66a5ca4b84715264d35
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515477"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="6da17-103">ASP.NET Core を使用した gRPC サービス</span><span class="sxs-lookup"><span data-stu-id="6da17-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="6da17-104">このドキュメントでは、gRPC を ASP.NET Core を使用してサービスを開始する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6da17-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="6da17-105">ASP.NET Core の gRPC サービスの概要</span><span class="sxs-lookup"><span data-stu-id="6da17-105">Get started with gRPC service in ASP.NET Core</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6da17-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6da17-106">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6da17-107">参照してください[gRPC サービスの使用開始](xref:tutorials/grpc/grpc-start)gRPC プロジェクトを作成する方法の詳細な手順についてはします。</span><span class="sxs-lookup"><span data-stu-id="6da17-107">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6da17-108">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6da17-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6da17-109">コマンド ラインから `dotnet new grpc -o GrpcGreeter` を実行します。</span><span class="sxs-lookup"><span data-stu-id="6da17-109">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="6da17-110">GRPC サービス、ASP.NET Core アプリを追加します。</span><span class="sxs-lookup"><span data-stu-id="6da17-110">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="6da17-111">gRPC では、次のパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="6da17-111">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="6da17-112">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="6da17-112">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="6da17-113">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) protobuf メッセージ Api。</span><span class="sxs-lookup"><span data-stu-id="6da17-113">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="6da17-114">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="6da17-114">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="6da17-115">GRPC を構成します。</span><span class="sxs-lookup"><span data-stu-id="6da17-115">Configure gRPC</span></span>

<span data-ttu-id="6da17-116">gRPC、有効化され、`AddGrpc`メソッド。</span><span class="sxs-lookup"><span data-stu-id="6da17-116">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="6da17-117">各 gRPC サービスを通じてルーティング パイプラインに追加されます、`MapGrpcService`メソッド。</span><span class="sxs-lookup"><span data-stu-id="6da17-117">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Startup.cs?name=snippet&highlight=16-19)]

<span data-ttu-id="6da17-118">ASP.NET Core のミドルウェアと機能を共有して、ルーティングのパイプライン、ハンドラーの追加の要求を処理するためにそのため、アプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="6da17-118">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="6da17-119">MVC コント ローラーなど、追加の要求ハンドラーは、gRPC が構成されているサービスと並列で動作します。</span><span class="sxs-lookup"><span data-stu-id="6da17-119">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="6da17-120">ASP.NET Core Api との統合</span><span class="sxs-lookup"><span data-stu-id="6da17-120">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="6da17-121">gRPC サービスが ASP.NET Core の機能へのフル アクセスをなどある[依存関係の注入](xref:fundamentals/dependency-injection)(DI) と[ログ](xref:fundamentals/logging/index)します。</span><span class="sxs-lookup"><span data-stu-id="6da17-121">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="6da17-122">たとえば、サービスの実装では、コンス トラクターによって DI コンテナーからロガー サービスを解決できます。</span><span class="sxs-lookup"><span data-stu-id="6da17-122">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="6da17-123">既定では、gRPC サービスの実装は、任意の有効期間 (シングルトン、スコープ ベース、または一時的) では、他の DI サービスを解決できます。</span><span class="sxs-lookup"><span data-stu-id="6da17-123">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="6da17-124">GRPC メソッドで HttpContext を解決します。</span><span class="sxs-lookup"><span data-stu-id="6da17-124">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="6da17-125">GRPC API は、メソッド、ホスト、ヘッダー、およびトレーラーなど、一部の http/2 メッセージ データへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="6da17-125">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="6da17-126">アクセスはを介して、 `ServerCallContext` gRPC の各メソッドに渡される引数。</span><span class="sxs-lookup"><span data-stu-id="6da17-126">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="6da17-127">`ServerCallContext` フル アクセスを提供しない`HttpContext`すべての ASP.NET Api でします。</span><span class="sxs-lookup"><span data-stu-id="6da17-127">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="6da17-128">`GetHttpContext`拡張メソッドへのフル アクセスを提供する、 `HttpContext` ASP.NET Api では、基になる HTTP/2 メッセージを表します。</span><span class="sxs-lookup"><span data-stu-id="6da17-128">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet1)]

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="6da17-129">要求本文データのレート制限</span><span class="sxs-lookup"><span data-stu-id="6da17-129">Request body data rate limit</span></span>

<span data-ttu-id="6da17-130">Kestrel サーバーは、既定で、[最小限の要求本文のレート](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>)します。</span><span class="sxs-lookup"><span data-stu-id="6da17-130">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="6da17-131">ストリーミング クライアントと双方向のストリーミング呼び出しは、このレートが満たされていないと、接続がタイムアウトする可能性があります。最小要求の本文はストリーミング クライアントと双方向のストリーミングを呼び出す、gRPC サービスが含まれる場合、データ速度の制限を無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6da17-131">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a><span data-ttu-id="6da17-132">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6da17-132">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
