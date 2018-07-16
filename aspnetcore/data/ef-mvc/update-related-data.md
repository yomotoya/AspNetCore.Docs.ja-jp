---
title: ASP.NET Core MVC と EF Core - 関連データの更新 - 7/10
author: rick-anderson
description: このチュートリアルでは、外部キー フィールドとナビゲーション プロパティを更新することで関連データを更新します。
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: ef8cb3916e5d1542e4d36cad694351462b94ed32
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126727"
---
# <a name="aspnet-core-mvc-with-ef-core---update-related-data---7-of-10"></a><span data-ttu-id="e5547-103">ASP.NET Core MVC と EF Core - 関連データの更新 - 7/10</span><span class="sxs-lookup"><span data-stu-id="e5547-103">ASP.NET Core MVC with EF Core - Update Related Data - 7 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e5547-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e5547-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e5547-105">Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e5547-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="e5547-106">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e5547-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="e5547-107">前のチュートリアルでは、関連データを表示しました。このチュートリアルでは、外部キー フィールドとナビゲーション プロパティを更新することで関連データを更新します。</span><span class="sxs-lookup"><span data-stu-id="e5547-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="e5547-108">以下の図は、使用するページの一部を示しています。</span><span class="sxs-lookup"><span data-stu-id="e5547-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Course/Edit ページ](update-related-data/_static/course-edit.png)

![Instructor/Edit ページ](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="e5547-111">Courses の Create ページと Edit ページをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="e5547-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="e5547-112">新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。</span><span class="sxs-lookup"><span data-stu-id="e5547-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="e5547-113">これを容易にするため、スキャフォールディング コードには、コントローラーのメソッドと、部門を選択するためのドロップダウン リストを含む Create ビューと Edit ビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e5547-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="e5547-114">ドロップダウン リストは、`Course.DepartmentID` 外部キー プロパティを設定します。これは、`Department` ナビゲーション プロパティを適切な Department エンティティとともに読み込むためにすべての Entity Framework で必要です。</span><span class="sxs-lookup"><span data-stu-id="e5547-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="e5547-115">このスキャフォールディング コードを使用しますが、エラー処理を追加し、ドロップダウン リストを並べ替えるために少し変更します。</span><span class="sxs-lookup"><span data-stu-id="e5547-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="e5547-116">*CoursesController.cs* で、4 つの Create メソッドと Edit メソッドを削除し、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e5547-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="e5547-117">`Edit` HttpPost メソッドの後に、ドロップダウン リストに部門情報を読み込む新しいメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="e5547-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="e5547-118">`PopulateDepartmentsDropDownList` メソッドはすべての部門を名前で並べ替えたリストを取得し、ドロップダウン リスト用に `SelectList` コレクションを作成し、そのコレクションを `ViewBag` でビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="e5547-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="e5547-119">このメソッドは、ドロップダウン リストがレンダリングされるときに選択される項目を指定するためのコード呼び出しを許可する、省略可能な `selectedDepartment` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="e5547-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="e5547-120">ビューが名前 "DepartmentID" を `<select>` タグ ヘルパーに渡すと、ヘルパーは `ViewBag` オブジェクト内で "DepartmentID" という名前の `SelectList` を探すようになります。</span><span class="sxs-lookup"><span data-stu-id="e5547-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="e5547-121">新しいコースには部門がまだ確立されていないため、HttpGet `Create` メソッドは、選択した項目を設定せずに `PopulateDepartmentsDropDownList` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e5547-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department isn't established yet:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="e5547-122">HttpGet `Edit` メソッドは、編集中のコースに既に割り当てられている部門の ID に基づいて、選択したアイテムを設定します。</span><span class="sxs-lookup"><span data-stu-id="e5547-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that's already assigned to the course being edited:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="e5547-123">`Create` と `Edit` の両方の HttpPost メソッドには、エラーの発生後にページを再表示するときに選択した項目を設定するコードも含まれています。</span><span class="sxs-lookup"><span data-stu-id="e5547-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="e5547-124">これにより、エラー メッセージを表示するためにページを再表示するときに、選択されていた部門が選択されたままになることを保証します。</span><span class="sxs-lookup"><span data-stu-id="e5547-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="e5547-125">.AsNoTracking を Details メソッドと Delete メソッドに追加する</span><span class="sxs-lookup"><span data-stu-id="e5547-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="e5547-126">Course の Details ページと Delete ページのパフォーマンスを最適化するため、`AsNoTracking` 呼び出しを `Details` メソッドと HttpGet `Delete` メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="e5547-127">Course ビューを変更する</span><span class="sxs-lookup"><span data-stu-id="e5547-127">Modify the Course views</span></span>

<span data-ttu-id="e5547-128">*Views/Courses/Create.cshtml* で、**[Department]\(部門\)** ドロップダウン リストに [Select Department]\(部門を選択\) オプションを追加し、キャプションを **[DepartmentID]** から **[Department]\(部門\)** に変更し、検証メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="e5547-129">*Views/Courses/Edit.cshtml* で、*Create.cshtml* で行ったのと同じ変更を [Department]\(部門\) フィールドに加えます。</span><span class="sxs-lookup"><span data-stu-id="e5547-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="e5547-130">また、*Views/Courses/Edit.cshtml* で、**[Title]\(タイトル\)** フィールドの前にコース番号フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="e5547-131">コース番号は主キーであるため表示されますが、変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="e5547-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="e5547-132">Edit ビューには、コース番号の隠しフィールド (`<input type="hidden">`) が既にあります。</span><span class="sxs-lookup"><span data-stu-id="e5547-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="e5547-133">`<label>` タグ ヘルパーを追加しても、ユーザーが **[Edit]** ページで **[保存]** をクリックしたときに、ポストされたデータにコース番号が含まれないため、隠しフィールドの必要性はなくなりません。</span><span class="sxs-lookup"><span data-stu-id="e5547-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="e5547-134">*Views/Courses/Delete.cshtml* で、上部にコース番号フィールドを追加し、部門 ID を部門名に変更します。</span><span class="sxs-lookup"><span data-stu-id="e5547-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="e5547-135">*Views/Courses/Details.cshtml* で、*Delete.cshtml* に行ったのと同じ変更を行います。</span><span class="sxs-lookup"><span data-stu-id="e5547-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="e5547-136">Course ページをテストする</span><span class="sxs-lookup"><span data-stu-id="e5547-136">Test the Course pages</span></span>

<span data-ttu-id="e5547-137">アプリを実行して、**[Courses]** タブを選択し、**[新規作成]** をクリックして新しいコースのデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="e5547-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Course/Create ページ](update-related-data/_static/course-create.png)

<span data-ttu-id="e5547-139">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-139">Click **Create**.</span></span> <span data-ttu-id="e5547-140">Courses/Index ページには、リストに追加された新しいコースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="e5547-141">Index ページのリストの部門名は、ナビゲーション プロパティから取得され、リレーションシップが正常に確立されていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="e5547-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="e5547-142">Courses/Index ページのコースで **[Edit]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Course/Edit ページ](update-related-data/_static/course-edit.png)

<span data-ttu-id="e5547-144">ページ上のデータを変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="e5547-145">Courses/Index ページには、更新されたコース データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="e5547-146">Instructors の Edit ページを追加する</span><span class="sxs-lookup"><span data-stu-id="e5547-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="e5547-147">インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="e5547-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="e5547-148">Instructor エンティティには、OfficeAssignment エンティティとの一対ゼロまたは一対一のリレーションシップがあります。これは、コードで次の状況を処理する必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="e5547-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="e5547-149">ユーザーが元は値のあったオフィスの割り当てをクリアする場合は、OfficeAssignment エンティティを削除する。</span><span class="sxs-lookup"><span data-stu-id="e5547-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="e5547-150">ユーザーが元は空白だったオフィスの割り当ての値を入力する場合は、新しい OfficeAssignment エンティティを作成する。</span><span class="sxs-lookup"><span data-stu-id="e5547-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="e5547-151">ユーザーがオフィスの割り当ての値を変更する場合は、既存の OfficeAssignment エンティティの値を変更する。</span><span class="sxs-lookup"><span data-stu-id="e5547-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="e5547-152">Instructors コントローラーを更新する</span><span class="sxs-lookup"><span data-stu-id="e5547-152">Update the Instructors controller</span></span>

<span data-ttu-id="e5547-153">*InstructorsController.cs* で、HttpGet `Edit` メソッド内のコードを、Instructor エンティティの `OfficeAssignment` ナビゲーション プロパティを読み込んで `AsNoTracking` を呼び出すように変更します。</span><span class="sxs-lookup"><span data-stu-id="e5547-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="e5547-154">HttpPost `Edit` メソッドを次のコードで置き換え、オフィスの割り当ての更新を処理します。</span><span class="sxs-lookup"><span data-stu-id="e5547-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="e5547-155">このコードは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="e5547-155">The code does the following:</span></span>

-  <span data-ttu-id="e5547-156">署名が HttpGet `Edit` メソッドと同じになっているため、メソッド名を `EditPost` に変更します (`ActionName` 属性は `/Edit/` URL が引き続き使用されることを指定します)。</span><span class="sxs-lookup"><span data-stu-id="e5547-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="e5547-157">`OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の Instructor エンティティをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="e5547-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="e5547-158">これは、HttpGet `Edit` メソッドで行ったのと同じです。</span><span class="sxs-lookup"><span data-stu-id="e5547-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="e5547-159">モデル バインダーからの値を使用して、取得した Instructor エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="e5547-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="e5547-160">`TryUpdateModel` オーバーロードは、含めたいプロパティをホワイトリストに登録できるようにします。</span><span class="sxs-lookup"><span data-stu-id="e5547-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="e5547-161">これにより、[2 番目のチュートリアル](crud.md)で説明したように、過剰ポスティングを防止します。</span><span class="sxs-lookup"><span data-stu-id="e5547-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets don't play well with <ul> [!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```

-   <span data-ttu-id="e5547-162">オフィスの場所が空白の場合は、OfficeAssignment テーブル内の関連する行が削除されるように、Instructor.OfficeAssignment プロパティを null に設定します。</span><span class="sxs-lookup"><span data-stu-id="e5547-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="e5547-163">データベースへの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="e5547-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="e5547-164">Instructors/Edit ビューを更新する</span><span class="sxs-lookup"><span data-stu-id="e5547-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="e5547-165">*Views/Instructors/Edit.cshtml* で、オフィスの場所を編集するための新しいフィールドを、**[Save]\(保存\)** ボタンの直前に追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="e5547-166">アプリを実行し、**[Instructors]\(インストラクター\)** タブを選択し、インストラクターで **[Edit]\(編集\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="e5547-167">**[Office Location]\(オフィスの場所\)** を変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-167">Change the **Office Location** and click **Save**.</span></span>

![Instructor/Edit ページ](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="e5547-169">Instructors/Edit ページにコースの割り当てを追加する</span><span class="sxs-lookup"><span data-stu-id="e5547-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="e5547-170">インストラクターは、任意の数のコースを担当する場合があります。</span><span class="sxs-lookup"><span data-stu-id="e5547-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="e5547-171">次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、コースの割り当てを変更する機能を追加して、Instructor/Edit ページを拡張します。</span><span class="sxs-lookup"><span data-stu-id="e5547-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![コースが表示された Instructor/Edit ページ](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="e5547-173">Course エンティティと Instructor エンティティ間には、多対多のリレーションシップがあります。</span><span class="sxs-lookup"><span data-stu-id="e5547-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="e5547-174">リレーションシップの追加と削除を行うには、CourseAssignments 結合エンティティ セットからエンティティを追加および削除します。</span><span class="sxs-lookup"><span data-stu-id="e5547-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="e5547-175">インストラクターに割り当てられるコースを変更できるようにする UI は、チェック ボックスのグループです。</span><span class="sxs-lookup"><span data-stu-id="e5547-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="e5547-176">データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているコースが選択されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="e5547-177">ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。</span><span class="sxs-lookup"><span data-stu-id="e5547-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="e5547-178">コースの数が非常に多い場合は、ビューにデータを表示する別のメソッドを使用したいと思うかもしれませんが、結合エンティティを操作してリレーションシップを作成または削除するのと同じメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="e5547-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="e5547-179">Instructors コントローラーを更新する</span><span class="sxs-lookup"><span data-stu-id="e5547-179">Update the Instructors controller</span></span>

<span data-ttu-id="e5547-180">チェック ボックスのリストのためにデータをビューに提供するには、ビュー モデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="e5547-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="e5547-181">*SchoolViewModels* フォルダー内に *AssignedCourseData.cs* を作成し、既存のコードを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e5547-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="e5547-182">*InstructorsController.cs* で、HttpGet `Edit` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e5547-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="e5547-183">変更が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="e5547-183">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="e5547-184">このコードは、`Courses` ナビゲーション プロパティに一括読み込みを追加し、新しい `PopulateAssignedCourseData` メソッドを呼び出して、`AssignedCourseData` ビュー モデル クラスを使用してチェック ボックス配列に情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="e5547-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="e5547-185">`PopulateAssignedCourseData` メソッド内のコードは、ビュー モデル クラスを使用してコースのリストを読み込むため、すべての Course エンティティを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="e5547-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="e5547-186">各コースに対し、コードはそのコースがインストラクターの `Courses` ナビゲーション プロパティ内に存在しているかどうかをチェックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="e5547-187">コースがインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するため、インストラクターに割り当てられているコースが `HashSet` コレクション内に配置されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="e5547-188">インストラクターが割り当てられているコースに対し、`Assigned` プロパティが true に設定されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="e5547-189">ビューは、このプロパティを使用して、どのチェック ボックスを選択済みとして表示する必要があるかを判断します。</span><span class="sxs-lookup"><span data-stu-id="e5547-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="e5547-190">最後に、リストは `ViewData` でビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="e5547-191">次に、ユーザーが **[Save]\(保存\)** をクリックしたときに実行されるコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="e5547-192">`EditPost` メソッドを次のコードで置き換え、Instructor エンティティの `Courses` ナビゲーション プロパティを更新する新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="e5547-193">現在、メソッドの署名は HttpGet `Edit`メソッドとは異なっているため、メソッド名を `EditPost` から `Edit` に戻します。</span><span class="sxs-lookup"><span data-stu-id="e5547-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="e5547-194">ビューには Course エンティティのコレクションがないため、モデル バインダーは `CourseAssignments` ナビゲーション プロパティを自動的に更新できません。</span><span class="sxs-lookup"><span data-stu-id="e5547-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="e5547-195">モデル バインダーを使用する代わりに、新しい `UpdateInstructorCourses` メソッドで `CourseAssignments` ナビゲーション プロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="e5547-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="e5547-196">そのため、モデル バインドから `CourseAssignments` プロパティを除外する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5547-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="e5547-197">これを行うために、`TryUpdateModel` を呼び出すコードを変更する必要はありません。これは、ホワイトリストのオーバーロードを使用していて、`CourseAssignments` がインクルード リスト内にないためです。</span><span class="sxs-lookup"><span data-stu-id="e5547-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="e5547-198">チェック ボックスが選択されていない場合、`UpdateInstructorCourses` のコードは空のコレクションを使用して `CourseAssignments` ナビゲーション プロパティを初期化し、次を返します。</span><span class="sxs-lookup"><span data-stu-id="e5547-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="e5547-199">その後コードは、データベース内のすべてのコースをループ処理し、各コースを現在インストラクターに割り当てられているコースとビューで選択されているコースを比較してチェックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="e5547-200">検索を効率化するため、最後の 2 つのコレクションが `HashSet` オブジェクトに格納されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="e5547-201">コースのチェック ボックスが選択されたが、そのコースが `Instructor.CourseAssignments`ナビゲーション プロパティにない場合、そのコースがナビゲーション プロパティ内のコレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="e5547-202">コースのチェック ボックスが選択さていないが、そのコースが `Instructor.CourseAssignments`ナビゲーション プロパティにある場合、そのコースがナビゲーション プロパティから削除されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="e5547-203">Instructor ビューを更新する</span><span class="sxs-lookup"><span data-stu-id="e5547-203">Update the Instructor views</span></span>

<span data-ttu-id="e5547-204">*Views/Instructors/Edit.cshtml* で、次のコードを **Office** フィールドの `div` 要素の直後、**Save** ボタンの `div` 要素の前に追加することで、チェック ボックスの配列を持つ **Courses** フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="e5547-205">Visual Studio にコードを貼り付けると、改行がコードを分割するように変更されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="e5547-206">Ctrl キーを押しながら Z キーを 1 回押して、オート フォーマットを元に戻します。</span><span class="sxs-lookup"><span data-stu-id="e5547-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="e5547-207">これにより、改行がここに示されているように修正されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="e5547-208">インデントは完璧である必要はありませんが、`@</tr><tr>`、`@:<td>`、`@:</td>`、および `@:</tr>` の行は、示されているようにそれぞれ 1 行にする必要があります。そうしないと、ランタイム エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e5547-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="e5547-209">新しいコードのブロックを選択して、Tab キーを 3 回押して、新しいコードと既存のコードを並べます。</span><span class="sxs-lookup"><span data-stu-id="e5547-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="e5547-210">この問題の状態は、[ここ](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)で確認できます。</span><span class="sxs-lookup"><span data-stu-id="e5547-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="e5547-211">このコードは、3 つの列を含む HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="e5547-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="e5547-212">各列には、チェック ボックスとその後に続くキャプションがあります。キャプションは、コース番号とタイトルから構成されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="e5547-213">チェック ボックスはすべて同じ名前 ("selectedCourses") を持ち、これらをグループとして扱うようにモデル バインダーに通知します。</span><span class="sxs-lookup"><span data-stu-id="e5547-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they're to be treated as a group.</span></span> <span data-ttu-id="e5547-214">各チェック ボックスの value 属性は `CourseID` の値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="e5547-215">ページがポストされると、モデル バインダーは、選択されたチェック ボックスの `CourseID` 値のみで構成される配列をコントローラーに渡します。</span><span class="sxs-lookup"><span data-stu-id="e5547-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="e5547-216">チェック ボックスが最初にレンダリングされるときに、インストラクターに割り当てられるコースのチェック ボックスが checked 属性を持ち、選択されます (チェック ボックスがオンになった状態で表示されます)。</span><span class="sxs-lookup"><span data-stu-id="e5547-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="e5547-217">アプリを実行し、**[Instructors]\(インストラクター\)** タブを選択し、インストラクターで **[Edit]\(編集\)** をクリックして **Edit** ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="e5547-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![コースが表示された Instructor/Edit ページ](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="e5547-219">一部のコース割り当てを変更し、[Save]\(保存\) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e5547-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="e5547-220">行った変更が Index ページに反映されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE]
> <span data-ttu-id="e5547-221">インストラクター コース データを編集するためにここで採用されている方法は、コースの数が限られている場合にはうまく機能します。</span><span class="sxs-lookup"><span data-stu-id="e5547-221">The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="e5547-222">非常に大きいコレクションの場合、別の UI と別の更新方法が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="e5547-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="e5547-223">Delete ページを更新する</span><span class="sxs-lookup"><span data-stu-id="e5547-223">Update the Delete page</span></span>

<span data-ttu-id="e5547-224">*InstructorsController.cs* で、`DeleteConfirmed` メソッドを削除し、その場所に次のコードを挿入します。</span><span class="sxs-lookup"><span data-stu-id="e5547-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="e5547-225">このコードにより、次の変更が行われます。</span><span class="sxs-lookup"><span data-stu-id="e5547-225">This code makes the following changes:</span></span>

* <span data-ttu-id="e5547-226">`CourseAssignments` ナビゲーション プロパティに対して一括読み込みを行います。</span><span class="sxs-lookup"><span data-stu-id="e5547-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="e5547-227">これを含める必要があります。そうしないと、EF で関連 `CourseAssignment` エンティティが認識されず、削除されません。</span><span class="sxs-lookup"><span data-stu-id="e5547-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="e5547-228">ここでこれらを読み取らなくても済むようにするには、データベースで連鎖削除を構成します。</span><span class="sxs-lookup"><span data-stu-id="e5547-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="e5547-229">削除されるインストラクターが任意の部門の管理者として割り当てられている場合、インストラクターの割り当てをその部門から削除します。</span><span class="sxs-lookup"><span data-stu-id="e5547-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="e5547-230">オフィスの場所とコースを Create ページに追加する</span><span class="sxs-lookup"><span data-stu-id="e5547-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="e5547-231">*InstructorsController.cs* で、HttpPost と HttpGet の `Create` メソッドを削除してから、その場所に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="e5547-232">このコードは、`Edit` メソッドでご覧になったものと似ていますが、最初にコースが選択されていない点が異なります。</span><span class="sxs-lookup"><span data-stu-id="e5547-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="e5547-233">HttpGet `Create` メソッドは、`PopulateAssignedCourseData` メソッドを呼び出します。これはコースが選択されている可能性があるからではなく、ビュー内の `foreach` ループに空のコレクションを提供するためです (そうしないと、コードの表示で null 参照例外がスローされる場合があります)。</span><span class="sxs-lookup"><span data-stu-id="e5547-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="e5547-234">HttpPost `Create` メソッドは、選択した各コースを `CourseAssignments` ナビゲーション プロパティに追加してから、検証エラーをチェックし、データベースに新しいインストラクターを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="e5547-235">モデル エラーが発生した (たとえばユーザーが無効な日付をキー指定した) 場合に、エラー メッセージとともにページが再表示され、行ったコースの選択がすべて自動的に復元されるように、コースはモデル エラーが発生しても追加されます。</span><span class="sxs-lookup"><span data-stu-id="e5547-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="e5547-236">コースを `CourseAssignments` ナビゲーション プロパティに追加できるようにするには、プロパティを空のコレクションとして初期化する必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e5547-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="e5547-237">コントローラー コードでこれを行うための別の方法として、Instructor モデルでこれを行うことができます。このためには、プロパティ ゲッターを変更して、コレクションが存在しない場合に自動的に作成するようにします。次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="e5547-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="e5547-238">`CourseAssignments` プロパティをこの方法で変更する場合、コントローラー内の明示的なプロパティの初期化コードを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="e5547-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="e5547-239">*Views/Instructor/Create.cshtml* で、オフィスの場所のテキスト ボックスとチェック ボックスを [Submit]\(送信\) ボタンの前のコースに追加します。</span><span class="sxs-lookup"><span data-stu-id="e5547-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="e5547-240">Edit ページの場合と同様に、[コードを貼り付けたときに Visual Studio がコードを再フォーマットする場合は、書式設定を修正します](#notepad)。</span><span class="sxs-lookup"><span data-stu-id="e5547-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="e5547-241">アプリを実行し、インストラクターを作成して、テストします。</span><span class="sxs-lookup"><span data-stu-id="e5547-241">Test by running the app and creating an instructor.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="e5547-242">トランザクションの処理</span><span class="sxs-lookup"><span data-stu-id="e5547-242">Handling Transactions</span></span>

<span data-ttu-id="e5547-243">[CRUD チュートリアル](crud.md)で説明したように、Entity Framework はトランザクションを暗黙的に実装します。</span><span class="sxs-lookup"><span data-stu-id="e5547-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="e5547-244">たとえば、Entity Framework の外部で行われる操作をトランザクションに含めたい場合など、より詳細な制御が必要なシナリオについては、「[Using Transactions](https://docs.microsoft.com/ef/core/saving/transactions)」(トランザクションの使用) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e5547-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="e5547-245">まとめ</span><span class="sxs-lookup"><span data-stu-id="e5547-245">Summary</span></span>

<span data-ttu-id="e5547-246">これで、関連データの概要が完了しました。</span><span class="sxs-lookup"><span data-stu-id="e5547-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="e5547-247">次のチュートリアルでは、同時実行の競合を処理する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="e5547-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="e5547-248">[前へ](read-related-data.md)
> [次へ](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="e5547-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>
