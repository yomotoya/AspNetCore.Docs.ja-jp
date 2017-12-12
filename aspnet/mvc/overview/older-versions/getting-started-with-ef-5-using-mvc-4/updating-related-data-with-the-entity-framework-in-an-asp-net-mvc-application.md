---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (10 の 6) で、Entity Framework と関連するデータの更新 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f2d480793d02c8bfa25c05fd11fa2e6ef9e54a60
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a><span data-ttu-id="dc28a-103">ASP.NET MVC アプリケーション (10 の 6) で、Entity Framework と関連するデータの更新</span><span class="sxs-lookup"><span data-stu-id="dc28a-103">Updating Related Data with the Entity Framework in an ASP.NET MVC Application (6 of 10)</span></span>
====================
<span data-ttu-id="dc28a-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="dc28a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="dc28a-105">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="dc28a-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="dc28a-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="dc28a-108">一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="dc28a-109">解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="dc28a-110">一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="dc28a-111">一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="dc28a-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="dc28a-112">前のチュートリアルでは、関連するデータが表示されます。このチュートリアルでは関連するデータを更新します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-112">In the previous tutorial you displayed related data; in this tutorial you'll update related data.</span></span> <span data-ttu-id="dc28a-113">ほとんどのリレーションシップするこれを適切な外部キー フィールドを更新します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-113">For most relationships, this can be done by updating the appropriate foreign key fields.</span></span> <span data-ttu-id="dc28a-114">多対多リレーションシップの場合、Entity Framework は開始されません結合テーブルを直接ように明示的に追加し、適切なナビゲーション プロパティからエンティティを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc28a-114">For many-to-many relationships, the Entity Framework doesn't expose the join table directly, so you must explicitly add and remove entities to and from the appropriate navigation properties.</span></span>

<span data-ttu-id="dc28a-115">次の図で作業をしているページを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-115">The following illustrations show the pages that you'll work with.</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="dc28a-118">コースを作成および編集ページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-118">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="dc28a-119">コースの新しいエンティティが作成されると、既存の部署とのリレーションシップが必要です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-119">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="dc28a-120">この作業を容易にスキャフォールディング コードには、コント ローラーのメソッドおよび部門を選択するためのドロップダウン リストを含むビューを作成および編集が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dc28a-120">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="dc28a-121">ドロップダウン リストを設定、`Course.DepartmentID`外部キーのプロパティを読み込むために、Entity Framework が必要なすべてが、`Department`ナビゲーション プロパティが、適切な`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-121">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate `Department` entity.</span></span> <span data-ttu-id="dc28a-122">スキャフォールディングのコードを使用することが、エラー処理を追加し、ドロップダウン リストを並べ替えるには少し変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-122">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="dc28a-123">*CourseController.cs*、4 つの削除`Edit`と`Create`メソッドに次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-123">In *CourseController.cs*, delete the four `Edit` and `Create` methods and replace them with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

<span data-ttu-id="dc28a-124">`PopulateDepartmentsDropDownList`メソッド名でソートのすべての部門の一覧を取得、作成、`SelectList`ドロップダウン一覧は、コレクション内のビューに、コレクションを渡すと、`ViewBag`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-124">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in a `ViewBag` property.</span></span> <span data-ttu-id="dc28a-125">このメソッドは、省略可能な受け取ります`selectedDepartment`パラメーターをドロップダウン リストが表示される場合に選択される項目を指定する呼び出し元のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-125">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="dc28a-126">ビューが名前を渡す`DepartmentID`に[、`DropDownList`ヘルパー](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)、ヘルパーは、ファイルの場所を認識しています、`ViewBag`オブジェクトに対して、`SelectList`という名前`DepartmentID`です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-126">The view will pass the name `DepartmentID` to [the `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), and the helper then knows to look in the `ViewBag` object for a `SelectList` named `DepartmentID`.</span></span>

<span data-ttu-id="dc28a-127">`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択したアイテムを設定しなくてもメソッド。</span><span class="sxs-lookup"><span data-stu-id="dc28a-127">The `HttpGet` `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="dc28a-128">`HttpGet` `Edit`メソッドは編集されているコースに既に割り当てられている部門の ID に基づいて、選択したアイテムを設定します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-128">The `HttpGet` `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="dc28a-129">`HttpPost`両方のメソッド`Create`と`Edit`もエラーが発生したページを再表示するときに、選択した項目を設定するコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-129">The `HttpPost` methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="dc28a-130">このコードにより、いるエラー メッセージを表示する、ページが表示され、ときにどのような部門が選択されている選択されたままとします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-130">This code ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

<span data-ttu-id="dc28a-131">*Views\Course\Create.cshtml*の強調表示されたコードを追加する前に新しいコース番号フィールドを作成、**タイトル**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-131">In *Views\Course\Create.cshtml*, add the highlighted code to create a new course number field before the **Title** field.</span></span> <span data-ttu-id="dc28a-132">主キー フィールドは既定では、スキャフォールディングされた前述の以前のチュートリアルがこの主キー、意味のあるユーザーが、キーの値を入力できるようにするためです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-132">As explained in an earlier tutorial, primary key fields aren't scaffolded by default, but this primary key is meaningful, so you want the user to be able to enter the key value.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

<span data-ttu-id="dc28a-133">*Views\Course\Edit.cshtml*、 *Views\Course\Delete.cshtml*、および*Views\Course\Details.cshtml*、コースの前に数値フィールドを追加、**タイトル**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-133">In *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, and *Views\Course\Details.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="dc28a-134">主キーであるためにが表示されますが、変更することはできません。</span><span class="sxs-lookup"><span data-stu-id="dc28a-134">Because it's the primary key, it's displayed, but it can't be changed.</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="dc28a-135">実行、**作成**ページ (コース インデックス ページを表示し、をクリックして**新規作成**) と新しいコースのデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-135">Run the **Create** page (display the Course Index page and click **Create New**) and enter data for a new course:</span></span>

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="dc28a-137">
              **[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-137">Click **Create**.</span></span> <span data-ttu-id="dc28a-138">一覧に追加された新しいコース コース インデックス ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-138">The Course Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="dc28a-139">インデックス ページの一覧で、部門名を示すリレーションシップが正常に確立されているナビゲーション プロパティに由来します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-139">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="dc28a-141">実行、**編集**ページ (コース インデックス ページを表示し、をクリックして**編集**コースで)。</span><span class="sxs-lookup"><span data-stu-id="dc28a-141">Run the **Edit** page (display the Course Index page and click **Edit** on a course).</span></span>

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="dc28a-143">ページ上のデータを変更し、クリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-143">Change data on the page and click **Save**.</span></span> <span data-ttu-id="dc28a-144">コース インデックス ページには、更新された一連のデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-144">The Course Index page is displayed with the updated course data.</span></span>

## <a name="adding-an-edit-page-for-instructors"></a><span data-ttu-id="dc28a-145">講習においてインストラクター編集ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-145">Adding an Edit Page for Instructors</span></span>

<span data-ttu-id="dc28a-146">講師レコードを編集するときに、インストラクター オフィス割り当てを更新することにします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-146">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="dc28a-147">`Instructor`エンティティと 0 または 1 を 1 つの関係には、`OfficeAssignment`エンティティで、次の状況を処理しなければならないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-147">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity, which means you must handle the following situations:</span></span>

- <span data-ttu-id="dc28a-148">ユーザーがオフィス割り当てをクリアし、値が最初にあった場合、は、解除し、削除する必要があります、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-148">If the user clears the office assignment and it originally had a value, you must remove and delete the `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="dc28a-149">新規に作成する必要があります office 代入値を入力すると、空か最初、`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-149">If the user enters an office assignment value and it originally was empty, you must create a new `OfficeAssignment` entity.</span></span>
- <span data-ttu-id="dc28a-150">Office の代入式の値を変更すると場合、は、既存の値を変更する必要があります`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-150">If the user changes the value of an office assignment, you must change the value in an existing `OfficeAssignment` entity.</span></span>

<span data-ttu-id="dc28a-151">開いている*InstructorController.cs*見ると、 `HttpGet` `Edit`メソッド。</span><span class="sxs-lookup"><span data-stu-id="dc28a-151">Open *InstructorController.cs* and look at the `HttpGet` `Edit` method:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="dc28a-152">スキャフォールディング コードをここでは、する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dc28a-152">The scaffolded code here isn't what you want.</span></span> <span data-ttu-id="dc28a-153">データを設定するには、ドロップダウン リストが必要なものは、テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="dc28a-153">It's setting up data for a drop-down list, but you what you need is a text box.</span></span> <span data-ttu-id="dc28a-154">このメソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-154">Replace this method with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="dc28a-155">このコードは、`ViewBag`ステートメント、関連付けられているため、一括読み込みを追加および`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-155">This code drops the `ViewBag` statement and adds eager loading for the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="dc28a-156">一括読み込みを行うことはできません、`Find`メソッド、ため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-156">You can't perform eager loading with the `Find` method, so the `Where` and `Single` methods are used instead to select the instructor.</span></span>

<span data-ttu-id="dc28a-157">置換、 `HttpPost` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="dc28a-157">Replace the `HttpPost` `Edit` method with the following code.</span></span> <span data-ttu-id="dc28a-158">これは、割り当ての office 更新プログラムを処理します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-158">which handles office assignment updates:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="dc28a-159">コードは次のとおり</span><span class="sxs-lookup"><span data-stu-id="dc28a-159">The code does the following:</span></span>

- <span data-ttu-id="dc28a-160">現在の取得`Instructor`の一括読み込みを使用して、データベースからのエンティティ、`OfficeAssignment`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="dc28a-160">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="dc28a-161">これと同じで行った、 `HttpGet` `Edit`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-161">This is the same as what you did in the `HttpGet` `Edit` method.</span></span>
- <span data-ttu-id="dc28a-162">更新、取得した`Instructor`モデル バインダーから値を持つエンティティ。</span><span class="sxs-lookup"><span data-stu-id="dc28a-162">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="dc28a-163">[TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.108).aspx)使用オーバー ロードを使用する*ホワイト リスト*に含めるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-163">The [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.108).aspx) overload used enables you to *whitelist* the properties you want to include.</span></span> <span data-ttu-id="dc28a-164">こうすれば、過剰な投稿で説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-164">This prevents over-posting, as explained in [the second tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- <span data-ttu-id="dc28a-165">オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null にできるように、関連する行で、`OfficeAssignment`テーブルは削除されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-165">If the office location is blank, sets the `Instructor.OfficeAssignment` property to null so that the related row in the `OfficeAssignment` table will be deleted.</span></span>

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- <span data-ttu-id="dc28a-166">データベースに変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-166">Saves the changes to the database.</span></span>

<span data-ttu-id="dc28a-167">*Views\Instructor\Edit.cshtml*の後に、`div`用の要素、 **Hire Date**フィールドに、オフィスの場所を編集するための新しいフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-167">In *Views\Instructor\Edit.cshtml*, after the `div` elements for the **Hire Date** field, add a new field for editing the office location:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

<span data-ttu-id="dc28a-168">ページの実行 (選択、**講習においてインストラクター**  タブをクリックして**編集**インストラクターに)。</span><span class="sxs-lookup"><span data-stu-id="dc28a-168">Run the page (select the **Instructors** tab and then click **Edit** on an instructor).</span></span> <span data-ttu-id="dc28a-169">変更、**オフィス** をクリック**保存**です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-169">Change the **Office Location** and click **Save**.</span></span>

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="dc28a-171">[編集] ページをインストラクターにコースの課題を追加します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-171">Adding Course Assignments to the Instructor Edit Page</span></span>

<span data-ttu-id="dc28a-172">講習においてインストラクターには、コースの任意の数を教えることがあります。</span><span class="sxs-lookup"><span data-stu-id="dc28a-172">Instructors may teach any number of courses.</span></span> <span data-ttu-id="dc28a-173">次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、course 割り当てを変更する機能を追加することでインストラクターの編集 ページを拡張します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-173">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="dc28a-175">間のリレーションシップ、`Course`と`Instructor`エンティティは多対多の結合テーブルに直接アクセスする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dc28a-175">The relationship between the `Course` and `Instructor` entities is many-to-many, which means you do not have direct access to the join table.</span></span> <span data-ttu-id="dc28a-176">追加する代わりに、およびとの間にエンティティを削除するか、`Instructor.Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="dc28a-176">Instead, you will add and remove entities to and from the `Instructor.Courses` navigation property.</span></span>

<span data-ttu-id="dc28a-177">UI コースを変更することができるインストラクターに割り当てられているチェック ボックスのグループです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-177">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="dc28a-178">データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているものを選択します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-178">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="dc28a-179">ユーザーでは、選択したり、コースの割り当てを変更する チェック ボックスをオフにすることができます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-179">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="dc28a-180">コースの数が非常に大きいと場合、は、他のビューでは、データの表示方法を使用することはおそらくが作成またはリレーションシップを削除するためにナビゲーション プロパティを操作するための同じメソッドを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-180">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating navigation properties in order to create or delete relationships.</span></span>

<span data-ttu-id="dc28a-181">チェック ボックスの一覧については、ビューにデータを提供するには、ビュー モデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-181">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span> <span data-ttu-id="dc28a-182">作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-182">Create *AssignedCourseData.cs* in the *ViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="dc28a-183">*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="dc28a-183">In *InstructorController.cs*, replace the `HttpGet` `Edit` method with the following code.</span></span> <span data-ttu-id="dc28a-184">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-184">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

<span data-ttu-id="dc28a-185">コードでは、追加の一括読み込み、`Courses`ナビゲーション プロパティ、新しいを呼び出すと`PopulateAssignedCourseData` チェック ボックスを配列の使用に関する情報を提供するメソッドを`AssignedCourseData`モデル クラスを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-185">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="dc28a-186">内のコード、`PopulateAssignedCourseData`メソッドはすべて読み取り`Course`ビューを使用してコースの一覧を読み込むためにエンティティ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="dc28a-186">The code in the `PopulateAssignedCourseData` method reads through all `Course` entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="dc28a-187">コードが、インストラクターにコースが存在するかどうかをチェック各コースに`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="dc28a-187">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="dc28a-188">コースをインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/en-us/library/bb359438.aspx)コレクション。</span><span class="sxs-lookup"><span data-stu-id="dc28a-188">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a [HashSet](https://msdn.microsoft.com/en-us/library/bb359438.aspx) collection.</span></span> <span data-ttu-id="dc28a-189">`Assigned`プロパティに設定されている`true`コース講師が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-189">The `Assigned` property is set to `true` for courses the instructor is assigned.</span></span> <span data-ttu-id="dc28a-190">ビューは、このプロパティを使用して、どのチェック ボックスとして表示する選択を確認されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-190">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="dc28a-191">最後に、一覧は、ビューに渡される、`ViewBag`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-191">Finally, the list is passed to the view in a `ViewBag` property.</span></span>

<span data-ttu-id="dc28a-192">次に、ユーザーがクリックしたときに実行されるコードを追加**保存**です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-192">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="dc28a-193">置換、 `HttpPost` `Edit`メソッドを次のコードを更新する新しいメソッドを呼び出した、`Courses`のナビゲーション プロパティ、`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-193">Replace the `HttpPost` `Edit` method with the following code, which calls a new method that updates the `Courses` navigation property of the `Instructor` entity.</span></span> <span data-ttu-id="dc28a-194">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-194">The changes are highlighted.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

<span data-ttu-id="dc28a-195">ビューのコレクションがあるないため`Course`エンティティ、モデル バインダーは自動的に更新、`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="dc28a-195">Since the view doesn't have a collection of `Course` entities, the model binder can't automatically update the `Courses` navigation property.</span></span> <span data-ttu-id="dc28a-196">モデル バインダーを使用して、コースのナビゲーション プロパティを更新するの代わりにすることによって行います新しい`UpdateInstructorCourses`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-196">Instead of using the model binder to update the Courses navigation property, you'll do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="dc28a-197">除外する必要があるため、`Courses`モデル バインドからのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-197">Therefore you need to exclude the `Courses` property from model binding.</span></span> <span data-ttu-id="dc28a-198">呼び出すコードの変更は必要ありません[TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト*オーバー ロードと`Courses`は include リストではありません。</span><span class="sxs-lookup"><span data-stu-id="dc28a-198">This doesn't require any change to the code that calls [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.98).aspx) because you're using the *whitelisting* overload and `Courses` isn't in the include list.</span></span>

<span data-ttu-id="dc28a-199">場合チェックを行わないボックスを選択した場合、コードで`UpdateInstructorCourses`を初期化、`Courses`ナビゲーション プロパティが空のコレクション。</span><span class="sxs-lookup"><span data-stu-id="dc28a-199">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `Courses` navigation property with an empty collection:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="dc28a-200">コードは、データベース内のすべてのコースをループ処理し、ビューで選択されたものではなくインストラクターに現在割り当てられているものに対しては、各コースを確認します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-200">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="dc28a-201">効率的な検索を容易に後者の 2 つのコレクションが格納されている`HashSet`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="dc28a-201">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="dc28a-202">コースのチェック ボックスが選択されましたが、コースがにないかどうか、`Instructor.Courses`ナビゲーション プロパティ、コースはナビゲーション プロパティで、コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-202">If the check box for a course was selected but the course isn't in the `Instructor.Courses` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

<span data-ttu-id="dc28a-203">コースのチェック ボックスが選択されますが、コースは場合、`Instructor.Courses`コースのナビゲーション プロパティは、ナビゲーション プロパティから削除します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-203">If the check box for a course wasn't selected, but the course is in the `Instructor.Courses` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

<span data-ttu-id="dc28a-204">*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを強調表示されている、次を追加することで配列にコードの直後に、`div`用の要素、 `OfficeAssignment`フィールド:</span><span class="sxs-lookup"><span data-stu-id="dc28a-204">In *Views\Instructor\Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following highlighted code immediately after the `div` elements for the `OfficeAssignment` field:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

<span data-ttu-id="dc28a-205">このコードでは、次の 3 つの列を含んでいる HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-205">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="dc28a-206">各列ではコースの番号とタイトルから構成されるキャプションを続けて チェック ボックスです。</span><span class="sxs-lookup"><span data-stu-id="dc28a-206">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="dc28a-207">すべてのチェック ボックスをグループとして扱う場合にモデル バインダーを通知する同じ名前 ("selectedCourses") であります。</span><span class="sxs-lookup"><span data-stu-id="dc28a-207">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="dc28a-208">`value`各チェック ボックスの属性がの値に設定されている`CourseID.`モデル バインダーがコント ローラーで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスが選択されている値。</span><span class="sxs-lookup"><span data-stu-id="dc28a-208">The `value` attribute of each check box is set to the value of `CourseID.` When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="dc28a-209">適合インストラクターに割り当てられているコースのチェック ボックスが表示される最初と`checked`属性は、それらを (それらのチェックが表示される) を選択します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-209">When the check boxes are initially rendered, those that are for courses assigned to the instructor have `checked` attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="dc28a-210">コースの割り当てを変更した後にしておくに戻ると、サイトに変更を確認できる、`Index`ページ。</span><span class="sxs-lookup"><span data-stu-id="dc28a-210">After changing course assignments, you'll want to be able to verify the changes when the site returns to the `Index` page.</span></span> <span data-ttu-id="dc28a-211">そのため、そのページ内のテーブルに列を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc28a-211">Therefore, you need to add a column to the table in that page.</span></span> <span data-ttu-id="dc28a-212">使用する必要はありませんここでは、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてそのページへ渡そうとしているエンティティ。</span><span class="sxs-lookup"><span data-stu-id="dc28a-212">In this case you don't need to use the `ViewBag` object, because the information you want to display is already in the `Courses` navigation property of the `Instructor` entity that you're passing to the page as the model.</span></span>

<span data-ttu-id="dc28a-213">*Views\Instructor\Index.cshtml*、追加、**コース**見出しの直後、 **Office**見出しで、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="dc28a-213">In *Views\Instructor\Index.cshtml*, add a **Courses** heading immediately following the **Office** heading, as shown in the following example:</span></span>

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

<span data-ttu-id="dc28a-214">オフィスの場所の詳細セルに引き続いてすぐに新しい詳細セルを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-214">Then add a new detail cell immediately following the office location detail cell:</span></span>

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

<span data-ttu-id="dc28a-215">実行、**インストラクター インデックス**ページを参照する各インストラクターに割り当てられているコース。</span><span class="sxs-lookup"><span data-stu-id="dc28a-215">Run the **Instructor Index** page to see the courses assigned to each instructor:</span></span>

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="dc28a-217">をクリックして**編集**の編集 ページを表示するには、あるインストラクターにします。</span><span class="sxs-lookup"><span data-stu-id="dc28a-217">Click **Edit** on an instructor to see the Edit page.</span></span>

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="dc28a-219">一部のコース割り当てを変更し、クリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-219">Change some course assignments and click **Save**.</span></span> <span data-ttu-id="dc28a-220">インデックス ページには、加えた変更が反映されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-220">The changes you make are reflected on the Index page.</span></span>

 <span data-ttu-id="dc28a-221">注: インストラクター コースのデータを編集するには、コースの数に制限がある場合にも動作します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-221">Note: The approach taken to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="dc28a-222">非常に大きいコレクションを別の UI と更新の別の方法が必要になります。</span><span class="sxs-lookup"><span data-stu-id="dc28a-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>  
 

## <a name="update-the-delete-method"></a><span data-ttu-id="dc28a-223">Update、Delete メソッド</span><span class="sxs-lookup"><span data-stu-id="dc28a-223">Update the Delete Method</span></span>

<span data-ttu-id="dc28a-224">講師が削除されたとき (存在する場合) は、office の割り当てのレコードが削除、HttpPost 削除メソッドのコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-224">Change the code in the HttpPost Delete method so the office assignment record (if any) is deleted when the instructor is deleted:</span></span>

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


<span data-ttu-id="dc28a-225">管理者としての部門に割り当てられているインストラクターを削除しようとすると、参照整合性エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-225">If you try to delete an instructor who is assigned to a department as administrator, you'll get a referential integrity error.</span></span> <span data-ttu-id="dc28a-226">参照してください[このチュートリアルの現在のバージョン](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)追加のコードを管理者としてインストラクターが割り当てられているその他の部門からインストラクターを自動的に削除されます。</span><span class="sxs-lookup"><span data-stu-id="dc28a-226">See [the current version of this tutorial](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) for additional code that will automatically remove the instructor from any department where the instructor is assigned as an administrator.</span></span>

## <a name="summary"></a><span data-ttu-id="dc28a-227">概要</span><span class="sxs-lookup"><span data-stu-id="dc28a-227">Summary</span></span>

<span data-ttu-id="dc28a-228">この概要に関連するデータの操作が完了しました。</span><span class="sxs-lookup"><span data-stu-id="dc28a-228">You have now completed this introduction to working with related data.</span></span> <span data-ttu-id="dc28a-229">これまでにこれらのチュートリアルでは、全範囲の CRUD 操作を実行したが、同時実行の問題に対処していません。</span><span class="sxs-lookup"><span data-stu-id="dc28a-229">So far in these tutorials you've done a full range of CRUD operations, but you haven't dealt with concurrency issues.</span></span> <span data-ttu-id="dc28a-230">次のチュートリアルは同時実行のトピックを紹介、それを処理するためのオプションについて説明し、同時処理を 1 つのエンティティの種類について記述した CRUD コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc28a-230">The next tutorial will introduce the topic of concurrency, explain options for handling it, and add concurrency handling to the CRUD code you've already written for one entity type.</span></span>

<span data-ttu-id="dc28a-231">最後に他の Entity Framework リソースへのリンクが見つかりません[このシリーズの前回のチュートリアル](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="dc28a-231">Links to other Entity Framework resources, can be found at the end of [the last tutorial in this series](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dc28a-232">[前へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="dc28a-232">[Previous](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
