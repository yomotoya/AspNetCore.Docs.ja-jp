---
title: gRPC の ASP.NET Core の構成
author: jamesnk
description: GRPC の ASP.NET Core アプリを構成する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983134"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="b1186-103">gRPC の ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="b1186-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="b1186-104">サービスのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="b1186-104">Configure services options</span></span>

<span data-ttu-id="b1186-105">次の表では、gRPC サービスの構成オプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b1186-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="b1186-106">オプション</span><span class="sxs-lookup"><span data-stu-id="b1186-106">Option</span></span> | <span data-ttu-id="b1186-107">既定値</span><span class="sxs-lookup"><span data-stu-id="b1186-107">Default Value</span></span> | <span data-ttu-id="b1186-108">説明</span><span class="sxs-lookup"><span data-stu-id="b1186-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="b1186-109">サーバーから送信できるバイト単位で最大メッセージ サイズ。</span><span class="sxs-lookup"><span data-stu-id="b1186-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="b1186-110">例外で構成されている最大メッセージ サイズの結果を超えるメッセージを送信しようとしています。</span><span class="sxs-lookup"><span data-stu-id="b1186-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="b1186-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="b1186-111">4 MB</span></span> | <span data-ttu-id="b1186-112">サーバーで受信できるをバイト単位で最大メッセージ サイズ。</span><span class="sxs-lookup"><span data-stu-id="b1186-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="b1186-113">サーバーは、この制限を超えるメッセージを受信した場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="b1186-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="b1186-114">この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="b1186-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b1186-115">場合`true`、詳細なサービス メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="b1186-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="b1186-116">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="b1186-116">The default is `false`.</span></span> <span data-ttu-id="b1186-117">これを設定`true`機密情報が漏れることができます。</span><span class="sxs-lookup"><span data-stu-id="b1186-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="b1186-118">gzip</span><span class="sxs-lookup"><span data-stu-id="b1186-118">gzip</span></span> | <span data-ttu-id="b1186-119">圧縮し、メッセージを圧縮解除に使用される圧縮プロバイダーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="b1186-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="b1186-120">圧縮のカスタム プロバイダーを作成し、コレクションに追加できます。</span><span class="sxs-lookup"><span data-stu-id="b1186-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="b1186-121">既定値には、プロバイダーのサポートが構成されている**gzip**圧縮します。</span><span class="sxs-lookup"><span data-stu-id="b1186-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="b1186-122">圧縮アルゴリズムは、サーバーから送信されたメッセージを圧縮するために使用します。</span><span class="sxs-lookup"><span data-stu-id="b1186-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="b1186-123">アルゴリズムで圧縮プロバイダーが一致する必要があります`CompressionProviders`します。</span><span class="sxs-lookup"><span data-stu-id="b1186-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="b1186-124">クライアントの応答を圧縮する algorthm 送信することによって、アルゴリズムをサポートを示す必要があります、 **grpc の同意のエンコード**ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="b1186-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="b1186-125">サーバーから送信されたメッセージを圧縮するために使用する圧縮レベルです。</span><span class="sxs-lookup"><span data-stu-id="b1186-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="b1186-126">オプションのデリゲートを提供することですべてのサービス オプションを構成でき、`AddGrpc`呼び出す`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="b1186-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="b1186-127">1 つのサービスのオプションで提供されるグローバル オプションを上書き`AddGrpc`を使用して構成できると`AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="b1186-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a><span data-ttu-id="b1186-128">Kestrel オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="b1186-128">Configure Kestrel options</span></span>

<span data-ttu-id="b1186-129">Kestrel サーバーでは、ASP.NET の gRPC の動作に影響する構成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="b1186-129">Kestrel server has configuration options that affect the behavior of gRPC for ASP.NET.</span></span>

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="b1186-130">要求本文データのレート制限</span><span class="sxs-lookup"><span data-stu-id="b1186-130">Request body data rate limit</span></span>

<span data-ttu-id="b1186-131">Kestrel サーバーは、既定で、[最小限の要求本文のレート](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>)します。</span><span class="sxs-lookup"><span data-stu-id="b1186-131">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="b1186-132">ストリーミング クライアントと双方向のストリーミング呼び出しは、このレートが満たされていないと、接続がタイムアウトする可能性があります。最小要求の本文はストリーミング クライアントと双方向のストリーミングを呼び出す、gRPC サービスが含まれる場合、データ速度の制限を無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b1186-132">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="b1186-133">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b1186-133">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
