---
title: Visual Studio for Mac を使用して ASP.NET Core Razor ページ アプリにモデルを追加する
author: rick-anderson
description: Visual Studio for Mac を使用し、ASP.NET Core で Razor ページ アプリにモデルを追加する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186759"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="244c1-103">Visual Studio for Mac を使用して ASP.NET Core Razor ページ アプリにモデルを追加する</span><span class="sxs-lookup"><span data-stu-id="244c1-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="244c1-104">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="244c1-104">Add a data model</span></span>

* <span data-ttu-id="244c1-105">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="244c1-106">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="244c1-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="244c1-107">*Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="244c1-108">**[新しいファイル]** ダイアログで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="244c1-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="244c1-109">左側のウィンドウで **[全般]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="244c1-110">中央ウィンドウで **[空のクラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="244c1-111">クラスに **Movie** という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="244c1-112">たとえば、`services.AddDbContext<MovieContext>(options =>` 行の `MovieContext` など、赤の波線を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="244c1-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="244c1-113">**[Quick Fix]\(クイック フィックス\)、[using RazorPagesMovie.Models;]\(RazorPagesMovie.Models; を使用\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="244c1-114">Visual studio によって using ステートメントが追加されます。</span><span class="sxs-lookup"><span data-stu-id="244c1-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="244c1-115">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="244c1-115">Build the project to verify you don't have any errors.</span></span>

![[作成] ページ](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="244c1-117">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="244c1-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="244c1-118">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="244c1-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="244c1-119">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) リンクをクリックして、使用するバージョン番号を取得します。</span><span class="sxs-lookup"><span data-stu-id="244c1-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="244c1-120">このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="244c1-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="244c1-121">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="244c1-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="244c1-122">*.csproj* ファイルを編集するには、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="244c1-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="244c1-123">**[ファイル]**、 > **[開く]** を選択して、*.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="244c1-124">**[オプション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-124">Select **Options**.</span></span>
* <span data-ttu-id="244c1-125">**[Open with]\(次で開く\)** を **[ソース コード エディター]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="244c1-125">Change **Open with** to **Source Code Editor**.</span></span>

![csproj ファイルの編集](model/csproj.png)

<span data-ttu-id="244c1-127">`Microsoft.EntityFrameworkCore.Tools.DotNet` ツール参照の 2 つ目の **\<ItemGroup>** への追加</span><span class="sxs-lookup"><span data-stu-id="244c1-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="244c1-128">執筆時点では、次のコードに示されているバージョン番号は正確です。</span><span class="sxs-lookup"><span data-stu-id="244c1-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="244c1-129">プロジェクトへの Pages/Movies ファイルの追加</span><span class="sxs-lookup"><span data-stu-id="244c1-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="244c1-130">Visual Studio で *Pages* フォルダーを右クリックし、**[追加]、[既存のフォルダーの追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="244c1-131">*Movies* フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="244c1-132">*[プロジェクトに含めるファイルを選びます]* ダイアログで、**[すべて含める]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="244c1-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="244c1-133">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="244c1-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="244c1-134">[前: はじめに](xref:tutorials/razor-pages-mac/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="244c1-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
