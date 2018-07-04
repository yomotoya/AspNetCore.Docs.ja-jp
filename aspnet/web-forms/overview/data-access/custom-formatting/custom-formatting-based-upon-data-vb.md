---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: データ (VB) に基づくカスタム書式設定 |Microsoft Docs
author: rick-anderson
description: GridView、DetailsView、またはにバインドされたデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルでは、l をしましょう.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ed55c181534e18bbbb4447ce9a4e5c19ff3717b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366201"
---
<a name="custom-formatting-based-upon-data-vb"></a><span data-ttu-id="cfa7b-104">データ (VB) に基づくカスタム書式設定</span><span class="sxs-lookup"><span data-stu-id="cfa7b-104">Custom Formatting Based Upon Data (VB)</span></span>
====================
<span data-ttu-id="cfa7b-105">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="cfa7b-106">[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe)または[PDF のダウンロード](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-106">[Download Sample App](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) or [Download PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)</span></span>

> <span data-ttu-id="cfa7b-107">GridView、DetailsView、またはにバインドされたデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-107">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="cfa7b-108">このチュートリアルでは、データ バインドのデータ バインドと RowDataBound イベント ハンドラーを使用して書式設定を実現する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-108">In this tutorial we'll look at how to accomplish data bound formatting through the use of the DataBound and RowDataBound event handlers.</span></span>


## <a name="introduction"></a><span data-ttu-id="cfa7b-109">はじめに</span><span class="sxs-lookup"><span data-stu-id="cfa7b-109">Introduction</span></span>

<span data-ttu-id="cfa7b-110">無数のスタイル関連プロパティの使用は、GridView、DetailsView、FormView コントロールの外観をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-110">The appearance of the GridView, DetailsView, and FormView controls can be customized through a myriad of style-related properties.</span></span> <span data-ttu-id="cfa7b-111">などのプロパティ`CssClass`、 `Font`、 `BorderWidth`、 `BorderStyle`、 `BorderColor`、 `Width`、および`Height`、他のユーザーの間でレンダリングされたコントロールの外観を決定します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-111">Properties like `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, and `Height`, among others, dictate the general appearance of the rendered control.</span></span> <span data-ttu-id="cfa7b-112">プロパティを含む`HeaderStyle`、 `RowStyle`、 `AlternatingRowStyle`、特定のセクションに適用されるこれらの同じスタイル設定を許可する他のユーザーとします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-112">Properties including `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, and others allow these same style settings to be applied to particular sections.</span></span> <span data-ttu-id="cfa7b-113">同様に、これらのスタイル設定は、フィールド レベルで適用できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-113">Likewise, these style settings can be applied at the field level.</span></span>

<span data-ttu-id="cfa7b-114">多くのシナリオで、書式設定の要件によって異なります、表示されるデータの値。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-114">In many scenarios though, the formatting requirements depend upon the value of the displayed data.</span></span> <span data-ttu-id="cfa7b-115">などを描画する注意の商品を在庫として保管する製品情報を一覧表示するレポート可能性があります設定を持つこれらの製品の黄色の背景色`UnitsInStock`と`UnitsOnOrder`フィールドは、どちらも 0 に等しい。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-115">For example, to draw attention to out of stock products, a report listing product information might set the background color to yellow for those products whose `UnitsInStock` and `UnitsOnOrder` fields are both equal to 0.</span></span> <span data-ttu-id="cfa7b-116">高価な製品を強調表示するには、太字のフォントで複数の 75.00 ドルのコストをこれらの製品の価格を表示することもします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-116">To highlight the more expensive products, we may want to display the prices of those products costing more than $75.00 in a bold font.</span></span>

<span data-ttu-id="cfa7b-117">GridView、DetailsView、またはにバインドされたデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-117">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="cfa7b-118">このチュートリアルではデータ バインドを使用すると書式設定を実行する方法に注目します、`DataBound`と`RowDataBound`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-118">In this tutorial we'll look at how to accomplish data bound formatting through the use of the `DataBound` and `RowDataBound` event handlers.</span></span> <span data-ttu-id="cfa7b-119">次のチュートリアルではその他の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-119">In the next tutorial we'll explore an alternative approach.</span></span>

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a><span data-ttu-id="cfa7b-120">DetailsView コントロールの`DataBound`イベント ハンドラー</span><span class="sxs-lookup"><span data-stu-id="cfa7b-120">Using the DetailsView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="cfa7b-121">データは、DetailsView にバインドされるデータ ソース コントロールまたはコントロールのデータをプログラムで割り当てることでいつ`DataSource`プロパティと呼び出し元の`DataBind()`メソッドでは、次の一連の手順が発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-121">When data is bound to a DetailsView, either from a data source control or through programmatically assigning data to the control's `DataSource` property and calling its `DataBind()` method, the following sequence of steps occur:</span></span>

1. <span data-ttu-id="cfa7b-122">データ Web コントロールの`DataBinding`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-122">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="cfa7b-123">データ Web コントロールにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-123">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="cfa7b-124">データ Web コントロールの`DataBound`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-124">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="cfa7b-125">カスタム ロジックは、イベント ハンドラーによって、手順 1 および 3 の直後に挿入できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-125">Custom logic can be injected immediately after steps 1 and 3 through an event handler.</span></span> <span data-ttu-id="cfa7b-126">イベント ハンドラーを作成して、`DataBound`イベントされたデータがデータ Web コントロールにバインドされ、必要に応じて、書式設定を調整プログラムで判断できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-126">By creating an event handler for the `DataBound` event we can programmatically determine the data that has been bound to the data Web control and adjust the formatting as needed.</span></span> <span data-ttu-id="cfa7b-127">それではこれを説明するために、製品に関する一般的な情報が一覧表示が表示されます DetailsView を作成、`UnitPrice`値、***太字、斜体フォント***75.00 ドルを超える場合。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-127">To illustrate this let's create a DetailsView that will list general information about a product, but will display the `UnitPrice` value in a ***bold, italic font*** if it exceeds $75.00.</span></span>

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a><span data-ttu-id="cfa7b-128">手順 1: DetailsView で製品情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-128">Step 1: Displaying the Product Information in a DetailsView</span></span>

<span data-ttu-id="cfa7b-129">開く、`CustomColors.aspx`ページで、`CustomFormatting`フォルダー、DetailsView コントロールをツールボックスからデザイナーにドラッグして、設定、`ID`プロパティの値を`ExpensiveProductsPriceInBoldItalic`を呼び出す新しい ObjectDataSource コントロールにバインドし、 `ProductsBLL`クラスの`GetProducts()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-129">Open the `CustomColors.aspx` page in the `CustomFormatting` folder, drag a DetailsView control from the Toolbox onto the Designer, set its `ID` property value to `ExpensiveProductsPriceInBoldItalic`, and bind it to a new ObjectDataSource control that invokes the `ProductsBLL` class's `GetProducts()` method.</span></span> <span data-ttu-id="cfa7b-130">ここに詳細に調査前のチュートリアルであるためここでこれを実現する詳細な手順を省略し、簡潔にするためのされます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-130">The detailed steps for accomplishing this are omitted here for brevity since we examined them in detail in previous tutorials.</span></span>

<span data-ttu-id="cfa7b-131">ObjectDataSource は、DetailsView にバインドしたら後、は、フィールド リストを変更するには、少しを実行します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-131">Once you've bound the ObjectDataSource to the DetailsView, take a moment to modify the field list.</span></span> <span data-ttu-id="cfa7b-132">削除することを選択する、 `ProductID`、 `SupplierID`、 `CategoryID`、 `UnitsInStock`、 `UnitsOnOrder`、 `ReorderLevel`、および`Discontinued`BoundFields の名前を変更して、残りの BoundFields を再フォーマットします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-132">I've opted to remove the `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, and `Discontinued` BoundFields and renamed and reformatted the remaining BoundFields.</span></span> <span data-ttu-id="cfa7b-133">私も取り除か、`Width`と`Height`設定します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-133">I also cleared out the `Width` and `Height` settings.</span></span> <span data-ttu-id="cfa7b-134">DetailsView では、1 つのレコードのみが表示されたら、以降のすべての製品を表示するエンドユーザーに許可するには、ページングを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-134">Since the DetailsView displays only a single record, we need to enable paging in order to allow the end user to view all of the products.</span></span> <span data-ttu-id="cfa7b-135">そのため、DetailsView のスマート タグでページングを有効にするチェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-135">Do so by checking the Enable Paging checkbox in the DetailsView's smart tag.</span></span>


<span data-ttu-id="cfa7b-136">[![図 1: チェック ボックスを有効にするページング DetailsView のスマート タグ](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-136">[![Figure 1: Check the Enable Paging Checkbox in the DetailsView's Smart Tag](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="cfa7b-137">**図 1**図 1: チェック ボックスを有効にするページング DetailsView のスマート タグ ([フルサイズの画像を表示する をクリックします。](custom-formatting-based-upon-data-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-137">**Figure 1**: Figure 1: Check the Enable Paging Checkbox in the DetailsView's Smart Tag ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image3.png))</span></span>


<span data-ttu-id="cfa7b-138">これらの変更後、DetailsView マークアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-138">After these changes, the DetailsView markup will be:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

<span data-ttu-id="cfa7b-139">このページをブラウザーでテストする時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-139">Take a moment to test out this page in your browser.</span></span>


<span data-ttu-id="cfa7b-140">[![DetailsView コントロールは、一度に 1 つの製品を表示します](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-140">[![The DetailsView Control Displays One Product at a Time](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)</span></span>

<span data-ttu-id="cfa7b-141">**図 2**:、DetailsView コントロール表示製品の 1 つずつ ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-141">**Figure 2**: The DetailsView Control Displays One Product at a Time ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image6.png))</span></span>


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="cfa7b-142">手順 2: データ バインド イベント ハンドラー内のデータの値をプログラムで判断します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-142">Step 2: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="cfa7b-143">これらの製品の太字、斜体のフォントの価格を表示するために持つ`UnitPrice`値 75.00 ドルを超えていること、最初にプログラムで判断できる必要があります、`UnitPrice`値。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-143">In order to display the price in a bold, italic font for those products whose `UnitPrice` value exceeds $75.00, we need to first be able to programmatically determine the `UnitPrice` value.</span></span> <span data-ttu-id="cfa7b-144">これは、DetailsView をで実行できます、`DataBound`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-144">For the DetailsView, this can be accomplished in the `DataBound` event handler.</span></span> <span data-ttu-id="cfa7b-145">イベントを作成するには、は、ハンドラーは、DetailsView、デザイナーでクリックし、[プロパティ] ウィンドウに移動します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-145">To create the event handler click on the DetailsView in the Designer then navigate to the Properties window.</span></span> <span data-ttu-id="cfa7b-146">表示、または [表示] メニューに移動ではない場合は、起動、f4 キーを押して、[プロパティ] ウィンドウのメニュー オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-146">Press F4 to bring it up, if it's not visible, or go to the View menu and select the Properties Window menu option.</span></span> <span data-ttu-id="cfa7b-147">[プロパティ] ウィンドウからは、DetailsView のイベントを一覧表示する稲妻のアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-147">From the Properties window, click on the lightning bolt icon to list the DetailsView's events.</span></span> <span data-ttu-id="cfa7b-148">次に、ダブルクリックするか、`DataBound`イベントまたはイベント ハンドラーを作成する名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-148">Next, either double-click the `DataBound` event or type in the name of the event handler you want to create.</span></span>


![データ バインド イベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image7.png)

<span data-ttu-id="cfa7b-150">**図 3**: イベント ハンドラーを作成、`DataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="cfa7b-150">**Figure 3**: Create an Event Handler for the `DataBound` Event</span></span>


> [!NOTE]
> <span data-ttu-id="cfa7b-151">ASP.NET ページのコードの部分からイベント ハンドラーを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-151">You can also create an event handler from the ASP.NET page's code portion.</span></span> <span data-ttu-id="cfa7b-152">あるページの上部にある 2 つのドロップダウン リストがあります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-152">There you'll find two drop-down lists at the top of the page.</span></span> <span data-ttu-id="cfa7b-153">左側のドロップダウン リストからオブジェクトを選択し、右のドロップダウン リストと Visual Studio からのハンドラーを作成するイベントが自動的に適切なイベント ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-153">Select the object from the left drop-down list and the event you want to create a handler for from the right drop-down list and Visual Studio will automatically create the appropriate event handler.</span></span>


<span data-ttu-id="cfa7b-154">これは自動的にイベント ハンドラーを作成に移動コード部分が追加されました。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-154">Doing so will automatically create the event handler and take you to the code portion where it has been added.</span></span> <span data-ttu-id="cfa7b-155">この時点で表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-155">At this point you will see:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

<span data-ttu-id="cfa7b-156">DetailsView にバインドされたデータを使用してアクセスできる、`DataItem`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-156">The data bound to the DetailsView can be accessed via the `DataItem` property.</span></span> <span data-ttu-id="cfa7b-157">コントロールに厳密に型指定された DataRow インスタンスのコレクションから構成される厳密に型指定された DataTable をバインドしていることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-157">Recall that we are binding our controls to a strongly-typed DataTable, which is composed of a collection of strongly-typed DataRow instances.</span></span> <span data-ttu-id="cfa7b-158">DetailsView に、DataTable 内の最初の DataRow が割り当てられている、DetailsView に DataTable をバインドするとと、`DataItem`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-158">When the DataTable is bound to the DetailsView, the first DataRow in the DataTable is assigned to the DetailsView's `DataItem` property.</span></span> <span data-ttu-id="cfa7b-159">具体的には、`DataItem`プロパティが割り当てられている、`DataRowView`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-159">Specifically, the `DataItem` property is assigned a `DataRowView` object.</span></span> <span data-ttu-id="cfa7b-160">使用して、`DataRowView`の`Row`が基になる DataRow オブジェクトへのアクセスを取得するプロパティが、実際に`ProductsRow`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-160">We can use the `DataRowView`'s `Row` property to get access to the underlying DataRow object, which is actually a `ProductsRow` instance.</span></span> <span data-ttu-id="cfa7b-161">これを行って`ProductsRow`インスタンス オブジェクトのプロパティの値を検査するだけで、意思決定を行んだことができます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-161">Once we have this `ProductsRow` instance we can make our decision by simply inspecting the object's property values.</span></span>

<span data-ttu-id="cfa7b-162">次のコード例を確認するかどうか、 `UnitPrice` DetailsView コントロールにバインドされている値 75.00 ドルを超えています。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-162">The following code illustrates how to determine whether the `UnitPrice` value bound to the DetailsView control is greater than $75.00:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> <span data-ttu-id="cfa7b-163">`UnitPrice`できますが、`NULL`データベース内の値は、最初に確認で扱っていないかどうかを確認する、`NULL`値にアクセスする前に、`ProductsRow`の`UnitPrice`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-163">Since `UnitPrice` can have a `NULL` value in the database, we first check to make sure that we're not dealing with a `NULL` value before accessing the `ProductsRow`'s `UnitPrice` property.</span></span> <span data-ttu-id="cfa7b-164">このチェックは重要ですのでにアクセスする場合、`UnitPrice`プロパティが、`NULL`値、`ProductsRow`オブジェクトがスローされます、 [StrongTypingException 例外](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-164">This check is important because if we attempt to access the `UnitPrice` property when it has a `NULL` value the `ProductsRow` object will throw a [StrongTypingException exception](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).</span></span>


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a><span data-ttu-id="cfa7b-165">手順 3: DetailsView で UnitPrice の値の書式設定</span><span class="sxs-lookup"><span data-stu-id="cfa7b-165">Step 3: Formatting the UnitPrice Value in the DetailsView</span></span>

<span data-ttu-id="cfa7b-166">この時点で判断できるかどうか、 `UnitPrice` DetailsView にバインドされている値を超える $75.00、値を持つが、DetailsView をプログラムで調整する方法については適切に書式のまだ用意しています。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-166">At this point we can determine whether the `UnitPrice` value bound to the DetailsView has a value that exceeds $75.00, but we've yet to see how to programmatically adjust the DetailsView's formatting accordingly.</span></span> <span data-ttu-id="cfa7b-167">DetailsView で行全体の書式設定を変更するプログラムでアクセスを使用して行`DetailsViewID.Rows(index)`; を変更する特定のセルを使用するアクセス`DetailsViewID.Rows(index).Cells(index)`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-167">To modify the formatting of an entire row in the DetailsView, programmatically access the row using `DetailsViewID.Rows(index)`; to modify a particular cell, access use `DetailsViewID.Rows(index).Cells(index)`.</span></span> <span data-ttu-id="cfa7b-168">行またはセルの外観のスタイル関連プロパティを設定し、調整できるへの参照を取得したら。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-168">Once we have a reference to the row or cell we can then adjust its appearance by setting its style-related properties.</span></span>

<span data-ttu-id="cfa7b-169">行をプログラムでアクセスするには、0 から始まる行のインデックスがわかっている必要があります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-169">Accessing a row programmatically requires that you know the row's index, which starts at 0.</span></span> <span data-ttu-id="cfa7b-170">`UnitPrice`行が 4 のインデックスを付けることと、プログラムでアクセスできるように、DetailsView の 5 番目の行を使用して`ExpensiveProductsPriceInBoldItalic.Rows(4)`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-170">The `UnitPrice` row is the fifth row in the DetailsView, giving it an index of 4 and making it programmatically accessible using `ExpensiveProductsPriceInBoldItalic.Rows(4)`.</span></span> <span data-ttu-id="cfa7b-171">この時点で、次のコードを使用して、太字、斜体のフォントで表示される行全体のコンテンツがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-171">At this point we could have the entire row's content displayed in a bold, italic font by using the following code:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

<span data-ttu-id="cfa7b-172">ただし、これにより、*両方*ラベル (価格) と太字と斜体の値。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-172">However, this will make *both* the label (Price) and the value bold and italic.</span></span> <span data-ttu-id="cfa7b-173">値を作成した太字と斜体書式設定を 2 番目のセル、行は、次を使用して実現可能でこれを適用する場合。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-173">If we want to make just the value bold and italic we need to apply this formatting to the second cell in the row, which can be accomplished using the following:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

<span data-ttu-id="cfa7b-174">ここまでチュートリアルは、レンダリングされたマークアップとスタイル関連の情報が明確に分離を維持するためにスタイル シートを使用して後、は、上記みましょう代わりに、特定のスタイル プロパティを設定するのではなく CSS クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-174">Since our tutorials thus far have used stylesheets to maintain a clean separation between the rendered markup and style-related information, rather than setting the specific style properties as shown above let's instead use a CSS class.</span></span> <span data-ttu-id="cfa7b-175">開く、`Styles.css`スタイル シートという名前の新しい CSS クラスを追加および`ExpensivePriceEmphasis`を次の定義。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-175">Open the `Styles.css` stylesheet and add a new CSS class named `ExpensivePriceEmphasis` with the following definition:</span></span>


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

<span data-ttu-id="cfa7b-176">次に、`DataBound`イベント ハンドラーでは、設定、セルの`CssClass`プロパティを`ExpensivePriceEmphasis`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-176">Then, in the `DataBound` event handler, set the cell's `CssClass` property to `ExpensivePriceEmphasis`.</span></span> <span data-ttu-id="cfa7b-177">次のコードは、`DataBound`全体のイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-177">The following code shows the `DataBound` event handler in its entirety:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

<span data-ttu-id="cfa7b-178">通常フォントで価格が表示されます、Chai 75.00 ドル未満のコスト、これを表示するときに (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-178">When viewing Chai, which costs less than $75.00, the price is displayed in a normal font (see Figure 4).</span></span> <span data-ttu-id="cfa7b-179">ただし、97.00 ドルの価格を持つ、Mishi 日本の神戸 Niku を表示すると、価格が太字、斜体のフォントに表示されます (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-179">However, when viewing Mishi Kobe Niku, which has a price of $97.00, the price is displayed in a bold, italic font (see Figure 5).</span></span>


<span data-ttu-id="cfa7b-180">[![通常フォントで $75.00 より低い価格が表示されます。](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-180">[![Prices Less than $75.00 are Displayed in a Normal Font](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)</span></span>

<span data-ttu-id="cfa7b-181">**図 4**: 標準フォントで $75.00 より低い価格が表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image10.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-181">**Figure 4**: Prices Less than $75.00 are Displayed in a Normal Font ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="cfa7b-182">[![高価な製品の価格は太字、斜体フォントで表示されます。](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-182">[![Expensive Products' Prices are Displayed in a Bold, Italic Font](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="cfa7b-183">**図 5**: 太字、斜体フォントで高価な製品の価格が表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image13.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-183">**Figure 5**: Expensive Products' Prices are Displayed in a Bold, Italic Font ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image13.png))</span></span>


## <a name="using-the-formview-controlsdataboundevent-handler"></a><span data-ttu-id="cfa7b-184">FormView コントロールの`DataBound`イベント ハンドラー</span><span class="sxs-lookup"><span data-stu-id="cfa7b-184">Using the FormView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="cfa7b-185">FormView にバインドされている基になるデータを決定するための手順はものと同じですが、DetailsView を作成、`DataBound`イベント ハンドラーでは、キャスト、`DataItem`プロパティを適切なオブジェクトの種類が、コントロールにバインドされ、続行する方法を決定します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-185">The steps for determining the underlying data bound to a FormView are identical to those for a DetailsView create a `DataBound` event handler, cast the `DataItem` property to the appropriate object type bound to the control, and determine how to proceed.</span></span> <span data-ttu-id="cfa7b-186">FormView と DetailsView が異なる場合、ただしでのユーザー インターフェイスの外観を更新する方法。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-186">The FormView and DetailsView differ, however, in how their user interface's appearance is updated.</span></span>

<span data-ttu-id="cfa7b-187">FormView 任意 BoundFields が含まれていないとがないため、`Rows`コレクション。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-187">The FormView does not contain any BoundFields and therefore lacks the `Rows` collection.</span></span> <span data-ttu-id="cfa7b-188">テンプレートは、静的な HTML の組み合わせを含めることができます、FormView を構成する代わりに、Web コントロール、およびデータ バインド構文。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-188">Instead, a FormView is composed of templates, which can contain a mix of static HTML, Web controls, and databinding syntax.</span></span> <span data-ttu-id="cfa7b-189">通常、フォーム ビューのスタイルを調整するには、FormView のテンプレート内の Web コントロールの 1 つ以上のスタイルを調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-189">Adjusting the style of a FormView typically involves adjusting the style of one or more of the Web controls within the FormView's templates.</span></span>

<span data-ttu-id="cfa7b-190">これを示すためには、みましょうみましょうなどが、前の例で、この時点で製品の一覧をフォーム ビューを表示、製品名と単位だけ在庫は 10 以下の場合は、赤いフォントで表示単位と在庫です。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-190">To illustrate this, let's use a FormView to list products like in the previous example, but this time let's display just the product name and units in stock with the units in stock displayed in a red font if it is less than or equal to 10.</span></span>

## <a name="step-4-displaying-the-product-information-in-a-formview"></a><span data-ttu-id="cfa7b-191">手順 4: FormView で製品情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-191">Step 4: Displaying the Product Information in a FormView</span></span>

<span data-ttu-id="cfa7b-192">フォーム ビューの追加、 `CustomColors.aspx` DetailsView やセットの下のページ、`ID`プロパティを`LowStockedProductsInRed`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-192">Add a FormView to the `CustomColors.aspx` page beneath the DetailsView and set its `ID` property to `LowStockedProductsInRed`.</span></span> <span data-ttu-id="cfa7b-193">FormView を前の手順で作成された ObjectDataSource コントロールにバインドします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-193">Bind the FormView to the ObjectDataSource control created from the previous step.</span></span> <span data-ttu-id="cfa7b-194">これが作成されます、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`FormView の。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-194">This will create an `ItemTemplate`, `EditItemTemplate`, and `InsertItemTemplate` for the FormView.</span></span> <span data-ttu-id="cfa7b-195">削除、`EditItemTemplate`と`InsertItemTemplate`および簡略化、`ItemTemplate`を含めるだけ`ProductName`と`UnitsInStock`値、それぞれを適切に名前付きの独自のラベル コントロールにします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-195">Remove the `EditItemTemplate` and `InsertItemTemplate` and simplify the `ItemTemplate` to include just the `ProductName` and `UnitsInStock` values, each in their own appropriately-named Label controls.</span></span> <span data-ttu-id="cfa7b-196">ある DetailsView、前述の例でも FormView のスマート タグでページングを有効にするチェック ボックスをチェックします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-196">As with the DetailsView from the earlier example, also check the Enable Paging checkbox in the FormView's smart tag.</span></span>

<span data-ttu-id="cfa7b-197">これらの編集後に、フォーム ビューのマークアップは、次のようなになります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-197">After these edits your FormView's markup should look similar to the following:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

<span data-ttu-id="cfa7b-198">なお、`ItemTemplate`が含まれています。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-198">Note that the `ItemTemplate` contains:</span></span>

- <span data-ttu-id="cfa7b-199">**静的な HTML**テキスト"Product:"と"在庫数:"と共に、`<br />`と`<b>`要素。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-199">**Static HTML** the text "Product:" and "Units In Stock:" along with the `<br />` and `<b>` elements.</span></span>
- <span data-ttu-id="cfa7b-200">**Web コントロール**2 つのラベル コントロール`ProductNameLabel`と`UnitsInStockLabel`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-200">**Web controls** the two Label controls, `ProductNameLabel` and `UnitsInStockLabel`.</span></span>
- <span data-ttu-id="cfa7b-201">**データ バインディング構文**、`<%# Bind("ProductName") %>`と`<%# Bind("UnitsInStock") %>`構文は、これらのフィールドから値をラベル コントロールに割り当てます`Text`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-201">**Databinding syntax** the `<%# Bind("ProductName") %>` and `<%# Bind("UnitsInStock") %>` syntax, which assigns the values from these fields to the Label controls' `Text` properties.</span></span>

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="cfa7b-202">手順 5: データ バインド イベント ハンドラー内のデータの値をプログラムで判断します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-202">Step 5: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="cfa7b-203">次の手順はフォーム ビューのマークアップを完全なプログラムによる場合を決定する、`UnitsInStock`値が 10 未満。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-203">With the FormView's markup complete, the next step is to programmatically determine if the `UnitsInStock` value is less than or equal to 10.</span></span> <span data-ttu-id="cfa7b-204">これには、DetailsView であったため、フォーム ビューとまったく同じ方法で実現されます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-204">This is accomplished in the exact same manner with the FormView as it was with the DetailsView.</span></span> <span data-ttu-id="cfa7b-205">FormView のため、イベント ハンドラーを作成して開始`DataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-205">Start by creating an event handler for the FormView's `DataBound` event.</span></span>


![データ バインド イベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image14.png)

<span data-ttu-id="cfa7b-207">**図 6**: 作成、`DataBound`イベント ハンドラー</span><span class="sxs-lookup"><span data-stu-id="cfa7b-207">**Figure 6**: Create the `DataBound` Event Handler</span></span>


<span data-ttu-id="cfa7b-208">イベント ハンドラーにキャスト FormView の`DataItem`プロパティを`ProductsRow`インスタンス化し、決定かどうか、`UnitsInPrice`値が赤いフォントで表示する必要がありますようにします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-208">In the event handler cast the FormView's `DataItem` property to a `ProductsRow` instance and determine whether the `UnitsInPrice` value is such that we need to display it in a red font.</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a><span data-ttu-id="cfa7b-209">手順 6: FormView の ItemTemplate の UnitsInStockLabel ラベル コントロールの書式設定</span><span class="sxs-lookup"><span data-stu-id="cfa7b-209">Step 6: Formatting the UnitsInStockLabel Label Control in the FormView's ItemTemplate</span></span>

<span data-ttu-id="cfa7b-210">最後の手順では、表示されている書式設定を`UnitsInStock`赤いフォントで値の場合は、値が 10 個以下。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-210">The final step is to format the displayed `UnitsInStock` value in a red font if the value is 10 or less.</span></span> <span data-ttu-id="cfa7b-211">プログラムでアクセスする必要があります。 これを実現する、`UnitsInStockLabel`を制御、`ItemTemplate`し、そのテキストが赤で表示されるように、そのスタイル プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-211">To accomplish this we need to programmatically access the `UnitsInStockLabel` control in the `ItemTemplate` and set its style properties so that its text is displayed in red.</span></span> <span data-ttu-id="cfa7b-212">テンプレートでの Web コントロールにアクセスするには、使用、`FindControl("controlID")`このようなメソッド。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-212">To access a Web control in a template, use the `FindControl("controlID")` method like this:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

<span data-ttu-id="cfa7b-213">この例では、ラベルにアクセスする必要のあるを制御`ID`値は`UnitsInStockLabel`を使用しますので。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-213">For our example we want to access a Label control whose `ID` value is `UnitsInStockLabel`, so we'd use:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

<span data-ttu-id="cfa7b-214">Web コントロールへの参照をプログラムによって取得したら、必要に応じて、そのスタイルに関連するプロパティ変更できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-214">Once we have a programmatic reference to the Web control, we can modify its style-related properties as needed.</span></span> <span data-ttu-id="cfa7b-215">前の例では、CSS クラスを設けて、`Styles.css`という`LowUnitsInStockEmphasis`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-215">As with the earlier example, I've created a CSS class in `Styles.css` named `LowUnitsInStockEmphasis`.</span></span> <span data-ttu-id="cfa7b-216">ラベルの Web コントロールには、このスタイルを適用するに次のように設定します。 その`CssClass`プロパティに応じて。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-216">To apply this style to the Label Web control, set its `CssClass` property accordingly.</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> <span data-ttu-id="cfa7b-217">プログラムにアクセスする Web コントロール テンプレートの書式を設定するための構文`FindControl("controlID")`を使用する場合、そのスタイル関連プロパティを設定しを使用こともできます[TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) GridView、DetailsView でコントロール。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-217">The syntax for formatting a template programmatically accessing the Web control using `FindControl("controlID")` and then setting its style-related properties can also be used when using [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) in the DetailsView or GridView controls.</span></span> <span data-ttu-id="cfa7b-218">TemplateFields で、次のチュートリアルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-218">We'll examine TemplateFields in our next tutorial.</span></span>


<span data-ttu-id="cfa7b-219">図 7 製品を表示するときに、フォーム ビューを示していますが`UnitsInStock`図 8 の製品があるその値が 10 未満の値が 10 より大きい。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-219">Figures 7 shows the FormView when viewing a product whose `UnitsInStock` value is greater than 10, while the product in Figure 8 has its value less than 10.</span></span>


<span data-ttu-id="cfa7b-220">[![カスタム書式の適用の製品で、十分に大きい Units In Stock、](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-220">[![For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)</span></span>

<span data-ttu-id="cfa7b-221">**図 7**: の製品で、十分に大きい Units In Stock"、"カスタム書式の適用 ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image17.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-221">**Figure 7**: For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image17.png))</span></span>


<span data-ttu-id="cfa7b-222">[![在庫数の単位は、製品の値を 10 以下の赤で表示します。](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-222">[![The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)</span></span>

<span data-ttu-id="cfa7b-223">**図 8**: これら製品での値が 10 以下の在庫数単位が赤で表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image20.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-223">**Figure 8**: The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image20.png))</span></span>


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a><span data-ttu-id="cfa7b-224">GridView の書式が設定`RowDataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="cfa7b-224">Formatting with the GridView's`RowDataBound`Event</span></span>

<span data-ttu-id="cfa7b-225">それ以前一連の手順、DetailsView を調べるし、FormView がデータ バインド時の進行状況を制御します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-225">Earlier we examined the sequence of steps the DetailsView and FormView controls progress through during databinding.</span></span> <span data-ttu-id="cfa7b-226">見て次の手順をもう一度リフレッシャーとして。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-226">Let's look over these steps once again as a refresher.</span></span>

1. <span data-ttu-id="cfa7b-227">データ Web コントロールの`DataBinding`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-227">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="cfa7b-228">データ Web コントロールにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-228">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="cfa7b-229">データ Web コントロールの`DataBound`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-229">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="cfa7b-230">1 つのレコードのみを表示するので、これら 3 つの簡単な手順は、DetailsView と FormView 十分です。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-230">These three simple steps are sufficient for the DetailsView and FormView because they display only a single record.</span></span> <span data-ttu-id="cfa7b-231">表示されます、GridView の*すべて*レコードにバインドされている手順 2 に (最初) だけでなく、少し複雑です。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-231">For the GridView, which displays *all* records bound to it (not just the first), step 2 is a bit more involved.</span></span>

<span data-ttu-id="cfa7b-232">GridView は、データ ソースを列挙し、各レコードでは、次のように作成します。 2 の手順で、`GridViewRow`インスタンスと、現在のレコードをバインドします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-232">In step 2 the GridView enumerates the data source and, for each record, creates a `GridViewRow` instance and binds the current record to it.</span></span> <span data-ttu-id="cfa7b-233">各`GridViewRow`GridView に追加すると、2 つのイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-233">For each `GridViewRow` added to the GridView, two events are raised:</span></span>

- <span data-ttu-id="cfa7b-234">**`RowCreated`** 後に起動、`GridViewRow`が作成されました</span><span class="sxs-lookup"><span data-stu-id="cfa7b-234">**`RowCreated`** fires after the `GridViewRow` has been created</span></span>
- <span data-ttu-id="cfa7b-235">**`RowDataBound`** 現在のレコードにバインドされた後に発生、`GridViewRow`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-235">**`RowDataBound`** fires after the current record has been bound to the `GridViewRow`.</span></span>

<span data-ttu-id="cfa7b-236">GridView、データ バインディングより正確に記載されている、次の一連の手順で。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-236">For the GridView, then, data binding is more accurately described by the following sequence of steps:</span></span>

1. <span data-ttu-id="cfa7b-237">GridView の`DataBinding`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-237">The GridView's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="cfa7b-238">データは、GridView にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-238">The data is bound to the GridView.</span></span>   
  
   <span data-ttu-id="cfa7b-239">データ ソース内の各レコード</span><span class="sxs-lookup"><span data-stu-id="cfa7b-239">For each record in the data source</span></span> 

    1. <span data-ttu-id="cfa7b-240">作成、`GridViewRow`オブジェクト</span><span class="sxs-lookup"><span data-stu-id="cfa7b-240">Create a `GridViewRow` object</span></span>
    2. <span data-ttu-id="cfa7b-241">火災、`RowCreated`イベント</span><span class="sxs-lookup"><span data-stu-id="cfa7b-241">Fire the `RowCreated` event</span></span>
    3. <span data-ttu-id="cfa7b-242">レコードにバインドします `GridViewRow`</span><span class="sxs-lookup"><span data-stu-id="cfa7b-242">Bind the record to the `GridViewRow`</span></span>
    4. <span data-ttu-id="cfa7b-243">火災、`RowDataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="cfa7b-243">Fire the `RowDataBound` event</span></span>
    5. <span data-ttu-id="cfa7b-244">追加、`GridViewRow`を`Rows`コレクション</span><span class="sxs-lookup"><span data-stu-id="cfa7b-244">Add the `GridViewRow` to the `Rows` collection</span></span>
3. <span data-ttu-id="cfa7b-245">GridView の`DataBound`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-245">The GridView's `DataBound` event fires.</span></span>

<span data-ttu-id="cfa7b-246">GridView の個々 のレコードの形式をカスタマイズするには、次に、必要がありますのイベント ハンドラーを作成する、`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-246">To customize the format of the GridView's individual records, then, we need to create an event handler for the `RowDataBound` event.</span></span> <span data-ttu-id="cfa7b-247">これを示すためには、追加を GridView、`CustomColors.aspx`名前、カテゴリ、およびこれらの製品価格がより小さい $10.00 黄色の背景色で強調表示し、製品ごとの価格を一覧表示されたページ。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-247">To illustrate this, let's add a GridView to the `CustomColors.aspx` page that lists the name, category, and price for each product, highlighting those products whose price is less than $10.00 with a yellow background color.</span></span>

## <a name="step-7-displaying-product-information-in-a-gridview"></a><span data-ttu-id="cfa7b-248">手順 7: GridView の製品情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-248">Step 7: Displaying Product Information in a GridView</span></span>

<span data-ttu-id="cfa7b-249">前の例から、GridView、FormView 下を追加し、設定、`ID`プロパティを`HighlightCheapProducts`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-249">Add a GridView beneath the FormView from the previous example and set its `ID` property to `HighlightCheapProducts`.</span></span> <span data-ttu-id="cfa7b-250">ページ上のすべての製品を返す、ObjectDataSource が既にある、ために、GridView をバインドします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-250">Since we already have an ObjectDataSource that returns all products on the page, bind the GridView to that.</span></span> <span data-ttu-id="cfa7b-251">最後に、製品の名前、カテゴリ、および価格だけを含める GridView の BoundFields を編集します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-251">Finally, edit the GridView's BoundFields to include just the products' names, categories, and prices.</span></span> <span data-ttu-id="cfa7b-252">これらの編集後よう GridView のマークアップになります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-252">After these edits the GridView's markup should look like:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

<span data-ttu-id="cfa7b-253">図 9 は、ブラウザーで表示した場合は、このポイントに進行状況を示します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-253">Figure 9 shows our progress to this point when viewed through a browser.</span></span>


<span data-ttu-id="cfa7b-254">[![GridView は、名前、カテゴリ、および各製品の価格を一覧表示されます。](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-254">[![The GridView Lists the Name, Category, and Price For Each Product](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)</span></span>

<span data-ttu-id="cfa7b-255">**図 9**: GridView には、名前、カテゴリ、および各製品の価格が一覧表示されます ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image23.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-255">**Figure 9**: The GridView Lists the Name, Category, and Price For Each Product ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image23.png))</span></span>


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a><span data-ttu-id="cfa7b-256">手順 8: RowDataBound イベント ハンドラー内のデータの値をプログラムで判断します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-256">Step 8: Programmatically Determining the Value of the Data in the RowDataBound Event Handler</span></span>

<span data-ttu-id="cfa7b-257">ときに、 `ProductsDataTable` GridView にバインドされてその`ProductsRow`インスタンスが列挙され、各`ProductsRow`、`GridViewRow`が作成されます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-257">When the `ProductsDataTable` is bound to the GridView its `ProductsRow` instances are enumerated and for each `ProductsRow` a `GridViewRow` is created.</span></span> <span data-ttu-id="cfa7b-258">`GridViewRow`の`DataItem`特定にプロパティが割り当てられている`ProductRow`、その後、GridView の`RowDataBound`イベント ハンドラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-258">The `GridViewRow`'s `DataItem` property is assigned to the particular `ProductRow`, after which the GridView's `RowDataBound` event handler is raised.</span></span> <span data-ttu-id="cfa7b-259">決定する、`UnitPrice`製品ごとの値は、GridView にバインドし、GridView のイベント ハンドラーを作成する必要があります`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-259">To determine the `UnitPrice` value for each product bound to the GridView, then, we need to create an event handler for the GridView's `RowDataBound` event.</span></span> <span data-ttu-id="cfa7b-260">このイベント ハンドラーを検査できる、 `UnitPrice` 、現在の値`GridViewRow`し、その行の書式設定を決定します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-260">In this event handler we can inspect the `UnitPrice` value for the current `GridViewRow` and make a formatting decision for that row.</span></span>

<span data-ttu-id="cfa7b-261">このイベント ハンドラーは、同じ一連の手順としてをフォーム ビューと DetailsView を使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-261">This event handler can be created using the same series of steps as with the FormView and DetailsView.</span></span>


![GridView の RowDataBound イベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-vb/_static/image24.png)

<span data-ttu-id="cfa7b-263">**図 10**: の GridView のイベント ハンドラーを作成`RowDataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="cfa7b-263">**Figure 10**: Create an Event Handler for the GridView's `RowDataBound` Event</span></span>


<span data-ttu-id="cfa7b-264">この方法でイベント ハンドラーを作成すると、ASP.NET ページのコード部分に自動的に追加するのには、次のコードが発生します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-264">Creating the event hander in this manner will cause the following code to be automatically added to the ASP.NET page's code portion:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

<span data-ttu-id="cfa7b-265">ときに、`RowDataBound`イベントが起動され、イベント ハンドラーとして渡される 2 番目のパラメーター型のオブジェクト`GridViewRowEventArgs`、という名前のプロパティを持つ`Row`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-265">When the `RowDataBound` event fires, the event handler is passed as its second parameter an object of type `GridViewRowEventArgs`, which has a property named `Row`.</span></span> <span data-ttu-id="cfa7b-266">このプロパティへの参照を返します、`GridViewRow`単データ バインドでした。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-266">This property returns a reference to the `GridViewRow` that was just data bound.</span></span> <span data-ttu-id="cfa7b-267">アクセスする、`ProductsRow`インスタンスにバインドされる、`GridViewRow`使用して、`DataItem`プロパティようになります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-267">To access the `ProductsRow` instance bound to the `GridViewRow` we use the `DataItem` property like so:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

<span data-ttu-id="cfa7b-268">使用する場合、`RowDataBound`イベント ハンドラーの注意のこのイベントが発生して GridView がさまざまな種類の行で構成することが重要*すべて*行型。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-268">When working with the `RowDataBound` event handler it is important to keep in mind that the GridView is composed of different types of rows and that this event is fired for *all* row types.</span></span> <span data-ttu-id="cfa7b-269">A`GridViewRow`の型によって決定できるその`RowType`プロパティ、使用可能な値のいずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-269">A `GridViewRow`'s type can be determined by its `RowType` property, and can have one of the possible values:</span></span>

- <span data-ttu-id="cfa7b-270">`DataRow` GridView からのレコードにバインドされている行 `DataSource`</span><span class="sxs-lookup"><span data-stu-id="cfa7b-270">`DataRow` a row that is bound to a record from the GridView's `DataSource`</span></span>
- <span data-ttu-id="cfa7b-271">`EmptyDataRow` 行の場合に表示されます、GridView の`DataSource`が空です</span><span class="sxs-lookup"><span data-stu-id="cfa7b-271">`EmptyDataRow` the row displayed if the GridView's `DataSource` is empty</span></span>
- <span data-ttu-id="cfa7b-272">`Footer` フッター行。表示されている場合、GridView の`ShowFooter`プロパティに設定 `True`</span><span class="sxs-lookup"><span data-stu-id="cfa7b-272">`Footer` the footer row; shown if the GridView's `ShowFooter` property is set to `True`</span></span>
- <span data-ttu-id="cfa7b-273">`Header` ヘッダー行です。GridView の ShowHeader のプロパティに設定されているかどうかを示す`True`(既定値)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-273">`Header` the header row; shown if the GridView's ShowHeader property is set to `True` (the default)</span></span>
- <span data-ttu-id="cfa7b-274">`Pager` ページング、ページング インターフェイスを表示する行を実装するは、GridView</span><span class="sxs-lookup"><span data-stu-id="cfa7b-274">`Pager` for GridView's that implement paging, the row that displays the paging interface</span></span>
- <span data-ttu-id="cfa7b-275">`Separator` GridView では使用されませんで使用される、 `RowType` DataList と Repeater のプロパティを制御する 2 つのデータ Web コントロールを紹介する今後のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="cfa7b-275">`Separator` not used for the GridView, but used by the `RowType` properties for the DataList and Repeater controls, two data Web controls we'll discuss in future tutorials</span></span>

<span data-ttu-id="cfa7b-276">以降、 `EmptyDataRow`、 `Header`、`Footer`と`Pager`行に関連付けられている、`DataSource`レコードは常に値があるの`Nothing`の`DataItem`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-276">Since the `EmptyDataRow`, `Header`, `Footer`, and `Pager` rows aren't associated with a `DataSource` record, they will always have a value of `Nothing` for their `DataItem` property.</span></span> <span data-ttu-id="cfa7b-277">このため、現在の作業を試みる前に`GridViewRow`の`DataItem`プロパティでは、最初にする必要があることを確認してを扱っていること、`DataRow`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-277">For this reason, before attempting to work with the current `GridViewRow`'s `DataItem` property, we first must make sure that we're dealing with a `DataRow`.</span></span> <span data-ttu-id="cfa7b-278">これをチェックして実現できます、`GridViewRow`の`RowType`プロパティようになります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-278">This can be accomplished by checking the `GridViewRow`'s `RowType` property like so:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a><span data-ttu-id="cfa7b-279">手順 9: が $10.00 未満には、行黄色と、UnitPrice の値を強調表示</span><span class="sxs-lookup"><span data-stu-id="cfa7b-279">Step 9: Highlighting the Row Yellow When the UnitPrice Value is Less than $10.00</span></span>

<span data-ttu-id="cfa7b-280">最後の手順ではプログラム全体を強調表示を`GridViewRow`場合、`UnitPrice`行が $10.00 未満の値します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-280">The last step is to programmatically highlight the entire `GridViewRow` if the `UnitPrice` value for that row is less than $10.00.</span></span> <span data-ttu-id="cfa7b-281">GridView の行またはセルにアクセスするための構文は、DetailsView と同じ`GridViewID.Rows(index)`、行全体にアクセスする`GridViewID.Rows(index).Cells(index)`特定のセルにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-281">The syntax for accessing a GridView's rows or cells is the same as with the DetailsView `GridViewID.Rows(index)` to access the entire row, `GridViewID.Rows(index).Cells(index)` to access a particular cell.</span></span> <span data-ttu-id="cfa7b-282">ただし、ときに、`RowDataBound`イベント ハンドラーは、データ バインドを発生させる`GridViewRow`GridView に追加するのにはまだ`Rows`コレクション。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-282">However, when the `RowDataBound` event handler fires the data bound `GridViewRow` has yet to be added to the GridView's `Rows` collection.</span></span> <span data-ttu-id="cfa7b-283">そのため、現在にアクセスできない`GridViewRow`インスタンスから、`RowDataBound`行のコレクションを使用してイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-283">Therefore you cannot access the current `GridViewRow` instance from the `RowDataBound` event handler using the Rows collection.</span></span>

<span data-ttu-id="cfa7b-284">代わりに`GridViewID.Rows(index)`、現在から参照できます`GridViewRow`インスタンス、`RowDataBound`イベント ハンドラーを使用して`e.Row`します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-284">Instead of `GridViewID.Rows(index)`, we can reference the current `GridViewRow` instance in the `RowDataBound` event handler using `e.Row`.</span></span> <span data-ttu-id="cfa7b-285">現在強調表示の順序は、`GridViewRow`インスタンスから、`RowDataBound`イベント ハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-285">That is, in order to highlight the current `GridViewRow` instance from the `RowDataBound` event handler we would use:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

<span data-ttu-id="cfa7b-286">設定ではなく、`GridViewRow`の`BackColor`プロパティを直接ものだけ作業 CSS クラスを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-286">Rather than set the `GridViewRow`'s `BackColor` property directly, let's stick with using CSS classes.</span></span> <span data-ttu-id="cfa7b-287">という名前の CSS クラスを作成しました`AffordablePriceEmphasis`背景色を黄色に設定します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-287">I've created a CSS class named `AffordablePriceEmphasis` that sets the background color to yellow.</span></span> <span data-ttu-id="cfa7b-288">完成した`RowDataBound`イベント ハンドラーに従います。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-288">The completed `RowDataBound` event handler follows:</span></span>


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


<span data-ttu-id="cfa7b-289">[![最も低コスト製品は黄色の強調表示されています。](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-289">[![The Most Affordable Products are Highlighted Yellow](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)</span></span>

<span data-ttu-id="cfa7b-290">**図 11**: 最も低コスト製品は黄色の強調表示されている ([フルサイズの画像を表示する をクリックします](custom-formatting-based-upon-data-vb/_static/image27.png))。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-290">**Figure 11**: The Most Affordable Products are Highlighted Yellow ([Click to view full-size image](custom-formatting-based-upon-data-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="cfa7b-291">まとめ</span><span class="sxs-lookup"><span data-stu-id="cfa7b-291">Summary</span></span>

<span data-ttu-id="cfa7b-292">このチュートリアルでは、GridView、DetailsView と FormView コントロールにバインドされたデータに基づいて書式設定する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-292">In this tutorial we saw how to format the GridView, DetailsView, and FormView based on the data bound to the control.</span></span> <span data-ttu-id="cfa7b-293">イベント ハンドラーを作成してこれを実現する、`DataBound`または`RowDataBound`イベント、必要な場合、書式設定の変更と共にに調査が基になるデータの場所。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-293">To accomplish this we created an event handler for the `DataBound` or `RowDataBound` events, where the underlying data was examined along with a formatting change, if needed.</span></span> <span data-ttu-id="cfa7b-294">使用して、DetailsView またはフォーム ビューにバインドされたデータにアクセスする、`DataItem`プロパティ、`DataBound`イベント ハンドラーは gridview が開きます各`GridViewRow`インスタンスの`DataItem`プロパティには、で使用可能な、その行にバインドされたデータが含まれています。`RowDataBound`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-294">To access the data bound to a DetailsView or FormView, we use the `DataItem` property in the `DataBound` event handler; for a GridView, each `GridViewRow` instance's `DataItem` property contains the data bound to that row, which is available in the `RowDataBound` event handler.</span></span>

<span data-ttu-id="cfa7b-295">データ Web コントロールの書式設定をプログラムで調整するための構文は、Web コントロールと書式設定するデータの表示方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-295">The syntax for programmatically adjusting the data Web control's formatting depends upon the Web control and how the data to be formatted is displayed.</span></span> <span data-ttu-id="cfa7b-296">DetailsView を GridView コントロールでは、行およびセルを序数インデックスによってアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-296">For DetailsView and GridView controls, the rows and cells can be accessed by an ordinal index.</span></span> <span data-ttu-id="cfa7b-297">テンプレートを使用して、フォーム ビューの`FindControl("controlID")`メソッドは、通常、テンプレート内から Web コントロールの検索に使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-297">For the FormView, which uses templates, the `FindControl("controlID")` method is commonly used to locate a Web control from within the template.</span></span>

<span data-ttu-id="cfa7b-298">次のチュートリアルでは、GridView と DetailsView をテンプレートを使用する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-298">In the next tutorial we'll look at how to use templates with the GridView and DetailsView.</span></span> <span data-ttu-id="cfa7b-299">さらに、基になるデータに基づく書式をカスタマイズするための別の手法を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-299">Additionally, we'll see another technique for customizing the formatting based on the underlying data.</span></span>

<span data-ttu-id="cfa7b-300">満足のプログラミングです。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-300">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="cfa7b-301">執筆者紹介</span><span class="sxs-lookup"><span data-stu-id="cfa7b-301">About the Author</span></span>

<span data-ttu-id="cfa7b-302">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-302">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="cfa7b-303">Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-303">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="cfa7b-304">最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-304">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="cfa7b-305">彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-305">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span> <span data-ttu-id="cfa7b-306">彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-306">or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="cfa7b-307">特別なに感謝します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-307">Special Thanks To</span></span>

<span data-ttu-id="cfa7b-308">このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-308">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="cfa7b-309">このチュートリアルでは、潜在顧客レビュー担当者が E.R.</span><span class="sxs-lookup"><span data-stu-id="cfa7b-309">Lead reviewers for this tutorial were E.R.</span></span> <span data-ttu-id="cfa7b-310">Gilmore、Dennis Patterson、および Dan Jagers します。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-310">Gilmore, Dennis Patterson, and Dan Jagers.</span></span> <span data-ttu-id="cfa7b-311">今後、MSDN の記事を確認したいですか。</span><span class="sxs-lookup"><span data-stu-id="cfa7b-311">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="cfa7b-312">場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-312">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cfa7b-313">[前へ](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [次へ](using-templatefields-in-the-gridview-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="cfa7b-313">[Previous](displaying-summary-information-in-the-gridview-s-footer-cs.md)
[Next](using-templatefields-in-the-gridview-control-vb.md)</span></span>
