---
title: ASP.NET Core の Razor ページと EF Core - 関連データの更新 - 7/8
author: rick-anderson
description: このチュートリアルでは、外部キー フィールドとナビゲーション プロパティを更新することで関連データを更新します。
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: b996e7f9c8e422060f6378a6004a53906e2af7c7
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087096"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="67f95-103">ASP.NET Core の Razor ページと EF Core - 関連データの更新 - 7/8</span><span class="sxs-lookup"><span data-stu-id="67f95-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="67f95-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="67f95-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="67f95-105">このチュートリアルでは、関連データの更新を示します。</span><span class="sxs-lookup"><span data-stu-id="67f95-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="67f95-106">解決できない問題が発生した場合は、[完成したアプリをダウンロードまたは表示](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)してください。</span><span class="sxs-lookup"><span data-stu-id="67f95-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="67f95-107">[ダウンロードの方法はこちらをご覧ください。](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="67f95-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="67f95-108">以下の図は、完成したページの一部を示しています。</span><span class="sxs-lookup"><span data-stu-id="67f95-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="67f95-109">![Course/Edit ページ](update-related-data/_static/course-edit.png)
![Instructor/Edit ページ](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="67f95-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="67f95-110">コースの Create (作成) ページと Edit (編集) ページを確認してテストします。</span><span class="sxs-lookup"><span data-stu-id="67f95-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="67f95-111">新しいコースを作成します。</span><span class="sxs-lookup"><span data-stu-id="67f95-111">Create a new course.</span></span> <span data-ttu-id="67f95-112">部門は、その名前ではなく主キー (整数) で選択されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="67f95-113">新しいコースを編集します。</span><span class="sxs-lookup"><span data-stu-id="67f95-113">Edit the new course.</span></span> <span data-ttu-id="67f95-114">テストが完了したら、新しいコースを削除します。</span><span class="sxs-lookup"><span data-stu-id="67f95-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="67f95-115">共通コードを共有する基底クラスを作成する</span><span class="sxs-lookup"><span data-stu-id="67f95-115">Create a base class to share common code</span></span>

<span data-ttu-id="67f95-116">Courses/Create ページと Courses/Edit ページにはそれぞれ部門名のリストが必要です。</span><span class="sxs-lookup"><span data-stu-id="67f95-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="67f95-117">Create ページと Edit ページ用に *Pages/Courses/DepartmentNamePageModel.cshtml.cs* 基底クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="67f95-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="67f95-118">上記のコードは、部門名のリストを格納するための [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) を作成します。</span><span class="sxs-lookup"><span data-stu-id="67f95-118">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="67f95-119">`selectedDepartment` を指定すると、`SelectList` でその部門が選択されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="67f95-120">Create と Edit のページ モデル クラスは、`DepartmentNamePageModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="67f95-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="67f95-121">Courses ページをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="67f95-121">Customize the Courses Pages</span></span>

<span data-ttu-id="67f95-122">新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。</span><span class="sxs-lookup"><span data-stu-id="67f95-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="67f95-123">コースを作成しながら部門を追加するため、Create と Edit の基底クラスには選択する部門のドロップダウン リストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="67f95-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="67f95-124">ドロップダウン リストは、`Course.DepartmentID` 外部キー (FK) プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="67f95-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="67f95-125">EF Core は `Course.DepartmentID` FK を使用して `Department` ナビゲーション プロパティを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="67f95-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![コースの作成](update-related-data/_static/ddl.png)

<span data-ttu-id="67f95-127">次のコードを使用して、Create ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-127">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="67f95-128">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="67f95-128">The preceding code:</span></span>

* <span data-ttu-id="67f95-129">`DepartmentNamePageModel`から派生します。</span><span class="sxs-lookup"><span data-stu-id="67f95-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="67f95-130">`TryUpdateModelAsync` を使用して[過剰ポスティング](xref:data/ef-rp/crud#overposting)を防止します。</span><span class="sxs-lookup"><span data-stu-id="67f95-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="67f95-131">`ViewData["DepartmentID"]` を `DepartmentNameSL` (基底クラスから) で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="67f95-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="67f95-132">`ViewData["DepartmentID"]` は厳密に型指定された `DepartmentNameSL` に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="67f95-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="67f95-133">厳密に型指定されたモデルは、弱く型指定されたモデルよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="67f95-134">詳細については、「[弱い型指定のデータ (ViewData と ViewBag)](xref:mvc/views/overview#VD_VB)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="67f95-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="67f95-135">Courses/Create ページを更新する</span><span class="sxs-lookup"><span data-stu-id="67f95-135">Update the Courses Create page</span></span>

<span data-ttu-id="67f95-136">次のマークアップを使用して *Pages/Courses/Create.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="67f95-137">上記のマークアップは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="67f95-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="67f95-138">キャプションを **DepartmentID** から **Department** (部門) に変更します。</span><span class="sxs-lookup"><span data-stu-id="67f95-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="67f95-139">`"ViewBag.DepartmentID"` を `DepartmentNameSL` (基底クラスから) で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="67f95-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="67f95-140">[Select Department]\(部門の選択\) オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="67f95-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="67f95-141">この変更により、最初の部門ではなく、[Select Department]\(部門の選択\) がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="67f95-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="67f95-142">部門が選択されていない場合に、検証メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="67f95-142">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="67f95-143">Razor ページは[選択タグ ヘルパー](xref:mvc/views/working-with-forms#the-select-tag-helper)を使用します。</span><span class="sxs-lookup"><span data-stu-id="67f95-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="67f95-144">Create ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="67f95-144">Test the Create page.</span></span> <span data-ttu-id="67f95-145">Create ページには、部門 ID ではなく、部門名が表示されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="67f95-146">Courses/Edit ページを更新する</span><span class="sxs-lookup"><span data-stu-id="67f95-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="67f95-147">次のコードを使用して、Edit ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-147">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="67f95-148">変更は、Create ページ モデルで行われたものと似ています。</span><span class="sxs-lookup"><span data-stu-id="67f95-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="67f95-149">上記のコードでは、ドロップダウン リストで指定された部門を選択する `PopulateDepartmentsDropDownList` が DepartmentID で渡されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="67f95-150">次のマークアップを使用して *Pages/Courses/Edit.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="67f95-151">上記のマークアップは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="67f95-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="67f95-152">コース ID を表示します。</span><span class="sxs-lookup"><span data-stu-id="67f95-152">Displays the course ID.</span></span> <span data-ttu-id="67f95-153">通常、エンティティの主キー (PK) は表示されません。</span><span class="sxs-lookup"><span data-stu-id="67f95-153">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="67f95-154">PK は通常、ユーザーにとっては意味がありません。</span><span class="sxs-lookup"><span data-stu-id="67f95-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="67f95-155">このケースでは、PK はコース番号です。</span><span class="sxs-lookup"><span data-stu-id="67f95-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="67f95-156">キャプションを **DepartmentID** から **Department** (部門) に変更します。</span><span class="sxs-lookup"><span data-stu-id="67f95-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="67f95-157">`"ViewBag.DepartmentID"` を `DepartmentNameSL` (基底クラスから) で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="67f95-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="67f95-158">このページには、コース番号の隠しフィールド (`<input type="hidden">`) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="67f95-158">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="67f95-159">`<label>` タグ ヘルパーと `asp-for="Course.CourseID"` を追加しても、隠しフィールドの必要性はなくなりません。</span><span class="sxs-lookup"><span data-stu-id="67f95-159">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="67f95-160">`<input type="hidden">` は、ユーザーが **[保存]** をクリックしたときに、ポストされたデータに含まれるコース番号にとって必要です。</span><span class="sxs-lookup"><span data-stu-id="67f95-160">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="67f95-161">更新されたコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="67f95-161">Test the updated code.</span></span> <span data-ttu-id="67f95-162">コースを作成、編集、および削除します。</span><span class="sxs-lookup"><span data-stu-id="67f95-162">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="67f95-163">AsNoTracking を Details (詳細) と Delete (削除) のページ モデルに追加する</span><span class="sxs-lookup"><span data-stu-id="67f95-163">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="67f95-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) は、追跡が必要ない場合に、パフォーマンスを向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="67f95-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="67f95-165">`AsNoTracking` を Details と Delete のページ モデルに追加します。</span><span class="sxs-lookup"><span data-stu-id="67f95-165">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="67f95-166">次のコードは、更新された Delete ページ モデルを示しています。</span><span class="sxs-lookup"><span data-stu-id="67f95-166">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="67f95-167">*Pages/Courses/Details.cshtml.cs* ファイルで `OnGetAsync` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-167">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="67f95-168">Delete ページと Details ページを変更する</span><span class="sxs-lookup"><span data-stu-id="67f95-168">Modify the Delete and Details pages</span></span>

<span data-ttu-id="67f95-169">次のマークアップを使用して、Delete Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-169">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="67f95-170">Details ページに同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="67f95-170">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="67f95-171">Course ページをテストする</span><span class="sxs-lookup"><span data-stu-id="67f95-171">Test the Course pages</span></span>

<span data-ttu-id="67f95-172">Create、Edit、Details、Delete の各ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="67f95-172">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="67f95-173">Instructor ページを更新する</span><span class="sxs-lookup"><span data-stu-id="67f95-173">Update the instructor pages</span></span>

<span data-ttu-id="67f95-174">次のセクションでは、Instructor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-174">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="67f95-175">オフィスの場所を追加する</span><span class="sxs-lookup"><span data-stu-id="67f95-175">Add office location</span></span>

<span data-ttu-id="67f95-176">インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="67f95-176">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="67f95-177">`Instructor` エンティティには、`OfficeAssignment` エンティティとの一対ゼロまたは一対一のリレーションシップがあります。</span><span class="sxs-lookup"><span data-stu-id="67f95-177">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="67f95-178">インストラクター コードは次を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="67f95-178">The instructor code must handle:</span></span>

* <span data-ttu-id="67f95-179">ユーザーがオフィスの割り当てをクリアした場合、`OfficeAssignment` エンティティを削除する。</span><span class="sxs-lookup"><span data-stu-id="67f95-179">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="67f95-180">ユーザーがオフィスの割り当てを入力し、それが空だった場合、新しい `OfficeAssignment` エンティティを作成する。</span><span class="sxs-lookup"><span data-stu-id="67f95-180">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="67f95-181">ユーザーがオフィスの割り当てを変更した場合、`OfficeAssignment` エンティティを更新する。</span><span class="sxs-lookup"><span data-stu-id="67f95-181">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="67f95-182">次のコードを使用して、Instructors/Edit ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-182">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="67f95-183">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="67f95-183">The preceding code:</span></span>

* <span data-ttu-id="67f95-184">`OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の `Instructor` エンティティをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="67f95-184">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
* <span data-ttu-id="67f95-185">モデル バインダーからの値を使用して、取得した `Instructor` エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="67f95-186">`TryUpdateModel` は[過剰ポスティング](xref:data/ef-rp/crud#overposting)を防止します。</span><span class="sxs-lookup"><span data-stu-id="67f95-186">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="67f95-187">オフィスの場所が空白の場合は、`Instructor.OfficeAssignment` を null に設定します。</span><span class="sxs-lookup"><span data-stu-id="67f95-187">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="67f95-188">`Instructor.OfficeAssignment` が null の場合、`OfficeAssignment` テーブル内の関連する行が削除されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-188">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="67f95-189">Instructors/Edit ページを更新する</span><span class="sxs-lookup"><span data-stu-id="67f95-189">Update the instructor Edit page</span></span>

<span data-ttu-id="67f95-190">オフィスの場所で *Pages/Instructors/Edit.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-190">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="67f95-191">インストラクターのオフィスの場所を変更できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="67f95-191">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="67f95-192">Instructors/Edit ページにコースの割り当てを追加する</span><span class="sxs-lookup"><span data-stu-id="67f95-192">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="67f95-193">インストラクターは、任意の数のコースを担当する場合があります。</span><span class="sxs-lookup"><span data-stu-id="67f95-193">Instructors may teach any number of courses.</span></span> <span data-ttu-id="67f95-194">このセクションでは、コースの割り当てを変更する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="67f95-194">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="67f95-195">次の図は、更新された Instructors/Edit ページを示しています。</span><span class="sxs-lookup"><span data-stu-id="67f95-195">The following image shows the updated instructor Edit page:</span></span>

![コースが表示された Instructor/Edit ページ](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="67f95-197">`Course` と `Instructor` は、多対多リレーションシップです。</span><span class="sxs-lookup"><span data-stu-id="67f95-197">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="67f95-198">リレーションシップの追加と削除を行うには、`CourseAssignments` 結合エンティティ セットからエンティティを追加および削除します。</span><span class="sxs-lookup"><span data-stu-id="67f95-198">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="67f95-199">チェック ボックスは、インストラクターが割り当てられるコースへの変更を有効にします。</span><span class="sxs-lookup"><span data-stu-id="67f95-199">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="67f95-200">データベース内のすべてのコースのチェック ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-200">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="67f95-201">インストラクターが割り当てられているコースがチェックされています。</span><span class="sxs-lookup"><span data-stu-id="67f95-201">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="67f95-202">ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。</span><span class="sxs-lookup"><span data-stu-id="67f95-202">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="67f95-203">コースの数が非常に多い場合:</span><span class="sxs-lookup"><span data-stu-id="67f95-203">If the number of courses were much greater:</span></span>

* <span data-ttu-id="67f95-204">別のユーザー インターフェイスを使用して、コースを表示します。</span><span class="sxs-lookup"><span data-stu-id="67f95-204">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="67f95-205">結合エンティティを操作してリレーションシップを作成または削除するメソッドは変わりません。</span><span class="sxs-lookup"><span data-stu-id="67f95-205">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="67f95-206">Instructors/Create ページと Instructors/Edit ページをサポートするクラスを追加する</span><span class="sxs-lookup"><span data-stu-id="67f95-206">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="67f95-207">次のコードを使用して、*SchoolViewModels/AssignedCourseData.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="67f95-207">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="67f95-208">`AssignedCourseData` クラスには、インストラクターごとの割り当てられたコースのチェック ボックスを作成するデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="67f95-208">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="67f95-209">*Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* 基底クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="67f95-209">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="67f95-210">`InstructorCoursesPageModel` は、Edit と Create のページ モデルに使用する基底クラスです。</span><span class="sxs-lookup"><span data-stu-id="67f95-210">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="67f95-211">`PopulateAssignedCourseData` は、`AssignedCourseDataList` に設定するためのすべての `Course` エンティティを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="67f95-211">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="67f95-212">コースごとに、コードは `CourseID`、タイトル、およびインストラクターがコースに割り当てられているかどうかを設定します。</span><span class="sxs-lookup"><span data-stu-id="67f95-212">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="67f95-213">効率的な参照を作成するために [HashSet](/dotnet/api/system.collections.generic.hashset-1) が使用されています。</span><span class="sxs-lookup"><span data-stu-id="67f95-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="67f95-214">Instructors/Edit ページ モデル</span><span class="sxs-lookup"><span data-stu-id="67f95-214">Instructors Edit page model</span></span>

<span data-ttu-id="67f95-215">次のコードを使用して、Instructors/Edit ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-215">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="67f95-216">上記のコードでは、オフィスの割り当て変更を処理します。</span><span class="sxs-lookup"><span data-stu-id="67f95-216">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="67f95-217">Instructor Razor ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-217">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="67f95-218">Visual Studio にコードを貼り付けると、改行がコードを分割するように変更されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-218">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="67f95-219">Ctrl キーを押しながら Z キーを 1 回押して、オート フォーマットを元に戻します。</span><span class="sxs-lookup"><span data-stu-id="67f95-219">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="67f95-220">Ctrl キーを押しながら Z キーを押すことで、改行がここに示されているように修正されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-220">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="67f95-221">インデントは完璧である必要はありませんが、`@</tr><tr>`、`@:<td>`、`@:</td>`、および `@:</tr>` の行は、示されているようにそれぞれ 1 行にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="67f95-221">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="67f95-222">新しいコードのブロックを選択して、Tab キーを 3 回押して、新しいコードと既存のコードを並べます。</span><span class="sxs-lookup"><span data-stu-id="67f95-222">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="67f95-223">このバグの状態を報告または確認するには、[このリンク](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)を使用してください。</span><span class="sxs-lookup"><span data-stu-id="67f95-223">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="67f95-224">上記のコードでは、3 つの列を含む HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="67f95-224">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="67f95-225">各列には、チェック ボックスと、コース番号とタイトルを含むキャプションがあります。</span><span class="sxs-lookup"><span data-stu-id="67f95-225">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="67f95-226">チェック ボックスはすべて、同じ名前 ("selectedCourses") を持ちます。</span><span class="sxs-lookup"><span data-stu-id="67f95-226">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="67f95-227">同じ名前を使用することで、これらをグループとして扱うようにモデル バインダーに通知します。</span><span class="sxs-lookup"><span data-stu-id="67f95-227">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="67f95-228">各チェック ボックスの value 属性は `CourseID` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-228">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="67f95-229">ページがポストされると、モデル バインダーは、選択されたチェック ボックスの `CourseID` 値のみで構成される配列を渡します。</span><span class="sxs-lookup"><span data-stu-id="67f95-229">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="67f95-230">チェック ボックスが最初にレンダリングされるときに、インストラクターに割り当てられているコースが checked 属性を持ちます。</span><span class="sxs-lookup"><span data-stu-id="67f95-230">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="67f95-231">アプリを実行し、更新された Instructors/Edit ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="67f95-231">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="67f95-232">一部のコース割り当てを変更します。</span><span class="sxs-lookup"><span data-stu-id="67f95-232">Change some course assignments.</span></span> <span data-ttu-id="67f95-233">変更が Index ページに反映されます。</span><span class="sxs-lookup"><span data-stu-id="67f95-233">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="67f95-234">メモ:インストラクター コース データを編集するためにここで採用されている方法は、コースの数が限られている場合にはうまく機能します。</span><span class="sxs-lookup"><span data-stu-id="67f95-234">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="67f95-235">非常に大きいコレクションの場合、別の UI と別の更新方法の方が有効で効率的な場合があります。</span><span class="sxs-lookup"><span data-stu-id="67f95-235">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="67f95-236">Instructors/Create ページを更新する</span><span class="sxs-lookup"><span data-stu-id="67f95-236">Update the instructors Create page</span></span>

<span data-ttu-id="67f95-237">次のコードを使用して、Instructors/Create ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-237">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="67f95-238">上記のコードは、*Pages/Instructors/Edit.cshtml.cs* コードに似ています。</span><span class="sxs-lookup"><span data-stu-id="67f95-238">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="67f95-239">次のマークアップを使用して、Instructors/Create Razor ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-239">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="67f95-240">Instructors/Create ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="67f95-240">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="67f95-241">Delete ページを更新する</span><span class="sxs-lookup"><span data-stu-id="67f95-241">Update the Delete page</span></span>

<span data-ttu-id="67f95-242">次のコードを使用して、Delete ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="67f95-242">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="67f95-243">上記のコードは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="67f95-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="67f95-244">一括読み込みを `CourseAssignments` ナビゲーション プロパティに使用します。</span><span class="sxs-lookup"><span data-stu-id="67f95-244">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="67f95-245">`CourseAssignments` を含める必要があります。そうしないと、インストラクターが削除されたときに削除されません。</span><span class="sxs-lookup"><span data-stu-id="67f95-245">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="67f95-246">これらを読み取らなくても済むようにするには、データベースで連鎖削除を構成します。</span><span class="sxs-lookup"><span data-stu-id="67f95-246">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="67f95-247">削除されるインストラクターが任意の部門の管理者として割り当てられている場合、インストラクターの割り当てをその部門から削除します。</span><span class="sxs-lookup"><span data-stu-id="67f95-247">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="67f95-248">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="67f95-248">Additional resources</span></span>

* [<span data-ttu-id="67f95-249">このチュートリアルの YouTube バージョン (パート 1)</span><span class="sxs-lookup"><span data-stu-id="67f95-249">YouTube version of this tutorial (Part 1)</span></span>](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [<span data-ttu-id="67f95-250">このチュートリアルの YouTube バージョン (パート 2)</span><span class="sxs-lookup"><span data-stu-id="67f95-250">YouTube version of this tutorial (Part 2)</span></span>](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> <span data-ttu-id="67f95-251">[前へ](xref:data/ef-rp/read-related-data)
> [次へ](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="67f95-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
