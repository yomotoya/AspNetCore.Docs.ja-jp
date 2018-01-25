---
title: "EF Core - と razor ページの読み取り関連データは、8 6"
author: rick-anderson
description: "このチュートリアルでは、読み取りおよびでナビゲーション プロパティに、Entity Framework が読み込まれるデータは、関連するデータを表示します。"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 532020a8fe4c5a0312cbd89278e61f614b1825f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="7293c-103">読み取りに関連したデータの EF コア Razor ページ (8 の 6)</span><span class="sxs-lookup"><span data-stu-id="7293c-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="7293c-104">によって[Tom Dykstra](https://github.com/tdykstra)、 [Jon P Smith](https://twitter.com/thereformedprog)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7293c-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="7293c-105">このチュートリアルでは、関連するデータが読み取られ、表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="7293c-106">関連するデータは、EF コアがナビゲーションのプロパティに読み込まれるデータです。</span><span class="sxs-lookup"><span data-stu-id="7293c-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="7293c-107">問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)です。</span><span class="sxs-lookup"><span data-stu-id="7293c-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="7293c-108">次の図は、このチュートリアルの完成したページを表示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-108">The following illustrations show the completed pages for this tutorial:</span></span>

![インデックス ページのコース](read-related-data/_static/courses-index.png)

![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="7293c-111">Eager、明示的なおよび関連するデータの読み込み遅延</span><span class="sxs-lookup"><span data-stu-id="7293c-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="7293c-112">いくつかの方法が EF コアが、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="7293c-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="7293c-113">[一括読み込み](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading)です。</span><span class="sxs-lookup"><span data-stu-id="7293c-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="7293c-114">一括読み込みは、エンティティの 1 つの種類のクエリも関連エンティティを読み込むときにです。</span><span class="sxs-lookup"><span data-stu-id="7293c-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="7293c-115">エンティティが読み込まれると、その関連データが取得されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="7293c-116">通常、この結果、すべてのために必要なデータを取得する 1 つの結合クエリ。</span><span class="sxs-lookup"><span data-stu-id="7293c-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="7293c-117">EF Core では、一括読み込みの種類によっては複数のクエリを発行します。</span><span class="sxs-lookup"><span data-stu-id="7293c-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="7293c-118">複数のクエリを発行することができます効率的である場合は、EF6 で一部のクエリの場合よりも 1 つのクエリがあった場合。</span><span class="sxs-lookup"><span data-stu-id="7293c-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="7293c-119">一括読み込みを指定した、`Include`と`ThenInclude`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="7293c-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![一括読み込みの例](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="7293c-121">一括読み込みでは、コレクション nvavigation が含まれるときに、複数のクエリが送信されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="7293c-122">メインのクエリに対して 1 つのクエリ</span><span class="sxs-lookup"><span data-stu-id="7293c-122">One query for the main query</span></span> 
 * <span data-ttu-id="7293c-123">「エッジ」ロード ツリー内にコレクションごとに 1 つのクエリ。</span><span class="sxs-lookup"><span data-stu-id="7293c-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="7293c-124">使用してクエリを区切る`Load`: データは、個別のクエリで取得できるまたは EF コア「修正プログラム」ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7293c-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="7293c-125">EF コアは、ナビゲーション プロパティを自動的に入力「を修正」を意味します。</span><span class="sxs-lookup"><span data-stu-id="7293c-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="7293c-126">使用してクエリを区切る`Load`明示的な一括読み込みよりを読み込むようにします。</span><span class="sxs-lookup"><span data-stu-id="7293c-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![別のクエリ例](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="7293c-128">注: EF コアをコンテキストのインスタンスに以前に読み込まれたその他のエンティティにナビゲーション プロパティを自動的に修正します。</span><span class="sxs-lookup"><span data-stu-id="7293c-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="7293c-129">ナビゲーション プロパティのデータが場合でも*いない*いくつかの場合は、プロパティを作成して明示的に含まれる、または以前に読み込まれたすべての関連するエンティティ。</span><span class="sxs-lookup"><span data-stu-id="7293c-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="7293c-130">[明示的な読み込み](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)です。</span><span class="sxs-lookup"><span data-stu-id="7293c-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="7293c-131">エンティティが読み込まれると、関連データが取得されていません。</span><span class="sxs-lookup"><span data-stu-id="7293c-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="7293c-132">必要なときに、関連するデータを取得するコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7293c-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="7293c-133">個別のクエリで明示的な読み込み、DB に送信される複数のクエリ結果になります。</span><span class="sxs-lookup"><span data-stu-id="7293c-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="7293c-134">明示的な読み込みでは、コードは、読み込まれるナビゲーションのプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="7293c-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="7293c-135">使用して、`Load`明示的な読み込みを行うメソッドです。</span><span class="sxs-lookup"><span data-stu-id="7293c-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="7293c-136">例:</span><span class="sxs-lookup"><span data-stu-id="7293c-136">For example:</span></span>

 ![明示的な読み込みの例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="7293c-138">[遅延読み込み](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading)です。</span><span class="sxs-lookup"><span data-stu-id="7293c-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="7293c-139">[EF Core では、遅延読み込みはサポートされていない](https://github.com/aspnet/EntityFrameworkCore/issues/3797)です。</span><span class="sxs-lookup"><span data-stu-id="7293c-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="7293c-140">エンティティが読み込まれると、関連データが取得されていません。</span><span class="sxs-lookup"><span data-stu-id="7293c-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="7293c-141">ナビゲーション プロパティがアクセスすると、最初にそのナビゲーション プロパティに必要なデータは自動的に取得されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="7293c-142">クエリは、毎回、ナビゲーション プロパティへのアクセスを初めて DB に送信されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="7293c-143">`Select`演算子が必要な関連するデータのみを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="7293c-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="7293c-144">部門名が表示されるコース ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="7293c-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="7293c-145">Course エンティティには含むナビゲーション プロパティが含まれています、`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="7293c-146">`Department`エンティティには、このコースに割り当てられている部門が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7293c-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="7293c-147">コースの一覧で、割り当てられている部門の名前を表示。</span><span class="sxs-lookup"><span data-stu-id="7293c-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="7293c-148">取得、`Name`プロパティから、`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="7293c-149">`Department`エンティティの取得元、`Course.Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7293c-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="7293c-151">Scaffold コース モデル</span><span class="sxs-lookup"><span data-stu-id="7293c-151">Scaffold the Course model</span></span>

* <span data-ttu-id="7293c-152">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="7293c-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="7293c-153">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="7293c-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7293c-154">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7293c-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="7293c-155">上記のコマンド スキャフォールディング、`Course`モデル。</span><span class="sxs-lookup"><span data-stu-id="7293c-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="7293c-156">Visual Studio でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="7293c-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="7293c-157">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="7293c-157">Build the project.</span></span> <span data-ttu-id="7293c-158">ビルドには、次のようなエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="7293c-159">グローバルに変更する`_context.Course`に`_context.Courses`(つまり、"s"を追加する`Course`)。</span><span class="sxs-lookup"><span data-stu-id="7293c-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="7293c-160">7 回の出現が検出され、更新します。</span><span class="sxs-lookup"><span data-stu-id="7293c-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="7293c-161">開いている*Pages/Courses/Index.cshtml.cs*を調べると、`OnGetAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="7293c-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="7293c-162">スキャフォールディング エンジンは、指定の一括読み込み、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7293c-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="7293c-163">`Include`メソッドは、一括読み込みを指定します。</span><span class="sxs-lookup"><span data-stu-id="7293c-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="7293c-164">アプリの実行を選択して、**コース**リンクします。</span><span class="sxs-lookup"><span data-stu-id="7293c-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="7293c-165">Department 列を表示、 `DepartmentID`、これには役に立ちません。</span><span class="sxs-lookup"><span data-stu-id="7293c-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="7293c-166">`OnGetAsync` メソッドを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="7293c-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="7293c-167">上記のコードを追加`AsNoTracking`です。</span><span class="sxs-lookup"><span data-stu-id="7293c-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="7293c-168">`AsNoTracking`返されるエンティティは追跡されませんのでパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="7293c-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="7293c-169">現在のコンテキストでない更新されるため、エンティティは追跡されません。</span><span class="sxs-lookup"><span data-stu-id="7293c-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="7293c-170">更新*Views/Courses/Index.cshtml*次の強調表示されているマークアップ。</span><span class="sxs-lookup"><span data-stu-id="7293c-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="7293c-171">スキャフォールディングのコードに、次のように変更されました。</span><span class="sxs-lookup"><span data-stu-id="7293c-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="7293c-172">インデックスからのコースに見出しを変更します。</span><span class="sxs-lookup"><span data-stu-id="7293c-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="7293c-173">追加、**数**を表示する列、`CourseID`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="7293c-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="7293c-174">既定では、主キーはいないだ通常エンドユーザーにとって意味のないスキャフォールディングされました。</span><span class="sxs-lookup"><span data-stu-id="7293c-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="7293c-175">ただし、ここでは、主キーは無効です。</span><span class="sxs-lookup"><span data-stu-id="7293c-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="7293c-176">変更、**部門**部門名を表示する列。</span><span class="sxs-lookup"><span data-stu-id="7293c-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="7293c-177">コードが表示されます、`Name`のプロパティ、`Department`に読み込まれるエンティティ、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7293c-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="7293c-178">アプリの実行を選択して、**コース**部署名を使用してリストを表示するタブです。</span><span class="sxs-lookup"><span data-stu-id="7293c-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![インデックス ページのコース](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="7293c-180">関連する Select とデータの読み込み</span><span class="sxs-lookup"><span data-stu-id="7293c-180">Loading related data with Select</span></span>

<span data-ttu-id="7293c-181">`OnGetAsync`メソッドに関連するデータを読み込みます、`Include`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7293c-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="7293c-182">`Select`演算子が必要な関連するデータのみを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="7293c-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="7293c-183">単一のアイテムのように、 `Department.Name` SQL INNER JOIN を使用します。</span><span class="sxs-lookup"><span data-stu-id="7293c-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="7293c-184">コレクションの 別のデータベースのアクセスを使用しますが、です。`Include`</span><span class="sxs-lookup"><span data-stu-id="7293c-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="7293c-185">コレクションに対して演算子です。</span><span class="sxs-lookup"><span data-stu-id="7293c-185">operator on collections.</span></span>

<span data-ttu-id="7293c-186">次のコードを持つ関連データの読み込み、`Select`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7293c-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="7293c-187">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="7293c-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="7293c-188">参照してください[IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml)と[IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)完全な例です。</span><span class="sxs-lookup"><span data-stu-id="7293c-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="7293c-189">コースおよびまで登録数を示す講習においてインストラクター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="7293c-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="7293c-190">このセクションでは、インストラクター ページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="7293c-191">![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="7293c-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="7293c-192">このページは、読み取りし、次のように関連するデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="7293c-193">講習においてインストラクターの一覧から関連するデータが表示されます、`OfficeAssignment`エンティティ (前のイメージでの Office)。</span><span class="sxs-lookup"><span data-stu-id="7293c-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="7293c-194">`Instructor`と`OfficeAssignment`エンティティに 0 または 1 を 1 つの関係。</span><span class="sxs-lookup"><span data-stu-id="7293c-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="7293c-195">一括読み込みのため、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="7293c-196">一括読み込みは、関連のデータを表示する必要がある場合に通常より効率的です。</span><span class="sxs-lookup"><span data-stu-id="7293c-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="7293c-197">この場合、オフィスの割り当て、講習においてインストラクターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="7293c-198">ユーザーが関連するインストラクター (潔助前のイメージで) を選択すると`Course`エンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="7293c-199">`Instructor`と`Course`エンティティに多対多の関係。</span><span class="sxs-lookup"><span data-stu-id="7293c-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="7293c-200">Eager を読み込み、`Course`エンティティとその関連`Department`エンティティを使用します。</span><span class="sxs-lookup"><span data-stu-id="7293c-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="7293c-201">ここでは、選択した講師のコースのみが必要なために、別々 にクエリがより効率的な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7293c-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="7293c-202">この例では、ナビゲーション プロパティのナビゲーション プロパティに含まれるエンティティの一括読み込みを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="7293c-203">データに関連するユーザーのコース (前のイメージでは、化学) を選択すると、`Enrollments`エンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="7293c-204">前のイメージで受講者名とクラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="7293c-205">`Course`と`Enrollment`エンティティに一対多の関係。</span><span class="sxs-lookup"><span data-stu-id="7293c-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="7293c-206">講師インデックス ビューのビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7293c-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="7293c-207">講習においてインストラクター ページでは、次の 3 つの異なるテーブルからデータを示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="7293c-208">3 つのテーブルを表す 3 つのエンティティを含むビュー モデルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="7293c-209">*SchoolViewModels*フォルダー作成*InstructorIndexData.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7293c-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="7293c-210">Scaffold インストラクター モデル</span><span class="sxs-lookup"><span data-stu-id="7293c-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="7293c-211">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="7293c-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="7293c-212">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="7293c-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7293c-213">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7293c-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="7293c-214">上記のコマンド スキャフォールディング、`Instructor`モデル。</span><span class="sxs-lookup"><span data-stu-id="7293c-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="7293c-215">Visual Studio でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="7293c-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="7293c-216">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="7293c-216">Build the project.</span></span> <span data-ttu-id="7293c-217">ビルドには、エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-217">The build generates errors.</span></span>

<span data-ttu-id="7293c-218">グローバルに変更する`_context.Instructor`に`_context.Instructors`(つまり、"s"を追加する`Instructor`)。</span><span class="sxs-lookup"><span data-stu-id="7293c-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="7293c-219">7 回の出現が検出され、更新します。</span><span class="sxs-lookup"><span data-stu-id="7293c-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="7293c-220">アプリケーションを実行し、インストラクターのページに移動します。</span><span class="sxs-lookup"><span data-stu-id="7293c-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="7293c-221">置き換える*Pages/Instructors/Index.cshtml.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7293c-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="7293c-222">`OnGetAsync`メソッドは、選択した講師の ID を省略可能なルート データを受け付けます。</span><span class="sxs-lookup"><span data-stu-id="7293c-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="7293c-223">クエリを調べて、 *Pages/Instructors/Index.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="7293c-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="7293c-224">2 つのクエリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7293c-224">The query has two includes:</span></span>

* <span data-ttu-id="7293c-225">`OfficeAssignment`: に表示され、[講習においてインストラクター ビュー](#IP)です。</span><span class="sxs-lookup"><span data-stu-id="7293c-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="7293c-226">`CourseAssignments`: これは、学習のコースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="7293c-227">講習においてインストラクター インデックス ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="7293c-227">Update the instructors Index page</span></span>

<span data-ttu-id="7293c-228">更新*Pages/Instructors/Index.cshtml*次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="7293c-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="7293c-229">上記のマークアップでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="7293c-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="7293c-230">更新プログラム、`page`からディレクティブ`@page`に`@page "{id:int?}"`です。</span><span class="sxs-lookup"><span data-stu-id="7293c-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="7293c-231">`"{id:int?}"`ルート テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="7293c-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="7293c-232">ルート テンプレートでは、ルート データを URL にクエリ文字列を整数値を変更します。</span><span class="sxs-lookup"><span data-stu-id="7293c-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="7293c-233">たとえばをクリックすると、**選択**ページ ディレクティブは、次のように URL を生成する際、インストラクターへのリンク。</span><span class="sxs-lookup"><span data-stu-id="7293c-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="7293c-234">ページ ディレクティブの場合は`@page "{id:int?}"`、以前の url:</span><span class="sxs-lookup"><span data-stu-id="7293c-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="7293c-235">ページ タイトルが**講習においてインストラクター**です。</span><span class="sxs-lookup"><span data-stu-id="7293c-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="7293c-236">追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ、 `item.OfficeAssignment` null でないです。</span><span class="sxs-lookup"><span data-stu-id="7293c-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="7293c-237">これは、0 または 1 を 1 つのリレーションシップであるため、ある可能性がありますいない関連 OfficeAssignment エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="7293c-238">追加、**コース**コースを表示する列が各インストラクターを学習します。</span><span class="sxs-lookup"><span data-stu-id="7293c-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="7293c-239">参照してください[行の明示的な移行`@:`](xref:mvc/views/razor#explicit-line-transition-with-)この razor 構文の詳細についてです。</span><span class="sxs-lookup"><span data-stu-id="7293c-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="7293c-240">動的に追加するコードを追加しました`class="success"`を`tr`選択した講師の要素。</span><span class="sxs-lookup"><span data-stu-id="7293c-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="7293c-241">これは、ブートス トラップのクラスを使用して、選択した行の背景色を設定します。</span><span class="sxs-lookup"><span data-stu-id="7293c-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="7293c-242">新しいラベル付けされるハイパーリンクを追加**選択**です。</span><span class="sxs-lookup"><span data-stu-id="7293c-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="7293c-243">このリンクを送信する ID を選択したインストラクター、`Index`メソッドおよび背景色を設定します。</span><span class="sxs-lookup"><span data-stu-id="7293c-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="7293c-244">アプリの実行を選択して、**講習においてインストラクター**タブです。ページが表示されます、 `Location` (office) から、関連する`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="7293c-245">場合 OfficeAssignment' は null、空のテーブル セルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![講習においてインストラクター インデックス ページが何も選択](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="7293c-247">をクリックして、**選択**リンクします。</span><span class="sxs-lookup"><span data-stu-id="7293c-247">Click on the **Select** link.</span></span> <span data-ttu-id="7293c-248">行のスタイル変更します。</span><span class="sxs-lookup"><span data-stu-id="7293c-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="7293c-249">選択した講師でコースを追加します。</span><span class="sxs-lookup"><span data-stu-id="7293c-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="7293c-250">更新プログラム、`OnGetAsync`メソッド*Pages/Instructors/Index.cshtml.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7293c-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="7293c-251">更新されたクエリを確認してください。</span><span class="sxs-lookup"><span data-stu-id="7293c-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="7293c-252">上記のクエリを追加、`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="7293c-253">講師が選択されているときに、次のコードが実行 (`id != null`)。</span><span class="sxs-lookup"><span data-stu-id="7293c-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="7293c-254">講習においてインストラクターにビューのモデルの一覧から選択した講師が取得されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="7293c-255">ビュー モデルの`Courses`プロパティが読み込まれ、`Course`そのインストラクターからエンティティ`CourseAssignments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7293c-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="7293c-256">`Where`メソッドがコレクションを返します。</span><span class="sxs-lookup"><span data-stu-id="7293c-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="7293c-257">上記の「`Where`メソッドを 1 つだけ`Instructor`エンティティが返されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="7293c-258">`Single`メソッドが、1 つに、コレクションを変換`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="7293c-259">`Instructor`エンティティへのアクセスを提供する、`CourseAssignments`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="7293c-260">`CourseAssignments`関連するへのアクセスを提供`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![講師-コース m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="7293c-262">`Single`メソッドは、コレクションは、1 つの項目と、コレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="7293c-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="7293c-263">`Single`コレクションが空の場合、または複数の項目がある場合、メソッドが例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="7293c-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="7293c-264">代わりに`SingleOrDefault`既定の値を返す (この場合は null) コレクションが空の場合。</span><span class="sxs-lookup"><span data-stu-id="7293c-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="7293c-265">使用して`SingleOrDefault`空のコレクションに対して。</span><span class="sxs-lookup"><span data-stu-id="7293c-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="7293c-266">例外が発生 (から検索しようとして、 `Courses` null 参照のプロパティ)。</span><span class="sxs-lookup"><span data-stu-id="7293c-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="7293c-267">例外メッセージは、問題の原因と明確に小さいを示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="7293c-268">次のコードには追加、ビュー モデルの`Enrollments`コースが選択されているプロパティ。</span><span class="sxs-lookup"><span data-stu-id="7293c-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="7293c-269">末尾に次のマークアップを追加、 *Pages/Courses/Index.cshtml* Razor ページ。</span><span class="sxs-lookup"><span data-stu-id="7293c-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="7293c-270">上記のマークアップには、あるインストラクターが選択されている場合、インストラクターに関連するコースの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="7293c-271">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="7293c-271">Test the app.</span></span> <span data-ttu-id="7293c-272">をクリックして、**選択**講習においてインストラクター ページ上のリンク。</span><span class="sxs-lookup"><span data-stu-id="7293c-272">Click on a **Select** link on the instructors page.</span></span>

![講習においてインストラクター インデックス ページ講師が選択されています。](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="7293c-274">学生のデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-274">Show student data</span></span>

<span data-ttu-id="7293c-275">このセクションでは、選択したコースを受講者データを表示するアプリが更新されます。</span><span class="sxs-lookup"><span data-stu-id="7293c-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="7293c-276">クエリでは、更新、`OnGetAsync`メソッド*Pages/Instructors/Index.cshtml.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7293c-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="7293c-277">更新*Pages/Instructors/Index.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="7293c-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="7293c-278">ファイルの末尾に次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="7293c-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="7293c-279">上記のマークアップは、選択したこのコースで登録されている学生のリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="7293c-280">ページを更新し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="7293c-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="7293c-281">登録済みの受講者との成績の一覧を表示するコースを選択します。</span><span class="sxs-lookup"><span data-stu-id="7293c-281">Select a course to see the list of enrolled students and their grades.</span></span>

![講習においてインストラクター インデックス ページ インストラクターと選択したコース](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="7293c-283">1 つを使用します。</span><span class="sxs-lookup"><span data-stu-id="7293c-283">Using Single</span></span>

<span data-ttu-id="7293c-284">`Single`メソッドを渡すことができます、`Where`条件呼び出す代わりに、`Where`メソッドとは別に。</span><span class="sxs-lookup"><span data-stu-id="7293c-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="7293c-285">前述の`Single`アプローチも利点は使用する場合より`Where`です。</span><span class="sxs-lookup"><span data-stu-id="7293c-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="7293c-286">一部の開発者が必要に応じて、`Single`スタイルに近づきます。</span><span class="sxs-lookup"><span data-stu-id="7293c-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="7293c-287">明示的読み込み</span><span class="sxs-lookup"><span data-stu-id="7293c-287">Explicit loading</span></span>

<span data-ttu-id="7293c-288">現在のコードを指定の一括読み込み`Enrollments`と`Students`:</span><span class="sxs-lookup"><span data-stu-id="7293c-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="7293c-289">ユーザーはほとんどコースに登録を表示するとします。</span><span class="sxs-lookup"><span data-stu-id="7293c-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="7293c-290">その場合は、最適化は要求された場合のみ登録データを読み込むことです。</span><span class="sxs-lookup"><span data-stu-id="7293c-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="7293c-291">このセクションで、`OnGetAsync`の明示的な読み込みを使用する更新`Enrollments`と`Students`です。</span><span class="sxs-lookup"><span data-stu-id="7293c-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="7293c-292">更新プログラム、`OnGetAsync`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7293c-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="7293c-293">上記のコードを削除、 *ThenInclude*の登録と学生データ メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7293c-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="7293c-294">コースを選択すると、強調表示されたコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="7293c-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="7293c-295">`Enrollment`選択コースのエンティティです。</span><span class="sxs-lookup"><span data-stu-id="7293c-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="7293c-296">`Student`各エンティティ`Enrollment`です。</span><span class="sxs-lookup"><span data-stu-id="7293c-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="7293c-297">前述のコードをコメントに注意してください。`.AsNoTracking()`です。</span><span class="sxs-lookup"><span data-stu-id="7293c-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="7293c-298">追跡対象のエンティティのナビゲーション プロパティが明示的に読み込まれただけことができます。</span><span class="sxs-lookup"><span data-stu-id="7293c-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="7293c-299">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="7293c-299">Test the app.</span></span> <span data-ttu-id="7293c-300">ユーザーの観点から、アプリの動作は同じです、以前のバージョンです。</span><span class="sxs-lookup"><span data-stu-id="7293c-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="7293c-301">次のチュートリアルでは、関連するデータを更新する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7293c-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="7293c-302">[前へ](xref:data/ef-rp/complex-data-model)
>[次へ](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="7293c-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
