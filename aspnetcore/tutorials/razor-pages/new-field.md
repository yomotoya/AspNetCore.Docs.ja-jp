---
title: ASP.NET Core で Razor ページに新しいファイルを追加する
author: rick-anderson
description: Entity Framework Core を使用した Razor ページへの新しいフィールドの追加方法
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 98de39c63c992dce7d60563df316d848339b811a
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410376"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="ca977-103">ASP.NET Core で Razor ページに新しいファイルを追加する</span><span class="sxs-lookup"><span data-stu-id="ca977-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="ca977-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca977-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="ca977-105">このセクションでは [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations を次の目的で使用します。</span><span class="sxs-lookup"><span data-stu-id="ca977-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="ca977-106">モデルに新しいフィールドを追加する。</span><span class="sxs-lookup"><span data-stu-id="ca977-106">Add a new field to the model.</span></span>
* <span data-ttu-id="ca977-107">新しいフィールド スキーマの変更をデータベースに移行する。</span><span class="sxs-lookup"><span data-stu-id="ca977-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="ca977-108">EF Code First を使用してデータベースを自動的に作成する場合、Code First では次が実行されます。</span><span class="sxs-lookup"><span data-stu-id="ca977-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="ca977-109">テーブルをデータベースに追加し、データベースのスキーマが生成元のモデル クラスと同期しているかを追跡します。</span><span class="sxs-lookup"><span data-stu-id="ca977-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="ca977-110">モデル クラスがデータベースと同期されていない場合、EF は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="ca977-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="ca977-111">同期中のスキーマ/モデルが自動的に検証されるようにすると、整合性のないデータベース/コードの問題を発見しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="ca977-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="ca977-112">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="ca977-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="ca977-113">*Models/Movie.cs* ファイルを開き、`Rating` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="ca977-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="ca977-114">アプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="ca977-114">Build the app.</span></span>

<span data-ttu-id="ca977-115">*Pages/Movies/Index.cshtml* を編集し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ca977-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="ca977-116">次のページを更新します。</span><span class="sxs-lookup"><span data-stu-id="ca977-116">Update the following pages:</span></span>

* <span data-ttu-id="ca977-117">[削除] と [詳細] ページに、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ca977-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="ca977-118">[Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) を `Rating` フィールドを使用して更新します。</span><span class="sxs-lookup"><span data-stu-id="ca977-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="ca977-119">[編集] ページに、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ca977-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="ca977-120">DB を更新して新しいフィールドが含まれるようになるまでアプリは動作しません。</span><span class="sxs-lookup"><span data-stu-id="ca977-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="ca977-121">ここで実行すると、アプリによって `SqlException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="ca977-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="ca977-122">このエラーが表示されるのは、更新された Movie モデル クラスがデータベースの Movie テーブルのスキーマと異なるためです。</span><span class="sxs-lookup"><span data-stu-id="ca977-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="ca977-123">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="ca977-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="ca977-124">このエラーを解決するための手法がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="ca977-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="ca977-125">Entity Framework に、新しいモデル クラス スキーマを使用してデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="ca977-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="ca977-126">この手法は、開発周期の早い段階で便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。</span><span class="sxs-lookup"><span data-stu-id="ca977-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="ca977-127">これの欠点は、データベースで既存のデータが失われることです。</span><span class="sxs-lookup"><span data-stu-id="ca977-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="ca977-128">実稼働データベースでこの方法は使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="ca977-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="ca977-129">スキーマの変更時に DB を削除し、初期化子を使用して、データベースにテスト データを自動的にシードすることは、多くの場合、アプリケーションを開発する上で有益な方法となります。</span><span class="sxs-lookup"><span data-stu-id="ca977-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="ca977-130">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="ca977-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="ca977-131">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="ca977-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="ca977-132">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="ca977-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="ca977-133">Code First Migrations を使用して、データベース スキーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="ca977-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="ca977-134">このチュートリアルでは、Code First Migrations を利用します。</span><span class="sxs-lookup"><span data-stu-id="ca977-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="ca977-135">新しい列に値を提供するように、`SeedData` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="ca977-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="ca977-136">下に変更のサンプルがありますが、`new Movie` ブロックごとにこの変更を行ってください。</span><span class="sxs-lookup"><span data-stu-id="ca977-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="ca977-137">[完成した SeedData.cs ファイル](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca977-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="ca977-138">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="ca977-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ca977-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca977-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="ca977-140">評価フィールドの移行を追加する</span><span class="sxs-lookup"><span data-stu-id="ca977-140">Add a migration for the rating field</span></span>

<span data-ttu-id="ca977-141">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="ca977-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="ca977-142">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="ca977-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="ca977-143">`Add-Migration` コマンドにより、フレームワークに次の指示があります。</span><span class="sxs-lookup"><span data-stu-id="ca977-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="ca977-144">`Movie` モデルを、`Movie` DB のスキーマと比較します。</span><span class="sxs-lookup"><span data-stu-id="ca977-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="ca977-145">DB スキーマを新しいモデルに移行するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="ca977-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="ca977-146">"Rating (評価)" という名前は任意です。移行ファイルに名前を付けるために利用されます。</span><span class="sxs-lookup"><span data-stu-id="ca977-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="ca977-147">移行ファイルには意味のある名前を使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="ca977-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="ca977-148">フレームワークは、データベースにスキーマ変更を適用するように、`Update-Database` コマンドから指示されます。</span><span class="sxs-lookup"><span data-stu-id="ca977-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="ca977-149">DB 内のすべてのレコードを削除すると、初期化子は DB にデータを初期投入し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ca977-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="ca977-150">これはブラウザーの削除リンクで行うか、[Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX) から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="ca977-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="ca977-151">別のオプションとしては、データベースを削除してから、移行を使用することで、データベースを再作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="ca977-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="ca977-152">SSOX 内でデータベースを削除するには: </span><span class="sxs-lookup"><span data-stu-id="ca977-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="ca977-153">SSOX でデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="ca977-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="ca977-154">データベースを右クリックし、*[削除]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="ca977-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="ca977-155">**[既存の接続を閉じる]** をチェックします。</span><span class="sxs-lookup"><span data-stu-id="ca977-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="ca977-156">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ca977-156">Select **OK**.</span></span>
* <span data-ttu-id="ca977-157">[PMC](xref:tutorials/razor-pages/new-field#pmc) でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="ca977-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="ca977-158">Visual Studio Code/Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ca977-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

<span data-ttu-id="ca977-159">次の .NET Core CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ca977-159">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<span data-ttu-id="ca977-160">`ef migrations add` コマンドにより、フレームワークに次の指示があります。</span><span class="sxs-lookup"><span data-stu-id="ca977-160">The `ef migrations add` command tells the framework to:</span></span>

* <span data-ttu-id="ca977-161">`Movie` モデルを、`Movie` DB のスキーマと比較します。</span><span class="sxs-lookup"><span data-stu-id="ca977-161">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="ca977-162">DB スキーマを新しいモデルに移行するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="ca977-162">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="ca977-163">"Rating (評価)" という名前は任意です。移行ファイルに名前を付けるために利用されます。</span><span class="sxs-lookup"><span data-stu-id="ca977-163">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="ca977-164">移行ファイルには意味のある名前を使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="ca977-164">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="ca977-165">フレームワークは、データベースにスキーマ変更を適用するように、`ef database update` コマンドから指示されます。</span><span class="sxs-lookup"><span data-stu-id="ca977-165">The `ef database update` command tells the framework to apply the schema changes to the database.</span></span>

<span data-ttu-id="ca977-166">DB 内のすべてのレコードを削除すると、初期化子は DB にデータを初期投入し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ca977-166">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="ca977-167">これを行うには、ブラウザー内の削除リンクを使用することも、SQLite ツールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ca977-167">You can do this with the delete links in the browser or by using a SQLite tool.</span></span>

<span data-ttu-id="ca977-168">別のオプションとしては、データベースを削除してから、移行を使用することで、データベースを再作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="ca977-168">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="ca977-169">データベースを削除するには、データベース ファイル (*MvcMovie.db*) を削除します。</span><span class="sxs-lookup"><span data-stu-id="ca977-169">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="ca977-170">次に、`ef database update` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ca977-170">Then run the `ef database update` command:</span></span> 

```console
dotnet ef database update
```

> [!NOTE]
> <span data-ttu-id="ca977-171">スキーマ変更操作の多くは、EF Core の SQLite プロバイダーによってサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="ca977-171">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="ca977-172">たとえば、列の追加はサポートされていますが、列の削除はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="ca977-172">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="ca977-173">移行を追加することで列を削除する場合、`ef migrations add` コマンドは成功しますが、`ef database update` コマンドは失敗します。</span><span class="sxs-lookup"><span data-stu-id="ca977-173">If you add a migration to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="ca977-174">制限事項の一部を解決するには、手動で移行コードを記述してテーブルのリビルドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ca977-174">You can work around some of the limitations by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="ca977-175">テーブルのリビルドには、既存のテーブルの名前変更、新しいテーブルの作成、新しいテーブルへのデータのコピー、および古いテーブルの削除が必要です。</span><span class="sxs-lookup"><span data-stu-id="ca977-175">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="ca977-176">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca977-176">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="ca977-177">SQLite EF Core データベース プロバイダーの制限事項</span><span class="sxs-lookup"><span data-stu-id="ca977-177">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="ca977-178">移行コードをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="ca977-178">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="ca977-179">データのシード処理</span><span class="sxs-lookup"><span data-stu-id="ca977-179">Data seeding</span></span>](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

<span data-ttu-id="ca977-180">アプリを実行し、`Rating` フィールドでムービーを作成、編集、表示できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ca977-180">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="ca977-181">データベースがシード処理されない場合は、`SeedData.Initialize` メソッドでブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="ca977-181">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ca977-182">[前へ:検索の追加](xref:tutorials/razor-pages/search)
> [次へ: 検証の追加](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="ca977-182">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
