---
title: 'チュートリアル: ASP.NET Core の Razor ページの概要'
author: rick-anderson
description: このチュートリアル シリーズでは、ASP.NET Core で Razor ページを使用する方法を示します。 モデルの作成、Razor ページのコードの生成、Entity Framework Core と SQL Server を使用したデータ アクセス、検索機能の追加、入力検証の追加、および移行を使用したモデルの更新の方法について説明します。
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d264ca4a605d8291e273a8f054c92e7eefa5548
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891157"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="83dad-104">チュートリアル: ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="83dad-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="83dad-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83dad-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="83dad-106">これは、シリーズの最初のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="83dad-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="83dad-107">[このシリーズ](xref:tutorials/razor-pages/index)では、ASP.NET Core の Razor Pages Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="83dad-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="83dad-108">シリーズの最後には、映画のデータベースを管理できるアプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="83dad-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="83dad-109">このチュートリアルでは、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="83dad-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="83dad-110">Razor Pages Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="83dad-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="83dad-111">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="83dad-111">Run the app.</span></span>
> * <span data-ttu-id="83dad-112">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="83dad-112">Examine the project files.</span></span>

<span data-ttu-id="83dad-113">このチュートリアルの最後には、以降のチュートリアルでビルドする作業用 Razor ページ Web アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="83dad-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="83dad-115">Razor ページ Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="83dad-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83dad-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83dad-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="83dad-117">Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="83dad-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="83dad-118">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="83dad-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="83dad-119">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="83dad-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="83dad-120">コードのコピーおよび貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="83dad-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="83dad-122">ドロップダウン リストで **[ASP.NET Core 2.2]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="83dad-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="83dad-124">次のスターター プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-124">The following starter project is created:</span></span>

  ![ソリューション エクスプローラー](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83dad-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83dad-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="83dad-127">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="83dad-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="83dad-128">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="83dad-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="83dad-129">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="83dad-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="83dad-130">`dotnet new` コマンド: *RazorPagesMovie* フォルダーに新しい Razor Pages プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="83dad-131">`code` コマンド: Visual Studio Code の新しいインスタンス内で *RazorPagesMovie* フォルダーが開かれます。</span><span class="sxs-lookup"><span data-stu-id="83dad-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="83dad-132">"**ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="83dad-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="83dad-133">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="83dad-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83dad-134">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="83dad-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="83dad-135">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="83dad-135">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="83dad-136">上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="83dad-137">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="83dad-137">Open the project</span></span>

<span data-ttu-id="83dad-138">Visual Studio から、**[ファイル]、[開く]** の順に選択し、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="83dad-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="83dad-139">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="83dad-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83dad-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83dad-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="83dad-141">Ctrl + F5 キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="83dad-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="83dad-142">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="83dad-143">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="83dad-144">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="83dad-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="83dad-145">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="83dad-146">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="83dad-147">アプリのホーム ページ上で、**[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="83dad-147">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="83dad-148">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="83dad-148">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="83dad-150">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="83dad-150">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="83dad-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83dad-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="83dad-153">**Ctrl + F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="83dad-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="83dad-154">Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動して、`http://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="83dad-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="83dad-155">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="83dad-156">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="83dad-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="83dad-157">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-157">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="83dad-158">アプリのホーム ページ上で、**[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="83dad-158">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="83dad-159">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="83dad-159">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="83dad-161">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="83dad-161">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="83dad-163">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="83dad-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="83dad-164">**Cmd - Opt - F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="83dad-164">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="83dad-165">Visual Studio は [Kestrel](xref:fundamentals/servers/kestrel) を開始し、ブラウザーを起動して、`http://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="83dad-165">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="83dad-166">アプリのホーム ページ上で、**[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="83dad-166">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="83dad-167">このアプリによって個人情報は追跡されません。しかし、欧州連合の[一般データ保護規則 (GDPR)](xref:security/gdpr) に準拠するための同意機能が必要な場合は、プロジェクト テンプレートにその機能が含められます。</span><span class="sxs-lookup"><span data-stu-id="83dad-167">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="83dad-169">次の図では、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="83dad-169">The following image shows the app after you give consent to tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="83dad-171">プロジェクト ファイルを確認する</span><span class="sxs-lookup"><span data-stu-id="83dad-171">Examine the project files</span></span>

<span data-ttu-id="83dad-172">以降のチュートリアルで使用するメイン プロジェクト フォルダーとファイルの概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="83dad-172">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="83dad-173">Pages フォルダー</span><span class="sxs-lookup"><span data-stu-id="83dad-173">Pages folder</span></span>

<span data-ttu-id="83dad-174">Razor ページとサポート ファイルが格納されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-174">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="83dad-175">各 Razor ページは、次のファイルのペアとなります。</span><span class="sxs-lookup"><span data-stu-id="83dad-175">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="83dad-176">*.cshtml* ファイル: HTML マークアップと、Razor 構文を使用した C# コードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-176">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="83dad-177">*.cshtml.cs* ファイル: ページ イベントを処理する C# コードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-177">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="83dad-178">サポート ファイルには、アンダー スコアで始まる名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="83dad-178">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="83dad-179">たとえば、*_Layout.cshtml* ファイルでは、すべてのページに共通の UI 要素が構成されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-179">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="83dad-180">このファイルでは、ページの上部に表示されるナビゲーション メニューと、ページの下部に表示される著作権の通知が設定されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-180">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="83dad-181">詳細については、「<xref:mvc/views/layout>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="83dad-181">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="83dad-182">wwwroot フォルダー</span><span class="sxs-lookup"><span data-stu-id="83dad-182">wwwroot folder</span></span>

<span data-ttu-id="83dad-183">HTML ファイル、JavaScript ファイル、CSS ファイルなどの静的ファイルが格納されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-183">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="83dad-184">詳細については、「<xref:fundamentals/static-files>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="83dad-184">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="83dad-185">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="83dad-185">appSettings.json</span></span>

<span data-ttu-id="83dad-186">接続文字列などの構成データが保存されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-186">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="83dad-187">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="83dad-187">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="83dad-188">Program.cs</span><span class="sxs-lookup"><span data-stu-id="83dad-188">Program.cs</span></span>

<span data-ttu-id="83dad-189">プログラムのエントリ ポイントが保存されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-189">Contains the entry point for the program.</span></span> <span data-ttu-id="83dad-190">詳細については、「<xref:fundamentals/host/web-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="83dad-190">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="83dad-191">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="83dad-191">Startup.cs</span></span>

<span data-ttu-id="83dad-192">cookie に対する同意が必要かどうかなど、アプリの動作を構成するコードが保存されます。</span><span class="sxs-lookup"><span data-stu-id="83dad-192">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="83dad-193">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="83dad-193">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83dad-194">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="83dad-194">Additional resources</span></span>

* [<span data-ttu-id="83dad-195">このチュートリアルの YouTube バージョン</span><span class="sxs-lookup"><span data-stu-id="83dad-195">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="83dad-196">次の手順</span><span class="sxs-lookup"><span data-stu-id="83dad-196">Next steps</span></span>

<span data-ttu-id="83dad-197">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="83dad-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="83dad-198">Razor ページ Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="83dad-198">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="83dad-199">アプリを実行する。</span><span class="sxs-lookup"><span data-stu-id="83dad-199">Ran the app.</span></span>
> * <span data-ttu-id="83dad-200">プロジェクト ファイルを確認する。</span><span class="sxs-lookup"><span data-stu-id="83dad-200">Examined the project files.</span></span>

<span data-ttu-id="83dad-201">このシリーズの次のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="83dad-201">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="83dad-202">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="83dad-202">Add a model</span></span>](xref:tutorials/razor-pages/model)
