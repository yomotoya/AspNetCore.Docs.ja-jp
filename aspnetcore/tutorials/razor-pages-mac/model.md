---
title: "Visual Studio for Mac を使用する Razor ページ アプリへのモデルの追加"
author: rick-anderson
description: "Visual Studio for Mac を使用する ASP.NET Core での Razor ページ アプリへのモデルの追加"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC,モデル"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: b234eb93fbca1f4c83712990712b86e9941968fd
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="39e25-104">Visual Studio for Mac を使用する ASP.NET Core での Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="39e25-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="39e25-105">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="39e25-105">Add a data model</span></span>

* <span data-ttu-id="39e25-106">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-106">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="39e25-107">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="39e25-107">Name the folder *Models*.</span></span>
* <span data-ttu-id="39e25-108">*Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-108">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="39e25-109">**[新しいファイル]** ダイアログで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="39e25-109">In the **New File** dialog:</span></span>

  * <span data-ttu-id="39e25-110">左側のウィンドウで **[全般]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-110">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="39e25-111">中央ウィンドウで **[空のクラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-111">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="39e25-112">クラスに **Movie** という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-112">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="39e25-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="39e25-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="39e25-114">たとえば、`services.AddDbContext<MovieContext>(options =>` 行の `MovieContext` など、赤の波線を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="39e25-114">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="39e25-115">**[Quick Fix]\(クイック フィックス\)、[using RazorPagesMovie.Models;](\RazorPagesMovie.Models; を使用\)** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-115">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="39e25-116">Visual studio によって using ステートメントが追加されます。</span><span class="sxs-lookup"><span data-stu-id="39e25-116">Visual studio adds the using statement.</span></span>

<span data-ttu-id="39e25-117">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="39e25-117">Build the project to verify you don't have any errors.</span></span>

![[作成] ページ](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="39e25-119">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="39e25-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="39e25-120">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="39e25-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="39e25-121">このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="39e25-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="39e25-122">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="39e25-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="39e25-123">*.csproj* ファイルを編集するには、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="39e25-123">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="39e25-124">**[ファイル]、[開く]** を選択して、*.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-124">Select **File > Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="39e25-125">**[オプション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-125">Select **Options**.</span></span>
* <span data-ttu-id="39e25-126">**[Open with](\次で開く\)** を **[ソース コード エディター]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="39e25-126">Change **Open with** to **Source Code Editor**.</span></span>

![csproj ファイルの編集](model/csproj.png)

<span data-ttu-id="39e25-128">次は、更新された *csproj* ファイルのコードです。</span><span class="sxs-lookup"><span data-stu-id="39e25-128">The following code shows the updated *csproj* file.</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="39e25-129">プロジェクトへの Pages/Movies ファイルの追加</span><span class="sxs-lookup"><span data-stu-id="39e25-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="39e25-130">Visual Studio で *Pages* フォルダーを右クリックし、**[追加]、[既存のフォルダーの追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="39e25-131">*Movies* フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="39e25-132">*[Chosse files to include in the project](\プロジェクトに含めるファイルを選択する\)* ダイアログで、**[Include All](\すべて含める\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="39e25-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="39e25-133">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="39e25-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="39e25-134">[前: はじめに](xref:tutorials/razor-pages-mac/razor-pages-start)
[次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="39e25-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
