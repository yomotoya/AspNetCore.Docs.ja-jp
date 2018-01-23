---
title: "ASP.NET Core での Razor ページ アプリへのモデルの追加"
author: rick-anderson
description: "ASP.NET Core での Razor ページ アプリへのモデルの追加"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 84e5ec27904b564fa6ee29843ceae0bb70754ea7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="dad35-103">Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="dad35-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="dad35-104">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="dad35-104">Add a data model</span></span>

<span data-ttu-id="dad35-105">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="dad35-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="dad35-106">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="dad35-106">Name the folder *Models*.</span></span>

<span data-ttu-id="dad35-107">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="dad35-107">Right click the *Models* folder.</span></span> <span data-ttu-id="dad35-108">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="dad35-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="dad35-109">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="dad35-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="dad35-110">データベース接続文字列の追加</span><span class="sxs-lookup"><span data-stu-id="dad35-110">Add a database connection string</span></span>

<span data-ttu-id="dad35-111">*appsettings.json* ファイルに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="dad35-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="dad35-112">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="dad35-112">Register the database context</span></span>

<span data-ttu-id="dad35-113">*Startup.cs* ファイルで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーを使用して、データベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="dad35-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="dad35-114">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="dad35-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="dad35-115">スキャフォールディング ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="dad35-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="dad35-116">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="dad35-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="dad35-117">Visual Studio Web コード生成パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="dad35-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="dad35-118">スキャフォールディング エンジンを実行するには、このパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="dad35-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="dad35-119">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="dad35-119">Add an initial migration.</span></span>
* <span data-ttu-id="dad35-120">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="dad35-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="dad35-121">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="dad35-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="dad35-123">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="dad35-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="dad35-124">`Install-Package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="dad35-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="dad35-125">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="dad35-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="dad35-126">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="dad35-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="dad35-127">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="dad35-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="dad35-128">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="dad35-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="dad35-129">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="dad35-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="dad35-130">`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="dad35-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="dad35-131">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="dad35-131">Test the app</span></span>

* <span data-ttu-id="dad35-132">アプリを実行し、ブラウザーで URL に `/Movies` を追加します (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="dad35-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="dad35-133">**[作成]** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="dad35-133">Test the **Create** link.</span></span>

 ![[作成] ページ](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="dad35-135">**[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="dad35-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="dad35-136">SQL の例外が発生した場合は、移行を実行済みであり、データベースを更新したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="dad35-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="dad35-137">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="dad35-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dad35-138">[前: はじめに](xref:tutorials/razor-pages/razor-pages-start)
[次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="dad35-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
