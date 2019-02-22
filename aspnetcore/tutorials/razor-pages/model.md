---
title: ASP.NET Core での Razor ページ アプリへのモデルの追加
author: rick-anderson
description: Entity Framework Core (EF Core) を利用し、データベースでムービーを管理するためのクラスを追加する方法について説明します。
ms.author: riande
ms.date: 02/12/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: c7341430e8e2ace7eb04faa308020095139d5b94
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410233"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="1c527-103">ASP.NET Core での Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="1c527-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="1c527-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c527-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="1c527-105">このセクションでは、データベースで映画を管理するクラスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="1c527-106">これらのクラスは、データベースを操作するために [Entity Framework Core](/ef/core) (EF Core) で使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="1c527-107">EF Core は、データ アクセス コードを簡略化するオブジェクト リレーショナル マッピング (ORM) フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="1c527-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="1c527-108">このモデル クラスは、EF Core に対する依存関係がないために、POCO クラス (plain-old CLR オブジェクト、つまり単純な従来の CLR) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1c527-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="1c527-109">これらは、データベースに格納されるデータのプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="1c527-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="1c527-110">サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start)します。</span><span class="sxs-lookup"><span data-stu-id="1c527-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="1c527-111">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="1c527-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c527-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c527-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1c527-113">**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="1c527-114">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="1c527-114">Name the folder *Models*.</span></span>

<span data-ttu-id="1c527-115">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="1c527-115">Right click the *Models* folder.</span></span> <span data-ttu-id="1c527-116">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="1c527-117">クラスに **Movie** と名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="1c527-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c527-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c527-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1c527-119">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c527-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="1c527-120">*Movie.cs* という名前の *Models* フォルダーにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c527-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c527-121">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1c527-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1c527-122">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="1c527-123">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="1c527-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="1c527-124">*Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="1c527-125">**[新しいファイル]** ダイアログで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="1c527-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="1c527-126">左側のウィンドウで **[全般]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="1c527-127">中央ウィンドウで **[空のクラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="1c527-128">クラスに **Movie** という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="1c527-129">プロジェクトをビルドして、コンパイル エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="1c527-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="1c527-130">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="1c527-130">Scaffold the movie model</span></span>

<span data-ttu-id="1c527-131">このセクションでは、ムービー モデルがスキャフォールディングされます。</span><span class="sxs-lookup"><span data-stu-id="1c527-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="1c527-132">つまり、スキャフォールディング ツールにより、ムービー モデルの作成、読み取り、更新、削除の (CRUD) 操作用のページが生成されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c527-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c527-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1c527-134">*Pages/Movies* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c527-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="1c527-135">*Pages* フォルダーを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="1c527-136">フォルダーに *Movies* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="1c527-136">Name the folder *Movies*</span></span>

<span data-ttu-id="1c527-137">*Pages/Movies* フォルダーを右クリックし、**[追加]** > **[スキャフォールディングされた新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前の手順からのイメージ。](model/_static/sca.png)

<span data-ttu-id="1c527-139">**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用する Razor ページ (CRUD)]** > **[追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![前の手順からのイメージ。](model/_static/add_scaffold.png)

<span data-ttu-id="1c527-141">**[Add Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用して Razor Pages (CRUD) を追加する\)** ダイアログを完了します。</span><span class="sxs-lookup"><span data-stu-id="1c527-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="1c527-142">**[モデル クラス]** ドロップ ダウンで、**[Movie (RazorPagesMovie.Models)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="1c527-143">**データ コンテキスト クラス**行で、**+** (プラス) 記号を選択し、生成された名前 **RazorPagesMovie.Models.RazorPagesMovieContext** を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="1c527-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="1c527-144">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="1c527-144">Select **Add**.</span></span>

![前の手順からのイメージ。](model/_static/arp.png)

<span data-ttu-id="1c527-146">*appsettings.json* ファイルは、ローカル データベースへの接続に使用される接続文字列を使用して更新されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c527-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c527-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="1c527-148">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="1c527-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="1c527-149">スキャフォールディング ツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1c527-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="1c527-150">**Windows の場合**:次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c527-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="1c527-151">**macOS および Linux の場合**:次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c527-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c527-152">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1c527-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1c527-153">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="1c527-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="1c527-154">スキャフォールディング ツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1c527-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```
* <span data-ttu-id="1c527-155">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c527-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="1c527-156">上記のコマンドで次の警告が生成されます。"エンティティ型 'Movie' の decimal 列 'Price' に型が指定されていません。</span><span class="sxs-lookup"><span data-stu-id="1c527-156">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="1c527-157">これにより、値が既定の有効桁数と小数点以下桁数に収まらない場合、自動的に切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="1c527-157">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="1c527-158">'HasColumnType()' を使用してすべての値に適合する SQL server 列の型を明示的に指定します。"</span><span class="sxs-lookup"><span data-stu-id="1c527-158">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="1c527-159">この警告は無視して構いません。後のチュートリアルで修正されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-159">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="1c527-160">スキャフォールディングのプロセスが作成され、次のファイルが更新されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-160">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="1c527-161">作成されたファイル</span><span class="sxs-lookup"><span data-stu-id="1c527-161">Files created</span></span>

* <span data-ttu-id="1c527-162">*Pages/Movies*: 作成、削除、詳細、編集、インデックス。</span><span class="sxs-lookup"><span data-stu-id="1c527-162">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="1c527-163">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="1c527-163">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="1c527-164">更新されたファイル</span><span class="sxs-lookup"><span data-stu-id="1c527-164">File updated</span></span>

* <span data-ttu-id="1c527-165">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="1c527-165">*Startup.cs*</span></span>

<span data-ttu-id="1c527-166">作成および更新されたファイルについては、次のセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="1c527-166">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="1c527-167">最初の移行</span><span class="sxs-lookup"><span data-stu-id="1c527-167">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c527-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c527-168">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="1c527-169">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="1c527-169">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="1c527-170">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="1c527-170">Add an initial migration.</span></span>
* <span data-ttu-id="1c527-171">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="1c527-171">Update the database with the initial migration.</span></span>

<span data-ttu-id="1c527-172">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1c527-172">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="1c527-174">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1c527-174">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c527-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c527-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c527-176">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1c527-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="1c527-177">`ef migrations add InitialCreate` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-177">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="1c527-178">このスキーマは、`DbContext` で指定されたモデルに基づきます (*RazorPagesMovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="1c527-178">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="1c527-179">`InitialCreate` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-179">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="1c527-180">任意の名前を使用できますが、規則により、移行を説明する名前が選択されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-180">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="1c527-181">`ef database update` コマンドは、*Migrations/\<time-stamp>_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c527-181">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="1c527-182">`Up` メソッドにより、データベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-182">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c527-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c527-183">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="1c527-184">依存関係挿入に登録されるコンテキストを調べる</span><span class="sxs-lookup"><span data-stu-id="1c527-184">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="1c527-185">ASP.NET Core には、[依存関係挿入](xref:fundamentals/dependency-injection)が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="1c527-185">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1c527-186">サービス (EF Core DB コンテキストなど) は、アプリケーションの起動時に依存関係挿入に登録されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-186">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="1c527-187">これらのサービスを必要とするコンポーネント (Razor ページなど) には、コンストラクターのパラメーターを介してこれらのサービスが指定されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-187">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="1c527-188">DB コンテキスト インスタンスを取得するコンストラクター コードは、チュートリアルの後半で示します。</span><span class="sxs-lookup"><span data-stu-id="1c527-188">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="1c527-189">スキャフォールディング ツールが自動的に DB コンテキストを作成し、依存関係挿入コンテナーと共に登録します。</span><span class="sxs-lookup"><span data-stu-id="1c527-189">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="1c527-190">`Startup.ConfigureServices` メソッドを調べます。</span><span class="sxs-lookup"><span data-stu-id="1c527-190">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="1c527-191">強調表示された行は、スキャフォールダーによって追加されました。</span><span class="sxs-lookup"><span data-stu-id="1c527-191">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="1c527-192">`RazorPagesMovieContext` では、`Movie` モデルのために EF Core 機能 (作成、読み取り、更新、削除など) が調整されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-192">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="1c527-193">データ コンテキスト (`RazorPagesMovieContext`) は [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) から派生されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-193">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="1c527-194">データ コンテキストによって、データ モデルに含めるエンティティが指定されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-194">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="1c527-195">上記のコードによって、エンティティ セットの [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-195">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="1c527-196">Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応します。</span><span class="sxs-lookup"><span data-stu-id="1c527-196">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="1c527-197">エンティティはテーブル内の行に対応します。</span><span class="sxs-lookup"><span data-stu-id="1c527-197">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="1c527-198">[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) オブジェクトでメソッドが呼び出され、接続文字列の名前がコンテキストに渡されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-198">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="1c527-199">ローカル開発の場合、[ASP.NET Core 構成システム](xref:fundamentals/configuration/index)が *appsettings.json* ファイルから接続文字列を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="1c527-199">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c527-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c527-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1c527-201">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1c527-201">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="1c527-202">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-202">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="1c527-203">このスキーマは、`RazorPagesMovieContext` で指定されたモデルに基づきます (*Data/RazorPagesMovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="1c527-203">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="1c527-204">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-204">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="1c527-205">任意の名前を使用できますが、規則により、移行を説明する名前が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c527-205">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="1c527-206">詳細については、「<xref:data/ef-mvc/migrations>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1c527-206">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="1c527-207">`Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c527-207">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="1c527-208">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="1c527-208">Test the app</span></span>

* <span data-ttu-id="1c527-209">アプリを実行し、ブラウザーで URL に `/Movies` を追加します ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="1c527-209">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="1c527-210">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="1c527-210">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="1c527-211">[移行手順](#pmc)を失敗しました。</span><span class="sxs-lookup"><span data-stu-id="1c527-211">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="1c527-212">**[作成]** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="1c527-212">Test the **Create** link.</span></span>

  ![[作成] ページ](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="1c527-214">`Price` フィールドに小数点のコンマを入力できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="1c527-214">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="1c527-215">小数点にコンマ (",") を使う英語以外のロケール、および英語 (米国) 以外の日付形式で、[jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c527-215">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="1c527-216">グローバル化の手順については、[この GitHub の記事](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1c527-216">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="1c527-217">**[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="1c527-217">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="1c527-218">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1c527-218">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c527-219">[前: はじめに](xref:tutorials/razor-pages/razor-pages-start)
> [次: スキャフォールディングされた Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="1c527-219">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
