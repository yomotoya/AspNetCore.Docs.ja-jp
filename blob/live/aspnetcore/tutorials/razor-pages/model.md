---
title: "ASP.NET Core での Razor ページ アプリへのモデルの追加"
author: rick-anderson
description: "ASP.NET Core での Razor ページ アプリへのモデルの追加"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 38f27a1d5ca80cec4b7bc43c3d5473fc829f1b05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="37446-104">Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="37446-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="37446-105">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="37446-105">Add a data model</span></span>

<span data-ttu-id="37446-106">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="37446-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="37446-107">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="37446-107">Name the folder *Models*.</span></span>

<span data-ttu-id="37446-108">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="37446-108">Right click the *Models* folder.</span></span> <span data-ttu-id="37446-109">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="37446-109">Select **Add** > **Class**.</span></span> <span data-ttu-id="37446-110">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="37446-110">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="37446-111">データベース接続文字列の追加</span><span class="sxs-lookup"><span data-stu-id="37446-111">Add a database connection string</span></span>

<span data-ttu-id="37446-112">*appsettings.json* ファイルに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="37446-112">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="37446-113">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="37446-113">Register the database context</span></span>

<span data-ttu-id="37446-114">*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="37446-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

<span data-ttu-id="37446-115">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="37446-115">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="37446-116">スキャフォールディング ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="37446-116">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="37446-117">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="37446-117">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="37446-118">Visual Studio Web コード生成パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="37446-118">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="37446-119">スキャフォールディング エンジンを実行するには、このパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="37446-119">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="37446-120">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="37446-120">Add an initial migration.</span></span>
* <span data-ttu-id="37446-121">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="37446-121">Update the database with the initial migration.</span></span>

<span data-ttu-id="37446-122">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="37446-122">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="37446-124">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="37446-124">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="37446-125">`Install-Package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="37446-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="37446-126">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="37446-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="37446-127">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="37446-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="37446-128">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="37446-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="37446-129">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="37446-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="37446-130">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="37446-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="37446-131">`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="37446-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="37446-132">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="37446-132">Test the app</span></span>

* <span data-ttu-id="37446-133">アプリを実行し、ブラウザーで URL に `/Movies` を追加します (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="37446-133">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="37446-134">**[作成]** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="37446-134">Test the **Create** link.</span></span>

 ![[作成] ページ](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="37446-136">**[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="37446-136">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="37446-137">SQL の例外が発生した場合は、移行を実行済みであり、データベースを更新したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="37446-137">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="37446-138">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="37446-138">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="37446-139">[前: はじめに](xref:tutorials/razor-pages/razor-pages-start)
[次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="37446-139">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
