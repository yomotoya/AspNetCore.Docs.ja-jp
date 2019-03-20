---
title: ASP.NET Core の gRPC の概要
author: juntaoluo
description: Kestrel サーバーと ASP.NET Core の gRPC サービスについて説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="63076-103">ASP.NET Core の gRPC の概要</span><span class="sxs-lookup"><span data-stu-id="63076-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="63076-104">作成者: [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="63076-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="63076-105">[gRPC](https://grpc.io/docs/guides/) は言語に依存しない高性能なリモート プロシージャ コール (RPC) フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="63076-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="63076-106">gRPC の基礎については、[gRPC ドキュメント ページ](https://grpc.io/docs/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="63076-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="63076-107">gRPC の主な利点:</span><span class="sxs-lookup"><span data-stu-id="63076-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="63076-108">最新の高性能軽量 RPC フレームワーク。</span><span class="sxs-lookup"><span data-stu-id="63076-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="63076-109">既定でプロトコル バッファーを使用する契約優先の API 開発。言語に依存しない実装を可能にします。</span><span class="sxs-lookup"><span data-stu-id="63076-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="63076-110">厳密に型指定されたサーバーとクライアントを生成する目的で、さまざまな言語で利用できるツール。</span><span class="sxs-lookup"><span data-stu-id="63076-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="63076-111">クライアント、サーバー、双方向ストリーミング呼び出しをサポートします。</span><span class="sxs-lookup"><span data-stu-id="63076-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="63076-112">Protobuf バイナリ シリアル化でネットワークの使用率を減らします。</span><span class="sxs-lookup"><span data-stu-id="63076-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="63076-113">以上の利点から gRPC は以下に最適です。</span><span class="sxs-lookup"><span data-stu-id="63076-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="63076-114">効率性が重要となる軽量のマイクロサービス。</span><span class="sxs-lookup"><span data-stu-id="63076-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="63076-115">開発に複数の言語が必要になる多言語システム。</span><span class="sxs-lookup"><span data-stu-id="63076-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="63076-116">ストリーミングの要求または応答を処理する必要があるポイントツーポイントのリアルタイム サービス。</span><span class="sxs-lookup"><span data-stu-id="63076-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="63076-117">公式の [gRPC ページ](https://grpc.io/docs/quickstart/csharp.html)では現在、C# 実装を利用できますが、現在の実装は C で記述されたネイティブ ライブラリに依存しています (gRPC [C-core](https://grpc.io/blog/grpc-stacks))。</span><span class="sxs-lookup"><span data-stu-id="63076-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="63076-118">Kestrel HTTP サーバーは完全管理の ASP.NET Core スタックを基盤とする新しい実装を提供するための作業が現在進行中です。</span><span class="sxs-lookup"><span data-stu-id="63076-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="63076-119">次のドキュメントは、この新しい実装で gRPC サービスを構築するための概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="63076-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63076-120">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="63076-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>