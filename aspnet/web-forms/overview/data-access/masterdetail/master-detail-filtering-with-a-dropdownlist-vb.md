---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: "マスター/詳細フィルターと共に DropDownList (VB) |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、DropDownList コントロールおよび GridView で選択した項目の詳細のマスター レコードを表示する方法を会いしましょう。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 88e5c65c100bea3cc39b1e08b1aa8a622b4ce7a6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a><span data-ttu-id="dfa44-103">マスター/詳細 DropDownList (VB) によるフィルター処理</span><span class="sxs-lookup"><span data-stu-id="dfa44-103">Master/Detail Filtering With a DropDownList (VB)</span></span>
====================
<span data-ttu-id="dfa44-104">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="dfa44-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="dfa44-105">[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe)または[PDF のダウンロード](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="dfa44-105">[Download Sample App](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) or [Download PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)</span></span>

> <span data-ttu-id="dfa44-106">このチュートリアルでは、DropDownList コントロールおよび GridView で選択した項目の詳細のマスター レコードを表示する方法を会いしましょう。</span><span class="sxs-lookup"><span data-stu-id="dfa44-106">In this tutorial we'll see how to display the master records in a DropDownList control and the details of the selected list item in a GridView.</span></span>


## <a name="introduction"></a><span data-ttu-id="dfa44-107">はじめに</span><span class="sxs-lookup"><span data-stu-id="dfa44-107">Introduction</span></span>

<span data-ttu-id="dfa44-108">レポートの一般的な種類は、*マスター/詳細レポート*、一部の「マスター」のレコードのセットを表示することによって、レポートを開始するのです。</span><span class="sxs-lookup"><span data-stu-id="dfa44-108">A common type of report is the *master/detail report*, in which the report begins by showing some set of "master" records.</span></span> <span data-ttu-id="dfa44-109">ユーザー、ドリルダウンできますマスターのレコードのいずれかにそれによってそのマスター レコードの"の詳細の表示"</span><span class="sxs-lookup"><span data-stu-id="dfa44-109">The user can then drill down into one of the master records, thereby viewing that master record's "details."</span></span> <span data-ttu-id="dfa44-110">マスター/詳細レポートはレポートなどの一対多の関係を視覚化する最適な選択肢すべてのカテゴリを示す特定のカテゴリを選択し、関連付けられた製品を表示するユーザーを許可します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-110">Master/detail reports are an ideal choice for visualizing one-to-many relationships, such as a report showing all of the categories and then allowing a user to select a particular category and display its associated products.</span></span> <span data-ttu-id="dfa44-111">さらに、マスター/詳細レポートは、特に「幅の広い」テーブル (多数の列があるもの) から詳細な情報を表示するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-111">Additionally, master/detail reports are useful for displaying detailed information from particularly "wide" tables (ones that have a lot of columns).</span></span> <span data-ttu-id="dfa44-112">たとえば、「マスター」マスター/詳細レポートのレベルは、データベースにだけ製品名と単位の価格、製品を表示する場合がありますおよび特定の製品へのドリル ダウンは、製品に関するその他のフィールドを表示 (カテゴリ、供給業者、単位あたりの数量とで)。</span><span class="sxs-lookup"><span data-stu-id="dfa44-112">For example, the "master" level of a master/detail report might show just the product name and unit price of the products in the database, and drilling down into a particular product would show the additional product fields (category, supplier, quantity per unit, and so on).</span></span>

<span data-ttu-id="dfa44-113">マスター/詳細レポートの実装されている方法はたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="dfa44-113">There are many ways with which a master/detail report can be implemented.</span></span> <span data-ttu-id="dfa44-114">これと次の 3 つのチュートリアルでは、マスター/詳細レポートは多岐に紹介します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-114">Over this and the next three tutorials we'll look at a variety of master/detail reports.</span></span> <span data-ttu-id="dfa44-115">このチュートリアルでは思いますのマスター レコードを表示する方法、 [DropDownList コントロール](https://msdn.microsoft.com/en-us/library/dtx91y0z.aspx)と GridView で選択した項目の詳細。</span><span class="sxs-lookup"><span data-stu-id="dfa44-115">In this tutorial we'll see how to display the master records in a [DropDownList control](https://msdn.microsoft.com/en-us/library/dtx91y0z.aspx) and the details of the selected list item in a GridView.</span></span> <span data-ttu-id="dfa44-116">具体的には、このチュートリアルのマスター/詳細レポートには、カテゴリおよび製品情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-116">In particular, this tutorial's master/detail report will list category and product information.</span></span>

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a><span data-ttu-id="dfa44-117">手順 1: DropDownList でカテゴリを表示します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-117">Step 1: Displaying the Categories in a DropDownList</span></span>

<span data-ttu-id="dfa44-118">マスター/詳細レポートは、表示される選択されたリスト項目の製品と、DropDownList のカテゴリを一覧表示が GridView でページ上にします。</span><span class="sxs-lookup"><span data-stu-id="dfa44-118">Our master/detail report will list the categories in a DropDownList, with the selected list item's products displayed further down in the page in a GridView.</span></span> <span data-ttu-id="dfa44-119">Us, 事前最初のタスクは、DropDownList で表示されるカテゴリを次に、です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-119">The first task ahead of us, then, is to have the categories displayed in a DropDownList.</span></span> <span data-ttu-id="dfa44-120">開く、 `FilterByDropDownList.aspx`  ページで、`Filtering`フォルダー、DropDownList のページのデザイナーには、ツールボックスからドラッグし、設定、`ID`プロパティを`Categories`です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-120">Open the `FilterByDropDownList.aspx` page in the `Filtering` folder, drag on a DropDownList from the Toolbox onto the page's designer, and set its `ID` property to `Categories`.</span></span> <span data-ttu-id="dfa44-121">次に、DropDownList のスマート タグからデータ ソースの選択 リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dfa44-121">Next, click on the Choose Data Source link from the DropDownList's smart tag.</span></span> <span data-ttu-id="dfa44-122">これにより、データ ソース構成ウィザードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-122">This will display the Data Source Configuration wizard.</span></span>


<span data-ttu-id="dfa44-123">[![DropDownList のデータ ソースを指定します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-123">[![Specify the DropDownList's Data Source](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="dfa44-124">**図 1**: DropDownList のデータ ソースを指定 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-124">**Figure 1**: Specify the DropDownList's Data Source ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="dfa44-125">という名前の新しい ObjectDataSource を追加する`CategoriesDataSource`を呼び出す、`CategoriesBLL`クラスの`GetCategories()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="dfa44-125">Choose to add a new ObjectDataSource named `CategoriesDataSource` that invokes the `CategoriesBLL` class's `GetCategories()` method.</span></span>


<span data-ttu-id="dfa44-126">[![CategoriesDataSource をという名前の新しい ObjectDataSource を追加します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-126">[![Add a New ObjectDataSource Named CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="dfa44-127">**図 2**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-127">**Figure 2**: Add a New ObjectDataSource Named `CategoriesDataSource` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="dfa44-128">[![CategoriesBLL クラスを使用します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-128">[![Choose to Use the CategoriesBLL Class](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="dfa44-129">**図 3**: 使用する、`CategoriesBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-129">**Figure 3**: Choose to Use the `CategoriesBLL` Class ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))</span></span>


<span data-ttu-id="dfa44-130">[![ObjectDataSource GetCategories() メソッドを使用して構成します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-130">[![Configure the ObjectDataSource to Use the GetCategories() Method](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)</span></span>

<span data-ttu-id="dfa44-131">**図 4**: 構成を使用する ObjectDataSource、`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-131">**Figure 4**: Configure the ObjectDataSource to Use the `GetCategories()` Method ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))</span></span>


<span data-ttu-id="dfa44-132">DropDownList でどのようなデータ ソースのフィールドを表示するかと、これを指定する必要があります ObjectDataSource を構成した後、リスト項目の値として関連付けられている 1 つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="dfa44-132">After configuring the ObjectDataSource we still need to specify what data source field should be displayed in DropDownList and which one should be associated as the value for the list item.</span></span> <span data-ttu-id="dfa44-133">`CategoryName`ディスプレイとしてフィールドと`CategoryID`各リスト項目の値として。</span><span class="sxs-lookup"><span data-stu-id="dfa44-133">Have the `CategoryName` field as the display and `CategoryID` as the value for each list item.</span></span>


<span data-ttu-id="dfa44-134">[![値として使用 CategoryID と CategoryName フィールドの DropDownList の表示があります。](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-134">[![Have the DropDownList Display the CategoryName Field and Use CategoryID as the Value](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)</span></span>

<span data-ttu-id="dfa44-135">**図 5**: DropDownList ディスプレイ、`CategoryName`フィールド`CategoryID`値として ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-135">**Figure 5**: Have the DropDownList Display the `CategoryName` Field and Use `CategoryID` as the Value ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))</span></span>


<span data-ttu-id="dfa44-136">レコードが挿入されます DropDownList コントロールがあるこの時点で、`Categories`テーブル (約 6 秒で行うすべて)。</span><span class="sxs-lookup"><span data-stu-id="dfa44-136">At this point we have a DropDownList control that's populated with the records from the `Categories` table (all accomplished in about six seconds).</span></span> <span data-ttu-id="dfa44-137">図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-137">Figure 6 shows our progress thus far when viewed through a browser.</span></span>


<span data-ttu-id="dfa44-138">[![ドロップダウン リストに現在のカテゴリを一覧表示します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-138">[![A Drop-Down Lists the Current Categories](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)</span></span>

<span data-ttu-id="dfa44-139">**図 6**: A ドロップダウン リスト、現在のカテゴリ ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-139">**Figure 6**: A Drop-Down Lists the Current Categories ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))</span></span>


## <a name="step-2-adding-the-products-gridview"></a><span data-ttu-id="dfa44-140">手順 2: 製品 GridView の追加</span><span class="sxs-lookup"><span data-stu-id="dfa44-140">Step 2: Adding the Products GridView</span></span>

<span data-ttu-id="dfa44-141">そのマスター/詳細レポートの最後の手順では、選択したカテゴリに関連付けられている製品の一覧です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-141">That last step in our master/detail report is to list the products associated with the selected category.</span></span> <span data-ttu-id="dfa44-142">これを実現する、GridView、ページを追加し、という名前の新しい ObjectDataSource を作成する`productsDataSource`です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-142">To accomplish this, add a GridView to the page and create a new ObjectDataSource named `productsDataSource`.</span></span> <span data-ttu-id="dfa44-143">`productsDataSource`コントロールからそのデータを選り分けて、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="dfa44-143">Have the `productsDataSource` control cull its data from the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method.</span></span>


<span data-ttu-id="dfa44-144">[![GetProductsByCategoryID(categoryID) 方法を選択します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-144">[![Select the GetProductsByCategoryID(categoryID) Method](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)</span></span>

<span data-ttu-id="dfa44-145">**図 7**: 選択、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-145">**Figure 7**: Select the `GetProductsByCategoryID(categoryID)` Method ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))</span></span>


<span data-ttu-id="dfa44-146">このメソッドを選択すると、ObjectDataSource ウィザードの指示に従って us メソッドの値の *`categoryID`* パラメーター。</span><span class="sxs-lookup"><span data-stu-id="dfa44-146">After choosing this method, the ObjectDataSource wizard prompts us for the value for the method's *`categoryID`* parameter.</span></span> <span data-ttu-id="dfa44-147">選択した値を使用する`categories`DropDownList 項目コントロールを処理するパラメーターのソースを設定する`Categories`です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-147">To use the value of the selected `categories` DropDownList item set the Parameter source to Control and the ControlID to `Categories`.</span></span>


<span data-ttu-id="dfa44-148">[![カテゴリの DropDownList の値に categoryID パラメーターを設定します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-148">[![Set the categoryID Parameter to the Value of the Categories DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)</span></span>

<span data-ttu-id="dfa44-149">**図 8**: 設定、  *`categoryID`* パラメーターの値を`Categories`DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-149">**Figure 8**: Set the *`categoryID`* Parameter to the Value of the `Categories` DropDownList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))</span></span>


<span data-ttu-id="dfa44-150">すぐをブラウザーで作業の進行状況を確認します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-150">Take a moment to check out our progress in a browser.</span></span> <span data-ttu-id="dfa44-151">これらの製品が、選択したカテゴリに属している場合、最初のページへのアクセス、(飲み物) が表示されます (図 9) が DropDownList を変更すると、データが更新されません。</span><span class="sxs-lookup"><span data-stu-id="dfa44-151">When first visiting the page, those products belong to the selected category (Beverages) are displayed (as shown in Figure 9), but changing the DropDownList doesn't update the data.</span></span> <span data-ttu-id="dfa44-152">これは、ため、更新する GridView のポストバックが発生したときです。</span><span class="sxs-lookup"><span data-stu-id="dfa44-152">This is because a postback must occur for the GridView to update.</span></span> <span data-ttu-id="dfa44-153">これを行うには、2 つのオプション (うちどちらが必要、コードを記述) があります。</span><span class="sxs-lookup"><span data-stu-id="dfa44-153">To accomplish this we have two options (neither of which requires writing any code):</span></span>

- <span data-ttu-id="dfa44-154">**設定カテゴリの DropDownList の**[AutoPostBack プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**True に設定します。**</span><span class="sxs-lookup"><span data-stu-id="dfa44-154">**Set the categories DropDownList's**[AutoPostBack property](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**to True.**</span></span> <span data-ttu-id="dfa44-155">(これを行う DropDownList のスマート タグで AutoPostBack を有効にするオプションをチェックしています。)DropDownList の選択されたときに、ポストバックがこれによりトリガー、ユーザーが項目を変更します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-155">(You can accomplish this by checking the Enable AutoPostBack option in the DropDownList's smart tag.) This will trigger a postback whenever the DropDownList's selected item is changed by the user.</span></span> <span data-ttu-id="dfa44-156">したがって、ユーザーは、次のドロップダウン リストから新しいカテゴリを選択したときにポストバックが発生したり、GridView、新しく選択したカテゴリの製品で更新されます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-156">Therefore, when the user selects a new category from the DropDownList a postback will ensue and the GridView will be updated with the products for the newly selected category.</span></span> <span data-ttu-id="dfa44-157">(これは、このチュートリアルで使用したアプローチです)。</span><span class="sxs-lookup"><span data-stu-id="dfa44-157">(This is the approach I've used in this tutorial.)</span></span>
- <span data-ttu-id="dfa44-158">**DropDownList の横にあるボタン Web コントロールを追加します。**</span><span class="sxs-lookup"><span data-stu-id="dfa44-158">**Add a Button Web control next to the DropDownList.**</span></span> <span data-ttu-id="dfa44-159">設定の`Text`プロパティを更新または同様です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-159">Set its `Text` property to Refresh or something similar.</span></span> <span data-ttu-id="dfa44-160">この方法で、ユーザーは、新しいカテゴリを選択し、ボタンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dfa44-160">With this approach, the user will need to select a new category and then click the Button.</span></span> <span data-ttu-id="dfa44-161">ボタンをクリックし、ポストバックを発生させるが、選択したカテゴリの製品を一覧表示する GridView を更新します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-161">Clicking the Button will cause a postback and update the GridView to list those products of the selected category.</span></span>

<span data-ttu-id="dfa44-162">図 9 と 10 は、マスター/詳細レポートの動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="dfa44-162">Figures 9 and 10 illustrate the master/detail report in action.</span></span>


<span data-ttu-id="dfa44-163">[![最初のページへのアクセス、飲み物の製品が表示されます。](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-163">[![When First Visiting the Page, the Beverage Products are Displayed](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)</span></span>

<span data-ttu-id="dfa44-164">**図 9**: 最初のページへのアクセス、飲み物の製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-164">**Figure 9**: When First Visiting the Page, the Beverage Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))</span></span>


<span data-ttu-id="dfa44-165">[![GridView の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-165">[![Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)</span></span>

<span data-ttu-id="dfa44-166">**図 10**: GridView の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択する ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-166">**Figure 10**: Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the GridView ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))</span></span>


## <a name="adding-a----choose-a-category----list-item"></a><span data-ttu-id="dfa44-167">「--カテゴリの選択-」リスト アイテムを追加します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-167">Adding a "-- Choose a Category --" List Item</span></span>

<span data-ttu-id="dfa44-168">最初にアクセスすると、 `FilterByDropDownList.aspx` DropDownList の最初のリスト項目 (飲み物) が既定では、飲み物の製品を示す GridView で選択したカテゴリのページです。</span><span class="sxs-lookup"><span data-stu-id="dfa44-168">When first visiting the `FilterByDropDownList.aspx` page the categories DropDownList's first list item (Beverages) is selected by default, showing the beverage products in the GridView.</span></span> <span data-ttu-id="dfa44-169">代わりに、DropDownList 項目が必要な場合があります、最初のカテゴリの製品の表示を選択するのではなく次のように「- カテゴリの選択-」が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-169">Rather than showing the first category's products, we may want to instead have a DropDownList item selected that says something like, "-- Choose a Category --".</span></span>

<span data-ttu-id="dfa44-170">次のドロップダウン リストに新しいリスト アイテムを追加するに [プロパティ] ウィンドウに移動し、内の省略記号をクリックして、`Items`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="dfa44-170">To add a new list item to the DropDownList, go to the Properties window and click on the ellipses in the `Items` property.</span></span> <span data-ttu-id="dfa44-171">新しい一覧項目を追加、 `Text` 「--カテゴリの選択-」、および`Value``-1`です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-171">Add a new list item with the `Text` "-- Choose a Category --" and the `Value` `-1`.</span></span>


<span data-ttu-id="dfa44-172">[![追加-カテゴリ--リスト アイテムの選択](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-172">[![Add a -- Choose a Category -- List Item](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)</span></span>

<span data-ttu-id="dfa44-173">**図 11**: 追加-カテゴリ--リスト アイテムの選択 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-173">**Figure 11**: Add a -- Choose a Category -- List Item ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))</span></span>


<span data-ttu-id="dfa44-174">代わりに、次のドロップダウン リストに、次のマークアップを追加することでリスト項目を追加できます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-174">Alternatively, you can add the list item by adding the following markup to the DropDownList:</span></span>


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

<span data-ttu-id="dfa44-175">さらの DropDownList の制御を設定する必要があります`AppendDataBoundItems`True の場合に、手動で追加したリスト アイテムを上書きします、カテゴリは、ObjectDataSource から DropDownList にバインドするために`AppendDataBoundItems`True はありません。</span><span class="sxs-lookup"><span data-stu-id="dfa44-175">Additionally, we need to set the DropDownList control's `AppendDataBoundItems` to True because when the categories are bound to the DropDownList from the ObjectDataSource they'll overwrite any manually-added list items if `AppendDataBoundItems` isn't True.</span></span>


![AppendDataBoundItems プロパティを True に設定します。](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

<span data-ttu-id="dfa44-177">**図 12**: 設定、`AppendDataBoundItems`プロパティを True に</span><span class="sxs-lookup"><span data-stu-id="dfa44-177">**Figure 12**: Set the `AppendDataBoundItems` Property to True</span></span>


<span data-ttu-id="dfa44-178">これらの変更後に「--カテゴリの選択-」オプションが選択されている最初のページにアクセスしたときと製品は表示されません。</span><span class="sxs-lookup"><span data-stu-id="dfa44-178">After these changes, when first visiting the page the "-- Choose a Category --" option is selected and no products are displayed.</span></span>


<span data-ttu-id="dfa44-179">[![最初のページ読み込みの製品が表示されません。](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-179">[![On the Initial Page Load No Products are Displayed](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)</span></span>

<span data-ttu-id="dfa44-180">**図 13**: での初期ページ負荷なし製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-180">**Figure 13**: On the Initial Page Load No Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))</span></span>


<span data-ttu-id="dfa44-181">製品が表示されていない場合、"--カテゴリ--"リスト アイテムの選択 が選択されているため理由がその値があるためには`-1`でデータベースに製品がいなくなると、`CategoryID`の`-1`します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-181">The reason no products are displayed when because the "-- Choose a Category --" list item is selected is because its value is `-1` and there are no products in the database with a `CategoryID` of `-1`.</span></span> <span data-ttu-id="dfa44-182">これは、動作する場合はこの時点で完了したら、!</span><span class="sxs-lookup"><span data-stu-id="dfa44-182">If this is the behavior you want then you're done at this point!</span></span> <span data-ttu-id="dfa44-183">ただし、表示する場合は、*すべて*、カテゴリ、"--カテゴリ--"リスト アイテムの選択 を選択すると、戻り値を`ProductsBLL`クラスし、カスタマイズ、`GetProductsByCategoryID(categoryID)`メソッドを呼び出すので、`GetProducts()`メソッド場合渡されたで *`categoryID`* パラメーターが 0 より小さい。</span><span class="sxs-lookup"><span data-stu-id="dfa44-183">If, however, you want to display *all* of the categories when the "-- Choose a Category --" list item is selected, return to the `ProductsBLL` class and customize the `GetProductsByCategoryID(categoryID)` method so that it invokes the `GetProducts()` method if the passed in *`categoryID`* parameter is less than zero:</span></span>


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

<span data-ttu-id="dfa44-184">ここで使用される手法はすべてのサプライヤーの表示に使用したアプローチに似ていますに戻り、[宣言型のパラメーター](../basic-reporting/declarative-parameters-cs.md)が、この例を使用して、値は、チュートリアル`-1`を示すすべてのレコードをする必要がありますなく取得`Nothing`です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-184">The technique used here is similar to the approach we used to display all suppliers back in the [Declarative Parameters](../basic-reporting/declarative-parameters-cs.md) tutorial, although for this example we're using a value of `-1` to indicate that all records should be retrieved as opposed to `Nothing`.</span></span> <span data-ttu-id="dfa44-185">これは、ため、  *`categoryID`* のパラメーター、`GetProductsByCategoryID(categoryID)`メソッドが必要ですが、渡される整数値として宣言型のパラメーターのチュートリアルでは、文字列の入力パラメーターに渡していた一方です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-185">This is because the *`categoryID`* parameter of the `GetProductsByCategoryID(categoryID)` method expects as integer value passed in, whereas in the Declarative Parameters tutorial we were passing in a string input parameter.</span></span>

<span data-ttu-id="dfa44-186">図 14 のスクリーン ショットに示します`FilterByDropDownList.aspx`「--カテゴリの選択-」オプションを選択するとします。</span><span class="sxs-lookup"><span data-stu-id="dfa44-186">Figure 14 shows a screen shot of `FilterByDropDownList.aspx` when the "-- Choose a Category --" option is selected.</span></span> <span data-ttu-id="dfa44-187">ここでは、既定では、すべての製品が表示され、ユーザーが特定のカテゴリを選択して、表示を絞ることができます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-187">Here, all of the products are displayed by default, and the user can narrow the display by choosing a specific category.</span></span>


<span data-ttu-id="dfa44-188">[![一覧で、既定では、すべての製品](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)</span><span class="sxs-lookup"><span data-stu-id="dfa44-188">[![All of the Products are Now Listed By Default](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)</span></span>

<span data-ttu-id="dfa44-189">**図 14**: 一覧で、既定では、すべての製品 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))</span><span class="sxs-lookup"><span data-stu-id="dfa44-189">**Figure 14**: All of the Products are Now Listed By Default ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))</span></span>


## <a name="summary"></a><span data-ttu-id="dfa44-190">概要</span><span class="sxs-lookup"><span data-stu-id="dfa44-190">Summary</span></span>

<span data-ttu-id="dfa44-191">階層的に関連するデータを表示するには、マスター/詳細レポート、元のユーザーは、階層の最上位からデータを参照するための開始し、詳細にドリル ダウンを使用してデータを表示する多くの場合、役立ちます。</span><span class="sxs-lookup"><span data-stu-id="dfa44-191">When displaying hierarchically-related data, it often helps to present the data using master/detail reports, from which the user can start perusing the data from the top of the hierarchy and drill down into details.</span></span> <span data-ttu-id="dfa44-192">このチュートリアルでは、選択したカテゴリの製品が表示された単純なマスター/詳細レポートを作成して調査します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-192">In this tutorial we examined building a simple master/detail report showing a selected category's products.</span></span> <span data-ttu-id="dfa44-193">これは、カテゴリと、選択したカテゴリに属する製品の GridView の一覧については、DropDownList を使用して行われました。</span><span class="sxs-lookup"><span data-stu-id="dfa44-193">This was accomplished by using a DropDownList for the list of categories and a GridView for the products belonging to the selected category.</span></span>

<span data-ttu-id="dfa44-194">[次のチュートリアル](master-detail-filtering-with-two-dropdownlists-vb.md)みましょう DropDownList インターフェイスの 1 つのステップさらに、2 つの DropDownLists を使用します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-194">In the [next tutorial](master-detail-filtering-with-two-dropdownlists-vb.md) we'll take the DropDownList interface one step further, using two DropDownLists.</span></span>

<span data-ttu-id="dfa44-195">満足プログラミング!</span><span class="sxs-lookup"><span data-stu-id="dfa44-195">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="dfa44-196">作成者について</span><span class="sxs-lookup"><span data-stu-id="dfa44-196">About the Author</span></span>

<span data-ttu-id="dfa44-197">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。</span><span class="sxs-lookup"><span data-stu-id="dfa44-197">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="dfa44-198">Scott は、コンサルタント、トレーナー、ライターとして機能します。</span><span class="sxs-lookup"><span data-stu-id="dfa44-198">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="dfa44-199">最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-199">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="dfa44-200">彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。</span><span class="sxs-lookup"><span data-stu-id="dfa44-200">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dfa44-201">[前へ](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
[次へ](master-detail-filtering-with-two-dropdownlists-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dfa44-201">[Previous](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
[Next](master-detail-filtering-with-two-dropdownlists-vb.md)</span></span>
