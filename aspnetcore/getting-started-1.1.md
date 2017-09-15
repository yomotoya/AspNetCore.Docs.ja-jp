---
title: "ASP.NET Core 1.1 の概要"
author: rick-anderson
description: "ASP.NET Core 1.1 を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。"
keywords: "ASP.NET Core,チュートリアル,概要"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="f9d06-104">ASP.NET Core 1.1 の概要</span><span class="sxs-lookup"><span data-stu-id="f9d06-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="f9d06-105">ここで説明する手順は ASP.NET Core 1.1 用です。</span><span class="sxs-lookup"><span data-stu-id="f9d06-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="f9d06-106">最新バージョンについては、</span><span class="sxs-lookup"><span data-stu-id="f9d06-106">Looking for the latest version?</span></span> <span data-ttu-id="f9d06-107">[このチュートリアルの最新バージョン](xref:getting-started)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f9d06-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="f9d06-108">[.NET Core 1.0.5 および 1.1.2 SDK 1.0.4 ダウンロード ページ](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f9d06-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="f9d06-109">新しい .NET Core プロジェクト用のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9d06-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="f9d06-110">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="f9d06-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f9d06-111">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="f9d06-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="f9d06-112">コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9d06-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="f9d06-113">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9d06-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="f9d06-114">パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="f9d06-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="f9d06-115">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f9d06-115">Run the app.</span></span>

   <span data-ttu-id="f9d06-116">必要に応じて、まず `dotnet run` コマンドでアプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="f9d06-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="f9d06-117">`http://localhost:5000` を参照します</span><span class="sxs-lookup"><span data-stu-id="f9d06-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="f9d06-118">次のステップ</span><span class="sxs-lookup"><span data-stu-id="f9d06-118">Next steps</span></span>

<span data-ttu-id="f9d06-119">入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f9d06-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="f9d06-120">ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f9d06-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="f9d06-121">ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f9d06-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="f9d06-122">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f9d06-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
