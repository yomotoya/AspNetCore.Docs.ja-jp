---
title: パフォーマンス診断ツール
author: mjrousos
description: ASP.NET Core アプリのパフォーマンスの問題を診断するための便利なツールです。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329201"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="f6858-103">パフォーマンスの診断ツール</span><span class="sxs-lookup"><span data-stu-id="f6858-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="f6858-104">によって[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="f6858-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="f6858-105">この記事では、ASP.NET Core でのパフォーマンスの問題を診断するためのツールを紹介します。</span><span class="sxs-lookup"><span data-stu-id="f6858-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="f6858-106">Visual Studio 診断ツール</span><span class="sxs-lookup"><span data-stu-id="f6858-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="f6858-107">[プロファイリングと診断ツール](/visualstudio/profiling)パフォーマンスの問題の調査を開始点としては、Visual Studio に組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="f6858-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="f6858-108">これらのツールとは、強力で、Visual Studio 開発環境から使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="f6858-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="f6858-109">ツールは、ASP.NET Core アプリでの CPU 使用率、メモリ使用量とパフォーマンス イベントの分析を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f6858-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="f6858-110">詳細については[Visual Studio ドキュメント](/visualstudio/profiling/profiling-overview)します。</span><span class="sxs-lookup"><span data-stu-id="f6858-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="f6858-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="f6858-111">Application Insights</span></span>

<span data-ttu-id="f6858-112">[Application Insights](/azure/application-insights/app-insights-overview)アプリの詳細なパフォーマンス データを提供します。</span><span class="sxs-lookup"><span data-stu-id="f6858-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="f6858-113">Application Insights は、自動的に反応率、エラー率、依存関係の応答時間に関するデータを収集します。</span><span class="sxs-lookup"><span data-stu-id="f6858-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="f6858-114">Application Insights では、アプリにカスタム イベントとメトリックが特定のログ記録をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f6858-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="f6858-115">Application Insights は、さまざまな環境で使用できます。</span><span class="sxs-lookup"><span data-stu-id="f6858-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="f6858-116">Azure で動作するように最適化します。</span><span class="sxs-lookup"><span data-stu-id="f6858-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="f6858-117">運用、開発、およびステージング環境で機能します。</span><span class="sxs-lookup"><span data-stu-id="f6858-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="f6858-118">ローカルで動作[Visual Studio](/azure/application-insights/app-insights-visual-studio)または他のホスト環境にします。</span><span class="sxs-lookup"><span data-stu-id="f6858-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="f6858-119">詳細については、「[Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6858-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="f6858-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="f6858-120">PerfView</span></span>

<span data-ttu-id="f6858-121">[PerfView](https://github.com/Microsoft/perfview)は .NET のパフォーマンスの問題を診断するためには、具体的には、.NET チームによって作成されたパフォーマンス分析ツールです。</span><span class="sxs-lookup"><span data-stu-id="f6858-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="f6858-122">PerfView は、CPU 使用率、メモリおよび GC の動作、パフォーマンス イベント、およびウォール クロック時間の分析を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f6858-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="f6858-123">PerfView および開始する方法の詳細については、 [PerfView のチュートリアル ビデオ](http://channel9.msdn.com/Series/PerfView-Tutorial)またはツールで使用可能なユーザー ガイドを参照してまたは[github](https://github.com/Microsoft/perfview)します。</span><span class="sxs-lookup"><span data-stu-id="f6858-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="f6858-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="f6858-124">PerfCollect</span></span>

<span data-ttu-id="f6858-125">PerfView が .NET シナリオ用の便利なパフォーマンス分析ツールの中でしか実行できません Windows のため、これを使用して、Linux 環境で実行されている ASP.NET Core アプリからトレースを収集することはできません。</span><span class="sxs-lookup"><span data-stu-id="f6858-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="f6858-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)ネイティブ Linux のプロファイリング ツールを使用する bash スクリプトは、([Perf](https://perf.wiki.kernel.org/index.php/Main_Page)と[LTTng](https://lttng.org/)) PerfView で分析できる Linux 上のトレースを収集します。</span><span class="sxs-lookup"><span data-stu-id="f6858-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="f6858-127">PerfCollect は、PerfView を直接使用することはできません、Linux 環境でパフォーマンスの問題を表示する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="f6858-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="f6858-128">代わりに、PerfCollect すると、PerfView を使用して Windows コンピューター上で、分析し、.NET Core アプリからトレースを収集できます。</span><span class="sxs-lookup"><span data-stu-id="f6858-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="f6858-129">インストールして PerfCollect の使用方法の詳細については、使用可能な[github](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)します。</span><span class="sxs-lookup"><span data-stu-id="f6858-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
