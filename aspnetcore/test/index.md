---
title: ASP.NET Core のテスト、デバッグ、およびトラブルシューティング
author: guardrex
description: ASP.NET Core アプリケーションをテストし、デバッグするためのリソースのリンク
ms.author: riande
ms.custom: mvc
ms.date: 06/13/2018
uid: test/index
ms.openlocfilehash: c5925d55a1b7d50d44d6bea4013331416ce3cec8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278802"
---
# <a name="test-debug-and-troubleshoot-in-aspnet-core"></a><span data-ttu-id="f2d9a-103">ASP.NET Core のテスト、デバッグ、およびトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="f2d9a-103">Test, debug, and troubleshoot in ASP.NET Core</span></span>

## <a name="test"></a><span data-ttu-id="f2d9a-104">テスト</span><span class="sxs-lookup"><span data-stu-id="f2d9a-104">Test</span></span>

[<span data-ttu-id="f2d9a-105">.NET Core と .NET Standard の単体テスト</span><span class="sxs-lookup"><span data-stu-id="f2d9a-105">Unit Testing in .NET Core and .NET Standard</span></span>](/dotnet/articles/core/testing/)  
<span data-ttu-id="f2d9a-106">.NET Core と .NET Standard プロジェクトでの単体テストの使用方法をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-106">See how to use unit testing in .NET Core and .NET Standard projects.</span></span>

[<span data-ttu-id="f2d9a-107">統合テスト</span><span class="sxs-lookup"><span data-stu-id="f2d9a-107">Integration tests</span></span>](xref:test/integration-tests)  
<span data-ttu-id="f2d9a-108">統合テストによってデータベース、ファイル システム、ネットワークなどのインフラストラクチャ レベルで、アプリのコンポーネントがどのように正しく機能するようになるかを説明します。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-108">Learn how integration tests ensure that an app's components function correctly at the infrastructure level, including the database, file system, and network.</span></span>

[<span data-ttu-id="f2d9a-109">Razor ページの単体テスト</span><span class="sxs-lookup"><span data-stu-id="f2d9a-109">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)  
<span data-ttu-id="f2d9a-110">Razor ページのアプリに単体テストを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-110">Discover how to create unit tests for Razor Pages apps.</span></span>

[<span data-ttu-id="f2d9a-111">テスト コントローラー</span><span class="sxs-lookup"><span data-stu-id="f2d9a-111">Test controllers</span></span>](xref:mvc/controllers/testing)  
<span data-ttu-id="f2d9a-112">Moq と xUnit を使って ASP.NET Core のコントローラーのロジックをテストする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-112">Learn how to test controller logic in ASP.NET Core with Moq and xUnit.</span></span>

## <a name="debug"></a><span data-ttu-id="f2d9a-113">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f2d9a-113">Debug</span></span>

[<span data-ttu-id="f2d9a-114">ASP.NET Core 2.x ソースのデバッグ</span><span class="sxs-lookup"><span data-stu-id="f2d9a-114">Debug ASP.NET Core 2.x source</span></span>](https://github.com/aspnet/Docs/issues/4155)  
<span data-ttu-id="f2d9a-115">.NET Core および ASP.NET Core ソースをデバッグする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-115">Learn how to debug .NET Core and ASP.NET Core sources.</span></span>

[<span data-ttu-id="f2d9a-116">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="f2d9a-116">Remote debugging</span></span>](/visualstudio/debugger/remote-debugging-azure)  
<span data-ttu-id="f2d9a-117">Visual Studio 2017 ASP.NET Core アプリをセットアップして構成し、これを Azure を使用して IIS に配置し、Visual Studio からリモート デバッガーをアタッチする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-117">Discover how to set up and configure a Visual Studio 2017 ASP.NET Core app, deploy it to IIS using Azure, and attach the remote debugger from Visual Studio.</span></span>

[<span data-ttu-id="f2d9a-118">スナップショットのデバッグ</span><span class="sxs-lookup"><span data-stu-id="f2d9a-118">Snapshot debugging</span></span>](/azure/application-insights/app-insights-snapshot-debugger)  
<span data-ttu-id="f2d9a-119">運用環境での問題を診断するために必要な情報が得られるように、特にスローされる例外のスナップショットを収集する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-119">Learn how to collect snapshots on your top-throwing exceptions so that you have the information you need to diagnose issues in production.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="f2d9a-120">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="f2d9a-120">Troubleshoot</span></span>

[<span data-ttu-id="f2d9a-121">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="f2d9a-121">Troubleshoot</span></span>](xref:test/troubleshoot)  
<span data-ttu-id="f2d9a-122">ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。</span><span class="sxs-lookup"><span data-stu-id="f2d9a-122">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>
