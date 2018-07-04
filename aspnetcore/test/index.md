---
title: ASP.NET Core のテスト、デバッグ、およびトラブルシューティング
author: guardrex
description: ASP.NET Core アプリケーションをテストし、デバッグするためのリソースのリンク
ms.author: riande
ms.custom: mvc
ms.date: 07/03/2018
uid: test/index
ms.openlocfilehash: 20f6804c1db588a88abb0d5686f894b7463ff6a9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433962"
---
# <a name="test-debug-and-troubleshoot-in-aspnet-core"></a><span data-ttu-id="24791-103">ASP.NET Core のテスト、デバッグ、およびトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="24791-103">Test, debug, and troubleshoot in ASP.NET Core</span></span>

## <a name="test"></a><span data-ttu-id="24791-104">テスト</span><span class="sxs-lookup"><span data-stu-id="24791-104">Test</span></span>

[<span data-ttu-id="24791-105">.NET Core と .NET Standard の単体テスト</span><span class="sxs-lookup"><span data-stu-id="24791-105">Unit Testing in .NET Core and .NET Standard</span></span>](/dotnet/articles/core/testing/)  
<span data-ttu-id="24791-106">.NET Core と .NET Standard プロジェクトでの単体テストの使用方法をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="24791-106">See how to use unit testing in .NET Core and .NET Standard projects.</span></span>

[<span data-ttu-id="24791-107">統合テスト</span><span class="sxs-lookup"><span data-stu-id="24791-107">Integration tests</span></span>](xref:test/integration-tests)  
<span data-ttu-id="24791-108">統合テストによってデータベース、ファイル システム、ネットワークなどのインフラストラクチャ レベルで、アプリのコンポーネントがどのように正しく機能するようになるかを説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-108">Learn how integration tests ensure that an app's components function correctly at the infrastructure level, including the database, file system, and network.</span></span>

[<span data-ttu-id="24791-109">Razor ページの単体テスト</span><span class="sxs-lookup"><span data-stu-id="24791-109">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)  
<span data-ttu-id="24791-110">Razor ページのアプリに単体テストを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-110">Discover how to create unit tests for Razor Pages apps.</span></span>

[<span data-ttu-id="24791-111">テスト コントローラー</span><span class="sxs-lookup"><span data-stu-id="24791-111">Test controllers</span></span>](xref:mvc/controllers/testing)  
<span data-ttu-id="24791-112">Moq と xUnit を使って ASP.NET Core のコントローラーのロジックをテストする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-112">Learn how to test controller logic in ASP.NET Core with Moq and xUnit.</span></span>

## <a name="debug"></a><span data-ttu-id="24791-113">デバッグ</span><span class="sxs-lookup"><span data-stu-id="24791-113">Debug</span></span>

[<span data-ttu-id="24791-114">Visual Studio を使用したデバッグについて理解する</span><span class="sxs-lookup"><span data-stu-id="24791-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="24791-115">ステップ バイ ステップのチュートリアルで Visual Studio デバッガーの機能を説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-115">Discover the features of the Visual Studio debugger in a step-by-step walkthrough.</span></span>

[<span data-ttu-id="24791-116">Visual Studio Code でのデバッグ</span><span class="sxs-lookup"><span data-stu-id="24791-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="24791-117">Visual Studio Code に組み込まれているデバッグのサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-117">Find out about the debugging support built into Visual Studio Code.</span></span>

[<span data-ttu-id="24791-118">ASP.NET Core 2.x ソースのデバッグ</span><span class="sxs-lookup"><span data-stu-id="24791-118">Debug ASP.NET Core 2.x source</span></span>](https://github.com/aspnet/Docs/issues/4155)  
<span data-ttu-id="24791-119">.NET Core および ASP.NET Core ソースをデバッグする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-119">Learn how to debug .NET Core and ASP.NET Core sources.</span></span>

[<span data-ttu-id="24791-120">リモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="24791-120">Remote debugging</span></span>](/visualstudio/debugger/remote-debugging-azure)  
<span data-ttu-id="24791-121">Visual Studio 2017 ASP.NET Core アプリをセットアップして構成し、Azure を使用してアプリを IIS に配置して、Visual Studio からリモート デバッガーをアタッチする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-121">Explore how to set up and configure a Visual Studio 2017 ASP.NET Core app, deploy it to IIS using Azure, and attach the remote debugger from Visual Studio.</span></span>

[<span data-ttu-id="24791-122">スナップショットのデバッグ</span><span class="sxs-lookup"><span data-stu-id="24791-122">Snapshot debugging</span></span>](/azure/application-insights/app-insights-snapshot-debugger)  
<span data-ttu-id="24791-123">運用環境での問題を診断するために必要な情報が得られるように、特にスローされる例外のスナップショットを収集する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="24791-123">Find out how to collect snapshots on your top-throwing exceptions so that you have the information you need to diagnose issues in production.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="24791-124">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="24791-124">Troubleshoot</span></span>

[<span data-ttu-id="24791-125">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="24791-125">Troubleshoot</span></span>](xref:test/troubleshoot)  
<span data-ttu-id="24791-126">ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。</span><span class="sxs-lookup"><span data-stu-id="24791-126">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>
