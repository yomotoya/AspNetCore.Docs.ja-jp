---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: クエリ文字列の値を使用してモデル バインディングでのデータをフィルター処理し、web フォーム |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="15ab4-104">モデル バインディングと web フォームをデータのフィルター選択するクエリ文字列の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="15ab4-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="15ab4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="15ab4-106">このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="15ab4-107">モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。</span><span class="sxs-lookup"><span data-stu-id="15ab4-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="15ab4-108">この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。</span><span class="sxs-lookup"><span data-stu-id="15ab4-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="15ab4-109">このチュートリアルでは、クエリ文字列の値を渡すし、その値を使用して、モデル バインディングからデータを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="15ab4-110">このチュートリアルで作成されたプロジェクトで、[以前](retrieving-data.md)シリーズの一部です。</span><span class="sxs-lookup"><span data-stu-id="15ab4-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="15ab4-111">実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。</span><span class="sxs-lookup"><span data-stu-id="15ab4-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="15ab4-112">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="15ab4-113">これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="15ab4-114">新機能のビルド</span><span class="sxs-lookup"><span data-stu-id="15ab4-114">What you'll build</span></span>

<span data-ttu-id="15ab4-115">このチュートリアルでは、次の手順を。</span><span class="sxs-lookup"><span data-stu-id="15ab4-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="15ab4-116">登録済みのコースを受講者に表示するのに新しいページを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="15ab4-117">登録済みのコースをクエリ文字列の値に基づいて、選択したスチューデントの取得します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="15ab4-118">新しいページに、グリッド ビューからクエリ文字列の値を持つハイパーリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="15ab4-119">このチュートリアルの手順はどのようなにほぼ一致には、前述[チュートリアル](sorting-paging-and-filtering-data.md)をドロップダウン リストでユーザーの選択に基づいて表示される受講者をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="15ab4-120">そのチュートリアルでは使用して、**コントロール**コントロールから、パラメーターの値を取得する select メソッドの属性です。</span><span class="sxs-lookup"><span data-stu-id="15ab4-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="15ab4-121">このチュートリアルで使用する、 **QueryString**クエリ文字列からパラメーターの値を取得する select メソッドの属性です。</span><span class="sxs-lookup"><span data-stu-id="15ab4-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="15ab4-122">スチューデントのコースを表示するための新しいページを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="15ab4-123">Site.master のマスター ページを使用する新しい web フォームを追加し、ページの名前を付けます**コース**です。</span><span class="sxs-lookup"><span data-stu-id="15ab4-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="15ab4-124">**Courses.aspx**ファイルに、選択した受講者コースを表示するグリッド ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="15ab4-125">Select メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-125">Define the select method</span></span>

<span data-ttu-id="15ab4-126">**Courses.aspx.cs**、グリッド ビューの指定した名前の選択メソッドを追加するは**SelectMethod**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="15ab4-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="15ab4-127">このメソッドには、スチューデントのコースを取得するためにクエリを定義し、パラメーターと同じ名前を持つクエリ文字列値から、パラメーターを取得するを指定します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="15ab4-128">最初に、次を追加する必要があります**を使用して**ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="15ab4-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="15ab4-129">その後、Courses.aspx.cs に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="15ab4-130">クエリ文字列の属性は、StudentID をという名前のクエリ文字列値がこのメソッドのパラメーターに自動的に割り当てられていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="15ab4-131">クエリ文字列の値を持つハイパーリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="15ab4-132">Students.aspx で、グリッド ビューでは、新しいコース ページにリンクするハイパーリンク フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="15ab4-133">ハイパーリンクは、スチューデントの id を持つクエリ文字列値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="15ab4-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="15ab4-134">Students.aspx では、合計のクレジットの次のフィールドをフィールドのすぐ下のグリッド ビューの列に追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="15ab4-135">アプリケーションを実行し、グリッド ビューに今すぐコースのリンクが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![ハイパーリンクを追加します。](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="15ab4-137">クリックすると、リンクのいずれか、そのスチューデントの登録済みのコースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="15ab4-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![コースを表示します。](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="15ab4-139">まとめ</span><span class="sxs-lookup"><span data-stu-id="15ab4-139">Conclusion</span></span>

<span data-ttu-id="15ab4-140">このチュートリアルでは、クエリ文字列の値を持つリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="15ab4-141">Select メソッドのパラメーター値には、そのクエリ文字列の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="15ab4-142">次の[チュートリアル](adding-business-logic-layer.md)、ビジネス ロジック層とデータ アクセス層に分離コード ファイルからコードを移動します。</span><span class="sxs-lookup"><span data-stu-id="15ab4-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="15ab4-143">[前へ](integrating-jquery-ui.md)
> [次へ](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="15ab4-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
