---
title: "ASP.NET Core の Razor ページの概要"
author: rick-anderson
description: "ASP.NET Core の Razor ページの概要"
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 69a5bc439130ffacf2d267c79b1a6b0347171e49
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="a202b-103">ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="a202b-103">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="a202b-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a202b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a202b-105">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="a202b-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="a202b-106">ASP.NET Core で Web アプリ用の UI を構築する際に Razor ページを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a202b-106">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="a202b-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="a202b-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="a202b-108">Windows 向け: 本チュートリアル</span><span class="sxs-lookup"><span data-stu-id="a202b-108">Windows: This tutorial</span></span>
* <span data-ttu-id="a202b-109">MacOS 向け: [Mac 向けの Visual Studio での Razor ページの概要](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="a202b-109">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="a202b-110">macOS、Linux、Windows 向け: [Visual Studio Code を使用する ASP.NET Core の Razor ページの概要](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="a202b-110">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="a202b-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a202b-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a202b-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a202b-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="a202b-113">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="a202b-113">Create a Razor web app</span></span>

* <span data-ttu-id="a202b-114">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="a202b-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a202b-115">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a202b-115">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="a202b-116">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="a202b-116">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="a202b-117">コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="a202b-117">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="a202b-118">![新しい ASP.NET Core Web アプリケーション](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="a202b-118">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="a202b-119">ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a202b-119">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="a202b-120">以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="a202b-120">The Visual Studio template creates a starter project:</span></span>

![ソリューション エクスプローラー](razor-pages-start/_static/se.png)

<span data-ttu-id="a202b-122">**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。</span><span class="sxs-lookup"><span data-stu-id="a202b-122">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/home.png)

* <span data-ttu-id="a202b-124">Visual Studio で [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a202b-124">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="a202b-125">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a202b-125">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="a202b-126">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="a202b-126">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="a202b-127">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="a202b-127">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="a202b-128">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a202b-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="a202b-129">上の図では、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="a202b-129">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="a202b-130">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a202b-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="a202b-131">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="a202b-131">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="a202b-132">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="a202b-132">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="a202b-133">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="a202b-133">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[<span data-ttu-id="a202b-134">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="a202b-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
