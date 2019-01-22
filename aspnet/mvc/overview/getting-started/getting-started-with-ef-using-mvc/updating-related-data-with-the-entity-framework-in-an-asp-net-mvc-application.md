---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリで EF で関連データを更新します。'
description: このチュートリアルでは、関連するデータを更新します。 ほとんどのリレーションシップは、これは外部キー フィールドまたはナビゲーション プロパティを更新することで行うことができます。
author: tdykstra
ms.author: riande
ms.date: 01/17/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 3f95470fd1832d7d25a331a1b6a9dfede7356f38
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444312"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="888d5-104">チュートリアル: ASP.NET MVC アプリで EF で関連データを更新します。</span><span class="sxs-lookup"><span data-stu-id="888d5-104">Tutorial: Update related data with EF in an ASP.NET MVC app</span></span>

<span data-ttu-id="888d5-105">前のチュートリアルでは、関連するデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-105">In the previous tutorial you displayed related data.</span></span> <span data-ttu-id="888d5-106">このチュートリアルでは、関連するデータを更新します。</span><span class="sxs-lookup"><span data-stu-id="888d5-106">In this tutorial you'll update related data.</span></span> <span data-ttu-id="888d5-107">ほとんどのリレーションシップは、これは外部キー フィールドまたはナビゲーション プロパティを更新することで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="888d5-107">For most relationships, this can be done by updating either foreign key fields or navigation properties.</span></span> <span data-ttu-id="888d5-108">多対多のリレーションシップで Entity Framework は結合テーブルを直接公開を追加し、該当するナビゲーション プロパティからエンティティを削除するようにします。</span><span class="sxs-lookup"><span data-stu-id="888d5-108">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="888d5-109">以下の図は、使用するページの一部を示しています。</span><span class="sxs-lookup"><span data-stu-id="888d5-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![コースで講師の編集](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="888d5-113">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="888d5-113">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="888d5-114">Courses ページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="888d5-114">Customize courses pages</span></span>
> * <span data-ttu-id="888d5-115">Instructors ページに office を追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-115">Add office to instructors page</span></span>
> * <span data-ttu-id="888d5-116">Instructors ページにコースを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-116">Add courses to instructors page</span></span>
> * <span data-ttu-id="888d5-117">DeleteConfirmed を更新します。</span><span class="sxs-lookup"><span data-stu-id="888d5-117">Update DeleteConfirmed</span></span>
> * <span data-ttu-id="888d5-118">オフィスの場所とコースを Create ページに追加する</span><span class="sxs-lookup"><span data-stu-id="888d5-118">Add office location and courses to the Create page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="888d5-119">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="888d5-119">Prerequisites</span></span>

* [<span data-ttu-id="888d5-120">関連データの読み取り</span><span class="sxs-lookup"><span data-stu-id="888d5-120">Reading Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a><span data-ttu-id="888d5-121">Courses ページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="888d5-121">Customize courses pages</span></span>

<span data-ttu-id="888d5-122">新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。</span><span class="sxs-lookup"><span data-stu-id="888d5-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="888d5-123">これを容易にするため、スキャフォールディング コードには、コントローラーのメソッドと、部門を選択するためのドロップダウン リストを含む Create ビューと Edit ビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="888d5-123">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="888d5-124">ドロップダウン リストのセット、`Course.DepartmentID`外部キー プロパティは、Entity Framework は、読み込むために必要なすべて、`Department`ナビゲーション プロパティを適切な`Department`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-124">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="888d5-125">このスキャフォールディング コードを使用しますが、エラー処理を追加し、ドロップダウン リストを並べ替えるために少し変更します。</span><span class="sxs-lookup"><span data-stu-id="888d5-125">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="888d5-126">*CourseController.cs*、4 つの削除`Create`と`Edit`メソッドし、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="888d5-126">In *CourseController.cs*, delete the four `Create` and `Edit` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

<span data-ttu-id="888d5-127">次の追加`using`ファイルの先頭にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="888d5-127">Add the following `using` statement at the beginning of the file:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="888d5-128">`PopulateDepartmentsDropDownList`メソッドの名前で並べ替えたすべての部門の一覧を取得、作成、`SelectList`のドロップダウン リストでは、コレクション ビューに、コレクションを渡すと、`ViewBag`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-128">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="888d5-129">このメソッドは、ドロップダウン リストがレンダリングされるときに選択される項目を指定するためのコード呼び出しを許可する、省略可能な `selectedDepartment` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="888d5-129">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="888d5-130">ビューが名前を渡す`DepartmentID`を[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper、およびヘルパー ファイルの場所を認識し、`ViewBag`オブジェクト、`SelectList`という名前`DepartmentID`します。</span><span class="sxs-lookup"><span data-stu-id="888d5-130">The view will pass the name `DepartmentID` to the [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="888d5-131">`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択した項目を設定せずメソッド。</span><span class="sxs-lookup"><span data-stu-id="888d5-131">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="888d5-132">`HttpGet` `Edit`メソッドが既に編集中のコースに割り当てられている部門の ID に基づいて、選択した項目を設定します。</span><span class="sxs-lookup"><span data-stu-id="888d5-132">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

<span data-ttu-id="888d5-133">`HttpPost`両方のメソッド`Create`と`Edit`も、ページを再表示エラーが発生したときに、選択した項目を設定するコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="888d5-133">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

<span data-ttu-id="888d5-134">このコードによりエラー メッセージを表示、ページが表示されときに、選択した常時されます部門が選択されているようになります。</span><span class="sxs-lookup"><span data-stu-id="888d5-134">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="888d5-135">Course ビューは、department フィールドのドロップダウン リストで既にスキャフォールディングされたが、次の強調表示されていることを変更するため、このフィールドでは、DepartmentID キャプションをたくない、 *Views\Course\Create.cshtml*ファイルキャプションを変更します。</span><span class="sxs-lookup"><span data-stu-id="888d5-135">The Course views are already scaffolded with drop-down lists for the department field, but you don't want the DepartmentID caption for this field, so make the following highlighted change to the *Views\Course\Create.cshtml* file to change the caption.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

<span data-ttu-id="888d5-136">同じ変更を行う*Views\Course\Edit.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="888d5-136">Make the same change in *Views\Course\Edit.cshtml*.</span></span>

<span data-ttu-id="888d5-137">通常、スキャフォールダーの主キー スキャフォールディングは、キー値データベースによって生成される変更ことはできず、ユーザーに表示する意味のある値はありません。</span><span class="sxs-lookup"><span data-stu-id="888d5-137">Normally the scaffolder doesn't scaffold a primary key because the key value is generated by the database and can't be changed and isn't a meaningful value to be displayed to users.</span></span> <span data-ttu-id="888d5-138">Course エンティティの場合、スキャフォルダーは、テキスト ボックスは含ま、`CourseID`フィールドを認識するため、`DatabaseGeneratedOption.None`属性は、ユーザーができることを意味主キーの値を入力します。</span><span class="sxs-lookup"><span data-stu-id="888d5-138">For Course entities the scaffolder does include an text box for the `CourseID` field because it understands that the `DatabaseGeneratedOption.None` attribute means the user should be able enter the primary key value.</span></span> <span data-ttu-id="888d5-139">ただし、番号がわかりやすいためにする手動で追加する必要があるため、他のビューで表示することを理解していません。</span><span class="sxs-lookup"><span data-stu-id="888d5-139">But it doesn't understand that because the number is meaningful you want to see it in the other views, so you need to add it manually.</span></span>

<span data-ttu-id="888d5-140">*Views\Course\Edit.cshtml*、前にコース番号フィールドを追加、**タイトル**フィールド。</span><span class="sxs-lookup"><span data-stu-id="888d5-140">In *Views\Course\Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="888d5-141">主キーであるためが表示されますが、変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="888d5-141">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

<span data-ttu-id="888d5-142">非表示フィールドが既に存在 (`Html.HiddenFor`ヘルパー) の編集ビューで、コース番号。</span><span class="sxs-lookup"><span data-stu-id="888d5-142">There's already a hidden field (`Html.HiddenFor` helper) for the course number in the Edit view.</span></span> <span data-ttu-id="888d5-143">追加、 *Html.LabelFor* 、ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号が原因であるために、ヘルパーは、隠しフィールドの必要性を排除しない**保存**[編集] ページ。</span><span class="sxs-lookup"><span data-stu-id="888d5-143">Adding an *Html.LabelFor* helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the Edit page.</span></span>

<span data-ttu-id="888d5-144">*Views\Course\Delete.cshtml*と*Views\Course\Details.cshtml*"Department"に"Name"から、部門名キャプションを変更して、前にコース番号フィールドを追加、**タイトル**フィールド。</span><span class="sxs-lookup"><span data-stu-id="888d5-144">In *Views\Course\Delete.cshtml* and *Views\Course\Details.cshtml*, change the department name caption from "Name" to "Department" and add a course number field before the **Title** field.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

<span data-ttu-id="888d5-145">実行、**作成**ページ (コースのインデックス ページを表示し、をクリックして**新規作成**) 新しいコースのデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="888d5-145">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

| <span data-ttu-id="888d5-146">[値]</span><span class="sxs-lookup"><span data-stu-id="888d5-146">Value</span></span> | <span data-ttu-id="888d5-147">設定</span><span class="sxs-lookup"><span data-stu-id="888d5-147">Setting</span></span> |
| ----- | ------- |
| <span data-ttu-id="888d5-148">数値</span><span class="sxs-lookup"><span data-stu-id="888d5-148">Number</span></span> | <span data-ttu-id="888d5-149">入力*1000*します。</span><span class="sxs-lookup"><span data-stu-id="888d5-149">Enter *1000*.</span></span> |
| <span data-ttu-id="888d5-150">タイトル</span><span class="sxs-lookup"><span data-stu-id="888d5-150">Title</span></span> | <span data-ttu-id="888d5-151">入力*代数*します。</span><span class="sxs-lookup"><span data-stu-id="888d5-151">Enter *Algebra*.</span></span> |
| <span data-ttu-id="888d5-152">謝辞</span><span class="sxs-lookup"><span data-stu-id="888d5-152">Credits</span></span> | <span data-ttu-id="888d5-153">入力*4*します。</span><span class="sxs-lookup"><span data-stu-id="888d5-153">Enter *4*.</span></span> |
|<span data-ttu-id="888d5-154">Department</span><span class="sxs-lookup"><span data-stu-id="888d5-154">Department</span></span> | <span data-ttu-id="888d5-155">選択**数学**します。</span><span class="sxs-lookup"><span data-stu-id="888d5-155">Select **Mathematics**.</span></span> |

<span data-ttu-id="888d5-156">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="888d5-156">Click **Create**.</span></span> <span data-ttu-id="888d5-157">コースの Index ページには、一覧に追加された新しいコースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-157">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="888d5-158">Index ページのリストの部門名は、ナビゲーション プロパティから取得され、リレーションシップが正常に確立されていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="888d5-158">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="888d5-159">実行、**編集**ページ (コースのインデックス ページを表示し、クリックして**編集**コースで)。</span><span class="sxs-lookup"><span data-stu-id="888d5-159">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

<span data-ttu-id="888d5-160">ページ上のデータを変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="888d5-160">Change data on the page and click **Save**.</span></span> <span data-ttu-id="888d5-161">コースの Index ページには、更新されたコース データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-161">The Course Index page is displayed with the updated course data.</span></span>

## <a name="add-office-to-instructors-page"></a><span data-ttu-id="888d5-162">Instructors ページに office を追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-162">Add office to instructors page</span></span>

<span data-ttu-id="888d5-163">インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="888d5-163">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="888d5-164">`Instructor`エンティティと 0 または 1 に 1 つリレーションシップを持つ、`OfficeAssignment`エンティティは、次の状況を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="888d5-164">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="888d5-165">解除し、削除する場合は、ユーザーがオフィスの割り当てをクリアした値する必要があります、`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-165">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="888d5-166">新規に作成する必要がある場合は、ユーザーがオフィスの割り当ての値を入力し、空か最初、`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-166">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="888d5-167">ユーザーは、オフィスの割り当ての値を変更する場合は、既存の値を変更する必要があります`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-167">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="888d5-168">開いている*InstructorController.cs*を見て、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="888d5-168">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="888d5-169">スキャフォールディングされたコードをここでは、対象はありません。</span><span class="sxs-lookup"><span data-stu-id="888d5-169">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="888d5-170">データの設定には、ドロップダウン リストがテキスト ボックスは、必要なものです。</span><span class="sxs-lookup"><span data-stu-id="888d5-170">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="888d5-171">このメソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="888d5-171">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

<span data-ttu-id="888d5-172">このコードを削除、`ViewBag`ステートメントと、一括読み込みを関連付けられている追加`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-172">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="888d5-173">一括読み込みを実行することはできません、`Find`メソッド、そのため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-173">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="888d5-174">置換、 `HttpPost` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="888d5-174">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="888d5-175">これは、オフィスの割り当ての更新を処理します。</span><span class="sxs-lookup"><span data-stu-id="888d5-175">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="888d5-176">参照を`RetryLimitExceededException`が必要です、`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="888d5-176">The reference to `RetryLimitExceededException` requires a `using` statement.</span></span> <span data-ttu-id="888d5-177">これを追加するポイント`RetryLimitExceededException`します。</span><span class="sxs-lookup"><span data-stu-id="888d5-177">To add it, hover over `RetryLimitExceededException`.</span></span> <span data-ttu-id="888d5-178">問題の説明が表示されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-178">An explanation of the issue appears.</span></span> <span data-ttu-id="888d5-179">選択**考えられる修正内容を表示する** をクリックし、 **System.Data.Entity.Infrastructure; を使用して**します。</span><span class="sxs-lookup"><span data-stu-id="888d5-179">Select **Show potential fixes** and then click **using System.Data.Entity.Infrastructure;**.</span></span>

![再試行の例外を解決するには](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="888d5-181">このコードは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="888d5-181">The code does the following:</span></span>

- <span data-ttu-id="888d5-182">メソッド名を変更`EditPost`署名と同じであるため、`HttpGet`メソッド (、`ActionName`属性は、/edit/URL がまだ使用されていることを指定します)。</span><span class="sxs-lookup"><span data-stu-id="888d5-182">Changes the method name to `EditPost` because the signature is now the same as the `HttpGet` method (the `ActionName` attribute specifies that the /Edit/ URL is still used).</span></span>
- <span data-ttu-id="888d5-183">`OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の `Instructor` エンティティをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="888d5-183">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="888d5-184">これで行ったのと同じ、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="888d5-184">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="888d5-185">モデル バインダーからの値を使用して、取得した `Instructor` エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="888d5-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="888d5-186">[Tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用するオーバー ロードを使用する*ホワイト リスト*プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-186">The [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="888d5-187">これにより、過剰ポスティングで説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="888d5-187">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- <span data-ttu-id="888d5-188">オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null ように、関連する行で、`OfficeAssignment`テーブルは削除されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-188">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- <span data-ttu-id="888d5-189">データベースへの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="888d5-189">Saves the changes to the database.</span></span>

<span data-ttu-id="888d5-190">*Views\Instructor\Edit.cshtml*後に、`div`の要素、 **Hire Date**フィールドで、オフィスの場所を編集するための新しいフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-190">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

<span data-ttu-id="888d5-191">ページの実行 (選択、 **Instructors**  タブをクリックして**編集**インストラクターで)。</span><span class="sxs-lookup"><span data-stu-id="888d5-191">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="888d5-192">**[Office Location]\(オフィスの場所\)** を変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="888d5-192">Change the **Office Location** and click **Save**.</span></span>

## <a name="add-courses-to-instructors-page"></a><span data-ttu-id="888d5-193">Instructors ページにコースを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-193">Add courses to instructors page</span></span>

<span data-ttu-id="888d5-194">インストラクターは、任意の数のコースを担当する場合があります。</span><span class="sxs-lookup"><span data-stu-id="888d5-194">Instructors may teach any number of courses.</span></span> <span data-ttu-id="888d5-195">チェック ボックスのグループを使用して、コースの割り当てを変更する機能を追加して、Instructor/edit ページを拡張します。</span><span class="sxs-lookup"><span data-stu-id="888d5-195">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes.</span></span>

<span data-ttu-id="888d5-196">間のリレーションシップ、`Course`と`Instructor`エンティティは多対多。 つまり、結合テーブル内にある外部キー プロパティに直接アクセスする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="888d5-196">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the foreign key properties which are in the join table.</span></span> <span data-ttu-id="888d5-197">代わりに、追加し、エンティティを削除して、`Instructor.Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-197">Instead, you add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="888d5-198">インストラクターに割り当てられるコースを変更できるようにする UI は、チェック ボックスのグループです。</span><span class="sxs-lookup"><span data-stu-id="888d5-198">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="888d5-199">データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているコースが選択されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-199">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="888d5-200">ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。</span><span class="sxs-lookup"><span data-stu-id="888d5-200">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="888d5-201">コースの数が非常に多い場合、ビューでのデータの表示のさまざまなメソッドを使用することが考えられますが、作成またはリレーションシップを削除するにはナビゲーション プロパティを操作するのと同じ方法を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="888d5-201">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="888d5-202">チェック ボックスのリストのためにデータをビューに提供するには、ビュー モデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="888d5-202">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="888d5-203">作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。</span><span class="sxs-lookup"><span data-stu-id="888d5-203">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="888d5-204">*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="888d5-204">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="888d5-205">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-205">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

<span data-ttu-id="888d5-206">このコードは、`Courses` ナビゲーション プロパティに一括読み込みを追加し、新しい `PopulateAssignedCourseData` メソッドを呼び出して、`AssignedCourseData` ビュー モデル クラスを使用してチェック ボックス配列に情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="888d5-206">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="888d5-207">内のコード、`PopulateAssignedCourseData`メソッドを読み取り、すべて`Course`ビューを使用したコースのリストを読み込むためにエンティティ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="888d5-207">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="888d5-208">各コースに対し、コードはそのコースがインストラクターの `Courses` ナビゲーション プロパティ内に存在しているかどうかをチェックします。</span><span class="sxs-lookup"><span data-stu-id="888d5-208">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="888d5-209">コースがインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/library/bb359438.aspx)コレクション。</span><span class="sxs-lookup"><span data-stu-id="888d5-209">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="888d5-210">`Assigned`プロパティに設定されて`true`コースの講師が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="888d5-210">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="888d5-211">ビューは、このプロパティを使用して、どのチェック ボックスを選択済みとして表示する必要があるかを判断します。</span><span class="sxs-lookup"><span data-stu-id="888d5-211">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="888d5-212">最後に、一覧がビューに渡される、`ViewBag`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-212">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="888d5-213">次に、ユーザーが **[Save]\(保存\)** をクリックしたときに実行されるコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-213">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="888d5-214">置換、`EditPost`メソッドを次のコードは、更新プログラムの新しいメソッドを呼び出し、`Courses`のナビゲーション プロパティ、`Instructor`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-214">Replace the `EditPost` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="888d5-215">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-215">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

<span data-ttu-id="888d5-216">メソッド シグネチャが異なるようになりました、 `HttpGet` `Edit`メソッド、メソッド名の変更から`EditPost`に`Edit`します。</span><span class="sxs-lookup"><span data-stu-id="888d5-216">The method signature is now different from the `HttpGet` `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="888d5-217">ビューのコレクションを持たないため`Course`エンティティ、モデル バインダーを更新できません自動的に、`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-217">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="888d5-218">モデル バインダーを使用して更新するのではなく、`Courses`ナビゲーション プロパティでは、新しい行います`UpdateInstructorCourses`メソッド。</span><span class="sxs-lookup"><span data-stu-id="888d5-218">Instead of using the model binder to update the `Courses` navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="888d5-219">そのため、モデル バインドから `Courses` プロパティを除外する必要があります。</span><span class="sxs-lookup"><span data-stu-id="888d5-219">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="888d5-220">これを呼び出すコードの変更が必要としない[tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト登録*オーバー ロードと`Courses`インクルード一覧に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="888d5-220">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="888d5-221">場合、ボックスが選択されていないチェック、コードでは、`UpdateInstructorCourses`を初期化します、`Courses`空のコレクションでのナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-221">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="888d5-222">その後コードは、データベース内のすべてのコースをループ処理し、各コースを現在インストラクターに割り当てられているコースとビューで選択されているコースを比較してチェックします。</span><span class="sxs-lookup"><span data-stu-id="888d5-222">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="888d5-223">検索を効率化するため、最後の 2 つのコレクションが `HashSet` オブジェクトに格納されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-223">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="888d5-224">コースのチェック ボックスが選択されたが、そのコースが `Instructor.Courses`ナビゲーション プロパティにない場合、そのコースがナビゲーション プロパティ内のコレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-224">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="888d5-225">コースのチェック ボックスが選択さていないが、そのコースが `Instructor.Courses`ナビゲーション プロパティにある場合、そのコースがナビゲーション プロパティから削除されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-225">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="888d5-226">*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを次を追加することで配列をコードの直後に、`div`の要素、`OfficeAssignment`フィールドと前に、`div`の要素、**保存**ボタン。</span><span class="sxs-lookup"><span data-stu-id="888d5-226">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the `OfficeAssignment` field and before the `div` element for the **Save** button:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

<span data-ttu-id="888d5-227">貼り付けた後、コード、改行の場合とインデントしないたいしてここで、次のように、ここにすべてを手動で修正します。</span><span class="sxs-lookup"><span data-stu-id="888d5-227">After you paste the code, if line breaks and indentation don't look like they do here, manually fix everything so that it looks like what you see here.</span></span> <span data-ttu-id="888d5-228">インデントは完璧である必要はありませんが、`@</tr><tr>`、`@:<td>`、`@:</td>`、および `@</tr>` の行は、示されているようにそれぞれ 1 行にする必要があります。そうしないと、ランタイム エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="888d5-228">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span>

<span data-ttu-id="888d5-229">このコードは、3 つの列を含む HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="888d5-229">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="888d5-230">各列には、チェック ボックスとその後に続くキャプションがあります。キャプションは、コース番号とタイトルから構成されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-230">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="888d5-231">チェック ボックスはすべて、同じ名前 ("selectedCourses") は、グループとして扱う場合にモデル バインダーに通知があります。</span><span class="sxs-lookup"><span data-stu-id="888d5-231">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="888d5-232">`value`各チェック ボックスの属性の値に設定されて`CourseID.`で構成されるコント ローラーにモデル バインダーが配列を渡しますページが投稿されたときに、`CourseID`チェック ボックスが選択されている値。</span><span class="sxs-lookup"><span data-stu-id="888d5-232">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="888d5-233">インストラクターに割り当てられるコースのチェック ボックスが最初に表示されると、`checked`属性は、(オンになった状態を表示します) それらを選択します。</span><span class="sxs-lookup"><span data-stu-id="888d5-233">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="888d5-234">コースの割り当てを変更した後に、サイトが返されるときに、変更を確認できる必要あります、`Index`ページ。</span><span class="sxs-lookup"><span data-stu-id="888d5-234">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="888d5-235">そのため、そのページ内のテーブルに列を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="888d5-235">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="888d5-236">ここで使用する必要はありません、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてページに渡しているエンティティ。</span><span class="sxs-lookup"><span data-stu-id="888d5-236">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="888d5-237">*Views\Instructor\Index.cshtml*、追加、**コース**直後に、次の見出し、 **Office**見出しで、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="888d5-237">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

<span data-ttu-id="888d5-238">オフィスの場所の詳細セルの直後に続く新しい詳細セルを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-238">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

<span data-ttu-id="888d5-239">実行、 **Instructor インデックス**ページに各インストラクターに割り当てられているコースをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="888d5-239">Run the **Instructor Index** page to see the courses assigned to each instructor.</span></span>

<span data-ttu-id="888d5-240">クリックして**編集**の講師が Edit ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="888d5-240">Click **Edit** on an instructor to see the Edit page.</span></span>

<span data-ttu-id="888d5-241">一部のコース割り当てを変更し、クリックして**保存**します。</span><span class="sxs-lookup"><span data-stu-id="888d5-241">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="888d5-242">行った変更が Index ページに反映されます。</span><span class="sxs-lookup"><span data-stu-id="888d5-242">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="888d5-243">メモ:ここで採用インストラクター コース データを編集するのには、コースの数に制限がある場合にも動作します。</span><span class="sxs-lookup"><span data-stu-id="888d5-243">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="888d5-244">非常に大きいコレクションの場合、別の UI と別の更新方法が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="888d5-244">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-deleteconfirmed"></a><span data-ttu-id="888d5-245">DeleteConfirmed を更新します。</span><span class="sxs-lookup"><span data-stu-id="888d5-245">Update DeleteConfirmed</span></span>

<span data-ttu-id="888d5-246">*InstructorController.cs*、削除、`DeleteConfirmed`代わりに、次のコード メソッドおよび挿入します。</span><span class="sxs-lookup"><span data-stu-id="888d5-246">In *InstructorController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

<span data-ttu-id="888d5-247">このコードは、次の変更です。</span><span class="sxs-lookup"><span data-stu-id="888d5-247">This code makes the following change:</span></span>

- <span data-ttu-id="888d5-248">インストラクターが任意の部門の管理者として割り当てられている場合は、その部門から、インストラクターの割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="888d5-248">If the instructor is assigned as administrator of any department, removes the instructor assignment from that department.</span></span> <span data-ttu-id="888d5-249">このコードがないが部門の管理者として割り当てられている講師を削除しようとする場合参照整合性エラーが発生したとします。</span><span class="sxs-lookup"><span data-stu-id="888d5-249">Without this code, you would get a referential integrity error if you tried to delete an instructor who was assigned as administrator for a department.</span></span>

<span data-ttu-id="888d5-250">このコードは、複数の部門の管理者として割り当てられている 1 つの講師のシナリオを処理しません。</span><span class="sxs-lookup"><span data-stu-id="888d5-250">This code doesn't handle the scenario of one instructor assigned as administrator for multiple departments.</span></span> <span data-ttu-id="888d5-251">最後のチュートリアルでは、そのシナリオが発生しないようにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-251">In the last tutorial you'll add code that prevents that scenario from happening.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="888d5-252">オフィスの場所とコースを Create ページに追加する</span><span class="sxs-lookup"><span data-stu-id="888d5-252">Add office location and courses to the Create page</span></span>

<span data-ttu-id="888d5-253">*InstructorController.cs*、削除、`HttpGet`と`HttpPost``Create`メソッド、代わりに次のコードを追加。</span><span class="sxs-lookup"><span data-stu-id="888d5-253">In *InstructorController.cs*, delete the `HttpGet` and `HttpPost` `Create` methods, and then add the following code in their place:</span></span>


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="888d5-254">このコードは、確認したものの Edit メソッドの最初にコースが選択されていない点が似ています。</span><span class="sxs-lookup"><span data-stu-id="888d5-254">This code is similar to what you saw for the Edit methods except that initially no courses are selected.</span></span> <span data-ttu-id="888d5-255">`HttpGet` `Create`メソッドの呼び出し、`PopulateAssignedCourseData`メソッド コースが選択されていますがある可能性があるためではなく注文の空のコレクションを提供する、 `foreach` (それ以外の場合、コードの表示は、null 参照例外をスローするビューでのループ).</span><span class="sxs-lookup"><span data-stu-id="888d5-255">The `HttpGet` `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="888d5-256">メソッドの HttpPost の作成では、テンプレート コードを検証エラーをチェックし、データベースに新しいインストラクターを追加する前に、Courses ナビゲーション プロパティを選択した各コースを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-256">The HttpPost Create method adds each selected course to the Courses navigation property before the template code that checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="888d5-257">モデル エラーがある場合でも、コースが追加されたように (たとえば、ユーザーが無効な日付キーになります) をモデル エラーがある場合に加えられたコースの選択が自動的に復元するとエラー メッセージで、ページが表示され、ようにします。</span><span class="sxs-lookup"><span data-stu-id="888d5-257">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date) so that when the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="888d5-258">コースを `Courses` ナビゲーション プロパティに追加できるようにするには、プロパティを空のコレクションとして初期化する必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="888d5-258">Notice that in order to be able to add courses to the `Courses` navigation property you have to initialize the property as an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="888d5-259">コントローラー コードでこれを行うための別の方法として、Instructor モデルでこれを行うことができます。このためには、プロパティ ゲッターを変更して、コレクションが存在しない場合に自動的に作成するようにします。次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="888d5-259">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="888d5-260">`Courses` プロパティをこの方法で変更する場合、コントローラー内の明示的なプロパティの初期化コードを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="888d5-260">If you modify the `Courses` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="888d5-261">*Views\Instructor\Create.cshtml*、オフィスの場所は、テキスト ボックスを追加し、雇用日フィールドの後と前にコースのチェック ボックス、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="888d5-261">In *Views\Instructor\Create.cshtml*, add an office location text box and course check boxes after the hire date field and before the **Submit** button.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

<span data-ttu-id="888d5-262">コードを貼り付けた後は、改行とインデント設定を編集 ページの前と同じ修正します。</span><span class="sxs-lookup"><span data-stu-id="888d5-262">After you paste the code, fix line breaks and indentation as you did earlier for the Edit page.</span></span>

<span data-ttu-id="888d5-263">ページの作成を実行し、インストラクターを追加します。</span><span class="sxs-lookup"><span data-stu-id="888d5-263">Run the Create page and add an instructor.</span></span>

<a id="transactions"></a>

## <a name="handling-transactions"></a><span data-ttu-id="888d5-264">トランザクションの処理</span><span class="sxs-lookup"><span data-stu-id="888d5-264">Handling transactions</span></span>

<span data-ttu-id="888d5-265">説明したように、[基本 CRUD 機能チュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)、既定では、Entity Framework に暗黙的にはトランザクションを実装します。</span><span class="sxs-lookup"><span data-stu-id="888d5-265">As explained in the [Basic CRUD Functionality tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), by default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="888d5-266">必要があります詳細に制御が - たとえば、--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクション操作](https://msdn.microsoft.com/data/dn456843)msdn です。</span><span class="sxs-lookup"><span data-stu-id="888d5-266">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Working with Transactions](https://msdn.microsoft.com/data/dn456843) on MSDN.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="888d5-267">コードを取得する</span><span class="sxs-lookup"><span data-stu-id="888d5-267">Get the code</span></span>

[<span data-ttu-id="888d5-268">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="888d5-268">Download the Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a><span data-ttu-id="888d5-269">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="888d5-269">Additional resources</span></span>

<span data-ttu-id="888d5-270">その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="888d5-270">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="888d5-271">次の手順</span><span class="sxs-lookup"><span data-stu-id="888d5-271">Next step</span></span>

<span data-ttu-id="888d5-272">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="888d5-272">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="888d5-273">カスタマイズされた courses ページ</span><span class="sxs-lookup"><span data-stu-id="888d5-273">Customized courses pages</span></span>
> * <span data-ttu-id="888d5-274">Instructors ページに office を追加しました</span><span class="sxs-lookup"><span data-stu-id="888d5-274">Added office to instructors page</span></span>
> * <span data-ttu-id="888d5-275">Instructors ページに追加されたコース</span><span class="sxs-lookup"><span data-stu-id="888d5-275">Added courses to instructors page</span></span>
> * <span data-ttu-id="888d5-276">更新された DeleteConfirmed</span><span class="sxs-lookup"><span data-stu-id="888d5-276">Updated DeleteConfirmed</span></span>
> * <span data-ttu-id="888d5-277">追加されたオフィスの場所とコースを作成 ページ</span><span class="sxs-lookup"><span data-stu-id="888d5-277">Added office location and courses to the Create page</span></span>

<span data-ttu-id="888d5-278">非同期のプログラミング モデルを実装する方法については、次の記事に進んでください。</span><span class="sxs-lookup"><span data-stu-id="888d5-278">Advance to the next article to learn how to implement an asynchronous programming model.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="888d5-279">非同期プログラミング モデル</span><span class="sxs-lookup"><span data-stu-id="888d5-279">Asynchronous programming model</span></span>](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
