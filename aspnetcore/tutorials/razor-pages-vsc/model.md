---
title: Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する
author: rick-anderson
description: Visual Studio Code を使用し、ASP.NET Core で Razor ページ アプリにモデルを追加する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 20282b491162e9f35e40702655532a78edceb89a
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="57a27-103">Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する</span><span class="sxs-lookup"><span data-stu-id="57a27-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="57a27-104">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="57a27-104">Add a data model</span></span>

* <span data-ttu-id="57a27-105">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="57a27-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="57a27-106">*Movie.cs* という名前の *Models* フォルダーにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="57a27-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="57a27-107">*Models/Movie.cs* ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="57a27-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="57a27-108">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="57a27-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="57a27-109">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="57a27-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="57a27-110">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="57a27-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="57a27-111">このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="57a27-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="57a27-112">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="57a27-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="57a27-113">以下のように、*RazorPagesMovie.csproj* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="57a27-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="57a27-114">**[ファイル]**、**[ファイルを開く]** の順に選択してから、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="57a27-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="57a27-115">`Microsoft.EntityFrameworkCore.Tools.DotNet` 用のツール参照の 2 つ目の **\<ItemGroup >** への追加</span><span class="sxs-lookup"><span data-stu-id="57a27-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="57a27-116">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="57a27-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="57a27-117">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="57a27-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="57a27-118">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="57a27-118">Run the following command:</span></span>

<span data-ttu-id="57a27-119">**注: Windows で次のコマンドを実行します。MacOS および Linux の場合は、次のコマンドを参照してください。**</span><span class="sxs-lookup"><span data-stu-id="57a27-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="57a27-120">MacOS および Linux では、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="57a27-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="57a27-121">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="57a27-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="57a27-122">Visual Studio を終了し、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="57a27-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="57a27-123">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="57a27-123">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="57a27-124">[前: はじめに](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="57a27-124">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
