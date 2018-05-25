---
title: ASP.NET と ASP.NET Core の選択
author: rick-anderson
description: ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 05/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 0c6924d40b7327d2032a0278c56a0b4fa41d15a1
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="3ed0d-103">ASP.NET と ASP.NET Core の選択</span><span class="sxs-lookup"><span data-stu-id="3ed0d-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="3ed0d-104">Windows Server を対象とするエンタープライズ Web アプリから、Linux コンテナーを対象とする小さなマイクロサービスまで、どのような Web アプリを作成している場合でも、ASP.NET はあらゆるソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="3ed0d-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ed0d-105">ASP.NET Core</span></span>

<span data-ttu-id="3ed0d-106">ASP.NET Core は、Windows、macOS、または Linux で最新のクラウド ベースの Web アプリを構築するための、オープン ソースのクロスプラットフォーム フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="3ed0d-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3ed0d-107">ASP.NET</span></span>

<span data-ttu-id="3ed0d-108">ASP.NET は成熟したフレームワークであり、Windows でエンタープライズ グレードのサーバー ベース Web アプリを構築するために必要なすべてのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="3ed0d-109">フレームワークの選択</span><span class="sxs-lookup"><span data-stu-id="3ed0d-109">Framework selection</span></span>

<span data-ttu-id="3ed0d-110">次の表で、どのフレームワークが最もニーズに適しているかをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="3ed0d-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ed0d-111">ASP.NET Core</span></span> | <span data-ttu-id="3ed0d-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3ed0d-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="3ed0d-113">Windows、macOS、Linux が対象</span><span class="sxs-lookup"><span data-stu-id="3ed0d-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="3ed0d-114">Windows が対象</span><span class="sxs-lookup"><span data-stu-id="3ed0d-114">Build for Windows</span></span>|
|<span data-ttu-id="3ed0d-115">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="3ed0d-116">[MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api)、[SignalR](xref:signalr/introduction) についても参照してください。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="3ed0d-117">[Web フォーム](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、[WebHooks](/aspnet/webhooks/)、または [Web ページ](/aspnet/web-pages)を使います。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="3ed0d-118">コンピューターごとに複数のバージョン</span><span class="sxs-lookup"><span data-stu-id="3ed0d-118">Multiple versions per machine</span></span>|<span data-ttu-id="3ed0d-119">コンピューターごとに 1 つのバージョン</span><span class="sxs-lookup"><span data-stu-id="3ed0d-119">One version per machine</span></span>|
|<span data-ttu-id="3ed0d-120">Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="3ed0d-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="3ed0d-121">Visual Studio で C#、VB、または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="3ed0d-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="3ed0d-122">ASP.NET より高いパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="3ed0d-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="3ed0d-123">よいパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="3ed0d-123">Good performance</span></span>|
|[<span data-ttu-id="3ed0d-124">.NET Framework または .NET Core ランタイムを選択します</span><span class="sxs-lookup"><span data-stu-id="3ed0d-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="3ed0d-125">.NET Framework ランタイムを使います</span><span class="sxs-lookup"><span data-stu-id="3ed0d-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="3ed0d-126">ASP.NET Core のシナリオ</span><span class="sxs-lookup"><span data-stu-id="3ed0d-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="3ed0d-127">[Razor ページ](xref:mvc/razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="3ed0d-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="3ed0d-128">Web サイト</span><span class="sxs-lookup"><span data-stu-id="3ed0d-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="3ed0d-129">API</span><span class="sxs-lookup"><span data-stu-id="3ed0d-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="3ed0d-130">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="3ed0d-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="3ed0d-131">ASP.NET のシナリオ</span><span class="sxs-lookup"><span data-stu-id="3ed0d-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="3ed0d-132">Web サイト</span><span class="sxs-lookup"><span data-stu-id="3ed0d-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="3ed0d-133">API</span><span class="sxs-lookup"><span data-stu-id="3ed0d-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="3ed0d-134">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="3ed0d-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="3ed0d-135">リソース</span><span class="sxs-lookup"><span data-stu-id="3ed0d-135">Resources</span></span>

* [<span data-ttu-id="3ed0d-136">ASP.NET の概要</span><span class="sxs-lookup"><span data-stu-id="3ed0d-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="3ed0d-137">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="3ed0d-137">Introduction to ASP.NET Core</span></span>](xref:index)
