---
title: ASP.NET Core のクラス ライブラリの再利用可能 Razor UI
author: Rick-Anderson
description: クラス ライブラリで再利用可能 Razor UI を作成する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="bbfbe-103">ASP.NET Core で Razor クラス ライブラリ プロジェクトを使用し、再利用可能 UI を作成します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="bbfbe-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bbfbe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bbfbe-105">Razor ビュー、ページ、コントローラー、ページ モデル、データ モデルは、Razor クラス Library (RCL) に組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library(RCL).</span></span> <span data-ttu-id="bbfbe-106">RCL はパッケージ化し、再利用できます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-106">The RCL can be and packaged and reused.</span></span> <span data-ttu-id="bbfbe-107">アプリケーションには RCL を含めることができます。また、それに含まれるビューやページをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="bbfbe-108">Web アプリと RCL の両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="bbfbe-109">この機能には [!INCLUDE[](~/includes/2.1-SDK.md)] が必要です。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="bbfbe-110">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="bbfbe-111">Razor UI を含むクラス ライブラリを作成する</span><span class="sxs-lookup"><span data-stu-id="bbfbe-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbfbe-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbfbe-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbfbe-113">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bbfbe-114">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bbfbe-115">**ASP.NET Core 2.1** 以降が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bbfbe-116">**[Razor クラス ライブラリ]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bbfbe-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbfbe-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bbfbe-118">コマンドラインから `dotnet new razorclasslib` を実行します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="bbfbe-119">例:</span><span class="sxs-lookup"><span data-stu-id="bbfbe-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="bbfbe-120">詳細については、「[dotnet new](/dotnet/core/tools/dotnet-new)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="bbfbe-121">Razor ファイルを RCL に追加します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="bbfbe-122">RCL コンテンツは *[区分]* フォルダーに入れることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="bbfbe-123">Razor クラス ライブラリ コンテンツを参照する</span><span class="sxs-lookup"><span data-stu-id="bbfbe-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="bbfbe-124">RCL は次によって参照できます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="bbfbe-125">NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-125">NuGet package.</span></span> <span data-ttu-id="bbfbe-126">「[Creating NuGet packages](/nuget/create-packages/creating-a-package)」 (NuGet パッケージの作成)、「[dotnet add package](/dotnet/core/tools/dotnet-add-package)」、「[Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)」 (NuGet パッケージの作成と公開) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="bbfbe-127">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bbfbe-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="bbfbe-128">「[dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="bbfbe-129">RCL の部分ファイル アクセス</span><span class="sxs-lookup"><span data-stu-id="bbfbe-129">Partial files access in the RCL</span></span>

<span data-ttu-id="bbfbe-130">RCL の外のコンテンツについては、ASP.NET Core ランタイムは RCL の部分ファイルを検索しません。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="bbfbe-131">たとえば、サンプル ダウンロードでは、*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分ビューは *WebApp1\Pages\About.cshtml* で参照**できません**。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="bbfbe-132">ただし、RCL *RazorUIClassLib/* のページは *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* にアクセス**できます**。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="bbfbe-133">チュートリアル: Razor クラス ライブラリ プロジェクトを作成し、Razor ページ プロジェクトから使用する</span><span class="sxs-lookup"><span data-stu-id="bbfbe-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="bbfbe-134">作成しなくても、[完全なプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples)をダウンロードしてテストできます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="bbfbe-135">サンプル ダウンロードには、プロジェクトのテストを簡単にする追加のコードやリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="bbfbe-136">GitHub の問題に関するフィードバックは[こちら](https://github.com/aspnet/Docs/issues/6098)で扱っています。ダウンロード サンプルと段階的指示の違いについてコメントを投稿できます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="bbfbe-137">ダウンロード アプリをテストする</span><span class="sxs-lookup"><span data-stu-id="bbfbe-137">Test the download app</span></span>

<span data-ttu-id="bbfbe-138">完全なアプリをダウンロードしておらず、チュートリアル プロジェクトを作成する場合、[次のセクション](#create-a-razor-class-library)に進んでください。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbfbe-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbfbe-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bbfbe-140">Visual Studio で *.sln* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="bbfbe-141">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bbfbe-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbfbe-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bbfbe-143">*cli* ディレクトリのコマンド プロンプトから、RCL と Web アプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="bbfbe-144">*WebApp1* ディレクトリに移動し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="bbfbe-145">[テスト WebApp1](#test) の指示に従ってください。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="bbfbe-146">Razor クラス ライブラリの作成</span><span class="sxs-lookup"><span data-stu-id="bbfbe-146">Create a Razor Class Library</span></span>

<span data-ttu-id="bbfbe-147">このセクションでは、Razor クラス ライブラリ (RCL) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="bbfbe-148">Razor ファイルが RCL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbfbe-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbfbe-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bbfbe-150">RCL プロジェクトの作成:</span><span class="sxs-lookup"><span data-stu-id="bbfbe-150">Create the RCL project:</span></span>

* <span data-ttu-id="bbfbe-151">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bbfbe-152">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bbfbe-153">アプリに **RazorUIClassLib** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="bbfbe-154">**ASP.NET Core 2.1** 以降が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bbfbe-155">**[Razor クラス ライブラリ]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="bbfbe-156">Razor ページ Web アプリの作成:</span><span class="sxs-lookup"><span data-stu-id="bbfbe-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="bbfbe-157">**ソリューション エクスプローラー**で、ソリューションを右クリックし、**[追加]**、**[新しいプロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="bbfbe-158">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bbfbe-159">アプリに **WebApp1** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="bbfbe-160">**ASP.NET Core 2.1** 以降が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bbfbe-161">**[Web アプリケーション]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="bbfbe-162">Razor のファイルとフォルダーをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="bbfbe-163">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* という名前が付いた Razor 部分ビュー ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="bbfbe-164">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* のマークアップを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="bbfbe-165">WebApp1 プロジェクトから *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml* に *_ViewStart.cshtml* ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="bbfbe-166">Razor ページ プロジェクトのレイアウトを使用するには、[viewstart](xref:mvc/views/layout#running-code-before-each-view) ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bbfbe-167">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbfbe-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bbfbe-168">コマンド ラインから次を実行します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="bbfbe-169">上のコマンドでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-169">The preceding commands:</span></span>

* <span data-ttu-id="bbfbe-170">`RazorUIClassLib` Razor クラス ライブラリ (RCL) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="bbfbe-171">Razor _Message ページが作成され、RCL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="bbfbe-172">`-np` パラメーターによって、`PageModel` なしでページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="bbfbe-173">[viewstart](xref:mvc/views/layout#running-code-before-each-view) ファイルが作成され、RCL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="bbfbe-174">(次のセクションで追加される) Razor ページ プロジェクトのレイアウトを使用するには、viewstart ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="bbfbe-175">Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="bbfbe-176">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* のマークアップを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="bbfbe-177">*RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* のマークアップを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="bbfbe-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` は部分ビュー (`<partial name="_Message" />`) を使用するために必要です。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="bbfbe-179">`@addTagHelper` ディレクティブを含める代わりに、*_ViewImports.cshtml* ファイルを追加できます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="bbfbe-180">例:</span><span class="sxs-lookup"><span data-stu-id="bbfbe-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="bbfbe-181">ビューのインポートについては、「[共有ディレクティブのインポート](xref:mvc/views/layout#importing-shared-directives)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="bbfbe-182">クラス ライブラリをビルドし、コンパイラ エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="bbfbe-183">ビルド出力には、*RazorUIClassLib.dll* と *RazorUIClassLib.Views.dll* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="bbfbe-184">*RazorUIClassLib.Views.dll* には、コンパイル済みの Razor コンテンツが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="bbfbe-185">Razor ページ プロジェクトから Razor UI ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bbfbe-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbfbe-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bbfbe-187">**ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="bbfbe-188">**ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[ビルド依存関係]**、**[プロジェクトの依存関係]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="bbfbe-189">**WebApp1** の依存関係として **RazorUIClassLib** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="bbfbe-190">**ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[追加]**、**[参照]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="bbfbe-191">**[参照マネージャー]** ダイアログで、**[RazorUIClassLib]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="bbfbe-192">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bbfbe-193">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bbfbe-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bbfbe-194">Razor ページ Web アプリと、Razor ページ アプリと Razor クラス ライブラリを含むソリューション ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="bbfbe-195">Web アプリをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="bbfbe-196">テスト WebApp1</span><span class="sxs-lookup"><span data-stu-id="bbfbe-196">Test WebApp1</span></span>

<span data-ttu-id="bbfbe-197">Razor UI クラス ライブラリが使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="bbfbe-198">`/MyFeature/Page1` を参照します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="bbfbe-199">ビュー、部分ビュー、ページのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="bbfbe-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="bbfbe-200">Web アプリと Razor クラス ライブラリの両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="bbfbe-201">たとえば、*WebApp1/Areas/MyFeature/Pages/Page1.cshtml* を WebApp1 に追加すると、WebApp1 の Page1 は、Razor クラス ライブラリの Page1 に優先します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="bbfbe-202">サンプル ダウンロードの *WebApp1/Areas/MyFeature2* の名前を *WebApp1/Areas/MyFeature* に変更し、優先設定をテストします。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="bbfbe-203">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分ビューを *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml* ビューにコピーします。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="bbfbe-204">新しい場所を示すようにマークアップを更新します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="bbfbe-205">アプリをビルドして実行し、アプリの部分ビューが使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbfbe-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
