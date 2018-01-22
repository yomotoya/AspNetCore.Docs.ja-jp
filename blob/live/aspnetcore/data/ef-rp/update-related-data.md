---
title: "Razor ページの EF コアの関連データ - 8 個の更新します。"
author: rick-anderson
description: "このチュートリアルでは外部キー フィールドとナビゲーション プロパティを更新することによって関連するデータを更新します。"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 817bfd48dce94e7dbad96cb6f822494e3adfae1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="d0878-103">関連するデータの EF コア Razor ページ (8 の 7) の更新</span><span class="sxs-lookup"><span data-stu-id="d0878-103">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="d0878-104">によって[Tom Dykstra](https://github.com/tdykstra)、および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0878-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d0878-105">このチュートリアルでは、関連するデータの更新を示します。</span><span class="sxs-lookup"><span data-stu-id="d0878-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="d0878-106">問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7)です。</span><span class="sxs-lookup"><span data-stu-id="d0878-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="d0878-107">次の図は、完了したページの一部を示します。</span><span class="sxs-lookup"><span data-stu-id="d0878-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="d0878-108">![コースの編集 ページ](update-related-data/_static/course-edit.png)
![インストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="d0878-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="d0878-109">確認し、テストを作成および編集コース ページ。</span><span class="sxs-lookup"><span data-stu-id="d0878-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="d0878-110">新しいコースを作成します。</span><span class="sxs-lookup"><span data-stu-id="d0878-110">Create a new course.</span></span> <span data-ttu-id="d0878-111">その主キー (整数)、名ではなく、部門が選択されます。</span><span class="sxs-lookup"><span data-stu-id="d0878-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="d0878-112">新しいコースを編集します。</span><span class="sxs-lookup"><span data-stu-id="d0878-112">Edit the new course.</span></span> <span data-ttu-id="d0878-113">テストが完了したら、新しいコースを削除します。</span><span class="sxs-lookup"><span data-stu-id="d0878-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="d0878-114">一般的なコードを共有する基本クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="d0878-114">Create a base class to share common code</span></span>

<span data-ttu-id="d0878-115">コース/作成およびコース/編集のそれぞれのページには、部門名の一覧が必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0878-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="d0878-116">作成、 *Pages/Courses/DepartmentNamePageModel.cshtml.cs*作成および編集するページの基本クラス。</span><span class="sxs-lookup"><span data-stu-id="d0878-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="d0878-117">上記のコードを作成、 [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0)部門名の一覧を格納します。</span><span class="sxs-lookup"><span data-stu-id="d0878-117">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="d0878-118">場合`selectedDepartment`を指定すると、その部門が選択されて、`SelectList`です。</span><span class="sxs-lookup"><span data-stu-id="d0878-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="d0878-119">作成および編集ページのモデル クラスが派生`DepartmentNamePageModel`です。</span><span class="sxs-lookup"><span data-stu-id="d0878-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="d0878-120">コースのページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="d0878-120">Customize the Courses Pages</span></span>

<span data-ttu-id="d0878-121">コースの新しいエンティティが作成されると、既存の部署とのリレーションシップが必要です。</span><span class="sxs-lookup"><span data-stu-id="d0878-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="d0878-122">コースを作成するときに、部署を追加するには、基本クラス作成および編集するにはには、部門を選択するためのドロップダウン リストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d0878-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="d0878-123">ドロップダウン リストを設定、`Course.DepartmentID`外部キー (FK) プロパティです。</span><span class="sxs-lookup"><span data-stu-id="d0878-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="d0878-124">EF コアを使用して、`Course.DepartmentID`を読み込む FK、`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d0878-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![コースを作成します。](update-related-data/_static/ddl.png)

<span data-ttu-id="d0878-126">次のコードで、モデルの作成 ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-126">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="d0878-127">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="d0878-127">The preceding code:</span></span>

* <span data-ttu-id="d0878-128">`DepartmentNamePageModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="d0878-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="d0878-129">使用して`TryUpdateModelAsync`を防ぐために[過剰ポスティング](xref:data/ef-rp/crud#overposting)です。</span><span class="sxs-lookup"><span data-stu-id="d0878-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="d0878-130">置換`ViewData["DepartmentID"]`で`DepartmentNameSL`(から基底クラス)。</span><span class="sxs-lookup"><span data-stu-id="d0878-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="d0878-131">`ViewData["DepartmentID"]`交換が厳密に型指定と`DepartmentNameSL`です。</span><span class="sxs-lookup"><span data-stu-id="d0878-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="d0878-132">厳密に型指定されたモデルは、厳密に型指定経由で優先されます。</span><span class="sxs-lookup"><span data-stu-id="d0878-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="d0878-133">詳細については、次を参照してください。[弱く型指定されたデータ (ViewData および ViewBag)](xref:mvc/views/overview#VD_VB)です。</span><span class="sxs-lookup"><span data-stu-id="d0878-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="d0878-134">更新のコースを作成 ページ</span><span class="sxs-lookup"><span data-stu-id="d0878-134">Update the Courses Create page</span></span>

<span data-ttu-id="d0878-135">更新*Pages/Courses/Create.cshtml*次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="d0878-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="d0878-136">上記のマークアップでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d0878-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d0878-137">キャプションを変更**DepartmentID**に**部門**です。</span><span class="sxs-lookup"><span data-stu-id="d0878-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="d0878-138">置換`"ViewBag.DepartmentID"`で`DepartmentNameSL`(から基底クラス)。</span><span class="sxs-lookup"><span data-stu-id="d0878-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="d0878-139">"選択 Department"オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="d0878-140">この変更は、最初の部署ではなく「部署の選択」を表示します。</span><span class="sxs-lookup"><span data-stu-id="d0878-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="d0878-141">部門が選択されていない場合は、検証メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-141">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="d0878-142">Razor ページを使用して、[タグ ヘルパーの選択](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="d0878-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="d0878-143">[作成] ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="d0878-143">Test the Create page.</span></span> <span data-ttu-id="d0878-144">[作成] ページは、部門 ID ではなく、部門名が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d0878-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="d0878-145">コースの編集 ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="d0878-146">次のコード編集ページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-146">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="d0878-147">変更は、モデルの作成 ページで行われたものと似ています。</span><span class="sxs-lookup"><span data-stu-id="d0878-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="d0878-148">上記のコードで`PopulateDepartmentsDropDownList`パス部門 ID のドロップダウン リストで指定された部門を選択します。</span><span class="sxs-lookup"><span data-stu-id="d0878-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="d0878-149">更新*Pages/Courses/Edit.cshtml*次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="d0878-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="d0878-150">上記のマークアップでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d0878-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="d0878-151">コース ID を表示します</span><span class="sxs-lookup"><span data-stu-id="d0878-151">Displays the course ID.</span></span> <span data-ttu-id="d0878-152">一般に、エンティティの主キー (PK) は表示されません。</span><span class="sxs-lookup"><span data-stu-id="d0878-152">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="d0878-153">Pk がユーザーに意味をなしません。</span><span class="sxs-lookup"><span data-stu-id="d0878-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="d0878-154">この例では、主キーは、コース数です。</span><span class="sxs-lookup"><span data-stu-id="d0878-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="d0878-155">キャプションを変更**DepartmentID**に**部門**です。</span><span class="sxs-lookup"><span data-stu-id="d0878-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="d0878-156">置換`"ViewBag.DepartmentID"`で`DepartmentNameSL`(から基底クラス)。</span><span class="sxs-lookup"><span data-stu-id="d0878-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="d0878-157">"選択 Department"オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="d0878-158">この変更は、最初の部署ではなく「部署の選択」を表示します。</span><span class="sxs-lookup"><span data-stu-id="d0878-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="d0878-159">部門が選択されていない場合は、検証メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-159">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="d0878-160">ページには、非表示フィールドが含まれています (`<input type="hidden">`) コース数。</span><span class="sxs-lookup"><span data-stu-id="d0878-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="d0878-161">追加する、`<label>`タグ ヘルパーを`asp-for="Course.CourseID"`隠しフィールドの必要性を排除しません。</span><span class="sxs-lookup"><span data-stu-id="d0878-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="d0878-162">`<input type="hidden">`ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号に必要な**保存**です。</span><span class="sxs-lookup"><span data-stu-id="d0878-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="d0878-163">更新されたコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="d0878-163">Test the updated code.</span></span> <span data-ttu-id="d0878-164">作成、編集、およびコースを削除します。</span><span class="sxs-lookup"><span data-stu-id="d0878-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="d0878-165">詳細を AsNoTracking を追加してページ モデルの削除</span><span class="sxs-lookup"><span data-stu-id="d0878-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="d0878-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__)追跡が必要ない場合に、パフォーマンスを向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="d0878-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="d0878-167">追加`AsNoTracking`Delete と詳細ページのモデルにします。</span><span class="sxs-lookup"><span data-stu-id="d0878-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="d0878-168">次のコードは、更新の削除 ページのモデルを示しています。</span><span class="sxs-lookup"><span data-stu-id="d0878-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="d0878-169">更新プログラム、`OnGetAsync`メソッドで、 *Pages/Courses/Details.cshtml.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d0878-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="d0878-170">削除と詳細のページを変更します。</span><span class="sxs-lookup"><span data-stu-id="d0878-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="d0878-171">次のマークアップを削除 Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="d0878-172">[詳細] ページに同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="d0878-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="d0878-173">コース ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="d0878-173">Test the Course pages</span></span>

<span data-ttu-id="d0878-174">テストを作成、編集、詳細、および削除します。</span><span class="sxs-lookup"><span data-stu-id="d0878-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="d0878-175">講師ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-175">Update the instructor pages</span></span>

<span data-ttu-id="d0878-176">次のセクションでは、インストラクター ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="d0878-177">オフィスの場所を追加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-177">Add office location</span></span>

<span data-ttu-id="d0878-178">講師レコードを編集するには、インストラクターのオフィス割り当てを更新することがあります。</span><span class="sxs-lookup"><span data-stu-id="d0878-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="d0878-179">`Instructor`エンティティと 0 または 1 を 1 つの関係には、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="d0878-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="d0878-180">講師コードを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0878-180">The instructor code must handle:</span></span>

* <span data-ttu-id="d0878-181">ユーザーがオフィス割り当てをクリアする場合は、削除、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="d0878-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="d0878-182">場合は、ユーザーがオフィス割り当てを入力し、空か、作成、新しい`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="d0878-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="d0878-183">ユーザーは、office の割り当てを変更する場合は、更新、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="d0878-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="d0878-184">次のコードで講習においてインストラクター編集ページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="d0878-185">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="d0878-185">The preceding code:</span></span>

- <span data-ttu-id="d0878-186">現在の取得`Instructor`の一括読み込みを使用して、データベースからのエンティティ、`OfficeAssignment`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d0878-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="d0878-187">更新、取得した`Instructor`モデル バインダーから値を持つエンティティ。</span><span class="sxs-lookup"><span data-stu-id="d0878-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="d0878-188">`TryUpdateModel`により、[過剰ポスティング](xref:data/ef-rp/crud#overposting)です。</span><span class="sxs-lookup"><span data-stu-id="d0878-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="d0878-189">オフィスの場所が空白の場合は、設定`Instructor.OfficeAssignment`を null にします。</span><span class="sxs-lookup"><span data-stu-id="d0878-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="d0878-190">ときに`Instructor.OfficeAssignment`は null にすると、関連する行で、`OfficeAssignment`テーブルを削除します。</span><span class="sxs-lookup"><span data-stu-id="d0878-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="d0878-191">講師の編集 ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-191">Update the instructor Edit page</span></span>

<span data-ttu-id="d0878-192">更新*Pages/Instructors/Edit.cshtml*オフィスの場所で。</span><span class="sxs-lookup"><span data-stu-id="d0878-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="d0878-193">講習においてインストラクター オフィスの場所を変更することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d0878-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="d0878-194">コースの課題をインストラクターの編集 ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="d0878-195">講習においてインストラクターには、コースの任意の数を教えることがあります。</span><span class="sxs-lookup"><span data-stu-id="d0878-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="d0878-196">このセクションでは、コースの割り当てを変更する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="d0878-197">次の図は、更新されたインストラクターの編集 ページを示しています。</span><span class="sxs-lookup"><span data-stu-id="d0878-197">The following image shows the updated instructor Edit page:</span></span>

![コースをインストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="d0878-199">`Course`および`Instructor`は多対多リレーションシップを持ちます。</span><span class="sxs-lookup"><span data-stu-id="d0878-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="d0878-200">追加し、リレーションシップを削除、追加または削除を行うエンティティから、`CourseAssignments`エンティティ セットに参加します。</span><span class="sxs-lookup"><span data-stu-id="d0878-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="d0878-201">チェック ボックスは、コースに割り当てられている、インストラクターへの変更を有効にします。</span><span class="sxs-lookup"><span data-stu-id="d0878-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="d0878-202">データベース内のすべてのコースにチェック ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d0878-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="d0878-203">割り当てられているインストラクター コースがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="d0878-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="d0878-204">ユーザーでは、選択したり、コースの割り当てを変更する チェック ボックスをオフにすることができます。</span><span class="sxs-lookup"><span data-stu-id="d0878-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="d0878-205">コースの数が非常に大きい: 場合</span><span class="sxs-lookup"><span data-stu-id="d0878-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="d0878-206">おそらく、コースを表示するのに別のユーザー インターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d0878-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="d0878-207">リレーションシップを作成または削除の結合エンティティを操作するためのメソッドは変化しません。</span><span class="sxs-lookup"><span data-stu-id="d0878-207">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="d0878-208">サポートするクラスを追加作成し、インストラクター ページの編集</span><span class="sxs-lookup"><span data-stu-id="d0878-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="d0878-209">作成*SchoolViewModels/AssignedCourseData.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="d0878-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="d0878-210">`AssignedCourseData`クラスには、あるインストラクターによって割り当てられたコースのチェック ボックスを作成するデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d0878-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="d0878-211">作成、 *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*基本クラス。</span><span class="sxs-lookup"><span data-stu-id="d0878-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="d0878-212">`InstructorCoursesPageModel`基本クラスで、編集を使用してページ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d0878-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="d0878-213">`PopulateAssignedCourseData`すべてを読み込み`Course`を設定するエンティティ`AssignedCourseDataList`です。</span><span class="sxs-lookup"><span data-stu-id="d0878-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="d0878-214">各コースに、設定するコードを`CourseID`タイトル、および、インストラクター コースに割り当てられるかどうか。</span><span class="sxs-lookup"><span data-stu-id="d0878-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="d0878-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1)効率的な参照を作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="d0878-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="d0878-216">講習においてインストラクター編集ページのモデル</span><span class="sxs-lookup"><span data-stu-id="d0878-216">Instructors Edit page model</span></span>

<span data-ttu-id="d0878-217">次のコードで、インストラクター編集ページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="d0878-218">上記のコードでは、office の割り当て変更を処理します。</span><span class="sxs-lookup"><span data-stu-id="d0878-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="d0878-219">講師 Razor ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-219">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="d0878-220">Visual Studio でコードを貼り付けるときに改行コードを中断するように変更されます。</span><span class="sxs-lookup"><span data-stu-id="d0878-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="d0878-221">Ctrl + Z を 1 回押して、オート フォーマットを元に戻します。</span><span class="sxs-lookup"><span data-stu-id="d0878-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="d0878-222">Ctrl + Z は、ここに表示するように表示されるように、改行を修正します。</span><span class="sxs-lookup"><span data-stu-id="d0878-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="d0878-223">インデント完璧なのに、する必要はありませんが、 `@</tr><tr>`、 `@:<td>`、 `@:</td>`、および`@:</tr>`行ことはできません。 1 行に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="d0878-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="d0878-224">選択された新しいコードのブロックして、Tab キーを押して 3 回、新しいコードと既存のコードの行にします。</span><span class="sxs-lookup"><span data-stu-id="d0878-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="d0878-225">投票またはこのバグの状態を確認[で次のリンク](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)です。</span><span class="sxs-lookup"><span data-stu-id="d0878-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="d0878-226">上記のコードでは、次の 3 つの列を含んでいる HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d0878-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="d0878-227">各列には、チェック ボックスとコース番号とタイトルを含むキャプション。</span><span class="sxs-lookup"><span data-stu-id="d0878-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="d0878-228">すべてのチェック ボックスは、同じ名前 ("selectedCourses") を持ちます。</span><span class="sxs-lookup"><span data-stu-id="d0878-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="d0878-229">同じ名前を使用するには、グループとして扱われるようにする、モデル バインダーが通知されます。</span><span class="sxs-lookup"><span data-stu-id="d0878-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="d0878-230">各チェック ボックスの値の属性に設定されて`CourseID`です。</span><span class="sxs-lookup"><span data-stu-id="d0878-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="d0878-231">モデル バインダーがで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスだけが選択されている値。</span><span class="sxs-lookup"><span data-stu-id="d0878-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="d0878-232">チェック ボックスが最初に表示されているコース インストラクターに割り当てられている属性がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="d0878-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="d0878-233">アプリを実行し、更新された講習においてインストラクターの編集 ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="d0878-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="d0878-234">一部のコース割り当てを変更します。</span><span class="sxs-lookup"><span data-stu-id="d0878-234">Change some course assignments.</span></span> <span data-ttu-id="d0878-235">インデックス ページには、変更が反映されます。</span><span class="sxs-lookup"><span data-stu-id="d0878-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="d0878-236">注: インストラクター コースのデータを編集するのには、ここに採用されているアプローチは、コースの数に制限がある場合にも動作します。</span><span class="sxs-lookup"><span data-stu-id="d0878-236">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="d0878-237">非常に大きいコレクションを別の UI と更新の別の方法になりますより使用可能かつ効率的です。</span><span class="sxs-lookup"><span data-stu-id="d0878-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="d0878-238">講習においてインストラクター作成ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-238">Update the instructors Create page</span></span>

<span data-ttu-id="d0878-239">モデルを更新するインストラクター作成ページを次のコード。</span><span class="sxs-lookup"><span data-stu-id="d0878-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="d0878-240">上記のコードがに似ていますが、 *Pages/Instructors/Edit.cshtml.cs*コード。</span><span class="sxs-lookup"><span data-stu-id="d0878-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="d0878-241">次のマークアップ、インストラクター作成 Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="d0878-242">講師の作成 ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="d0878-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d0878-243">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="d0878-243">Update the Delete page</span></span>

<span data-ttu-id="d0878-244">次のコードで Delete ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="d0878-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="d0878-245">上記のコードでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d0878-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="d0878-246">一括読み込みを使用して、`CourseAssignments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d0878-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="d0878-247">`CourseAssignments`含める必要があるインストラクターが削除されたときに削除されないまたはします。</span><span class="sxs-lookup"><span data-stu-id="d0878-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="d0878-248">それらを読み取る必要はありませんを回避するのには、データベースで連鎖削除を構成します。</span><span class="sxs-lookup"><span data-stu-id="d0878-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="d0878-249">講師が削除されますが、部門の管理者として割り当てられている場合は、その部門からインストラクター割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="d0878-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d0878-250">[前へ](xref:data/ef-rp/read-related-data)
[次へ](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="d0878-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
