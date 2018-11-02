---
title: ASP.NET Core SignalR でサポートされているプラットフォーム
author: tdykstra
description: ASP.NET Core SignalR サポートされているプラットフォームについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758181"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="494dc-103">ASP.NET Core SignalR でサポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="494dc-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="494dc-104">サーバーのシステム要件</span><span class="sxs-lookup"><span data-stu-id="494dc-104">Server system requirements</span></span>

<span data-ttu-id="494dc-105">ASP.NET core SignalR は ASP.NET Core をサポートする任意のサーバー プラットフォームをサポートします。</span><span class="sxs-lookup"><span data-stu-id="494dc-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="494dc-106">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="494dc-106">JavaScript client</span></span>

<span data-ttu-id="494dc-107">[JavaScript クライアント](https://www.npmjs.com/package/@aspnet/signalr)NodeJS 8 以降のバージョンと、次のブラウザーで実行されます。</span><span class="sxs-lookup"><span data-stu-id="494dc-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="494dc-108">ブラウザー</span><span class="sxs-lookup"><span data-stu-id="494dc-108">Browser</span></span>                         | <span data-ttu-id="494dc-109">Version</span><span class="sxs-lookup"><span data-stu-id="494dc-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="494dc-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="494dc-110">Microsoft Edge</span></span>                  | <span data-ttu-id="494dc-111">現在の</span><span class="sxs-lookup"><span data-stu-id="494dc-111">current</span></span> |
| <span data-ttu-id="494dc-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="494dc-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="494dc-113">現在の</span><span class="sxs-lookup"><span data-stu-id="494dc-113">current</span></span> |
| <span data-ttu-id="494dc-114">Google Chrome;Android が含まれています</span><span class="sxs-lookup"><span data-stu-id="494dc-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="494dc-115">現在の</span><span class="sxs-lookup"><span data-stu-id="494dc-115">current</span></span> |
| <span data-ttu-id="494dc-116">Safari;iOS が含まれています</span><span class="sxs-lookup"><span data-stu-id="494dc-116">Safari; includes iOS</span></span>            | <span data-ttu-id="494dc-117">現在の</span><span class="sxs-lookup"><span data-stu-id="494dc-117">current</span></span> |
| <span data-ttu-id="494dc-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="494dc-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="494dc-119">11</span><span class="sxs-lookup"><span data-stu-id="494dc-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="494dc-120">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="494dc-120">.NET client</span></span>

<span data-ttu-id="494dc-121">[.NET クライアント](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)は ASP.NET Core でサポートされている任意のサーバー プラットフォームで実行されます。</span><span class="sxs-lookup"><span data-stu-id="494dc-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="494dc-122">サーバーは、IIS を実行する場合、Websocket トランスポートには、IIS 8.0 または Windows Server 2012 で高い以上が必要です。</span><span class="sxs-lookup"><span data-stu-id="494dc-122">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="494dc-123">他のトランスポートは、すべてのプラットフォームでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="494dc-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="494dc-124">Java クライアント</span><span class="sxs-lookup"><span data-stu-id="494dc-124">Java client</span></span>

<span data-ttu-id="494dc-125">[Java クライアント](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)Java 8 と以降のバージョンをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="494dc-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="494dc-126">サポートされていないクライアント</span><span class="sxs-lookup"><span data-stu-id="494dc-126">Unsupported clients</span></span>

<span data-ttu-id="494dc-127">次のクライアントは使用できますが、試験段階または非公式には。</span><span class="sxs-lookup"><span data-stu-id="494dc-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="494dc-128">現在サポートされていない、ならない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="494dc-128">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="494dc-129">C++ クライアント</span><span class="sxs-lookup"><span data-stu-id="494dc-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="494dc-130">Swift クライアント</span><span class="sxs-lookup"><span data-stu-id="494dc-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
