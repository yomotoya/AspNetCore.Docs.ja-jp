---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: 並べ替え、ページング、およびモデルのバインディングと web フォームを使用してデータをフィルター処理 |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 7529c811e6196327094f8f735de1bd65be76ee3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398308"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="df285-104">並べ替え、ページング、およびモデルのバインディングと web フォームを使用してデータをフィルター処理</span><span class="sxs-lookup"><span data-stu-id="df285-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="df285-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="df285-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="df285-106">このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="df285-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="df285-107">モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="df285-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="df285-108">このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。</span><span class="sxs-lookup"><span data-stu-id="df285-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="df285-109">このチュートリアルでは、並べ替え、ページング、およびモデル バインドによるデータのフィルター処理を追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="df285-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="df285-110">このチュートリアルは、最初に作成されたプロジェクトでビルド[一部](retrieving-data.md)シリーズの。</span><span class="sxs-lookup"><span data-stu-id="df285-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="df285-111">できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト</span><span class="sxs-lookup"><span data-stu-id="df285-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="df285-112">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="df285-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="df285-113">これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="df285-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="df285-114">構築します</span><span class="sxs-lookup"><span data-stu-id="df285-114">What you'll build</span></span>

<span data-ttu-id="df285-115">このチュートリアルではあります。</span><span class="sxs-lookup"><span data-stu-id="df285-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="df285-116">並べ替えとデータのページングを有効にします。</span><span class="sxs-lookup"><span data-stu-id="df285-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="df285-117">ユーザーは選択に基づくデータのフィルター処理を有効にします。</span><span class="sxs-lookup"><span data-stu-id="df285-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="df285-118">並べ替えを追加します。</span><span class="sxs-lookup"><span data-stu-id="df285-118">Add sorting</span></span>

<span data-ttu-id="df285-119">GridView の並べ替え機能を有効にすることは非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="df285-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="df285-120">Student.aspx ファイル内の設定だけ**AllowSorting**に**true** GridView にします。</span><span class="sxs-lookup"><span data-stu-id="df285-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="df285-121">設定する必要はありません、 **SortExpression** DataField は自動的に使用されるため、各列の値します。</span><span class="sxs-lookup"><span data-stu-id="df285-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="df285-122">GridView は選択した値で、データの並べ替えを含めるようにクエリを変更します。</span><span class="sxs-lookup"><span data-stu-id="df285-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="df285-123">次の強調表示されたコードでは、並べ替えを有効にするために必要な追加を示します。</span><span class="sxs-lookup"><span data-stu-id="df285-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="df285-124">Web アプリケーションを実行し、別の列に値によって並べ替え生徒の記録をテストします。</span><span class="sxs-lookup"><span data-stu-id="df285-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![並べ替えの受講者](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="df285-126">ページングを追加します。</span><span class="sxs-lookup"><span data-stu-id="df285-126">Add paging</span></span>

<span data-ttu-id="df285-127">ページングを有効にも非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="df285-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="df285-128">Gridview では、設定、 **AllowPaging**プロパティを**true**設定と、 **PageSize**プロパティに各ページに表示するレコードの数。</span><span class="sxs-lookup"><span data-stu-id="df285-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="df285-129">このチュートリアルで 4 に設定できます。</span><span class="sxs-lookup"><span data-stu-id="df285-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="df285-130">Web アプリケーションを実行して、レコードは、1 ページに表示される 4 個のレコードに複数のページに分かれています。 これを注意してください。</span><span class="sxs-lookup"><span data-stu-id="df285-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![ページングを追加します。](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="df285-132">クエリの遅延実行では、アプリケーションの効率が向上します。</span><span class="sxs-lookup"><span data-stu-id="df285-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="df285-133">データ セット全体を取得するには、代わりに、GridView は、現在のページのレコードのみを取得するクエリを変更します。</span><span class="sxs-lookup"><span data-stu-id="df285-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="df285-134">ユーザーの選択でレコードをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="df285-134">Filter records by user selection</span></span>

<span data-ttu-id="df285-135">モデル バインドでは、モデル バインディングのメソッドでパラメーターの値を設定する方法を指定することを可能にするいくつかの属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="df285-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="df285-136">これらの属性は、 **System.Web.ModelBinding**名前空間。</span><span class="sxs-lookup"><span data-stu-id="df285-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="df285-137">Windows コモン コントロールには以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="df285-137">They include:</span></span>

- <span data-ttu-id="df285-138">コントロール</span><span class="sxs-lookup"><span data-stu-id="df285-138">Control</span></span>
- <span data-ttu-id="df285-139">クッキー</span><span class="sxs-lookup"><span data-stu-id="df285-139">Cookie</span></span>
- <span data-ttu-id="df285-140">フォーム</span><span class="sxs-lookup"><span data-stu-id="df285-140">Form</span></span>
- <span data-ttu-id="df285-141">Profile</span><span class="sxs-lookup"><span data-stu-id="df285-141">Profile</span></span>
- <span data-ttu-id="df285-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="df285-142">QueryString</span></span>
- <span data-ttu-id="df285-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="df285-143">RouteData</span></span>
- <span data-ttu-id="df285-144">セッション</span><span class="sxs-lookup"><span data-stu-id="df285-144">Session</span></span>
- <span data-ttu-id="df285-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="df285-145">UserProfile</span></span>
- <span data-ttu-id="df285-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="df285-146">ViewState</span></span>

<span data-ttu-id="df285-147">このチュートリアルでは、GridView に表示するレコードをフィルター処理するのにコントロールの値を使用します。</span><span class="sxs-lookup"><span data-stu-id="df285-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="df285-148">追加、**コントロール**属性を前に作成したクエリ メソッド。</span><span class="sxs-lookup"><span data-stu-id="df285-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="df285-149">[後で](using-query-string-values-to-retrieve-data.md)チュートリアルが適用されます、 **QueryString**属性をクエリ文字列の値からパラメーター値を取得することを指定するパラメーター。</span><span class="sxs-lookup"><span data-stu-id="df285-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="df285-150">まず、ValidationSummary 上には、ドロップ ダウン受講者が示すようにフィルター処理するためのリストを追加します。</span><span class="sxs-lookup"><span data-stu-id="df285-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="df285-151">分離コード ファイルで、コントロールから値を受信する select メソッドを変更し、値を提供するコントロールの名前に、パラメーターの名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="df285-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="df285-152">追加する必要があります、**を使用して**のステートメント、 **System.Web.ModelBinding**コントロールの属性を解決する名前空間。</span><span class="sxs-lookup"><span data-stu-id="df285-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="df285-153">次のコードでは、ドロップダウン リストの値に基づいて、返されるデータをフィルター処理する再作業 select メソッドを示します。</span><span class="sxs-lookup"><span data-stu-id="df285-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="df285-154">指定するパラメーターはこのパラメーターの値が同じ名前のコントロールから取得される前に、コントロールの属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="df285-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="df285-155">Web アプリケーションを実行し、ドロップダウン リストの学生の一覧をフィルター処理から異なる値を選択します。</span><span class="sxs-lookup"><span data-stu-id="df285-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![フィルターの受講者](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="df285-157">まとめ</span><span class="sxs-lookup"><span data-stu-id="df285-157">Conclusion</span></span>

<span data-ttu-id="df285-158">このチュートリアルでは並べ替えおよびデータのページングを有効になります。</span><span class="sxs-lookup"><span data-stu-id="df285-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="df285-159">有効にしても、コントロールの値によって、データのフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="df285-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="df285-160">次の[チュートリアル](integrating-jquery-ui.md)に動的なデータ テンプレートの JQuery UI ウィジェットを統合することによって、UI を拡張します。</span><span class="sxs-lookup"><span data-stu-id="df285-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df285-161">[前へ](updating-deleting-and-creating-data.md)
> [次へ](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="df285-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
