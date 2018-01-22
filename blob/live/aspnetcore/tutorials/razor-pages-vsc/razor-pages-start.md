---
title: "Visual Studio Code を使用する ASP.NET Core の Razor ページの概要"
author: rick-anderson
description: "Visual Studio Code を使用する ASP.NET Core の Razor ページの概要"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 727c3d5c8bed0aef3094816b7e5f0a940aea654c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="c7535-103">Visual Studio Code を使用する ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="c7535-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="c7535-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7535-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c7535-105">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="c7535-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="c7535-106">このチュートリアルを開始する前に、[Razor ページの概要](xref:mvc/razor-pages/index)に関するページを参照することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c7535-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="c7535-107">ASP.NET Core で Web アプリケーション用の UI を構築する際に Razor ページを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c7535-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7535-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="c7535-108">Prerequisites</span></span>

<span data-ttu-id="c7535-109">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="c7535-109">Install the following:</span></span>

* <span data-ttu-id="c7535-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降</span><span class="sxs-lookup"><span data-stu-id="c7535-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="c7535-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c7535-111">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="c7535-112">VS Code [C# 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="c7535-112">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c7535-113">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="c7535-113">Create a Razor web app</span></span>

<span data-ttu-id="c7535-114">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c7535-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="c7535-115">上のコマンドでは [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトを作成して実行します。</span><span class="sxs-lookup"><span data-stu-id="c7535-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="c7535-116">ブラウザーを開いて http://localhost:5000 にアクセスし、アプリケーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="c7535-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![ホームまたはインデックス ページ](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="c7535-118">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="c7535-118">Open the project</span></span>

<span data-ttu-id="c7535-119">Ctrl + C キーを押して、アプリケーションをシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="c7535-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="c7535-120">Visual Studio Code (VS Code) から、**[ファイル]、[フォルダーを開く]** の順に選択し、*RazorPagesMovie* フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="c7535-120">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="c7535-121">"ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="c7535-121">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="c7535-122">選択します。</span><span class="sxs-lookup"><span data-stu-id="c7535-122">Add them?"</span></span>
- <span data-ttu-id="c7535-123">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c7535-123">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="c7535-124">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="c7535-124">Launch the app</span></span>

<span data-ttu-id="c7535-125">Ctrl + F5 キーを押して、デバッグを行わずにアプリを開始します。</span><span class="sxs-lookup"><span data-stu-id="c7535-125">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="c7535-126">または、**[デバッグ]** メニューの **[デバッグなしで開始]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c7535-126">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="c7535-127">次のチュートリアルでは、プロジェクトにモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="c7535-127">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="c7535-128">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="c7535-128">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
