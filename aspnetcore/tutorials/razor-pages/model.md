---
title: "ASP.NET Core での Razor ページ アプリへのモデルの追加"
author: rick-anderson
description: "ASP.NET Core での Razor ページ アプリへのモデルの追加"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/modelz
ms.openlocfilehash: 1a08ecf1ee12fa0860cb6a18c1a63eaff2ddfbed
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="f6167-104">Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="f6167-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f6167-105">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="f6167-105">Add a data model</span></span>

<span data-ttu-id="f6167-106">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f6167-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="f6167-107">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="f6167-107">Name the folder *Models*.</span></span>

<span data-ttu-id="f6167-108">*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f6167-108">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="f6167-109">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="f6167-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="f6167-110">データベース接続文字列の追加</span><span class="sxs-lookup"><span data-stu-id="f6167-110">Add a database connection string</span></span>

<span data-ttu-id="f6167-111">*appsettings.json* ファイルに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="f6167-111">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="f6167-112">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="f6167-112">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="f6167-113">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="f6167-113">Register the database context</span></span>

<span data-ttu-id="f6167-114">*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="f6167-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

<span data-ttu-id="f6167-115">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="f6167-115">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]</span></span>

<span data-ttu-id="f6167-116">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="f6167-116">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="f6167-117">スキャフォールディング ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="f6167-117">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="f6167-118">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="f6167-118">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="f6167-119">Visual Studio Web コード生成パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="f6167-119">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="f6167-120">スキャフォールディング エンジンを実行するには、このパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="f6167-120">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="f6167-121">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="f6167-121">Add an initial migration.</span></span>
* <span data-ttu-id="f6167-122">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="f6167-122">Update the database with the initial migration.</span></span>

<span data-ttu-id="f6167-123">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f6167-123">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="f6167-125">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="f6167-125">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f6167-126">`Install-Package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="f6167-126">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="f6167-127">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f6167-127">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="f6167-128">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="f6167-128">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="f6167-129">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="f6167-129">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f6167-130">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="f6167-130">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="f6167-131">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6167-131">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="f6167-132">`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f6167-132">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

<span data-ttu-id="f6167-133">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f6167-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f6167-134">[前: はじめに](xref:tutorials/razor-pages/razor-pages-start)
[次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f6167-134">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    