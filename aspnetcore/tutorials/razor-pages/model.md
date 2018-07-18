---
title: ASP.NET Core での Razor ページ アプリへのモデルの追加
author: rick-anderson
description: Entity Framework Core (EF Core) を利用し、データベースでムービーを管理するためのクラスを追加する方法について説明します。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 9e0fd0ee98c84bb2336749640b2a3c103e4639b6
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938327"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="26573-103">ASP.NET Core での Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="26573-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="26573-104">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="26573-104">Add a data model</span></span>

<span data-ttu-id="26573-105">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="26573-106">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="26573-106">Name the folder *Models*.</span></span>

<span data-ttu-id="26573-107">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="26573-107">Right click the *Models* folder.</span></span> <span data-ttu-id="26573-108">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="26573-109">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="26573-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="26573-110">`Movie` クラスの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="26573-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="26573-111">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="26573-111">Scaffold the movie model</span></span>

<span data-ttu-id="26573-112">このセクションでは、ムービー モデルがスキャフォールディングされます。</span><span class="sxs-lookup"><span data-stu-id="26573-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="26573-113">つまり、スキャフォールディング ツールにより、ムービー モデルの作成、読み取り、更新、削除の (CRUD) 操作用のページが生成されます。</span><span class="sxs-lookup"><span data-stu-id="26573-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="26573-114">*Pages/Movies* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="26573-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="26573-115">**ソリューション エクスプローラー**で、*[Pages]* フォルダーを右クリックして、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="26573-116">フォルダーに *Movies* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="26573-116">Name the folder *Movies*</span></span>

<span data-ttu-id="26573-117">**ソリューション エクスプローラー**で、*[Pages/Movies]* フォルダーを右クリックし、**[追加]** > **[スキャフォールディングされた新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前の手順からのイメージ。](model/_static/sca.png)

<span data-ttu-id="26573-119">**[スキャフォールディングを追加]** ダイアログで、**[Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用する Razor Pages (CRUD)\)** > **[追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![前の手順からのイメージ。](model/_static/add_scaffold.png)

<span data-ttu-id="26573-121">**[Add Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用して Razor Pages (CRUD) を追加する\)** ダイアログを完了します。</span><span class="sxs-lookup"><span data-stu-id="26573-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="26573-122">**[モデル クラス]** ドロップ ダウンで、**[Movie (RazorPagesMovie.Models)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="26573-123">**データ コンテキスト クラス**行で、**+** (プラス) 記号を選択し、生成された名前 **RazorPagesMovie.Models.RazorPagesMovieContext** を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="26573-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="26573-124">**[データ コンテキスト クラス]** ドロップ ダウンで、**[RazorPagesMovie.Models.RazorPagesMovieContext]** を選択します</span><span class="sxs-lookup"><span data-stu-id="26573-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="26573-125">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="26573-125">Select **Add**.</span></span>

![前の手順からのイメージ。](model/_static/arp.png)

<span data-ttu-id="26573-127">スキャフォールディングのプロセスが作成され、次のファイルが変更されます。</span><span class="sxs-lookup"><span data-stu-id="26573-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="26573-128">作成されたファイル</span><span class="sxs-lookup"><span data-stu-id="26573-128">Files created</span></span>

* <span data-ttu-id="26573-129">*Pages/Movies* 作成、削除、詳細、編集、インデックス。</span><span class="sxs-lookup"><span data-stu-id="26573-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="26573-130">これらのページを次のチュートリアルで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="26573-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="26573-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="26573-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="26573-132">更新されたファイル</span><span class="sxs-lookup"><span data-stu-id="26573-132">Files updates</span></span>

* <span data-ttu-id="26573-133">*Startup.cs*: このファイルに対しての変更を次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="26573-133">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="26573-134">*appsettings.json*: ローカル データベースへの接続に使用される接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="26573-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="26573-135">依存関係挿入に登録されるコンテキストを調べる</span><span class="sxs-lookup"><span data-stu-id="26573-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="26573-136">ASP.NET Core には、[依存関係挿入](xref:fundamentals/dependency-injection)が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="26573-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="26573-137">サービス (EF Core DB コンテキストなど) は、アプリケーションの起動時に依存関係挿入に登録されます。</span><span class="sxs-lookup"><span data-stu-id="26573-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="26573-138">これらのサービスを必要とするコンポーネント (Razor ページなど) には、コンストラクターのパラメーターを介してこれらのサービスが指定されます。</span><span class="sxs-lookup"><span data-stu-id="26573-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="26573-139">DB コンテキスト インスタンスを取得するコンストラクター コードは、チュートリアルの後半で示します。</span><span class="sxs-lookup"><span data-stu-id="26573-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="26573-140">スキャフォールディング ツールが自動的に DB コンテキストを作成し、依存関係挿入コンテナーと共に登録します。</span><span class="sxs-lookup"><span data-stu-id="26573-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="26573-141">`Startup.ConfigureServices` メソッドを調べます。</span><span class="sxs-lookup"><span data-stu-id="26573-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="26573-142">強調表示された行は、スキャフォールダーによって追加されました。</span><span class="sxs-lookup"><span data-stu-id="26573-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="26573-143">所与のデータ モデルの EF Core 機能を調整するメイン クラスは、DB コンテキスト クラスです。</span><span class="sxs-lookup"><span data-stu-id="26573-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="26573-144">データ コンテキストは [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) から派生されます。</span><span class="sxs-lookup"><span data-stu-id="26573-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="26573-145">データ コンテキストによって、データ モデルに含めるエンティティが指定されます。</span><span class="sxs-lookup"><span data-stu-id="26573-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="26573-146">このプロジェクトでは、クラスに `RazorPagesMovieContext` という名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="26573-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="26573-147">上記のコードによって、エンティティ セットの [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="26573-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="26573-148">Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応します。</span><span class="sxs-lookup"><span data-stu-id="26573-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="26573-149">エンティティはテーブル内の行に対応します。</span><span class="sxs-lookup"><span data-stu-id="26573-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="26573-150">[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) オブジェクトでメソッドが呼び出され、接続文字列の名前がコンテキストに渡されます。</span><span class="sxs-lookup"><span data-stu-id="26573-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="26573-151">ローカル開発の場合、[ASP.NET Core 構成システム](xref:fundamentals/configuration/index)が *appsettings.json* ファイルから接続文字列を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="26573-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="26573-152">最初の移行の実行</span><span class="sxs-lookup"><span data-stu-id="26573-152">Perform initial migration</span></span>

<span data-ttu-id="26573-153">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="26573-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="26573-154">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="26573-154">Add an initial migration.</span></span>
* <span data-ttu-id="26573-155">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="26573-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="26573-156">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="26573-158">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="26573-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="26573-159">代わりに、次の .NET Core CLI コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="26573-159">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="26573-160">次の警告メッセージは無視してください。次のチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="26573-160">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="26573-161">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="26573-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="26573-162">このスキーマは、`RazorPagesMovieContext` で指定されたモデルに基づきます (*Data/RazorPagesMovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="26573-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="26573-163">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="26573-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="26573-164">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="26573-165">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="26573-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="26573-166">`Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="26573-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="26573-167">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="26573-167">If you get the error:</span></span>

<span data-ttu-id="26573-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.\(SqlException: ログインで要求されている "RazorPagesMovieContext-GUID" データベースを開くことができませんでした。\)</span><span class="sxs-lookup"><span data-stu-id="26573-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="26573-169">The login failed.\(ログインに失敗しました。\)</span><span class="sxs-lookup"><span data-stu-id="26573-169">The login failed.</span></span>
<span data-ttu-id="26573-170">Login failed for user 'User-name'.\(ユーザー 'ユーザー名' はログインできませんでした。\)</span><span class="sxs-lookup"><span data-stu-id="26573-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="26573-171">[移行手順](#pmc)を失敗しました。</span><span class="sxs-lookup"><span data-stu-id="26573-171">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="26573-172">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="26573-172">Add a data model</span></span>

<span data-ttu-id="26573-173">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="26573-174">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="26573-174">Name the folder *Models*.</span></span>

<span data-ttu-id="26573-175">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="26573-175">Right click the *Models* folder.</span></span> <span data-ttu-id="26573-176">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="26573-177">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="26573-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="26573-178">データベース接続文字列の追加</span><span class="sxs-lookup"><span data-stu-id="26573-178">Add a database connection string</span></span>

<span data-ttu-id="26573-179">*appsettings.json* ファイルに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="26573-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="26573-180">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="26573-180">Register the database context</span></span>

<span data-ttu-id="26573-181">[Startup クラスの ConfigureServices メソッド](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) で、[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーを使用してデータベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="26573-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="26573-182">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="26573-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="26573-183">スキャフォールディング ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="26573-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="26573-184">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="26573-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="26573-185">Visual Studio Web コード生成パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="26573-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="26573-186">スキャフォールディング エンジンを実行するには、このパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="26573-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="26573-187">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="26573-187">Add an initial migration.</span></span>
* <span data-ttu-id="26573-188">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="26573-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="26573-189">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="26573-191">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="26573-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="26573-192">代わりに、次の .NET Core CLI コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="26573-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="26573-193">次のメッセージは無視してください。</span><span class="sxs-lookup"><span data-stu-id="26573-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="26573-194">これは次のチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="26573-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="26573-195">`Install-Package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="26573-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="26573-196">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="26573-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="26573-197">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="26573-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="26573-198">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="26573-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="26573-199">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="26573-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="26573-200">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="26573-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="26573-201">`Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="26573-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="26573-202">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="26573-202">Test the app</span></span>

* <span data-ttu-id="26573-203">アプリを実行し、ブラウザーで URL に `/Movies` を追加します ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="26573-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="26573-204">**[作成]** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="26573-204">Test the **Create** link.</span></span>

  ![[作成] ページ](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="26573-206">**[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="26573-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="26573-207">SQL の例外が発生した場合は、移行を実行済みであり、データベースを更新したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="26573-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="26573-208">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="26573-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="26573-209">[前: はじめに](xref:tutorials/razor-pages/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="26573-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
