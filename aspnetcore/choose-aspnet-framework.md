---
title: "ASP.NET と ASP.NET Core の選択"
author: rick-anderson
description: "ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 232e82ed66ff2363230ff09d435db1074c02b53b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="4f6a2-103">ASP.NET と ASP.NET Core の選択</span><span class="sxs-lookup"><span data-stu-id="4f6a2-103">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="4f6a2-104">Windows Server を対象とするエンタープライズ Web アプリケーションから、Linux コンテナーを対象とする小さなマイクロサービスまで、どのような Web アプリケーションを作成している場合でも、ASP.NET はあらゆるソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="4f6a2-104">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="4f6a2-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f6a2-105">ASP.NET Core</span></span>

<span data-ttu-id="4f6a2-106">ASP.NET Core は、Windows、macOS、または Linux で最新のクラウド ベースの Web アプリケーションを構築するための、オープン ソースのクロスプラットフォーム フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="4f6a2-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="4f6a2-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f6a2-107">ASP.NET</span></span>

<span data-ttu-id="4f6a2-108">ASP.NET は成熟したフレームワークであり、Windows でエンタープライズ クラスのサーバー ベース Web アプリケーションを構築するために必要なすべてのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="4f6a2-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="4f6a2-109">どちらが適していますか?</span><span class="sxs-lookup"><span data-stu-id="4f6a2-109">Which one is right for me?</span></span>

| <span data-ttu-id="4f6a2-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f6a2-110">ASP.NET Core</span></span> | <span data-ttu-id="4f6a2-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f6a2-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="4f6a2-112">Windows、macOS、Linux が対象</span><span class="sxs-lookup"><span data-stu-id="4f6a2-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="4f6a2-113">Windows が対象</span><span class="sxs-lookup"><span data-stu-id="4f6a2-113">Build for Windows</span></span>|
|<span data-ttu-id="4f6a2-114">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.0 で Web UI を作成するための推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="4f6a2-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="4f6a2-115">[MVC](xref:mvc/overview) および [Web API](xref:tutorials/first-web-api) もご覧ください</span><span class="sxs-lookup"><span data-stu-id="4f6a2-115">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="4f6a2-116">[Web フォーム](https://docs.microsoft.com/aspnet/web-forms)、[SignalR](https://docs.microsoft.com/aspnet/signalr)、[MVC](https://docs.microsoft.com/aspnet/mvc)、[Web API](https://docs.microsoft.com/aspnet/web-api/)、または [Web ページ](https://docs.microsoft.com/aspnet/web-pages)を使います</span><span class="sxs-lookup"><span data-stu-id="4f6a2-116">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="4f6a2-117">コンピューターごとに複数のバージョン</span><span class="sxs-lookup"><span data-stu-id="4f6a2-117">Multiple versions per machine</span></span>|<span data-ttu-id="4f6a2-118">コンピューターごとに 1 つのバージョン</span><span class="sxs-lookup"><span data-stu-id="4f6a2-118">One version per machine</span></span>|
|<span data-ttu-id="4f6a2-119">Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="4f6a2-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="4f6a2-120">Visual Studio で C#、VB、または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="4f6a2-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="4f6a2-121">ASP.NET より高いパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="4f6a2-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="4f6a2-122">よいパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="4f6a2-122">Good performance</span></span>|
|[<span data-ttu-id="4f6a2-123">.NET Framework または .NET Core ランタイムを選択します</span><span class="sxs-lookup"><span data-stu-id="4f6a2-123">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="4f6a2-124">.NET Framework ランタイムを使います</span><span class="sxs-lookup"><span data-stu-id="4f6a2-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="4f6a2-125">ASP.NET Core のシナリオ</span><span class="sxs-lookup"><span data-stu-id="4f6a2-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="4f6a2-126">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.0 で Web UI を作成するための推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="4f6a2-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="4f6a2-127">Web サイト</span><span class="sxs-lookup"><span data-stu-id="4f6a2-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="4f6a2-128">API</span><span class="sxs-lookup"><span data-stu-id="4f6a2-128">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="4f6a2-129">ASP.NET のシナリオ</span><span class="sxs-lookup"><span data-stu-id="4f6a2-129">ASP.NET scenarios</span></span>

* [<span data-ttu-id="4f6a2-130">Web サイト</span><span class="sxs-lookup"><span data-stu-id="4f6a2-130">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="4f6a2-131">API</span><span class="sxs-lookup"><span data-stu-id="4f6a2-131">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="4f6a2-132">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="4f6a2-132">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="4f6a2-133">リソース</span><span class="sxs-lookup"><span data-stu-id="4f6a2-133">Resources</span></span>

* [<span data-ttu-id="4f6a2-134">ASP.NET の概要</span><span class="sxs-lookup"><span data-stu-id="4f6a2-134">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="4f6a2-135">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="4f6a2-135">Introduction to ASP.NET Core</span></span>](xref:index)
