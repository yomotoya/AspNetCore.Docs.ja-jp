---
title: .ASP.NET Core MVC アプリへのモデルの追加
author: rick-anderson
description: 単純な ASP.NET Core アプリケーションにモデルを追加します。
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 4204d4e2d474db51692d42751a9f82373e9f0c0d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="32beb-103">.ASP.NET Core MVC アプリへのモデルの追加</span><span class="sxs-lookup"><span data-stu-id="32beb-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="32beb-104">注: ASP.NET Core 2.0 テンプレートには、*Models* フォルダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="32beb-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="32beb-105">*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="32beb-105">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="32beb-106">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="32beb-106">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="32beb-107">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="32beb-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="32beb-108">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="32beb-108">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="32beb-109">これで、**M**VC アプリに、**モ**デルがあることになります。</span><span class="sxs-lookup"><span data-stu-id="32beb-109">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="32beb-110">コントローラーのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="32beb-110">Scaffolding a controller</span></span>

<span data-ttu-id="32beb-111">**ソリューション エクスプローラー**で、*Controllers* フォルダーを右クリックし、**[追加]、[コントローラー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="32beb-111">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![前述の手順を参照](adding-model/_static/add_controller.png)

<span data-ttu-id="32beb-113">**[MVC 依存関係の追加]** ダイアログ ボックスが表示された場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="32beb-113">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="32beb-114">[Visual Studio を最新バージョンに更新します](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="32beb-114">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="32beb-115">15.5 より前のバージョンの Visual Studio の場合はこのダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="32beb-115">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="32beb-116">更新できない場合は、**[追加]** を選択してから、もう一度コントローラーの追加手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="32beb-116">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="32beb-117">**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順にタップします。</span><span class="sxs-lookup"><span data-stu-id="32beb-117">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="32beb-119">**[コントローラーの追加]** ダイアログ ボックスを完了します。</span><span class="sxs-lookup"><span data-stu-id="32beb-119">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="32beb-120">**[Model class]\(モデル クラス\):** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="32beb-120">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="32beb-121">**[Data context class]\(データ コンテキスト クラス\):****+** アイコンを選択し、既定の **MvcMovie.Models.MvcMovieContext** を追加します。</span><span class="sxs-lookup"><span data-stu-id="32beb-121">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![[データの追加] コンテキスト](adding-model/_static/dc.png)

* <span data-ttu-id="32beb-123">**ビュー:** 各オプションの既定値をオンにします。</span><span class="sxs-lookup"><span data-stu-id="32beb-123">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="32beb-124">**コントローラー名:** 既定の *MoviesController* のままにします。</span><span class="sxs-lookup"><span data-stu-id="32beb-124">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="32beb-125">**[追加]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="32beb-125">Tap **Add**</span></span>

![[コントローラーの追加] ダイアログ](adding-model/_static/add_controller2.png)

<span data-ttu-id="32beb-127">Visual Studio では、次が作成されます。</span><span class="sxs-lookup"><span data-stu-id="32beb-127">Visual Studio creates:</span></span>

* <span data-ttu-id="32beb-128">Entity Framework Core の[データベース コンテキスト クラス](xref:data/ef-mvc/intro#create-the-database-context)(*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="32beb-128">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="32beb-129">ムービー コントローラー (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="32beb-129">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="32beb-130">作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="32beb-130">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="32beb-131">データベース コンテキストと [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。</span><span class="sxs-lookup"><span data-stu-id="32beb-131">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="32beb-132">ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。</span><span class="sxs-lookup"><span data-stu-id="32beb-132">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="32beb-133">アプリを実行し、**[MVC Movie]\(MVC ムービー\)** リンクをクリックすると、次のようなエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="32beb-133">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="32beb-134">データベースを作成する必要があり、それには EF Core [移行](xref:data/ef-mvc/migrations)機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="32beb-134">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="32beb-135">移行では、データ モデルに一致するデータベースを作成し、データ モデルの変更時にデータベース スキーマを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="32beb-135">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="32beb-136">EF ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="32beb-136">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="32beb-137">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="32beb-137">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="32beb-138">Entity Framework Core のツール パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="32beb-138">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="32beb-139">このパッケージは移行を追加し、データベースを更新するために必要です。</span><span class="sxs-lookup"><span data-stu-id="32beb-139">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="32beb-140">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="32beb-140">Add an initial migration.</span></span>
* <span data-ttu-id="32beb-141">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="32beb-141">Update the database with the initial migration.</span></span>

<span data-ttu-id="32beb-142">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="32beb-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC メニュー](adding-model/_static/pmc.png)

<span data-ttu-id="32beb-144">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="32beb-144">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="32beb-145">**注:** `Install-Package` コマンドでエラーが発生した場合は、NuGet パッケージ マネージャーを開き、`Microsoft.EntityFrameworkCore.Tools` パッケージを検索してください。</span><span class="sxs-lookup"><span data-stu-id="32beb-145">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="32beb-146">これにより、パッケージをインストールしたり、パッケージが既にインストールされているかどうかを確認したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="32beb-146">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="32beb-147">または、PMC で問題がある場合、[CLI を使用したアプローチ](#cli)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="32beb-147">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="32beb-148">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="32beb-148">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="32beb-149">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Data/MvcMovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="32beb-149">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="32beb-150">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="32beb-150">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="32beb-151">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="32beb-151">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="32beb-152">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="32beb-152">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="32beb-153">`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_Initial.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="32beb-153">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="32beb-154">前述の手順は、PMC ではなく、コマンド ライン インターフェイス (CLI) を使用して実行できます。</span><span class="sxs-lookup"><span data-stu-id="32beb-154">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="32beb-155">[EF Core ツール](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)を *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="32beb-155">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="32beb-156">(プロジェクト ディレクトリの) コンソールから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="32beb-156">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  
  <span data-ttu-id="32beb-157">アプリを実行してエラーが表示される場合:</span><span class="sxs-lookup"><span data-stu-id="32beb-157">If you run the app and get the error:</span></span>
  
  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="32beb-158">` dotnet ef database update` を実行していない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="32beb-158">You probably have not run ` dotnet ef database update`.</span></span>
  
[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model4.md)]

![Intellisense のコンテキスト メニュー (Model アイテムの ID、価格、リリース日、およびタイトル プロパティ)](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="32beb-160">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="32beb-160">Additional resources</span></span>

* [<span data-ttu-id="32beb-161">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="32beb-161">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="32beb-162">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="32beb-162">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="32beb-163">[前のチュートリアル ビューの追加](adding-view.md)
> [次のチュートリアル SQL の使用](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="32beb-163">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
