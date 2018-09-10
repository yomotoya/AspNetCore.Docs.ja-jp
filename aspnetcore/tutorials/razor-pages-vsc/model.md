---
title: Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する
author: rick-anderson
description: Visual Studio Code を使用し、ASP.NET Core で Razor ページ アプリにモデルを追加する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: b891b921baf1fe6d167c7bfb8b4c5278ce9fe9f5
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055865"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="90cd5-103">Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する</span><span class="sxs-lookup"><span data-stu-id="90cd5-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="90cd5-104">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="90cd5-104">Add a data model</span></span>

* <span data-ttu-id="90cd5-105">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="90cd5-106">*Movie.cs* という名前の *Models* フォルダーにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="90cd5-107">*Models/Movie.cs* ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="90cd5-108">SQLite 用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="90cd5-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="90cd5-109">コマンド ラインから、次の .NET Core CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="90cd5-110">*Startup.cs* の先頭に次の `using` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="90cd5-111">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-111">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="90cd5-112">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="90cd5-112">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="90cd5-113">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="90cd5-113">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="90cd5-114">このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-114">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="90cd5-115">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="90cd5-115">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="90cd5-116">以下のように、*RazorPagesMovie.csproj* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-116">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="90cd5-117">**[ファイル]**、 > **[ファイルを開く]** の順に選択してから、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-117">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="90cd5-118">`Microsoft.EntityFrameworkCore.Tools.DotNet` 用のツール参照の 2 つ目の **\<ItemGroup >** への追加</span><span class="sxs-lookup"><span data-stu-id="90cd5-118">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="90cd5-119">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="90cd5-119">Scaffold the Movie model</span></span>

* <span data-ttu-id="90cd5-120">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="90cd5-120">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="90cd5-121">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-121">Run the following command:</span></span>

<span data-ttu-id="90cd5-122">**注: Windows で次のコマンドを実行します。MacOS および Linux の場合は、次のコマンドを参照してください。**</span><span class="sxs-lookup"><span data-stu-id="90cd5-122">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="90cd5-123">MacOS および Linux では、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-123">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="90cd5-124">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="90cd5-124">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="90cd5-125">Visual Studio を終了し、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-125">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="90cd5-126">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="90cd5-126">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="90cd5-127">[前: はじめに](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="90cd5-127">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
