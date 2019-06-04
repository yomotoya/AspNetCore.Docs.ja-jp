---
title: gRPC の ASP.NET Core の構成
author: jamesnk
description: GRPC の ASP.NET Core アプリを構成する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 5/30/2019
uid: grpc/configuration
ms.openlocfilehash: 1f8250dc9aa8b82da384ee28287011baa19dc11f
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491233"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="76bde-103">gRPC の ASP.NET Core の構成</span><span class="sxs-lookup"><span data-stu-id="76bde-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="76bde-104">サービスのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="76bde-104">Configure services options</span></span>

<span data-ttu-id="76bde-105">次の表では、gRPC サービスの構成オプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="76bde-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="76bde-106">オプション</span><span class="sxs-lookup"><span data-stu-id="76bde-106">Option</span></span> | <span data-ttu-id="76bde-107">既定値</span><span class="sxs-lookup"><span data-stu-id="76bde-107">Default Value</span></span> | <span data-ttu-id="76bde-108">説明</span><span class="sxs-lookup"><span data-stu-id="76bde-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="76bde-109">サーバーから送信できるバイト単位で最大メッセージ サイズ。</span><span class="sxs-lookup"><span data-stu-id="76bde-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="76bde-110">例外で構成されている最大メッセージ サイズの結果を超えるメッセージを送信しようとしています。</span><span class="sxs-lookup"><span data-stu-id="76bde-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="76bde-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="76bde-111">4 MB</span></span> | <span data-ttu-id="76bde-112">サーバーで受信できるをバイト単位で最大メッセージ サイズ。</span><span class="sxs-lookup"><span data-stu-id="76bde-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="76bde-113">サーバーは、この制限を超えるメッセージを受信した場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="76bde-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="76bde-114">この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="76bde-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="76bde-115">場合`true`、詳細なサービス メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。</span><span class="sxs-lookup"><span data-stu-id="76bde-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="76bde-116">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="76bde-116">The default is `false`.</span></span> <span data-ttu-id="76bde-117">設定`EnableDetailedErrors`に`true`機密情報が漏れることができます。</span><span class="sxs-lookup"><span data-stu-id="76bde-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="76bde-118">gzip</span><span class="sxs-lookup"><span data-stu-id="76bde-118">gzip</span></span> | <span data-ttu-id="76bde-119">圧縮し、メッセージを圧縮解除に使用される圧縮プロバイダーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="76bde-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="76bde-120">圧縮のカスタム プロバイダーを作成し、コレクションに追加できます。</span><span class="sxs-lookup"><span data-stu-id="76bde-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="76bde-121">既定値には、プロバイダーのサポートが構成されている**gzip**圧縮します。</span><span class="sxs-lookup"><span data-stu-id="76bde-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="76bde-122">圧縮アルゴリズムは、サーバーから送信されたメッセージを圧縮するために使用します。</span><span class="sxs-lookup"><span data-stu-id="76bde-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="76bde-123">アルゴリズムで圧縮プロバイダーが一致する必要があります`CompressionProviders`します。</span><span class="sxs-lookup"><span data-stu-id="76bde-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="76bde-124">送信することによって、アルゴリズムをサポートの応答を圧縮する、アルゴリズムでは、クライアントが示す必要があります、 **grpc の同意のエンコード**ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="76bde-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="76bde-125">サーバーから送信されたメッセージを圧縮するために使用する圧縮レベルです。</span><span class="sxs-lookup"><span data-stu-id="76bde-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="76bde-126">オプションのデリゲートを提供することですべてのサービス オプションを構成でき、`AddGrpc`呼び出す`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="76bde-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="76bde-127">1 つのサービスのオプションで提供されるグローバル オプションを上書き`AddGrpc`を使用して構成できると`AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="76bde-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="76bde-128">クライアント オプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="76bde-128">Configure client options</span></span>

<span data-ttu-id="76bde-129">次のコードでは、クライアントの最大送信を設定し、受信メッセージ サイズ。</span><span class="sxs-lookup"><span data-stu-id="76bde-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="76bde-130">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="76bde-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
