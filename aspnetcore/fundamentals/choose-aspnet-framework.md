---
title: ASP.NET と ASP.NET Core の選択
author: rick-anderson
description: ASP.NET と ASP.NET Core のどちらかを選択する方法について説明します。
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297231"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="10fc6-103">ASP.NET と ASP.NET Core の選択</span><span class="sxs-lookup"><span data-stu-id="10fc6-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="10fc6-104">Windows Server を対象とするエンタープライズ Web アプリから、Linux コンテナーを対象とする小さなマイクロサービスまで、どのような Web アプリを作成している場合でも、ASP.NET はあらゆるソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="10fc6-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="10fc6-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10fc6-105">ASP.NET Core</span></span>

<span data-ttu-id="10fc6-106">ASP.NET Core は、Windows、macOS、または Linux で最新のクラウド ベースの Web アプリを構築するための、オープン ソースのクロスプラットフォーム フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="10fc6-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="10fc6-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="10fc6-107">ASP.NET</span></span>

<span data-ttu-id="10fc6-108">ASP.NET は成熟したフレームワークであり、Windows でエンタープライズ グレードのサーバー ベース Web アプリを構築するために必要なすべてのサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="10fc6-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="10fc6-109">フレームワークの選択</span><span class="sxs-lookup"><span data-stu-id="10fc6-109">Framework selection</span></span>

<span data-ttu-id="10fc6-110">次の表で、どのフレームワークが最もニーズに適しているかをご確認ください。</span><span class="sxs-lookup"><span data-stu-id="10fc6-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="10fc6-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10fc6-111">ASP.NET Core</span></span> | <span data-ttu-id="10fc6-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="10fc6-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="10fc6-113">Windows、macOS、Linux が対象</span><span class="sxs-lookup"><span data-stu-id="10fc6-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="10fc6-114">Windows が対象</span><span class="sxs-lookup"><span data-stu-id="10fc6-114">Build for Windows</span></span>|
|<span data-ttu-id="10fc6-115">[Razor ページ](xref:razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="10fc6-115">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="10fc6-116">[MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api)、[SignalR](xref:signalr/introduction) についても参照してください。</span><span class="sxs-lookup"><span data-stu-id="10fc6-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="10fc6-117">[Web フォーム](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、[WebHooks](/aspnet/webhooks/)、または [Web ページ](/aspnet/web-pages)を使います。</span><span class="sxs-lookup"><span data-stu-id="10fc6-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="10fc6-118">コンピューターごとに複数のバージョン</span><span class="sxs-lookup"><span data-stu-id="10fc6-118">Multiple versions per machine</span></span>|<span data-ttu-id="10fc6-119">コンピューターごとに 1 つのバージョン</span><span class="sxs-lookup"><span data-stu-id="10fc6-119">One version per machine</span></span>|
|<span data-ttu-id="10fc6-120">Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)、または [Visual Studio Code](https://code.visualstudio.com/) で C# または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="10fc6-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="10fc6-121">Visual Studio で C#、VB、または F# を使って開発します</span><span class="sxs-lookup"><span data-stu-id="10fc6-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="10fc6-122">ASP.NET より高いパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="10fc6-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="10fc6-123">よいパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="10fc6-123">Good performance</span></span>|
|[<span data-ttu-id="10fc6-124">.NET Framework または .NET Core ランタイムを選択します</span><span class="sxs-lookup"><span data-stu-id="10fc6-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="10fc6-125">.NET Framework ランタイムを使います</span><span class="sxs-lookup"><span data-stu-id="10fc6-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="10fc6-126">ASP.NET Core のシナリオ</span><span class="sxs-lookup"><span data-stu-id="10fc6-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="10fc6-127">[Razor ページ](xref:razor-pages/index)は、ASP.NET Core 2.x の時点で Web UI を作成する場合に推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="10fc6-127">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="10fc6-128">Web サイト</span><span class="sxs-lookup"><span data-stu-id="10fc6-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="10fc6-129">API</span><span class="sxs-lookup"><span data-stu-id="10fc6-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="10fc6-130">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="10fc6-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="10fc6-131">ASP.NET のシナリオ</span><span class="sxs-lookup"><span data-stu-id="10fc6-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="10fc6-132">Web サイト</span><span class="sxs-lookup"><span data-stu-id="10fc6-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="10fc6-133">API</span><span class="sxs-lookup"><span data-stu-id="10fc6-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="10fc6-134">リアルタイム</span><span class="sxs-lookup"><span data-stu-id="10fc6-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="10fc6-135">リソース</span><span class="sxs-lookup"><span data-stu-id="10fc6-135">Resources</span></span>

* [<span data-ttu-id="10fc6-136">ASP.NET の概要</span><span class="sxs-lookup"><span data-stu-id="10fc6-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="10fc6-137">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="10fc6-137">Introduction to ASP.NET Core</span></span>](xref:index)
