---
title: "ASP.NET Core MVC アプリへのモデルの追加"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
ms.author: riande
manager: wpickett
ms.date: 12/8/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: c2cd3cc81221c146dec70e487a17b33360eb6112
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="22821-103">注: ASP.NET Core 2.0 テンプレートには、*Models* フォルダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="22821-103">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="22821-104">*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="22821-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="22821-105">クラスに **Movie** という名前を付けて、次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="22821-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="22821-106">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="22821-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="22821-107">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="22821-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="22821-108">これで、**M**VC アプリに、**モ**デルがあることになります。</span><span class="sxs-lookup"><span data-stu-id="22821-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="22821-109">コントローラーのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="22821-109">Scaffolding a controller</span></span>

<span data-ttu-id="22821-110">**ソリューション エクスプローラー**で、*Controllers* フォルダーを右クリックし、**[追加]、[コントローラー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="22821-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![前述の手順を参照](adding-model/_static/add_controller.png)

<span data-ttu-id="22821-112">**[MVC 依存関係の追加]** ダイアログ ボックスが表示された場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="22821-112">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="22821-113">[Visual Studio を最新バージョンに更新します](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="22821-113">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="22821-114">15.5 より前のバージョンの Visual Studio の場合はこのダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="22821-114">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="22821-115">更新できない場合は、**[追加]** を選択してから、もう一度コントローラーの追加手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="22821-115">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="22821-116">**[スキャフォールディングを追加]** ダイアログで、**[Entity Framework を使用したビューがある MVC コントローラー]、[追加]** の順にタップします。</span><span class="sxs-lookup"><span data-stu-id="22821-116">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[スキャフォールディングを追加] ダイアログ](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="22821-118">**[コントローラーの追加]** ダイアログ ボックスを完了します。</span><span class="sxs-lookup"><span data-stu-id="22821-118">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="22821-119">**[Model class]\(モデル クラス\):** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="22821-119">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="22821-120">**[Data context class]\(データ コンテキスト クラス\):****+** アイコンを選択し、既定の **MvcMovie.Models.MvcMovieContext** を追加します。</span><span class="sxs-lookup"><span data-stu-id="22821-120">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![[データの追加] コンテキスト](adding-model/_static/dc.png)

* <span data-ttu-id="22821-122">**ビュー:** 各オプションの既定値をオンにします。</span><span class="sxs-lookup"><span data-stu-id="22821-122">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="22821-123">**コントローラー名:** 既定の *MoviesController* のままにします。</span><span class="sxs-lookup"><span data-stu-id="22821-123">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="22821-124">**[追加]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="22821-124">Tap **Add**</span></span>

![[コントローラーの追加] ダイアログ](adding-model/_static/add_controller2.png)

<span data-ttu-id="22821-126">Visual Studio では、次が作成されます。</span><span class="sxs-lookup"><span data-stu-id="22821-126">Visual Studio creates:</span></span>

* <span data-ttu-id="22821-127">Entity Framework Core の[データベース コンテキスト クラス](xref:data/ef-mvc/intro#create-the-database-context)(*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="22821-127">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="22821-128">ムービー コントローラー (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="22821-128">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="22821-129">作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="22821-129">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="22821-130">データベース コンテキストと [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。</span><span class="sxs-lookup"><span data-stu-id="22821-130">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="22821-131">ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。</span><span class="sxs-lookup"><span data-stu-id="22821-131">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="22821-132">アプリを実行し、**[MVC Movie]\(MVC ムービー\)** リンクをクリックすると、次のようなエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="22821-132">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="22821-133">データベースを作成する必要があり、それには EF Core [移行](xref:data/ef-mvc/migrations)機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="22821-133">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="22821-134">移行では、データ モデルに一致するデータベースを作成し、データ モデルの変更時にデータベース スキーマを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="22821-134">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="22821-135">EF ツールの追加と初期移行の実行</span><span class="sxs-lookup"><span data-stu-id="22821-135">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="22821-136">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、次の作業を行います。</span><span class="sxs-lookup"><span data-stu-id="22821-136">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="22821-137">Entity Framework Core のツール パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="22821-137">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="22821-138">このパッケージは移行を追加し、データベースを更新するために必要です。</span><span class="sxs-lookup"><span data-stu-id="22821-138">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="22821-139">初期移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="22821-139">Add an initial migration.</span></span>
* <span data-ttu-id="22821-140">初期移行でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="22821-140">Update the database with the initial migration.</span></span>

<span data-ttu-id="22821-141">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="22821-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC メニュー](adding-model/_static/pmc.png)

<span data-ttu-id="22821-143">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="22821-143">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="22821-144">**注:** `Install-Package` コマンドでエラーが発生した場合は、NuGet パッケージ マネージャーを開き、`Microsoft.EntityFrameworkCore.Tools` パッケージを検索してください。</span><span class="sxs-lookup"><span data-stu-id="22821-144">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="22821-145">これにより、パッケージをインストールしたり、パッケージが既にインストールされているかどうかを確認したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="22821-145">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="22821-146">または、PMC で問題がある場合、[CLI を使用したアプローチ](#cli)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="22821-146">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="22821-147">`Add-Migration` コマンドによって最初のデータベース スキーマを作成するコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="22821-147">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="22821-148">このスキーマは、`DbContext` で指定されたモデルに基づきます (*Data/MvcMovieContext.cs* ファイル内)。</span><span class="sxs-lookup"><span data-stu-id="22821-148">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="22821-149">`Initial` 引数は移行の命名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="22821-149">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="22821-150">任意の名前を使用できますが、慣例により、移行を説明する名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="22821-150">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="22821-151">詳細については、「[Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations)」 (移行の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="22821-151">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="22821-152">`Update-Database` コマンドは、データベースを作成する、*Migrations/\<time-stamp>_Initial.cs* ファイルの `Up` メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22821-152">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="22821-153">前述の手順は、PMC ではなく、コマンド ライン インターフェイス (CLI) を使用して実行できます。</span><span class="sxs-lookup"><span data-stu-id="22821-153">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="22821-154">[EF Core ツール](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations)を *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="22821-154">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="22821-155">(プロジェクト ディレクトリの) コンソールから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22821-155">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Intellisense のコンテキスト メニュー (Model アイテムの ID、価格、リリース日、およびタイトル プロパティ)](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="22821-157">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="22821-157">Additional resources</span></span>

* [<span data-ttu-id="22821-158">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="22821-158">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="22821-159">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="22821-159">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="22821-160">[前のチュートリアル ビューの追加](adding-view.md)
[次のチュートリアル SQL の使用](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="22821-160">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
