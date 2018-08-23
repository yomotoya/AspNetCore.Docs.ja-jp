---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 4 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: f4a8e7f1cafd169f392485ff4ecb260073209210
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833566"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a><span data-ttu-id="48b0e-104">Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 4</span><span class="sxs-lookup"><span data-stu-id="48b0e-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 4</span></span>
====================
<span data-ttu-id="48b0e-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="48b0e-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="48b0e-106">Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="48b0e-107">チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="48b0e-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="working-with-related-data"></a><span data-ttu-id="48b0e-108">関連データの使用</span><span class="sxs-lookup"><span data-stu-id="48b0e-108">Working with Related Data</span></span>

<span data-ttu-id="48b0e-109">前のチュートリアルでは使用して、`EntityDataSource`フィルター、並べ替え、およびデータをグループ化を制御します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-109">In the previous tutorial you used the `EntityDataSource` control to filter, sort, and group data.</span></span> <span data-ttu-id="48b0e-110">このチュートリアルでは、表示し、関連データを更新します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-110">In this tutorial you'll display and update related data.</span></span>

<span data-ttu-id="48b0e-111">インストラクターのリストを示す Instructors ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-111">You'll create the Instructors page that shows a list of instructors.</span></span> <span data-ttu-id="48b0e-112">インストラクターを選択するとそのインストラクターが指導するコースのリストを参照してください。</span><span class="sxs-lookup"><span data-stu-id="48b0e-112">When you select an instructor, you see a list of courses taught by that instructor.</span></span> <span data-ttu-id="48b0e-113">コースを選択すると、コースと、コースに登録されている学生の一覧の詳細を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48b0e-113">When you select a course, you see details for the course and a list of students enrolled in the course.</span></span> <span data-ttu-id="48b0e-114">インストラクターの名前、入社日、およびオフィスの割り当てを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="48b0e-114">You can edit the instructor name, hire date, and office assignment.</span></span> <span data-ttu-id="48b0e-115">オフィスの割り当ては、ナビゲーション プロパティを介してアクセスする別のエンティティ セットです。</span><span class="sxs-lookup"><span data-stu-id="48b0e-115">The office assignment is a separate entity set that you access through a navigation property.</span></span>

<span data-ttu-id="48b0e-116">マスター データは、マークアップまたはコードの詳細データにリンクできます。</span><span class="sxs-lookup"><span data-stu-id="48b0e-116">You can link master data to detail data in markup or in code.</span></span> <span data-ttu-id="48b0e-117">このチュートリアルでは、両方のメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-117">In this part of the tutorial, you'll use both methods.</span></span>

<span data-ttu-id="48b0e-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)</span></span>

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a><span data-ttu-id="48b0e-119">表示および GridView コントロール内の関連エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-119">Displaying and Updating Related Entities in a GridView Control</span></span>

<span data-ttu-id="48b0e-120">という名前の新しい web ページを作成する*Instructors.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:</span><span class="sxs-lookup"><span data-stu-id="48b0e-120">Create a new web page named *Instructors.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

<span data-ttu-id="48b0e-121">このマークアップを作成、`EntityDataSource`インストラクターを選択し、更新を有効にするコントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-121">This markup creates an `EntityDataSource` control that selects instructors and enables updates.</span></span> <span data-ttu-id="48b0e-122">`div`要素は、後で、右側の列を追加できるように、左側に表示するマークアップを構成します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-122">The `div` element configures markup to render on the left so that you can add a column on the right later.</span></span>

<span data-ttu-id="48b0e-123">間、`EntityDataSource`マークアップと、終了`</div>`タグを作成する次のマークアップを追加、`GridView`コントロールと`Label`エラー メッセージに使用するコントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-123">Between the `EntityDataSource` markup and the closing `</div>` tag, add the following markup that creates a `GridView` control and a `Label` control that you'll use for error messages:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

<span data-ttu-id="48b0e-124">これは、`GridView`コントロールの行を選択できるように、淡いグレーの背景の色で選択されている行を強調表示、およびのハンドラー (これは後で作成します) を指定します、`SelectedIndexChanged`と`Updating`イベント。</span><span class="sxs-lookup"><span data-stu-id="48b0e-124">This `GridView` control enables row selection, highlights the selected row with a light gray background color, and specifies handlers (which you'll create later) for the `SelectedIndexChanged` and `Updating` events.</span></span> <span data-ttu-id="48b0e-125">また、指定`PersonID`の`DataKeyNames`プロパティ、いるため、選択した行のキーの値を後で追加する別のコントロールに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="48b0e-125">It also specifies `PersonID` for the `DataKeyNames` property, so that the key value of the selected row can be passed to another control that you'll add later.</span></span>

<span data-ttu-id="48b0e-126">最後の列にはナビゲーション プロパティに格納されている場合は、インストラクターのオフィスの割り当てが含まれています、`Person`エンティティ関連付けられたエンティティからを備えているためです。</span><span class="sxs-lookup"><span data-stu-id="48b0e-126">The last column contains the instructor's office assignment, which is stored in a navigation property of the `Person` entity because it comes from an associated entity.</span></span> <span data-ttu-id="48b0e-127">注意、`EditItemTemplate`要素を指定します`Eval`の代わりに`Bind`ため、`GridView`それらを更新するには、コントロールはナビゲーション プロパティにバインド直接ことはできません。</span><span class="sxs-lookup"><span data-stu-id="48b0e-127">Notice that the `EditItemTemplate` element specifies `Eval` instead of `Bind`, because the `GridView` control cannot directly bind to navigation properties in order to update them.</span></span> <span data-ttu-id="48b0e-128">コードでオフィスの割り当てを更新します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-128">You'll update the office assignment in code.</span></span> <span data-ttu-id="48b0e-129">そのためにはへの参照が必要、`TextBox`して、コントロールを取得しでこれを保存、`TextBox`コントロールの`Init`イベント。</span><span class="sxs-lookup"><span data-stu-id="48b0e-129">To do that, you'll need a reference to the `TextBox` control, and you'll get and save that in the `TextBox` control's `Init` event.</span></span>

<span data-ttu-id="48b0e-130">次の`GridView`コントロールが、`Label`エラー メッセージを使用するコントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-130">Following the `GridView` control is a `Label` control that's used for error messages.</span></span> <span data-ttu-id="48b0e-131">コントロールの`Visible`プロパティは`false`、のみとコード、可視化でエラー発生時に、ラベルが表示されるようビュー ステートがオフになっています。</span><span class="sxs-lookup"><span data-stu-id="48b0e-131">The control's `Visible` property is `false`, and view state is turned off, so that the label will appear only when code makes it visible in response to an error.</span></span>

<span data-ttu-id="48b0e-132">開く、 *Instructors.aspx.cs*ファイルを開き、次の追加`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="48b0e-132">Open the *Instructors.aspx.cs* file and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

<span data-ttu-id="48b0e-133">オフィスの割り当てのテキスト ボックスへの参照を保持するために部分クラス名の宣言の直後後には、プライベート クラス フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-133">Add a private class field immediately after the partial-class name declaration to hold a reference to the office assignment text box.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

<span data-ttu-id="48b0e-134">追加のスタブ、`SelectedIndexChanged`イベント ハンドラーが、後でコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-134">Add a stub for the `SelectedIndexChanged` event handler that you'll add code for later.</span></span> <span data-ttu-id="48b0e-135">また、オフィスの割り当てのハンドラーを追加`TextBox`コントロールの`Init`イベントへの参照を保存すること、`TextBox`コントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-135">Also add a handler for the office assignment `TextBox` control's `Init` event so that you can store a reference to the `TextBox` control.</span></span> <span data-ttu-id="48b0e-136">ユーザーがナビゲーション プロパティに関連付けられたエンティティを更新するために入力した値を取得するのに、この参照を使用します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-136">You'll use this reference to get the value the user entered in order to update the entity associated with the navigation property.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

<span data-ttu-id="48b0e-137">使用して、`GridView`コントロールの`Updating`を更新するイベント、`Location`プロパティ、関連付けられている`OfficeAssignment`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="48b0e-137">You'll use the `GridView` control's `Updating` event to update the `Location` property of the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="48b0e-138">次のハンドラーを追加、`Updating`イベント。</span><span class="sxs-lookup"><span data-stu-id="48b0e-138">Add the following handler for the `Updating` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

<span data-ttu-id="48b0e-139">ユーザーがクリックしたときにこのコードが実行**Update**で、`GridView`行。</span><span class="sxs-lookup"><span data-stu-id="48b0e-139">This code is run when the user clicks **Update** in a `GridView` row.</span></span> <span data-ttu-id="48b0e-140">コードでは、LINQ to Entities を使用して、取得、`OfficeAssignment`に現在関連付けられているエンティティ`Person`、エンティティを使用して、`PersonID`イベント引数から選択した行の。</span><span class="sxs-lookup"><span data-stu-id="48b0e-140">The code uses LINQ to Entities to retrieve the `OfficeAssignment` entity that's associated with the current `Person` entity, using the `PersonID` of the selected row from the event argument.</span></span>

<span data-ttu-id="48b0e-141">コードはの値に応じて、次の操作の 1 つ、`InstructorOfficeTextBox`コントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-141">The code then takes one of the following actions depending on the value in the `InstructorOfficeTextBox` control:</span></span>

- <span data-ttu-id="48b0e-142">かどうか、テキスト ボックスの値を持つし、はない`OfficeAssignment`を更新するエンティティが 1 つ作成されます。</span><span class="sxs-lookup"><span data-stu-id="48b0e-142">If the text box has a value and there's no `OfficeAssignment` entity to update, it creates one.</span></span>
- <span data-ttu-id="48b0e-143">テキスト ボックスの値を持つしがあるかどうか、 `OfficeAssignment` 、エンティティを更新、`Location`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="48b0e-143">If the text box has a value and there's an `OfficeAssignment` entity, it updates the `Location` property value.</span></span>
- <span data-ttu-id="48b0e-144">テキスト ボックスが空の場合、`OfficeAssignment`エンティティが存在する場合、エンティティを削除します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-144">If the text box is empty and an `OfficeAssignment` entity exists, it deletes the entity.</span></span>

<span data-ttu-id="48b0e-145">その後、変更をデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-145">After this, it saves the changes to the database.</span></span> <span data-ttu-id="48b0e-146">例外が発生する場合は、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48b0e-146">If an exception occurs, it displays an error message.</span></span>

<span data-ttu-id="48b0e-147">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-147">Run the page.</span></span>

<span data-ttu-id="48b0e-148">[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-148">[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)</span></span>

<span data-ttu-id="48b0e-149">をクリックして**編集**し、すべてのフィールドがテキスト ボックスに変更します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-149">Click **Edit** and all fields change to text boxes.</span></span>

<span data-ttu-id="48b0e-150">[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-150">[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)</span></span>

<span data-ttu-id="48b0e-151">これらの値のいずれかを変更する**オフィスの割り当て**します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-151">Change any of these values, including **Office Assignment**.</span></span> <span data-ttu-id="48b0e-152">クリックして**Update**し、リストに反映される変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48b0e-152">Click **Update** and you'll see the changes reflected in the list.</span></span>

## <a name="displaying-related-entities-in-a-separate-control"></a><span data-ttu-id="48b0e-153">個別のコントロールに関連するエンティティを表示します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-153">Displaying Related Entities in a Separate Control</span></span>

<span data-ttu-id="48b0e-154">各講師は 1 つまたは複数のコースを追加しますので、`EntityDataSource`コントロールと`GridView`どのインストラクターが選択されて、instructors に関連付けられているコースの一覧を表示するコントロール`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-154">Each instructor can teach one or more courses, so you'll add an `EntityDataSource` control and a `GridView` control to list the courses associated with whichever instructor is selected in the instructors `GridView` control.</span></span> <span data-ttu-id="48b0e-155">見出しを作成して、`EntityDataSource`コース エンティティの管理、エラー メッセージの間で、次のマークアップを追加`Label`コントロールおよび決算`</div>`タグ。</span><span class="sxs-lookup"><span data-stu-id="48b0e-155">To create a heading and the `EntityDataSource` control for courses entities, add the following markup between the error message `Label` control and the closing `</div>` tag:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

<span data-ttu-id="48b0e-156">`Where`パラメーターには値が含まれています、`PersonID`の講師である行が選択されて、`InstructorsGridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-156">The `Where` parameter contains the value of the `PersonID` of the instructor whose row is selected in the `InstructorsGridView` control.</span></span> <span data-ttu-id="48b0e-157">`Where`プロパティに関連付けられているすべてを取得するサブセレクト コマンドが含まれています`Person`からエンティティを`Course`エンティティの`People`ナビゲーション プロパティを選択します、`Course`エンティティ場合にのみ関連付けられているのいずれかの`Person`エンティティが含まれていますが、選択した`PersonID`値。</span><span class="sxs-lookup"><span data-stu-id="48b0e-157">The `Where` property contains a subselect command that gets all associated `Person` entities from a `Course` entity's `People` navigation property and selects the `Course` entity only if one of the associated `Person` entities contains the selected `PersonID` value.</span></span>

<span data-ttu-id="48b0e-158">作成する、`GridView`コントロール。 直後に続く次のマークアップを追加、`CoursesEntityDataSource`コントロール (終了する前に`</div>`タグ)。</span><span class="sxs-lookup"><span data-stu-id="48b0e-158">To create the `GridView` control., add the following markup immediately following the `CoursesEntityDataSource` control (before the closing `</div>` tag):</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

<span data-ttu-id="48b0e-159">インストラクターが選択されていない場合のコースは表示されませんので、`EmptyDataTemplate`要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="48b0e-159">Because no courses will be displayed if no instructor is selected, an `EmptyDataTemplate` element is included.</span></span>

<span data-ttu-id="48b0e-160">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-160">Run the page.</span></span>

<span data-ttu-id="48b0e-161">[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-161">[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)</span></span>

<span data-ttu-id="48b0e-162">割り当てられている、1 つまたは複数のコースを持つインストラクターを選択し、コースまたはコースが一覧に表示します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-162">Select an instructor who has one or more courses assigned, and the course or courses appear in the list.</span></span> <span data-ttu-id="48b0e-163">(注: データベース スキーマには、複数のコースができますが、データベースに付属しているテスト データでインストラクター実際にはない 1 つ以上のコースです。</span><span class="sxs-lookup"><span data-stu-id="48b0e-163">(Note: although the database schema allows multiple courses, in the test data supplied with the database no instructor actually has more than one course.</span></span> <span data-ttu-id="48b0e-164">自分自身を追加できますコースにデータベースを使用して、**サーバー エクスプ ローラー**ウィンドウまたは*CoursesAdd.aspx*ページで、後のチュートリアルで追加します)。</span><span class="sxs-lookup"><span data-stu-id="48b0e-164">You can add courses to the database yourself using the **Server Explorer** window or the *CoursesAdd.aspx* page, which you'll add in a later tutorial.)</span></span>

<span data-ttu-id="48b0e-165">[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-165">[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)</span></span>

<span data-ttu-id="48b0e-166">`CoursesGridView`コントロールには、いくつかのコース フィールドのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48b0e-166">The `CoursesGridView` control shows only a few course fields.</span></span> <span data-ttu-id="48b0e-167">コースのすべての詳細を表示するには、使用する、`DetailsView`ユーザーが選択したコースのコントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-167">To display all the details for a course, you'll use a `DetailsView` control for the course that the user selects.</span></span> <span data-ttu-id="48b0e-168">*Instructors.aspx*、終了後に次のマークアップを追加`</div>`タグ (このマークアップを配置するかどうかを確認**後**終了 div タグを前にない)。</span><span class="sxs-lookup"><span data-stu-id="48b0e-168">In *Instructors.aspx*, add the following markup after the closing `</div>` tag (make sure you place this markup **after** the closing div tag, not before it):</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

<span data-ttu-id="48b0e-169">このマークアップを作成、`EntityDataSource`コントロールにバインドされている、`Courses`エンティティ セット。</span><span class="sxs-lookup"><span data-stu-id="48b0e-169">This markup creates an `EntityDataSource` control that's bound to the `Courses` entity set.</span></span> <span data-ttu-id="48b0e-170">`Where`プロパティの選択を使用して、コース、`CourseID`コースで選択した行の値`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-170">The `Where` property selects a course using the `CourseID` value of the selected row in the courses `GridView` control.</span></span> <span data-ttu-id="48b0e-171">マークアップのハンドラーを指定する、`Selected`は別のレベルが階層内の下位イベント、学生の成績を表示するため後で使用します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-171">The markup specifies a handler for the `Selected` event, which you'll use later for displaying student grades, which is another level lower in the hierarchy.</span></span>

<span data-ttu-id="48b0e-172">*Instructors.aspx.cs*、次のスタブを作成、`CourseDetailsEntityDataSource_Selected`メソッド。</span><span class="sxs-lookup"><span data-stu-id="48b0e-172">In *Instructors.aspx.cs*, create the following stub for the `CourseDetailsEntityDataSource_Selected` method.</span></span> <span data-ttu-id="48b0e-173">(チュートリアルの後半でこのスタブを入力します。 ここでは、必要なページがコンパイルされ実行されるようにします)。</span><span class="sxs-lookup"><span data-stu-id="48b0e-173">(You'll fill this stub out later in the tutorial; for now, you need it so that the page will compile and run.)</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

<span data-ttu-id="48b0e-174">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-174">Run the page.</span></span>

<span data-ttu-id="48b0e-175">[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-175">[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)</span></span>

<span data-ttu-id="48b0e-176">最初にないコースの詳細コースが選択されていないためです。</span><span class="sxs-lookup"><span data-stu-id="48b0e-176">Initially there are no course details because no course is selected.</span></span> <span data-ttu-id="48b0e-177">、割り当てられているコースを持つインストラクターを選択し、詳細を表示するコースを選択します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-177">Select an instructor who has a course assigned, and then select a course to see the details.</span></span>

<span data-ttu-id="48b0e-178">[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-178">[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)</span></span>

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a><span data-ttu-id="48b0e-179">EntityDataSource を使用して「選択」イベント関連データを表示するには</span><span class="sxs-lookup"><span data-stu-id="48b0e-179">Using the EntityDataSource "Selected" Event to Display Related Data</span></span>

<span data-ttu-id="48b0e-180">最後に、すべての登録済みの受講者とその成績、選択したコースを表示します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-180">Finally, you want to show all of the enrolled students and their grades for the selected course.</span></span> <span data-ttu-id="48b0e-181">これを行うには、使用する、`Selected`のイベント、`EntityDataSource`コースにコントロールがバインドされている`DetailsView`します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-181">To do this, you'll use the `Selected` event of the `EntityDataSource` control bound to the course `DetailsView`.</span></span>

<span data-ttu-id="48b0e-182">*Instructors.aspx*、後に次のマークアップを追加、`DetailsView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-182">In *Instructors.aspx*, add the following markup after the `DetailsView` control:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

<span data-ttu-id="48b0e-183">このマークアップを作成、`ListView`学生と、選択したコースの成績のリストを表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="48b0e-183">This markup creates a `ListView` control that displays a list of students and their grades for the selected course.</span></span> <span data-ttu-id="48b0e-184">データ バインド コード内でコントロールがわかるため、データ ソースを指定しません。</span><span class="sxs-lookup"><span data-stu-id="48b0e-184">No data source is specified because you'll databind the control in code.</span></span> <span data-ttu-id="48b0e-185">`EmptyDataTemplate`要素は、コースが選択されていないときに表示するメッセージを提供します。-その場合は、表示する学生はありません。</span><span class="sxs-lookup"><span data-stu-id="48b0e-185">The `EmptyDataTemplate` element provides a message to display when no course is selected—in that case, there are no students to display.</span></span> <span data-ttu-id="48b0e-186">`LayoutTemplate`要素の一覧を表示する HTML テーブルを作成して、`ItemTemplate`を表示する列を指定します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-186">The `LayoutTemplate` element creates an HTML table to display the list, and the `ItemTemplate` specifies the columns to display.</span></span> <span data-ttu-id="48b0e-187">学生 ID と生徒の成績が、`StudentGrade`エンティティと受講者名が、 `Person` Entity Framework で利用するエンティティ、`Person`のナビゲーション プロパティ、`StudentGrade`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="48b0e-187">The student ID and the student grade are from the `StudentGrade` entity, and the student name is from the `Person` entity that the Entity Framework makes available in the `Person` navigation property of the `StudentGrade` entity.</span></span>

<span data-ttu-id="48b0e-188">*Instructors.aspx.cs*、スタブとして作成された出力を置き換える`CourseDetailsEntityDataSource_Selected`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="48b0e-188">In *Instructors.aspx.cs*, replace the stubbed-out `CourseDetailsEntityDataSource_Selected` method with the following code:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

<span data-ttu-id="48b0e-189">このイベントのイベント引数がない場合、項目数は 0 または 1 つの項目がコレクションの形式で選択したデータを提供する場合、`Course`エンティティを選択します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-189">The event argument for this event provides the selected data in the form of a collection, which will have zero items if nothing is selected or one item if a `Course` entity is selected.</span></span> <span data-ttu-id="48b0e-190">場合、`Course`エンティティが選択されている場合、コードを使用して、`First`コレクションを 1 つのオブジェクトに変換します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-190">If a `Course` entity is selected, the code uses the `First` method to convert the collection to a single object.</span></span> <span data-ttu-id="48b0e-191">取得し、 `StudentGrade` 、ナビゲーション プロパティからエンティティが、コレクションに変換し、バインド、`GradesListView`コレクションを制御します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-191">It then gets `StudentGrade` entities from the navigation property, converts them to a collection, and binds the `GradesListView` control to the collection.</span></span>

<span data-ttu-id="48b0e-192">これは、最初に、ページが表示されますが、空のデータ テンプレート内のメッセージに表示されることを確認するには成績を表示するための十分なとコースが選択されていないときにします。</span><span class="sxs-lookup"><span data-stu-id="48b0e-192">This is sufficient to display grades, but you want to make sure that the message in the empty data template is displayed the first time the page is displayed and whenever a course is not selected.</span></span> <span data-ttu-id="48b0e-193">そのためには、次のメソッドは、2 つの場所から呼び出すことがありますを作成します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-193">To do that, create the following method, which you'll call from two places:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

<span data-ttu-id="48b0e-194">この新しいメソッドを呼び出し、`Page_Load`ページが表示される空のデータ テンプレートの最初の時刻を表示するメソッド。</span><span class="sxs-lookup"><span data-stu-id="48b0e-194">Call this new method from the `Page_Load` method to display the empty data template the first time the page is displayed.</span></span> <span data-ttu-id="48b0e-195">それを呼び出すと、`InstructorsGridView_SelectedIndexChanged`メソッド新しいコースの意味がコースに読み込まれるため、インストラクターが選択されたときに、そのイベントを発生は、`GridView`コントロールと none がまだ選択されています。</span><span class="sxs-lookup"><span data-stu-id="48b0e-195">And call it from the `InstructorsGridView_SelectedIndexChanged` method because that event is raised when an instructor is selected, which means new courses are loaded into the courses `GridView` control and none is selected yet.</span></span> <span data-ttu-id="48b0e-196">2 つの呼び出しを次に示します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-196">Here are the two calls:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

<span data-ttu-id="48b0e-197">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-197">Run the page.</span></span>

<span data-ttu-id="48b0e-198">[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-198">[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)</span></span>

<span data-ttu-id="48b0e-199">割り当てられているコースがインストラクターを選択し、コースを選択します。</span><span class="sxs-lookup"><span data-stu-id="48b0e-199">Select an instructor that has a course assigned, and then select the course.</span></span>

<span data-ttu-id="48b0e-200">[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="48b0e-200">[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)</span></span>

<span data-ttu-id="48b0e-201">関連データを操作するいくつかの方法を確認できました。</span><span class="sxs-lookup"><span data-stu-id="48b0e-201">You have now seen a few ways to work with related data.</span></span> <span data-ttu-id="48b0e-202">次のチュートリアルでは、既存のエンティティ間のリレーションシップを追加する方法が学習、リレーションシップを削除する方法と、既存のエンティティの関係がある新しいエンティティを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="48b0e-202">In the following tutorial, you'll learn how to add relationships between existing entities, how to remove relationships, and how to add a new entity that has a relationship to an existing entity.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48b0e-203">[前へ](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="48b0e-203">[Previous](the-entity-framework-and-aspnet-getting-started-part-3.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-5.md)</span></span>
