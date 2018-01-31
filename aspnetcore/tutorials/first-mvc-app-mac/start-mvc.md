---
title: "ASP.NET Core MVC と Visual Studio for Mac の概要"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio の概要"
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 05a2323851c58c95667066a74c11f1d015405e6f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="539cd-103">ASP.NET Core MVC と Visual Studio for Mac の概要</span><span class="sxs-lookup"><span data-stu-id="539cd-103">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="539cd-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="539cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="539cd-105">このチュートリアルでは、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="539cd-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="539cd-106">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="539cd-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="539cd-107">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="539cd-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="539cd-108">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="539cd-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="539cd-109">Linux、macOS、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを構築する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="539cd-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="539cd-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="539cd-110">Prerequisites</span></span>

<span data-ttu-id="539cd-111">このチュートリアルには、[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="539cd-111">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="539cd-112">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="539cd-112">Install the following:</span></span>

- <span data-ttu-id="539cd-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降</span><span class="sxs-lookup"><span data-stu-id="539cd-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="539cd-114">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="539cd-114">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="539cd-115">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="539cd-115">Create a web app</span></span>

<span data-ttu-id="539cd-116">Visual Studio で **[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="539cd-116">From Visual Studio, select **File > New Solution**.</span></span>

![macOS の新しいソリューション](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="539cd-118">**[.NET Core アプリ]、[ASP.NET Core]、[Web アプリ]、[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="539cd-118">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/1.png)

<span data-ttu-id="539cd-120">プロジェクトに **MvcMovie** という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="539cd-120">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="539cd-122">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="539cd-122">Launch the app</span></span>

<span data-ttu-id="539cd-123">Visual Studio で、**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="539cd-123">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="539cd-124">Visual Studio で [Kestrel](xref:fundamentals/servers/index#kestrel) が開始し、ブラウザーが起動して `http://localhost:port` にアクセスします。*port* はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="539cd-124">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![新しいプロジェクトでのブラウザー](start-mvc/b1.png)

* <span data-ttu-id="539cd-126">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="539cd-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="539cd-127">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="539cd-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="539cd-128">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="539cd-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="539cd-129">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="539cd-129">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="539cd-130">**[実行]** メニューから、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="539cd-130">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="539cd-131">既定のテンプレートには、**[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="539cd-131">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="539cd-132">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="539cd-132">The browser image above doesn't show these links.</span></span> <span data-ttu-id="539cd-133">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="539cd-133">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![新しいプロジェクトでのブラウザー](start-mvc/b2.png)

<span data-ttu-id="539cd-135">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="539cd-135">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="539cd-136">次へ</span><span class="sxs-lookup"><span data-stu-id="539cd-136">Next</span></span>](adding-controller.md)  
