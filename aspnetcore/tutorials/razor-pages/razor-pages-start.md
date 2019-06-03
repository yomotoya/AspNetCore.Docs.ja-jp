---
title: 'チュートリアル: ASP.NET Core の Razor ページの概要'
author: rick-anderson
description: このチュートリアル シリーズでは、ASP.NET Core で Razor ページを使用する方法を示します。 モデルの作成、Razor ページのコードの生成、Entity Framework Core と SQL Server を使用したデータ アクセス、検索機能の追加、入力検証の追加、および移行を使用したモデルの更新の方法について説明します。
ms.author: riande
ms.date: 05/30/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e9f11f68aa138ab74a0ffbbd0e32067bc984606d
ms.sourcegitcommit: 9ae1fd11f39b0a72b2ae42f0b450345e6e306bc0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/30/2019
ms.locfileid: "66415669"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f767b-104">チュートリアル: ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="f767b-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f767b-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f767b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f767b-106">これは、シリーズの最初のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="f767b-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="f767b-107">[このシリーズ](xref:tutorials/razor-pages/index)では、ASP.NET Core の Razor Pages Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="f767b-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="f767b-108">シリーズの最後には、映画のデータベースを管理できるアプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="f767b-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="f767b-109">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="f767b-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f767b-110">Razor Pages Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="f767b-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="f767b-111">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f767b-111">Run the app.</span></span>
> * <span data-ttu-id="f767b-112">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="f767b-112">Examine the project files.</span></span>

<span data-ttu-id="f767b-113">このチュートリアルの最後には、以降のチュートリアルでビルドする作業用 Razor Pages Web アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="f767b-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="f767b-115">Razor ページ Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="f767b-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f767b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f767b-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f767b-117">Visual Studio の **[ファイル]** メニューから、 **[新規作成]**  >  **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f767b-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="f767b-118">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f767b-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f767b-119">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="f767b-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f767b-120">コードのコピーおよび貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="f767b-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="f767b-122">ドロップダウン リストで **[ASP.NET Core 2.2]** を選択してから、 **[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f767b-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="f767b-124">次のスターター プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-124">The following starter project is created:</span></span>

  ![ソリューション エクスプローラー](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f767b-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f767b-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f767b-127">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="f767b-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="f767b-128">プロジェクトを格納するディレクトリ (`cd`) に変更します。</span><span class="sxs-lookup"><span data-stu-id="f767b-128">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="f767b-129">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f767b-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="f767b-130">`dotnet new` コマンド: *RazorPagesMovie* フォルダーに新しい Razor Pages プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="f767b-131">`code` コマンドは、Visual Studio Code の現在のインスタンス内で *RazorPagesMovie* フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="f767b-131">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="f767b-132">状態バーの OmniSharp フレーム アイコンが緑色になり、"**ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?** " という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="f767b-132">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="f767b-133">**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f767b-133">Select **Yes**.</span></span>

  <span data-ttu-id="f767b-134">*launch.json* ファイルと *tasks.json* ファイルを格納している *.vscode* ディレクトリが、プロジェクトのルート ディレクトリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-134">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f767b-135">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f767b-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f767b-136">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f767b-136">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="f767b-137">上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-137">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="f767b-138">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="f767b-138">Open the project</span></span>

<span data-ttu-id="f767b-139">Visual Studio から、 **[ファイル]、[開く]** の順に選択し、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="f767b-139">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="f767b-140">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="f767b-140">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f767b-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f767b-141">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f767b-142">Ctrl + F5 キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="f767b-142">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="f767b-143">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-143">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="f767b-144">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-144">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f767b-145">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="f767b-145">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="f767b-146">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-146">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f767b-147">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-147">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="f767b-148">アプリのホーム ページ上で、 **[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="f767b-148">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f767b-149">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="f767b-149">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f767b-151">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="f767b-151">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f767b-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f767b-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="f767b-154">**Ctrl + F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="f767b-154">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f767b-155">Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動して、`http://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="f767b-155">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="f767b-156">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-156">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f767b-157">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="f767b-157">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="f767b-158">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-158">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="f767b-159">アプリのホーム ページ上で、 **[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="f767b-159">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f767b-160">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="f767b-160">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="f767b-162">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="f767b-162">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f767b-164">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f767b-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="f767b-165">**Cmd - Opt - F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="f767b-165">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="f767b-166">Visual Studio は [Kestrel](xref:fundamentals/servers/kestrel) を開始し、ブラウザーを起動して、`http://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="f767b-166">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="f767b-167">アプリのホーム ページ上で、 **[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="f767b-167">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="f767b-168">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="f767b-168">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="f767b-170">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="f767b-170">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="f767b-172">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="f767b-172">Examine the project files</span></span>

<span data-ttu-id="f767b-173">以降のチュートリアルで使用するメイン プロジェクト フォルダーとファイルの概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f767b-173">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="f767b-174">Pages フォルダー</span><span class="sxs-lookup"><span data-stu-id="f767b-174">Pages folder</span></span>

<span data-ttu-id="f767b-175">Razor ページとサポート ファイルが格納されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-175">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="f767b-176">各 Razor ページは、次のファイルのペアとなります。</span><span class="sxs-lookup"><span data-stu-id="f767b-176">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="f767b-177">*.cshtml* ファイル: HTML マークアップと、Razor 構文を使用した C# コードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-177">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="f767b-178">*.cshtml.cs* ファイル: ページ イベントを処理する C# コードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-178">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="f767b-179">サポート ファイルには、アンダー スコアで始まる名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="f767b-179">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="f767b-180">たとえば、 *_Layout.cshtml* ファイルでは、すべてのページに共通の UI 要素が構成されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-180">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="f767b-181">このファイルでは、ページの上部に表示されるナビゲーション メニューと、ページの下部に表示される著作権の通知が設定されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-181">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="f767b-182">詳細については、「<xref:mvc/views/layout>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f767b-182">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="f767b-183">wwwroot フォルダー</span><span class="sxs-lookup"><span data-stu-id="f767b-183">wwwroot folder</span></span>

<span data-ttu-id="f767b-184">HTML ファイル、JavaScript ファイル、CSS ファイルなどの静的ファイルが格納されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-184">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="f767b-185">詳細については、「<xref:fundamentals/static-files>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f767b-185">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="f767b-186">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="f767b-186">appSettings.json</span></span>

<span data-ttu-id="f767b-187">接続文字列などの構成データが保存されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-187">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="f767b-188">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f767b-188">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="f767b-189">Program.cs</span><span class="sxs-lookup"><span data-stu-id="f767b-189">Program.cs</span></span>

<span data-ttu-id="f767b-190">プログラムのエントリ ポイントが保存されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-190">Contains the entry point for the program.</span></span> <span data-ttu-id="f767b-191">詳細については、「<xref:fundamentals/host/web-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f767b-191">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="f767b-192">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f767b-192">Startup.cs</span></span>

<span data-ttu-id="f767b-193">cookie に対する同意が必要かどうかなど、アプリの動作を構成するコードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="f767b-193">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="f767b-194">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f767b-194">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f767b-195">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f767b-195">Additional resources</span></span>

* [<span data-ttu-id="f767b-196">このチュートリアルの YouTube バージョン</span><span class="sxs-lookup"><span data-stu-id="f767b-196">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="f767b-197">次の手順</span><span class="sxs-lookup"><span data-stu-id="f767b-197">Next steps</span></span>

<span data-ttu-id="f767b-198">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="f767b-198">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f767b-199">Razor ページ Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="f767b-199">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="f767b-200">アプリを実行する。</span><span class="sxs-lookup"><span data-stu-id="f767b-200">Ran the app.</span></span>
> * <span data-ttu-id="f767b-201">プロジェクト ファイルを確認する。</span><span class="sxs-lookup"><span data-stu-id="f767b-201">Examined the project files.</span></span>

<span data-ttu-id="f767b-202">このシリーズの次のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="f767b-202">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f767b-203">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="f767b-203">Add a model</span></span>](xref:tutorials/razor-pages/model)
