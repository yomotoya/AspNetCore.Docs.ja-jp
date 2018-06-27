---
title: ASP.NET Core で Razor ページに新しいファイルを追加する
author: rick-anderson
description: Entity Framework Core を使用した Razor ページへの新しいフィールドの追加方法
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 926091a7643bbe584316677cbaae6574471c47f8
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582711"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="71c4e-103">ASP.NET Core で Razor ページに新しいファイルを追加する</span><span class="sxs-lookup"><span data-stu-id="71c4e-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="71c4e-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71c4e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71c4e-105">このセクションでは、[Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations を利用し、新しいフィールドをモデルに追加し、その変更をデータベースに移行します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="71c4e-106">EF Code First を使用してデータベースを自動的に作成する場合、Code First では次が実行されます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="71c4e-107">テーブルをデータベースに追加し、データベースのスキーマが生成元のモデル クラスと同期しているかを追跡します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="71c4e-108">モデル クラスがデータベースと同期されていない場合、EF は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="71c4e-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="71c4e-109">同期中のスキーマ/モデルが自動的に検証されるようにすると、整合性のないデータベース/コードの問題を発見しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="71c4e-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="71c4e-110">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="71c4e-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="71c4e-111">*Models/Movie.cs* ファイルを開き、`Rating` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="71c4e-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="71c4e-112">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="71c4e-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="71c4e-113">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>

::: moniker-end

<span data-ttu-id="71c4e-114">アプリをビルドします (Ctrl+Shift+B)。</span><span class="sxs-lookup"><span data-stu-id="71c4e-114">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="71c4e-115">*Pages/Movies/Index.cshtml* を編集し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="71c4e-116">[削除] と [詳細] ページに、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="71c4e-117">*Create.cshtml* を `Rating` フィールドを使用して更新します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="71c4e-118">前の `<div>` 要素をコピー/貼り付けし、intelliSense にフィールドを更新させることができます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="71c4e-119">IntelliSense は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)と連動します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![開発者は、ビューの 2 番目のラベル要素で、asp-for の属性値に文字 R を入力しました。](new-field/_static/cr.png)

<span data-ttu-id="71c4e-123">次に、`Rating` フィールドがある *Create.cshtml* のコードを示します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="71c4e-124">[編集] ページに、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-124">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="71c4e-125">DB を更新して新しいフィールドが含まれるようになるまでアプリは動作しません。</span><span class="sxs-lookup"><span data-stu-id="71c4e-125">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="71c4e-126">ここで実行すると、アプリによって `SqlException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-126">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="71c4e-127">このエラーが表示されるのは、更新された Movie モデル クラスがデータベースの Movie テーブルのスキーマと異なるためです。</span><span class="sxs-lookup"><span data-stu-id="71c4e-127">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="71c4e-128">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="71c4e-128">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="71c4e-129">このエラーを解決するための手法がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="71c4e-129">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="71c4e-130">Entity Framework に、新しいモデル クラス スキーマを使用してデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-130">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="71c4e-131">この手法は、開発周期の早い段階で便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-131">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="71c4e-132">これの欠点は、データベースで既存のデータが失われることです。</span><span class="sxs-lookup"><span data-stu-id="71c4e-132">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="71c4e-133">実稼働データベースでこの方法は使用したくないでしょう。</span><span class="sxs-lookup"><span data-stu-id="71c4e-133">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="71c4e-134">スキーマの変更時に DB を削除し、初期化子を使用して、データベースにテスト データを自動的にシードすることは、多くの場合、アプリケーションを開発する上で有益な方法となります。</span><span class="sxs-lookup"><span data-stu-id="71c4e-134">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="71c4e-135">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="71c4e-136">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="71c4e-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="71c4e-137">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="71c4e-138">Code First Migrations を使用して、データベース スキーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="71c4e-139">このチュートリアルでは、Code First Migrations を利用します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-139">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="71c4e-140">新しい列に値を提供するように、`SeedData` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="71c4e-141">下に変更のサンプルがありますが、`new Movie` ブロックごとにこの変更を行ってください。</span><span class="sxs-lookup"><span data-stu-id="71c4e-141">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="71c4e-142">[完成した SeedData.cs ファイル](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="71c4e-142">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="71c4e-143">[完成した SeedData.cs ファイル](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="71c4e-143">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>
::: moniker-end

<span data-ttu-id="71c4e-144">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="71c4e-144">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="71c4e-145">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="71c4e-146">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-146">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="71c4e-147">`Add-Migration` コマンドにより、フレームワークに次の指示があります。</span><span class="sxs-lookup"><span data-stu-id="71c4e-147">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="71c4e-148">`Movie` モデルを、`Movie` DB のスキーマと比較します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-148">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="71c4e-149">DB スキーマを新しいモデルに移行するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-149">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="71c4e-150">"Rating (評価)" という名前は任意です。移行ファイルに名前を付けるために利用されます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-150">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="71c4e-151">移行ファイルには意味のある名前を使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="71c4e-151">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="71c4e-152">DB 内のすべてのレコードを削除すると、初期化子は DB にデータを初期投入し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-152">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="71c4e-153">これはブラウザーの削除リンクで行うか、[Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX) から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="71c4e-153">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="71c4e-154">SSOX からデータベースを削除するには:</span><span class="sxs-lookup"><span data-stu-id="71c4e-154">To delete the database from SSOX:</span></span>

* <span data-ttu-id="71c4e-155">SSOX でデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-155">Select the database in SSOX.</span></span>
* <span data-ttu-id="71c4e-156">データベースを右クリックし、*[削除]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-156">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="71c4e-157">**[既存の接続を閉じる]** をチェックします。</span><span class="sxs-lookup"><span data-stu-id="71c4e-157">Check **Close existing connections**.</span></span>
* <span data-ttu-id="71c4e-158">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-158">Select **OK**.</span></span>
* <span data-ttu-id="71c4e-159">[PMC](xref:tutorials/razor-pages/new-field#pmc) でデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-159">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="71c4e-160">アプリを実行し、`Rating` フィールドでムービーを作成、編集、表示できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-160">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="71c4e-161">データベースがシードされていない場合は IIS Express を停止し、アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="71c4e-161">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71c4e-162">[前: 検索の追加](xref:tutorials/razor-pages/search)
> [次: 検証の追加](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="71c4e-162">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
