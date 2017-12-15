---
title: "Razor ページの EF コア CRUD - 2 8"
author: rick-anderson
description: "作成、読み取り、更新、EF コアで削除する方法を示します"
keywords: "ASP.NET Core、Entity Framework Core、CRUD、作成、読み取り、更新、削除"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: b4b24c155c29a0ef8ffffda752253f56097e50ed
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="2fafd-104">作成、読み取り、更新、および削除 - EF コア Razor ページ (2/8)</span><span class="sxs-lookup"><span data-stu-id="2fafd-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="2fafd-105">によって[Tom Dykstra](https://github.com/tdykstra)、 [Jon P Smith](https://twitter.com/thereformedprog)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2fafd-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="2fafd-106">このチュートリアルでは、スキャフォールディング CRUD (作成、読み取り、更新、削除) コードの確認し、カスタマイズができます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="2fafd-107">注: EF コアに重点を置いてこれらのチュートリアルの変更に複雑さを軽減するには、EF コア コードが使用 Razor ページの分離コード ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="2fafd-108">一部の開発者で、サービス レイヤー、またはリポジトリ パターンを使用して、UI (Razor ページ) とデータ アクセス層の間で抽象化レイヤーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="2fafd-109">このチュートリアル、作成、編集、削除、および詳細 Razor ページで、*学生*フォルダーを変更します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="2fafd-110">スキャフォールディングのコードでは、作成、編集、および削除のページの次のパターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="2fafd-111">取得し、HTTP GET メソッドを使用して、要求されたデータを表示`OnGetAsync`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="2fafd-112">HTTP POST メソッドを使用してデータへの変更を保存`OnPostAsync`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="2fafd-113">インデックスおよび詳細ページが取得し、HTTP GET メソッドを使用して、要求されたデータを表示`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="2fafd-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="2fafd-114">FirstOrDefaultAsync で SingleOrDefaultAsync を置き換えます</span><span class="sxs-lookup"><span data-stu-id="2fafd-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="2fafd-115">生成されたコードを使用して[SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)を要求されたエンティティを取得します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="2fafd-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) 1 つのエンティティのフェッチでより効率的です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="2fafd-117">コードがあることを確認する必要がありますいない限り、クエリから返される 1 つ以上のエンティティはありませんがあります。</span><span class="sxs-lookup"><span data-stu-id="2fafd-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="2fafd-118">`SingleOrDefaultAsync`多くのデータをフェッチし、不要な作業を行います。</span><span class="sxs-lookup"><span data-stu-id="2fafd-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="2fafd-119">`SingleOrDefaultAsync`フィルター部品に適合する 1 つ以上のエンティティがある場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="2fafd-120">`FirstOrDefaultAsync`フィルター部品に適合する 1 つ以上のエンティティがある場合は、例外がスローされません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="2fafd-121">グローバルに置き換える`SingleOrDefaultAsync`で`FirstOrDefaultAsync`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="2fafd-122">`SingleOrDefaultAsync`5 桁で使用されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="2fafd-123">`OnGetAsync`[詳細] ページにします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="2fafd-124">`OnGetAsync`および`OnPostAsync`編集および削除のページにします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="2fafd-125">FindAsync</span><span class="sxs-lookup"><span data-stu-id="2fafd-125">FindAsync</span></span>

<span data-ttu-id="2fafd-126">スキャフォールディングのコードの多くで[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___)の代わりに使用できる`FirstOrDefaultAsync`または`SingleOrDefaultAsync`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="2fafd-127">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="2fafd-127">`FindAsync`:</span></span>

* <span data-ttu-id="2fafd-128">主キー (PK) を持つエンティティを検索します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="2fafd-129">主キーを持つエンティティはコンテキストによって追跡されているが場合、は、要求なし、DB に返されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="2fafd-130">単純で簡潔なです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-130">Is simple and concise.</span></span>
* <span data-ttu-id="2fafd-131">1 つのエンティティの検索に最適化されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="2fafd-132">できますが、一部の状況でパフォーマンスの利点があります、通常の web シナリオの再生になることはほとんどありません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="2fafd-133">暗黙的に使用[FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)の代わりに[SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="2fafd-134">他のエンティティを追加するかどうかは、検索に不要になったが、適切なします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="2fafd-135">つまりに検索を破棄し、アプリの進行状況に応じて、クエリに移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2fafd-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="2fafd-136">[詳細] ページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-136">Customize the Details page</span></span>

<span data-ttu-id="2fafd-137">参照`Pages/Students`ページ。</span><span class="sxs-lookup"><span data-stu-id="2fafd-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="2fafd-138">**編集**、**詳細**、および**削除**へのリンクがによって生成される、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)で、*ページ/受講者/Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2fafd-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="2fafd-139">詳細情報のリンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-139">Select a Details link.</span></span> <span data-ttu-id="2fafd-140">URL の形式は、`http://localhost:5000/Students/Details?id=2`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="2fafd-141">クエリ文字列を使用して、学生 ID が渡されます (`?id=2`)。</span><span class="sxs-lookup"><span data-stu-id="2fafd-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="2fafd-142">更新を使用するには、編集、詳細、および Razor ページの削除、`"{id:int}"`ルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="2fafd-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="2fafd-143">これらの各ページのページ ディレクティブを `@page` から `@page "{id:int}"` に変更します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="2fafd-144">"{Id: int}"のルート テンプレートを持つページへの要求**いない**整数ルート値を返します、HTTP 404 (見つかりません) エラーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="2fafd-145">たとえば、 `http://localhost:5000/Students/Details` 404 エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="2fafd-146">ID を省略するには、次のように `?` をルート制約に追加します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="2fafd-147">アプリを実行、詳細リンクをクリックして、URL がルート データとして ID を渡すことを確認してください (`http://localhost:5000/Students/Details/2`)。</span><span class="sxs-lookup"><span data-stu-id="2fafd-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="2fafd-148">グローバルに変更しない`@page`に`@page "{id:int}"`ホームへのリンクの解除を行って、およびページを作成します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="2fafd-149">関連するデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-149">Add related data</span></span>

<span data-ttu-id="2fafd-150">受講者インデックス ページのスキャフォールディング コードを含まない、`Enrollments`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="2fafd-151">このセクションの内容で、`Enrollments`コレクションは、詳細ページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="2fafd-152">`OnGetAsync`メソッドの*Pages/Students/Details.cshtml.cs*を使用して、 `FirstOrDefaultAsync` 、1 つを取得する方法を`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="2fafd-153">次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="2fafd-154">`Include`と`ThenInclude`メソッドで発生する読み込みコンテキスト、`Student.Enrollments`ナビゲーション プロパティ、および各登録内で、`Enrollment.Course`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="2fafd-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="2fafd-155">これらのメソッドは、データの読み取りに関連するチュートリアル」で詳しく examinied はします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="2fafd-156">`AsNoTracking`メソッドによって返されるエンティティは、現在のコンテキストでは更新されない時のシナリオでパフォーマンスを向上します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="2fafd-157">`AsNoTracking`このチュートリアルで後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="2fafd-158">[詳細] ページの関連する登録を表示します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="2fafd-159">開いている*Pages/Students/Details.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="2fafd-160">登録の一覧を表示する次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="2fafd-161">コードを貼り付けた後でコードのインデントが正しくない場合は、修正して CTRL-D-K キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="2fafd-162">上記のコードをループ内のエンティティ、`Enrollments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="2fafd-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="2fafd-163">各登録の場合は、コースのタイトルとグレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="2fafd-164">格納されているコース エンティティからコース タイトルが取得される、`Course`登録エンティティのナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="2fafd-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="2fafd-165">アプリを実行する、選択、**受講者**タブをクリックし、をクリックして、**詳細**受講者をリンクします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="2fafd-166">Courses に、選択した学生の成績の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="2fafd-167">更新プログラムの作成 ページ</span><span class="sxs-lookup"><span data-stu-id="2fafd-167">Update the Create page</span></span>

<span data-ttu-id="2fafd-168">更新プログラム、`OnPostAsync`メソッド*Pages/Students/Create.cshtml.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="2fafd-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="2fafd-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="2fafd-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="2fafd-170">確認、 [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_)コード。</span><span class="sxs-lookup"><span data-stu-id="2fafd-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="2fafd-171">上記のコードで`TryUpdateModelAsync<Student>`を更新しようとする、`emptyStudent`オブジェクトからポストされたフォーム値を使用して、 [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext)プロパティに、 [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0)です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="2fafd-172">`TryUpdateModelAsync`プロパティを一覧表示の更新のみ (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="2fafd-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="2fafd-173">上記のサンプル。</span><span class="sxs-lookup"><span data-stu-id="2fafd-173">In the preceding sample:</span></span>

* <span data-ttu-id="2fafd-174">2 番目の引数 (` "student", // Prefix`) は、プレフィックスを使用して値を検索します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="2fafd-175">大文字小文字を区別することはできません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-175">It's not case sensitive.</span></span>
* <span data-ttu-id="2fafd-176">ポストされたフォーム値の型に変換、`Student`モデルを使用して[モデル バインディング](xref:mvc/models/model-binding#how-model-binding-works)です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="2fafd-177">過剰ポスティング</span><span class="sxs-lookup"><span data-stu-id="2fafd-177">Overposting</span></span>

<span data-ttu-id="2fafd-178">使用して`TryUpdateModel`ポストされた値を持つフィールドを更新するにはセキュリティのベスト プラクティスを overposting を防ぐために、します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="2fafd-179">たとえば、受講者エンティティが含まれています、`Secret`プロパティをこの web ページを更新または追加する必要がありますいません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="2fafd-180">アプリを持っていない場合でも、 `Secret` Razor ページでは、ハッカーの作成/更新でフィールドが設定でした、`Secret`過剰ポスティングによっての値。</span><span class="sxs-lookup"><span data-stu-id="2fafd-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="2fafd-181">ハッカーでした、Fiddler などのツールを使用してまたは投稿に、一部の JavaScript を書き込み、`Secret`値を作成します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="2fafd-182">学生インスタンスの作成時にモデル バインダーが使用されるフィールドの制限は、元のコードがありません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="2fafd-183">値の指定されたハッカー、 `Secret` DB のフォーム フィールドを更新します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="2fafd-184">次の図は、Fiddler ツールを追加する、`Secret`ポストされたフォーム値へのフィールド (に値"OverPost") です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler シークレット フィールドの追加](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="2fafd-186">値に"OverPost"を正常に追加、`Secret`挿入された行のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="2fafd-187">アプリケーション デザイナーを想定しなかった、`Secret`の作成 ページで設定されるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="2fafd-188">ビュー モデル</span><span class="sxs-lookup"><span data-stu-id="2fafd-188">View model</span></span>

<span data-ttu-id="2fafd-189">ビュー モデルには、通常、アプリケーションによって使用されるモデルに含まれるプロパティのサブセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2fafd-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="2fafd-190">アプリケーション モデルは、ドメイン モデルと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-190">The application model is often called the domain model.</span></span> <span data-ttu-id="2fafd-191">ドメイン モデルには、通常、データベース内の対応するエンティティに必要なすべてのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2fafd-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="2fafd-192">ビュー モデルには、UI 層 (たとえば、作成 ページ) に必要なプロパティのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2fafd-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="2fafd-193">ビュー モデルだけでなく、一部のアプリケーションは、Razor ページの分離コード クラスとブラウザー間でデータを受け渡すバインド モデルまたは入力モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="2fafd-194">次の検討`Student`ビュー モデル。</span><span class="sxs-lookup"><span data-stu-id="2fafd-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="2fafd-195">ビューのモデルでは、overposting を防ぐために別の方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="2fafd-196">ビュー モデルには、(表示) を表示または更新するプロパティのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2fafd-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="2fafd-197">次のコードでは、`StudentVM`ビュー モデルに新しい学生を作成します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="2fafd-198">[SetValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_)メソッドでは、このオブジェクトの値を設定を別の値を読み取って[PropertyValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="2fafd-198">The [SetValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="2fafd-199">`SetValues`プロパティの名前の一致を使用します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="2fafd-200">ビュー モデルの種類が、モデルの種類に関連している必要がある、一致するプロパティだけが必要です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="2fafd-201">使用して`StudentVM`必要[CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml)に更新する`StudentVM`なく`Student`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="2fafd-202">Razor ページで、`PageModel`派生クラスは、ビュー モデルです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="2fafd-203">更新プログラムの編集 ページ</span><span class="sxs-lookup"><span data-stu-id="2fafd-203">Update the Edit page</span></span>

<span data-ttu-id="2fafd-204">編集ページの分離コード ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="2fafd-205">コードの変更は、いくつかの例外の作成 ページに似ています。</span><span class="sxs-lookup"><span data-stu-id="2fafd-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="2fafd-206">`OnPostAsync`省略可能な`id`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="2fafd-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="2fafd-207">現在の受講者が、DB からフェッチされた空の学生を作成するのではなくです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="2fafd-208">`FirstOrDefaultAsync`置き換えられました[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="2fafd-209">`FindAsync`主キーからエンティティを選択するときに、適切な選択です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="2fafd-210">参照してください[FindAsync](#FindAsync)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="2fafd-211">編集をテストし、ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="2fafd-212">作成し、いくつかの学生エンティティを編集します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="2fafd-213">エンティティの状態</span><span class="sxs-lookup"><span data-stu-id="2fafd-213">Entity States</span></span>

<span data-ttu-id="2fafd-214">DB コンテキストは、メモリ内のエンティティが、DB 内の対応する行との同期であるかどうかの追跡を保持します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="2fafd-215">DB コンテキストの同期情報の動作を決定するときに`SaveChanges`と呼びます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="2fafd-216">たとえば、新しいエンティティが渡されたときにする、`Add`メソッドに設定されているエンティティの状態を`Added`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="2fafd-217">ときに`SaveChanges`が呼び出されると、データベース コンテキストは、SQL の INSERT コマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="2fafd-218">次の状態のいずれかでエンティティがあります。</span><span class="sxs-lookup"><span data-stu-id="2fafd-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="2fafd-219">`Added`: エンティティは、db では、まだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="2fafd-220">`SaveChanges`メソッドは、INSERT ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="2fafd-221">`Unchanged`変更する必要があります: このエンティティで保存されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="2fafd-222">エンティティでは、データベースから読み取られるときにこの状態があります。</span><span class="sxs-lookup"><span data-stu-id="2fafd-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="2fafd-223">`Modified`: 一部またはすべてのエンティティのプロパティの値が変更されています。</span><span class="sxs-lookup"><span data-stu-id="2fafd-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="2fafd-224">`SaveChanges`メソッドは、UPDATE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="2fafd-225">`Deleted`: エンティティは、削除のマークされています。</span><span class="sxs-lookup"><span data-stu-id="2fafd-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="2fafd-226">`SaveChanges`メソッドは、DELETE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="2fafd-227">`Detached`: エンティティは、DB コンテキストによって追跡されているはありません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="2fafd-228">デスクトップ アプリでは、通常状態の変更から自動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="2fafd-229">エンティティの読み取り、変更を加えると、エンティティの状態に自動的に変更する`Modified`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="2fafd-230">呼び出す`SaveChanges`変更されたプロパティのみを更新する SQL UPDATE ステートメントを生成します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="2fafd-231">Web アプリで、`DbContext`を読み取るエンティティと、ページが表示された後、データが破棄されて表示されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="2fafd-232">ときに、ページ`OnPostAsync`メソッドが呼び出されると、新しい web 要求が行われるの新しいインスタンスを使用して、`DbContext`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="2fafd-233">デスクトップの処理をシミュレートする新しいコンテキスト内のエンティティを再読み込みします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="2fafd-234">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="2fafd-234">Update the Delete page</span></span>

<span data-ttu-id="2fafd-235">ここでは、カスタム エラーを実装するコードを追加メッセージへの呼び出し`SaveChanges`は失敗します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="2fafd-236">Possile エラー メッセージを格納する文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-236">Add a string to contain possile error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="2fafd-237">`OnGetAsync` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="2fafd-238">上記のコードには、省略可能なパラメーターが含まれています。`saveChangesError`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="2fafd-239">`saveChangesError`student オブジェクトの削除に失敗した後、メソッドが呼び出されたかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="2fafd-240">一時的なネットワークの問題があるため、削除操作が失敗可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2fafd-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="2fafd-241">ネットワークの一時的なエラーは、クラウドで可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="2fafd-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="2fafd-242">`saveChangesError`ときに、false 削除ページ`OnGetAsync`UI から呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="2fafd-243">ときに`OnGetAsync`によって呼び出される`OnPostAsync`(ため、削除操作が失敗しました)、`saveChangesError`パラメーターが true です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="2fafd-244">Delete ページ OnPostAsync メソッド</span><span class="sxs-lookup"><span data-stu-id="2fafd-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="2fafd-245">置換、`OnPostAsync`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="2fafd-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="2fafd-246">上記のコードは、選択したエンティティを取得し、呼び出して、`Remove`エンティティの状態を設定するメソッドを`Deleted`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="2fafd-247">ときに`SaveChanges`が呼び出されると、SQL の DELETE コマンドを生成します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="2fafd-248">場合`Remove`は失敗します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-248">If `Remove` fails:</span></span>

* <span data-ttu-id="2fafd-249">DB 例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-249">The DB exception is caught.</span></span>
* <span data-ttu-id="2fafd-250">Delete ページ`OnGetAsync`メソッドが呼び出された`saveChangesError=true`です。</span><span class="sxs-lookup"><span data-stu-id="2fafd-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="2fafd-251">Delete Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="2fafd-252">Razor ページの削除するには、次の強調表示されたエラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="2fafd-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="2fafd-253">削除をテストします。</span><span class="sxs-lookup"><span data-stu-id="2fafd-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="2fafd-254">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="2fafd-254">Common errors</span></span>

<span data-ttu-id="2fafd-255">学生/ホームまたはその他のリンクは機能しません。</span><span class="sxs-lookup"><span data-stu-id="2fafd-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="2fafd-256">Razor ページが含まれていますが、正しいことを確認`@page`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="2fafd-257">など、受講者/ホーム Razor ページする必要があります**いない**ルート テンプレートを含めます。</span><span class="sxs-lookup"><span data-stu-id="2fafd-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="2fafd-258">各 Razor ページを含める必要があります、`@page`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="2fafd-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2fafd-259">[前へ](xref:data/ef-rp/intro)
[次へ](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="2fafd-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
