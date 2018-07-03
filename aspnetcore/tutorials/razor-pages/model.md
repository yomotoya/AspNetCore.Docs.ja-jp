---
title: ASP.NET Core での Razor ページ アプリへのモデルの追加
author: rick-anderson
description: Entity Framework Core (EF Core) を利用し、データベースでムービーを管理するためのクラスを追加する方法について説明します。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327553"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="aecd8-103">ASP.NET Core での Razor ページ アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="aecd8-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="aecd8-104">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="aecd8-104">Add a data model</span></span>

<span data-ttu-id="aecd8-105">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="aecd8-106">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-106">Name the folder *Models*.</span></span>

<span data-ttu-id="aecd8-107">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="aecd8-107">Right click the *Models* folder.</span></span> <span data-ttu-id="aecd8-108">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="aecd8-109">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="aecd8-110">`Movie` クラスの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="aecd8-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="aecd8-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="aecd8-112">ムービー モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="aecd8-112">Scaffold the movie model</span></span>

<span data-ttu-id="aecd8-113">このセクションでは、ムービー モデルがスキャフォールディングされます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="aecd8-114">つまり、スキャフォールディング ツールにより、ムービー モデルの作成、読み取り、更新、削除の (CRUD) 操作用のページが生成されます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="aecd8-115">*Pages/Movies* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="aecd8-116">**ソリューション エクスプローラー**で、*[Pages]* フォルダーを右クリックして、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="aecd8-117">フォルダーに *Movies* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-117">Name the folder *Movies*</span></span>

<span data-ttu-id="aecd8-118">**ソリューション エクスプローラー**で、*[Pages/Movies]* フォルダーを右クリックし、**[追加]** > **[スキャフォールディングされた新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![前の手順からのイメージ。](model/_static/sca.png)

<span data-ttu-id="aecd8-120">**[スキャフォールディングを追加]** ダイアログで、**[Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用する Razor Pages (CRUD)\)** > **[追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![前の手順からのイメージ。](model/_static/add_scaffold.png)

<span data-ttu-id="aecd8-122">**[Add Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用して Razor Pages (CRUD) を追加する\)** ダイアログを完了します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="aecd8-123">**[モデル クラス]** ドロップ ダウンで、**[Movie (RazorPagesMovie.Models)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="aecd8-124">**データ コンテキスト クラス**行で、**+** (プラス) 記号を選択し、生成された名前 **RazorPagesMovie.Models.RazorPagesMovieContext** を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="aecd8-125">**[データ コンテキスト クラス]** ドロップ ダウンで、**[RazorPagesMovie.Models.RazorPagesMovieContext]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="aecd8-126">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-126">Select **Add**.</span></span>

![前の手順からのイメージ。](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="aecd8-128">最初の移行の実行</span><span class="sxs-lookup"><span data-stu-id="aecd8-128">Perform initial migration</span></span>

<span data-ttu-id="aecd8-129">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="aecd8-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="aecd8-130">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-130">Add an initial migration.</span></span>
* <span data-ttu-id="aecd8-131">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="aecd8-132">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="aecd8-134">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="aecd8-135">代わりに、次の .NET Core CLI コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="aecd8-136">次の警告メッセージは無視してください。次のチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="aecd8-137">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="aecd8-138">このスキーマは、`RazorPagesMovieContext` で指定されたモデルに基づきます (*Data/RazorPagesMovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="aecd8-138">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="aecd8-139">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="aecd8-140">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="aecd8-141">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aecd8-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="aecd8-142">`Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="aecd8-143">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="aecd8-143">If you get the error:</span></span>

<span data-ttu-id="aecd8-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.\(SqlException: ログインで要求されている "RazorPagesMovieContext-GUID" データベースを開くことができませんでした。\)</span><span class="sxs-lookup"><span data-stu-id="aecd8-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="aecd8-145">The login failed.\(ログインに失敗しました。\)</span><span class="sxs-lookup"><span data-stu-id="aecd8-145">The login failed.</span></span>
<span data-ttu-id="aecd8-146">Login failed for user 'User-name'.\(ユーザー 'ユーザー名' はログインできませんでした。\)</span><span class="sxs-lookup"><span data-stu-id="aecd8-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="aecd8-147">[移行手順](#pmc)を失敗しました。</span><span class="sxs-lookup"><span data-stu-id="aecd8-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="aecd8-148">データ モデルの追加</span><span class="sxs-lookup"><span data-stu-id="aecd8-148">Add a data model</span></span>

<span data-ttu-id="aecd8-149">ソリューション エクスプローラーで、**RazorPagesMovie** プロジェクトを右クリックし、**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="aecd8-150">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-150">Name the folder *Models*.</span></span>

<span data-ttu-id="aecd8-151">*Models* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="aecd8-151">Right click the *Models* folder.</span></span> <span data-ttu-id="aecd8-152">**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="aecd8-153">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="aecd8-154">データベース接続文字列の追加</span><span class="sxs-lookup"><span data-stu-id="aecd8-154">Add a database connection string</span></span>

<span data-ttu-id="aecd8-155">*appsettings.json* ファイルに接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="aecd8-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="aecd8-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="aecd8-157">データベース コンテキストの登録</span><span class="sxs-lookup"><span data-stu-id="aecd8-157">Register the database context</span></span>

<span data-ttu-id="aecd8-158">[Startup クラスの ConfigureServices メソッド](xref:fundamentals/startup#the-startup-class) (*Startup.cs*) で、[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーを使用してデータベース コンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="aecd8-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="aecd8-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="aecd8-160">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="aecd8-161">スキャフォールディング ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="aecd8-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="aecd8-162">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="aecd8-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="aecd8-163">Visual Studio Web コード生成パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="aecd8-164">スキャフォールディング エンジンを実行するには、このパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="aecd8-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="aecd8-165">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-165">Add an initial migration.</span></span>
* <span data-ttu-id="aecd8-166">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="aecd8-167">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC メニュー](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="aecd8-169">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="aecd8-170">代わりに、次の .NET Core CLI コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="aecd8-171">次のメッセージは無視してください。</span><span class="sxs-lookup"><span data-stu-id="aecd8-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="aecd8-172">これは次のチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="aecd8-173">`Install-Package` コマンドでスキャフォールディング エンジンを実行するために必要なツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aecd8-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="aecd8-174">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="aecd8-175">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Models/MovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="aecd8-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="aecd8-176">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="aecd8-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="aecd8-177">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="aecd8-178">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aecd8-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="aecd8-179">`Update-Database` コマンドは、データベースを作成する、*Migrations/{time-stamp}_InitialCreate.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="aecd8-180">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="aecd8-180">Test the app</span></span>

* <span data-ttu-id="aecd8-181">アプリを実行し、ブラウザーで URL に `/Movies` を追加します (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="aecd8-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="aecd8-182">**[作成]** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="aecd8-182">Test the **Create** link.</span></span>

  ![[作成] ページ](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="aecd8-184">**[編集]**、**[詳細]**、および **[削除]** の各リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="aecd8-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="aecd8-185">SQL の例外が発生した場合は、移行を実行済みであり、データベースを更新したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="aecd8-186">次のチュートリアルでは、スキャフォールディングによって作成されるファイルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="aecd8-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aecd8-187">[前: はじめに](xref:tutorials/razor-pages/razor-pages-start)
> [次: Razor ページのスキャフォールディング](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="aecd8-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
