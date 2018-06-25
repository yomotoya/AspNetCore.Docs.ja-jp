---
title: ASP.NET Core の Razor ページと EF Core - CRUD - 2/8
author: rick-anderson
description: EF Core で作成、読み取り、更新、削除を行う方法を示します
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 17d48cae50745508a64a9fb8a153b7b891e64a23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278688"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="3c4c6-103">ASP.NET Core の Razor ページと EF Core - CRUD - 2/8</span><span class="sxs-lookup"><span data-stu-id="3c4c6-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

<span data-ttu-id="3c4c6-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3c4c6-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="3c4c6-105">このチュートリアルでは、スキャフォールディング CRUD (作成、読み取り、更新、削除) コードのレビューとカスタマイズを行います。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="3c4c6-106">注: 複雑さを最小限に抑え、これらのチュートリアルを通して主眼を EF Core に置くために、EF Core コードは Razor ページのページ モデルで使用されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="3c4c6-107">一部の開発者は、サービス レイヤーまたはリポジトリ パターンを使用して、UI (Razor ページ) とデータ アクセス層との間に抽象化レイヤーを作成しています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="3c4c6-108">このチュートリアルでは、"*受講者*" フォルダー内の [作成]、[編集]、[削除]、[詳細] の各 Razor ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="3c4c6-109">スキャフォールディング コードでは、[作成]、[編集]、[削除] ページに対して次のパターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="3c4c6-110">要求されたデータを HTTP GET メソッド `OnGetAsync` を使用して取得および表示します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="3c4c6-111">データに加えた変更を HTTP POST メソッド `OnPostAsync` を使用して保存します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="3c4c6-112">[インデックス] および [詳細] ページでは、要求されたデータを HTTP GET メソッド `OnGetAsync` を使用して取得および表示します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="3c4c6-113">SingleOrDefaultAsync を FirstOrDefaultAsync に置換する</span><span class="sxs-lookup"><span data-stu-id="3c4c6-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="3c4c6-114">生成したコードは、[SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) を使用することで、要求されたエンティティを取得します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-114">The generated code uses [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="3c4c6-115">[FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) は、次の場合、1 つのエンティティをフェッチする際により効率的です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-115">[FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="3c4c6-116">クエリから返されるエンティティが 1 つ以下であることを、コードで検証する必要がない。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="3c4c6-117">`SingleOrDefaultAsync` がより多くのデータをフェッチし、不要な作業を行う。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="3c4c6-118">フィルター部に適合するエンティティが複数存在する場合に `SingleOrDefaultAsync` によって例外がスローされる。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="3c4c6-119">フィルター部に適合するエンティティが複数存在する場合に `FirstOrDefaultAsync` によって例外がスローされない。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="3c4c6-120">`SingleOrDefaultAsync` を `FirstOrDefaultAsync` にグローバルに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="3c4c6-121">`SingleOrDefaultAsync` は次の 5 か所で使用されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="3c4c6-122">[詳細] ページの `OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="3c4c6-123">[編集] ページと [削除] ページの `OnGetAsync` および `OnPostAsync`。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="3c4c6-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="3c4c6-124">FindAsync</span></span>

<span data-ttu-id="3c4c6-125">スキャフォールディング コードの多くでは、`FirstOrDefaultAsync` または `SingleOrDefaultAsync` ではなく、[FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-125">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="3c4c6-126">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="3c4c6-126">`FindAsync`:</span></span>

* <span data-ttu-id="3c4c6-127">主キー (PK) を持つエンティティを検索します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="3c4c6-128">PK を持つエンティティがコンテキストによって追跡されている場合、DB に対する要求がなくても該当するエンティティが返されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="3c4c6-129">単純で簡潔です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-129">Is simple and concise.</span></span>
* <span data-ttu-id="3c4c6-130">単一のエンティティを検索するように最適化されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="3c4c6-131">一部のシナリオではパフォーマンスの利点を得られますが、通常の Web シナリオではほとんど利点がありません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="3c4c6-132">[SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) ではなく、[FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) を暗黙的に使用します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-132">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="3c4c6-133">ただし、他のエンティティを追加する場合、Find はもはや適切ではありません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="3c4c6-134">つまり、アプリの進行状況に応じて、Find を破棄しクエリに移行することが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="3c4c6-135">[詳細] ページをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="3c4c6-135">Customize the Details page</span></span>

<span data-ttu-id="3c4c6-136">`Pages/Students` ページを参照します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="3c4c6-137">**[編集]**、**[詳細]**、**[削除]** の各リンクは、*Pages/Students/Index.cshtml* ファイルで[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="3c4c6-138">[詳細] リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-138">Select a Details link.</span></span> <span data-ttu-id="3c4c6-139">URL の形式は、`http://localhost:5000/Students/Details?id=2` です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="3c4c6-140">クエリ文字列 (`?id=2`) によって受講者 ID が渡されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="3c4c6-141">`"{id:int}"` ルート テンプレートを使用するには、[編集]、[詳細]、[削除] の Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="3c4c6-142">これらの各ページのページ ディレクティブを `@page` から `@page "{id:int}"` に変更します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="3c4c6-143">整数ルート値を**含まない**、"{id:int}" ルート テンプレートを使用するページへの要求では、HTTP 404 (見つかりません) エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3c4c6-144">たとえば、`http://localhost:5000/Students/Details` は 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="3c4c6-145">ID を省略するには、次のように `?` をルート制約に追加します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3c4c6-146">アプリを実行し、[詳細] リンクをクリックし、URL がルート データとして ID を渡していることを確認します (`http://localhost:5000/Students/Details/2`)。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="3c4c6-147">`@page` から `@page "{id:int}"` への変更をグローバルに行わないでください。これを行うと、[ホーム] ページと [作成] ページへのリンクが解除されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="3c4c6-148">関連データを追加する</span><span class="sxs-lookup"><span data-stu-id="3c4c6-148">Add related data</span></span>

<span data-ttu-id="3c4c6-149">Students インデックス ページのスキャフォールディング コードには、`Enrollments` プロパティが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="3c4c6-150">このセクションでは、`Enrollments` コレクションのコンテンツが [詳細] ページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="3c4c6-151">*Pages/Students/Details.cshtml.cs* の `OnGetAsync` メソッドでは、`FirstOrDefaultAsync` メソッドを使用して単一の `Student` エンティティを取得します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="3c4c6-152">次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-152">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="3c4c6-153">`Include` メソッドと `ThenInclude` メソッドにより、コンテキストは `Student.Enrollments` ナビゲーション プロパティと、各登録内の `Enrollment.Course` ナビゲーション プロパティを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="3c4c6-154">これらのメソッドは、データの読み取りに関連するチュートリアルで詳しく検討します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-154">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="3c4c6-155">`AsNoTracking` メソッドは、返されたエンティティが現在のコンテキストで更新されない場合のシナリオでパフォーマンスを改善します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="3c4c6-156">`AsNoTracking` は、このチュートリアルで後述します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="3c4c6-157">[詳細] ページに関連する登録を表示する</span><span class="sxs-lookup"><span data-stu-id="3c4c6-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="3c4c6-158">*Pages/Students/Details.cshtml* を開きます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="3c4c6-159">登録の一覧を表示するために、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <span data-ttu-id="3c4c6-160"><!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]</span><span class="sxs-lookup"><span data-stu-id="3c4c6-160"><!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]</span></span>

<span data-ttu-id="3c4c6-161">コードを貼り付けた後でコードのインデントが乱れた場合は、CTRL + D + K キーを押してそれを修正します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="3c4c6-162">上記のコードは、`Enrollments` ナビゲーション プロパティ内でエンティティをループ処理します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="3c4c6-163">登録ごとに、コースのタイトルとグレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="3c4c6-164">コース タイトルは、Enrollments エンティティの `Course` ナビゲーション プロパティに格納されている Course エンティティから取得されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="3c4c6-165">アプリを実行し、**[Students]** タブを選択し、学生用の **[詳細]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="3c4c6-166">選択した受講者のコースとグレードの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="3c4c6-167">[作成] ページを更新する</span><span class="sxs-lookup"><span data-stu-id="3c4c6-167">Update the Create page</span></span>

<span data-ttu-id="3c4c6-168">次のコードを使用して、*Pages/Students/Create.cshtml.cs* 内の `OnPostAsync` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="3c4c6-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="3c4c6-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="3c4c6-170">次の [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) コードを検討します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-170">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="3c4c6-171">上記のコードで、`TryUpdateModelAsync<Student>` は、[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0) の [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) プロパティからのポストされたフォームの値を使用して `emptyStudent` オブジェクトの更新を試みます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="3c4c6-172">`TryUpdateModelAsync` は、リストされたプロパティのみを更新します (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="3c4c6-173">上記のサンプルについて:</span><span class="sxs-lookup"><span data-stu-id="3c4c6-173">In the preceding sample:</span></span>

* <span data-ttu-id="3c4c6-174">2 番目の引数 (` "student", // Prefix`) には、値を検索するためのプレフィックスが使用されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="3c4c6-175">大文字と小文字の区別はありません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-175">It's not case sensitive.</span></span>
* <span data-ttu-id="3c4c6-176">ポストされたフォームの値は、[モデル バインディング](xref:mvc/models/model-binding#how-model-binding-works) を使用して `Student` モデル内の型に変換されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="3c4c6-177">過剰ポスティング</span><span class="sxs-lookup"><span data-stu-id="3c4c6-177">Overposting</span></span>

<span data-ttu-id="3c4c6-178">ポストされた値を持つフィールドを更新するために `TryUpdateModel` を使用することは、過剰ポスティングの防止につながり、セキュリティ上のベスト プラクティスとなります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="3c4c6-179">たとえば、Student エンティティには、この Web ページで更新または追加できない `Secret` プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-179">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="3c4c6-180">アプリの [作成]/[更新] Razor ページに `Secret` フィールドが含まれていない場合でも、ハッカーは過剰ポスティングによって `Secret` を設定することが可能です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="3c4c6-181">ハッカーは、Fiddler などのツールを使用するか、または何らかの JavaScript を作成して、`Secret` フォーム値をポストすることが可能です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="3c4c6-182">元のコードでは、Student インスタンスの作成時にモデル バインダーによって使用されるフィールドを制限していません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="3c4c6-183">`Secret` フォーム フィールドに対してハッカーが指定した値はいずれも、DB 内で更新されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="3c4c6-184">次の図では、Fiddler ツールを使用して、ポストされたフォームの値に `Secret` フィールド (値 "OverPost" を含む) が追加されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler による Secret フィールドの追加](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="3c4c6-186">挿入された行の `Secret` プロパティに値 "OverPost" が正常に追加されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="3c4c6-187">アプリ デザイナーは、[作成] ページで `Secret` プロパティが設定されることを想定していませんでした。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="3c4c6-188">ビュー モデル</span><span class="sxs-lookup"><span data-stu-id="3c4c6-188">View model</span></span>

<span data-ttu-id="3c4c6-189">ビュー モデルには、通常、アプリケーションで使用されるモデルに含まれるプロパティのサブセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="3c4c6-190">アプリケーション モデルは、しばしばドメイン モデルと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-190">The application model is often called the domain model.</span></span> <span data-ttu-id="3c4c6-191">ドメイン モデルには、通常、データベース内の対応するエンティティによって必要とされるすべてのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="3c4c6-192">ビュー モデルには、UI レイヤーで必要なプロパティのみが含まれています (たとえば、[作成] ページ)。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="3c4c6-193">一部のアプリでは、ビュー モデルに加えて、Razor ページのページ モデル クラスとブラウザーとの間でデータを渡すためにバインド モデルまたは入力モデルも使用します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="3c4c6-194">次の `Student` ビュー モデルを考えてみます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-194">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="3c4c6-195">ビュー モデルは、過剰ポスティングを防ぐもう 1 つの方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="3c4c6-196">ビュー モデルには、表示または更新されるプロパティのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="3c4c6-197">次のコードでは、`StudentVM` ビュー モデルを使用して新しい受講生を作成します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3c4c6-198">[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) メソッドでは、このオブジェクトの値を設定するために、別の [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) オブジェクトから値を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-198">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="3c4c6-199">`SetValues` では一致するプロパティ名が使用されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="3c4c6-200">ビュー モデルの型はモデルの型に関連している必要はなく、プロパティが一致している必要があるだけです。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="3c4c6-201">`StudentVM` を使用するには、`Student` ではなく `StudentVM` を使用するように [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="3c4c6-202">Razor ページで、`PageModel` 派生クラスはビュー モデルです。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="3c4c6-203">Edit ページを更新する</span><span class="sxs-lookup"><span data-stu-id="3c4c6-203">Update the Edit page</span></span>

<span data-ttu-id="3c4c6-204">[編集] ページのページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-204">Update the page model for the Edit page.</span></span> <span data-ttu-id="3c4c6-205">大きな変更点が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-205">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="3c4c6-206">コードの変更は [作成] ページに似ています。ただし、次のようないくつかの例外があります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-206">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="3c4c6-207">`OnPostAsync` には省略可能な `id` パラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-207">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="3c4c6-208">現在の受講者は、空の受講者を作成するのではなく、DB からフェッチされます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-208">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="3c4c6-209">`FirstOrDefaultAsync` は、[FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0) に置換されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-209">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="3c4c6-210">主キーからエンティティを選択する場合は、`FindAsync` をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-210">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="3c4c6-211">詳細については、「[FindAsync](#FindAsync)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-211">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="3c4c6-212">[編集] ページと [作成] ページをテストする</span><span class="sxs-lookup"><span data-stu-id="3c4c6-212">Test the Edit and Create pages</span></span>

<span data-ttu-id="3c4c6-213">いくつかの受講者エンティティを作成して編集します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-213">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="3c4c6-214">エンティティの状態</span><span class="sxs-lookup"><span data-stu-id="3c4c6-214">Entity States</span></span>

<span data-ttu-id="3c4c6-215">DB コンテキストは、メモリ内のエンティティが、DB 内の対応する行と同期状態にあるかどうかを追跡します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-215">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="3c4c6-216">DB コンテキストの同期情報から、`SaveChanges` が呼び出されたときに何が起こっているかを特定できます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-216">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="3c4c6-217">たとえば、新しいエンティティが `Add` メソッドに渡されたとき、そのエンティティの状態は `Added` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-217">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="3c4c6-218">`SaveChanges` が呼び出されると、DB コンテキストは SQL の INSERT コマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-218">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="3c4c6-219">エンティティの状態は、次のいずれかになる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-219">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="3c4c6-220">`Added`: エンティティは DB にまだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-220">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="3c4c6-221">`SaveChanges` メソッドは INSERT ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-221">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="3c4c6-222">`Unchanged`: このエンティティでは変更を保存する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-222">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="3c4c6-223">エンティティがこの状態になるのは、エンティティが DB から読み取られた場合です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-223">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="3c4c6-224">`Modified`: エンティティのプロパティ値の一部またはすべてが変更されています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-224">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="3c4c6-225">`SaveChanges` メソッドは UPDATE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-225">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="3c4c6-226">`Deleted`: エンティティには削除のマークが付けられています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-226">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="3c4c6-227">`SaveChanges` メソッドは DELETE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-227">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="3c4c6-228">`Detached`: エンティティは DB コンテキストによって追跡されていません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-228">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="3c4c6-229">デスクトップ アプリにおいて、通常、状態の変更は自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-229">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="3c4c6-230">エンティティが読み取られ、変更が加えられると、エンティティの状態は自動的に `Modified` に変更されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-230">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="3c4c6-231">`SaveChanges` を呼び出すと、変更されたプロパティのみを更新する SQL UPDATE ステートメントが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-231">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="3c4c6-232">Web アプリにおいて、エンティティを読み取り、データを表示する `DbContext` は、ページが表示された後で破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-232">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="3c4c6-233">ページの `OnPostAsync` メソッドが呼び出されると、新しい Web 要求が行われ、`DbContext` の新しいインスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-233">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="3c4c6-234">その新しいコンテキスト内のエンティティの再読み取りを行うと、デスクトップの処理がシミュレートされます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-234">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="3c4c6-235">[削除] ページを更新する</span><span class="sxs-lookup"><span data-stu-id="3c4c6-235">Update the Delete page</span></span>

<span data-ttu-id="3c4c6-236">このセクションでは、`SaveChanges` の呼び出しが失敗したときにカスタム エラー メッセージを実装するためのコードが追加されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-236">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="3c4c6-237">考えられるエラー メッセージを含められるように文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-237">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="3c4c6-238">`OnGetAsync` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-238">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="3c4c6-239">上記のコードには、省略可能なパラメーター `saveChangesError` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-239">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="3c4c6-240">`saveChangesError` は、受講者オブジェクトの削除に失敗した後で、メソッドが呼び出されたかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-240">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="3c4c6-241">一時的なネットワークの問題により、削除操作が失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-241">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="3c4c6-242">一時的なネットワーク エラーが高い可能性でクラウド内に発生しています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-242">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="3c4c6-243">[削除] ページの `OnGetAsync` が UI から呼び出された場合、`saveChangesError` は false です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-243">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="3c4c6-244">`OnPostAsync` によって `OnGetAsync` が呼び出された場合 (削除操作が失敗したため)、`saveChangesError` パラメーターは true です。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-244">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="3c4c6-245">[削除] ページの OnPostAsync メソッド</span><span class="sxs-lookup"><span data-stu-id="3c4c6-245">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="3c4c6-246">`OnPostAsync` を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-246">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3c4c6-247">上記のコードでは、選択されたエンティティを取得してから、`Remove` メソッドを呼び出してエンティティの状態を `Deleted` に設定しています。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-247">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="3c4c6-248">`SaveChanges` が呼び出されると、SQL DELETE コマンドが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-248">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="3c4c6-249">`Remove` が失敗した場合:</span><span class="sxs-lookup"><span data-stu-id="3c4c6-249">If `Remove` fails:</span></span>

* <span data-ttu-id="3c4c6-250">DB 例外がキャッチされます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-250">The DB exception is caught.</span></span>
* <span data-ttu-id="3c4c6-251">[削除] ページの `OnGetAsync` メソッドが、`saveChangesError=true` を指定して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-251">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="3c4c6-252">[削除] Razor ページを更新する</span><span class="sxs-lookup"><span data-stu-id="3c4c6-252">Update the Delete Razor Page</span></span>

<span data-ttu-id="3c4c6-253">次の強調表示されたエラー メッセージを [削除] Razor ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-253">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="3c4c6-254">[削除] をテストします。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-254">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="3c4c6-255">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="3c4c6-255">Common errors</span></span>

<span data-ttu-id="3c4c6-256">[受講者]/[ホーム] またはその他のリンクが機能しません。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-256">Student/Home or other links don't work:</span></span>

<span data-ttu-id="3c4c6-257">Razor ページに正しい `@page` ディレクティブが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-257">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="3c4c6-258">たとえば、[受講者]/[ホーム] Razor ページにルート テンプレートを含めることは**できません**。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-258">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="3c4c6-259">各 Razor ページには、`@page` ディレクティブを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c4c6-259">Each Razor Page must include the `@page` directive.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c4c6-260">[前へ](xref:data/ef-rp/intro)
> [次へ](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="3c4c6-260">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
