---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズでは、Entity Framework 4.0 のチュートリアル シリーズの概要を作成した Contoso University web アプリケーションに基づいています。 ここには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378911"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="0c5cc-104">Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理</span><span class="sxs-lookup"><span data-stu-id="0c5cc-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="0c5cc-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0c5cc-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="0c5cc-106">このチュートリアル シリーズは、Contoso University web アプリケーションによって作成される、 [、Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)チュートリアル シリーズです。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="0c5cc-107">前のチュートリアルを完了していない場合は、このチュートリアルの開始点としてできます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)に、作成します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="0c5cc-108">できます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完全なチュートリアル シリーズで作成します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="0c5cc-109">チュートリアルについて質問等がございましたらを投稿できます、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="0c5cc-110">前のチュートリアルでは、Entity Framework を使用する n 層 web アプリケーションでリポジトリ パターンを実装し、`ObjectDataSource`コントロール。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="0c5cc-111">このチュートリアルでは、並べ替えとフィルター処理を実行してマスター/詳細のシナリオを処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="0c5cc-112">次の機能強化を追加します、 *Departments.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="0c5cc-113">部門名を選択するためのテキスト ボックスです。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="0c5cc-114">グリッドに表示されている各部門のコースのリスト。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="0c5cc-115">列見出しをクリックしてソートする権限です。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="0c5cc-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0c5cc-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="0c5cc-117">GridView の列の並べ替えに機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="0c5cc-118">開く、 *Departments.aspx*ページし、追加、`SortParameterName="sortExpression"`属性を`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="0c5cc-119">(後でに作成、`GetDepartments`という名前のパラメーターを受け取るメソッド`sortExpression`)。コントロールの開始タグのマークアップには、次の例では、今すぐに似ています。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="0c5cc-120">追加、`AllowSorting="true"`属性の開始タグを`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="0c5cc-121">コントロールの開始タグのマークアップには、次の例では、今すぐに似ています。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="0c5cc-122">*Departments.aspx.cs*、呼び出すことによって、既定の並べ替え順序を設定、`GridView`コントロールの`Sort`からメソッド、`Page_Load`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="0c5cc-123">ビジネス ロジック クラスまたはリポジトリ クラスのいずれかでは、並べ替え、またはフィルター処理するコードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="0c5cc-124">場合に、ビジネス ロジック クラスでは、並べ替えまたはフィルター作業に、データベースからデータが取得された後、ビジネス ロジック クラスを使用するため、`IEnumerable`リポジトリによって返されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="0c5cc-125">並べ替えとフィルタ リングのリポジトリ クラスにコードを追加して、LINQ 式の前に行うまたはオブジェクト クエリに変換された場合、`IEnumerable`オブジェクトをコマンドは、これは通常より効率的な処理には、データベースに渡されるされます。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="0c5cc-126">このチュートリアルでは、並べ替えと、データベースで行われる処理を原因となる方法でフィルター処理を実装します-つまり、リポジトリにします。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="0c5cc-127">並べ替え機能を追加するには、リポジトリ インターフェイスおよびリポジトリ クラスにも、ビジネス ロジック クラスに新しいメソッドを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="0c5cc-128">*ISchoolRepository.cs*ファイルに追加し、新しい`GetDepartments`を受け取るメソッドを`sortExpression`部門の返される一覧の並べ替えに使用されるパラメーター。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="0c5cc-129">`sortExpression`パラメーターには、並べ替えに使用する列と並べ替えの方向を指定します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="0c5cc-130">新しいメソッドのコードを追加、 *SchoolRepository.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="0c5cc-131">既存のパラメーターのない変更`GetDepartments`に新しいメソッドを呼び出すメソッド。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="0c5cc-132">次の新しいメソッドを追加、テスト プロジェクトで*MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c5cc-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="0c5cc-133">並べ替えられた一覧を返すこのメソッドに依存する任意の単体テストを作成する場合は、一覧を返す前に並べ替える必要があります。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="0c5cc-134">しません作成することにテストそのようなこのチュートリアルでは並べ替えられていない部門の一覧を返すことが、メソッド、します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="0c5cc-135">*SchoolBL.cs*ファイルに、ビジネス ロジック クラスに次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="0c5cc-136">このコードは、リポジトリ メソッドに並べ替えパラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="0c5cc-137">実行、 *Departments.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="0c5cc-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0c5cc-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="0c5cc-139">その列を並べ替えるには任意の列見出しをクリックすることができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="0c5cc-140">列が既に並べ替えられている場合、見出しをクリックすると、並べ替えの方向を反転させます。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="0c5cc-141">検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-141">Adding a Search Box</span></span>

<span data-ttu-id="0c5cc-142">このセクションでは、検索テキスト ボックスを追加、リンク、`ObjectDataSource`コントロール パラメーターを使用して制御し、フィルター処理をサポートするためにビジネス ロジック クラスにメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="0c5cc-143">開く、 *Departments.aspx*ページ、見出しと 1 つ目の間は、次のマークアップを追加および`ObjectDataSource`コントロール。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="0c5cc-144">`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="0c5cc-145">追加、`SelectParameters`という名前のパラメーター要素`nameSearchString`に入力された値を取得する、`SearchTextBox`コントロール。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="0c5cc-146">変更、`SelectMethod`属性に値`GetDepartmentsByName`します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="0c5cc-147">(作成するメソッドのこの後でします。)</span><span class="sxs-lookup"><span data-stu-id="0c5cc-147">(You'll create this method later.)</span></span>

<span data-ttu-id="0c5cc-148">マークアップを`ObjectDataSource`コントロール次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="0c5cc-149">*ISchoolRepository.cs*、追加、`GetDepartmentsByName`両方を受け取るメソッド`sortExpression`と`nameSearchString`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="0c5cc-150">*SchoolRepository.cs*、次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="0c5cc-151">このコードを使用して、`Where`検索文字列が含まれている項目を選択するメソッド。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="0c5cc-152">検索文字列が空の場合は、すべてのレコードが選択されます。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="0c5cc-153">このような 1 つのステートメントでまとめてメソッドの呼び出しを指定する場合は注意 (`Include`、し`OrderBy`、し`Where`)、`Where`メソッドを常に最後にある必要があります。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="0c5cc-154">既存の変更`GetDepartments`を受け取るメソッドを`sortExpression`新しいメソッドを呼び出すパラメーター。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="0c5cc-155">*MockSchoolRepository.cs*テスト プロジェクトで、次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="0c5cc-156">*SchoolBL.cs*、次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="0c5cc-157">実行、 *Departments.aspx*ページし、選択ロジックが機能するかどうかを確認する検索文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="0c5cc-158">テキスト ボックスを空のままにして、すべてのレコードが返されることを確認するために検索を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="0c5cc-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="0c5cc-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="0c5cc-160">グリッドの行ごとの詳細の列を追加します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="0c5cc-161">次に、すべてのグリッドの右側のセルに表示される各部門のコースを表示します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="0c5cc-162">これを行うには、使用する、入れ子になった`GridView`コントロールとデータ バインドからのデータを`Courses`のナビゲーション プロパティ、`Department`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="0c5cc-163">開いている*Departments.aspx*のマークアップで、`GridView`を制御するハンドラーを指定の`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="0c5cc-164">コントロールの開始タグのマークアップには、次の例では、今すぐに似ています。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="0c5cc-165">新しい追加`TemplateField`要素の後に、`Administrator`テンプレート フィールド。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="0c5cc-166">このマークアップを作成、入れ子になった`GridView`コース番号とコースのリストのタイトルを表示するコントロールです。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="0c5cc-167">データ バインドがわかるために、データ ソースは指定しません内のコードで、`RowDataBound`ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="0c5cc-168">開いている*Departments.aspx.cs*の次のハンドラーを追加し、`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="0c5cc-169">このコードを取得、`Department`イベントの引数からのエンティティに変換します、`Courses`ナビゲーション プロパティを`List`コレクション、および入れ子になったして`GridView`コレクションにします。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="0c5cc-170">開く、 *SchoolRepository.cs*ファイルし、一括読み込みを指定、`Courses`ナビゲーション プロパティを呼び出して、`Include`メソッドで作成したオブジェクトのクエリで、`GetDepartmentsByName`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="0c5cc-171">`return`内のステートメント、`GetDepartmentsByName`メソッド例では、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="0c5cc-172">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-172">Run the page.</span></span> <span data-ttu-id="0c5cc-173">GridView コントロールには、並べ替えとフィルター処理する前に追加する機能、だけでなく、各部門の入れ子になったコースの詳細に表示されます。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="0c5cc-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0c5cc-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="0c5cc-175">これにより、並べ替え、フィルター処理、およびマスター詳細シナリオの概要が完了します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="0c5cc-176">次のチュートリアルでは、同時実行を処理する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="0c5cc-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c5cc-177">[前へ](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="0c5cc-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
