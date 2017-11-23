---
title: "ASP.NET Core の Razor ページの概要"
author: rick-anderson
description: "ASP.NET Core の Razor ページの概要"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 5c58b5156f62572687755c9c0878db10c3c14eb1
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="ed72b-104">ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="ed72b-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ed72b-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed72b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed72b-106">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="ed72b-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ed72b-107">このチュートリアルを開始する前に、[Razor ページの概要](xref:mvc/razor-pages/index)に関するページを参照することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ed72b-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="ed72b-108">ASP.NET Core で Web アプリケーション用の UI を構築する際に Razor ページを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ed72b-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

<span data-ttu-id="ed72b-109">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="ed72b-109">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="ed72b-110">Windows 向け: 本チュートリアル</span><span class="sxs-lookup"><span data-stu-id="ed72b-110">Windows: This tutorial</span></span>
* <span data-ttu-id="ed72b-111">MacOS 向け: [Mac 向けの Visual Studio での Razor ページの概要](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ed72b-111">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="ed72b-112">macOS、Linux、Windows 向け: [Visual Studio Code を使用する ASP.NET Core の Razor ページの概要](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ed72b-112">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed72b-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="ed72b-113">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ed72b-114">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="ed72b-114">Create a Razor web app</span></span>

* <span data-ttu-id="ed72b-115">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="ed72b-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ed72b-116">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed72b-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ed72b-117">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ed72b-118">コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="ed72b-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="ed72b-119">![新しい ASP.NET Core Web アプリケーション](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="ed72b-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="ed72b-120">ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ed72b-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
  <span data-ttu-id="ed72b-121">![Web アプリケーション (Razor ページ)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="ed72b-121">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="ed72b-122">以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed72b-122">The Visual Studio template creates a starter project:</span></span>

![ソリューション エクスプローラー](razor-pages-start/_static/se.png)

<span data-ttu-id="ed72b-124">**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。</span><span class="sxs-lookup"><span data-stu-id="ed72b-124">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/home.png)

* <span data-ttu-id="ed72b-126">Visual Studio で [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-126">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ed72b-127">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ed72b-128">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="ed72b-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ed72b-129">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-129">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ed72b-130">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ed72b-131">上の図では、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="ed72b-131">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="ed72b-132">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-132">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ed72b-133">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-133">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ed72b-134">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="ed72b-134">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="ed72b-135">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="ed72b-135">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
