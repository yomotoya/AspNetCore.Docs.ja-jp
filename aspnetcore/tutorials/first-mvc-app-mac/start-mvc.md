---
title: "ASP.NET Core MVC と Visual Studio for Mac の概要"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio の概要"
keywords: ASP.NET Core,MVC,Visual Studio for Mac,Entity Framework
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 387d7a91ae7d58cbc293c04039017df1dd208c82
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="3af0d-104">ASP.NET Core MVC と Visual Studio for Mac の概要</span><span class="sxs-lookup"><span data-stu-id="3af0d-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="3af0d-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3af0d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3af0d-106">このチュートリアルでは、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="3af0d-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="3af0d-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="3af0d-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="3af0d-108">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3af0d-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="3af0d-109">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3af0d-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="3af0d-110">Linux、macOS、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3af0d-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3af0d-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3af0d-111">Prerequisites</span></span>

<span data-ttu-id="3af0d-112">このチュートリアルには、[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="3af0d-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="3af0d-113">ASP.NET Core 1.1 バージョンについては、[この PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3af0d-113">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="3af0d-114">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3af0d-114">Install the following:</span></span>

- <span data-ttu-id="3af0d-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降</span><span class="sxs-lookup"><span data-stu-id="3af0d-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="3af0d-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3af0d-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="3af0d-117">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="3af0d-117">Create a web app</span></span>

<span data-ttu-id="3af0d-118">Visual Studio で **[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="3af0d-118">From Visual Studio, select **File > New Solution**.</span></span>

![macOS の新しいソリューション](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="3af0d-120">**[.NET Core アプリ]、[ASP.NET Core]、[Web アプリ]、[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="3af0d-120">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/1.png)

<span data-ttu-id="3af0d-122">プロジェクトに **MvcMovie** という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3af0d-122">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="3af0d-124">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="3af0d-124">Launch the app</span></span>

<span data-ttu-id="3af0d-125">Visual Studio で、**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="3af0d-125">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="3af0d-126">Visual Studio で [Kestrel](xref:fundamentals/servers/index#Kestrel) が開始し、ブラウザーが起動して `http://localhost:port` にアクセスします。*port* はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="3af0d-126">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#Kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![新しいプロジェクトでのブラウザー](start-mvc/b1.png)

* <span data-ttu-id="3af0d-128">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3af0d-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="3af0d-129">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="3af0d-129">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="3af0d-130">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3af0d-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="3af0d-131">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3af0d-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="3af0d-132">**[実行]** メニューから、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="3af0d-132">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="3af0d-133">既定のテンプレートには、**[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="3af0d-133">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="3af0d-134">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="3af0d-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="3af0d-135">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="3af0d-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![新しいプロジェクトでのブラウザー](start-mvc/b2.png)

<span data-ttu-id="3af0d-137">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="3af0d-137">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="3af0d-138">次へ</span><span class="sxs-lookup"><span data-stu-id="3af0d-138">Next</span></span>](adding-controller.md)  
