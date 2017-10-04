---
title: "ASP.NET と ASP.NET Core の選択"
author: rick-anderson
description: "ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。"
keywords: "ASP.NET Core,基礎,概要"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.assetid: f0d17abf-3c69-413e-87fc-30780805e33f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 875064bd3437acc4e2a53220e19e86431d8c159b
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="d132a-104">ASP.NET と ASP.NET Core の選択</span><span class="sxs-lookup"><span data-stu-id="d132a-104">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="d132a-105">Windows Server を対象とするエンタープライズ Web アプリケーションから、Linux コンテナーを対象とする小さなマイクロサービスまで、どのような Web アプリケーションを作成している場合でも、ASP.NET はあらゆるソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="d132a-105">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="d132a-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d132a-106">ASP.NET Core</span></span>

<span data-ttu-id="d132a-107">ASP.NET Core は、Windows、macOS、または Linux で最新のクラウド ベースの Web アプリケーションを構築するための、オープン ソースのクロスプラットフォーム フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="d132a-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="d132a-108">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d132a-108">ASP.NET</span></span>

<span data-ttu-id="d132a-109">ASP.NET は成熟したフレームワークであり、Windows でエンタープライズ クラスのサーバー ベース Web アプリケーションを構築するために必要なすべてのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="d132a-109">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="d132a-110">どちらが適していますか?</span><span class="sxs-lookup"><span data-stu-id="d132a-110">Which one is right for me?</span></span>

| <span data-ttu-id="d132a-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d132a-111">ASP.NET Core</span></span> | <span data-ttu-id="d132a-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d132a-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="d132a-113">Windows、macOS、Linux が対象</span><span class="sxs-lookup"><span data-stu-id="d132a-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="d132a-114">Windows が対象</span><span class="sxs-lookup"><span data-stu-id="d132a-114">Build for Windows</span></span>|
|<span data-ttu-id="d132a-115">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.0 で Web UI を作成するための推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="d132a-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="d132a-116">[MVC](xref:mvc/overview) および [Web API](xref:tutorials/first-web-api) もご覧ください</span><span class="sxs-lookup"><span data-stu-id="d132a-116">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="d132a-117">[Web フォーム](https://docs.microsoft.com/aspnet/web-forms)、[SignalR](https://docs.microsoft.com/aspnet/signalr)、[MVC](https://docs.microsoft.com/aspnet/mvc)、[Web API](https://docs.microsoft.com/aspnet/web-api/)、または [Web ページ](https://docs.microsoft.com/aspnet/web-pages)を使います</span><span class="sxs-lookup"><span data-stu-id="d132a-117">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="d132a-118">コンピューターごとに複数のバージョン</span><span class="sxs-lookup"><span data-stu-id="d132a-118">Multiple versions per machine</span></span>|<span data-ttu-id="d132a-119">コンピューターごとに 1 つのバージョン</span><span class="sxs-lookup"><span data-stu-id="d132a-119">One version per machine</span></span>|
|<span data-ttu-id="d132a-120">Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="d132a-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="d132a-121">Visual Studio で C#、VB、または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="d132a-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="d132a-122">ASP.NET より高いパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="d132a-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="d132a-123">よいパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="d132a-123">Good performance</span></span>|
|[<span data-ttu-id="d132a-124">.NET Framework または .NET Core ランタイムを選択します</span><span class="sxs-lookup"><span data-stu-id="d132a-124">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="d132a-125">.NET Framework ランタイムを使います</span><span class="sxs-lookup"><span data-stu-id="d132a-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="d132a-126">ASP.NET Core のシナリオ</span><span class="sxs-lookup"><span data-stu-id="d132a-126">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="d132a-127">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.0 で Web UI を作成するための推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="d132a-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="d132a-128">Web サイト</span><span class="sxs-lookup"><span data-stu-id="d132a-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="d132a-129">API</span><span class="sxs-lookup"><span data-stu-id="d132a-129">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="d132a-130">ASP.NET のシナリオ</span><span class="sxs-lookup"><span data-stu-id="d132a-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="d132a-131">Web サイト</span><span class="sxs-lookup"><span data-stu-id="d132a-131">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="d132a-132">API</span><span class="sxs-lookup"><span data-stu-id="d132a-132">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="d132a-133">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="d132a-133">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="d132a-134">リソース</span><span class="sxs-lookup"><span data-stu-id="d132a-134">Resources</span></span>

* [<span data-ttu-id="d132a-135">ASP.NET の概要</span><span class="sxs-lookup"><span data-stu-id="d132a-135">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="d132a-136">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="d132a-136">Introduction to ASP.NET Core</span></span>](xref:index)
