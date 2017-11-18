---
title: "新しいフィールドの追加"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 16efbacf-fe7b-4b41-84b0-06a1574b95c2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 9c872a48aba4974ddac2e49ca40c944da356f0e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-new-field"></a><span data-ttu-id="d8705-103">新しいフィールドの追加</span><span class="sxs-lookup"><span data-stu-id="d8705-103">Adding a New Field</span></span>

<span data-ttu-id="d8705-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8705-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d8705-105">このセクションでは、[Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations を利用し、新しいフィールドをモデルに追加し、その変更をデータベースに移行します。</span><span class="sxs-lookup"><span data-stu-id="d8705-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="d8705-106">EF Code First を利用してデータベースを自動作成すると、Code First はテーブルをデータベースに追加し、データベースのスキーマが生成元のモデル クラスと同期しているか追跡します。</span><span class="sxs-lookup"><span data-stu-id="d8705-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="d8705-107">同期していない場合、EF は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="d8705-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="d8705-108">一貫性のないデータベース/コードの問題を簡単に見つけられます。</span><span class="sxs-lookup"><span data-stu-id="d8705-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="d8705-109">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="d8705-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="d8705-110">*Models/Movie.cs* ファイルを開き、`Rating` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="d8705-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="d8705-111">アプリをビルドします (Ctrl+Shift+B)。</span><span class="sxs-lookup"><span data-stu-id="d8705-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="d8705-112">新しいフィールドを `Movie` クラスに追加したので、この新しいプロパティが含まれるように、拘束力のあるホワイト リストを更新する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="d8705-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="d8705-113">*MoviesController.cs* で、アクション メソッドの `Create` と `Edit` の両方の `[Bind]` 属性を更新し、`Rating` プロパティが含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="d8705-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="d8705-114">ブラウザー ビューで新しい `Rating` プロパティを表示、作成、編集する目的でビュー テンプレートを更新する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="d8705-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="d8705-115">*/Views/Movies/Index.cshtml* ファイルを編集し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d8705-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="d8705-116">*/Views/Movies/Create.cshtml* を `Rating` フィールドで更新します。</span><span class="sxs-lookup"><span data-stu-id="d8705-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="d8705-117">前の "form group" をコピー/貼り付けし、intelliSense にフィールドを更新させることができます。</span><span class="sxs-lookup"><span data-stu-id="d8705-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="d8705-118">IntelliSense は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)と連動します。</span><span class="sxs-lookup"><span data-stu-id="d8705-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="d8705-119">注: RTM バージョンの Visual Studio 2017 では、Razor intelliSense の [Razor 言語サービス](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices)をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8705-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="d8705-120">これは次のリリースで修正されます。</span><span class="sxs-lookup"><span data-stu-id="d8705-120">This will be fixed in the next release.</span></span>

![開発者は、ビューの 2 番目のラベル要素で、asp-for の属性値に文字 R を入力しました。](new-field/_static/cr.png)

<span data-ttu-id="d8705-124">DB を更新して新しいフィールドが含まれるようになるまでアプリは動作しません。</span><span class="sxs-lookup"><span data-stu-id="d8705-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="d8705-125">ここですぐに実行すると、次の `SqlException` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d8705-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="d8705-126">このエラーが表示されるのは、更新された Movie モデル クラスが既存のデータベースの Movie テーブルのスキーマと異なるためです </span><span class="sxs-lookup"><span data-stu-id="d8705-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="d8705-127">(データベース テーブルに [評価] 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="d8705-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="d8705-128">このエラーを解決するための手法がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="d8705-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="d8705-129">Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="d8705-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="d8705-130">この手法は、開発周期の早い段階で、テスト データベースで開発しているときに非常に便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。</span><span class="sxs-lookup"><span data-stu-id="d8705-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="d8705-131">ただし、欠点もあり、データベースの既存データが失われます。本稼働データベースではこの手法は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="d8705-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="d8705-132">初期化子を利用し、データベースにテスト データを自動的に初期投入します。多くの場合、アプリケーション開発の手法として有益な方法です。</span><span class="sxs-lookup"><span data-stu-id="d8705-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="d8705-133">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="d8705-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="d8705-134">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="d8705-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="d8705-135">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="d8705-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="d8705-136">Code First Migrations を使用して、データベース スキーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="d8705-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="d8705-137">このチュートリアルでは、Code First Migrations を利用します。</span><span class="sxs-lookup"><span data-stu-id="d8705-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="d8705-138">新しい列に値を提供するように、`SeedData` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="d8705-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="d8705-139">下に変更のサンプルがありますが、`new Movie` ごとにこの変更を行ってください。</span><span class="sxs-lookup"><span data-stu-id="d8705-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="d8705-140">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="d8705-140">Build the solution.</span></span>

<span data-ttu-id="d8705-141">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d8705-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC メニュー](adding-model/_static/pmc.png)

<span data-ttu-id="d8705-143">PMC で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d8705-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="d8705-144">`Add-Migration` コマンドは移行フレームワークに現在の `Movie` モデルを現在の `Movie` DB スキーマで調べ、DB を新しいモデルに移行するために必要なコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8705-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="d8705-145">"Rating (評価)" という名前は任意です。移行ファイルに名前を付けるために利用されます。</span><span class="sxs-lookup"><span data-stu-id="d8705-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="d8705-146">移行ファイルには意味のある名前を使用すると便利です。</span><span class="sxs-lookup"><span data-stu-id="d8705-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="d8705-147">DB 内のすべてのレコードを削除すると、初期化子は DB にデータを初期投入し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d8705-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="d8705-148">これはブラウザーの削除リンクで行うか、SSOX から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="d8705-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="d8705-149">アプリを実行し、`Rating` フィールドでムービーを作成、編集、表示できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d8705-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="d8705-150">また、`Rating` フィールドはビュー テンプレートの `Edit`、`Details`、`Delete` にも追加してください。</span><span class="sxs-lookup"><span data-stu-id="d8705-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d8705-151">[前へ](search.md)
[次へ](validation.md)</span><span class="sxs-lookup"><span data-stu-id="d8705-151">[Previous](search.md)
[Next](validation.md)</span></span>  
