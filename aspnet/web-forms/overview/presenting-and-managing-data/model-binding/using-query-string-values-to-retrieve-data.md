---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: クエリ文字列の値を使用してモデル バインドでデータをフィルター処理し、web フォーム |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: fc4ec64cf583f25eca6795f7ff9215f025170054
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828285"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="4a0b6-104">モデル バインディングと web フォームでデータをフィルター処理クエリ文字列の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="4a0b6-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4a0b6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4a0b6-106">このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4a0b6-107">モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4a0b6-108">このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4a0b6-109">このチュートリアルでは、クエリ文字列の値を渡すし、その値を使用してモデル バインドでデータを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="4a0b6-110">このチュートリアルで作成したプロジェクトで、[以前](retrieving-data.md)系列の部分。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="4a0b6-111">できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト</span><span class="sxs-lookup"><span data-stu-id="4a0b6-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4a0b6-112">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4a0b6-113">これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="4a0b6-114">構築します</span><span class="sxs-lookup"><span data-stu-id="4a0b6-114">What you'll build</span></span>

<span data-ttu-id="4a0b6-115">このチュートリアルではあります。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4a0b6-116">学生の登録済みのコースを表示する新しいページを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="4a0b6-117">クエリ文字列の値に基づいて選択した学生の登録済みのコースを取得します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="4a0b6-118">新しいページに、グリッド ビューからクエリ文字列の値を持つハイパーリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="4a0b6-119">このチュートリアルの手順ではどのようなにかなり似ています、以前に[チュートリアル](sorting-paging-and-filtering-data.md)をドロップダウン リストでユーザーの選択に基づいて表示されている受講者をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="4a0b6-120">このチュートリアルで使用して、**コントロール**select メソッド パラメーターの値がコントロールから取得されるかを指定する属性。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="4a0b6-121">このチュートリアルで使用する、 **QueryString**クエリ文字列からパラメーターの値を取得することを指定する select メソッドの属性。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="4a0b6-122">受講者のコースを表示するための新しいページを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="4a0b6-123">Site.master のマスター ページを使用する新しい web フォームを追加し、ページの名前**コース**します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="4a0b6-124">**Courses.aspx**ファイルを選択した受講者コースを表示するグリッド ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="4a0b6-125">Select メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-125">Define the select method</span></span>

<span data-ttu-id="4a0b6-126">**Courses.aspx.cs**、グリッド ビューの指定した名前を持つ select メソッドを追加するは**SelectMethod**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="4a0b6-127">メソッドには、受講者のコースでは、取得するためのクエリを定義し、パラメーターがパラメーターと同じ名前のクエリ文字列値から取得されるかを指定します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="4a0b6-128">最初に、次を追加する必要があります**を使用して**ステートメント。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="4a0b6-129">次に、Courses.aspx.cs に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="4a0b6-130">クエリ文字列の属性は、このメソッドのパラメーターに StudentID という名前のクエリ文字列値が自動的に割り当てられているを意味します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="4a0b6-131">クエリ文字列の値を持つハイパーリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="4a0b6-132">Students.aspx で、グリッド ビューは、新しいコースのページにリンクするハイパーリンクのフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="4a0b6-133">ハイパーリンクは、学生の id を持つクエリ文字列の値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="4a0b6-134">Students.aspx では、クレジットの合計を次のフィールドをフィールドのすぐ下のグリッド ビューの列に追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="4a0b6-135">アプリケーションを実行し、グリッド ビューに今すぐコースのリンクが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![ハイパーリンクを追加します。](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="4a0b6-137">いずれかのリンクをクリックすると、学生の登録済みのコースを確認します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![コースを表示します。](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="4a0b6-139">まとめ</span><span class="sxs-lookup"><span data-stu-id="4a0b6-139">Conclusion</span></span>

<span data-ttu-id="4a0b6-140">このチュートリアルでは、クエリ文字列の値を持つリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="4a0b6-141">Select メソッドのパラメーター値には、そのクエリ文字列の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="4a0b6-142">次の[チュートリアル](adding-business-logic-layer.md)、ビジネス ロジック層とデータ アクセス層に分離コード ファイルからコードを移動します。</span><span class="sxs-lookup"><span data-stu-id="4a0b6-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4a0b6-143">[前へ](integrating-jquery-ui.md)
> [次へ](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="4a0b6-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
