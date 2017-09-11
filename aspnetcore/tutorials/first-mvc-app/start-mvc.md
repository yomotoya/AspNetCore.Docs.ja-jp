---
title: "ASP.NET Core MVC と Visual Studio の概要"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio の概要"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 9636e0e51e506d294ffb50a21165195c9d04fe20
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="e10f9-104">ASP.NET Core MVC と Visual Studio の概要</span><span class="sxs-lookup"><span data-stu-id="e10f9-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="e10f9-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e10f9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e10f9-106">このチュートリアルでは、[Visual Studio 2017](https://www.visualstudio.com/) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="e10f9-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="e10f9-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e10f9-108">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e10f9-108">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e10f9-109">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e10f9-109">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e10f9-110">macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e10f9-110">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="e10f9-111">このチュートリアルの Visual Studio 2015 バージョンについては、[VS 2015 バージョンの ASP.NET Core ドキュメント (PDF 形式)](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e10f9-111">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="e10f9-112">Visual Studio と .NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="e10f9-112">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e10f9-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e10f9-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e10f9-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e10f9-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e10f9-115">Visual Studio Community 2017 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e10f9-115">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="e10f9-116">コミュニティ ダウンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-116">Select the Community download.</span></span> <span data-ttu-id="e10f9-117">Visual Studio 2017 をインストールしている場合は、この手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="e10f9-117">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="e10f9-118">Visual Studio 2017 ホーム ページのインストーラー</span><span class="sxs-lookup"><span data-stu-id="e10f9-118">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)

<span data-ttu-id="e10f9-119">インストーラーを実行し、次のワークロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-119">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="e10f9-120">**ASP.NET と Web 開発** (**[Web & Cloud]\(Web とクラウド\)** の下)</span><span class="sxs-lookup"><span data-stu-id="e10f9-120">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="e10f9-121">**.NET Core クロスプラットフォームの開発** (**[他のツールセット]** の下)</span><span class="sxs-lookup"><span data-stu-id="e10f9-121">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET と Web の開発ツール** (**[Web & Cloud]\(Web とクラウド\)** の下)](start-mvc/_static/web_workload.png)

![**.NET Core クロスクロスプラットフォームの開発** (**[他のツールセット]** の下)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="e10f9-124">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="e10f9-124">Create a web app</span></span>

<span data-ttu-id="e10f9-125">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-125">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="e10f9-127">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-127">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e10f9-128">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="e10f9-128">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="e10f9-129">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="e10f9-129">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="e10f9-130">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="e10f9-130">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="e10f9-131">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="e10f9-131">Tap **OK**</span></span>

![<span data-ttu-id="e10f9-132">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="e10f9-132">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e10f9-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e10f9-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e10f9-134">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-134">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e10f9-135">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.-]** を選択します</span><span class="sxs-lookup"><span data-stu-id="e10f9-135">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="e10f9-136">**[Web Application(Model-View-Controller)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="e10f9-136">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="e10f9-137">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="e10f9-137">Tap **OK**.</span></span>

![<span data-ttu-id="e10f9-138">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="e10f9-138">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e10f9-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e10f9-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e10f9-140">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-140">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e10f9-141">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 1.1]** をタップします</span><span class="sxs-lookup"><span data-stu-id="e10f9-141">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="e10f9-142">**[Web アプリケーション]** をタップします</span><span class="sxs-lookup"><span data-stu-id="e10f9-142">Tap **Web Application**</span></span>
* <span data-ttu-id="e10f9-143">既定の **[No Authentication]\(認証なし\)** のままにします</span><span class="sxs-lookup"><span data-stu-id="e10f9-143">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="e10f9-144">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="e10f9-144">Tap **OK**.</span></span>

![新しい ASP.NET Core Web アプリ](start-mvc/_static/p3.png)

---

<span data-ttu-id="e10f9-146">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="e10f9-146">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="e10f9-147">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="e10f9-147">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e10f9-148">これは単純なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e10f9-148">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="e10f9-149">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-149">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)

* <span data-ttu-id="e10f9-151">Visual Studio で [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e10f9-151">Visual Studio starts [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e10f9-152">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e10f9-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e10f9-153">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="e10f9-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e10f9-154">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e10f9-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e10f9-155">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="e10f9-155">In the image above, the port number is 5000.</span></span> <span data-ttu-id="e10f9-156">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e10f9-156">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e10f9-157">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="e10f9-157">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e10f9-158">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="e10f9-158">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e10f9-159">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="e10f9-159">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e10f9-161">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="e10f9-161">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="e10f9-163">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="e10f9-163">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e10f9-164">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="e10f9-164">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e10f9-165">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="e10f9-165">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="e10f9-167">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-167">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="e10f9-168">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="e10f9-168">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e10f9-169">次へ</span><span class="sxs-lookup"><span data-stu-id="e10f9-169">Next</span></span>](adding-controller.md)  
