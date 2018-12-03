---
title: ASP.NET Core MVC と Visual Studio の概要
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio の概要について説明します。
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 9d50607899058c887597a3d73198552d3ef5b020
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710089"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="8133a-103">ASP.NET Core MVC と Visual Studio の概要</span><span class="sxs-lookup"><span data-stu-id="8133a-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="8133a-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8133a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="8133a-105">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="8133a-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="8133a-106">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8133a-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="8133a-107">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8133a-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="8133a-108">macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8133a-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

> [!NOTE]
> <span data-ttu-id="8133a-109">ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。</span><span class="sxs-lookup"><span data-stu-id="8133a-109">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="8133a-110">現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5)。</span><span class="sxs-lookup"><span data-stu-id="8133a-110">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="8133a-111">Visual Studio と .NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="8133a-111">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="8133a-112">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="8133a-112">Create a web app</span></span>

<span data-ttu-id="8133a-113">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="8133a-113">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="8133a-115">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8133a-115">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8133a-116">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8133a-116">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="8133a-117">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8133a-117">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="8133a-118">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="8133a-118">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="8133a-119">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="8133a-119">Tap **OK**</span></span>

![<span data-ttu-id="8133a-120">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8133a-120">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="8133a-121">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8133a-121">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8133a-122">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8133a-122">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="8133a-123">**[Web アプリケーション (モデル ビュー コントローラー)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="8133a-123">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="8133a-124">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8133a-124">Tap **OK**.</span></span>

![<span data-ttu-id="8133a-125">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8133a-125">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="8133a-126">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="8133a-126">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="8133a-127">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="8133a-127">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8133a-128">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8133a-128">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="8133a-129">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8133a-129">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="8133a-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="8133a-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="8133a-131">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-131">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8133a-132">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-132">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8133a-133">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="8133a-133">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8133a-134">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-134">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8133a-135">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="8133a-135">In the image above, the port number is 5000.</span></span> <span data-ttu-id="8133a-136">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="8133a-136">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="8133a-137">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-137">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8133a-138">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="8133a-138">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8133a-139">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="8133a-139">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8133a-140">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="8133a-140">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8133a-142">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="8133a-142">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="8133a-144">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="8133a-144">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8133a-145">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="8133a-145">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8133a-146">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="8133a-146">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="8133a-148">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8133a-148">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="8133a-149">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="8133a-149">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="8133a-150">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="8133a-150">Create a web app</span></span>

<span data-ttu-id="8133a-151">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="8133a-151">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="8133a-153">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8133a-153">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8133a-154">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8133a-154">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="8133a-155">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8133a-155">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="8133a-156">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="8133a-156">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="8133a-157">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="8133a-157">Tap **OK**</span></span>

![<span data-ttu-id="8133a-158">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8133a-158">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="8133a-159">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8133a-159">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8133a-160">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.-]** を選択します</span><span class="sxs-lookup"><span data-stu-id="8133a-160">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="8133a-161">**[Web Application(Model-View-Controller)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="8133a-161">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="8133a-162">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8133a-162">Tap **OK**.</span></span>

![<span data-ttu-id="8133a-163">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8133a-163">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="8133a-164">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="8133a-164">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="8133a-165">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="8133a-165">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8133a-166">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8133a-166">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="8133a-167">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8133a-167">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="8133a-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="8133a-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="8133a-169">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-169">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8133a-170">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-170">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8133a-171">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="8133a-171">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8133a-172">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-172">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8133a-173">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="8133a-173">In the image above, the port number is 5000.</span></span> <span data-ttu-id="8133a-174">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="8133a-174">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="8133a-175">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8133a-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8133a-176">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="8133a-176">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8133a-177">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="8133a-177">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8133a-178">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="8133a-178">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8133a-180">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="8133a-180">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="8133a-182">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="8133a-182">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8133a-183">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="8133a-183">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8133a-184">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="8133a-184">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="8133a-186">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8133a-186">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="8133a-187">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="8133a-187">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="8133a-188">次へ</span><span class="sxs-lookup"><span data-stu-id="8133a-188">Next</span></span>](adding-controller.md)  
