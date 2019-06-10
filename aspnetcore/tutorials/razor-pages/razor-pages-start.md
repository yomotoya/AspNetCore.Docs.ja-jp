---
title: 'チュートリアル: ASP.NET Core の Razor ページの概要'
author: rick-anderson
description: このチュートリアル シリーズでは、ASP.NET Core で Razor ページを使用する方法を示します。 モデルの作成、Razor ページのコードの生成、Entity Framework Core と SQL Server を使用したデータ アクセス、検索機能の追加、入力検証の追加、および移行を使用したモデルの更新の方法について説明します。
ms.author: riande
ms.date: 6/3/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d843e47ccb5180fab34b4c4c4a4b5cbda21289bf
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491207"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="c270f-104">チュートリアル: ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="c270f-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c270f-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c270f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c270f-106">これは、シリーズの最初のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="c270f-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="c270f-107">[このシリーズ](xref:tutorials/razor-pages/index)では、ASP.NET Core の Razor Pages Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="c270f-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="c270f-108">シリーズの最後には、映画のデータベースを管理できるアプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="c270f-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="c270f-109">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="c270f-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c270f-110">Razor Pages Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="c270f-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="c270f-111">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c270f-111">Run the app.</span></span>
> * <span data-ttu-id="c270f-112">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="c270f-112">Examine the project files.</span></span>

<span data-ttu-id="c270f-113">このチュートリアルの最後には、以降のチュートリアルでビルドする作業用 Razor Pages Web アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="c270f-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="c270f-115">Razor ページ Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="c270f-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c270f-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c270f-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c270f-117">Visual Studio の **[ファイル]** メニューから、 **[新規作成]**  >  **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="c270f-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="c270f-118">新しい ASP.NET CoreWeb アプリケーションを作成し、 **[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c270f-118">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="c270f-120">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="c270f-120">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c270f-121">コードのコピーおよび貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="c270f-121">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code code.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/config.png)

* <span data-ttu-id="c270f-123">ドロップダウンの **[ASP.NET Core 2.2]** 、 **[Web アプリケーション]** の順に選択し、 **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c270f-123">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="c270f-125">次のスターター プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-125">The following starter project is created:</span></span>

  ![ソリューション エクスプローラー](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c270f-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c270f-127">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c270f-128">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="c270f-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="c270f-129">プロジェクトを格納するディレクトリ (`cd`) に変更します。</span><span class="sxs-lookup"><span data-stu-id="c270f-129">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="c270f-130">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c270f-130">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="c270f-131">`dotnet new` コマンド: *RazorPagesMovie* フォルダーに新しい Razor Pages プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-131">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="c270f-132">`code` コマンドは、Visual Studio Code の現在のインスタンス内で *RazorPagesMovie* フォルダーを開きます。</span><span class="sxs-lookup"><span data-stu-id="c270f-132">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="c270f-133">状態バーの OmniSharp フレーム アイコンが緑色になり、"**ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?** " という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="c270f-133">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="c270f-134">**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c270f-134">Select **Yes**.</span></span>

  <span data-ttu-id="c270f-135">*launch.json* ファイルと *tasks.json* ファイルを格納している *.vscode* ディレクトリが、プロジェクトのルート ディレクトリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-135">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c270f-136">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c270f-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c270f-137">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c270f-137">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="c270f-138">上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-138">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="c270f-139">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="c270f-139">Open the project</span></span>

<span data-ttu-id="c270f-140">Visual Studio から、 **[ファイル]、[開く]** の順に選択し、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="c270f-140">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="c270f-141">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="c270f-141">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c270f-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c270f-142">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c270f-143">Ctrl + F5 キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="c270f-143">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="c270f-144">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-144">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c270f-145">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-145">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c270f-146">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="c270f-146">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="c270f-147">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-147">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c270f-148">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-148">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="c270f-149">アプリのホーム ページ上で、 **[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="c270f-149">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c270f-150">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="c270f-150">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="c270f-152">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="c270f-152">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c270f-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c270f-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="c270f-155">**Ctrl + F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="c270f-155">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c270f-156">Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動して、`http://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="c270f-156">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="c270f-157">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c270f-158">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="c270f-158">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="c270f-159">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-159">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="c270f-160">アプリのホーム ページ上で、 **[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="c270f-160">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c270f-161">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="c270f-161">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="c270f-163">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="c270f-163">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c270f-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c270f-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="c270f-166">**Cmd - Opt - F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="c270f-166">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c270f-167">Visual Studio は [Kestrel](xref:fundamentals/servers/kestrel) を開始し、ブラウザーを起動して、`http://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="c270f-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="c270f-168">アプリのホーム ページ上で、 **[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="c270f-168">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c270f-169">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="c270f-169">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="c270f-171">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="c270f-171">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="c270f-173">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="c270f-173">Examine the project files</span></span>

<span data-ttu-id="c270f-174">以降のチュートリアルで使用するメイン プロジェクト フォルダーとファイルの概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c270f-174">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="c270f-175">Pages フォルダー</span><span class="sxs-lookup"><span data-stu-id="c270f-175">Pages folder</span></span>

<span data-ttu-id="c270f-176">Razor ページとサポート ファイルが格納されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-176">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="c270f-177">各 Razor ページは、次のファイルのペアとなります。</span><span class="sxs-lookup"><span data-stu-id="c270f-177">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="c270f-178">*.cshtml* ファイル: HTML マークアップと、Razor 構文を使用した C# コードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-178">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="c270f-179">*.cshtml.cs* ファイル: ページ イベントを処理する C# コードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-179">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="c270f-180">サポート ファイルには、アンダー スコアで始まる名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="c270f-180">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="c270f-181">たとえば、 *_Layout.cshtml* ファイルでは、すべてのページに共通の UI 要素が構成されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-181">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="c270f-182">このファイルでは、ページの上部に表示されるナビゲーション メニューと、ページの下部に表示される著作権の通知が設定されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-182">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="c270f-183">詳細については、「<xref:mvc/views/layout>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c270f-183">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="c270f-184">wwwroot フォルダー</span><span class="sxs-lookup"><span data-stu-id="c270f-184">wwwroot folder</span></span>

<span data-ttu-id="c270f-185">HTML ファイル、JavaScript ファイル、CSS ファイルなどの静的ファイルが格納されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-185">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="c270f-186">詳細については、「<xref:fundamentals/static-files>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c270f-186">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c270f-187">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="c270f-187">appSettings.json</span></span>

<span data-ttu-id="c270f-188">接続文字列などの構成データが保存されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-188">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="c270f-189">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c270f-189">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="c270f-190">Program.cs</span><span class="sxs-lookup"><span data-stu-id="c270f-190">Program.cs</span></span>

<span data-ttu-id="c270f-191">プログラムのエントリ ポイントが保存されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-191">Contains the entry point for the program.</span></span> <span data-ttu-id="c270f-192">詳細については、「<xref:fundamentals/host/web-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c270f-192">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="c270f-193">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c270f-193">Startup.cs</span></span>

<span data-ttu-id="c270f-194">cookie に対する同意が必要かどうかなど、アプリの動作を構成するコードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="c270f-194">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="c270f-195">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c270f-195">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c270f-196">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c270f-196">Additional resources</span></span>

* [<span data-ttu-id="c270f-197">このチュートリアルの YouTube バージョン</span><span class="sxs-lookup"><span data-stu-id="c270f-197">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="c270f-198">次の手順</span><span class="sxs-lookup"><span data-stu-id="c270f-198">Next steps</span></span>

<span data-ttu-id="c270f-199">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="c270f-199">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c270f-200">Razor ページ Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="c270f-200">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="c270f-201">アプリを実行する。</span><span class="sxs-lookup"><span data-stu-id="c270f-201">Ran the app.</span></span>
> * <span data-ttu-id="c270f-202">プロジェクト ファイルを確認する。</span><span class="sxs-lookup"><span data-stu-id="c270f-202">Examined the project files.</span></span>

<span data-ttu-id="c270f-203">このシリーズの次のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="c270f-203">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c270f-204">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="c270f-204">Add a model</span></span>](xref:tutorials/razor-pages/model)
