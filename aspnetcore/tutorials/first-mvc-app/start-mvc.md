---
title: ASP.NET Core MVC と Visual Studio の概要
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio の概要について説明します。
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 3272700c7739778a6a341ae8ee424fd69605ca53
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729718"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="8d1a6-103">ASP.NET Core MVC と Visual Studio の概要</span><span class="sxs-lookup"><span data-stu-id="8d1a6-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="8d1a6-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8d1a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="8d1a6-105">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="8d1a6-106">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8d1a6-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="8d1a6-107">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8d1a6-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="8d1a6-108">macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8d1a6-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="8d1a6-109">Visual Studio と .NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="8d1a6-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8d1a6-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="8d1a6-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="8d1a6-111">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="8d1a6-111">Create a web app</span></span>

<span data-ttu-id="8d1a6-112">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-112">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="8d1a6-114">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8d1a6-115">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="8d1a6-116">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="8d1a6-117">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="8d1a6-118">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="8d1a6-118">Tap **OK**</span></span>

![<span data-ttu-id="8d1a6-119">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8d1a6-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="8d1a6-120">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8d1a6-121">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="8d1a6-122">**[Web Application(Model-View-Controller)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="8d1a6-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="8d1a6-123">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-123">Tap **OK**.</span></span>

![<span data-ttu-id="8d1a6-124">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8d1a6-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="8d1a6-125">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="8d1a6-126">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8d1a6-127">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="8d1a6-128">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)

* <span data-ttu-id="8d1a6-130">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8d1a6-131">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8d1a6-132">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8d1a6-133">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8d1a6-134">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="8d1a6-135">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="8d1a6-136">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8d1a6-137">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8d1a6-138">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8d1a6-139">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8d1a6-141">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="8d1a6-141">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="8d1a6-143">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8d1a6-144">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8d1a6-145">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="8d1a6-147">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="8d1a6-148">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8d1a6-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8d1a6-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="8d1a6-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="8d1a6-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8d1a6-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8d1a6-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="8d1a6-152">Visual Studio Community 2017 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="8d1a6-153">コミュニティ ダウンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-153">Select the Community download.</span></span> <span data-ttu-id="8d1a6-154">Visual Studio 2017 をインストールしている場合は、この手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="8d1a6-155">Visual Studio 2017 ホーム ページのインストーラー</span><span class="sxs-lookup"><span data-stu-id="8d1a6-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="8d1a6-156">インストーラーを実行し、次のワークロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="8d1a6-157">**ASP.NET と Web 開発** (**[Web & Cloud]\(Web とクラウド\)** の下)</span><span class="sxs-lookup"><span data-stu-id="8d1a6-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="8d1a6-158">**.NET Core クロスプラットフォームの開発** (**[他のツールセット]** の下)</span><span class="sxs-lookup"><span data-stu-id="8d1a6-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET と Web の開発ツール** (**[Web & Cloud]\(Web とクラウド\)** の下)](start-mvc/_static/web_workload.png)

![**.NET Core クロスクロスプラットフォームの開発** (**[他のツールセット]** の下)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="8d1a6-161">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="8d1a6-161">Create a web app</span></span>

<span data-ttu-id="8d1a6-162">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-162">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="8d1a6-164">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8d1a6-165">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="8d1a6-166">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="8d1a6-167">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="8d1a6-168">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="8d1a6-168">Tap **OK**</span></span>

![<span data-ttu-id="8d1a6-169">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8d1a6-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8d1a6-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8d1a6-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8d1a6-171">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8d1a6-172">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.-]** を選択します</span><span class="sxs-lookup"><span data-stu-id="8d1a6-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="8d1a6-173">**[Web Application(Model-View-Controller)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="8d1a6-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="8d1a6-174">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-174">Tap **OK**.</span></span>

![<span data-ttu-id="8d1a6-175">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="8d1a6-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8d1a6-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8d1a6-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8d1a6-177">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8d1a6-178">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 1.1]** をタップします</span><span class="sxs-lookup"><span data-stu-id="8d1a6-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="8d1a6-179">**[Web アプリケーション]** をタップします</span><span class="sxs-lookup"><span data-stu-id="8d1a6-179">Tap **Web Application**</span></span>
* <span data-ttu-id="8d1a6-180">既定の **[No Authentication]\(認証なし\)** のままにします</span><span class="sxs-lookup"><span data-stu-id="8d1a6-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="8d1a6-181">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-181">Tap **OK**.</span></span>

![新しい ASP.NET Core Web アプリ](start-mvc/_static/p3.png)

---

<span data-ttu-id="8d1a6-183">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="8d1a6-184">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8d1a6-185">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="8d1a6-186">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)

* <span data-ttu-id="8d1a6-188">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8d1a6-189">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8d1a6-190">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8d1a6-191">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8d1a6-192">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="8d1a6-193">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="8d1a6-194">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8d1a6-195">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8d1a6-196">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8d1a6-197">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8d1a6-199">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="8d1a6-199">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="8d1a6-201">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8d1a6-202">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8d1a6-203">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="8d1a6-205">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="8d1a6-206">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="8d1a6-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="8d1a6-207">次へ</span><span class="sxs-lookup"><span data-stu-id="8d1a6-207">Next</span></span>](adding-controller.md)  
