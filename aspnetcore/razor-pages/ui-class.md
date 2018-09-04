---
title: ASP.NET Core のクラス ライブラリの再利用可能 Razor UI
author: Rick-Anderson
description: クラス ライブラリで再利用可能 Razor UI を作成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2018
ms.locfileid: "42909650"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="bd022-103">ASP.NET Core で Razor クラス ライブラリ プロジェクトを使用し、再利用可能 UI を作成します。</span><span class="sxs-lookup"><span data-stu-id="bd022-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="bd022-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bd022-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bd022-105">Razor ビュー、ページ、コントローラー、ページ モデル、[ビュー コンポーネント](xref:mvc/views/view-components)、データ モデルは、Razor クラス ライブラリ (RCL) に組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="bd022-105">Razor views, pages, controllers, page models, [View components](xref:mvc/views/view-components), and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="bd022-106">RCL はパッケージ化し、再利用できます。</span><span class="sxs-lookup"><span data-stu-id="bd022-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="bd022-107">アプリケーションには RCL を含めることができます。また、それに含まれるビューやページをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="bd022-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="bd022-108">Web アプリと RCL の両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="bd022-109">この機能が必要です。 [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="bd022-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="bd022-110">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bd022-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="bd022-111">Razor UI を含むクラス ライブラリを作成する</span><span class="sxs-lookup"><span data-stu-id="bd022-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bd022-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd022-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bd022-113">Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bd022-114">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bd022-115">ライブラリに名前を付け ("RazorClassLib" など)、**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-115">Name the library (for example, "RazorClassLib") > **OK**.</span></span> <span data-ttu-id="bd022-116">生成されたビュー ライブラリとファイル名の競合を避けるため、ライブラリ名の末尾が `.Views` ではないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-116">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>
* <span data-ttu-id="bd022-117">**ASP.NET Core 2.1** 以降が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-117">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bd022-118">**[Razor クラス ライブラリ]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-118">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="bd022-119">Razor クラス ライブラリでは、次のプロジェクト ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="bd022-119">A Razor Class Library has the following project file:</span></span>

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bd022-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bd022-120">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bd022-121">コマンドラインから `dotnet new razorclasslib` を実行します。</span><span class="sxs-lookup"><span data-stu-id="bd022-121">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="bd022-122">例えば:</span><span class="sxs-lookup"><span data-stu-id="bd022-122">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="bd022-123">詳細については、「[dotnet new](/dotnet/core/tools/dotnet-new)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd022-123">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span> <span data-ttu-id="bd022-124">生成されたビュー ライブラリとファイル名の競合を避けるため、ライブラリ名の末尾が `.Views` ではないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-124">To avoid a file name collision with the generated view library, ensure the library name doesn't end in `.Views`.</span></span>

------
<span data-ttu-id="bd022-125">Razor ファイルを RCL に追加します。</span><span class="sxs-lookup"><span data-stu-id="bd022-125">Add Razor files to the RCL.</span></span>

<span data-ttu-id="bd022-126">ASP.NET Core テンプレートは、RCL コンテンツが前提としています。、*領域*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="bd022-126">The ASP.NET Core templates assume the RCL content is in the *Areas* folder.</span></span> <span data-ttu-id="bd022-127">参照してください[RCL ページ レイアウト](#afs)でコンテンツを公開する RCL を作成する`~/Pages`なく`~/Areas/Pages`します。</span><span class="sxs-lookup"><span data-stu-id="bd022-127">See [RCL Pages layout](#afs) to create a RCL that exposes content in `~/Pages` rather than `~/Areas/Pages`.</span></span>

## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="bd022-128">Razor クラス ライブラリ コンテンツを参照する</span><span class="sxs-lookup"><span data-stu-id="bd022-128">Referencing Razor Class Library content</span></span>

<span data-ttu-id="bd022-129">RCL は次によって参照できます。</span><span class="sxs-lookup"><span data-stu-id="bd022-129">The RCL can be referenced by:</span></span>

* <span data-ttu-id="bd022-130">NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="bd022-130">NuGet package.</span></span> <span data-ttu-id="bd022-131">「[Creating NuGet packages](/nuget/create-packages/creating-a-package)」 (NuGet パッケージの作成)、「[dotnet add package](/dotnet/core/tools/dotnet-add-package)」、「[Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)」 (NuGet パッケージの作成と公開) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd022-131">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="bd022-132">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="bd022-132">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="bd022-133">「[dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd022-133">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="bd022-134">チュートリアル: Razor クラス ライブラリ プロジェクトを作成し、Razor ページ プロジェクトから使用する</span><span class="sxs-lookup"><span data-stu-id="bd022-134">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="bd022-135">作成しなくても、[完全なプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)をダウンロードしてテストできます。</span><span class="sxs-lookup"><span data-stu-id="bd022-135">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="bd022-136">サンプル ダウンロードには、プロジェクトのテストを簡単にする追加のコードやリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bd022-136">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="bd022-137">GitHub の問題に関するフィードバックは[こちら](https://github.com/aspnet/Docs/issues/6098)で扱っています。ダウンロード サンプルと段階的指示の違いについてコメントを投稿できます。</span><span class="sxs-lookup"><span data-stu-id="bd022-137">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="bd022-138">ダウンロード アプリをテストする</span><span class="sxs-lookup"><span data-stu-id="bd022-138">Test the download app</span></span>

<span data-ttu-id="bd022-139">完全なアプリをダウンロードしておらず、チュートリアル プロジェクトを作成する場合、[次のセクション](#create-a-razor-class-library)に進んでください。</span><span class="sxs-lookup"><span data-stu-id="bd022-139">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bd022-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd022-140">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bd022-141">Visual Studio で *.sln* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="bd022-141">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="bd022-142">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bd022-142">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bd022-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bd022-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bd022-144">*cli* ディレクトリのコマンド プロンプトから、RCL と Web アプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="bd022-144">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="bd022-145">*WebApp1* ディレクトリに移動し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bd022-145">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="bd022-146">[テスト WebApp1](#test) の指示に従ってください。</span><span class="sxs-lookup"><span data-stu-id="bd022-146">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="bd022-147">Razor クラス ライブラリの作成</span><span class="sxs-lookup"><span data-stu-id="bd022-147">Create a Razor Class Library</span></span>

<span data-ttu-id="bd022-148">このセクションでは、Razor クラス ライブラリ (RCL) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-148">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="bd022-149">Razor ファイルが RCL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-149">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bd022-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd022-150">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bd022-151">RCL プロジェクトの作成:</span><span class="sxs-lookup"><span data-stu-id="bd022-151">Create the RCL project:</span></span>

* <span data-ttu-id="bd022-152">Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-152">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="bd022-153">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-153">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bd022-154">アプリの名前を付けます**RazorUIClassLib** > **OK**します。</span><span class="sxs-lookup"><span data-stu-id="bd022-154">Name the app **RazorUIClassLib** > **OK**.</span></span>
* <span data-ttu-id="bd022-155">**ASP.NET Core 2.1** 以降が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-155">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bd022-156">**[Razor クラス ライブラリ]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-156">Select **Razor Class Library** > **OK**.</span></span>
* <span data-ttu-id="bd022-157">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* という名前が付いた Razor 部分ビュー ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="bd022-157">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bd022-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bd022-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bd022-159">コマンド ラインから次を実行します。</span><span class="sxs-lookup"><span data-stu-id="bd022-159">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="bd022-160">上のコマンドでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="bd022-160">The preceding commands:</span></span>

* <span data-ttu-id="bd022-161">`RazorUIClassLib` Razor クラス ライブラリ (RCL) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-161">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="bd022-162">Razor _Message ページが作成され、RCL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-162">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="bd022-163">`-np` パラメーターによって、`PageModel` なしでページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-163">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="bd022-164">[viewstart](xref:mvc/views/layout#running-code-before-each-view) ファイルが作成され、RCL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-164">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="bd022-165">(次のセクションで追加される) Razor ページ プロジェクトのレイアウトを使用するには、viewstart ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="bd022-165">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

------

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="bd022-166">Razor のファイルとフォルダーをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="bd022-166">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="bd022-167">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* のマークアップを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="bd022-167">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="bd022-168">*RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* のマークアップを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="bd022-168">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="bd022-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` は部分ビュー (`<partial name="_Message" />`) を使用するために必要です。</span><span class="sxs-lookup"><span data-stu-id="bd022-169">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="bd022-170">`@addTagHelper` ディレクティブを含める代わりに、*_ViewImports.cshtml* ファイルを追加できます。</span><span class="sxs-lookup"><span data-stu-id="bd022-170">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="bd022-171">例えば:</span><span class="sxs-lookup"><span data-stu-id="bd022-171">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="bd022-172">ビューのインポートについては、「[共有ディレクティブのインポート](xref:mvc/views/layout#importing-shared-directives)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd022-172">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="bd022-173">クラス ライブラリをビルドし、コンパイラ エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-173">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="bd022-174">ビルド出力には、*RazorUIClassLib.dll* と *RazorUIClassLib.Views.dll* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bd022-174">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="bd022-175">*RazorUIClassLib.Views.dll* には、コンパイル済みの Razor コンテンツが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bd022-175">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="bd022-176">Razor ページ プロジェクトから Razor UI ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="bd022-176">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bd022-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd022-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bd022-178">Razor ページ Web アプリの作成:</span><span class="sxs-lookup"><span data-stu-id="bd022-178">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="bd022-179">**ソリューション エクスプローラー**で、ソリューションを右クリックし、**[追加]**、**[新しいプロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-179">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="bd022-180">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-180">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="bd022-181">アプリに **WebApp1** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="bd022-181">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="bd022-182">**ASP.NET Core 2.1** 以降が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-182">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="bd022-183">**[Web アプリケーション]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-183">Select **Web Application** > **OK**.</span></span>

* <span data-ttu-id="bd022-184">**ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-184">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="bd022-185">**ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[ビルド依存関係]**、**[プロジェクトの依存関係]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-185">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="bd022-186">**WebApp1** の依存関係として **RazorUIClassLib** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-186">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="bd022-187">**ソリューション エクスプローラー**で、**WebApp1** を右クリックし、**[追加]**、**[参照]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-187">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="bd022-188">**[参照マネージャー]** ダイアログで、**[RazorUIClassLib]**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="bd022-188">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="bd022-189">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="bd022-189">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bd022-190">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bd022-190">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bd022-191">Razor ページ Web アプリと、Razor ページ アプリと Razor クラス ライブラリを含むソリューション ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="bd022-191">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

<span data-ttu-id="bd022-192">Web アプリをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="bd022-192">Build and run the web app:</span></span>

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="bd022-193">テスト WebApp1</span><span class="sxs-lookup"><span data-stu-id="bd022-193">Test WebApp1</span></span>

<span data-ttu-id="bd022-194">Razor UI クラス ライブラリが使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-194">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="bd022-195">`/MyFeature/Page1` を参照します。</span><span class="sxs-lookup"><span data-stu-id="bd022-195">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="bd022-196">ビュー、部分ビュー、ページのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="bd022-196">Override views, partial views, and pages</span></span>

<span data-ttu-id="bd022-197">Web アプリと Razor クラス ライブラリの両方にビュー、部分ビュー、Razor ページがあるとき、Web アプリの Razor マークアップ (*.cshtml* ファイル) が優先されます。</span><span class="sxs-lookup"><span data-stu-id="bd022-197">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="bd022-198">たとえば、*WebApp1/Areas/MyFeature/Pages/Page1.cshtml* を WebApp1 に追加すると、WebApp1 の Page1 は、Razor クラス ライブラリの Page1 に優先します。</span><span class="sxs-lookup"><span data-stu-id="bd022-198">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="bd022-199">サンプル ダウンロードの *WebApp1/Areas/MyFeature2* の名前を *WebApp1/Areas/MyFeature* に変更し、優先設定をテストします。</span><span class="sxs-lookup"><span data-stu-id="bd022-199">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="bd022-200">*RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分ビューを *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml* ビューにコピーします。</span><span class="sxs-lookup"><span data-stu-id="bd022-200">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="bd022-201">新しい場所を示すようにマークアップを更新します。</span><span class="sxs-lookup"><span data-stu-id="bd022-201">Update the markup to indicate the new location.</span></span> <span data-ttu-id="bd022-202">アプリをビルドして実行し、アプリの部分ビューが使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bd022-202">Build and run the app to verify the app's version of the partial is being used.</span></span>

<a name="afs"></a>

### <a name="rcl-pages-layout"></a><span data-ttu-id="bd022-203">RCL ページ レイアウト</span><span class="sxs-lookup"><span data-stu-id="bd022-203">RCL Pages layout</span></span>

<span data-ttu-id="bd022-204">RCL コンテンツは、web アプリのページのフォルダーの一部である場合と同様に参照するには、次のファイル構造で RCL プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bd022-204">To reference RCL content as though it is part of the web app's Pages folder, create the RCL project with the following file structure:</span></span>

* <span data-ttu-id="bd022-205">*RazorUIClassLib/ページ*</span><span class="sxs-lookup"><span data-stu-id="bd022-205">*RazorUIClassLib/Pages*</span></span>
* <span data-ttu-id="bd022-206">*RazorUIClassLib/ページ/共有*</span><span class="sxs-lookup"><span data-stu-id="bd022-206">*RazorUIClassLib/Pages/Shared*</span></span>

<span data-ttu-id="bd022-207">たとえば、 *RazorUIClassLib/ページ/共有*2 つの部分的なファイルが含まれています *_Header.cshtml*と *_Footer.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="bd022-207">Suppose *RazorUIClassLib/Pages/Shared* contains two partial files, *_Header.cshtml* and *_Footer.cshtml*.</span></span> <span data-ttu-id="bd022-208"><partial>タグに追加できる *_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bd022-208">The <partial> tags could be added to *_Layout.cshtml* file:</span></span> 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```
