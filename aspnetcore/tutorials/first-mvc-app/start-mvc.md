---
title: ASP.NET Core MVC と Visual Studio の概要
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio の概要について説明します。
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011707"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="07d03-103">ASP.NET Core MVC と Visual Studio の概要</span><span class="sxs-lookup"><span data-stu-id="07d03-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="07d03-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="07d03-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="07d03-105">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="07d03-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="07d03-106">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="07d03-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="07d03-107">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="07d03-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="07d03-108">macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="07d03-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="07d03-109">Visual Studio と .NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="07d03-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="07d03-110">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="07d03-110">Create a web app</span></span>

<span data-ttu-id="07d03-111">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="07d03-111">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="07d03-113">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="07d03-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="07d03-114">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="07d03-115">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="07d03-116">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="07d03-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="07d03-117">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="07d03-117">Tap **OK**</span></span>

![<span data-ttu-id="07d03-118">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="07d03-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="07d03-119">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="07d03-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="07d03-120">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="07d03-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="07d03-121">**[Web Application(Model-View-Controller)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="07d03-121">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="07d03-122">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-122">Tap **OK**.</span></span>

![<span data-ttu-id="07d03-123">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="07d03-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="07d03-124">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="07d03-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="07d03-125">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="07d03-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="07d03-126">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="07d03-126">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="07d03-127">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="07d03-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="07d03-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="07d03-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="07d03-129">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="07d03-130">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="07d03-131">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="07d03-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="07d03-132">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="07d03-133">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="07d03-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="07d03-134">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="07d03-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="07d03-135">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="07d03-136">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="07d03-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="07d03-137">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="07d03-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="07d03-138">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="07d03-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="07d03-140">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="07d03-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="07d03-142">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="07d03-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="07d03-143">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="07d03-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="07d03-144">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="07d03-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="07d03-146">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="07d03-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="07d03-147">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="07d03-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07d03-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07d03-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07d03-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07d03-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="07d03-150">Visual Studio Community 2017 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="07d03-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="07d03-151">コミュニティ ダウンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="07d03-151">Select the Community download.</span></span> <span data-ttu-id="07d03-152">Visual Studio 2017 をインストールしている場合は、この手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="07d03-153">Visual Studio 2017 ホーム ページのインストーラー</span><span class="sxs-lookup"><span data-stu-id="07d03-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="07d03-154">インストーラーを実行し、次のワークロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="07d03-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="07d03-155">**ASP.NET と Web 開発** (**[Web & Cloud]\(Web とクラウド\)** の下)</span><span class="sxs-lookup"><span data-stu-id="07d03-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="07d03-156">**.NET Core クロスプラットフォームの開発** (**[他のツールセット]** の下)</span><span class="sxs-lookup"><span data-stu-id="07d03-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET と Web の開発ツール** (**[Web & Cloud]\(Web とクラウド\)** の下)](start-mvc/_static/web_workload.png)

![**.NET Core クロスクロスプラットフォームの開発** (**[他のツールセット]** の下)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="07d03-159">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="07d03-159">Create a web app</span></span>

<span data-ttu-id="07d03-160">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="07d03-160">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="07d03-162">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="07d03-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="07d03-163">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="07d03-164">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="07d03-165">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="07d03-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="07d03-166">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="07d03-166">Tap **OK**</span></span>

![<span data-ttu-id="07d03-167">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="07d03-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07d03-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07d03-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="07d03-169">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="07d03-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="07d03-170">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.-]** を選択します</span><span class="sxs-lookup"><span data-stu-id="07d03-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="07d03-171">**[Web Application(Model-View-Controller)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="07d03-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="07d03-172">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-172">Tap **OK**.</span></span>

![<span data-ttu-id="07d03-173">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="07d03-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07d03-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07d03-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="07d03-175">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="07d03-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="07d03-176">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 1.1]** をタップします</span><span class="sxs-lookup"><span data-stu-id="07d03-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="07d03-177">**[Web アプリケーション]** をタップします</span><span class="sxs-lookup"><span data-stu-id="07d03-177">Tap **Web Application**</span></span>
* <span data-ttu-id="07d03-178">既定の **[No Authentication]\(認証なし\)** のままにします</span><span class="sxs-lookup"><span data-stu-id="07d03-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="07d03-179">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="07d03-179">Tap **OK**.</span></span>

![新しい ASP.NET Core Web アプリ](start-mvc/_static/p3.png)

---

<span data-ttu-id="07d03-181">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="07d03-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="07d03-182">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="07d03-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="07d03-183">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="07d03-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="07d03-184">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="07d03-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="07d03-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="07d03-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="07d03-186">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="07d03-187">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="07d03-188">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="07d03-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="07d03-189">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="07d03-190">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="07d03-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="07d03-191">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="07d03-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="07d03-192">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="07d03-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="07d03-193">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="07d03-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="07d03-194">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="07d03-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="07d03-195">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="07d03-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="07d03-197">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="07d03-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="07d03-199">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="07d03-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="07d03-200">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="07d03-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="07d03-201">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="07d03-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="07d03-203">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="07d03-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="07d03-204">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="07d03-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="07d03-205">次へ</span><span class="sxs-lookup"><span data-stu-id="07d03-205">Next</span></span>](adding-controller.md)  
