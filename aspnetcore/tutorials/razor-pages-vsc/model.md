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
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 39b069f8c81ca9460abc33b32b0bcc27118939cb
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="0752f-104">Visual Studio for Mac を使用する ASP.NET Core での Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="0752f-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="0752f-105">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="0752f-105">Add a data model</span></span>

* <span data-ttu-id="0752f-106">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="0752f-106">Add a folder named *Models*.</span></span>
* <span data-ttu-id="0752f-107">*Movie.cs* という名前の *Models* フォルダーにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="0752f-107">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="0752f-108">*Models/Movie.cs* ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0752f-108">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="0752f-109">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="0752f-109">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="0752f-110">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="0752f-110">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="0752f-111">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="0752f-111">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="0752f-112">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="0752f-112">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="0752f-113">このパッケージをインストールするには、*.csproj* ファイルの `DotNetCliToolReference` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="0752f-113">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="0752f-114">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="0752f-114">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="0752f-115">以下のように、*RazorPagesMovie.csproj* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="0752f-115">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="0752f-116">**[ファイル]、[ファイルを開く]** の順に選択してから、*RazorPagesMovie.csproj* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="0752f-116">Select **File > Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="0752f-117">次のコードで強調表示されているように `<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />` を追加します。</span><span class="sxs-lookup"><span data-stu-id="0752f-117">Add `<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />` as highlighted in the following code:</span></span>

<span data-ttu-id="0752f-118">[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)] [!INCLUDE[model 3](../../includes/RP/model3.md)]</span><span class="sxs-lookup"><span data-stu-id="0752f-118">[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)] [!INCLUDE[model 3](../../includes/RP/model3.md)]</span></span>

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="0752f-119">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="0752f-119">Scaffold the Movie model</span></span>

* <span data-ttu-id="0752f-120">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="0752f-120">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0752f-121">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0752f-121">Run the following command:</span></span>

<span data-ttu-id="0752f-122">**注: Windows で次のコマンドを実行します。MacOS および Linux の場合は、次のコマンドを参照してください。**</span><span class="sxs-lookup"><span data-stu-id="0752f-122">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="0752f-123">MacOS および Linux では、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0752f-123">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="0752f-124">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="0752f-124">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="0752f-125">Visual Studio を終了し、コマンドを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="0752f-125">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="0752f-126"> 次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0752f-126"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0752f-127">[前: はじめに](xref:tutorials/razor-pages-vsc/razor-pages-start)
[次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="0752f-127">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
