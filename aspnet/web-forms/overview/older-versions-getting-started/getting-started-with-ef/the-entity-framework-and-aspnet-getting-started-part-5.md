---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 5 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 0d0ae8385af96cf5e5951ac85f5cf6ae3c201a4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366677"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a><span data-ttu-id="4204b-104">Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 5</span><span class="sxs-lookup"><span data-stu-id="4204b-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 5</span></span>
====================
<span data-ttu-id="4204b-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4204b-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="4204b-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4204b-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="4204b-107">チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="4204b-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="working-with-related-data-continued"></a><span data-ttu-id="4204b-108">続き関連データを扱う</span><span class="sxs-lookup"><span data-stu-id="4204b-108">Working with Related Data, Continued</span></span>

<span data-ttu-id="4204b-109">使用を開始し、前のチュートリアルで、`EntityDataSource`関連データを操作するコントロール。</span><span class="sxs-lookup"><span data-stu-id="4204b-109">In the previous tutorial you began to use the `EntityDataSource` control to work with related data.</span></span> <span data-ttu-id="4204b-110">階層の複数のレベルを表示し、ナビゲーション プロパティのデータを編集します。</span><span class="sxs-lookup"><span data-stu-id="4204b-110">You displayed multiple levels of hierarchy and edited data in navigation properties.</span></span> <span data-ttu-id="4204b-111">このチュートリアルで追加してリレーションシップを削除して、既存のエンティティの関係がある新しいエンティティを追加することで、関連するデータを操作する進みます。</span><span class="sxs-lookup"><span data-stu-id="4204b-111">In this tutorial you'll continue to work with related data by adding and deleting relationships and by adding a new entity that has a relationship to an existing entity.</span></span>

<span data-ttu-id="4204b-112">学科に割り当てられているコースを追加するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="4204b-112">You'll create a page that adds courses that are assigned to departments.</span></span> <span data-ttu-id="4204b-113">各部署が既に存在して、新しいコースを作成するときに同時が関係を確立して、既存の部門間。</span><span class="sxs-lookup"><span data-stu-id="4204b-113">The departments already exist, and when you create a new course, at the same time you'll establish a relationship between it and an existing department.</span></span>

<span data-ttu-id="4204b-114">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4204b-114">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)</span></span>

<span data-ttu-id="4204b-115">(選択した 2 つのエンティティ間のリレーションシップの追加) コースにインストラクターを割り当てることで、多対多リレーションシップで動作するページを作成することもありますまたはコースの講師を削除する (2 つのエンティティ間のリレーションシップを削除します。オンにします。</span><span class="sxs-lookup"><span data-stu-id="4204b-115">You'll also create a page that works with a many-to-many relationship by assigning an instructor to a course (adding a relationship between two entities that you select) or removing an instructor from a course (removing a relationship between two entities that you select).</span></span> <span data-ttu-id="4204b-116">データベース内に追加される新しい行の結果、インストラクターとコースの間のリレーションシップの追加、`CourseInstructor`関連テーブル; からの行を削除するリレーションシップは、削除、`CourseInstructor`関連テーブル。</span><span class="sxs-lookup"><span data-stu-id="4204b-116">In the database, adding a relationship between an instructor and a course results in a new row being added to the `CourseInstructor` association table; removing a relationship involves deleting a row from the `CourseInstructor` association table.</span></span> <span data-ttu-id="4204b-117">ただし、これを行う Entity Framework で参照することがなくナビゲーション プロパティを設定して、`CourseInstructor`明示的にテーブルです。</span><span class="sxs-lookup"><span data-stu-id="4204b-117">However, you do this in the Entity Framework by setting navigation properties, without referring to the `CourseInstructor` table explicitly.</span></span>

<span data-ttu-id="4204b-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4204b-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)</span></span>

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a><span data-ttu-id="4204b-119">既存のエンティティにリレーションシップを持つエンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="4204b-119">Adding an Entity with a Relationship to an Existing Entity</span></span>

<span data-ttu-id="4204b-120">という名前の新しい web ページを作成する*CoursesAdd.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:</span><span class="sxs-lookup"><span data-stu-id="4204b-120">Create a new web page named *CoursesAdd.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

<span data-ttu-id="4204b-121">このマークアップを作成、`EntityDataSource`を挿入できるようにして、ハンドラーを指定する、コースの選択コントロール、`Inserting`イベント。</span><span class="sxs-lookup"><span data-stu-id="4204b-121">This markup creates an `EntityDataSource` control that selects courses, that enables inserting, and that specifies a handler for the `Inserting` event.</span></span> <span data-ttu-id="4204b-122">ハンドラーの更新に使用する、`Department`ナビゲーション プロパティの新しい`Course`エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="4204b-122">You'll use the handler to update the `Department` navigation property when a new `Course` entity is created.</span></span>

<span data-ttu-id="4204b-123">マークアップを作成も、`DetailsView`新規追加に使用するコントロール`Course`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-123">The markup also creates a `DetailsView` control to use for adding new `Course` entities.</span></span> <span data-ttu-id="4204b-124">マークアップにバインドされたフィールドを使用して`Course`エンティティのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-124">The markup uses bound fields for `Course` entity properties.</span></span> <span data-ttu-id="4204b-125">入力する必要がある、`CourseID`これがシステムによって生成された ID フィールドではないため、その値します。</span><span class="sxs-lookup"><span data-stu-id="4204b-125">You have to enter the `CourseID` value because this is not a system-generated ID field.</span></span> <span data-ttu-id="4204b-126">代わりに、コースの作成時に手動で指定する必要がありますコース番号になります。</span><span class="sxs-lookup"><span data-stu-id="4204b-126">Instead, it's a course number that must be specified manually when the course is created.</span></span>

<span data-ttu-id="4204b-127">テンプレート フィールドを使用する、`Department`ナビゲーション プロパティとナビゲーション プロパティを使用できないため`BoundField`コントロール。</span><span class="sxs-lookup"><span data-stu-id="4204b-127">You use a template field for the `Department` navigation property because navigation properties cannot be used with `BoundField` controls.</span></span> <span data-ttu-id="4204b-128">[テンプレート] フィールドでは、部門を選択するドロップダウン リストを提供します。</span><span class="sxs-lookup"><span data-stu-id="4204b-128">The template field provides a drop-down list to select the department.</span></span> <span data-ttu-id="4204b-129">ドロップダウン リストにバインドされた、`Departments`エンティティ セットを使用して`Eval`なく`Bind`、もう一度ため、それらを更新するにはナビゲーション プロパティを直接バインドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="4204b-129">The drop-down list is bound to the `Departments` entity set by using `Eval` rather than `Bind`, again because you cannot directly bind navigation properties in order to update them.</span></span> <span data-ttu-id="4204b-130">ハンドラーを指定する、`DropDownList`コントロールの`Init`イベントを更新するコードで使用するためのコントロールへの参照を保存するため、`DepartmentID`外部キーです。</span><span class="sxs-lookup"><span data-stu-id="4204b-130">You specify a handler for the `DropDownList` control's `Init` event so that you can store a reference to the control for use by the code that updates the `DepartmentID` foreign key.</span></span>

<span data-ttu-id="4204b-131">*CoursesAdd.aspx.cs*への参照を保持するためにクラス フィールドを追加、部分クラス宣言の直後後、`DepartmentsDropDownList`コントロール。</span><span class="sxs-lookup"><span data-stu-id="4204b-131">In *CoursesAdd.aspx.cs* just after the partial-class declaration, add a class field to hold a reference to the `DepartmentsDropDownList` control:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

<span data-ttu-id="4204b-132">ハンドラーを追加、`DepartmentsDropDownList`コントロールの`Init`イベントのため、コントロールへの参照を格納することです。</span><span class="sxs-lookup"><span data-stu-id="4204b-132">Add a handler for the `DepartmentsDropDownList` control's `Init` event so that you can store a reference to the control.</span></span> <span data-ttu-id="4204b-133">これにより、ユーザーが入力した値を取得し、更新に使用できます、`DepartmentID`の値、`Course`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-133">This lets you get the value the user has entered and use it to update the `DepartmentID` value of the `Course` entity.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

<span data-ttu-id="4204b-134">ハンドラーを追加、`DetailsView`コントロールの`Inserting`イベント。</span><span class="sxs-lookup"><span data-stu-id="4204b-134">Add a handler for the `DetailsView` control's `Inserting` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

<span data-ttu-id="4204b-135">ユーザーがクリックすると`Insert`、`Inserting`新しいレコードが挿入される前に、イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="4204b-135">When the user clicks `Insert`, the `Inserting` event is raised before the new record is inserted.</span></span> <span data-ttu-id="4204b-136">ハンドラーのコードを取得、`DepartmentID`から、`DropDownList`を制御し、使用される値を設定するため、`DepartmentID`のプロパティ、`Course`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-136">The code in the handler gets the `DepartmentID` from the `DropDownList` control and uses it to set the value that will be used for the `DepartmentID` property of the `Course` entity.</span></span>

<span data-ttu-id="4204b-137">Entity Framework が処理するには、このコースを追加する、`Courses`ナビゲーション プロパティ、関連付けられている`Department`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-137">The Entity Framework will take care of adding this course to the `Courses` navigation property of the associated `Department` entity.</span></span> <span data-ttu-id="4204b-138">部署をさらに追加、`Department`のナビゲーション プロパティ、`Course`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-138">It also adds the department to the `Department` navigation property of the `Course` entity.</span></span>

<span data-ttu-id="4204b-139">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="4204b-139">Run the page.</span></span>

<span data-ttu-id="4204b-140">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4204b-140">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)</span></span>

<span data-ttu-id="4204b-141">ID、タイトル、クレジットの数値を入力し、部署を選択します をクリックして**挿入**します。</span><span class="sxs-lookup"><span data-stu-id="4204b-141">Enter an ID, a title, a number of credits, and select a department, then click **Insert**.</span></span>

<span data-ttu-id="4204b-142">実行、 *Courses.aspx*ページ、および新しいコースを表示する同じ部門を選択します。</span><span class="sxs-lookup"><span data-stu-id="4204b-142">Run the *Courses.aspx* page, and select the same department to see the new course.</span></span>

<span data-ttu-id="4204b-143">[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4204b-143">[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)</span></span>

## <a name="working-with-many-to-many-relationships"></a><span data-ttu-id="4204b-144">多対多リレーションシップの使用</span><span class="sxs-lookup"><span data-stu-id="4204b-144">Working with Many-to-Many Relationships</span></span>

<span data-ttu-id="4204b-145">間のリレーションシップ、`Courses`エンティティ セットと`People`エンティティ セットは、多対多リレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="4204b-145">The relationship between the `Courses` entity set and the `People` entity set is a many-to-many relationship.</span></span> <span data-ttu-id="4204b-146">A`Course`エンティティという名前のナビゲーション プロパティには`People`0、1、またはその関連する詳細を格納できる`Person`エンティティ (そのコースを教えるに割り当てられている講師を表す)。</span><span class="sxs-lookup"><span data-stu-id="4204b-146">A `Course` entity has a navigation property named `People` that can contain zero, one, or more related `Person` entities (representing instructors assigned to teach that course).</span></span> <span data-ttu-id="4204b-147">および`Person`エンティティという名前のナビゲーション プロパティには`Courses`0、1、またはその関連する詳細を含めることができます`Course`エンティティ (コースを表すについて解説するそのインストラクターが割り当てられます)。</span><span class="sxs-lookup"><span data-stu-id="4204b-147">And a `Person` entity has a navigation property named `Courses` that can contain zero, one, or more related `Course` entities (representing courses that instructor is assigned to teach).</span></span> <span data-ttu-id="4204b-148">1 人の講師は、複数のコースを学ぶことし、複数の講師が 1 つのコースを担当する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4204b-148">One instructor might teach multiple courses, and one course might be taught by multiple instructors.</span></span> <span data-ttu-id="4204b-149">チュートリアルのこのセクションで追加し、間のリレーションシップを削除`Person`と`Course`エンティティ関連エンティティのナビゲーション プロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="4204b-149">In this section of the walkthrough, you'll add and remove relationships between `Person` and `Course` entities by updating the navigation properties of the related entities.</span></span>

<span data-ttu-id="4204b-150">という名前の新しい web ページを作成する*InstructorsCourses.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:</span><span class="sxs-lookup"><span data-stu-id="4204b-150">Create a new web page named *InstructorsCourses.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

<span data-ttu-id="4204b-151">このマークアップを作成、 `EntityDataSource` 、名前を取得するコントロールと`PersonID`の`Person`インストラクター エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-151">This markup creates an `EntityDataSource` control that retrieves the name and `PersonID` of `Person` entities for instructors.</span></span> <span data-ttu-id="4204b-152">A`DropDrownList`コントロールにバインドする、`EntityDataSource`コントロール。</span><span class="sxs-lookup"><span data-stu-id="4204b-152">A `DropDrownList` control is bound to the `EntityDataSource` control.</span></span> <span data-ttu-id="4204b-153">`DropDownList`コントロールのハンドラーを指定する、`DataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="4204b-153">The `DropDownList` control specifies a handler for the `DataBound` event.</span></span> <span data-ttu-id="4204b-154">このハンドラーのコースを表示するデータ バインド、2 つのドロップダウン リストを使用します。</span><span class="sxs-lookup"><span data-stu-id="4204b-154">You'll use this handler to databind the two drop-down lists that display courses.</span></span>

<span data-ttu-id="4204b-155">マークアップでは、選択したインストラクターにコースの割り当てに使用するコントロールの次のグループも作成されます。</span><span class="sxs-lookup"><span data-stu-id="4204b-155">The markup also creates the following group of controls to use for assigning a course to the selected instructor:</span></span>

- <span data-ttu-id="4204b-156">A`DropDownList`を割り当てるコースを選択するためのコントロール。</span><span class="sxs-lookup"><span data-stu-id="4204b-156">A `DropDownList` control for selecting a course to assign.</span></span> <span data-ttu-id="4204b-157">選択したインストラクターに現在割り当てられていないコースでは、このコントロールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4204b-157">This control will be populated with courses that are currently not assigned to the selected instructor.</span></span>
- <span data-ttu-id="4204b-158">A`Button`割り当てを開始するコントロール。</span><span class="sxs-lookup"><span data-stu-id="4204b-158">A `Button` control to initiate the assignment.</span></span>
- <span data-ttu-id="4204b-159">A`Label`割り当てが失敗した場合、エラー メッセージを表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="4204b-159">A `Label` control to display an error message if the assignment fails.</span></span>

<span data-ttu-id="4204b-160">最後に、マークアップでは、選択したインストラクターのコースを削除するために使用するコントロールのグループも作成します。</span><span class="sxs-lookup"><span data-stu-id="4204b-160">Finally, the markup also creates a group of controls to use for removing a course from the selected instructor.</span></span>

<span data-ttu-id="4204b-161">*InstructorsCourses.aspx.cs*を使用して、追加ステートメント。</span><span class="sxs-lookup"><span data-stu-id="4204b-161">In *InstructorsCourses.aspx.cs*, add a using statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

<span data-ttu-id="4204b-162">コースを表示する 2 つのドロップダウン リストを設定するためのメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="4204b-162">Add a method for populating the two drop-down lists that display courses:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

<span data-ttu-id="4204b-163">このコードからすべてのコースを取得する、`Courses`エンティティが設定されからコースを取得、`Courses`のナビゲーション プロパティ、`Person`選択したインストラクター エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-163">This code gets all courses from the `Courses` entity set and gets the courses from the `Courses` navigation property of the `Person` entity for the selected instructor.</span></span> <span data-ttu-id="4204b-164">コースは、そのインストラクターに割り当てられているかを決定し、ドロップダウン リストを適宜設定します。</span><span class="sxs-lookup"><span data-stu-id="4204b-164">It then determines which courses are assigned to that instructor and populates the drop-down lists accordingly.</span></span>

<span data-ttu-id="4204b-165">ハンドラーを追加、`Assign`ボタンの`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="4204b-165">Add a handler for the `Assign` button's `Click` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

<span data-ttu-id="4204b-166">このコードを取得、`Person`選択されたインストラクターのエンティティを取得、`Course`選択したコースのエンティティを選択したコースを追加します、`Courses`のインストラクターのナビゲーション プロパティ`Person`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-166">This code gets the `Person` entity for the selected instructor, gets the `Course` entity for the selected course, and adds the selected course to the `Courses` navigation property of the instructor's `Person` entity.</span></span> <span data-ttu-id="4204b-167">データベースに変更を保存し、結果をすぐに確認できるように、ドロップダウン リストを再作成します。</span><span class="sxs-lookup"><span data-stu-id="4204b-167">It then saves the changes to the database and repopulates the drop-down lists so the results can be seen immediately.</span></span>

<span data-ttu-id="4204b-168">ハンドラーを追加、`Remove`ボタンの`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="4204b-168">Add a handler for the `Remove` button's `Click` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

<span data-ttu-id="4204b-169">このコードを取得、`Person`選択されたインストラクターのエンティティを取得、`Course`選択したコースのエンティティから選択したコースを削除し、`Person`エンティティの`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4204b-169">This code gets the `Person` entity for the selected instructor, gets the `Course` entity for the selected course, and removes the selected course from the `Person` entity's `Courses` navigation property.</span></span> <span data-ttu-id="4204b-170">データベースに変更を保存し、結果をすぐに確認できるように、ドロップダウン リストを再作成します。</span><span class="sxs-lookup"><span data-stu-id="4204b-170">It then saves the changes to the database and repopulates the drop-down lists so the results can be seen immediately.</span></span>

<span data-ttu-id="4204b-171">コードを追加して、`Page_Load`ことを確認して、エラー メッセージは、メソッドはレポートと用のハンドラーを追加するとエラーがない場合に表示されない、`DataBound`と`SelectedIndexChanged`コースのドロップダウン リストを設定するインストラクターのドロップダウン リストのイベント。</span><span class="sxs-lookup"><span data-stu-id="4204b-171">Add code to the `Page_Load` method that makes sure the error messages are not visible when there's no error to report, and add handlers for the `DataBound` and `SelectedIndexChanged` events of the instructors drop-down list to populate the courses drop-down lists:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

<span data-ttu-id="4204b-172">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="4204b-172">Run the page.</span></span>

<span data-ttu-id="4204b-173">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="4204b-173">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)</span></span>

<span data-ttu-id="4204b-174">インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="4204b-174">Select an instructor.</span></span> <span data-ttu-id="4204b-175"><strong>コースを割り当てる</strong>、インストラクターが指導するコースを表示するドロップダウン リスト、<strong>コースを削除</strong>ドロップダウン リストに既に割り当てられている、インストラクター コースを表示します。</span><span class="sxs-lookup"><span data-stu-id="4204b-175">The <strong>Assign a Course</strong> drop-down list displays the courses that the instructor doesn't teach, and the <strong>Remove a Course</strong> drop-down list displays the courses that the instructor is already assigned to.</span></span> <span data-ttu-id="4204b-176"><strong>コースを割り当てる</strong>セクション、コースを選択し、クリックして<strong>割り当てる</strong>します。</span><span class="sxs-lookup"><span data-stu-id="4204b-176">In the <strong>Assign a Course</strong> section, select a course and then click <strong>Assign</strong>.</span></span> <span data-ttu-id="4204b-177">コースに移動、<strong>コースを削除</strong>ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="4204b-177">The course moves to the <strong>Remove a Course</strong> drop-down list.</span></span> <span data-ttu-id="4204b-178">コースを選択、<strong>コースを削除</strong>セクションし、をクリックして<strong>削除</strong><em>します。</em></span><span class="sxs-lookup"><span data-stu-id="4204b-178">Select a course in the <strong>Remove a Course</strong> section and click <strong>Remove</strong><em>.</em></span></span> <span data-ttu-id="4204b-179">コースに移動、<strong>コースを割り当てる</strong>ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="4204b-179">The course moves to the <strong>Assign a Course</strong> drop-down list.</span></span>

<span data-ttu-id="4204b-180">関連データを操作する方法がいくつか見てきました。</span><span class="sxs-lookup"><span data-stu-id="4204b-180">You have now seen some more ways to work with related data.</span></span> <span data-ttu-id="4204b-181">次のチュートリアルでは、アプリケーションの保守容易性を向上させるために、データ モデルで継承を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="4204b-181">In the following tutorial, you'll learn how to use inheritance in the data model to improve the maintainability of your application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4204b-182">[前へ](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="4204b-182">[Previous](the-entity-framework-and-aspnet-getting-started-part-4.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-6.md)</span></span>
