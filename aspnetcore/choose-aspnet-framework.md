---
title: ASP.NET と ASP.NET Core の選択
author: rick-anderson
description: ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: e6ac9f54ef623895b81eea33d90791e5f0ad5120
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="e048b-103">ASP.NET と ASP.NET Core の選択</span><span class="sxs-lookup"><span data-stu-id="e048b-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="e048b-104">Windows Server を対象とするエンタープライズ Web アプリから、Linux コンテナーを対象とする小さなマイクロサービスまで、どのような Web アプリを作成している場合でも、ASP.NET はあらゆるソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="e048b-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="e048b-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e048b-105">ASP.NET Core</span></span>

<span data-ttu-id="e048b-106">ASP.NET Core は、Windows、macOS、または Linux で最新のクラウド ベースの Web アプリを構築するための、オープン ソースのクロスプラットフォーム フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="e048b-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="e048b-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e048b-107">ASP.NET</span></span>

<span data-ttu-id="e048b-108">ASP.NET は成熟したフレームワークであり、Windows でエンタープライズ クラスのサーバー ベース Web アプリを構築するために必要なすべてのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="e048b-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web apps on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="e048b-109">どちらが適していますか?</span><span class="sxs-lookup"><span data-stu-id="e048b-109">Which one is right for me?</span></span>

| <span data-ttu-id="e048b-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e048b-110">ASP.NET Core</span></span> | <span data-ttu-id="e048b-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e048b-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="e048b-112">Windows、macOS、Linux が対象</span><span class="sxs-lookup"><span data-stu-id="e048b-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="e048b-113">Windows が対象</span><span class="sxs-lookup"><span data-stu-id="e048b-113">Build for Windows</span></span>|
|<span data-ttu-id="e048b-114">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="e048b-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="e048b-115">[MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api)、[SignalR](xref:signalr/introduction) についても参照してください。</span><span class="sxs-lookup"><span data-stu-id="e048b-115">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="e048b-116">[Web フォーム](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、または [Web ページ](/aspnet/web-pages)を使います</span><span class="sxs-lookup"><span data-stu-id="e048b-116">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="e048b-117">コンピューターごとに複数のバージョン</span><span class="sxs-lookup"><span data-stu-id="e048b-117">Multiple versions per machine</span></span>|<span data-ttu-id="e048b-118">コンピューターごとに 1 つのバージョン</span><span class="sxs-lookup"><span data-stu-id="e048b-118">One version per machine</span></span>|
|<span data-ttu-id="e048b-119">Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="e048b-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="e048b-120">Visual Studio で C#、VB、または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="e048b-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="e048b-121">ASP.NET より高いパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="e048b-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="e048b-122">よいパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="e048b-122">Good performance</span></span>|
|[<span data-ttu-id="e048b-123">.NET Framework または .NET Core ランタイムを選択します</span><span class="sxs-lookup"><span data-stu-id="e048b-123">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="e048b-124">.NET Framework ランタイムを使います</span><span class="sxs-lookup"><span data-stu-id="e048b-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="e048b-125">ASP.NET Core のシナリオ</span><span class="sxs-lookup"><span data-stu-id="e048b-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="e048b-126">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="e048b-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="e048b-127">Web サイト</span><span class="sxs-lookup"><span data-stu-id="e048b-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="e048b-128">API</span><span class="sxs-lookup"><span data-stu-id="e048b-128">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="e048b-129">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="e048b-129">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="e048b-130">ASP.NET のシナリオ</span><span class="sxs-lookup"><span data-stu-id="e048b-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="e048b-131">Web サイト</span><span class="sxs-lookup"><span data-stu-id="e048b-131">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="e048b-132">API</span><span class="sxs-lookup"><span data-stu-id="e048b-132">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="e048b-133">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="e048b-133">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="e048b-134">リソース</span><span class="sxs-lookup"><span data-stu-id="e048b-134">Resources</span></span>

* [<span data-ttu-id="e048b-135">ASP.NET の概要</span><span class="sxs-lookup"><span data-stu-id="e048b-135">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="e048b-136">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="e048b-136">Introduction to ASP.NET Core</span></span>](xref:index)
