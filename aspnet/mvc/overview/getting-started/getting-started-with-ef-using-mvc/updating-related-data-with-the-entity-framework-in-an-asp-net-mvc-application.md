---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーションで Entity Framework で関連データの更新 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 05/01/2015
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e7f5fd725a0d151f19f49be9ceaf52b049d459c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831989"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="5fbf8-103">ASP.NET MVC アプリケーションで Entity Framework で関連データの更新</span><span class="sxs-lookup"><span data-stu-id="5fbf8-103">Updating Related Data with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="5fbf8-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5fbf8-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5fbf8-105">[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="5fbf8-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="5fbf8-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 6 Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="5fbf8-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="5fbf8-108">前のチュートリアルには、関連データが表示されます。このチュートリアルでは、関連するデータを更新します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-108">In the previous tutorial you displayed related data; in this tutorial you'll update related data.</span></span> <span data-ttu-id="5fbf8-109">ほとんどのリレーションシップは、これは外部キー フィールドまたはナビゲーション プロパティを更新することで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-109">For most relationships, this can be done by updating either foreign key fields or navigation properties.</span></span> <span data-ttu-id="5fbf8-110">多対多のリレーションシップで Entity Framework は結合テーブルを直接公開を追加し、該当するナビゲーション プロパティからエンティティを削除するようにします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-110">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="5fbf8-111">以下の図は、使用するページの一部を示しています。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-111">The following illustrations show some of the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![コースで講師の編集](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="5fbf8-115">Courses の Create ページと Edit ページをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="5fbf8-115">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="5fbf8-116">新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-116">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="5fbf8-117">これを容易にするため、スキャフォールディング コードには、コントローラーのメソッドと、部門を選択するためのドロップダウン リストを含む Create ビューと Edit ビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-117">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="5fbf8-118">ドロップダウン リストのセット、`Course.DepartmentID`外部キー プロパティは、Entity Framework は、読み込むために必要なすべて、`Department`ナビゲーション プロパティを適切な`Department`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-118">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="5fbf8-119">このスキャフォールディング コードを使用しますが、エラー処理を追加し、ドロップダウン リストを並べ替えるために少し変更します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-119">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="5fbf8-120">*CourseController.cs*、4 つの削除`Create`と`Edit`メソッドし、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-120">In *CourseController.cs*, delete the four `Create` and `Edit` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

<span data-ttu-id="5fbf8-121">次の追加`using`ファイルの先頭にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-121">Add the following `using` statement at the beginning of the file:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="5fbf8-122">`PopulateDepartmentsDropDownList`メソッドの名前で並べ替えたすべての部門の一覧を取得、作成、`SelectList`のドロップダウン リストでは、コレクション ビューに、コレクションを渡すと、`ViewBag`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-122">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="5fbf8-123">このメソッドは、ドロップダウン リストがレンダリングされるときに選択される項目を指定するためのコード呼び出しを許可する、省略可能な `selectedDepartment` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-123">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="5fbf8-124">ビューが名前を渡す`DepartmentID`を[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper、およびヘルパー ファイルの場所を認識し、`ViewBag`オブジェクト、`SelectList`という名前`DepartmentID`します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-124">The view will pass the name `DepartmentID` to the [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="5fbf8-125">`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択した項目を設定せずメソッド。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-125">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="5fbf8-126">`HttpGet` `Edit`メソッドが既に編集中のコースに割り当てられている部門の ID に基づいて、選択した項目を設定します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-126">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

<span data-ttu-id="5fbf8-127">`HttpPost`両方のメソッド`Create`と`Edit`も、ページを再表示エラーが発生したときに、選択した項目を設定するコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-127">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

<span data-ttu-id="5fbf8-128">このコードによりエラー メッセージを表示、ページが表示されときに、選択した常時されます部門が選択されているようになります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-128">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="5fbf8-129">Course ビューは、department フィールドのドロップダウン リストで既にスキャフォールディングされたが、次の強調表示されていることを変更するため、このフィールドでは、DepartmentID キャプションをたくない、 *Views\Course\Create.cshtml*ファイルキャプションを変更します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-129">The Course views are already scaffolded with drop-down lists for the department field, but you don't want the DepartmentID caption for this field, so make the following highlighted change to the *Views\Course\Create.cshtml* file to change the caption.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

<span data-ttu-id="5fbf8-130">同じ変更を行う*Views\Course\Edit.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-130">Make the same change in *Views\Course\Edit.cshtml*.</span></span>

<span data-ttu-id="5fbf8-131">通常、スキャフォールダーの主キー スキャフォールディングは、キー値データベースによって生成される変更ことはできず、ユーザーに表示する意味のある値はありません。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-131">Normally the scaffolder doesn't scaffold a primary key because the key value is generated by the database and can't be changed and isn't a meaningful value to be displayed to users.</span></span> <span data-ttu-id="5fbf8-132">Course エンティティの場合、スキャフォルダーは、テキスト ボックスは含ま、`CourseID`フィールドを認識するため、`DatabaseGeneratedOption.None`属性は、ユーザーができることを意味主キーの値を入力します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-132">For Course entities the scaffolder does include an text box for the `CourseID` field because it understands that the `DatabaseGeneratedOption.None` attribute means the user should be able enter the primary key value.</span></span> <span data-ttu-id="5fbf8-133">ただし、番号がわかりやすいためにする手動で追加する必要があるため、他のビューで表示することを理解していません。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-133">But it doesn't understand that because the number is meaningful you want to see it in the other views, so you need to add it manually.</span></span>

<span data-ttu-id="5fbf8-134">*Views\Course\Edit.cshtml*、前にコース番号フィールドを追加、**タイトル**フィールド。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-134">In *Views\Course\Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="5fbf8-135">主キーであるためが表示されますが、変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-135">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

<span data-ttu-id="5fbf8-136">非表示フィールドが既に存在 (`Html.HiddenFor`ヘルパー) の編集ビューで、コース番号。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-136">There's already a hidden field (`Html.HiddenFor` helper) for the course number in the Edit view.</span></span> <span data-ttu-id="5fbf8-137">追加、 *Html.LabelFor* 、ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号が原因であるために、ヘルパーは、隠しフィールドの必要性を排除しない**保存**[編集] ページ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-137">Adding an *Html.LabelFor* helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the Edit page.</span></span>

<span data-ttu-id="5fbf8-138">*Views\Course\Delete.cshtml*と*Views\Course\Details.cshtml*"Department"に"Name"から、部門名キャプションを変更して、前にコース番号フィールドを追加、**タイトル**フィールド。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-138">In *Views\Course\Delete.cshtml* and *Views\Course\Details.cshtml*, change the department name caption from "Name" to "Department" and add a course number field before the **Title** field.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

<span data-ttu-id="5fbf8-139">実行、**作成**ページ (コースのインデックス ページを表示し、をクリックして**新規作成**) 新しいコースのデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-139">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="5fbf8-141">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-141">Click **Create**.</span></span> <span data-ttu-id="5fbf8-142">コースの Index ページには、一覧に追加された新しいコースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-142">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="5fbf8-143">Index ページのリストの部門名は、ナビゲーション プロパティから取得され、リレーションシップが正常に確立されていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-143">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="5fbf8-145">実行、**編集**ページ (コースのインデックス ページを表示し、クリックして**編集**コースで)。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-145">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="5fbf8-147">ページ上のデータを変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-147">Change data on the page and click **Save**.</span></span> <span data-ttu-id="5fbf8-148">コースの Index ページには、更新されたコース データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-148">The Course Index page is displayed with the updated course data.</span></span>

## <a name="adding-an-edit-page-for-instructors"></a><span data-ttu-id="5fbf8-149">Instructors の Edit ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-149">Adding an Edit Page for Instructors</span></span>

<span data-ttu-id="5fbf8-150">インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-150">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="5fbf8-151">`Instructor`エンティティと 0 または 1 に 1 つリレーションシップを持つ、`OfficeAssignment`エンティティは、次の状況を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-151">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="5fbf8-152">解除し、削除する場合は、ユーザーがオフィスの割り当てをクリアした値する必要があります、`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-152">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="5fbf8-153">新規に作成する必要がある場合は、ユーザーがオフィスの割り当ての値を入力し、空か最初、`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-153">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="5fbf8-154">ユーザーは、オフィスの割り当ての値を変更する場合は、既存の値を変更する必要があります`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-154">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="5fbf8-155">開いている*InstructorController.cs*を見て、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-155">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="5fbf8-156">スキャフォールディングされたコードをここでは、対象はありません。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-156">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="5fbf8-157">データの設定には、ドロップダウン リストがテキスト ボックスは、必要なものです。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-157">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="5fbf8-158">このメソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-158">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

<span data-ttu-id="5fbf8-159">このコードを削除、`ViewBag`ステートメントと、一括読み込みを関連付けられている追加`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-159">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="5fbf8-160">一括読み込みを実行することはできません、`Find`メソッド、そのため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-160">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="5fbf8-161">置換、 `HttpPost` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-161">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="5fbf8-162">これは、オフィスの割り当ての更新を処理します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-162">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="5fbf8-163">参照を`RetryLimitExceededException`が必要です、`using`ステートメントであることを追加するには、右クリック`RetryLimitExceededException`、順にクリックします**解決** - **System.Data.Entity.Infrastructureを使用して**.</span><span class="sxs-lookup"><span data-stu-id="5fbf8-163">The reference to `RetryLimitExceededException` requires a `using` statement; to add it, right-click `RetryLimitExceededException`, and then click **Resolve** - **using System.Data.Entity.Infrastructure**.</span></span>

![再試行の例外を解決するには](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="5fbf8-165">このコードは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-165">The code does the following:</span></span>

- <span data-ttu-id="5fbf8-166">メソッド名を変更`EditPost`署名と同じであるため、`HttpGet`メソッド (、`ActionName`属性は、/edit/URL がまだ使用されていることを指定します)。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-166">Changes the method name to `EditPost` because the signature is now the same as the `HttpGet` method (the `ActionName` attribute specifies that the /Edit/ URL is still used).</span></span>
- <span data-ttu-id="5fbf8-167">`OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の `Instructor` エンティティをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-167">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="5fbf8-168">これで行ったのと同じ、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-168">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="5fbf8-169">モデル バインダーからの値を使用して、取得した `Instructor` エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-169">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="5fbf8-170">[Tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用するオーバー ロードを使用する*ホワイト リスト*プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-170">The [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="5fbf8-171">これにより、過剰ポスティングで説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-171">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- <span data-ttu-id="5fbf8-172">オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null ように、関連する行で、`OfficeAssignment`テーブルは削除されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-172">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- <span data-ttu-id="5fbf8-173">データベースへの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-173">Saves the changes to the database.</span></span>

<span data-ttu-id="5fbf8-174">*Views\Instructor\Edit.cshtml*後に、`div`の要素、 **Hire Date**フィールドで、オフィスの場所を編集するための新しいフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-174">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

<span data-ttu-id="5fbf8-175">ページの実行 (選択、 **Instructors**  タブをクリックして**編集**インストラクターで)。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-175">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="5fbf8-176">**[Office Location]\(オフィスの場所\)** を変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-176">Change the **Office Location** and click **Save**.</span></span>

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="5fbf8-178">[編集] ページの講師にコースの割り当てを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-178">Adding Course Assignments to the Instructor Edit Page</span></span>

<span data-ttu-id="5fbf8-179">インストラクターは、任意の数のコースを担当する場合があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-179">Instructors may teach any number of courses.</span></span> <span data-ttu-id="5fbf8-180">次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、コースの割り当てを変更する機能を追加して、Instructor/Edit ページを拡張します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-180">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="5fbf8-182">間のリレーションシップ、`Course`と`Instructor`エンティティは多対多。 つまり、結合テーブル内にある外部キー プロパティに直接アクセスする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-182">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the foreign key properties which are in the join table.</span></span> <span data-ttu-id="5fbf8-183">代わりに、追加し、エンティティを削除して、`Instructor.Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-183">Instead, you add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="5fbf8-184">インストラクターに割り当てられるコースを変更できるようにする UI は、チェック ボックスのグループです。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-184">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="5fbf8-185">データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているコースが選択されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-185">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="5fbf8-186">ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-186">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="5fbf8-187">コースの数が非常に多い場合、ビューでのデータの表示のさまざまなメソッドを使用することが考えられますが、作成またはリレーションシップを削除するにはナビゲーション プロパティを操作するのと同じ方法を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-187">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="5fbf8-188">チェック ボックスのリストのためにデータをビューに提供するには、ビュー モデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-188">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="5fbf8-189">作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-189">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="5fbf8-190">*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-190">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="5fbf8-191">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-191">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

<span data-ttu-id="5fbf8-192">このコードは、`Courses` ナビゲーション プロパティに一括読み込みを追加し、新しい `PopulateAssignedCourseData` メソッドを呼び出して、`AssignedCourseData` ビュー モデル クラスを使用してチェック ボックス配列に情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-192">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="5fbf8-193">内のコード、`PopulateAssignedCourseData`メソッドを読み取り、すべて`Course`ビューを使用したコースのリストを読み込むためにエンティティ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-193">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="5fbf8-194">各コースに対し、コードはそのコースがインストラクターの `Courses` ナビゲーション プロパティ内に存在しているかどうかをチェックします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-194">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="5fbf8-195">コースがインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/library/bb359438.aspx)コレクション。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-195">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="5fbf8-196">`Assigned`プロパティに設定されて`true`コースの講師が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-196">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="5fbf8-197">ビューは、このプロパティを使用して、どのチェック ボックスを選択済みとして表示する必要があるかを判断します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-197">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="5fbf8-198">最後に、一覧がビューに渡される、`ViewBag`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-198">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="5fbf8-199">次に、ユーザーが **[Save]\(保存\)** をクリックしたときに実行されるコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-199">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="5fbf8-200">置換、`EditPost`メソッドを次のコードは、更新プログラムの新しいメソッドを呼び出し、`Courses`のナビゲーション プロパティ、`Instructor`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-200">Replace the `EditPost` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="5fbf8-201">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-201">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

<span data-ttu-id="5fbf8-202">メソッド シグネチャが異なるようになりました、 `HttpGet` `Edit`メソッド、メソッド名の変更から`EditPost`に`Edit`します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-202">The method signature is now different from the `HttpGet` `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="5fbf8-203">ビューのコレクションを持たないため`Course`エンティティ、モデル バインダーを更新できません自動的に、`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-203">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="5fbf8-204">モデル バインダーを使用して更新するのではなく、`Courses`ナビゲーション プロパティでは、新しい行います`UpdateInstructorCourses`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-204">Instead of using the model binder to update the `Courses` navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="5fbf8-205">そのため、モデル バインドから `Courses` プロパティを除外する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-205">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="5fbf8-206">これを呼び出すコードの変更が必要としない[tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト登録*オーバー ロードと`Courses`インクルード一覧に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-206">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="5fbf8-207">場合、ボックスが選択されていないチェック、コードでは、`UpdateInstructorCourses`を初期化します、`Courses`空のコレクションでのナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-207">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="5fbf8-208">その後コードは、データベース内のすべてのコースをループ処理し、各コースを現在インストラクターに割り当てられているコースとビューで選択されているコースを比較してチェックします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-208">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="5fbf8-209">検索を効率化するため、最後の 2 つのコレクションが `HashSet` オブジェクトに格納されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-209">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="5fbf8-210">コースのチェック ボックスが選択されたが、そのコースが `Instructor.Courses`ナビゲーション プロパティにない場合、そのコースがナビゲーション プロパティ内のコレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-210">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="5fbf8-211">コースのチェック ボックスが選択さていないが、そのコースが `Instructor.Courses`ナビゲーション プロパティにある場合、そのコースがナビゲーション プロパティから削除されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-211">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="5fbf8-212">*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを次を追加することで配列をコードの直後に、`div`の要素、`OfficeAssignment`フィールドと前に、`div`の要素、**保存**ボタン。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-212">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the `OfficeAssignment` field and before the `div` element for the **Save** button:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

<span data-ttu-id="5fbf8-213">貼り付けた後、コード、改行の場合とインデントしないたいしてここで、次のように、ここにすべてを手動で修正します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-213">After you paste the code, if line breaks and indentation don't look like they do here, manually fix everything so that it looks like what you see here.</span></span> <span data-ttu-id="5fbf8-214">インデントは完璧である必要はありませんが、`@</tr><tr>`、`@:<td>`、`@:</td>`、および `@</tr>` の行は、示されているようにそれぞれ 1 行にする必要があります。そうしないと、ランタイム エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-214">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span>

<span data-ttu-id="5fbf8-215">このコードは、3 つの列を含む HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-215">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="5fbf8-216">各列には、チェック ボックスとその後に続くキャプションがあります。キャプションは、コース番号とタイトルから構成されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-216">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="5fbf8-217">チェック ボックスはすべて、同じ名前 ("selectedCourses") は、グループとして扱う場合にモデル バインダーに通知があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-217">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="5fbf8-218">`value`各チェック ボックスの属性の値に設定されて`CourseID.`で構成されるコント ローラーにモデル バインダーが配列を渡しますページが投稿されたときに、`CourseID`チェック ボックスが選択されている値。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-218">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="5fbf8-219">インストラクターに割り当てられるコースのチェック ボックスが最初に表示されると、`checked`属性は、(オンになった状態を表示します) それらを選択します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-219">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="5fbf8-220">コースの割り当てを変更した後に、サイトが返されるときに、変更を確認できる必要あります、`Index`ページ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-220">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="5fbf8-221">そのため、そのページ内のテーブルに列を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-221">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="5fbf8-222">ここで使用する必要はありません、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてページに渡しているエンティティ。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-222">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="5fbf8-223">*Views\Instructor\Index.cshtml*、追加、**コース**直後に、次の見出し、 **Office**見出しで、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-223">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

<span data-ttu-id="5fbf8-224">オフィスの場所の詳細セルの直後に続く新しい詳細セルを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-224">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

<span data-ttu-id="5fbf8-225">実行、 **Instructor インデックス**ページに各インストラクターに割り当てられているコースをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-225">Run the **Instructor Index** page to see the courses assigned to each instructor:</span></span>

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="5fbf8-227">クリックして**編集**の講師が Edit ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-227">Click **Edit** on an instructor to see the Edit page.</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

<span data-ttu-id="5fbf8-229">一部のコース割り当てを変更し、クリックして**保存**します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-229">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="5fbf8-230">行った変更が Index ページに反映されます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-230">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="5fbf8-231">注: インストラクター コース データを編集するのには、ここアプローチは、コースの数に制限がある場合にも動作します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-231">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="5fbf8-232">非常に大きいコレクションの場合、別の UI と別の更新方法が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-232">For collections that are much larger, a different UI and a different updating method would be required.</span></span>  
 

## <a name="update-the-deleteconfirmed-method"></a><span data-ttu-id="5fbf8-233">Update DeleteConfirmed メソッド</span><span class="sxs-lookup"><span data-stu-id="5fbf8-233">Update the DeleteConfirmed Method</span></span>

<span data-ttu-id="5fbf8-234">*InstructorController.cs*、削除、`DeleteConfirmed`代わりに、次のコード メソッドおよび挿入します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-234">In *InstructorController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

<span data-ttu-id="5fbf8-235">このコードは、次の変更です。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-235">This code makes the following change:</span></span>

- <span data-ttu-id="5fbf8-236">インストラクターが任意の部門の管理者として割り当てられている場合は、その部門から、インストラクターの割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-236">If the instructor is assigned as administrator of any department, removes the instructor assignment from that department.</span></span> <span data-ttu-id="5fbf8-237">このコードがないが部門の管理者として割り当てられている講師を削除しようとする場合参照整合性エラーが発生したとします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-237">Without this code, you would get a referential integrity error if you tried to delete an instructor who was assigned as administrator for a department.</span></span>

<span data-ttu-id="5fbf8-238">このコードは、複数の部門の管理者として割り当てられている 1 つの講師のシナリオを処理しません。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-238">This code doesn't handle the scenario of one instructor assigned as administrator for multiple departments.</span></span> <span data-ttu-id="5fbf8-239">最後のチュートリアルでは、そのシナリオが発生しないようにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-239">In the last tutorial you'll add code that prevents that scenario from happening.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="5fbf8-240">オフィスの場所とコースを Create ページに追加する</span><span class="sxs-lookup"><span data-stu-id="5fbf8-240">Add office location and courses to the Create page</span></span>

<span data-ttu-id="5fbf8-241">*InstructorController.cs*、削除、`HttpGet`と`HttpPost``Create`メソッド、代わりに次のコードを追加。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-241">In *InstructorController.cs*, delete the `HttpGet` and `HttpPost` `Create` methods, and then add the following code in their place:</span></span>


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="5fbf8-242">このコードは、確認したものの Edit メソッドの最初にコースが選択されていない点が似ています。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-242">This code is similar to what you saw for the Edit methods except that initially no courses are selected.</span></span> <span data-ttu-id="5fbf8-243">`HttpGet` `Create`メソッドの呼び出し、`PopulateAssignedCourseData`メソッド コースが選択されていますがある可能性があるためではなく注文の空のコレクションを提供する、 `foreach` (それ以外の場合、コードの表示は、null 参照例外をスローするビューでのループ).</span><span class="sxs-lookup"><span data-stu-id="5fbf8-243">The `HttpGet` `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="5fbf8-244">メソッドの HttpPost の作成では、テンプレート コードを検証エラーをチェックし、データベースに新しいインストラクターを追加する前に、Courses ナビゲーション プロパティを選択した各コースを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-244">The HttpPost Create method adds each selected course to the Courses navigation property before the template code that checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="5fbf8-245">モデル エラーがある場合でも、コースが追加されたように (たとえば、ユーザーが無効な日付キーになります) をモデル エラーがある場合に加えられたコースの選択が自動的に復元するとエラー メッセージで、ページが表示され、ようにします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-245">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date) so that when the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="5fbf8-246">コースを `Courses` ナビゲーション プロパティに追加できるようにするには、プロパティを空のコレクションとして初期化する必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-246">Notice that in order to be able to add courses to the `Courses` navigation property you have to initialize the property as an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="5fbf8-247">コントローラー コードでこれを行うための別の方法として、Instructor モデルでこれを行うことができます。このためには、プロパティ ゲッターを変更して、コレクションが存在しない場合に自動的に作成するようにします。次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-247">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="5fbf8-248">`Courses` プロパティをこの方法で変更する場合、コントローラー内の明示的なプロパティの初期化コードを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-248">If you modify the `Courses` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="5fbf8-249">*Views\Instructor\Create.cshtml*、オフィスの場所は、テキスト ボックスを追加し、雇用日フィールドの後と前にコースのチェック ボックス、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-249">In *Views\Instructor\Create.cshtml*, add an office location text box and course check boxes after the hire date field and before the **Submit** button.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

<span data-ttu-id="5fbf8-250">コードを貼り付けた後は、改行とインデント設定を編集 ページの前と同じ修正します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-250">After you paste the code, fix line breaks and indentation as you did earlier for the Edit page.</span></span>

<span data-ttu-id="5fbf8-251">ページの作成を実行し、インストラクターを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-251">Run the Create page and add an instructor.</span></span>

![インストラクターがコースを作成します。](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a><span data-ttu-id="5fbf8-253">トランザクションの処理</span><span class="sxs-lookup"><span data-stu-id="5fbf8-253">Handling Transactions</span></span>

<span data-ttu-id="5fbf8-254">説明したように、[基本 CRUD 機能チュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)、既定では、Entity Framework に暗黙的にはトランザクションを実装します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-254">As explained in the [Basic CRUD Functionality tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), by default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="5fbf8-255">必要があります詳細に制御が - たとえば、--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクション操作](https://msdn.microsoft.com/data/dn456843)msdn です。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-255">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Working with Transactions](https://msdn.microsoft.com/data/dn456843) on MSDN.</span></span>

## <a name="summary"></a><span data-ttu-id="5fbf8-256">まとめ</span><span class="sxs-lookup"><span data-stu-id="5fbf8-256">Summary</span></span>

<span data-ttu-id="5fbf8-257">この概要に関連するデータの操作が完了しました。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-257">You have now completed this introduction to working with related data.</span></span> <span data-ttu-id="5fbf8-258">これまでにこれらのチュートリアルで、同期 I/O を実行するコードで作業したことです。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-258">So far in these tutorials you've worked with code that does synchronous I/O.</span></span> <span data-ttu-id="5fbf8-259">非同期のコードを実装することで、web サーバーのリソースをより効率的に使用するアプリケーションを行いは、次のチュートリアルで何があります。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-259">You can make the application use web server resources more efficiently by implementing asynchronous code, and that's what you'll do in the next tutorial.</span></span>

<span data-ttu-id="5fbf8-260">このチュートリアルの立った方法で改善できましたフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-260">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="5fbf8-261">また、新しいトピックを要求することもできます。[表示 Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-261">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="5fbf8-262">その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。</span><span class="sxs-lookup"><span data-stu-id="5fbf8-262">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5fbf8-263">[前へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="5fbf8-263">[Previous](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
