---
title: ASP.NET Core MVC の概要
author: rick-anderson
description: ASP.NET Core MVC の概要について説明します。
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dc3499c89860190b76d6be7b8abeeaef827880d6
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491247"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="e8e77-103">ASP.NET Core MVC の概要</span><span class="sxs-lookup"><span data-stu-id="e8e77-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="e8e77-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e8e77-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="e8e77-105">このチュートリアルでは、ASP.NET Core の MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="e8e77-106">このアプリでは、映画タイトルのデータベースを管理します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="e8e77-107">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e8e77-108">Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="e8e77-108">Create a web app.</span></span>
> * <span data-ttu-id="e8e77-109">モデルを追加してスキャフォールディングする。</span><span class="sxs-lookup"><span data-stu-id="e8e77-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="e8e77-110">データベースを使用する。</span><span class="sxs-lookup"><span data-stu-id="e8e77-110">Work with a database.</span></span>
> * <span data-ttu-id="e8e77-111">検索と検証を追加する。</span><span class="sxs-lookup"><span data-stu-id="e8e77-111">Add search and validation.</span></span>

<span data-ttu-id="e8e77-112">最後には、映画データを管理および表示できるアプリが完成します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="e8e77-113">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="e8e77-113">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8e77-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8e77-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e8e77-115">Visual Studio から **[新しいプロジェクトの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-115">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="e8e77-116">**[ASP.NET Core Web アプリケーション]** を選択し、 **[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-116">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![新しい ASP.NET Core Web アプリケーション](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="e8e77-118">プロジェクトに **MvcMovie** という名前を付けて、 **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-118">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="e8e77-119">コードをコピーするときに名前空間が一致するように、プロジェクトに **MvcMovie** と名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="e8e77-119">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](start-mvc/_static/config.png)


* <span data-ttu-id="e8e77-121">**[Web Application(Model-View-Controller)]\(Web アプリケーション (Model-View-Controller)\)** を選択し、 **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-121">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="e8e77-122">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="e8e77-122">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="e8e77-123">Visual Studio では、作成した MVC プロジェクトに既定のテンプレートが使用されました。</span><span class="sxs-lookup"><span data-stu-id="e8e77-123">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="e8e77-124">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="e8e77-124">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e8e77-125">これは基本的なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e8e77-125">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8e77-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8e77-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e8e77-127">このチュートリアルは VS Code の知識があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="e8e77-127">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="e8e77-128">詳細については、[VS Code の概要](https://code.visualstudio.com/docs)、および [Visual Studio Code のヘルプ](#visual-studio-code-help)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e8e77-128">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="e8e77-129">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-129">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e8e77-130">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-130">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="e8e77-131">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-131">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="e8e77-132">"**ビルドとデバッグに必要な資産が 'MvcMovie' にありません。追加しますか?** " という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="e8e77-132">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="e8e77-133">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="e8e77-133">Select **Yes**</span></span>

  * <span data-ttu-id="e8e77-134">`dotnet new mvc -o MvcMovie`: *MvcMovie* フォルダー内に新しい ASP.NET Core MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-134">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="e8e77-135">`code -r MvcMovie`:Visual Studio Code で *MvcMovie.csproj* プロジェクト ファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-135">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8e77-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e8e77-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e8e77-137">**[ファイル]** 、 **[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-137">Select **File** > **New Solution**.</span></span>

  ![macOS の新しいソリューション](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="e8e77-139">**[.NET Core]**  >  **[アプリ]**  >  **[Web Application (Model-View-Controller)]\(Web アプリケーション (Model-View-Controller)\)**  >  **[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-139">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS の [新しいプロジェクト] ダイアログ](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="e8e77-141">**[Configure your new ASP.NET Core Web API]\(新しい ASP.NET Core Web API を構成する\)** ダイアログで、 **[.NET Core 2.2]** という既定の **[ターゲット フレームワーク]** を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-141">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![macOS .NET Core 2.2 の選択](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="e8e77-143">プロジェクトに **MvcMovie** という名前を付けて、 **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-143">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="e8e77-144">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="e8e77-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8e77-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8e77-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e8e77-146">**Ctrl + F5** キーを押して、デバッグ以外のモードでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-146">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="e8e77-147">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="e8e77-148">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-148">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e8e77-149">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="e8e77-149">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e8e77-150">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-150">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="e8e77-151">Ctrl + F5 キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、コード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-151">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e8e77-152">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-152">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e8e77-153">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-153">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e8e77-155">**[IIS Express]** ボタンを選択することで、アプリをデバッグできます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-155">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="e8e77-157">**[同意する]** を選択し、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-157">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e8e77-158">このアプリでは個人情報は追跡されません。</span><span class="sxs-lookup"><span data-stu-id="e8e77-158">This app doesn't track personal information.</span></span> <span data-ttu-id="e8e77-159">テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-159">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](start-mvc/_static/privacy.png)

  <span data-ttu-id="e8e77-161">次の図は、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="e8e77-161">The following image shows the app after accepting tracking:</span></span>

  ![ホームまたはインデックス ページ](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e8e77-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e8e77-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e8e77-164">Ctrl + F5 キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-164">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="e8e77-165">Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動して、`https://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-165">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="e8e77-166">アドレス バーには、`example.com` などではなく、`localhost:port:5001` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-166">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="e8e77-167">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="e8e77-167">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="e8e77-168">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-168">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="e8e77-169">Ctrl + F5 キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、コード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-169">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e8e77-170">多くの開発者は、ページを更新して変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-170">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="e8e77-171">**[同意する]** を選択し、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-171">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e8e77-172">このアプリでは個人情報は追跡されません。</span><span class="sxs-lookup"><span data-stu-id="e8e77-172">This app doesn't track personal information.</span></span> <span data-ttu-id="e8e77-173">テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-173">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](start-mvc/_static/privacy.png)

  <span data-ttu-id="e8e77-175">次の図は、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="e8e77-175">The following image shows the app after accepting tracking:</span></span>

  ![ホームまたはインデックス ページ](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e8e77-177">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e8e77-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e8e77-178">**[実行]**  >  **[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-178">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="e8e77-179">Visual Studio for Mac によって [Kestrel](xref:fundamentals/servers/index#kestrel) サーバーが開始され、ブラウザーが起動して `http://localhost:port` にアクセスします。*port* はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="e8e77-179">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="e8e77-180">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-180">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e8e77-181">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="e8e77-181">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e8e77-182">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-182">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e8e77-183">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-183">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e8e77-184">**[実行]** メニューから、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-184">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="e8e77-185">**[同意する]** を選択し、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-185">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="e8e77-186">このアプリでは個人情報は追跡されません。</span><span class="sxs-lookup"><span data-stu-id="e8e77-186">This app doesn't track personal information.</span></span> <span data-ttu-id="e8e77-187">テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e8e77-187">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="e8e77-189">次の図は、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="e8e77-189">The following image shows the app after accepting tracking:</span></span>

  ![ホームまたはインデックス ページ](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="e8e77-191">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="e8e77-191">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e8e77-192">次へ</span><span class="sxs-lookup"><span data-stu-id="e8e77-192">Next</span></span>](adding-controller.md)
