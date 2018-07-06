---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (6/10) で Entity Framework で関連データの更新 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e85162f58ed9826132db8bd854914a14709f709d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809434"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a><span data-ttu-id="efd4b-103">ASP.NET MVC アプリケーション (6/10) で Entity Framework で関連データの更新</span><span class="sxs-lookup"><span data-stu-id="efd4b-103">Updating Related Data with the Entity Framework in an ASP.NET MVC Application (6 of 10)</span></span>
====================
<span data-ttu-id="efd4b-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="efd4b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="efd4b-105">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="efd4b-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="efd4b-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="efd4b-107">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="efd4b-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="efd4b-108">チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。</span><span class="sxs-lookup"><span data-stu-id="efd4b-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="efd4b-109">を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="efd4b-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="efd4b-110">問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="efd4b-111">一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="efd4b-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="efd4b-112">前のチュートリアルには、関連データが表示されます。このチュートリアルでは、関連するデータを更新します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-112">In the previous tutorial you displayed related data; in this tutorial you'll update related data.</span></span> <span data-ttu-id="efd4b-113">ほとんどのリレーションシップは、これは適切な外部キー フィールドを更新することで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-113">For most relationships, this can be done by updating the appropriate foreign key fields.</span></span> <span data-ttu-id="efd4b-114">多対多のリレーションシップで Entity Framework は結合テーブルを直接公開、ため、明示的に追加し、該当するナビゲーション プロパティからエンティティを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-114">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you must explicitly add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="efd4b-115">以下の図は、使用するページを示しています。</span><span class="sxs-lookup"><span data-stu-id="efd4b-115">The following illustrations show the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="efd4b-118">Courses の Create ページと Edit ページをカスタマイズする</span><span class="sxs-lookup"><span data-stu-id="efd4b-118">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="efd4b-119">新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-119">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="efd4b-120">これを容易にするため、スキャフォールディング コードには、コントローラーのメソッドと、部門を選択するためのドロップダウン リストを含む Create ビューと Edit ビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-120">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="efd4b-121">ドロップダウン リストのセット、`Course.DepartmentID`外部キー プロパティは、Entity Framework は、読み込むために必要なすべて、`Department`ナビゲーション プロパティを適切な`Department`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-121">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="efd4b-122">このスキャフォールディング コードを使用しますが、エラー処理を追加し、ドロップダウン リストを並べ替えるために少し変更します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-122">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="efd4b-123">*CourseController.cs*、4 つの削除`Edit`と`Create`メソッドし、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-123">In *CourseController.cs*, delete the four `Edit` and `Create` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

<span data-ttu-id="efd4b-124">`PopulateDepartmentsDropDownList`メソッドの名前で並べ替えたすべての部門の一覧を取得、作成、`SelectList`のドロップダウン リストでは、コレクション ビューに、コレクションを渡すと、`ViewBag`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-124">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="efd4b-125">このメソッドは、ドロップダウン リストがレンダリングされるときに選択される項目を指定するためのコード呼び出しを許可する、省略可能な `selectedDepartment` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-125">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="efd4b-126">ビューが名前を渡す`DepartmentID`に[、`DropDownList`ヘルパー](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)、ヘルパーがファイルの場所を認識し、`ViewBag`オブジェクト、`SelectList`という名前`DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="efd4b-126">The view will pass the name `DepartmentID` to [the `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="efd4b-127">`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択した項目を設定せずメソッド。</span><span class="sxs-lookup"><span data-stu-id="efd4b-127">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="efd4b-128">`HttpGet` `Edit`メソッドが既に編集中のコースに割り当てられている部門の ID に基づいて、選択した項目を設定します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-128">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="efd4b-129">`HttpPost`両方のメソッド`Create`と`Edit`も、ページを再表示エラーが発生したときに、選択した項目を設定するコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-129">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="efd4b-130">このコードによりエラー メッセージを表示、ページが表示されときに、選択した常時されます部門が選択されているようになります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-130">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="efd4b-131">*Views\Course\Create.cshtml*、作成する前に、新しいコース番号フィールドを強調表示されたコードを追加、**タイトル**フィールド。</span><span class="sxs-lookup"><span data-stu-id="efd4b-131">In *Views\Course\Create.cshtml*, add the highlighted code to create a new course number field before the **Title** field.</span></span> <span data-ttu-id="efd4b-132">前述の以前のチュートリアルでは、主キー フィールドが既定では、スキャフォールディングされませんが、この主キーは、ユーザーは、キーの値を入力できるようにするために、わかりやすい。</span><span class="sxs-lookup"><span data-stu-id="efd4b-132">As explained in an earlier tutorial, primary key fields aren't scaffolded by default, but this primary key is meaningful, so you want the user to be able to enter the key value.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

<span data-ttu-id="efd4b-133">*Views\Course\Edit.cshtml*、 *Views\Course\Delete.cshtml*、および*Views\Course\Details.cshtml*、前にコース番号フィールドを追加、**タイトル**フィールド。</span><span class="sxs-lookup"><span data-stu-id="efd4b-133">In *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, and *Views\Course\Details.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="efd4b-134">主キーであるためが表示されますが、変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="efd4b-134">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="efd4b-135">実行、**作成**ページ (コースのインデックス ページを表示し、をクリックして**新規作成**) 新しいコースのデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-135">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="efd4b-137">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="efd4b-137">Click **Create**.</span></span> <span data-ttu-id="efd4b-138">コースの Index ページには、一覧に追加された新しいコースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-138">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="efd4b-139">Index ページのリストの部門名は、ナビゲーション プロパティから取得され、リレーションシップが正常に確立されていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="efd4b-139">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="efd4b-141">実行、**編集**ページ (コースのインデックス ページを表示し、クリックして**編集**コースで)。</span><span class="sxs-lookup"><span data-stu-id="efd4b-141">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="efd4b-143">ページ上のデータを変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="efd4b-143">Change data on the page and click **Save**.</span></span> <span data-ttu-id="efd4b-144">コースの Index ページには、更新されたコース データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-144">The Course Index page is displayed with the updated course data.</span></span>

## <a name="adding-an-edit-page-for-instructors"></a><span data-ttu-id="efd4b-145">Instructors の Edit ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-145">Adding an Edit Page for Instructors</span></span>

<span data-ttu-id="efd4b-146">インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-146">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="efd4b-147">`Instructor`エンティティと 0 または 1 に 1 つリレーションシップを持つ、`OfficeAssignment`エンティティは、次の状況を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-147">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="efd4b-148">解除し、削除する場合は、ユーザーがオフィスの割り当てをクリアした値する必要があります、`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-148">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="efd4b-149">新規に作成する必要がある場合は、ユーザーがオフィスの割り当ての値を入力し、空か最初、`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-149">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="efd4b-150">ユーザーは、オフィスの割り当ての値を変更する場合は、既存の値を変更する必要があります`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-150">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="efd4b-151">開いている*InstructorController.cs*を見て、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="efd4b-151">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="efd4b-152">スキャフォールディングされたコードをここでは、対象はありません。</span><span class="sxs-lookup"><span data-stu-id="efd4b-152">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="efd4b-153">データの設定には、ドロップダウン リストがテキスト ボックスは、必要なものです。</span><span class="sxs-lookup"><span data-stu-id="efd4b-153">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="efd4b-154">このメソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-154">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="efd4b-155">このコードを削除、`ViewBag`ステートメントと、一括読み込みを関連付けられている追加`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-155">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="efd4b-156">一括読み込みを実行することはできません、`Find`メソッド、そのため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-156">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="efd4b-157">置換、 `HttpPost` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="efd4b-157">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="efd4b-158">これは、オフィスの割り当ての更新を処理します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-158">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="efd4b-159">このコードは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="efd4b-159">The code does the following:</span></span>

- <span data-ttu-id="efd4b-160">`OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の `Instructor` エンティティをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-160">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="efd4b-161">これで行ったのと同じ、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="efd4b-161">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="efd4b-162">モデル バインダーからの値を使用して、取得した `Instructor` エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-162">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="efd4b-163">[Tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用するオーバー ロードを使用する*ホワイト リスト*プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-163">The [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="efd4b-164">これにより、過剰ポスティングで説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-164">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- <span data-ttu-id="efd4b-165">オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null ように、関連する行で、`OfficeAssignment`テーブルは削除されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-165">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- <span data-ttu-id="efd4b-166">データベースへの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-166">Saves the changes to the database.</span></span>

<span data-ttu-id="efd4b-167">*Views\Instructor\Edit.cshtml*後に、`div`の要素、 **Hire Date**フィールドで、オフィスの場所を編集するための新しいフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-167">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

<span data-ttu-id="efd4b-168">ページの実行 (選択、 **Instructors**  タブをクリックして**編集**インストラクターで)。</span><span class="sxs-lookup"><span data-stu-id="efd4b-168">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="efd4b-169">**[Office Location]\(オフィスの場所\)** を変更し、**[Save]\(保存\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="efd4b-169">Change the **Office Location** and click **Save**.</span></span>

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="efd4b-171">[編集] ページの講師にコースの割り当てを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-171">Adding Course Assignments to the Instructor Edit Page</span></span>

<span data-ttu-id="efd4b-172">インストラクターは、任意の数のコースを担当する場合があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-172">Instructors may teach any number of courses.</span></span> <span data-ttu-id="efd4b-173">次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、コースの割り当てを変更する機能を追加して、Instructor/Edit ページを拡張します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-173">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="efd4b-175">間のリレーションシップ、`Course`と`Instructor`エンティティは多対多。 つまり、結合テーブルに直接アクセスする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="efd4b-175">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the join table.</span></span> <span data-ttu-id="efd4b-176">追加する代わりに、およびとの間にエンティティを削除するか、`Instructor.Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-176">Instead, you will add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="efd4b-177">インストラクターに割り当てられるコースを変更できるようにする UI は、チェック ボックスのグループです。</span><span class="sxs-lookup"><span data-stu-id="efd4b-177">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="efd4b-178">データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているコースが選択されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-178">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="efd4b-179">ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-179">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="efd4b-180">コースの数が非常に多い場合、ビューでのデータの表示のさまざまなメソッドを使用することが考えられますが、作成またはリレーションシップを削除するにはナビゲーション プロパティを操作するのと同じ方法を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="efd4b-180">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="efd4b-181">チェック ボックスのリストのためにデータをビューに提供するには、ビュー モデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-181">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="efd4b-182">作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-182">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="efd4b-183">*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="efd4b-183">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="efd4b-184">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-184">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

<span data-ttu-id="efd4b-185">このコードは、`Courses` ナビゲーション プロパティに一括読み込みを追加し、新しい `PopulateAssignedCourseData` メソッドを呼び出して、`AssignedCourseData` ビュー モデル クラスを使用してチェック ボックス配列に情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-185">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="efd4b-186">内のコード、`PopulateAssignedCourseData`メソッドを読み取り、すべて`Course`ビューを使用したコースのリストを読み込むためにエンティティ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="efd4b-186">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="efd4b-187">各コースに対し、コードはそのコースがインストラクターの `Courses` ナビゲーション プロパティ内に存在しているかどうかをチェックします。</span><span class="sxs-lookup"><span data-stu-id="efd4b-187">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="efd4b-188">コースがインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/library/bb359438.aspx)コレクション。</span><span class="sxs-lookup"><span data-stu-id="efd4b-188">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="efd4b-189">`Assigned`プロパティに設定されて`true`コースの講師が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-189">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="efd4b-190">ビューは、このプロパティを使用して、どのチェック ボックスを選択済みとして表示する必要があるかを判断します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-190">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="efd4b-191">最後に、一覧がビューに渡される、`ViewBag`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-191">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="efd4b-192">次に、ユーザーが **[Save]\(保存\)** をクリックしたときに実行されるコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-192">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="efd4b-193">置換、 `HttpPost` `Edit`メソッドを次のコードは、更新プログラムの新しいメソッドを呼び出し、`Courses`のナビゲーション プロパティ、`Instructor`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-193">Replace the `HttpPost` `Edit` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="efd4b-194">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-194">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

<span data-ttu-id="efd4b-195">ビューのコレクションを持たないため`Course`エンティティ、モデル バインダーを更新できません自動的に、`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-195">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="efd4b-196">モデル バインダーを使用して、コースのナビゲーション プロパティを更新するではなく行います新しい`UpdateInstructorCourses`メソッド。</span><span class="sxs-lookup"><span data-stu-id="efd4b-196">Instead of using the model binder to update the Courses navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="efd4b-197">そのため、モデル バインドから `Courses` プロパティを除外する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-197">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="efd4b-198">これを呼び出すコードの変更が必要としない[tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト登録*オーバー ロードと`Courses`インクルード一覧に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="efd4b-198">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="efd4b-199">場合、ボックスが選択されていないチェック、コードでは、`UpdateInstructorCourses`を初期化します、`Courses`空のコレクションでのナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-199">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="efd4b-200">その後コードは、データベース内のすべてのコースをループ処理し、各コースを現在インストラクターに割り当てられているコースとビューで選択されているコースを比較してチェックします。</span><span class="sxs-lookup"><span data-stu-id="efd4b-200">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="efd4b-201">検索を効率化するため、最後の 2 つのコレクションが `HashSet` オブジェクトに格納されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-201">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="efd4b-202">コースのチェック ボックスが選択されたが、そのコースが `Instructor.Courses`ナビゲーション プロパティにない場合、そのコースがナビゲーション プロパティ内のコレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-202">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

<span data-ttu-id="efd4b-203">コースのチェック ボックスが選択さていないが、そのコースが `Instructor.Courses`ナビゲーション プロパティにある場合、そのコースがナビゲーション プロパティから削除されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-203">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="efd4b-204">*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを強調表示されている、次を追加することで配列をコードの直後に、`div`の要素、 `OfficeAssignment`フィールド:</span><span class="sxs-lookup"><span data-stu-id="efd4b-204">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following highlighted code immediately after the `div` elements for the `OfficeAssignment` field:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

<span data-ttu-id="efd4b-205">このコードは、3 つの列を含む HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-205">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="efd4b-206">各列には、チェック ボックスとその後に続くキャプションがあります。キャプションは、コース番号とタイトルから構成されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-206">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="efd4b-207">チェック ボックスはすべて、同じ名前 ("selectedCourses") は、グループとして扱う場合にモデル バインダーに通知があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-207">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="efd4b-208">`value`各チェック ボックスの属性の値に設定されて`CourseID.`で構成されるコント ローラーにモデル バインダーが配列を渡しますページが投稿されたときに、`CourseID`チェック ボックスが選択されている値。</span><span class="sxs-lookup"><span data-stu-id="efd4b-208">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="efd4b-209">インストラクターに割り当てられるコースのチェック ボックスが最初に表示されると、`checked`属性は、(オンになった状態を表示します) それらを選択します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-209">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="efd4b-210">コースの割り当てを変更した後に、サイトが返されるときに、変更を確認できる必要あります、`Index`ページ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-210">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="efd4b-211">そのため、そのページ内のテーブルに列を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-211">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="efd4b-212">ここで使用する必要はありません、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてページに渡しているエンティティ。</span><span class="sxs-lookup"><span data-stu-id="efd4b-212">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="efd4b-213">*Views\Instructor\Index.cshtml*、追加、**コース**直後に、次の見出し、 **Office**見出しで、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="efd4b-213">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

<span data-ttu-id="efd4b-214">オフィスの場所の詳細セルの直後に続く新しい詳細セルを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-214">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

<span data-ttu-id="efd4b-215">実行、 **Instructor インデックス**ページに各インストラクターに割り当てられているコースをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="efd4b-215">Run the **Instructor Index** page to see the courses assigned to each instructor:</span></span>

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="efd4b-217">クリックして**編集**の講師が Edit ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd4b-217">Click **Edit** on an instructor to see the Edit page.</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="efd4b-219">一部のコース割り当てを変更し、クリックして**保存**します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-219">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="efd4b-220">行った変更が Index ページに反映されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-220">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="efd4b-221">注: インストラクター コース データを編集するには、コースの数に制限がある場合にも動作します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-221">Note: The approach taken to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="efd4b-222">非常に大きいコレクションの場合、別の UI と別の更新方法が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="efd4b-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>  
 

## <a name="update-the-delete-method"></a><span data-ttu-id="efd4b-223">Update、Delete メソッド</span><span class="sxs-lookup"><span data-stu-id="efd4b-223">Update the Delete Method</span></span>

<span data-ttu-id="efd4b-224">インストラクターが削除されたときに (ある場合) は、office の割り当てのレコードが削除されるので、HttpPost Delete メソッドのコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-224">Change the code in the HttpPost Delete method so the office assignment record (if any) is deleted when the instructor is deleted:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


<span data-ttu-id="efd4b-225">管理者として、学科に割り当てられている講師を削除しようとすると、参照整合性エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-225">If you try to delete an instructor who is assigned to a department as administrator, you'll get a referential integrity error.</span></span> <span data-ttu-id="efd4b-226">参照してください[このチュートリアルの現在のバージョン](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)追加のコードを instructor を管理者として、インストラクターが割り当てられている任意の部門から自動的に削除されます。</span><span class="sxs-lookup"><span data-stu-id="efd4b-226">See [the current version of this tutorial](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) for additional code that will automatically remove the instructor from any department where the instructor is assigned as an administrator.</span></span>

## <a name="summary"></a><span data-ttu-id="efd4b-227">まとめ</span><span class="sxs-lookup"><span data-stu-id="efd4b-227">Summary</span></span>

<span data-ttu-id="efd4b-228">この概要に関連するデータの操作が完了しました。</span><span class="sxs-lookup"><span data-stu-id="efd4b-228">You have now completed this introduction to working with related data.</span></span> <span data-ttu-id="efd4b-229">これまでにこれらのチュートリアルでは、完全な範囲の CRUD 操作を実行したが、同時実行の問題に対処していません。</span><span class="sxs-lookup"><span data-stu-id="efd4b-229">So far in these tutorials you've done a full range of CRUD operations, but you haven't dealt with concurrency issues.</span></span> <span data-ttu-id="efd4b-230">次のチュートリアルは同時実行のトピックを紹介、それを処理するためのオプションについて説明し、同時実行処理を 1 つのエンティティの種類について既に記述した CRUD コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-230">The next tutorial will introduce the topic of concurrency, explain options for handling it, and add concurrency handling to the CRUD code you've already written for one entity type.</span></span>

<span data-ttu-id="efd4b-231">最後に、その他の Entity Framework リソースへのリンクが見つかります[このシリーズの最終チュートリアル](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="efd4b-231">Links to other Entity Framework resources, can be found at the end of [the last tutorial in this series](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="efd4b-232">[前へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="efd4b-232">[Previous](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
