---
title: Visual Studio for Mac を使用し、macOS で ASP.NET Core Razor ページを構築する方法の概要
author: rick-anderson
description: Visual Studio for Mac を使用して ASP.NET Core Razor ページを構築する方法の基本を説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 29eb11d0195f483b144394e505dd63fb6016161b
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252335"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="39709-103">Visual Studio for Mac を使用し、macOS で ASP.NET Core Razor ページを構築する方法の概要</span><span class="sxs-lookup"><span data-stu-id="39709-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="39709-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="39709-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="39709-105">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="39709-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="39709-106">このチュートリアルを開始する前に、[Razor ページの概要](xref:mvc/razor-pages/index)に関するページを参照することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="39709-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="39709-107">ASP.NET Core で Web アプリケーション用の UI を構築する際に Razor ページを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="39709-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39709-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="39709-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="39709-109">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="39709-109">Create a Razor web app</span></span>

<span data-ttu-id="39709-110">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="39709-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="39709-112">上のコマンドでは [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトを作成して実行します。</span><span class="sxs-lookup"><span data-stu-id="39709-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="39709-113">ブラウザーを開いて http://localhost:5000 にアクセスし、アプリケーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="39709-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![ホームまたはインデックス ページ](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="39709-115">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="39709-115">Open the project</span></span>

<span data-ttu-id="39709-116">Ctrl + C キーを押して、アプリケーションをシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="39709-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="39709-117">Visual Studio から、**[ファイル]、[開く]** の順に選択し、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="39709-117">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="39709-118">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="39709-118">Launch the app</span></span>

<span data-ttu-id="39709-119">Visual Studio で、**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="39709-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="39709-120">Visual Studio は [Kestrel](xref:fundamentals/servers/kestrel) を開始し、ブラウザーを起動して、`http://localhost:5000` に移動します。</span><span class="sxs-lookup"><span data-stu-id="39709-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="39709-121">次のチュートリアルでは、プロジェクトにモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="39709-121">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="39709-122">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="39709-122">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
