---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 4 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886870"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a><span data-ttu-id="c57c5-104">データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 4</span><span class="sxs-lookup"><span data-stu-id="c57c5-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 4</span></span>
====================
<span data-ttu-id="c57c5-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c57c5-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="c57c5-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="c57c5-107">一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="c57c5-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="working-with-related-data"></a><span data-ttu-id="c57c5-108">関連するデータの使用</span><span class="sxs-lookup"><span data-stu-id="c57c5-108">Working with Related Data</span></span>

<span data-ttu-id="c57c5-109">前のチュートリアルでは使用して、`EntityDataSource`フィルター、並べ替え、およびデータをグループ化を制御します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-109">In the previous tutorial you used the `EntityDataSource` control to filter, sort, and group data.</span></span> <span data-ttu-id="c57c5-110">このチュートリアルを表示し、関連するデータを更新します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-110">In this tutorial you'll display and update related data.</span></span>

<span data-ttu-id="c57c5-111">講習においてインストラクターの一覧を表示するインストラクター ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-111">You'll create the Instructors page that shows a list of instructors.</span></span> <span data-ttu-id="c57c5-112">あるインストラクターを選択するとそのインストラクターでコースの一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c57c5-112">When you select an instructor, you see a list of courses taught by that instructor.</span></span> <span data-ttu-id="c57c5-113">コースを選択すると、コースを受講者コースに登録済みの一覧の詳細を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c57c5-113">When you select a course, you see details for the course and a list of students enrolled in the course.</span></span> <span data-ttu-id="c57c5-114">教師名、入社日とオフィス割り当てを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="c57c5-114">You can edit the instructor name, hire date, and office assignment.</span></span> <span data-ttu-id="c57c5-115">Office の割り当ては、ナビゲーション プロパティを介してアクセスする別のエンティティ セットです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-115">The office assignment is a separate entity set that you access through a navigation property.</span></span>

<span data-ttu-id="c57c5-116">マスター データは、マークアップやコードの詳細データにリンクできます。</span><span class="sxs-lookup"><span data-stu-id="c57c5-116">You can link master data to detail data in markup or in code.</span></span> <span data-ttu-id="c57c5-117">このチュートリアルでは、両方のメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-117">In this part of the tutorial, you'll use both methods.</span></span>

<span data-ttu-id="c57c5-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-118">[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)</span></span>

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a><span data-ttu-id="c57c5-119">表示および GridView コントロール内の関連エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-119">Displaying and Updating Related Entities in a GridView Control</span></span>

<span data-ttu-id="c57c5-120">という名前の新しい web ページを作成する*Instructors.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:</span><span class="sxs-lookup"><span data-stu-id="c57c5-120">Create a new web page named *Instructors.aspx* that uses the *Site.Master* master page, and add the following markup to the `Content` control named `Content2`:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

<span data-ttu-id="c57c5-121">このマークアップを作成、`EntityDataSource`講習においてインストラクターを選択し、更新を有効にするコントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-121">This markup creates an `EntityDataSource` control that selects instructors and enables updates.</span></span> <span data-ttu-id="c57c5-122">`div`要素は後で、右側の列を追加できるように、左側に表示するマークアップを構成します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-122">The `div` element configures markup to render on the left so that you can add a column on the right later.</span></span>

<span data-ttu-id="c57c5-123">間、`EntityDataSource`マークアップと終了`</div>`タグは、次のマークアップを作成するを追加、`GridView`コントロールと`Label`エラー メッセージに使用するコントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-123">Between the `EntityDataSource` markup and the closing `</div>` tag, add the following markup that creates a `GridView` control and a `Label` control that you'll use for error messages:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

<span data-ttu-id="c57c5-124">これは、`GridView`コントロールにより、行の選択、明るい灰色の背景色で選択した行を強調表示およびのハンドラーを作成した後で) を指定します、`SelectedIndexChanged`と`Updating`イベント。</span><span class="sxs-lookup"><span data-stu-id="c57c5-124">This `GridView` control enables row selection, highlights the selected row with a light gray background color, and specifies handlers (which you'll create later) for the `SelectedIndexChanged` and `Updating` events.</span></span> <span data-ttu-id="c57c5-125">指定`PersonID`の`DataKeyNames`プロパティ、選択した行のキーの値は、後で追加する別のコントロールに渡すことができるようにします。</span><span class="sxs-lookup"><span data-stu-id="c57c5-125">It also specifies `PersonID` for the `DataKeyNames` property, so that the key value of the selected row can be passed to another control that you'll add later.</span></span>

<span data-ttu-id="c57c5-126">最後の列にはナビゲーション プロパティに格納されている、インストラクター オフィス割り当てが含まれています、`Person`エンティティに関連付けられているエンティティのものであるためです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-126">The last column contains the instructor's office assignment, which is stored in a navigation property of the `Person` entity because it comes from an associated entity.</span></span> <span data-ttu-id="c57c5-127">注意して、`EditItemTemplate`要素を指定`Eval`の代わりに`Bind`であるため、`GridView`それらを更新するために、コントロールはナビゲーション プロパティにバインド直接ことはできません。</span><span class="sxs-lookup"><span data-stu-id="c57c5-127">Notice that the `EditItemTemplate` element specifies `Eval` instead of `Bind`, because the `GridView` control cannot directly bind to navigation properties in order to update them.</span></span> <span data-ttu-id="c57c5-128">コードでの office 割り当てを更新します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-128">You'll update the office assignment in code.</span></span> <span data-ttu-id="c57c5-129">実行するへの参照を必要があります、`TextBox`制御、およびするを取得し、保存するには`TextBox`コントロールの`Init`イベント。</span><span class="sxs-lookup"><span data-stu-id="c57c5-129">To do that, you'll need a reference to the `TextBox` control, and you'll get and save that in the `TextBox` control's `Init` event.</span></span>

<span data-ttu-id="c57c5-130">次の`GridView`コントロールは、`Label`エラー メッセージを使用するコントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-130">Following the `GridView` control is a `Label` control that's used for error messages.</span></span> <span data-ttu-id="c57c5-131">コントロールの`Visible`プロパティは`false`、のみコードが発行した場合、エラーへの応答に表示されるラベルが表示されるようにビュー ステートは、無効になります。</span><span class="sxs-lookup"><span data-stu-id="c57c5-131">The control's `Visible` property is `false`, and view state is turned off, so that the label will appear only when code makes it visible in response to an error.</span></span>

<span data-ttu-id="c57c5-132">開く、 *Instructors.aspx.cs*ファイルし、次の追加`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="c57c5-132">Open the *Instructors.aspx.cs* file and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

<span data-ttu-id="c57c5-133">Office の割り当てのテキスト ボックスへの参照を保持するために、部分クラス名宣言の直後後には、プライベート クラス フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-133">Add a private class field immediately after the partial-class name declaration to hold a reference to the office assignment text box.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

<span data-ttu-id="c57c5-134">スタブを追加、`SelectedIndexChanged`イベント ハンドラーの後でコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-134">Add a stub for the `SelectedIndexChanged` event handler that you'll add code for later.</span></span> <span data-ttu-id="c57c5-135">また、office の割り当てのハンドラーを追加`TextBox`コントロールの`Init`イベントへの参照を保存すること、`TextBox`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-135">Also add a handler for the office assignment `TextBox` control's `Init` event so that you can store a reference to the `TextBox` control.</span></span> <span data-ttu-id="c57c5-136">ユーザーがナビゲーション プロパティに関連付けられているエンティティを更新するために入力した値を取得するのににはこの参照を使用します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-136">You'll use this reference to get the value the user entered in order to update the entity associated with the navigation property.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

<span data-ttu-id="c57c5-137">使用して、`GridView`コントロールの`Updating`を更新するイベント、 `Location` 、関連するプロパティ`OfficeAssignment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-137">You'll use the `GridView` control's `Updating` event to update the `Location` property of the associated `OfficeAssignment` entity.</span></span> <span data-ttu-id="c57c5-138">次のハンドラーを追加、`Updating`イベント。</span><span class="sxs-lookup"><span data-stu-id="c57c5-138">Add the following handler for the `Updating` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

<span data-ttu-id="c57c5-139">ユーザーがクリックしたときにこのコードが実行**更新**で、`GridView`行です。</span><span class="sxs-lookup"><span data-stu-id="c57c5-139">This code is run when the user clicks **Update** in a `GridView` row.</span></span> <span data-ttu-id="c57c5-140">コードでは、LINQ to Entities を使用して、取得、`OfficeAssignment`に現在関連付けられているエンティティ`Person`、エンティティを使用して、`PersonID`イベント引数から、選択した行のです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-140">The code uses LINQ to Entities to retrieve the `OfficeAssignment` entity that's associated with the current `Person` entity, using the `PersonID` of the selected row from the event argument.</span></span>

<span data-ttu-id="c57c5-141">コードはの値によって次の操作のいずれか、`InstructorOfficeTextBox`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-141">The code then takes one of the following actions depending on the value in the `InstructorOfficeTextBox` control:</span></span>

- <span data-ttu-id="c57c5-142">テキスト ボックスにある値とがあるかどうかはない`OfficeAssignment`を更新するエンティティが 1 つ作成されます。</span><span class="sxs-lookup"><span data-stu-id="c57c5-142">If the text box has a value and there's no `OfficeAssignment` entity to update, it creates one.</span></span>
- <span data-ttu-id="c57c5-143">テキスト ボックスにある値とがあるかどうか、 `OfficeAssignment` 、エンティティを更新、`Location`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="c57c5-143">If the text box has a value and there's an `OfficeAssignment` entity, it updates the `Location` property value.</span></span>
- <span data-ttu-id="c57c5-144">テキスト ボックスが空の場合、`OfficeAssignment`エンティティが存在すると、エンティティを削除します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-144">If the text box is empty and an `OfficeAssignment` entity exists, it deletes the entity.</span></span>

<span data-ttu-id="c57c5-145">その後、変更をデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-145">After this, it saves the changes to the database.</span></span> <span data-ttu-id="c57c5-146">例外が発生する場合は、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c57c5-146">If an exception occurs, it displays an error message.</span></span>

<span data-ttu-id="c57c5-147">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-147">Run the page.</span></span>

<span data-ttu-id="c57c5-148">[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-148">[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)</span></span>

<span data-ttu-id="c57c5-149">をクリックして**編集**し、すべてのフィールドがテキスト ボックスに変更します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-149">Click **Edit** and all fields change to text boxes.</span></span>

<span data-ttu-id="c57c5-150">[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-150">[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)</span></span>

<span data-ttu-id="c57c5-151">変更を含め、これらの値のいずれかの**オフィス割り当て**です。</span><span class="sxs-lookup"><span data-stu-id="c57c5-151">Change any of these values, including **Office Assignment**.</span></span> <span data-ttu-id="c57c5-152">をクリックして**更新**と一覧に反映された変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c57c5-152">Click **Update** and you'll see the changes reflected in the list.</span></span>

## <a name="displaying-related-entities-in-a-separate-control"></a><span data-ttu-id="c57c5-153">個別のコントロールに関連するエンティティを表示します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-153">Displaying Related Entities in a Separate Control</span></span>

<span data-ttu-id="c57c5-154">追加するため、各インストラクターは、1 つまたは複数のコースを教えることができます、`EntityDataSource`コントロールと`GridView`、インストラクターにどの講師が選択されている関連付けのコースを一覧表示するコントロール`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-154">Each instructor can teach one or more courses, so you'll add an `EntityDataSource` control and a `GridView` control to list the courses associated with whichever instructor is selected in the instructors `GridView` control.</span></span> <span data-ttu-id="c57c5-155">見出しを作成して、`EntityDataSource`コース エンティティの制御、エラー メッセージの間で、次のマークアップを追加`Label`コントロールと、終了`</div>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c57c5-155">To create a heading and the `EntityDataSource` control for courses entities, add the following markup between the error message `Label` control and the closing `</div>` tag:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

<span data-ttu-id="c57c5-156">`Where`パラメーターには値が含まれています、`PersonID`内の行が選択されている講師、`InstructorsGridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-156">The `Where` parameter contains the value of the `PersonID` of the instructor whose row is selected in the `InstructorsGridView` control.</span></span> <span data-ttu-id="c57c5-157">`Where`プロパティには、サブセレクト取得するコマンドすべての関連にはが含まれています`Person`からエンティティ、`Course`エンティティの`People`ナビゲーション プロパティを選択、`Course`エンティティ場合にのみ関連付けられているのいずれかの`Person`。エンティティが含まれていますが、選択した`PersonID`値。</span><span class="sxs-lookup"><span data-stu-id="c57c5-157">The `Where` property contains a subselect command that gets all associated `Person` entities from a `Course` entity's `People` navigation property and selects the `Course` entity only if one of the associated `Person` entities contains the selected `PersonID` value.</span></span>

<span data-ttu-id="c57c5-158">作成する、`GridView`コントロール。 引き続いてすぐに次のマークアップを追加、`CoursesEntityDataSource`コントロール (終了する前に`</div>`タグ)。</span><span class="sxs-lookup"><span data-stu-id="c57c5-158">To create the `GridView` control., add the following markup immediately following the `CoursesEntityDataSource` control (before the closing `</div>` tag):</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

<span data-ttu-id="c57c5-159">講師が選択されていない場合のコースは表示されませんので、`EmptyDataTemplate`要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c57c5-159">Because no courses will be displayed if no instructor is selected, an `EmptyDataTemplate` element is included.</span></span>

<span data-ttu-id="c57c5-160">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-160">Run the page.</span></span>

<span data-ttu-id="c57c5-161">[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-161">[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)</span></span>

<span data-ttu-id="c57c5-162">割り当てられると、1 つまたは複数のコースを持っているインストラクターを選択し、コースまたはコースが一覧に表示します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-162">Select an instructor who has one or more courses assigned, and the course or courses appear in the list.</span></span> <span data-ttu-id="c57c5-163">(注: データベース スキーマには、複数のコースが使用できますが、データベースに付属しているテスト データでインストラクター実際にはない複数のコースです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-163">(Note: although the database schema allows multiple courses, in the test data supplied with the database no instructor actually has more than one course.</span></span> <span data-ttu-id="c57c5-164">自分自身を追加できますコースにデータベースを使用して、**サーバー エクスプ ローラー**ウィンドウまたは*CoursesAdd.aspx*ページで、後のチュートリアルで追加します)。</span><span class="sxs-lookup"><span data-stu-id="c57c5-164">You can add courses to the database yourself using the **Server Explorer** window or the *CoursesAdd.aspx* page, which you'll add in a later tutorial.)</span></span>

<span data-ttu-id="c57c5-165">[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-165">[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)</span></span>

<span data-ttu-id="c57c5-166">`CoursesGridView`コントロールがいくつかのコース フィールドだけを表示します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-166">The `CoursesGridView` control shows only a few course fields.</span></span> <span data-ttu-id="c57c5-167">コースのすべての詳細を表示するには、使用する、`DetailsView`ユーザーが選択したコースのコントロールです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-167">To display all the details for a course, you'll use a `DetailsView` control for the course that the user selects.</span></span> <span data-ttu-id="c57c5-168">*Instructors.aspx*、終了後、次のマークアップを追加`</div>`タグ (このマークアップを配置するかどうかを確認**後**終了 div タグを前にない)。</span><span class="sxs-lookup"><span data-stu-id="c57c5-168">In *Instructors.aspx*, add the following markup after the closing `</div>` tag (make sure you place this markup **after** the closing div tag, not before it):</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

<span data-ttu-id="c57c5-169">このマークアップを作成、`EntityDataSource`コントロールにバインドされている、`Courses`エンティティ セット。</span><span class="sxs-lookup"><span data-stu-id="c57c5-169">This markup creates an `EntityDataSource` control that's bound to the `Courses` entity set.</span></span> <span data-ttu-id="c57c5-170">`Where`コースを使用して、プロパティの選択、`CourseID`コースで選択した行の値`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-170">The `Where` property selects a course using the `CourseID` value of the selected row in the courses `GridView` control.</span></span> <span data-ttu-id="c57c5-171">マークアップのハンドラーを指定する、`Selected`別のレベルが階層内の下位であるイベント、学生の成績を表示するため後で使用されます。</span><span class="sxs-lookup"><span data-stu-id="c57c5-171">The markup specifies a handler for the `Selected` event, which you'll use later for displaying student grades, which is another level lower in the hierarchy.</span></span>

<span data-ttu-id="c57c5-172">*Instructors.aspx.cs*、次のスタブを作成、`CourseDetailsEntityDataSource_Selected`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-172">In *Instructors.aspx.cs*, create the following stub for the `CourseDetailsEntityDataSource_Selected` method.</span></span> <span data-ttu-id="c57c5-173">(を記入するこのスタブ、チュートリアルの後半で、ここでは、必要なページがコンパイルされ実行できるようにします)。</span><span class="sxs-lookup"><span data-stu-id="c57c5-173">(You'll fill this stub out later in the tutorial; for now, you need it so that the page will compile and run.)</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

<span data-ttu-id="c57c5-174">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-174">Run the page.</span></span>

<span data-ttu-id="c57c5-175">[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-175">[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)</span></span>

<span data-ttu-id="c57c5-176">最初にはありません。 コースの詳細のためコースが選択されていません。</span><span class="sxs-lookup"><span data-stu-id="c57c5-176">Initially there are no course details because no course is selected.</span></span> <span data-ttu-id="c57c5-177">割り当てられている、コースを持っているインストラクターを選択し、詳細を表示するコースを選択します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-177">Select an instructor who has a course assigned, and then select a course to see the details.</span></span>

<span data-ttu-id="c57c5-178">[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-178">[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)</span></span>

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a><span data-ttu-id="c57c5-179">EntityDataSource を使用して「選択」関連データを表示するイベント</span><span class="sxs-lookup"><span data-stu-id="c57c5-179">Using the EntityDataSource "Selected" Event to Display Related Data</span></span>

<span data-ttu-id="c57c5-180">最後に、すべての登録済みの受講者と選択したコースの成績を表示します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-180">Finally, you want to show all of the enrolled students and their grades for the selected course.</span></span> <span data-ttu-id="c57c5-181">これを行うには、使用する、`Selected`のイベント、`EntityDataSource`コースにコントロールがバインドされている`DetailsView`です。</span><span class="sxs-lookup"><span data-stu-id="c57c5-181">To do this, you'll use the `Selected` event of the `EntityDataSource` control bound to the course `DetailsView`.</span></span>

<span data-ttu-id="c57c5-182">*Instructors.aspx*、後に、次のマークアップを追加、`DetailsView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-182">In *Instructors.aspx*, add the following markup after the `DetailsView` control:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

<span data-ttu-id="c57c5-183">このマークアップを作成、`ListView`受講者と選択したコースの成績の一覧を表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="c57c5-183">This markup creates a `ListView` control that displays a list of students and their grades for the selected course.</span></span> <span data-ttu-id="c57c5-184">データ ソースが指定されていないため、データ バインド コード内でコントロールがあります。</span><span class="sxs-lookup"><span data-stu-id="c57c5-184">No data source is specified because you'll databind the control in code.</span></span> <span data-ttu-id="c57c5-185">`EmptyDataTemplate`要素はコースが選択されていないときに表示されるメッセージを表示、その場合は、表示する、受講者はありません。</span><span class="sxs-lookup"><span data-stu-id="c57c5-185">The `EmptyDataTemplate` element provides a message to display when no course is selected—in that case, there are no students to display.</span></span> <span data-ttu-id="c57c5-186">`LayoutTemplate`要素の一覧を表示する HTML テーブルが作成され、`ItemTemplate`を表示する列を指定します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-186">The `LayoutTemplate` element creates an HTML table to display the list, and the `ItemTemplate` specifies the columns to display.</span></span> <span data-ttu-id="c57c5-187">学生 ID と学生の成績、`StudentGrade`からは、エンティティ、および、受講者名、 `Person` Entity Framework での使用可能なエンティティ、`Person`のナビゲーション プロパティ、`StudentGrade`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="c57c5-187">The student ID and the student grade are from the `StudentGrade` entity, and the student name is from the `Person` entity that the Entity Framework makes available in the `Person` navigation property of the `StudentGrade` entity.</span></span>

<span data-ttu-id="c57c5-188">*Instructors.aspx.cs*、スタブを置換`CourseDetailsEntityDataSource_Selected`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="c57c5-188">In *Instructors.aspx.cs*, replace the stubbed-out `CourseDetailsEntityDataSource_Selected` method with the following code:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

<span data-ttu-id="c57c5-189">このイベントのイベント引数は 0 個の項目が選択されていない場合、またはを持つ 1 つの項目コレクションの形式で選択したデータを提供する場合、`Course`エンティティが選択されています。</span><span class="sxs-lookup"><span data-stu-id="c57c5-189">The event argument for this event provides the selected data in the form of a collection, which will have zero items if nothing is selected or one item if a `Course` entity is selected.</span></span> <span data-ttu-id="c57c5-190">場合、`Course`エンティティが選択されている場合、コードを使用して、`First`コレクションを 1 つのオブジェクトに変換します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-190">If a `Course` entity is selected, the code uses the `First` method to convert the collection to a single object.</span></span> <span data-ttu-id="c57c5-191">取得し、`StudentGrade`ナビゲーション プロパティからのエンティティが、コレクションに変換し、バインド、`GradesListView`コレクションを制御します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-191">It then gets `StudentGrade` entities from the navigation property, converts them to a collection, and binds the `GradesListView` control to the collection.</span></span>

<span data-ttu-id="c57c5-192">これを表示するグレードが初めて、ページが表示されますが、空のデータ テンプレート内のメッセージに表示されることを確認するための十分なは、コースが選択されていないときにします。</span><span class="sxs-lookup"><span data-stu-id="c57c5-192">This is sufficient to display grades, but you want to make sure that the message in the empty data template is displayed the first time the page is displayed and whenever a course is not selected.</span></span> <span data-ttu-id="c57c5-193">実行するには、2 つの場所から呼び出すことになります、次の方法を作成します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-193">To do that, create the following method, which you'll call from two places:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

<span data-ttu-id="c57c5-194">この新しいメソッドを呼び出して、`Page_Load`ページが表示される空のデータ テンプレートの最初の時刻を表示するメソッド。</span><span class="sxs-lookup"><span data-stu-id="c57c5-194">Call this new method from the `Page_Load` method to display the empty data template the first time the page is displayed.</span></span> <span data-ttu-id="c57c5-195">それを呼び出すと、`InstructorsGridView_SelectedIndexChanged`メソッドそのイベントが発生した場合に、インストラクターを選択すると、つまり、新しいコースに読み込まれるコース`GridView`コントロールおよび none がまだ選択されていません。</span><span class="sxs-lookup"><span data-stu-id="c57c5-195">And call it from the `InstructorsGridView_SelectedIndexChanged` method because that event is raised when an instructor is selected, which means new courses are loaded into the courses `GridView` control and none is selected yet.</span></span> <span data-ttu-id="c57c5-196">2 つの呼び出しを次に示します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-196">Here are the two calls:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

<span data-ttu-id="c57c5-197">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-197">Run the page.</span></span>

<span data-ttu-id="c57c5-198">[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-198">[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)</span></span>

<span data-ttu-id="c57c5-199">割り当てられている、コースのあるインストラクターを選択し、コースを選択します。</span><span class="sxs-lookup"><span data-stu-id="c57c5-199">Select an instructor that has a course assigned, and then select the course.</span></span>

<span data-ttu-id="c57c5-200">[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="c57c5-200">[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)</span></span>

<span data-ttu-id="c57c5-201">関連するデータを操作する方法はいくつか見てきました。</span><span class="sxs-lookup"><span data-stu-id="c57c5-201">You have now seen a few ways to work with related data.</span></span> <span data-ttu-id="c57c5-202">次のチュートリアルでは、既存のエンティティ間のリレーションシップを追加する方法が学習を既存のエンティティへのリレーションシップを持つ新しいエンティティを追加する方法と、リレーションシップを削除する方法です。</span><span class="sxs-lookup"><span data-stu-id="c57c5-202">In the following tutorial, you'll learn how to add relationships between existing entities, how to remove relationships, and how to add a new entity that has a relationship to an existing entity.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c57c5-203">[前へ](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="c57c5-203">[Previous](the-entity-framework-and-aspnet-getting-started-part-3.md)
[Next](the-entity-framework-and-aspnet-getting-started-part-5.md)</span></span>
