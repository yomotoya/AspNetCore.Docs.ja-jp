---
title: Visual Studio Code を使用する ASP.NET Core の Razor ページの概要
author: rick-anderson
description: Visual Studio Code を使用して ASP.NET Core の Razor ページ Web アプリを構築する方法の基礎について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 17ab52b80a40f6204e2bf2cf9048071c55c0a708
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252218"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="1688b-103">Visual Studio Code を使用する ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="1688b-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="1688b-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1688b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1688b-105">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="1688b-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="1688b-106">このチュートリアルを開始する前に、[Razor ページの概要](xref:mvc/razor-pages/index)に関するページを参照することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1688b-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="1688b-107">ASP.NET Core で Web アプリケーション用の UI を構築する際に Razor ページを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1688b-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1688b-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1688b-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="1688b-109">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="1688b-109">Create a Razor web app</span></span>

<span data-ttu-id="1688b-110">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1688b-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="1688b-112">上のコマンドでは [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトを作成して実行します。</span><span class="sxs-lookup"><span data-stu-id="1688b-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="1688b-113">ブラウザーを開いて http://localhost:5000 にアクセスし、アプリケーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="1688b-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![ホームまたはインデックス ページ](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="1688b-115">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="1688b-115">Open the project</span></span>

<span data-ttu-id="1688b-116">Ctrl + C キーを押して、アプリケーションをシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="1688b-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="1688b-117">Visual Studio Code (VS Code) から、**[ファイル]、[フォルダーを開く]** の順に選択し、*RazorPagesMovie* フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="1688b-117">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="1688b-118">"ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="1688b-118">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="1688b-119">選択します。</span><span class="sxs-lookup"><span data-stu-id="1688b-119">Add them?"</span></span>
- <span data-ttu-id="1688b-120">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1688b-120">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="1688b-121">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="1688b-121">Launch the app</span></span>

<span data-ttu-id="1688b-122">Ctrl + F5 キーを押して、デバッグを行わずにアプリを開始します。</span><span class="sxs-lookup"><span data-stu-id="1688b-122">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="1688b-123">または、**[デバッグ]** メニューの **[デバッグなしで開始]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1688b-123">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="1688b-124">次のチュートリアルでは、プロジェクトにモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="1688b-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="1688b-125">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="1688b-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
