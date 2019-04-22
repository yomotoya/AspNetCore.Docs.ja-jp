---
title: ASP.NET Core SignalR でサポートされているプラットフォーム
author: bradygaster
description: ASP.NET Core SignalR サポートされているプラットフォームについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/06/2019
uid: signalr/supported-platforms
ms.openlocfilehash: fefaaf97de3f1fabf8f3154bf5b4ccb37195ccff
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068223"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="f81f8-103">ASP.NET Core SignalR でサポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="f81f8-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="f81f8-104">サーバーのシステム要件</span><span class="sxs-lookup"><span data-stu-id="f81f8-104">Server system requirements</span></span>

<span data-ttu-id="f81f8-105">ASP.NET core SignalR は ASP.NET Core をサポートする任意のサーバー プラットフォームをサポートします。</span><span class="sxs-lookup"><span data-stu-id="f81f8-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="f81f8-106">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="f81f8-106">JavaScript client</span></span>

<span data-ttu-id="f81f8-107">[JavaScript クライアント](https://www.npmjs.com/package/@aspnet/signalr)NodeJS 8 以降のバージョンと、次のブラウザーで実行されます。</span><span class="sxs-lookup"><span data-stu-id="f81f8-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="f81f8-108">ブラウザー</span><span class="sxs-lookup"><span data-stu-id="f81f8-108">Browser</span></span>                         | <span data-ttu-id="f81f8-109">Version</span><span class="sxs-lookup"><span data-stu-id="f81f8-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="f81f8-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="f81f8-110">Microsoft Edge</span></span>                  | <span data-ttu-id="f81f8-111">現在の</span><span class="sxs-lookup"><span data-stu-id="f81f8-111">current</span></span> |
| <span data-ttu-id="f81f8-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="f81f8-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="f81f8-113">現在の</span><span class="sxs-lookup"><span data-stu-id="f81f8-113">current</span></span> |
| <span data-ttu-id="f81f8-114">Google Chrome;Android が含まれています</span><span class="sxs-lookup"><span data-stu-id="f81f8-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="f81f8-115">現在の</span><span class="sxs-lookup"><span data-stu-id="f81f8-115">current</span></span> |
| <span data-ttu-id="f81f8-116">Safari;iOS が含まれています</span><span class="sxs-lookup"><span data-stu-id="f81f8-116">Safari; includes iOS</span></span>            | <span data-ttu-id="f81f8-117">現在の</span><span class="sxs-lookup"><span data-stu-id="f81f8-117">current</span></span> |
| <span data-ttu-id="f81f8-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="f81f8-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="f81f8-119">11</span><span class="sxs-lookup"><span data-stu-id="f81f8-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="f81f8-120">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="f81f8-120">.NET client</span></span>

<span data-ttu-id="f81f8-121">[.NET クライアント](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)は ASP.NET Core でサポートされている任意のプラットフォームで実行されます。</span><span class="sxs-lookup"><span data-stu-id="f81f8-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="f81f8-122">たとえば、 [Xamarin 開発者は、SignalR を使用できます](https://github.com/aspnet/Announcements/issues/305)8.4.0.1 Xamarin.Android を使用して Android アプリを構築するためと後で、Xamarin.iOS 11.14.0.4 を使用して iOS アプリ以降。</span><span class="sxs-lookup"><span data-stu-id="f81f8-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="f81f8-123">サーバーは、IIS を実行する場合、Websocket トランスポートには、IIS 8.0 または Windows Server 2012 以降、またはそれ以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="f81f8-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="f81f8-124">他のトランスポートは、すべてのプラットフォームでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="f81f8-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="f81f8-125">Java クライアント</span><span class="sxs-lookup"><span data-stu-id="f81f8-125">Java client</span></span>

<span data-ttu-id="f81f8-126">[Java クライアント](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)Java 8 と以降のバージョンをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f81f8-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="f81f8-127">サポートされていないクライアント</span><span class="sxs-lookup"><span data-stu-id="f81f8-127">Unsupported clients</span></span>

<span data-ttu-id="f81f8-128">次のクライアントは使用できますが、試験段階または非公式には。</span><span class="sxs-lookup"><span data-stu-id="f81f8-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="f81f8-129">現在サポートされていない、ならない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f81f8-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="f81f8-130">C++ クライアント</span><span class="sxs-lookup"><span data-stu-id="f81f8-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="f81f8-131">Swift クライアント</span><span class="sxs-lookup"><span data-stu-id="f81f8-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
