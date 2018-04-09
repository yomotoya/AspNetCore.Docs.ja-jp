---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーションに Entity Framework と関連するデータの更新 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cf4a6183e068e8668eb706d9a9e311616649e863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="472af-103">ASP.NET MVC アプリケーションに Entity Framework と関連するデータの更新</span><span class="sxs-lookup"><span data-stu-id="472af-103">Updating Related Data with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="472af-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="472af-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="472af-105">[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="472af-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="472af-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="472af-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="472af-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="472af-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="472af-108">前のチュートリアルでは、関連するデータが表示されます。このチュートリアルでは関連するデータを更新します。</span><span class="sxs-lookup"><span data-stu-id="472af-108">In the previous tutorial you displayed related data; in this tutorial you'll update related data.</span></span> <span data-ttu-id="472af-109">ほとんどのリレーションシップでこれを外部キー フィールドまたはナビゲーション プロパティのいずれかの更新します。</span><span class="sxs-lookup"><span data-stu-id="472af-109">For most relationships, this can be done by updating either foreign key fields or navigation properties.</span></span> <span data-ttu-id="472af-110">多対多のリレーションシップで Entity Framework は開始されません結合テーブルを直接追加し、適切なナビゲーション プロパティからエンティティを削除するようにします。</span><span class="sxs-lookup"><span data-stu-id="472af-110">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="472af-111">以下の図は、使用するページの一部を示しています。</span><span class="sxs-lookup"><span data-stu-id="472af-111">The following illustrations show some of the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![コースをインストラクター編集](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="472af-115">Courses の Create ページと Edit ページをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="472af-115">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="472af-116">新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。</span><span class="sxs-lookup"><span data-stu-id="472af-116">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="472af-117">これを容易にするため、スキャフォールディング コードには、コントローラーのメソッドと、部門を選択するためのドロップダウン リストを含む Create ビューと Edit ビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="472af-117">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="472af-118">ドロップダウン リストを設定、`Course.DepartmentID`外部キーのプロパティを読み込むために、Entity Framework が必要なすべてが、`Department`ナビゲーション プロパティが、適切な`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="472af-118">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="472af-119">このスキャフォールディング コードを使用しますが、エラー処理を追加し、ドロップダウン リストを並べ替えるために少し変更します。</span><span class="sxs-lookup"><span data-stu-id="472af-119">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="472af-120">*CourseController.cs*、4 つの削除`Create`と`Edit`メソッドに次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="472af-120">In *CourseController.cs*, delete the four `Create` and `Edit` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

<span data-ttu-id="472af-121">次の追加`using`ファイルの先頭にステートメント。</span><span class="sxs-lookup"><span data-stu-id="472af-121">Add the following `using` statement at the beginning of the file:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="472af-122">`PopulateDepartmentsDropDownList`メソッド名でソートのすべての部門の一覧を取得、作成、`SelectList`ドロップダウン一覧は、コレクション内のビューに、コレクションを渡すと、`ViewBag`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="472af-122">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="472af-123">このメソッドは、ドロップダウン リストがレンダリングされるときに選択される項目を指定するためのコード呼び出しを許可する、省略可能な `selectedDepartment` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="472af-123">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="472af-124">ビューが名前を渡す`DepartmentID`を[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)とヘルパーのヘルパーに渡し、ファイルの場所を認識し、`ViewBag`オブジェクトに対する、`SelectList`という名前`DepartmentID`です。</span><span class="sxs-lookup"><span data-stu-id="472af-124">The view will pass the name `DepartmentID` to the [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="472af-125">`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択したアイテムを設定しなくてもメソッド。</span><span class="sxs-lookup"><span data-stu-id="472af-125">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="472af-126">`HttpGet` `Edit`メソッドは編集されているコースに既に割り当てられている部門の ID に基づいて、選択したアイテムを設定します。</span><span class="sxs-lookup"><span data-stu-id="472af-126">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

<span data-ttu-id="472af-127">`HttpPost`両方のメソッド`Create`と`Edit`もエラーが発生したページを再表示するときに、選択した項目を設定するコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="472af-127">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

<span data-ttu-id="472af-128">このコードにより、いるエラー メッセージを表示する、ページが表示され、ときにどのような部門が選択されている選択されたままとします。</span><span class="sxs-lookup"><span data-stu-id="472af-128">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="472af-129">コース ビューは、部門フィールドのドロップダウン リストを持つスキャフォールディングされた既にを使用しない DepartmentID キャプションこのフィールドは、変更、次の強調表示するように、 *Views\Course\Create.cshtml*ファイルの名前をキャプションを変更します。</span><span class="sxs-lookup"><span data-stu-id="472af-129">The Course views are already scaffolded with drop-down lists for the department field, but you don't want the DepartmentID caption for this field, so make the following highlighted change to the *Views\Course\Create.cshtml* file to change the caption.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

<span data-ttu-id="472af-130">同じ変更を加える*Views\Course\Edit.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="472af-130">Make the same change in *Views\Course\Edit.cshtml*.</span></span>

<span data-ttu-id="472af-131">通常、scaffolder は、キーの値は、データベースによって生成されるとは変更できませんあり、意味のあるユーザーに表示される値ではないために、主キーをスキャフォールディングしません。</span><span class="sxs-lookup"><span data-stu-id="472af-131">Normally the scaffolder doesn't scaffold a primary key because the key value is generated by the database and can't be changed and isn't a meaningful value to be displayed to users.</span></span> <span data-ttu-id="472af-132">コースのエンティティ、scaffolder はテキスト ボックスを`CourseID`を認識するためのフィールド、`DatabaseGeneratedOption.None`属性は、ユーザーができることを意味主キーの値を入力します。</span><span class="sxs-lookup"><span data-stu-id="472af-132">For Course entities the scaffolder does include an text box for the `CourseID` field because it understands that the `DatabaseGeneratedOption.None` attribute means the user should be able enter the primary key value.</span></span> <span data-ttu-id="472af-133">数値が意味のあることを手動で追加する必要がありますので、他のビューで参照を理解していないこと。</span><span class="sxs-lookup"><span data-stu-id="472af-133">But it doesn't understand that because the number is meaningful you want to see it in the other views, so you need to add it manually.</span></span>

<span data-ttu-id="472af-134">*Views\Course\Edit.cshtml*、コースの前に数値フィールドを追加、**タイトル**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="472af-134">In *Views\Course\Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="472af-135">主キーであるためにが表示されますが、変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="472af-135">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

<span data-ttu-id="472af-136">非表示フィールドが既に存在 (`Html.HiddenFor`ヘルパー) の編集ビューでコース番号。</span><span class="sxs-lookup"><span data-stu-id="472af-136">There's already a hidden field (`Html.HiddenFor` helper) for the course number in the Edit view.</span></span> <span data-ttu-id="472af-137">追加する、 *Html.LabelFor* 、ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号が原因であるために、ヘルパーは隠しフィールドの必要性を排除しない**保存**[編集] ページ。</span><span class="sxs-lookup"><span data-stu-id="472af-137">Adding an *Html.LabelFor* helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the Edit page.</span></span>

<span data-ttu-id="472af-138">*Views\Course\Delete.cshtml*と*Views\Course\Details.cshtml*部門名キャプションを"Department"を"Name"からの変更、および前にコース数値フィールドを追加、**タイトル**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="472af-138">In *Views\Course\Delete.cshtml* and *Views\Course\Details.cshtml*, change the department name caption from "Name" to "Department" and add a course number field before the **Title** field.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

<span data-ttu-id="472af-139">実行、**作成**ページ (コース インデックス ページを表示し、をクリックして**新規作成**) と新しいコースのデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="472af-139">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="472af-141">**[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="472af-141">Click **Create**.</span></span> <span data-ttu-id="472af-142">一覧に追加された新しいコース コース インデックス ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="472af-142">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="472af-143">Index ページのリストの部門名は、ナビゲーション プロパティから取得され、リレーションシップが正常に確立されていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="472af-143">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="472af-145">実行、**編集**ページ (コース インデックス ページを表示し、をクリックして**編集**コースで)。</span><span class="sxs-lookup"><span data-stu-id="472af-145">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="472af-147">ページ上のデータを変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="472af-147">Change data on the page and click **Save**.</span></span> <span data-ttu-id="472af-148">コース インデックス ページには、更新された一連のデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="472af-148">The Course Index page is displayed with the updated course data.</span></span>

## <a name="adding-an-edit-page-for-instructors"></a><span data-ttu-id="472af-149">講習においてインストラクター編集ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-149">Adding an Edit Page for Instructors</span></span>

<span data-ttu-id="472af-150">インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="472af-150">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="472af-151">`Instructor`エンティティと 0 または 1 を 1 つの関係には、`OfficeAssignment`エンティティで、次の状況を処理しなければならないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="472af-151">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="472af-152">ユーザーがオフィス割り当てをクリアし、値が最初にあった場合、は、解除し、削除する必要があります、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="472af-152">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="472af-153">新規に作成する必要があります office 代入値を入力すると、空か最初、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="472af-153">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="472af-154">Office の代入式の値を変更すると場合、は、既存の値を変更する必要があります`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="472af-154">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="472af-155">開いている*InstructorController.cs*見ると、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="472af-155">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="472af-156">スキャフォールディング コードをここでは、する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="472af-156">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="472af-157">データを設定するには、ドロップダウン リストが必要なものは、テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="472af-157">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="472af-158">このメソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="472af-158">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

<span data-ttu-id="472af-159">このコードは、`ViewBag`ステートメント、関連付けられているため、一括読み込みを追加および`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="472af-159">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="472af-160">一括読み込みを行うことはできません、`Find`メソッド、ため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="472af-160">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="472af-161">置換、 `HttpPost` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="472af-161">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="472af-162">これは、割り当ての office 更新プログラムを処理します。</span><span class="sxs-lookup"><span data-stu-id="472af-162">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="472af-163">参照を`RetryLimitExceededException`が必要です、`using`ステートメントですこれを追加するには、を右クリック`RetryLimitExceededException`、順にクリック**解決** - **System.Data.Entity.Infrastructureを使用して**.</span><span class="sxs-lookup"><span data-stu-id="472af-163">The reference to `RetryLimitExceededException` requires a `using` statement; to add it, right-click `RetryLimitExceededException`, and then click **Resolve** - **using System.Data.Entity.Infrastructure**.</span></span>

![再試行の例外を解決するには](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="472af-165">このコードは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="472af-165">The code does the following:</span></span>

- <span data-ttu-id="472af-166">メソッドの名前を変更`EditPost`署名が、同じため、`HttpGet`メソッド (、`ActionName`属性は、/Edit/URL はまだ使用されていることを指定します)。</span><span class="sxs-lookup"><span data-stu-id="472af-166">Changes the method name to `EditPost` because the signature is now the same as the `HttpGet` method (the `ActionName` attribute specifies that the /Edit/ URL is still used).</span></span>
- <span data-ttu-id="472af-167">`OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の `Instructor` エンティティをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="472af-167">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="472af-168">これと同じで行った、 `HttpGet` `Edit`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="472af-168">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="472af-169">モデル バインダーからの値を使用して、取得した `Instructor` エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="472af-169">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="472af-170">[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用オーバー ロードを使用する*ホワイト リスト*に含めるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="472af-170">The [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="472af-171">こうすれば、過剰な投稿で説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="472af-171">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- <span data-ttu-id="472af-172">オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null にできるように、関連する行で、`OfficeAssignment`テーブルは削除されます。</span><span class="sxs-lookup"><span data-stu-id="472af-172">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- <span data-ttu-id="472af-173">データベースへの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="472af-173">Saves the changes to the database.</span></span>

<span data-ttu-id="472af-174">*Views\Instructor\Edit.cshtml*の後に、`div`用の要素、 **Hire Date**フィールドに、オフィスの場所を編集するための新しいフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-174">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

<span data-ttu-id="472af-175">ページの実行 (選択、**講習においてインストラクター**  タブをクリックして**編集**インストラクターに)。</span><span class="sxs-lookup"><span data-stu-id="472af-175">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="472af-176">**[Office Location]\(オフィスの場所\)** を変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="472af-176">Change the **Office Location** and click **Save**.</span></span>

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="472af-178">[編集] ページをインストラクターにコースの課題を追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-178">Adding Course Assignments to the Instructor Edit Page</span></span>

<span data-ttu-id="472af-179">インストラクターは、任意の数のコースを担当する場合があります。</span><span class="sxs-lookup"><span data-stu-id="472af-179">Instructors may teach any number of courses.</span></span> <span data-ttu-id="472af-180">次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、コースの割り当てを変更する機能を追加して、Instructor/Edit ページを拡張します。</span><span class="sxs-lookup"><span data-stu-id="472af-180">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="472af-182">間のリレーションシップ、`Course`と`Instructor`エンティティは多対多の結合テーブルでは、外部キーのプロパティに直接アクセスする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="472af-182">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the foreign key properties which are in the join table.</span></span> <span data-ttu-id="472af-183">代わりの追加し、削除のエンティティとの間、`Instructor.Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="472af-183">Instead, you add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="472af-184">インストラクターに割り当てられるコースを変更できるようにする UI は、チェック ボックスのグループです。</span><span class="sxs-lookup"><span data-stu-id="472af-184">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="472af-185">データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているコースが選択されます。</span><span class="sxs-lookup"><span data-stu-id="472af-185">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="472af-186">ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。</span><span class="sxs-lookup"><span data-stu-id="472af-186">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="472af-187">コースの数が非常に大きいと場合、は、他のビューでは、データの表示方法を使用することはおそらくが作成またはリレーションシップを削除するためにナビゲーション プロパティを操作するための同じメソッドを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="472af-187">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="472af-188">チェック ボックスのリストのためにデータをビューに提供するには、ビュー モデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="472af-188">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="472af-189">作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。</span><span class="sxs-lookup"><span data-stu-id="472af-189">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="472af-190">*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="472af-190">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="472af-191">変更が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="472af-191">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

<span data-ttu-id="472af-192">このコードは、`Courses` ナビゲーション プロパティに一括読み込みを追加し、新しい `PopulateAssignedCourseData` メソッドを呼び出して、`AssignedCourseData` ビュー モデル クラスを使用してチェック ボックス配列に情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="472af-192">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="472af-193">内のコード、`PopulateAssignedCourseData`メソッドはすべて読み取り`Course`ビューを使用してコースの一覧を読み込むためにエンティティ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="472af-193">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="472af-194">各コースに対し、コードはそのコースがインストラクターの `Courses` ナビゲーション プロパティ内に存在しているかどうかをチェックします。</span><span class="sxs-lookup"><span data-stu-id="472af-194">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="472af-195">コースをインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/library/bb359438.aspx)コレクション。</span><span class="sxs-lookup"><span data-stu-id="472af-195">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="472af-196">`Assigned`プロパティに設定されている`true`コース講師が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="472af-196">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="472af-197">ビューは、このプロパティを使用して、どのチェック ボックスを選択済みとして表示する必要があるかを判断します。</span><span class="sxs-lookup"><span data-stu-id="472af-197">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="472af-198">最後に、一覧は、ビューに渡される、`ViewBag`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="472af-198">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="472af-199">次に、ユーザーが **[Save]\(保存\)** をクリックしたときに実行されるコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-199">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="472af-200">置換、`EditPost`メソッドを次のコードを更新する新しいメソッドを呼び出した、`Courses`のナビゲーション プロパティ、`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="472af-200">Replace the `EditPost` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="472af-201">変更が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="472af-201">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

<span data-ttu-id="472af-202">メソッドのシグネチャが異なるようになりました、 `HttpGet` `Edit`メソッド、メソッド名の変更から`EditPost`に`Edit`です。</span><span class="sxs-lookup"><span data-stu-id="472af-202">The method signature is now different from the `HttpGet` `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="472af-203">ビューのコレクションがあるないため`Course`エンティティ、モデル バインダーは自動的に更新、`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="472af-203">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="472af-204">モデル バインダーを使用して更新するのではなく、`Courses`ナビゲーション プロパティで、新しい実行を`UpdateInstructorCourses`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="472af-204">Instead of using the model binder to update the `Courses` navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="472af-205">そのため、モデル バインドから `Courses` プロパティを除外する必要があります。</span><span class="sxs-lookup"><span data-stu-id="472af-205">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="472af-206">呼び出すコードの変更は必要ありません[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト*オーバー ロードと`Courses`は include リストではありません。</span><span class="sxs-lookup"><span data-stu-id="472af-206">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="472af-207">場合チェックを行わないボックスを選択した場合、コードで`UpdateInstructorCourses`を初期化、`Courses`ナビゲーション プロパティが空のコレクション。</span><span class="sxs-lookup"><span data-stu-id="472af-207">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="472af-208">その後コードは、データベース内のすべてのコースをループ処理し、各コースを現在インストラクターに割り当てられているコースとビューで選択されているコースを比較してチェックします。</span><span class="sxs-lookup"><span data-stu-id="472af-208">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="472af-209">検索を効率化するため、最後の 2 つのコレクションが `HashSet` オブジェクトに格納されます。</span><span class="sxs-lookup"><span data-stu-id="472af-209">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="472af-210">コースのチェック ボックスが選択されたが、そのコースが `Instructor.Courses`ナビゲーション プロパティにない場合、そのコースがナビゲーション プロパティ内のコレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="472af-210">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

<span data-ttu-id="472af-211">コースのチェック ボックスが選択さていないが、そのコースが `Instructor.Courses`ナビゲーション プロパティにある場合、そのコースがナビゲーション プロパティから削除されます。</span><span class="sxs-lookup"><span data-stu-id="472af-211">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="472af-212">*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを次を追加することで配列にコードの直後に、`div`用の要素、`OfficeAssignment`フィールドと前に、`div`要素を**保存**ボタン。</span><span class="sxs-lookup"><span data-stu-id="472af-212">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the `OfficeAssignment` field and before the `div` element for the **Save** button:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

<span data-ttu-id="472af-213">貼り付けた後、コード、改行する場合とインデントされないように見えます。 ここでは、ここに表示するように見えるようににすべてを手動で修正します。</span><span class="sxs-lookup"><span data-stu-id="472af-213">After you paste the code, if line breaks and indentation don't look like they do here, manually fix everything so that it looks like what you see here.</span></span> <span data-ttu-id="472af-214">インデントは完璧である必要はありませんが、`@</tr><tr>`、`@:<td>`、`@:</td>`、および `@</tr>` の行は、示されているようにそれぞれ 1 行にする必要があります。そうしないと、ランタイム エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="472af-214">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span>

<span data-ttu-id="472af-215">このコードは、3 つの列を含む HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="472af-215">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="472af-216">各列には、チェック ボックスとその後に続くキャプションがあります。キャプションは、コース番号とタイトルから構成されます。</span><span class="sxs-lookup"><span data-stu-id="472af-216">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="472af-217">すべてのチェック ボックスをグループとして扱う場合にモデル バインダーを通知する同じ名前 ("selectedCourses") であります。</span><span class="sxs-lookup"><span data-stu-id="472af-217">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="472af-218">`value`各チェック ボックスの属性がの値に設定されている`CourseID.`モデル バインダーがコント ローラーで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスが選択されている値。</span><span class="sxs-lookup"><span data-stu-id="472af-218">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="472af-219">適合インストラクターに割り当てられているコースのチェック ボックスが表示される最初と`checked`属性は、それらを (それらのチェックが表示される) を選択します。</span><span class="sxs-lookup"><span data-stu-id="472af-219">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="472af-220">コースの割り当てを変更した後にしておくに戻ると、サイトに変更を確認できる、`Index`ページ。</span><span class="sxs-lookup"><span data-stu-id="472af-220">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="472af-221">そのため、そのページ内のテーブルに列を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="472af-221">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="472af-222">使用する必要はありませんここでは、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてそのページへ渡そうとしているエンティティ。</span><span class="sxs-lookup"><span data-stu-id="472af-222">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="472af-223">*Views\Instructor\Index.cshtml*、追加、**コース**見出しの直後、 **Office**見出しで、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="472af-223">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

<span data-ttu-id="472af-224">オフィスの場所の詳細セルに引き続いてすぐに新しい詳細セルを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-224">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

<span data-ttu-id="472af-225">実行、**インストラクター インデックス**ページを参照する各インストラクターに割り当てられているコース。</span><span class="sxs-lookup"><span data-stu-id="472af-225">Run the **Instructor Index** page to see the courses assigned to each instructor:</span></span>

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="472af-227">をクリックして**編集**の編集 ページを表示するには、あるインストラクターにします。</span><span class="sxs-lookup"><span data-stu-id="472af-227">Click **Edit** on an instructor to see the Edit page.</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

<span data-ttu-id="472af-229">一部のコース割り当てを変更し、クリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="472af-229">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="472af-230">行った変更が Index ページに反映されます。</span><span class="sxs-lookup"><span data-stu-id="472af-230">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="472af-231">注: インストラクター コースのデータを編集するのには、ここに採用されているアプローチは、コースの数に制限がある場合にも動作します。</span><span class="sxs-lookup"><span data-stu-id="472af-231">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="472af-232">非常に大きいコレクションの場合、別の UI と別の更新方法が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="472af-232">For collections that are much larger, a different UI and a different updating method would be required.</span></span>  
 

## <a name="update-the-deleteconfirmed-method"></a><span data-ttu-id="472af-233">Update DeleteConfirmed メソッド</span><span class="sxs-lookup"><span data-stu-id="472af-233">Update the DeleteConfirmed Method</span></span>

<span data-ttu-id="472af-234">*InstructorController.cs*、削除、`DeleteConfirmed`代わりに、次のコードにメソッドおよび挿入します。</span><span class="sxs-lookup"><span data-stu-id="472af-234">In *InstructorController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

<span data-ttu-id="472af-235">このコードは、次の変更を加え。</span><span class="sxs-lookup"><span data-stu-id="472af-235">This code makes the following change:</span></span>

- <span data-ttu-id="472af-236">講師がその他の部門の管理者として割り当てられている場合は、その部門からインストラクター割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="472af-236">If the instructor is assigned as administrator of any department, removes the instructor assignment from that department.</span></span> <span data-ttu-id="472af-237">このコードがない場合、取得した参照整合性エラー部門の管理者として割り当てられている講師を削除しようとしています。 します。</span><span class="sxs-lookup"><span data-stu-id="472af-237">Without this code, you would get a referential integrity error if you tried to delete an instructor who was assigned as administrator for a department.</span></span>

<span data-ttu-id="472af-238">このコードは、複数の部門の管理者として割り当てられている 1 つのインストラクターのシナリオを処理しません。</span><span class="sxs-lookup"><span data-stu-id="472af-238">This code doesn't handle the scenario of one instructor assigned as administrator for multiple departments.</span></span> <span data-ttu-id="472af-239">前回のチュートリアルでは、そのシナリオが発生しないようにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-239">In the last tutorial you'll add code that prevents that scenario from happening.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="472af-240">オフィスの場所とコースを Create ページに追加する</span><span class="sxs-lookup"><span data-stu-id="472af-240">Add office location and courses to the Create page</span></span>

<span data-ttu-id="472af-241">*InstructorController.cs*、削除、`HttpGet`と`HttpPost``Create`メソッド、代わりに、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-241">In *InstructorController.cs*, delete the `HttpGet` and `HttpPost` `Create` methods, and then add the following code in their place:</span></span>


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="472af-242">このコードは、新機能を確認しました編集メソッドの最初にコースが選択されていないする点を除いてに似ています。</span><span class="sxs-lookup"><span data-stu-id="472af-242">This code is similar to what you saw for the Edit methods except that initially no courses are selected.</span></span> <span data-ttu-id="472af-243">`HttpGet` `Create`メソッドの呼び出し、`PopulateAssignedCourseData`メソッド コースが選択されている場合がある可能性がありますのでない注文の空のコレクションを提供する、 `foreach` (それ以外の場合、コードの表示は、例外をスロー null 参照ビューでループ).</span><span class="sxs-lookup"><span data-stu-id="472af-243">The `HttpGet` `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="472af-244">HttpPost 作成メソッドでは、テンプレート コードの検証エラーをチェックし、データベースに新しいインストラクターを追加する前に、コースのナビゲーション プロパティを選択した各コースを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-244">The HttpPost Create method adds each selected course to the Courses navigation property before the template code that checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="472af-245">モデル エラーがある場合でも、コースが追加されたようにモデル エラー (例については、無効な日付をキーとしたユーザー) があるときに行ったコース選択内容を自動的に復元すると、エラー メッセージで、ページが表示され、ようにします。</span><span class="sxs-lookup"><span data-stu-id="472af-245">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date) so that when the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="472af-246">コースを `Courses` ナビゲーション プロパティに追加できるようにするには、プロパティを空のコレクションとして初期化する必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="472af-246">Notice that in order to be able to add courses to the `Courses` navigation property you have to initialize the property as an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="472af-247">コントローラー コードでこれを行うための別の方法として、Instructor モデルでこれを行うことができます。このためには、プロパティ ゲッターを変更して、コレクションが存在しない場合に自動的に作成するようにします。次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="472af-247">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

<span data-ttu-id="472af-248">`Courses` プロパティをこの方法で変更する場合、コントローラー内の明示的なプロパティの初期化コードを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="472af-248">If you modify the `Courses` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="472af-249">*Views\Instructor\Create.cshtml*オフィスの場所テキスト ボックスを追加、および、雇用日フィールドの後と前に、チェック ボックスをコース、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="472af-249">In *Views\Instructor\Create.cshtml*, add an office location text box and course check boxes after the hire date field and before the **Submit** button.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

<span data-ttu-id="472af-250">コードを貼り付けると後、は、改行してインデントを編集 ページの前に行ったように修正できます。</span><span class="sxs-lookup"><span data-stu-id="472af-250">After you paste the code, fix line breaks and indentation as you did earlier for the Edit page.</span></span>

<span data-ttu-id="472af-251">ページの作成を実行し、インストラクターを追加します。</span><span class="sxs-lookup"><span data-stu-id="472af-251">Run the Create page and add an instructor.</span></span>

![講師がコースを作成します。](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a><span data-ttu-id="472af-253">トランザクションの処理</span><span class="sxs-lookup"><span data-stu-id="472af-253">Handling Transactions</span></span>

<span data-ttu-id="472af-254">説明に従って、[基本的な CRUD 機能チュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)既定では、Entity Framework に暗黙的に実装するトランザクション。</span><span class="sxs-lookup"><span data-stu-id="472af-254">As explained in the [Basic CRUD Functionality tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), by default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="472af-255">必要な複数を制御する--など--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクションを使用](https://msdn.microsoft.com/data/dn456843)msdn です。</span><span class="sxs-lookup"><span data-stu-id="472af-255">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Working with Transactions](https://msdn.microsoft.com/data/dn456843) on MSDN.</span></span>

## <a name="summary"></a><span data-ttu-id="472af-256">まとめ</span><span class="sxs-lookup"><span data-stu-id="472af-256">Summary</span></span>

<span data-ttu-id="472af-257">この概要に関連するデータの操作が完了しました。</span><span class="sxs-lookup"><span data-stu-id="472af-257">You have now completed this introduction to working with related data.</span></span> <span data-ttu-id="472af-258">これまでにこれらのチュートリアルは、同期 I/O を実行するコードで作業しました。</span><span class="sxs-lookup"><span data-stu-id="472af-258">So far in these tutorials you've worked with code that does synchronous I/O.</span></span> <span data-ttu-id="472af-259">非同期のコードを実装することによって、web サーバーのリソースをより効率的に使用するアプリケーションを行い、それが次のチュートリアルで実行されます。</span><span class="sxs-lookup"><span data-stu-id="472af-259">You can make the application use web server resources more efficiently by implementing asynchronous code, and that's what you'll do in the next tutorial.</span></span>

<span data-ttu-id="472af-260">このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="472af-260">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="472af-261">新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。</span><span class="sxs-lookup"><span data-stu-id="472af-261">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="472af-262">その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。</span><span class="sxs-lookup"><span data-stu-id="472af-262">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="472af-263">[前へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="472af-263">[Previous](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
