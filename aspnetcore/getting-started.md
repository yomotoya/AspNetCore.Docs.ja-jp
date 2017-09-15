---
title: "ASP.NET Core 2.0 の概要"
author: rick-anderson
description: "ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。"
keywords: "ASP.NET Core,チュートリアル,概要"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="a6409-104">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="a6409-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="a6409-105">ここで説明する手順は最新バージョンの ASP.NET Core 用です。</span><span class="sxs-lookup"><span data-stu-id="a6409-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="a6409-106">以前のバージョンの概要については、</span><span class="sxs-lookup"><span data-stu-id="a6409-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="a6409-107">[このチュートリアルの 1.1 バージョン](xref:getting-started-1.1)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a6409-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="a6409-108">[.NET Core](https://microsoft.com/net/core/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a6409-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="a6409-109">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="a6409-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="a6409-110">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="a6409-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="a6409-111">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="a6409-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. <span data-ttu-id="a6409-112">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="a6409-112">Run the app.</span></span>

   <span data-ttu-id="a6409-113">必要に応じて、まず `dotnet run` コマンドでアプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a6409-113">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="a6409-114">`http://localhost:5000` を参照します</span><span class="sxs-lookup"><span data-stu-id="a6409-114">Browse to `http://localhost:5000`</span></span>

### <a name="next-steps"></a><span data-ttu-id="a6409-115">次のステップ</span><span class="sxs-lookup"><span data-stu-id="a6409-115">Next steps</span></span>

<span data-ttu-id="a6409-116">入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a6409-116">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="a6409-117">ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a6409-117">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="a6409-118">ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a6409-118">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="a6409-119">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a6409-119">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
