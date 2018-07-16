---
title: ASP.NET Core の Razor ページと EF Core - 関連データの読み込み - 6/8
author: rick-anderson
description: このチュートリアルでは、関連データ (Entity Framework がナビゲーション プロパティに読み込むデータ) の読み取りと表示を行います。
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: fa3147cc4ad121784911eef802e04ca91f16448f
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063313"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="54452-103">ASP.NET Core の Razor ページと EF Core - 関連データの読み込み - 6/8</span><span class="sxs-lookup"><span data-stu-id="54452-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="54452-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="54452-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="54452-105">このチュートリアルでは、関連データが読み取られ、表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="54452-106">関連データとは、EF Core がナビゲーション プロパティに読み込むデータのことです。</span><span class="sxs-lookup"><span data-stu-id="54452-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="54452-107">解決できない問題が発生した場合は、[このステージの完成したアプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)をダウンロードしてください。</span><span class="sxs-lookup"><span data-stu-id="54452-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="54452-108">以下の図は、このチュートリアルの完成したページを示しています。</span><span class="sxs-lookup"><span data-stu-id="54452-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Courses/Index ページ](read-related-data/_static/courses-index.png)

![Instructors/Index ページ](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="54452-111">関連データの一括読み込み、明示的読み込み、遅延読み込み</span><span class="sxs-lookup"><span data-stu-id="54452-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="54452-112">EF Core がエンティティのナビゲーション プロパティに関連データを読み込むには、複数の方法があります。</span><span class="sxs-lookup"><span data-stu-id="54452-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="54452-113">[一括読み込み](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="54452-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="54452-114">一括読み込みは、エンティティの 1 つの型に対するクエリが関連エンティティも読み込む場合です。</span><span class="sxs-lookup"><span data-stu-id="54452-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="54452-115">エンティティが読み取られるときに、その関連データが取得されます。</span><span class="sxs-lookup"><span data-stu-id="54452-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="54452-116">これは通常、必要なすべてのデータを取得する 1 つの結合クエリになります。</span><span class="sxs-lookup"><span data-stu-id="54452-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="54452-117">EF Core は、一部の型の一括読み込みに対して複数のクエリを発行します。</span><span class="sxs-lookup"><span data-stu-id="54452-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="54452-118">複数のクエリを発行することで、1 つのクエリしかなかった EF6 の一部のクエリよりも、効率を高めることができます。</span><span class="sxs-lookup"><span data-stu-id="54452-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="54452-119">一括読み込みは、`Include` メソッドと `ThenInclude` メソッドを使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="54452-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![一括読み込みの例](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="54452-121">一括読み込みでは、コレクション ナビゲーションが含まれるときに、複数のクエリが送信されます。</span><span class="sxs-lookup"><span data-stu-id="54452-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="54452-122">メイン クエリに 1 つのクエリ</span><span class="sxs-lookup"><span data-stu-id="54452-122">One query for the main query</span></span> 
  * <span data-ttu-id="54452-123">読み込みツリー内のコレクション "エッジ" ごとに 1 つのクエリ</span><span class="sxs-lookup"><span data-stu-id="54452-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="54452-124">`Load` で分離したクエリ: データは分離したクエリで取得でき、EF Core がナビゲーション プロパティを "修正" します。</span><span class="sxs-lookup"><span data-stu-id="54452-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="54452-125">"修正" は、ナビゲーション プロパティが EF Core によって自動的に入力されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="54452-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="54452-126">`Load` で分離したクエリは、一括読み込みよりも明示的読み込みに似ています。</span><span class="sxs-lookup"><span data-stu-id="54452-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![分離したクエリの例](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="54452-128">注: EF Core は、コンテキスト インスタンスに以前に読み込まれたその他のエンティティに対して、ナビゲーション プロパティを自動的に修正します。</span><span class="sxs-lookup"><span data-stu-id="54452-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="54452-129">ナビゲーション プロパティのデータが明示的に含まれ*ない*場合でも、関連エンティティの一部またはすべてが以前に読み込まれていれば、プロパティを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="54452-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="54452-130">[明示的読み込み](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="54452-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="54452-131">エンティティが最初に読み込まれるときに、関連データは取得されません。</span><span class="sxs-lookup"><span data-stu-id="54452-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="54452-132">必要なときに関連するデータを取得するコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54452-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="54452-133">分離したクエリによる明示的読み込みにより、複数のクエリが DB に送信されます。</span><span class="sxs-lookup"><span data-stu-id="54452-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="54452-134">明示的読み込みでは、コードで読み込まれるナビゲーション プロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="54452-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="54452-135">明示的読み込みを行うには、`Load` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="54452-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="54452-136">例:</span><span class="sxs-lookup"><span data-stu-id="54452-136">For example:</span></span>

  ![明示的読み込みの例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="54452-138">[遅延読み込み](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="54452-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="54452-139">[EF Core では、現在、遅延読み込みをサポートしていません](https://github.com/aspnet/EntityFrameworkCore/issues/3797)。</span><span class="sxs-lookup"><span data-stu-id="54452-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="54452-140">エンティティが最初に読み込まれるときに、関連データは取得されません。</span><span class="sxs-lookup"><span data-stu-id="54452-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="54452-141">ナビゲーション プロパティに初めてアクセスすると、そのナビゲーション プロパティに必要なデータが自動的に取得されます。</span><span class="sxs-lookup"><span data-stu-id="54452-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="54452-142">初めてナビゲーション プロパティにアクセスされるたびに、クエリが DB に送信されます。</span><span class="sxs-lookup"><span data-stu-id="54452-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="54452-143">`Select` 演算子は必要な関連データのみを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="54452-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="54452-144">部門名を表示する Courses ページを作成する</span><span class="sxs-lookup"><span data-stu-id="54452-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="54452-145">Course エンティティには、`Department` エンティティを含むナビゲーション プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="54452-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="54452-146">`Department` エンティティには、コースが割り当てられる部門が含まれています。</span><span class="sxs-lookup"><span data-stu-id="54452-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="54452-147">コースの一覧で割り当てられている部門の名前を表示するには:</span><span class="sxs-lookup"><span data-stu-id="54452-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="54452-148">`Department` エンティティから `Name` プロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="54452-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="54452-149">`Department` エンティティは `Course.Department` ナビゲーション プロパティから取得されます。</span><span class="sxs-lookup"><span data-stu-id="54452-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="54452-151">Course モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="54452-151">Scaffold the Course model</span></span>

* <span data-ttu-id="54452-152">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="54452-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="54452-153">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="54452-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="54452-154">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="54452-154">Run the following command:</span></span>

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="54452-155">上記のコマンドは、`Course` モデルをスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="54452-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="54452-156">Visual Studio でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="54452-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="54452-157">*Pages/Courses/Index.cshtml.cs* を開き、`OnGetAsync` メソッドを調べます。</span><span class="sxs-lookup"><span data-stu-id="54452-157">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="54452-158">スキャフォールディング エンジンは、`Department` ナビゲーション プロパティに一括読み込みを指定しました。</span><span class="sxs-lookup"><span data-stu-id="54452-158">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="54452-159">`Include` メソッドが一括読み込みを指定します。</span><span class="sxs-lookup"><span data-stu-id="54452-159">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="54452-160">アプリを実行し、**[Courses]** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="54452-160">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="54452-161">Department 列に `DepartmentID` が表示されますが、これには役に立ちません。</span><span class="sxs-lookup"><span data-stu-id="54452-161">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="54452-162">`OnGetAsync` メソッドを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-162">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="54452-163">上のコードは `AsNoTracking` を追加します。</span><span class="sxs-lookup"><span data-stu-id="54452-163">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="54452-164">`AsNoTracking` は、返されるエンティティが追跡されないため、パフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="54452-164">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="54452-165">これらのエンティティは現在のコンテキストでは更新されないため、追跡されません。</span><span class="sxs-lookup"><span data-stu-id="54452-165">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="54452-166">次の強調表示されているマークアップで *Pages/Courses/Index.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-166">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="54452-167">スキャフォールディング コードに、次の変更が行われました。</span><span class="sxs-lookup"><span data-stu-id="54452-167">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="54452-168">見出しが Index から Courses に変更されました。</span><span class="sxs-lookup"><span data-stu-id="54452-168">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="54452-169">`CourseID` プロパティ値を示す **Number** 列が追加されました。</span><span class="sxs-lookup"><span data-stu-id="54452-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="54452-170">既定では、主キーは、通常、エンドユーザーにとって意味がないため、スキャフォールディングされません。</span><span class="sxs-lookup"><span data-stu-id="54452-170">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="54452-171">ただし、このケースでは、主キーは意味があります。</span><span class="sxs-lookup"><span data-stu-id="54452-171">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="54452-172">部門名が表示されるように、**Department** 列を変更しました。</span><span class="sxs-lookup"><span data-stu-id="54452-172">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="54452-173">コードは、`Department` ナビゲーション プロパティに読み込まれる `Department` エンティティの `Name` プロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="54452-173">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="54452-174">アプリを実行し、**[Courses]** タブを選択して部門名のリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="54452-174">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Courses/Index ページ](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="54452-176">Select を使用した関連データの読み込み</span><span class="sxs-lookup"><span data-stu-id="54452-176">Loading related data with Select</span></span>

<span data-ttu-id="54452-177">`OnGetAsync` メソッドは、`Include` メソッドを使用して関連データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="54452-177">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="54452-178">`Select` 演算子は必要な関連データのみを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="54452-178">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="54452-179">`Department.Name` のような単一の項目の場合、SQL INNER JOIN が使用されます。</span><span class="sxs-lookup"><span data-stu-id="54452-179">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="54452-180">コレクションの場合は、別のデータベース アクセスが使用されますが、コレクションの `Include` 演算子でも同じです。</span><span class="sxs-lookup"><span data-stu-id="54452-180">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="54452-181">次のコードは、`Select` メソッドを使用して関連データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="54452-181">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="54452-182">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="54452-182">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="54452-183">完全な例については、[IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) と [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54452-183">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="54452-184">コース登録を示す Instructors ページを作成する</span><span class="sxs-lookup"><span data-stu-id="54452-184">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="54452-185">このセクションでは、Instructors ページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="54452-185">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="54452-186">![Instructors/Index ページ](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="54452-186">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="54452-187">このページは、次の方法で関連データを読み取って表示します。</span><span class="sxs-lookup"><span data-stu-id="54452-187">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="54452-188">インストラクターのリストには `OfficeAssignment` エンティティからの関連データが表示されます (上の図の Office)。</span><span class="sxs-lookup"><span data-stu-id="54452-188">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="54452-189">`Instructor` エンティティと `OfficeAssignment` エンティティは、一対ゼロまたは一対一のリレーションシップです。</span><span class="sxs-lookup"><span data-stu-id="54452-189">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="54452-190">`OfficeAssignment` エンティティには一括読み込みが使用されています。</span><span class="sxs-lookup"><span data-stu-id="54452-190">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="54452-191">一括読み込みは一般的に、関連データを表示する必要がある場合により効率的です。</span><span class="sxs-lookup"><span data-stu-id="54452-191">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="54452-192">この場合、インストラクターへのオフィスの割り当てが表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-192">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="54452-193">ユーザーがインストラクターを選択 (上の図では Harui) すると、関連 `Course` エンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-193">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="54452-194">`Instructor` エンティティと `Course` エンティティは多対多リレーションシップです。</span><span class="sxs-lookup"><span data-stu-id="54452-194">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="54452-195">`Course` エンティティとその関連 `Department` エンティティには一括読み込みが使用されます。</span><span class="sxs-lookup"><span data-stu-id="54452-195">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="54452-196">このケースでは、選択したインストラクターのコースのみが必要なため、分離したクエリの方が効率的な場合があります。</span><span class="sxs-lookup"><span data-stu-id="54452-196">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="54452-197">この例では、ナビゲーション プロパティ内のエンティティのナビゲーション プロパティに一括読み込みを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="54452-197">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="54452-198">ユーザーがコースを選択すると (上の図では Chemistry (化学))、`Enrollments` エンティティからの関連データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-198">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="54452-199">上の図では、受講者名とグレードが表示されています。</span><span class="sxs-lookup"><span data-stu-id="54452-199">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="54452-200">`Course` エンティティと `Enrollment` エンティティは一対多リレーションシップです。</span><span class="sxs-lookup"><span data-stu-id="54452-200">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="54452-201">Instructor インデックス ビューのビュー モデルを作成する</span><span class="sxs-lookup"><span data-stu-id="54452-201">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="54452-202">Instructors ページには、3 つの異なるテーブルからのデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-202">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="54452-203">3 つのテーブルを表す 3 つのエンティティを含むビュー モデルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="54452-203">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="54452-204">次のコードを使用して、*SchoolViewModels* フォルダー内に *InstructorIndexData.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="54452-204">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="54452-205">Instructor モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="54452-205">Scaffold the Instructor model</span></span>

* <span data-ttu-id="54452-206">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="54452-206">Exit Visual Studio.</span></span>
* <span data-ttu-id="54452-207">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="54452-207">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="54452-208">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="54452-208">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="54452-209">上記のコマンドは、`Instructor` モデルをスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="54452-209">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="54452-210">Visual Studio でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="54452-210">Open the project in Visual Studio.</span></span>

<span data-ttu-id="54452-211">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="54452-211">Build the project.</span></span> <span data-ttu-id="54452-212">ビルドがエラーを生成します。</span><span class="sxs-lookup"><span data-stu-id="54452-212">The build generates errors.</span></span>

<span data-ttu-id="54452-213">`_context.Instructor` を `_context.Instructors` にグローバルに変更します (つまり、"s" を `Instructor` に追加します)。</span><span class="sxs-lookup"><span data-stu-id="54452-213">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="54452-214">7 回の出現が見つかり、更新されます。</span><span class="sxs-lookup"><span data-stu-id="54452-214">7 occurrences are found and updated.</span></span>

<span data-ttu-id="54452-215">アプリを実行し、Instructors ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="54452-215">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="54452-216">*Pages/Instructors/Index.cshtml.cs* を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="54452-216">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="54452-217">`OnGetAsync` メソッドは、選択したインストラクターの ID の任意のルート データを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="54452-217">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="54452-218">*Pages/Instructors/Index.cshtml.cs* ファイルでクエリを調べます。</span><span class="sxs-lookup"><span data-stu-id="54452-218">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="54452-219">クエリには次の 2 つが含まれています。</span><span class="sxs-lookup"><span data-stu-id="54452-219">The query has two includes:</span></span>

* <span data-ttu-id="54452-220">`OfficeAssignment`: [Instructors ビュー](#IP)に表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-220">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="54452-221">`CourseAssignments`: 担当したコースを取り込みます。</span><span class="sxs-lookup"><span data-stu-id="54452-221">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="54452-222">Instructors/Index ページを更新する</span><span class="sxs-lookup"><span data-stu-id="54452-222">Update the instructors Index page</span></span>

<span data-ttu-id="54452-223">次のマークアップを使用して *Pages/Instructors/Index.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-223">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="54452-224">上記のマークアップは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="54452-224">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="54452-225">`page` ディレクティブを `@page` から `@page "{id:int?}"` に更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-225">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="54452-226">`"{id:int?}"` はルート テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="54452-226">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="54452-227">ルート テンプレートは、URL 内の整数クエリ文字列をルート データに変更します。</span><span class="sxs-lookup"><span data-stu-id="54452-227">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="54452-228">たとえば、`@page` ディレクティブのみのインストラクターで **[Select]** リンクをクリックすると、次のような URL を生成します。</span><span class="sxs-lookup"><span data-stu-id="54452-228">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="54452-229">ページ ディレクティブが `@page "{id:int?}"` の場合、上記の URL は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="54452-229">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="54452-230">ページ タイトルは **Instructors** です。</span><span class="sxs-lookup"><span data-stu-id="54452-230">Page title is **Instructors**.</span></span>
* <span data-ttu-id="54452-231">`item.OfficeAssignment` が null ではない場合にのみ `item.OfficeAssignment.Location` を表示する **Office** 列を追加しました。</span><span class="sxs-lookup"><span data-stu-id="54452-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="54452-232">これは、一対ゼロまたは一対一のリレーションシップであるため、関連する OfficeAssignment エンティティがない場合があります。</span><span class="sxs-lookup"><span data-stu-id="54452-232">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="54452-233">インストラクターごとに担当したコースを表示する **Courses** 列を追加しました。</span><span class="sxs-lookup"><span data-stu-id="54452-233">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="54452-234">この Razor 構文の詳細については、「[Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-)」(@: による明示的な行の遷移) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54452-234">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="54452-235">選択したインストラクターの `tr` 要素に `class="success"` を動的に追加するコードを追加しました。</span><span class="sxs-lookup"><span data-stu-id="54452-235">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="54452-236">これは、ブートストラップ クラスを使用して、選択した行の背景色を設定します。</span><span class="sxs-lookup"><span data-stu-id="54452-236">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="54452-237">**Select** とラベル付けされるハイパーリンクを追加しました。</span><span class="sxs-lookup"><span data-stu-id="54452-237">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="54452-238">このリンクは、選択したインストラクターの ID を `Index` メソッドに送信し、背景色を設定します。</span><span class="sxs-lookup"><span data-stu-id="54452-238">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="54452-239">アプリを実行し、**[Instructors]** タブを選択します。関連する `OfficeAssignment` エンティティから `Location` (オフィス) がページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-239">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="54452-240">OfficeAssignment\` が null の場合、空のテーブル セルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="54452-240">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![何も選択されていない Instructors/Index ページ](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="54452-242">**[Select]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="54452-242">Click on the **Select** link.</span></span> <span data-ttu-id="54452-243">行のスタイルが変更されます。</span><span class="sxs-lookup"><span data-stu-id="54452-243">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="54452-244">選択したインストラクターが担当するコースを追加する</span><span class="sxs-lookup"><span data-stu-id="54452-244">Add courses taught by selected instructor</span></span>

<span data-ttu-id="54452-245">次のコードを使用して、*Pages/Instructors/Index.cshtml.cs* 内の `OnGetAsync` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-245">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="54452-246">`public int CourseID { get; set; }` を追加します</span><span class="sxs-lookup"><span data-stu-id="54452-246">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="54452-247">更新されたクエリを確認します。</span><span class="sxs-lookup"><span data-stu-id="54452-247">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="54452-248">上記のクエリは `Department` エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="54452-248">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="54452-249">次のコードは、インストラクターが選択されたとき (`id != null`) に実行されます。</span><span class="sxs-lookup"><span data-stu-id="54452-249">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="54452-250">選択されたインストラクターがビュー モデルのインストラクターのリストから取得されます。</span><span class="sxs-lookup"><span data-stu-id="54452-250">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="54452-251">ビュー モデルの `Courses` プロパティが `Course` エンティティと共にそのインストラクターの `CourseAssignments` ナビゲーション プロパティから読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="54452-251">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="54452-252">`Where` メソッドはコレクションを返します。</span><span class="sxs-lookup"><span data-stu-id="54452-252">The `Where` method returns a collection.</span></span> <span data-ttu-id="54452-253">上記の `Where` メソッドでは、1 つの `Instructor` エンティティのみが返されます。</span><span class="sxs-lookup"><span data-stu-id="54452-253">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="54452-254">`Single` メソッドはコレクションを 1 つの `Instructor` エンティティに変換します。</span><span class="sxs-lookup"><span data-stu-id="54452-254">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="54452-255">`Instructor` エンティティは `CourseAssignments` プロパティへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="54452-255">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="54452-256">`CourseAssignments` は関連する `Course` エンティティへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="54452-256">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![インストラクター対コース m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="54452-258">コレクションに 1 つの項目しかない場合は、`Single` メソッドがコレクションで使用されます。</span><span class="sxs-lookup"><span data-stu-id="54452-258">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="54452-259">コレクションが空の場合、または複数の項目がある場合、`Single` メソッドは例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="54452-259">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="54452-260">代わりに、コレクションが空の場合に既定値を返す (この場合は null) `SingleOrDefault` を使用します。</span><span class="sxs-lookup"><span data-stu-id="54452-260">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="54452-261">空のコレクションで `SingleOrDefault` を使用すると、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="54452-261">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="54452-262">(null 参照で `Courses` プロパティを見つけようとすると) 例外になります。</span><span class="sxs-lookup"><span data-stu-id="54452-262">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="54452-263">例外メッセージに問題の原因が明確に示されない場合があります。</span><span class="sxs-lookup"><span data-stu-id="54452-263">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="54452-264">次のコードは、コースが選択されたときにビュー モデルの `Enrollments` プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="54452-264">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="54452-265">*Pages/Instructors/Index.cshtml* Razor ページの末尾に次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="54452-265">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="54452-266">上記のマークアップは、インストラクターが選択されたときに、インストラクターに関連するコースのリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="54452-266">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="54452-267">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="54452-267">Test the app.</span></span> <span data-ttu-id="54452-268">Instructors ページの **[Select]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="54452-268">Click on a **Select** link on the instructors page.</span></span>

![インストラクターが選択された Instructors/Index ページ](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="54452-270">受講者データを表示する</span><span class="sxs-lookup"><span data-stu-id="54452-270">Show student data</span></span>

<span data-ttu-id="54452-271">このセクションでは、選択したコースの受講者データを表示するため、アプリが更新されます。</span><span class="sxs-lookup"><span data-stu-id="54452-271">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="54452-272">次のコードを使用して、*Pages/Instructors/Index.cshtml.cs* 内の `OnGetAsync` メソッドのクエリを更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-272">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="54452-273">*Pages/Instructors/Index.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-273">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="54452-274">ファイルの末尾に次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="54452-274">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="54452-275">上記のマークアップは、選択したコースに登録されている受講者のリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="54452-275">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="54452-276">ページを更新し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="54452-276">Refresh the page and select an instructor.</span></span> <span data-ttu-id="54452-277">コースを選択して、登録済みの受講者とその成績のリストを表示します。</span><span class="sxs-lookup"><span data-stu-id="54452-277">Select a course to see the list of enrolled students and their grades.</span></span>

![インストラクターとコースが選択された Instructors/Index ページ](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="54452-279">Single を使用する</span><span class="sxs-lookup"><span data-stu-id="54452-279">Using Single</span></span>

<span data-ttu-id="54452-280">`Single` メソッドは、`Where` メソッドを別に呼び出す代わりに、`Where` 条件で渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="54452-280">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="54452-281">上記の `Single` アプローチでは、`Where` を使用すること以上のメリットは提供されません。</span><span class="sxs-lookup"><span data-stu-id="54452-281">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="54452-282">一部の開発者は、`Single` アプローチ スタイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="54452-282">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="54452-283">明示的読み込み</span><span class="sxs-lookup"><span data-stu-id="54452-283">Explicit loading</span></span>

<span data-ttu-id="54452-284">現在のコードは、`Enrollments` と `Students` に一括読み込みを指定します。</span><span class="sxs-lookup"><span data-stu-id="54452-284">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="54452-285">ユーザーがコースの登録を表示することはほとんどないとします。</span><span class="sxs-lookup"><span data-stu-id="54452-285">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="54452-286">その場合、最適化は要求された場合にのみ登録データを読み込むことです。</span><span class="sxs-lookup"><span data-stu-id="54452-286">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="54452-287">このセクションでは、`Enrollments` と `Students` の明示的読み込みを使用するために `OnGetAsync` が更新されます。</span><span class="sxs-lookup"><span data-stu-id="54452-287">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="54452-288">次のコードを使用して `OnGetAsync` を更新します。</span><span class="sxs-lookup"><span data-stu-id="54452-288">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="54452-289">上記のコードは、登録と学生データの *ThenInclude* メソッド呼び出しを破棄します。</span><span class="sxs-lookup"><span data-stu-id="54452-289">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="54452-290">コースが選択されると、強調表示されたコードが以下を取得します。</span><span class="sxs-lookup"><span data-stu-id="54452-290">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="54452-291">選択したコースの `Enrollment` エンティティ。</span><span class="sxs-lookup"><span data-stu-id="54452-291">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="54452-292">各 `Enrollment` の `Student` エンティティ。</span><span class="sxs-lookup"><span data-stu-id="54452-292">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="54452-293">上記のコードでは、`.AsNoTracking()` がコメント アウトされていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="54452-293">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="54452-294">追跡対象のエンティティに対して、ナビゲーション プロパティのみを明示的に読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="54452-294">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="54452-295">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="54452-295">Test the app.</span></span> <span data-ttu-id="54452-296">ユーザーの観点からは、アプリの動作は以前のバージョンと同じです。</span><span class="sxs-lookup"><span data-stu-id="54452-296">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="54452-297">次のチュートリアルでは、関連データの更新方法を示します。</span><span class="sxs-lookup"><span data-stu-id="54452-297">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="54452-298">[前へ](xref:data/ef-rp/complex-data-model)
>[次へ](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="54452-298">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
