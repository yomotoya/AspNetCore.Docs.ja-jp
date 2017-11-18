---
title: "Mac 用 ASP.NET Core の Razor ページの概要"
author: rick-anderson
description: "Visual Studio for Mac を使用する ASP.NET Core の Razor ページの概要"
keywords: "ASP.NET Core,Razor ページ,スキャフォールディング,Entity Framework Core,EF,EF Core,データベース,mac,macOS,Visual Studio for Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 9431e8160011d1485925db5cc4f9f83bf7381e97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="bdb1a-104">Visual Studio for Mac を使用する ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="bdb1a-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="bdb1a-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bdb1a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bdb1a-106">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="bdb1a-107">このチュートリアルを開始する前に、[Razor ページの概要](xref:mvc/razor-pages/index)に関するページを参照することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="bdb1a-108">ASP.NET Core で Web アプリケーション用の UI を構築する際に Razor ページを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdb1a-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="bdb1a-109">Prerequisites</span></span>

<span data-ttu-id="bdb1a-110">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-110">Install the following:</span></span>

* <span data-ttu-id="bdb1a-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降</span><span class="sxs-lookup"><span data-stu-id="bdb1a-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="bdb1a-112">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bdb1a-112">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="bdb1a-113">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="bdb1a-113">Create a Razor web app</span></span>

<span data-ttu-id="bdb1a-114">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="bdb1a-115">上のコマンドでは [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトを作成して実行します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="bdb1a-116">ブラウザーを開いて http://localhost:5000 にアクセスし、アプリケーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![ホームまたはインデックス ページ](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="bdb1a-118">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="bdb1a-118">Open the project</span></span>

<span data-ttu-id="bdb1a-119">Ctrl + C キーを押して、アプリケーションをシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="bdb1a-120">Visual Studio から、**[ファイル]、[開く]** の順に選択し、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-120">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="bdb1a-121">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="bdb1a-121">Launch the app</span></span>

<span data-ttu-id="bdb1a-122">Visual Studio で、**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-122">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="bdb1a-123">Visual Studio は [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) を開始し、ブラウザーを起動して、`http://localhost:5000` に移動します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-123">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="bdb1a-124">次のチュートリアルでは、プロジェクトにモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="bdb1a-124">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="bdb1a-125">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="bdb1a-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
