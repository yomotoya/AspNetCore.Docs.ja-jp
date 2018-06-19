---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: 並べ替え、ページング、およびモデル バインディング機能と web フォームを使用してデータをフィルタ リング |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885407"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="f63b3-104">並べ替え、ページング、およびモデル バインディング機能と web フォームを使用してデータをフィルター処理</span><span class="sxs-lookup"><span data-stu-id="f63b3-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="f63b3-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f63b3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f63b3-106">このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="f63b3-107">モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。</span><span class="sxs-lookup"><span data-stu-id="f63b3-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="f63b3-108">この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。</span><span class="sxs-lookup"><span data-stu-id="f63b3-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="f63b3-109">このチュートリアルでは、並べ替え、ページング、およびモデル バインディングによるデータのフィルター処理を追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="f63b3-110">このチュートリアルは、最初に作成されたプロジェクトに基づいて[一部](retrieving-data.md)シリーズのです。</span><span class="sxs-lookup"><span data-stu-id="f63b3-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="f63b3-111">実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。</span><span class="sxs-lookup"><span data-stu-id="f63b3-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="f63b3-112">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="f63b3-113">これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f63b3-114">新機能のビルド</span><span class="sxs-lookup"><span data-stu-id="f63b3-114">What you'll build</span></span>

<span data-ttu-id="f63b3-115">このチュートリアルでは、次の手順を。</span><span class="sxs-lookup"><span data-stu-id="f63b3-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="f63b3-116">並べ替えとデータのページングを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f63b3-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="f63b3-117">ユーザーが選択範囲に基づくデータのフィルター処理を有効にします。</span><span class="sxs-lookup"><span data-stu-id="f63b3-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="f63b3-118">並べ替えを追加します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-118">Add sorting</span></span>

<span data-ttu-id="f63b3-119">GridView で並べ替えを有効にすることは非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="f63b3-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="f63b3-120">Student.aspx ファイル内の設定だけで**AllowSorting**に**true** GridView でします。</span><span class="sxs-lookup"><span data-stu-id="f63b3-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="f63b3-121">設定する必要はありません、 **SortExpression** DataField が自動的に使用されるように各列の値します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="f63b3-122">GridView は、選択した値によって、データの順序を含めるようにクエリを変更します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="f63b3-123">並べ替えを有効にする必要がある追加の強調表示されたコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="f63b3-124">Web アプリケーションを実行し、別の列の値で並べ替え学生レコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="f63b3-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![並べ替えの受講者](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="f63b3-126">ページングを追加します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-126">Add paging</span></span>

<span data-ttu-id="f63b3-127">ページングを有効化も非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="f63b3-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="f63b3-128">GridView で次のように設定します。、 **AllowPaging**プロパティを**true**設定と、 **PageSize**プロパティを各ページに表示するレコードの数。</span><span class="sxs-lookup"><span data-stu-id="f63b3-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="f63b3-129">このチュートリアルでは 4 に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="f63b3-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="f63b3-130">Web アプリケーションを実行し、これで、レコードが複数のページを 1 ページに表示される 4 個のレコードで割って算出することを確認します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![ページングを追加します。](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="f63b3-132">クエリの遅延実行では、アプリケーションの効率が向上します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="f63b3-133">データ セット全体を取得するには、代わりには、GridView は、現在のページのレコードだけを取得するクエリを変更します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="f63b3-134">ユーザーの選択内容でレコードをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-134">Filter records by user selection</span></span>

<span data-ttu-id="f63b3-135">モデル バインドは、モデル バインド メソッドのパラメーターの値を設定する方法を指定するいくつかの属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="f63b3-136">これらの属性はでは、 **System.Web.ModelBinding**名前空間。</span><span class="sxs-lookup"><span data-stu-id="f63b3-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="f63b3-137">Windows コモン コントロールには以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f63b3-137">They include:</span></span>

- <span data-ttu-id="f63b3-138">コントロール</span><span class="sxs-lookup"><span data-stu-id="f63b3-138">Control</span></span>
- <span data-ttu-id="f63b3-139">クッキー</span><span class="sxs-lookup"><span data-stu-id="f63b3-139">Cookie</span></span>
- <span data-ttu-id="f63b3-140">フォーム</span><span class="sxs-lookup"><span data-stu-id="f63b3-140">Form</span></span>
- <span data-ttu-id="f63b3-141">Profile</span><span class="sxs-lookup"><span data-stu-id="f63b3-141">Profile</span></span>
- <span data-ttu-id="f63b3-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="f63b3-142">QueryString</span></span>
- <span data-ttu-id="f63b3-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="f63b3-143">RouteData</span></span>
- <span data-ttu-id="f63b3-144">セッション</span><span class="sxs-lookup"><span data-stu-id="f63b3-144">Session</span></span>
- <span data-ttu-id="f63b3-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="f63b3-145">UserProfile</span></span>
- <span data-ttu-id="f63b3-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="f63b3-146">ViewState</span></span>

<span data-ttu-id="f63b3-147">このチュートリアルでは、コントロールの値を使用して GridView に表示するレコードをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="f63b3-148">追加、**コントロール**以前作成したクエリ メソッドに属性します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="f63b3-149">[後](using-query-string-values-to-retrieve-data.md)チュートリアルを適用する、 **QueryString**属性値は、クエリ文字列の値からパラメーター値を取得することを指定するパラメーターをします。</span><span class="sxs-lookup"><span data-stu-id="f63b3-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="f63b3-150">最初に、上、ValidationSummary には、ドロップ ダウン受講者が示すようにフィルター選択のリストを追加します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="f63b3-151">分離コード ファイルで、コントロールから値を受け取る select メソッドを変更し、値を提供するコントロールの名前に、パラメーターの名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="f63b3-152">追加する必要があります、**を使用して**のステートメント、 **System.Web.ModelBinding**コントロールの属性を解決するのには名前空間。</span><span class="sxs-lookup"><span data-stu-id="f63b3-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="f63b3-153">次のコードでは、ドロップダウン リストの値に基づいて、返されたデータをフィルター処理を再動作していた select メソッドを示します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="f63b3-154">パラメーターは、このパラメーターの値は、同じ名前を持つコントロールから取得するを指定する前に、コントロールの属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="f63b3-155">Web アプリケーションを実行し、ドロップダウン生徒数の一覧をフィルターするリストから別の値を選択します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![フィルターの受講者](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="f63b3-157">まとめ</span><span class="sxs-lookup"><span data-stu-id="f63b3-157">Conclusion</span></span>

<span data-ttu-id="f63b3-158">このチュートリアルでは並べ替えとデータのページングを有効になります。</span><span class="sxs-lookup"><span data-stu-id="f63b3-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="f63b3-159">コントロールの値によって、データのフィルター処理するも有効にします。</span><span class="sxs-lookup"><span data-stu-id="f63b3-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="f63b3-160">次の[チュートリアル](integrating-jquery-ui.md)動的なデータ テンプレートに JQuery UI ウィジェットを統合することにより、UI を拡張します。</span><span class="sxs-lookup"><span data-stu-id="f63b3-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f63b3-161">[前へ](updating-deleting-and-creating-data.md)
> [次へ](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="f63b3-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
