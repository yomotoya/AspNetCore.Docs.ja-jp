---
title: .ASP.NET Core MVC アプリへのモデルの追加
author: rick-anderson
description: 単純な ASP.NET Core アプリケーションにモデルを追加します。
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 1e562116de8e6a88666f578f7255e325735c10a9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272323"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="4abc0-103">.ASP.NET Core MVC アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="4abc0-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="4abc0-104">*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="4abc0-105">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="4abc0-106">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="4abc0-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="4abc0-107">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="4abc0-108">これで、**M**VC アプリに、**モ**デルがあることになります。</span><span class="sxs-lookup"><span data-stu-id="4abc0-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="4abc0-109">コントローラーのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="4abc0-109">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4abc0-110">*ソリューション エクスプローラー*で、**Controllers** フォルダーを右クリックし、**[追加]、[スキャフォールディングされた新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![前述の手順を参照](adding-model/_static/add_controller21.png)

<span data-ttu-id="4abc0-112">**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順にタップします。</span><span class="sxs-lookup"><span data-stu-id="4abc0-112">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="4abc0-114">**ソリューション エクスプローラー**で、*Controllers* フォルダーを右クリックし、**[追加]、[コントローラー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-114">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![前述の手順を参照](adding-model/_static/add_controller.png)

<span data-ttu-id="4abc0-116">**[MVC 依存関係の追加]** ダイアログ ボックスが表示された場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="4abc0-116">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="4abc0-117">[Visual Studio を最新バージョンに更新します](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="4abc0-117">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="4abc0-118">15.5 より前のバージョンの Visual Studio の場合はこのダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-118">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="4abc0-119">更新できない場合は、**[追加]** を選択してから、もう一度コントローラーの追加手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="4abc0-119">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="4abc0-120">**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順にタップします。</span><span class="sxs-lookup"><span data-stu-id="4abc0-120">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="4abc0-122">**[コントローラーの追加]** ダイアログ ボックスを完了します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-122">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="4abc0-123">**[Model class]\(モデル クラス\):** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="4abc0-123">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="4abc0-124">**[Data context class]\(データ コンテキスト クラス\):****+** アイコンを選択し、既定の **MvcMovie.Models.MvcMovieContext** を追加します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-124">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![[データの追加] コンテキスト](adding-model/_static/dc.png)

* <span data-ttu-id="4abc0-126">**ビュー:** 各オプションの既定値をオンにします。</span><span class="sxs-lookup"><span data-stu-id="4abc0-126">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="4abc0-127">**コントローラー名:** 既定の *MoviesController* のままにします。</span><span class="sxs-lookup"><span data-stu-id="4abc0-127">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="4abc0-128">**[追加]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="4abc0-128">Tap **Add**</span></span>

![[コントローラーの追加] ダイアログ](adding-model/_static/add_controller2.png)

<span data-ttu-id="4abc0-130">Visual Studio では、次が作成されます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-130">Visual Studio creates:</span></span>

* <span data-ttu-id="4abc0-131">Entity Framework Core の[データベース コンテキスト クラス](xref:data/ef-mvc/intro#create-the-database-context)(*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="4abc0-131">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="4abc0-132">ムービー コントローラー (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="4abc0-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="4abc0-133">作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="4abc0-133">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="4abc0-134">データベース コンテキストと [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。</span><span class="sxs-lookup"><span data-stu-id="4abc0-134">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="4abc0-135">ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="4abc0-136">アプリを実行し、**[MVC Movie]\(MVC ムービー\)** リンクをクリックすると、次のようなエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-136">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="4abc0-137">データベースを作成する必要があり、それには EF Core [移行](xref:data/ef-mvc/migrations)機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-137">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="4abc0-138">移行では、データ モデルに一致するデータベースを作成し、データ モデルの変更時にデータベース スキーマを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-138">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="4abc0-139">EF ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="4abc0-139">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="4abc0-140">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="4abc0-140">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="4abc0-141">Entity Framework Core のツール パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-141">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="4abc0-142">このパッケージは移行を追加し、データベースを更新するために必要です。</span><span class="sxs-lookup"><span data-stu-id="4abc0-142">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="4abc0-143">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-143">Add an initial migration.</span></span>
* <span data-ttu-id="4abc0-144">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-144">Update the database with the initial migration.</span></span>

<span data-ttu-id="4abc0-145">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<span data-ttu-id="4abc0-146"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC メニュー](adding-model/_static/pmc.png)</span><span class="sxs-lookup"><span data-stu-id="4abc0-146"><!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menu](adding-model/_static/pmc.png)</span></span>

<span data-ttu-id="4abc0-147">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-147">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="4abc0-148">次のエラー メッセージは無視してください。次のチュートリアルで修正しました。</span><span class="sxs-lookup"><span data-stu-id="4abc0-148">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="4abc0-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="4abc0-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="4abc0-150">*エンティティ型 'Movie' の decimal 列 'Price' に型が指定されていません。これにより、値が既定の有効桁数と小数点以下桁数に収まらない場合、自動的に切り捨てられます。'ForHasColumnType()' を使用してすべての値に適合する SQL server 列の型を明示的に指定します。*</span><span class="sxs-lookup"><span data-stu-id="4abc0-150">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="4abc0-151">**注:** `Install-Package` コマンドでエラーが発生した場合は、NuGet パッケージ マネージャーを開き、`Microsoft.EntityFrameworkCore.Tools` パッケージを検索してください。</span><span class="sxs-lookup"><span data-stu-id="4abc0-151">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="4abc0-152">これにより、パッケージをインストールしたり、パッケージが既にインストールされているかどうかを確認したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-152">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="4abc0-153">または、PMC で問題がある場合、[CLI を使用したアプローチ](#cli)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4abc0-153">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="4abc0-154">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-154">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="4abc0-155">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Data/MvcMovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="4abc0-155">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="4abc0-156">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-156">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="4abc0-157">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-157">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="4abc0-158">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4abc0-158">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="4abc0-159">`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_Initial.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-159">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="4abc0-160">前述の手順は、PMC ではなく、コマンド ライン インターフェイス (CLI) を使用して実行できます。</span><span class="sxs-lookup"><span data-stu-id="4abc0-160">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="4abc0-161">[EF Core ツール](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)を *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-161">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="4abc0-162">(プロジェクト ディレクトリの) コンソールから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4abc0-162">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="4abc0-163">アプリを実行してエラーが表示される場合:</span><span class="sxs-lookup"><span data-stu-id="4abc0-163">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="4abc0-164">`dotnet ef database update` を実行していない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4abc0-164">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="4abc0-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="4abc0-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="4abc0-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="4abc0-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Intellisense のコンテキスト メニュー (Model アイテムの ID、価格、リリース日、およびタイトル プロパティ)](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="4abc0-168">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="4abc0-168">Additional resources</span></span>

* [<span data-ttu-id="4abc0-169">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="4abc0-169">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="4abc0-170">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="4abc0-170">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="4abc0-171">[前のチュートリアル ビューの追加](adding-view.md)
> [次のチュートリアル SQL の使用](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="4abc0-171">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
