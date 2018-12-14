---
title: ASP.NET Core の Razor ページの概要
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: このチュートリアル シリーズでは、ASP.NET Core で Razor ページを使用する方法を示します。 モデルの作成、Razor ページのコードの生成、Entity Framework Core と SQL Server を使用したデータ アクセス、検索機能の追加、入力検証の追加、および移行を使用したモデルの更新の方法について説明します。
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861629"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="49a78-104">チュートリアル: ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="49a78-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="49a78-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49a78-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="49a78-106">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="49a78-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="49a78-107">このアプリでは、映画タイトルのデータベースを管理します。</span><span class="sxs-lookup"><span data-stu-id="49a78-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="49a78-108">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="49a78-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="49a78-109">Razor Pages Web アプリを作成する。</span><span class="sxs-lookup"><span data-stu-id="49a78-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="49a78-110">モデルを追加してスキャフォールディングする。</span><span class="sxs-lookup"><span data-stu-id="49a78-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="49a78-111">データベースを使用する。</span><span class="sxs-lookup"><span data-stu-id="49a78-111">Work with a database.</span></span>
> * <span data-ttu-id="49a78-112">検索と検証を追加する。</span><span class="sxs-lookup"><span data-stu-id="49a78-112">Add search and validation.</span></span>

<span data-ttu-id="49a78-113">最後には、映画タイトル項目を管理および表示できるアプリが完成します。</span><span class="sxs-lookup"><span data-stu-id="49a78-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="49a78-114">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="49a78-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="49a78-115">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="49a78-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="49a78-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a78-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="49a78-117">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="49a78-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="49a78-118">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="49a78-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="49a78-119">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="49a78-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="49a78-120">コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="49a78-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="49a78-121">![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="49a78-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="49a78-122">ドロップダウン リストで **[ASP.NET Core 2.2]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="49a78-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="49a78-124">次のスターター プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-124">The following starter project is created:</span></span>

  ![ソリューション エクスプローラー](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="49a78-126">**Ctrl + F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="49a78-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="49a78-127">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="49a78-128">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="49a78-129">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="49a78-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="49a78-130">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="49a78-131">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="49a78-132">上の図では、ポート番号は 5001 です。</span><span class="sxs-lookup"><span data-stu-id="49a78-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="49a78-133">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="49a78-134">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="49a78-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="49a78-135">多くの開発者は、ページを更新して変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="49a78-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="49a78-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="49a78-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="49a78-137">[統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。</span><span class="sxs-lookup"><span data-stu-id="49a78-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="49a78-138">ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="49a78-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="49a78-139">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="49a78-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="49a78-140">"**ビルドとデバッグに必要な資産が 'RazorPagesMovie' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、</span><span class="sxs-lookup"><span data-stu-id="49a78-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="49a78-141">**[はい]** を選択します</span><span class="sxs-lookup"><span data-stu-id="49a78-141">Select **Yes**</span></span>

  * <span data-ttu-id="49a78-142">`dotnet new webapp -o RazorPagesMovie`: *RazorPagesMovie*  フォルダーに新しい Razor Pages プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="49a78-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="49a78-143">`code -r RazorPagesMovie`: Visual Studio Code で *RazorPagesMovie.csproj* プロジェクト ファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="49a78-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="49a78-144">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="49a78-144">Launch the app</span></span>

* <span data-ttu-id="49a78-145">**Ctrl + F5** キーを押して、デバッガーなしで実行します。</span><span class="sxs-lookup"><span data-stu-id="49a78-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="49a78-146">Visual Studio Code で [Kestrel](xref:fundamentals/servers/kestrel) が開始され、ブラウザーが起動され、`http://localhost:5001` に移動されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="49a78-147">アドレス バーには、`example.com` などではなく、`localhost:port:5001` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="49a78-148">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="49a78-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="49a78-149">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="49a78-150">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="49a78-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="49a78-151">多くの開発者は、ページを更新して変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="49a78-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="49a78-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="49a78-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="49a78-153">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="49a78-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="49a78-154">上のコマンドでは [.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、Razor ページ プロジェクトを作成して実行します。</span><span class="sxs-lookup"><span data-stu-id="49a78-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="49a78-155">ブラウザーを開いて http://localhost:5000 にアクセスし、アプリケーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="49a78-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="49a78-156">プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="49a78-156">Open the project</span></span>

<span data-ttu-id="49a78-157">Ctrl + C キーを押して、アプリケーションをシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="49a78-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="49a78-158">Visual Studio から、**[ファイル]、[開く]** の順に選択し、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="49a78-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="49a78-159">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="49a78-159">Launch the app</span></span>

<span data-ttu-id="49a78-160">**[実行]、[デバッグなしで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="49a78-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="49a78-161">Visual Studio は [Kestrel](xref:fundamentals/servers/kestrel) を開始し、ブラウザーを起動して、`http://localhost:5001` に移動します。</span><span class="sxs-lookup"><span data-stu-id="49a78-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="49a78-162">**[同意する]** を選択し、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="49a78-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="49a78-163">このアプリでは個人情報は追跡されません。</span><span class="sxs-lookup"><span data-stu-id="49a78-163">This app doesn't track personal information.</span></span> <span data-ttu-id="49a78-164">テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="49a78-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="49a78-166">次の図は、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="49a78-166">The following image shows the app after accepting tracking:</span></span>

  ![ホームまたはインデックス ページ](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="49a78-168">プロジェクトのファイルとフォルダー</span><span class="sxs-lookup"><span data-stu-id="49a78-168">Project files and folders</span></span>

<span data-ttu-id="49a78-169">次の表に、プロジェクトのファイルとフォルダーをリストします。</span><span class="sxs-lookup"><span data-stu-id="49a78-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="49a78-170">チュートリアルのこの段階では、*Startup.cs* ファイルを理解することが最も重要です。</span><span class="sxs-lookup"><span data-stu-id="49a78-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="49a78-171">以下に示す各リンクをレビューする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="49a78-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="49a78-172">リンクは、プロジェクトのファイルまたはフォルダーについて詳しい情報が必要な場合の参照として提供されているものです。</span><span class="sxs-lookup"><span data-stu-id="49a78-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="49a78-173">ファイルまたはフォルダー</span><span class="sxs-lookup"><span data-stu-id="49a78-173">File or folder</span></span>              | <span data-ttu-id="49a78-174">目的</span><span class="sxs-lookup"><span data-stu-id="49a78-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="49a78-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="49a78-175">*wwwroot*</span></span> | <span data-ttu-id="49a78-176">静的ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49a78-176">Contains static files.</span></span> <span data-ttu-id="49a78-177">[静的ファイル](xref:fundamentals/static-files)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="49a78-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="49a78-178">*ページ*</span><span class="sxs-lookup"><span data-stu-id="49a78-178">*Pages*</span></span> | <span data-ttu-id="49a78-179">[Razor ページ](xref:razor-pages/index)のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="49a78-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="49a78-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="49a78-180">*appsettings.json*</span></span> | [<span data-ttu-id="49a78-181">構成</span><span class="sxs-lookup"><span data-stu-id="49a78-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="49a78-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="49a78-182">*Program.cs*</span></span> | <span data-ttu-id="49a78-183">ASP.NET Core アプリを[ホスト](xref:fundamentals/host/index)します。</span><span class="sxs-lookup"><span data-stu-id="49a78-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="49a78-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="49a78-184">*Startup.cs*</span></span> | <span data-ttu-id="49a78-185">サービスと要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="49a78-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="49a78-186">[スタートアップ](xref:fundamentals/startup)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="49a78-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="49a78-187">Pages フォルダー</span><span class="sxs-lookup"><span data-stu-id="49a78-187">The Pages folder</span></span>

<span data-ttu-id="49a78-188">*_Layout.cshtml* ファイルは共通の HTML 要素 (スクリプトとスタイルシート) を含み、アプリケーションのレイアウトを設定します。</span><span class="sxs-lookup"><span data-stu-id="49a78-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="49a78-189">たとえば、**[RazorPagesMovie]**、**[ホーム]**、または **[プライバシー]** をクリックすると、同じ要素が表示されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="49a78-190">共通の要素には、ウィンドウの下部のヘッダーと上部のナビゲーション メニューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="49a78-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="49a78-191">詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="49a78-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="49a78-192">*_ViewImports.cshtml* ファイルには、各 Razor ページにインポートされた Razor ディレクティブが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49a78-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="49a78-193">詳細については、「[Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)」 (共有ディレクティブのインポート) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="49a78-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="49a78-194">*_ViewStart.cshtml* では、*_Layout.cshtml* ファイルを使用するように Razor ページの `Layout` プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="49a78-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="49a78-195">詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="49a78-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="49a78-196">*_ValidationScriptsPartial.cshtml* ファイルでは、[jQuery](https://jquery.com/) 検証スクリプトへの参照が提供されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="49a78-197">チュートリアルの後半で `Create` および `Edit` ページを追加するときは、*_ValidationScriptsPartial.cshtml* ファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="49a78-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="49a78-198">次の目的で `Index`、`Error`、`Privacy` ページが用意されています。</span><span class="sxs-lookup"><span data-stu-id="49a78-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="49a78-199">`Index`: アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="49a78-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="49a78-200">`Error`: エラー情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="49a78-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="49a78-201">`Privacy`: サイトのプライバシー ポリシーに関する詳細情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="49a78-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="49a78-202">このチュートリアルでは、前のページは使用されません。</span><span class="sxs-lookup"><span data-stu-id="49a78-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="49a78-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49a78-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="49a78-204">F7 キーを使用して Razor ページと PageModel を切り替える</span><span class="sxs-lookup"><span data-stu-id="49a78-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="49a78-205">F7 キーを押すと、Razor ページ (*\*.cshtml* ファイル) と C# ファイル (*\*.cshtml.cs*) が切り替えられます。</span><span class="sxs-lookup"><span data-stu-id="49a78-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="49a78-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="49a78-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="49a78-207">規約により、Razor Page (*\*.cshtml* ファイル) と関連する `PageModel` のルート ファイル名は同じです。</span><span class="sxs-lookup"><span data-stu-id="49a78-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="49a78-208">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="49a78-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="49a78-209">規約により、Razor Page (*\*.cshtml* ファイル) と関連する `PageModel` のルート ファイル名は同じです。</span><span class="sxs-lookup"><span data-stu-id="49a78-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="49a78-210">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="49a78-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)