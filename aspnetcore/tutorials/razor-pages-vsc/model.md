---
title: Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する
author: rick-anderson
description: Visual Studio Code を使用し、ASP.NET Core で Razor ページ アプリにモデルを追加する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244724"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="326ef-103">Visual Studio Code を使用して ASP.NET Core Razor ページ アプリにモデルを追加する</span><span class="sxs-lookup"><span data-stu-id="326ef-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="326ef-104">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="326ef-104">Add a data model</span></span>

* <span data-ttu-id="326ef-105">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="326ef-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="326ef-106">*Movie.cs* という名前の *Models* フォルダーにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="326ef-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="326ef-107">*Models/Movie.cs* ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="326ef-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="326ef-108">SQLite 用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="326ef-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="326ef-109">コマンド ラインから、次の .NET Core CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="326ef-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="326ef-110">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="326ef-110">Register the database context</span></span>

<span data-ttu-id="326ef-111">*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="326ef-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

<span data-ttu-id="326ef-112">*Startup.cs* の先頭に次の `using` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="326ef-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="326ef-113">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="326ef-113">Build the project to verify you don't have any errors.</span></span>

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="326ef-114">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="326ef-114">Scaffold the Movie model</span></span>

* <span data-ttu-id="326ef-115">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="326ef-115">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="326ef-116">**Windows の場合**: 次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="326ef-116">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="326ef-117">**macOS および Linux の場合**: 次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="326ef-117">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="326ef-118">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="326ef-118">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="326ef-119">[前: はじめに](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="326ef-119">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
