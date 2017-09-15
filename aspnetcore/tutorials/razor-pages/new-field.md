---
title: "Razor ページへの新しいフィールドの追加"
author: rick-anderson
description: "Entity Framework Core を使用した Razor ページへの新しいフィールドの追加方法"
keywords: "ASP.NET Core,Entity Framework Core,移行"
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: bda00f290043251ad308192c5b1a873ae7cd0d85
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="2ef7a-104">Razor ページへの新しいフィールドの追加</span><span class="sxs-lookup"><span data-stu-id="2ef7a-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="2ef7a-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ef7a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2ef7a-106">このセクションでは、[Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First Migrations を利用し、新しいフィールドをモデルに追加し、その変更をデータベースに移行します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-106">In this section you'll use [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="2ef7a-107">EF Code First を利用してデータベースを自動作成すると、Code First はテーブルをデータベースに追加し、データベースのスキーマが生成元のモデル クラスと同期しているか追跡します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="2ef7a-108">同期していない場合、EF は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="2ef7a-109">一貫性のないデータベース/コードの問題を簡単に見つけられます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="2ef7a-110">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="2ef7a-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="2ef7a-111">*Models/Movie.cs* ファイルを開き、`Rating` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

<span data-ttu-id="2ef7a-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="2ef7a-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

<span data-ttu-id="2ef7a-113">アプリをビルドします (Ctrl+Shift+B)。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="2ef7a-114">*Pages/Movies/Index.cshtml* を編集し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-114">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

<span data-ttu-id="2ef7a-115">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span><span class="sxs-lookup"><span data-stu-id="2ef7a-115">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span></span>

<span data-ttu-id="2ef7a-116">[削除] と [詳細] ページに、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="2ef7a-117">*Create.cshtml* を `Rating` フィールドを使用して更新します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="2ef7a-118">前の `<div>` 要素をコピー/貼り付けし、intelliSense にフィールドを更新させることができます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="2ef7a-119">IntelliSense は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)と連動します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![開発者は、ビューの 2 番目のラベル要素で、asp-for の属性値に文字 R を入力しました。](new-field/_static/cr.png)

<span data-ttu-id="2ef7a-123">次に、`Rating` フィールドがある *Create.cshtml* のコードを示します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

<span data-ttu-id="2ef7a-124">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span><span class="sxs-lookup"><span data-stu-id="2ef7a-124">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span></span>

<span data-ttu-id="2ef7a-125">[編集] ページに、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-125">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="2ef7a-126">DB を更新して新しいフィールドが含まれるようになるまでアプリは動作しません。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-126">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="2ef7a-127">ここで実行すると、アプリによって `SqlException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-127">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="2ef7a-128">このエラーが表示されるのは、更新された Movie モデル クラスがデータベースの Movie テーブルのスキーマと異なるためです。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-128">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="2ef7a-129">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-129">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="2ef7a-130">このエラーを解決するための手法がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="2ef7a-131">Entity Framework に、新しいモデル クラス スキーマを使用してデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-131">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="2ef7a-132">この手法は、開発周期の早い段階で便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-132">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="2ef7a-133">これの欠点は、データベースで既存のデータが失われることです。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-133">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="2ef7a-134">実稼働データベースでこの方法は使用したくないでしょう。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-134">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="2ef7a-135">スキーマの変更時に DB を削除し、初期化子を使用して、データベースにテスト データを自動的にシードすることは、多くの場合、アプリケーションを開発する上で有益な方法となります。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-135">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="2ef7a-136">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-136">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="2ef7a-137">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-137">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="2ef7a-138">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-138">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="2ef7a-139">Code First Migrations を使用して、データベース スキーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-139">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="2ef7a-140">このチュートリアルでは、Code First Migrations を利用します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-140">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="2ef7a-141">新しい列に値を提供するように、`SeedData` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-141">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="2ef7a-142">下に変更のサンプルがありますが、`new Movie` ブロックごとにこの変更を行ってください。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-142">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

<span data-ttu-id="2ef7a-143">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="2ef7a-143">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span></span>

<span data-ttu-id="2ef7a-144">[完成した SeedData.cs ファイル](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-144">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="2ef7a-145">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-145">Build the solution.</span></span>

<a name="pmc"></a>

<span data-ttu-id="2ef7a-146">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="2ef7a-147">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-147">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Rating
Update-Database
```

<span data-ttu-id="2ef7a-148">`Add-Migration` コマンドにより、フレームワークに次の指示があります。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-148">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="2ef7a-149">`Movie` モデルを、`Movie` DB のスキーマと比較します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-149">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="2ef7a-150">DB スキーマを新しいモデルに移行するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-150">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="2ef7a-151">"Rating (評価)" という名前は任意です。移行ファイルに名前を付けるために利用されます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-151">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="2ef7a-152">移行ファイルには意味のある名前を使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-152">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="2ef7a-153"><a name="ssox"></a> DB 内のすべてのレコードを削除すると、初期化子は DB にデータを初期投入し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-153"><a name="ssox"></a> If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="2ef7a-154">これはブラウザーの削除リンクで行うか、[Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX) から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-154">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="2ef7a-155">SSOX からデータベースを削除するには:</span><span class="sxs-lookup"><span data-stu-id="2ef7a-155">To delete the database from SSOX:</span></span>

* <span data-ttu-id="2ef7a-156">SSOX でデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-156">Select the database in SSOX.</span></span>
* <span data-ttu-id="2ef7a-157">データベースを右クリックし、*[削除]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-157">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="2ef7a-158">**[既存の接続を閉じる]* をチェックします。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-158">Check **Close existing connections*</span></span>
* <span data-ttu-id="2ef7a-159">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-159">Select **OK**</span></span>
* <span data-ttu-id="2ef7a-160">[PMC](xref:tutorials/razor-pages/new-field#pmc) でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-160">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database</span></span> 

    ```PMC
    Update-Database
    ```

<span data-ttu-id="2ef7a-161">アプリを実行し、`Rating` フィールドでムービーを作成、編集、表示できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-161">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="2ef7a-162">データベースがシードされていない場合は IIS Express を停止し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="2ef7a-162">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2ef7a-163">[前: 検索の追加](xref:tutorials/razor-pages/search)
[次: 新しいフィールドの追加](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="2ef7a-163">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
