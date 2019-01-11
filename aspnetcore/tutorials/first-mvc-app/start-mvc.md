---
title: ASP.NET Core MVC の概要
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: ASP.NET Core MVC の概要について説明します。
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: cfce3b5792a5d0673bae5ddbba9e2d4d515a6279
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381804"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="48fe6-103">ASP.NET Core MVC の概要</span><span class="sxs-lookup"><span data-stu-id="48fe6-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="48fe6-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48fe6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

https://docs.microsoft.com/en-us/visualstudio/ide/visual-studio-ide?view=vs-2017

<span data-ttu-id="48fe6-105">このチュートリアルでは、ASP.NET Core の MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="48fe6-106">このアプリでは、映画タイトルのデータベースを管理します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="48fe6-107">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="48fe6-108">Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="48fe6-108">Create a web app.</span></span>
> * <span data-ttu-id="48fe6-109">モデルを追加してスキャフォールディングする。</span><span class="sxs-lookup"><span data-stu-id="48fe6-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="48fe6-110">データベースを使用する。</span><span class="sxs-lookup"><span data-stu-id="48fe6-110">Work with a database.</span></span>
> * <span data-ttu-id="48fe6-111">検索と検証を追加する。</span><span class="sxs-lookup"><span data-stu-id="48fe6-111">Add search and validation.</span></span>

<span data-ttu-id="48fe6-112">最後には、映画データを管理および表示できるアプリが完成します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

> [!NOTE]
> <span data-ttu-id="48fe6-113">ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。</span><span class="sxs-lookup"><span data-stu-id="48fe6-113">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="48fe6-114">現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。</span><span class="sxs-lookup"><span data-stu-id="48fe6-114">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="48fe6-115">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="48fe6-115">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48fe6-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48fe6-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="48fe6-117">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-117">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="48fe6-119">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-119">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="48fe6-120">左側のウィンドウで、**[.NET Core]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-120">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="48fe6-121">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-121">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="48fe6-122">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="48fe6-122">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="48fe6-123">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-123">select **OK**</span></span>

![<span data-ttu-id="48fe6-124">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="48fe6-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="48fe6-125">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-125">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="48fe6-126">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.2]** を選択します</span><span class="sxs-lookup"><span data-stu-id="48fe6-126">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="48fe6-127">**[Web アプリケーション (モデル ビュー コントローラー)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="48fe6-127">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="48fe6-128">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-128">select **OK**.</span></span>

![<span data-ttu-id="48fe6-129">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="48fe6-129">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="48fe6-130">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="48fe6-130">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="48fe6-131">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="48fe6-131">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="48fe6-132">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="48fe6-132">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="48fe6-133">**Ctrl + F5** キーを押して、デバッグ以外のモードでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-133">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

* <span data-ttu-id="48fe6-134">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-134">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="48fe6-135">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-135">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="48fe6-136">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="48fe6-136">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="48fe6-137">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-137">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="48fe6-138">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="48fe6-138">In the image above, the port number is 5000.</span></span> <span data-ttu-id="48fe6-139">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-139">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="48fe6-140">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-140">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="48fe6-141">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-141">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="48fe6-142">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-142">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="48fe6-143">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-143">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="48fe6-145">**[IIS Express]** ボタンを選択することで、アプリをデバッグできます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-145">You can debug the app by selecting the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="48fe6-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48fe6-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="48fe6-148">このチュートリアルは VS Code の知識があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="48fe6-148">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="48fe6-149">詳細については、[VS Code の概要](https://code.visualstudio.com/docs)、および [Visual Studio Code のヘルプ](#visual-studio-code-help)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="48fe6-149">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="48fe6-150">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-150">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="48fe6-151">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-151">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="48fe6-152">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-152">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="48fe6-153">"**ビルドとデバッグに必要な資産が 'MvcMovie' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="48fe6-153">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="48fe6-154">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="48fe6-154">Select **Yes**</span></span>

  * <span data-ttu-id="48fe6-155">`dotnet new mvc -o MvcMovie`: *MvcMovie* フォルダー内に新しい ASP.NET Core MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-155">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="48fe6-156">`code -r MvcMovie`:Visual Studio Code で *MvcMovie.csproj* プロジェクト ファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-156">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="48fe6-157">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="48fe6-157">Launch the app</span></span>

* <span data-ttu-id="48fe6-158">**Ctrl + F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-158">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="48fe6-159">Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動され、`http://localhost:5001` に移動されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-159">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="48fe6-160">アドレス バーには、`example.com` などではなく、`localhost:port:5001` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-160">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="48fe6-161">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="48fe6-161">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="48fe6-162">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-162">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="48fe6-163">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="48fe6-164">多くの開発者は、ページを更新して変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-164">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="48fe6-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="48fe6-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="48fe6-166">**[ファイル]**、**[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-166">Select **File** > **New Solution**.</span></span>

  ![macOS の新しいソリューション](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="48fe6-168">**[.NET Core アプリ]** > **[ASP.NET Core]** > **[ASP.NET Core Web アプリ (MVC)]** > **[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-168">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![macOS の [新しいプロジェクト] ダイアログ](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="48fe6-170">**[Configure your new ASP.NET Core Web API]\(新しい ASP.NET Core Web API を構成する\)** ダイアログ ボックスで、既定の**ターゲット フレームワーク** \**.NET Core 2.2* を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-170">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="48fe6-171">プロジェクトに **MvcMovie** という名前を付けて、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-171">Name the project **MvcMovie**, and then select **Create**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="48fe6-172">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="48fe6-172">Launch the app</span></span>

<span data-ttu-id="48fe6-173">**[実行]** > **[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="48fe6-174">Visual Studio for Mac によって [Kestrel](xref:fundamentals/servers/index#kestrel) サーバーが開始され、ブラウザーが起動して `http://localhost:port` にアクセスします。*port* はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="48fe6-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

* <span data-ttu-id="48fe6-175">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="48fe6-176">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="48fe6-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="48fe6-177">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="48fe6-178">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="48fe6-179">**[実行]** メニューから、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---  
<!-- End of VS tabs -->

* <span data-ttu-id="48fe6-180">**[同意する]** を選択し、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-180">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="48fe6-181">このアプリでは個人情報は追跡されません。</span><span class="sxs-lookup"><span data-stu-id="48fe6-181">This app doesn't track personal information.</span></span> <span data-ttu-id="48fe6-182">テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="48fe6-182">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](start-mvc/_static/privacy.png)

  <span data-ttu-id="48fe6-184">次の図は、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="48fe6-184">The following image shows the app after accepting tracking:</span></span>

  ![ホームまたはインデックス ページ](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="48fe6-186">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="48fe6-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="48fe6-187">次へ</span><span class="sxs-lookup"><span data-stu-id="48fe6-187">Next</span></span>](adding-controller.md)  
