---
title: "Razor ページの EF コアの関連データ - 8 個の更新します。"
author: rick-anderson
description: "このチュートリアルでは外部キー フィールドとナビゲーション プロパティを更新することによって関連するデータを更新します。"
keywords: "ASP.NET Core、Entity Framework Core では、関連するデータの結合します。"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: f07a33c19ba1be623fae14228f8fbc909d766816
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2017
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="14f4f-104">関連するデータの EF コア Razor ページ (8 の 7) の更新</span><span class="sxs-lookup"><span data-stu-id="14f4f-104">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="14f4f-105">によって[Tom Dykstra](https://github.com/tdykstra)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14f4f-105">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="14f4f-106">このチュートリアルでは、関連するデータの更新を示します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-106">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="14f4f-107">問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7)です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="14f4f-108">次の図は、完了したページの一部を示します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="14f4f-109">![コースの編集 ページ](update-related-data/_static/course-edit.png)
![インストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="14f4f-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="14f4f-110">確認し、テストを作成および編集コース ページ。</span><span class="sxs-lookup"><span data-stu-id="14f4f-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="14f4f-111">新しいコースを作成します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-111">Create a new course.</span></span> <span data-ttu-id="14f4f-112">その主キー (整数)、名ではなく、部門が選択されます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="14f4f-113">新しいコースを編集します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-113">Edit the new course.</span></span> <span data-ttu-id="14f4f-114">テストが完了したら、新しいコースを削除します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="14f4f-115">一般的なコードを共有する基本クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-115">Create a base class to share common code</span></span>

<span data-ttu-id="14f4f-116">コース/作成およびコース/編集のそれぞれのページには、部門名の一覧が必要があります。</span><span class="sxs-lookup"><span data-stu-id="14f4f-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="14f4f-117">作成、 *Pages/Courses/DepartmentNamePageModel.cshtml.cs*作成および編集するページの基本クラス。</span><span class="sxs-lookup"><span data-stu-id="14f4f-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="14f4f-118">上記のコードを作成、 [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0)部門名の一覧を格納します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-118">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="14f4f-119">場合`selectedDepartment`を指定すると、その部門が選択されて、`SelectList`です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="14f4f-120">作成および編集ページのモデル クラスが派生`DepartmentNamePageModel`です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="14f4f-121">コースのページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-121">Customize the Courses Pages</span></span>

<span data-ttu-id="14f4f-122">コースの新しいエンティティが作成されると、既存の部署とのリレーションシップが必要です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="14f4f-123">コースを作成するときに、部署を追加するには、基本クラス作成および編集するにはには、部門を選択するためのドロップダウン リストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="14f4f-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="14f4f-124">ドロップダウン リストを設定、`Course.DepartmentID`外部キー (FK) プロパティです。</span><span class="sxs-lookup"><span data-stu-id="14f4f-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="14f4f-125">EF コアを使用して、`Course.DepartmentID`を読み込む FK、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="14f4f-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![コースを作成します。](update-related-data/_static/ddl.png)

<span data-ttu-id="14f4f-127">次のコードで、モデルの作成 ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-127">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="14f4f-128">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-128">The preceding code:</span></span>

* <span data-ttu-id="14f4f-129">`DepartmentNamePageModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="14f4f-130">使用して`TryUpdateModelAsync`を防ぐために[過剰ポスティング](xref:data/ef-rp/crud#overposting)です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="14f4f-131">置換`ViewData["DepartmentID"]`で`DepartmentNameSL`(から基底クラス)。</span><span class="sxs-lookup"><span data-stu-id="14f4f-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="14f4f-132">`ViewData["DepartmentID"]`交換が厳密に型指定と`DepartmentNameSL`です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="14f4f-133">厳密に型指定されたモデルは、厳密に型指定経由で優先されます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="14f4f-134">詳細については、次を参照してください。[弱く型指定されたデータ (ViewData および ViewBag)](xref:mvc/views/overview#VD_VB)です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="14f4f-135">更新のコースを作成 ページ</span><span class="sxs-lookup"><span data-stu-id="14f4f-135">Update the Courses Create page</span></span>

<span data-ttu-id="14f4f-136">更新*Pages/Courses/Create.cshtml*次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="14f4f-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="14f4f-137">上記のマークアップでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="14f4f-138">キャプションを変更**DepartmentID**に**部門**です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="14f4f-139">置換`"ViewBag.DepartmentID"`で`DepartmentNameSL`(から基底クラス)。</span><span class="sxs-lookup"><span data-stu-id="14f4f-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="14f4f-140">"選択 Department"オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="14f4f-141">この変更は、最初の部署ではなく「部署の選択」を表示します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="14f4f-142">部門が選択されていない場合は、検証メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-142">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="14f4f-143">Razor ページを使用して、[タグ ヘルパーの選択](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="14f4f-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="14f4f-144">[作成] ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-144">Test the Create page.</span></span> <span data-ttu-id="14f4f-145">[作成] ページは、部門 ID ではなく、部門名が表示されます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="14f4f-146">コースの編集 ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="14f4f-147">次のコード編集ページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-147">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="14f4f-148">変更は、モデルの作成 ページで行われたものと似ています。</span><span class="sxs-lookup"><span data-stu-id="14f4f-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="14f4f-149">上記のコードで`PopulateDepartmentsDropDownList`パス部門 ID のドロップダウン リストで指定された部門を選択します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="14f4f-150">更新*Pages/Courses/Edit.cshtml*次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="14f4f-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="14f4f-151">上記のマークアップでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="14f4f-152">コース ID を表示します</span><span class="sxs-lookup"><span data-stu-id="14f4f-152">Displays the course ID.</span></span> <span data-ttu-id="14f4f-153">一般に、エンティティの主キー (PK) は表示されません。</span><span class="sxs-lookup"><span data-stu-id="14f4f-153">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="14f4f-154">Pk がユーザーに意味をなしません。</span><span class="sxs-lookup"><span data-stu-id="14f4f-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="14f4f-155">この例では、主キーは、コース数です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="14f4f-156">キャプションを変更**DepartmentID**に**部門**です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="14f4f-157">置換`"ViewBag.DepartmentID"`で`DepartmentNameSL`(から基底クラス)。</span><span class="sxs-lookup"><span data-stu-id="14f4f-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="14f4f-158">"選択 Department"オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-158">Adds the "Select Department" option.</span></span> <span data-ttu-id="14f4f-159">この変更は、最初の部署ではなく「部署の選択」を表示します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-159">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="14f4f-160">部門が選択されていない場合は、検証メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-160">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="14f4f-161">ページには、非表示フィールドが含まれています (`<input type="hidden">`) コース数。</span><span class="sxs-lookup"><span data-stu-id="14f4f-161">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="14f4f-162">追加する、`<label>`タグ ヘルパーを`asp-for="Course.CourseID"`隠しフィールドの必要性を排除しません。</span><span class="sxs-lookup"><span data-stu-id="14f4f-162">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="14f4f-163">`<input type="hidden">`ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号に必要な**保存**です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-163">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="14f4f-164">更新されたコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-164">Test the updated code.</span></span> <span data-ttu-id="14f4f-165">作成、編集、およびコースを削除します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-165">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="14f4f-166">詳細を AsNoTracking を追加してページ モデルの削除</span><span class="sxs-lookup"><span data-stu-id="14f4f-166">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="14f4f-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__)追跡が必要ない場合に、パフォーマンスを向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-167">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="14f4f-168">追加`AsNoTracking`Delete と詳細ページのモデルにします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-168">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="14f4f-169">次のコードは、更新の削除 ページのモデルを示しています。</span><span class="sxs-lookup"><span data-stu-id="14f4f-169">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="14f4f-170">更新プログラム、`OnGetAsync`メソッドで、 *Pages/Courses/Details.cshtml.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="14f4f-170">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="14f4f-171">削除と詳細のページを変更します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-171">Modify the Delete and Details pages</span></span>

<span data-ttu-id="14f4f-172">次のマークアップを削除 Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-172">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="14f4f-173">[詳細] ページに同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="14f4f-173">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="14f4f-174">コース ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-174">Test the Course pages</span></span>

<span data-ttu-id="14f4f-175">テストを作成、編集、詳細、および削除します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-175">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="14f4f-176">講師ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-176">Update the instructor pages</span></span>

<span data-ttu-id="14f4f-177">次のセクションでは、インストラクター ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-177">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="14f4f-178">オフィスの場所を追加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-178">Add office location</span></span>

<span data-ttu-id="14f4f-179">講師レコードを編集するには、インストラクターのオフィス割り当てを更新することがあります。</span><span class="sxs-lookup"><span data-stu-id="14f4f-179">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="14f4f-180">`Instructor`エンティティと 0 または 1 を 1 つの関係には、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="14f4f-180">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="14f4f-181">講師コードを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="14f4f-181">The instructor code must handle:</span></span>

* <span data-ttu-id="14f4f-182">ユーザーがオフィス割り当てをクリアする場合は、削除、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="14f4f-182">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="14f4f-183">場合は、ユーザーがオフィス割り当てを入力し、空か、作成、新しい`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="14f4f-183">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="14f4f-184">ユーザーは、office の割り当てを変更する場合は、更新、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="14f4f-184">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="14f4f-185">次のコードで講習においてインストラクター編集ページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-185">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="14f4f-186">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-186">The preceding code:</span></span>

- <span data-ttu-id="14f4f-187">現在の取得`Instructor`の一括読み込みを使用して、データベースからのエンティティ、`OfficeAssignment`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="14f4f-187">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="14f4f-188">更新、取得した`Instructor`モデル バインダーから値を持つエンティティ。</span><span class="sxs-lookup"><span data-stu-id="14f4f-188">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="14f4f-189">`TryUpdateModel`により、[過剰ポスティング](xref:data/ef-rp/crud#overposting)です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-189">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="14f4f-190">オフィスの場所が空白の場合は、設定`Instructor.OfficeAssignment`を null にします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-190">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="14f4f-191">ときに`Instructor.OfficeAssignment`は null にすると、関連する行で、`OfficeAssignment`テーブルを削除します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-191">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="14f4f-192">講師の編集 ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-192">Update the instructor Edit page</span></span>

<span data-ttu-id="14f4f-193">更新*Pages/Instructors/Edit.cshtml*オフィスの場所で。</span><span class="sxs-lookup"><span data-stu-id="14f4f-193">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="14f4f-194">講習においてインストラクター オフィスの場所を変更することを確認します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-194">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="14f4f-195">コースの課題をインストラクターの編集 ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-195">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="14f4f-196">講習においてインストラクターには、コースの任意の数を教えることがあります。</span><span class="sxs-lookup"><span data-stu-id="14f4f-196">Instructors may teach any number of courses.</span></span> <span data-ttu-id="14f4f-197">このセクションでは、コースの割り当てを変更する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-197">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="14f4f-198">次の図は、更新されたインストラクターの編集 ページを示しています。</span><span class="sxs-lookup"><span data-stu-id="14f4f-198">The following image shows the updated instructor Edit page:</span></span>

![コースをインストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="14f4f-200">`Course`および`Instructor`は多対多リレーションシップを持ちます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-200">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="14f4f-201">追加し、リレーションシップを削除、追加または削除を行うエンティティから、`CourseAssignments`エンティティ セットに参加します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-201">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="14f4f-202">チェック ボックスは、コースに割り当てられている、インストラクターへの変更を有効にします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-202">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="14f4f-203">データベース内のすべてのコースにチェック ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-203">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="14f4f-204">割り当てられているインストラクター コースがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-204">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="14f4f-205">ユーザーでは、選択したり、コースの割り当てを変更する チェック ボックスをオフにすることができます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-205">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="14f4f-206">コースの数が非常に大きい: 場合</span><span class="sxs-lookup"><span data-stu-id="14f4f-206">If the number of courses were much greater:</span></span>

* <span data-ttu-id="14f4f-207">おそらく、コースを表示するのに別のユーザー インターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-207">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="14f4f-208">リレーションシップを作成または削除の結合エンティティを操作するためのメソッドは変化しません。</span><span class="sxs-lookup"><span data-stu-id="14f4f-208">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="14f4f-209">サポートするクラスを追加作成し、インストラクター ページの編集</span><span class="sxs-lookup"><span data-stu-id="14f4f-209">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="14f4f-210">作成*SchoolViewModels/AssignedCourseData.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="14f4f-210">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="14f4f-211">`AssignedCourseData`クラスには、あるインストラクターによって割り当てられたコースのチェック ボックスを作成するデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="14f4f-211">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="14f4f-212">作成、 *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*基本クラス。</span><span class="sxs-lookup"><span data-stu-id="14f4f-212">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="14f4f-213">`InstructorCoursesPageModel`基本クラスで、編集を使用してページ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-213">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="14f4f-214">`PopulateAssignedCourseData`すべてを読み込み`Course`を設定するエンティティ`AssignedCourseDataList`です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-214">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="14f4f-215">各コースに、設定するコードを`CourseID`タイトル、および、インストラクター コースに割り当てられるかどうか。</span><span class="sxs-lookup"><span data-stu-id="14f4f-215">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="14f4f-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1)効率的な参照を作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-216">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="14f4f-217">講習においてインストラクター編集ページのモデル</span><span class="sxs-lookup"><span data-stu-id="14f4f-217">Instructors Edit page model</span></span>

<span data-ttu-id="14f4f-218">次のコードで、インストラクター編集ページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-218">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="14f4f-219">上記のコードでは、office の割り当て変更を処理します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-219">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="14f4f-220">講師 Razor ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-220">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="14f4f-221">Visual Studio でコードを貼り付けるときに改行コードを中断するように変更されます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-221">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="14f4f-222">Ctrl + Z を 1 回押して、オート フォーマットを元に戻します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-222">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="14f4f-223">Ctrl + Z は、ここに表示するように表示されるように、改行を修正します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-223">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="14f4f-224">インデント完璧なのに、する必要はありませんが、 `@</tr><tr>`、 `@:<td>`、 `@:</td>`、および`@:</tr>`行ことはできません。 1 行に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-224">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="14f4f-225">選択された新しいコードのブロックして、Tab キーを押して 3 回、新しいコードと既存のコードの行にします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-225">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="14f4f-226">投票またはこのバグの状態を確認[で次のリンク](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-226">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="14f4f-227">上記のコードでは、次の 3 つの列を含んでいる HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-227">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="14f4f-228">各列には、チェック ボックスとコース番号とタイトルを含むキャプション。</span><span class="sxs-lookup"><span data-stu-id="14f4f-228">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="14f4f-229">すべてのチェック ボックスは、同じ名前 ("selectedCourses") を持ちます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-229">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="14f4f-230">同じ名前を使用するには、グループとして扱われるようにする、モデル バインダーが通知されます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-230">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="14f4f-231">各チェック ボックスの値の属性に設定されて`CourseID`です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-231">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="14f4f-232">モデル バインダーがで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスだけが選択されている値。</span><span class="sxs-lookup"><span data-stu-id="14f4f-232">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="14f4f-233">チェック ボックスが最初に表示されているコース インストラクターに割り当てられている属性がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-233">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="14f4f-234">アプリを実行し、更新された講習においてインストラクターの編集 ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-234">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="14f4f-235">一部のコース割り当てを変更します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-235">Change some course assignments.</span></span> <span data-ttu-id="14f4f-236">インデックス ページには、変更が反映されます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-236">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="14f4f-237">注: インストラクター コースのデータを編集するのには、ここに採用されているアプローチは、コースの数に制限がある場合にも動作します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-237">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="14f4f-238">非常に大きいコレクションを別の UI と更新の別の方法になりますより使用可能かつ効率的です。</span><span class="sxs-lookup"><span data-stu-id="14f4f-238">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="14f4f-239">講習においてインストラクター作成ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-239">Update the instructors Create page</span></span>

<span data-ttu-id="14f4f-240">モデルを更新するインストラクター作成ページを次のコード。</span><span class="sxs-lookup"><span data-stu-id="14f4f-240">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="14f4f-241">上記のコードがに似ていますが、 *Pages/Instructors/Edit.cshtml.cs*コード。</span><span class="sxs-lookup"><span data-stu-id="14f4f-241">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="14f4f-242">次のマークアップ、インストラクター作成 Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-242">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="14f4f-243">講師の作成 ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-243">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="14f4f-244">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="14f4f-244">Update the Delete page</span></span>

<span data-ttu-id="14f4f-245">次のコードで Delete ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-245">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="14f4f-246">上記のコードでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="14f4f-246">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="14f4f-247">一括読み込みを使用して、`CourseAssignments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="14f4f-247">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="14f4f-248">`CourseAssignments`含める必要があるインストラクターが削除されたときに削除されないまたはします。</span><span class="sxs-lookup"><span data-stu-id="14f4f-248">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="14f4f-249">それらを読み取る必要はありませんを回避するのには、データベースで連鎖削除を構成します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-249">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="14f4f-250">講師が削除されますが、部門の管理者として割り当てられている場合は、その部門からインストラクター割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="14f4f-250">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="14f4f-251">[前へ](xref:data/ef-rp/read-related-data)
[次へ](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="14f4f-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>