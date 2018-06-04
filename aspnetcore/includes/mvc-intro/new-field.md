<!-- This include not used by windows version -->
# <a name="adding-a-new-field"></a><span data-ttu-id="9fafc-101">新しいフィールドの追加</span><span class="sxs-lookup"><span data-stu-id="9fafc-101">Adding a new field</span></span>

<span data-ttu-id="9fafc-102">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9fafc-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9fafc-103">このチュートリアルでは、`Movies` テーブルに新しいフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="9fafc-104">ここでは、スキーマを変更する (新しいフィールドを追加する) 際にデータベースをドロップし、新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="9fafc-105">このワークフローは、保存する実稼働データがない開発の早い段階に適しています。</span><span class="sxs-lookup"><span data-stu-id="9fafc-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="9fafc-106">アプリが配置され、保存する必要があるデータが存在する状態で、スキーマを変更する必要がある場合は DB をドロップすることはできません。</span><span class="sxs-lookup"><span data-stu-id="9fafc-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="9fafc-107">Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) では、スキーマを更新し、データを失うことなくデータベースを移行できます。</span><span class="sxs-lookup"><span data-stu-id="9fafc-107">Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="9fafc-108">Migrations は SQL Server でよく使用される機能ですが、SQLlite では多くの移行スキーマ操作がサポートされないため、実行できるのはごく簡単な移行のみとなります。</span><span class="sxs-lookup"><span data-stu-id="9fafc-108">Migrations is a popular feature when using SQL Server, but SQLlite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="9fafc-109">詳細については、[SQLite の制限事項](/ef/core/providers/sqlite/limitations)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9fafc-109">See [SQLite Limitations](/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="9fafc-110">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="9fafc-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="9fafc-111">*Models/Movie.cs* ファイルを開き、`Rating` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="9fafc-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="9fafc-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="9fafc-113">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="9fafc-113">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>
::: moniker-end

<span data-ttu-id="9fafc-114">新しいフィールドを `Movie` クラスに追加したため、この新しいプロパティが含まれるように、拘束力のあるホワイトリストを更新する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="9fafc-114">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="9fafc-115">*MoviesController.cs* で、アクション メソッドの `Create` と `Edit` の両方の `[Bind]` 属性を更新し、`Rating` プロパティが含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="9fafc-115">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="9fafc-116">ブラウザー ビューで新しい `Rating` プロパティを表示、作成、編集するためにビュー テンプレートを更新する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="9fafc-116">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="9fafc-117">*/Views/Movies/Index.cshtml* ファイルを編集し、`Rating` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-117">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="9fafc-118">*/Views/Movies/Create.cshtml* を `Rating` フィールドで更新します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-118">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="9fafc-119">DB を更新して新しいフィールドが含まれるようになるまでアプリは動作しません。</span><span class="sxs-lookup"><span data-stu-id="9fafc-119">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="9fafc-120">ここですぐに実行すると、次の `SqliteException` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9fafc-120">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="9fafc-121">このエラーが表示されるのは、更新された Movie モデル クラスが既存のデータベースの Movie テーブルのスキーマと異なるためです </span><span class="sxs-lookup"><span data-stu-id="9fafc-121">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="9fafc-122">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="9fafc-122">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="9fafc-123">このエラーを解決するための手法がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="9fafc-123">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="9fafc-124">データベースをドロップし、Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的に再作成させます。</span><span class="sxs-lookup"><span data-stu-id="9fafc-124">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="9fafc-125">この手法では、データベース内の既存のデータが失われるため、実稼働データベースでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="9fafc-125">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="9fafc-126">初期化子を使用して、データベースにテスト データを自動的にシードすることは、多くの場合、アプリケーションを開発する上で有益な方法となります。</span><span class="sxs-lookup"><span data-stu-id="9fafc-126">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="9fafc-127">モデル クラスと一致するように、既存のデータベースのスキーマを手動で変更します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-127">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="9fafc-128">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="9fafc-128">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="9fafc-129">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="9fafc-129">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="9fafc-130">Code First Migrations を使用して、データベース スキーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-130">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="9fafc-131">このチュートリアルでは、スキーマの変更時にデータベースをドロップして再作成します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-131">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="9fafc-132">端末から次のコマンドを実行して、db をドロップします。</span><span class="sxs-lookup"><span data-stu-id="9fafc-132">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="9fafc-133">新しい列に値を提供するように、`SeedData` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-133">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="9fafc-134">下に変更のサンプルがありますが、`new Movie` ごとにこの変更を行ってください。</span><span class="sxs-lookup"><span data-stu-id="9fafc-134">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="9fafc-135">`Rating` フィールドを `Edit`、`Details`、および `Delete` ビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-135">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="9fafc-136">アプリを実行し、`Rating` フィールドでムービーを作成、編集、表示できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9fafc-136">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="9fafc-137">テンプレート。</span><span class="sxs-lookup"><span data-stu-id="9fafc-137">templates.</span></span>
