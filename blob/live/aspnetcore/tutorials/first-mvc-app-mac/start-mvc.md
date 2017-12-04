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
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="f8693-104">ASP.NET Core MVC と Visual Studio for Mac の概要</span><span class="sxs-lookup"><span data-stu-id="f8693-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="f8693-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8693-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f8693-106">このチュートリアルでは、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="f8693-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="f8693-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="f8693-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f8693-108">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f8693-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="f8693-109">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f8693-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="f8693-110">Linux、macOS、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="f8693-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8693-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f8693-111">Prerequisites</span></span>

<span data-ttu-id="f8693-112">このチュートリアルには、[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="f8693-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="f8693-113">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f8693-113">Install the following:</span></span>

- <span data-ttu-id="f8693-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降</span><span class="sxs-lookup"><span data-stu-id="f8693-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="f8693-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f8693-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="f8693-116">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="f8693-116">Create a web app</span></span>

<span data-ttu-id="f8693-117">Visual Studio で **[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f8693-117">From Visual Studio, select **File > New Solution**.</span></span>

![macOS の新しいソリューション](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="f8693-119">**[.NET Core アプリ]、[ASP.NET Core]、[Web アプリ]、[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f8693-119">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/1.png)

<span data-ttu-id="f8693-121">プロジェクトに **MvcMovie** という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8693-121">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="f8693-123">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="f8693-123">Launch the app</span></span>

<span data-ttu-id="f8693-124">Visual Studio で、**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="f8693-124">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="f8693-125">Visual Studio で [Kestrel](xref:fundamentals/servers/index#kestrel) が開始し、ブラウザーが起動して `http://localhost:port` にアクセスします。*port* はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="f8693-125">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![新しいプロジェクトでのブラウザー](start-mvc/b1.png)

* <span data-ttu-id="f8693-127">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f8693-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f8693-128">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="f8693-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f8693-129">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f8693-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f8693-130">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f8693-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f8693-131">**[実行]** メニューから、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="f8693-131">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="f8693-132">既定のテンプレートには、**[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="f8693-132">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="f8693-133">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="f8693-133">The browser image above doesn't show these links.</span></span> <span data-ttu-id="f8693-134">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="f8693-134">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![新しいプロジェクトでのブラウザー](start-mvc/b2.png)

<span data-ttu-id="f8693-136">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="f8693-136">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f8693-137">次へ</span><span class="sxs-lookup"><span data-stu-id="f8693-137">Next</span></span>](adding-controller.md)  
