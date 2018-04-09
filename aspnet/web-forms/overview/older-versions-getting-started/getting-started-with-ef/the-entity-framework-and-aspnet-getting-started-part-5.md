---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 5 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7f351718123e881e832f4ac95af506ed601d3337
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a><span data-ttu-id="9e029-104">データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 5</span><span class="sxs-lookup"><span data-stu-id="9e029-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 5</span></span>
====================
<span data-ttu-id="9e029-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9e029-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="9e029-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9e029-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="9e029-107">一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="9e029-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="working-with-related-data-continued"></a><span data-ttu-id="9e029-108">関連するデータを扱う続き</span><span class="sxs-lookup"><span data-stu-id="9e029-108">Working with Related Data, Continued</span></span>

<span data-ttu-id="9e029-109">前のチュートリアルで使用を開始し、`EntityDataSource`関連データを操作するコントロール。</span><span class="sxs-lookup"><span data-stu-id="9e029-109">In the previous tutorial you began to use the `EntityDataSource` control to work with related data.</span></span> <span data-ttu-id="9e029-110">複数レベルの階層を表示し、ナビゲーション プロパティでデータを編集します。</span><span class="sxs-lookup"><span data-stu-id="9e029-110">You displayed multiple levels of hierarchy and edited data in navigation properties.</span></span> <span data-ttu-id="9e029-111">このチュートリアルで追加してリレーションシップを削除して、既存のエンティティの関係がある新しいエンティティを追加することで、関連するデータを操作に進みます。</span><span class="sxs-lookup"><span data-stu-id="9e029-111">In this tutorial you'll continue to work with related data by adding and deleting relationships and by adding a new entity that has a relationship to an existing entity.</span></span>

<span data-ttu-id="9e029-112">部門に割り当てられているコースを追加するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="9e029-112">You'll create a page that adds courses that are assigned to departments.</span></span> <span data-ttu-id="9e029-113">部門が既に存在して、新しいコースを作成するときに、同時にあります関係を確立すると、既存の部門とします。</span><span class="sxs-lookup"><span data-stu-id="9e029-113">The departments already exist, and when you create a new course, at the same time you'll establish a relationship between it and an existing department.</span></span>

<span data-ttu-id="9e029-114">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9e029-114">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)</span></span>

<span data-ttu-id="9e029-115">(選択した 2 つのエンティティ間のリレーションシップの追加) コースをインストラクターを割り当てることによって、多対多リレーションシップで動作するページを作成することもありますまたはコースの、インストラクターを削除する (2 つのエンティティ間のリレーションシップを削除します。オンにします。</span><span class="sxs-lookup"><span data-stu-id="9e029-115">You'll also create a page that works with a many-to-many relationship by assigning an instructor to a course (adding a relationship between two entities that you select) or removing an instructor from a course (removing a relationship between two entities that you select).</span></span> <span data-ttu-id="9e029-116">データベース内に追加される新しい行の結果、インストラクターとコースの間のリレーションシップの追加、`CourseInstructor`関連テーブル以外の場合は、リレーションシップから行を削除する削除、`CourseInstructor`関連テーブル。</span><span class="sxs-lookup"><span data-stu-id="9e029-116">In the database, adding a relationship between an instructor and a course results in a new row being added to the `CourseInstructor` association table; removing a relationship involves deleting a row from the `CourseInstructor` association table.</span></span> <span data-ttu-id="9e029-117">ただし、これを行う、Entity framework を参照しなくても、ナビゲーション プロパティの設定によって、`CourseInstructor`明示的にテーブルです。</span><span class="sxs-lookup"><span data-stu-id="9e029-117">However, you do this in the Entity Framework by setting navigation properties, without referring to the `CourseInstructor` table explicitly.</span></span>

<span data-ttu-id="9e029-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9e029-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)</span></span>

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a><span data-ttu-id="9e029-119">既存のエンティティへのリレーションシップを持つエンティティの追加</span><span class="sxs-lookup"><span data-stu-id="9e029-119">Adding an Entity with a Relationship to an Existing Entity</span></span>

<span data-ttu-id="9e029-120">という名前の新しい web ページを作成する*CoursesAdd.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:</span><span class="sxs-lookup"><span data-stu-id="9e029-120">Create a new web page named *CoursesAdd.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

<span data-ttu-id="9e029-121">このマークアップを作成、`EntityDataSource`コースを選択するの挿入を有効にするを指定するコントロールのハンドラーを`Inserting`イベント。</span><span class="sxs-lookup"><span data-stu-id="9e029-121">This markup creates an `EntityDataSource` control that selects courses, that enables inserting, and that specifies a handler for the `Inserting` event.</span></span> <span data-ttu-id="9e029-122">ハンドラーは、更新に使用する、`Department`ナビゲーション プロパティの新しい`Course`エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="9e029-122">You'll use the handler to update the `Department` navigation property when a new `Course` entity is created.</span></span>

<span data-ttu-id="9e029-123">またマークアップを作成、`DetailsView`新規追加に使用するコントロール`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-123">The markup also creates a `DetailsView` control to use for adding new `Course` entities.</span></span> <span data-ttu-id="9e029-124">マークアップのバインドされたフィールドを使用して`Course`エンティティのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-124">The markup uses bound fields for `Course` entity properties.</span></span> <span data-ttu-id="9e029-125">入力する必要がある、`CourseID`値のため、これはシステムによって生成された ID フィールドではありません。</span><span class="sxs-lookup"><span data-stu-id="9e029-125">You have to enter the `CourseID` value because this is not a system-generated ID field.</span></span> <span data-ttu-id="9e029-126">代わりに、これは、コースの作成時に手動で指定する必要がありますをコース番号です。</span><span class="sxs-lookup"><span data-stu-id="9e029-126">Instead, it's a course number that must be specified manually when the course is created.</span></span>

<span data-ttu-id="9e029-127">テンプレートのフィールドを使用する、`Department`ナビゲーション プロパティとナビゲーション プロパティを使用できないため`BoundField`コントロール。</span><span class="sxs-lookup"><span data-stu-id="9e029-127">You use a template field for the `Department` navigation property because navigation properties cannot be used with `BoundField` controls.</span></span> <span data-ttu-id="9e029-128">[テンプレート] フィールドでは、部門を選択するドロップダウン リストを提供します。</span><span class="sxs-lookup"><span data-stu-id="9e029-128">The template field provides a drop-down list to select the department.</span></span> <span data-ttu-id="9e029-129">ドロップダウン リストにバインドされて、`Departments`エンティティ セットを使用して`Eval`なく`Bind`、もう一度ため、それらを更新するためにナビゲーション プロパティを直接バインドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="9e029-129">The drop-down list is bound to the `Departments` entity set by using `Eval` rather than `Bind`, again because you cannot directly bind navigation properties in order to update them.</span></span> <span data-ttu-id="9e029-130">ハンドラーを指定する、`DropDownList`コントロールの`Init`イベントを更新するコードによって使用される、コントロールへの参照を保存すること、`DepartmentID`外部キーです。</span><span class="sxs-lookup"><span data-stu-id="9e029-130">You specify a handler for the `DropDownList` control's `Init` event so that you can store a reference to the control for use by the code that updates the `DepartmentID` foreign key.</span></span>

<span data-ttu-id="9e029-131">*CoursesAdd.aspx.cs*部分クラス宣言の直後にへの参照を保持するためにクラス フィールドの追加、`DepartmentsDropDownList`コントロール。</span><span class="sxs-lookup"><span data-stu-id="9e029-131">In *CoursesAdd.aspx.cs* just after the partial-class declaration, add a class field to hold a reference to the `DepartmentsDropDownList` control:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

<span data-ttu-id="9e029-132">ハンドラーを追加、`DepartmentsDropDownList`コントロールの`Init`イベント コントロールへの参照を保存することです。</span><span class="sxs-lookup"><span data-stu-id="9e029-132">Add a handler for the `DepartmentsDropDownList` control's `Init` event so that you can store a reference to the control.</span></span> <span data-ttu-id="9e029-133">これにより、ユーザーが入力した値を取得および更新に使用できます、`DepartmentID`の値、`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-133">This lets you get the value the user has entered and use it to update the `DepartmentID` value of the `Course` entity.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

<span data-ttu-id="9e029-134">ハンドラーを追加、`DetailsView`コントロールの`Inserting`イベント。</span><span class="sxs-lookup"><span data-stu-id="9e029-134">Add a handler for the `DetailsView` control's `Inserting` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

<span data-ttu-id="9e029-135">ユーザーがクリックしたとき`Insert`、`Inserting`イベントは、新しいレコードが挿入される前に発生します。</span><span class="sxs-lookup"><span data-stu-id="9e029-135">When the user clicks `Insert`, the `Inserting` event is raised before the new record is inserted.</span></span> <span data-ttu-id="9e029-136">ハンドラーのコードを取得、`DepartmentID`から、`DropDownList`制御に使用される値の設定を使用して、`DepartmentID`のプロパティ、`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-136">The code in the handler gets the `DepartmentID` from the `DropDownList` control and uses it to set the value that will be used for the `DepartmentID` property of the `Course` entity.</span></span>

<span data-ttu-id="9e029-137">Entity Framework に対処するには、このコースを追加する、`Courses`関連付けられているナビゲーション プロパティ`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-137">The Entity Framework will take care of adding this course to the `Courses` navigation property of the associated `Department` entity.</span></span> <span data-ttu-id="9e029-138">部門にも追加、`Department`のナビゲーション プロパティ、`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-138">It also adds the department to the `Department` navigation property of the `Course` entity.</span></span>

<span data-ttu-id="9e029-139">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="9e029-139">Run the page.</span></span>

<span data-ttu-id="9e029-140">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9e029-140">[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)</span></span>

<span data-ttu-id="9e029-141">ID、タイトル、クレジットの数を入力してください、部門を選択し、をクリックして**挿入**です。</span><span class="sxs-lookup"><span data-stu-id="9e029-141">Enter an ID, a title, a number of credits, and select a department, then click **Insert**.</span></span>

<span data-ttu-id="9e029-142">実行、 *Courses.aspx*ページ、および新しいコースを表示する同じ部門を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e029-142">Run the *Courses.aspx* page, and select the same department to see the new course.</span></span>

<span data-ttu-id="9e029-143">[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9e029-143">[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)</span></span>

## <a name="working-with-many-to-many-relationships"></a><span data-ttu-id="9e029-144">多対多リレーションシップの使用</span><span class="sxs-lookup"><span data-stu-id="9e029-144">Working with Many-to-Many Relationships</span></span>

<span data-ttu-id="9e029-145">間のリレーションシップ、`Courses`エンティティ セットと`People`エンティティ セットは、多対多のリレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="9e029-145">The relationship between the `Courses` entity set and the `People` entity set is a many-to-many relationship.</span></span> <span data-ttu-id="9e029-146">A`Course`エンティティがという名前のナビゲーション プロパティ`People`0、1、またはその関連する詳細を持つことができる`Person`エンティティ (そのコースを教えるに割り当てられている講習においてインストラクターを表す) です。</span><span class="sxs-lookup"><span data-stu-id="9e029-146">A `Course` entity has a navigation property named `People` that can contain zero, one, or more related `Person` entities (representing instructors assigned to teach that course).</span></span> <span data-ttu-id="9e029-147">および`Person`エンティティがという名前のナビゲーション プロパティ`Courses`0、1、またはその関連する詳細を含めることができます`Course`エンティティ (コースを表すの指導を求めましたその講師が割り当てられます)。</span><span class="sxs-lookup"><span data-stu-id="9e029-147">And a `Person` entity has a navigation property named `Courses` that can contain zero, one, or more related `Course` entities (representing courses that instructor is assigned to teach).</span></span> <span data-ttu-id="9e029-148">講師が 1 つは、複数のコースを教えることがあり、複数講習においてインストラクターによって 1 つのコースを教える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9e029-148">One instructor might teach multiple courses, and one course might be taught by multiple instructors.</span></span> <span data-ttu-id="9e029-149">チュートリアルのこのセクションに追加して間のリレーションシップを削除する`Person`と`Course`関連エンティティのナビゲーション プロパティを更新することでエンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-149">In this section of the walkthrough, you'll add and remove relationships between `Person` and `Course` entities by updating the navigation properties of the related entities.</span></span>

<span data-ttu-id="9e029-150">という名前の新しい web ページを作成する*InstructorsCourses.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:</span><span class="sxs-lookup"><span data-stu-id="9e029-150">Create a new web page named *InstructorsCourses.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

<span data-ttu-id="9e029-151">このマークアップを作成、`EntityDataSource`名前を取得するコントロールと`PersonID`の`Person`講習においてインストラクター エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-151">This markup creates an `EntityDataSource` control that retrieves the name and `PersonID` of `Person` entities for instructors.</span></span> <span data-ttu-id="9e029-152">A`DropDrownList`コントロールにバインドする、`EntityDataSource`コントロール。</span><span class="sxs-lookup"><span data-stu-id="9e029-152">A `DropDrownList` control is bound to the `EntityDataSource` control.</span></span> <span data-ttu-id="9e029-153">`DropDownList`コントロールのハンドラーを指定する、`DataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="9e029-153">The `DropDownList` control specifies a handler for the `DataBound` event.</span></span> <span data-ttu-id="9e029-154">このハンドラーのコースを表示するデータ バインド、2 つのドロップダウン リストを使用します。</span><span class="sxs-lookup"><span data-stu-id="9e029-154">You'll use this handler to databind the two drop-down lists that display courses.</span></span>

<span data-ttu-id="9e029-155">また、マークアップを選択した講師コースに割り当てるために使用するコントロールの次のグループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="9e029-155">The markup also creates the following group of controls to use for assigning a course to the selected instructor:</span></span>

- <span data-ttu-id="9e029-156">A`DropDownList`に割り当てるコースを選択するためのコントロールです。</span><span class="sxs-lookup"><span data-stu-id="9e029-156">A `DropDownList` control for selecting a course to assign.</span></span> <span data-ttu-id="9e029-157">このコントロールは、選択したインストラクターに現在割り当てられていないコースで入力されます。</span><span class="sxs-lookup"><span data-stu-id="9e029-157">This control will be populated with courses that are currently not assigned to the selected instructor.</span></span>
- <span data-ttu-id="9e029-158">A`Button`割り当てを開始するコントロール。</span><span class="sxs-lookup"><span data-stu-id="9e029-158">A `Button` control to initiate the assignment.</span></span>
- <span data-ttu-id="9e029-159">A`Label`コントロールに割り当てが失敗した場合、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9e029-159">A `Label` control to display an error message if the assignment fails.</span></span>

<span data-ttu-id="9e029-160">最後に、マークアップでは、選択した講師からコースを削除するのに使用するコントロールのグループも作成します。</span><span class="sxs-lookup"><span data-stu-id="9e029-160">Finally, the markup also creates a group of controls to use for removing a course from the selected instructor.</span></span>

<span data-ttu-id="9e029-161">*InstructorsCourses.aspx.cs*を使用して、追加のステートメント。</span><span class="sxs-lookup"><span data-stu-id="9e029-161">In *InstructorsCourses.aspx.cs*, add a using statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

<span data-ttu-id="9e029-162">コースを表示する 2 つのドロップダウン リストを設定するためのメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9e029-162">Add a method for populating the two drop-down lists that display courses:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

<span data-ttu-id="9e029-163">このコードの取得からすべてのコース、`Courses`セットされからコースを取得するエンティティ、`Courses`のナビゲーション プロパティ、`Person`講師が選択されているエンティティ。</span><span class="sxs-lookup"><span data-stu-id="9e029-163">This code gets all courses from the `Courses` entity set and gets the courses from the `Courses` navigation property of the `Person` entity for the selected instructor.</span></span> <span data-ttu-id="9e029-164">そのインストラクターに割り当てられているどのコースを決定し、それに応じてドロップダウン リストを設定します。</span><span class="sxs-lookup"><span data-stu-id="9e029-164">It then determines which courses are assigned to that instructor and populates the drop-down lists accordingly.</span></span>

<span data-ttu-id="9e029-165">ハンドラーを追加、`Assign`ボタンの`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="9e029-165">Add a handler for the `Assign` button's `Click` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

<span data-ttu-id="9e029-166">このコードを取得、`Person`講師が、選択したエンティティを取得、`Course`選択のコースのエンティティを選択したコースを追加し、`Courses`インストラクターのナビゲーション プロパティ`Person`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9e029-166">This code gets the `Person` entity for the selected instructor, gets the `Course` entity for the selected course, and adds the selected course to the `Courses` navigation property of the instructor's `Person` entity.</span></span> <span data-ttu-id="9e029-167">データベースに変更を保存し、結果はすぐに表示されることができますので、ドロップダウン リストを再作成します。</span><span class="sxs-lookup"><span data-stu-id="9e029-167">It then saves the changes to the database and repopulates the drop-down lists so the results can be seen immediately.</span></span>

<span data-ttu-id="9e029-168">ハンドラーを追加、`Remove`ボタンの`Click`イベント。</span><span class="sxs-lookup"><span data-stu-id="9e029-168">Add a handler for the `Remove` button's `Click` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

<span data-ttu-id="9e029-169">このコードを取得、`Person`講師が、選択したエンティティを取得、`Course`選択のコースのエンティティから選択したコースを削除し、`Person`エンティティの`Courses`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="9e029-169">This code gets the `Person` entity for the selected instructor, gets the `Course` entity for the selected course, and removes the selected course from the `Person` entity's `Courses` navigation property.</span></span> <span data-ttu-id="9e029-170">データベースに変更を保存し、結果はすぐに表示されることができますので、ドロップダウン リストを再作成します。</span><span class="sxs-lookup"><span data-stu-id="9e029-170">It then saves the changes to the database and repopulates the drop-down lists so the results can be seen immediately.</span></span>

<span data-ttu-id="9e029-171">コードを追加して、`Page_Load`ことを確認して、エラー メッセージは、メソッドはレポートには、しのハンドラーを追加するとエラーがない場合に表示されない、`DataBound`と`SelectedIndexChanged`コースのドロップダウン リストを設定するインストラクター ドロップダウン リストのイベント。</span><span class="sxs-lookup"><span data-stu-id="9e029-171">Add code to the `Page_Load` method that makes sure the error messages are not visible when there's no error to report, and add handlers for the `DataBound` and `SelectedIndexChanged` events of the instructors drop-down list to populate the courses drop-down lists:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

<span data-ttu-id="9e029-172">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="9e029-172">Run the page.</span></span>

<span data-ttu-id="9e029-173">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="9e029-173">[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)</span></span>

<span data-ttu-id="9e029-174">あるインストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="9e029-174">Select an instructor.</span></span> <span data-ttu-id="9e029-175"><strong>コースを割り当てる</strong>インストラクターしない講義では、コースがドロップダウン リストに表示されます、<strong>コースを削除する</strong>ドロップダウン リストには、インストラクターに既に割り当てられているコースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9e029-175">The <strong>Assign a Course</strong> drop-down list displays the courses that the instructor doesn't teach, and the <strong>Remove a Course</strong> drop-down list displays the courses that the instructor is already assigned to.</span></span> <span data-ttu-id="9e029-176"><strong>コースを割り当てる</strong>セクションでコースを選択し、をクリックして<strong>割り当てる</strong>です。</span><span class="sxs-lookup"><span data-stu-id="9e029-176">In the <strong>Assign a Course</strong> section, select a course and then click <strong>Assign</strong>.</span></span> <span data-ttu-id="9e029-177">コースに移動、<strong>コースを削除する</strong>ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="9e029-177">The course moves to the <strong>Remove a Course</strong> drop-down list.</span></span> <span data-ttu-id="9e029-178">コースを選択、<strong>コースを削除する</strong>セクションし、をクリックして<strong>削除</strong><em>です。</em></span><span class="sxs-lookup"><span data-stu-id="9e029-178">Select a course in the <strong>Remove a Course</strong> section and click <strong>Remove</strong><em>.</em></span></span> <span data-ttu-id="9e029-179">コースに移動、<strong>コースを割り当てる</strong>ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="9e029-179">The course moves to the <strong>Assign a Course</strong> drop-down list.</span></span>

<span data-ttu-id="9e029-180">関連するデータを操作する方法がいくつか見てきました。</span><span class="sxs-lookup"><span data-stu-id="9e029-180">You have now seen some more ways to work with related data.</span></span> <span data-ttu-id="9e029-181">次のチュートリアルでは、アプリケーションの保守容易性を向上させるために、データ モデルでの継承を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="9e029-181">In the following tutorial, you'll learn how to use inheritance in the data model to improve the maintainability of your application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9e029-182">[前へ](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="9e029-182">[Previous](the-entity-framework-and-aspnet-getting-started-part-4.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-6.md)</span></span>
