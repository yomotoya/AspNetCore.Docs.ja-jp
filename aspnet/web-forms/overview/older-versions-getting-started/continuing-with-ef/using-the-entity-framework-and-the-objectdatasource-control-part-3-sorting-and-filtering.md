---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、Entity Framework 4.0 チュートリアル シリーズの概要を作成した Contoso 大学 web アプリケーションに基づいています。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887656"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="8792c-104">Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理</span><span class="sxs-lookup"><span data-stu-id="8792c-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="8792c-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8792c-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="8792c-106">このチュートリアル シリーズのビルドによって作成される Contoso 大学 web アプリケーションで、 [Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)一連のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="8792c-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="8792c-107">前のチュートリアルを完了していない場合このチュートリアルの開始点とすることができます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)作成したとします。</span><span class="sxs-lookup"><span data-stu-id="8792c-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="8792c-108">こともできます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)一連の完全なチュートリアルで作成します。</span><span class="sxs-lookup"><span data-stu-id="8792c-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="8792c-109">チュートリアルについて質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="8792c-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="8792c-110">前のチュートリアルでは、Entity Framework を使用する n 層の web アプリケーションでリポジトリ パターンを実装し、`ObjectDataSource`コントロール。</span><span class="sxs-lookup"><span data-stu-id="8792c-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="8792c-111">このチュートリアルでは、並べ替えとフィルター処理を行うし、マスター/詳細シナリオを処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8792c-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="8792c-112">次の拡張機能を追加、 *Departments.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="8792c-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="8792c-113">部門名でを選択できるようにするテキスト ボックスです。</span><span class="sxs-lookup"><span data-stu-id="8792c-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="8792c-114">グリッドに表示される各部門のコースの一覧です。</span><span class="sxs-lookup"><span data-stu-id="8792c-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="8792c-115">列見出しをクリックして並べ替えることです。</span><span class="sxs-lookup"><span data-stu-id="8792c-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="8792c-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8792c-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="8792c-117">GridView 列の並べ替えに機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="8792c-118">開く、 *Departments.aspx*ページし、追加、`SortParameterName="sortExpression"`属性を`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`です。</span><span class="sxs-lookup"><span data-stu-id="8792c-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="8792c-119">(を作成した後で、`GetDepartments`をという名前のパラメーターを受け取るメソッド`sortExpression`)。コントロールの開始タグのマークアップには、次の例がようになります。</span><span class="sxs-lookup"><span data-stu-id="8792c-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="8792c-120">追加、`AllowSorting="true"`属性の開始タグを`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="8792c-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="8792c-121">コントロールの開始タグのマークアップには、次の例がようになります。</span><span class="sxs-lookup"><span data-stu-id="8792c-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="8792c-122">*Departments.aspx.cs*、呼び出すことによって既定の並べ替え順序を設定、`GridView`コントロールの`Sort`メソッドから、`Page_Load`メソッド。</span><span class="sxs-lookup"><span data-stu-id="8792c-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="8792c-123">ビジネス ロジックのクラスまたはリポジトリ クラスのいずれかで、並べ替えまたはフィルター処理するコードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="8792c-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="8792c-124">場合は、ビジネス ロジックのクラスで、並べ替えまたは作業をフィルター処理を行いますデータがデータベースから取得された後、ビジネス ロジックのクラスを使用するため、`IEnumerable`リポジトリから返されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8792c-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="8792c-125">並べ替えとフィルター処理、リポジトリ クラス内のコードを追加して LINQ の式より前に行うまたはオブジェクト クエリに変換された場合、`IEnumerable`オブジェクトのコマンドをこれは通常より効率的な処理のため、データベースに渡されます。</span><span class="sxs-lookup"><span data-stu-id="8792c-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="8792c-126">このチュートリアルでは並べ替えと、データベースで行われる処理の原因となる方法でフィルター処理を実装します-つまり、リポジトリにします。</span><span class="sxs-lookup"><span data-stu-id="8792c-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="8792c-127">並べ替え機能を追加するには、リポジトリ インターフェイスおよびリポジトリ クラスもビジネス ロジックのクラスに新しいメソッドを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8792c-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="8792c-128">*ISchoolRepository.cs*ファイルに追加し、新しい`GetDepartments`を受け取るメソッド、`sortExpression`返される部門の一覧の並べ替えに使用されるパラメーター。</span><span class="sxs-lookup"><span data-stu-id="8792c-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="8792c-129">`sortExpression`パラメーターには、並べ替えに使用する列と並べ替えの方向を指定します。</span><span class="sxs-lookup"><span data-stu-id="8792c-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="8792c-130">新しいメソッドのコードを追加、 *SchoolRepository.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8792c-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="8792c-131">既存のパラメーターなしの変更`GetDepartments`に新しいメソッドを呼び出すメソッド。</span><span class="sxs-lookup"><span data-stu-id="8792c-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="8792c-132">テスト プロジェクト内に次の新しいメソッドを追加*MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="8792c-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="8792c-133">並べ替えられた一覧を返すこのメソッドに依存する他の単体テストを作成する場合を返す前にリストを並べ替える必要があります。</span><span class="sxs-lookup"><span data-stu-id="8792c-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="8792c-134">しないに作成するテストのようこのチュートリアルでは、メソッドは、部門の並べ替えられていないリストを返すだけできるようです。</span><span class="sxs-lookup"><span data-stu-id="8792c-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="8792c-135">*SchoolBL.cs*ファイルにビジネス ロジックのクラスに次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="8792c-136">このコードは、リポジトリのメソッドに並べ替えのパラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="8792c-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="8792c-137">実行、 *Departments.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="8792c-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="8792c-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8792c-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="8792c-139">今すぐその列で並べ替えを行う任意の列見出しをクリックできます。</span><span class="sxs-lookup"><span data-stu-id="8792c-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="8792c-140">列が既に並べ替えられて場合、並べ替えの方向を反転の見出しをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8792c-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="8792c-141">検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-141">Adding a Search Box</span></span>

<span data-ttu-id="8792c-142">このセクションではあります検索テキスト ボックスを追加、リンク、`ObjectDataSource`コントロール パラメーターの使用を制御し、フィルター処理をサポートするためにビジネス ロジックのクラスにメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="8792c-143">開く、 *Departments.aspx*  ページ、見出しと、最初の間で、次のマークアップを追加して`ObjectDataSource`コントロール。</span><span class="sxs-lookup"><span data-stu-id="8792c-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="8792c-144">`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="8792c-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="8792c-145">追加、`SelectParameters`という名前のパラメーターの要素`nameSearchString`に入力された値を取得する、`SearchTextBox`コントロール。</span><span class="sxs-lookup"><span data-stu-id="8792c-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="8792c-146">変更、`SelectMethod`属性に値`GetDepartmentsByName`です。</span><span class="sxs-lookup"><span data-stu-id="8792c-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="8792c-147">(ありますメソッドを作成するこの後でします。)</span><span class="sxs-lookup"><span data-stu-id="8792c-147">(You'll create this method later.)</span></span>

<span data-ttu-id="8792c-148">マークアップ、`ObjectDataSource`コントロール次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="8792c-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="8792c-149">*ISchoolRepository.cs*、追加、`GetDepartmentsByName`を両方を受け取るメソッド`sortExpression`と`nameSearchString`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="8792c-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="8792c-150">*SchoolRepository.cs*、次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="8792c-151">このコードを使用して、`Where`メソッドを検索文字列が含まれる項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="8792c-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="8792c-152">検索文字列が空の場合は、すべてのレコードが選択されます。</span><span class="sxs-lookup"><span data-stu-id="8792c-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="8792c-153">メソッドは次のように 1 つのステートメントで一緒に呼び出すかを指定するときに注意してください (`Include`、し`OrderBy`、し`Where`) では、`Where`メソッドを常に最後にある必要があります。</span><span class="sxs-lookup"><span data-stu-id="8792c-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="8792c-154">既存の変更`GetDepartments`を受け取るメソッド、`sortExpression`新しいメソッドを呼び出すパラメーター。</span><span class="sxs-lookup"><span data-stu-id="8792c-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="8792c-155">*MockSchoolRepository.cs*テスト プロジェクトで、次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="8792c-156">*SchoolBL.cs*、次の新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="8792c-157">実行、 *Departments.aspx*ページし、選択するロジックが動作するかどうかを確認する検索文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="8792c-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="8792c-158">テキスト ボックスを空のままにして、すべてのレコードが返されるかどうかを確認するために検索を再試行してください。</span><span class="sxs-lookup"><span data-stu-id="8792c-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="8792c-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8792c-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="8792c-160">グリッドの行ごとに詳細列を追加します。</span><span class="sxs-lookup"><span data-stu-id="8792c-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="8792c-161">次に、すべてのグリッドの右側のセルに表示される各部門のコースを表示します。</span><span class="sxs-lookup"><span data-stu-id="8792c-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="8792c-162">これを行うには、使用する、入れ子になった`GridView`コントロールとデータ バインドからのデータを`Courses`のナビゲーション プロパティ、`Department`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="8792c-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="8792c-163">開いている*Departments.aspx*およびのマークアップで、`GridView`を制御するハンドラーを指定の`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="8792c-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="8792c-164">コントロールの開始タグのマークアップには、次の例がようになります。</span><span class="sxs-lookup"><span data-stu-id="8792c-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="8792c-165">新しい`TemplateField`要素の後に、`Administrator`テンプレート フィールド。</span><span class="sxs-lookup"><span data-stu-id="8792c-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="8792c-166">このマークアップを作成、入れ子になった`GridView`コース番号とタイトルのコースの一覧を表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="8792c-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="8792c-167">データ バインドがわかるために、データ ソースは指定しません内のコードで、`RowDataBound`ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="8792c-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="8792c-168">開いている*Departments.aspx.cs*の次のハンドラーを追加し、`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="8792c-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="8792c-169">このコードを取得、`Department`イベントの引数からのエンティティに変換、`Courses`へのナビゲーション プロパティ、`List`コレクション、および入れ子になったして`GridView`をコレクションにします。</span><span class="sxs-lookup"><span data-stu-id="8792c-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="8792c-170">開く、 *SchoolRepository.cs*ファイルし、の一括読み込みの指定、`Courses`ナビゲーション プロパティを呼び出して、`Include`メソッドで作成したオブジェクトのクエリで、`GetDepartmentsByName`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8792c-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="8792c-171">`return`内のステートメント、`GetDepartmentsByName`メソッド次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="8792c-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="8792c-172">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="8792c-172">Run the page.</span></span> <span data-ttu-id="8792c-173">だけでなく、並べ替えとフィルター処理前に追加した機能、GridView コントロールは今すぐ部門ごとに入れ子になったコースの詳細を示します。</span><span class="sxs-lookup"><span data-stu-id="8792c-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="8792c-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8792c-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="8792c-175">これは、並べ替え、フィルター、およびマスター/詳細シナリオの概要を完了します。</span><span class="sxs-lookup"><span data-stu-id="8792c-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="8792c-176">次のチュートリアルでは、同時実行の処理方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8792c-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8792c-177">[前へ](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="8792c-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
