---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: カスタムの書式設定データ (c#) に基づいて |Microsoft ドキュメント
author: rick-anderson
description: GridView、DetailsView、またはバインドされているデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。 このチュートリアルでは l します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 31cf628baf2250c2e7e71ab38cd64b218dc927e7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876866"
---
<a name="custom-formatting-based-upon-data-c"></a><span data-ttu-id="9941b-104">データ (c#) に基づくカスタム書式設定</span><span class="sxs-lookup"><span data-stu-id="9941b-104">Custom Formatting Based Upon Data (C#)</span></span>
====================
<span data-ttu-id="9941b-105">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="9941b-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="9941b-106">[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe)または[PDF のダウンロード](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="9941b-106">[Download Sample App](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) or [Download PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)</span></span>

> <span data-ttu-id="9941b-107">GridView、DetailsView、またはバインドされているデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。</span><span class="sxs-lookup"><span data-stu-id="9941b-107">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="9941b-108">このチュートリアルでは、データ バインドのデータ バインドおよび RowDataBound のイベント ハンドラーを使用して書式設定を実行する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="9941b-108">In this tutorial we'll look at how to accomplish data bound formatting through the use of the DataBound and RowDataBound event handlers.</span></span>


## <a name="introduction"></a><span data-ttu-id="9941b-109">はじめに</span><span class="sxs-lookup"><span data-stu-id="9941b-109">Introduction</span></span>

<span data-ttu-id="9941b-110">多数のスタイル関連プロパティの使用は、GridView、DetailsView、フォーム ビューの各コントロールの外観をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="9941b-110">The appearance of the GridView, DetailsView, and FormView controls can be customized through a myriad of style-related properties.</span></span> <span data-ttu-id="9941b-111">ようなプロパティ`CssClass`、 `Font`、 `BorderWidth`、 `BorderStyle`、 `BorderColor`、 `Width`、および`Height`、他、レンダリングされたコントロールの外観を決定します。</span><span class="sxs-lookup"><span data-stu-id="9941b-111">Properties like `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, and `Height`, among others, dictate the general appearance of the rendered control.</span></span> <span data-ttu-id="9941b-112">プロパティを含む`HeaderStyle`、 `RowStyle`、 `AlternatingRowStyle`、特定のセクションに適用される同じこれらのスタイル設定を許可するものとします。</span><span class="sxs-lookup"><span data-stu-id="9941b-112">Properties including `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, and others allow these same style settings to be applied to particular sections.</span></span> <span data-ttu-id="9941b-113">同様に、これらのスタイル設定は、フィールド レベルで適用できます。</span><span class="sxs-lookup"><span data-stu-id="9941b-113">Likewise, these style settings can be applied at the field level.</span></span>

<span data-ttu-id="9941b-114">多くのシナリオでは、書式の要件によって異なります表示されるデータの値。</span><span class="sxs-lookup"><span data-stu-id="9941b-114">In many scenarios though, the formatting requirements depend upon the value of the displayed data.</span></span> <span data-ttu-id="9941b-115">たとえばを描画する注意の製品在庫として保管する製品情報を一覧表示するレポート可能性があります設定をこれらの製品の黄色の背景色`UnitsInStock`と`UnitsOnOrder`フィールドは、どちらも 0 に等しい。</span><span class="sxs-lookup"><span data-stu-id="9941b-115">For example, to draw attention to out of stock products, a report listing product information might set the background color to yellow for those products whose `UnitsInStock` and `UnitsOnOrder` fields are both equal to 0.</span></span> <span data-ttu-id="9941b-116">高価な製品を強調表示するには、コスト太字のフォントで 75.00 ドル以上のこれらの製品の価格を表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="9941b-116">To highlight the more expensive products, we may want to display the prices of those products costing more than $75.00 in a bold font.</span></span>

<span data-ttu-id="9941b-117">GridView、DetailsView、またはバインドされているデータに基づくフォーム ビューの形式を調整することは、複数の方法で実現できます。</span><span class="sxs-lookup"><span data-stu-id="9941b-117">Adjusting the format of the GridView, DetailsView, or FormView based upon the data bound to it can be accomplished in multiple ways.</span></span> <span data-ttu-id="9941b-118">このチュートリアルでに紹介をデータ バインドを使用すると書式設定を実行する方法、`DataBound`と`RowDataBound`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="9941b-118">In this tutorial we'll look at how to accomplish data bound formatting through the use of the `DataBound` and `RowDataBound` event handlers.</span></span> <span data-ttu-id="9941b-119">次のチュートリアルでは、その他の方法を調べておみます。</span><span class="sxs-lookup"><span data-stu-id="9941b-119">In the next tutorial we'll explore an alternative approach.</span></span>

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a><span data-ttu-id="9941b-120">DetailsView コントロールを使用して`DataBound`イベント ハンドラー</span><span class="sxs-lookup"><span data-stu-id="9941b-120">Using the DetailsView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="9941b-121">データが、DetailsView にバインドされているデータ ソース コントロールから、またはコントロールのプログラムでデータを割り当てることによりとき`DataSource`プロパティと呼び出し元の`DataBind()`メソッドでは、次の一連の手順が発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-121">When data is bound to a DetailsView, either from a data source control or through programmatically assigning data to the control's `DataSource` property and calling its `DataBind()` method, the following sequence of steps occur:</span></span>

1. <span data-ttu-id="9941b-122">データ Web コントロールの`DataBinding`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-122">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="9941b-123">データは、Web コントロールのデータにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="9941b-123">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="9941b-124">データ Web コントロールの`DataBound`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-124">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="9941b-125">カスタム ロジックは、手順 1. と手順 3 でイベント ハンドラーの直後に挿入できます。</span><span class="sxs-lookup"><span data-stu-id="9941b-125">Custom logic can be injected immediately after steps 1 and 3 through an event handler.</span></span> <span data-ttu-id="9941b-126">イベント ハンドラーを作成することで、`DataBound`イベントされているデータがデータ Web コントロールにバインドされ、必要に応じて書式設定を調整するプログラムで判断できます。</span><span class="sxs-lookup"><span data-stu-id="9941b-126">By creating an event handler for the `DataBound` event we can programmatically determine the data that has been bound to the data Web control and adjust the formatting as needed.</span></span> <span data-ttu-id="9941b-127">この説明を示すためがあるが、製品に関する一般的な情報を一覧表示されます DetailsView を作成、`UnitPrice`値で、***太字、斜体のフォント***75.00 ドルを超えた場合。</span><span class="sxs-lookup"><span data-stu-id="9941b-127">To illustrate this let's create a DetailsView that will list general information about a product, but will display the `UnitPrice` value in a ***bold, italic font*** if it exceeds $75.00.</span></span>

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a><span data-ttu-id="9941b-128">手順 1: DetailsView で製品情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="9941b-128">Step 1: Displaying the Product Information in a DetailsView</span></span>

<span data-ttu-id="9941b-129">開く、 `CustomColors.aspx`  ページで、`CustomFormatting`フォルダー、DetailsView コントロールをツールボックスからデザイナーにドラッグして、設定、`ID`プロパティの値を`ExpensiveProductsPriceInBoldItalic`を起動する新しい ObjectDataSource コントロールにバインドし、 `ProductsBLL`クラスの`GetProducts()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9941b-129">Open the `CustomColors.aspx` page in the `CustomFormatting` folder, drag a DetailsView control from the Toolbox onto the Designer, set its `ID` property value to `ExpensiveProductsPriceInBoldItalic`, and bind it to a new ObjectDataSource control that invokes the `ProductsBLL` class's `GetProducts()` method.</span></span> <span data-ttu-id="9941b-130">これを実現する詳細な手順は、ここに詳細に調査前のチュートリアルで後簡略化のためここで省略されます。</span><span class="sxs-lookup"><span data-stu-id="9941b-130">The detailed steps for accomplishing this are omitted here for brevity since we examined them in detail in previous tutorials.</span></span>

<span data-ttu-id="9941b-131">DetailsView に、ObjectDataSource をバインドしたら、実行、フィールドの一覧を変更するのにはすぐになります。</span><span class="sxs-lookup"><span data-stu-id="9941b-131">Once you've bound the ObjectDataSource to the DetailsView, take a moment to modify the field list.</span></span> <span data-ttu-id="9941b-132">削除する選択した、 `ProductID`、 `SupplierID`、 `CategoryID`、 `UnitsInStock`、 `UnitsOnOrder`、 `ReorderLevel`、および`Discontinued`BoundFields の名前を変更し、残りの BoundFields を再フォーマットします。</span><span class="sxs-lookup"><span data-stu-id="9941b-132">I've opted to remove the `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, and `Discontinued` BoundFields and renamed and reformatted the remaining BoundFields.</span></span> <span data-ttu-id="9941b-133">消去したも、`Width`と`Height`設定します。</span><span class="sxs-lookup"><span data-stu-id="9941b-133">I also cleared out the `Width` and `Height` settings.</span></span> <span data-ttu-id="9941b-134">DetailsView には、単一のレコードだけが表示されたら、ためすべての製品を表示するのには、エンドユーザーに許可するためにページングを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9941b-134">Since the DetailsView displays only a single record, we need to enable paging in order to allow the end user to view all of the products.</span></span> <span data-ttu-id="9941b-135">DetailsView のスマート タグの表示でページングを有効にするチェック ボックスをオンようになります。</span><span class="sxs-lookup"><span data-stu-id="9941b-135">Do so by checking the Enable Paging checkbox in the DetailsView's smart tag.</span></span>


<span data-ttu-id="9941b-136">[![チェック ボックスを有効にするページング DetailsView のスマート タグ](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-136">[![Check the Enable Paging Checkbox in the DetailsView's Smart Tag](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="9941b-137">**図 1**: チェック ボックスを有効にするページング DetailsView のスマート タグ ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-137">**Figure 1**: Check the Enable Paging Checkbox in the DetailsView's Smart Tag ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image3.png))</span></span>


<span data-ttu-id="9941b-138">これらの変更後にマークアップを DetailsView になります。</span><span class="sxs-lookup"><span data-stu-id="9941b-138">After these changes, the DetailsView markup will be:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

<span data-ttu-id="9941b-139">すぐにこのページをブラウザーでテストをします。</span><span class="sxs-lookup"><span data-stu-id="9941b-139">Take a moment to test out this page in your browser.</span></span>


<span data-ttu-id="9941b-140">[![DetailsView コントロールでは、一度に 1 つの製品が表示されます。](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-140">[![The DetailsView Control Displays One Product at a Time](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)</span></span>

<span data-ttu-id="9941b-141">**図 2**:「DetailsView コントロールが表示されます 1 つの製品、一度に ([フルサイズ イメージを表示するに、をクリックして](custom-formatting-based-upon-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-141">**Figure 2**: The DetailsView Control Displays One Product at a Time ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image6.png))</span></span>


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="9941b-142">手順 2: プログラムによって、データ バインドされたイベント ハンドラー内のデータの値の決定</span><span class="sxs-lookup"><span data-stu-id="9941b-142">Step 2: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="9941b-143">太字、斜体のフォントをこれらの製品の価格を表示するために持つ`UnitPrice`値 75.00 ドルを超えています、最初にプログラムで確認できる必要があります、`UnitPrice`値。</span><span class="sxs-lookup"><span data-stu-id="9941b-143">In order to display the price in a bold, italic font for those products whose `UnitPrice` value exceeds $75.00, we need to first be able to programmatically determine the `UnitPrice` value.</span></span> <span data-ttu-id="9941b-144">これは、detailsview で実行できます、`DataBound`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="9941b-144">For the DetailsView, this can be accomplished in the `DataBound` event handler.</span></span> <span data-ttu-id="9941b-145">イベントを作成するには、ハンドラーはデザイナーで DetailsView をクリックし、[プロパティ] ウィンドウに移動します。</span><span class="sxs-lookup"><span data-stu-id="9941b-145">To create the event handler click on the DetailsView in the Designer then navigate to the Properties window.</span></span> <span data-ttu-id="9941b-146">表示、または、[表示] メニューに移動ではない場合は、表示、f4 キーを押して、[プロパティ] ウィンドウのメニュー オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="9941b-146">Press F4 to bring it up, if it's not visible, or go to the View menu and select the Properties Window menu option.</span></span> <span data-ttu-id="9941b-147">[プロパティ] ウィンドウを DetailsView のイベントを一覧表示される稲妻アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9941b-147">From the Properties window, click on the lightning bolt icon to list the DetailsView's events.</span></span> <span data-ttu-id="9941b-148">次をダブルクリックするか、`DataBound`イベントまたはイベント ハンドラーを作成する名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="9941b-148">Next, either double-click the `DataBound` event or type in the name of the event handler you want to create.</span></span>


![データ バインドされたイベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-cs/_static/image7.png)

<span data-ttu-id="9941b-150">**図 3**: イベント ハンドラーを作成、`DataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="9941b-150">**Figure 3**: Create an Event Handler for the `DataBound` Event</span></span>


<span data-ttu-id="9941b-151">これは自動的にイベント ハンドラーを作成に移動コード部分が追加されました。</span><span class="sxs-lookup"><span data-stu-id="9941b-151">Doing so will automatically create the event handler and take you to the code portion where it has been added.</span></span> <span data-ttu-id="9941b-152">この時点で表示されます。</span><span class="sxs-lookup"><span data-stu-id="9941b-152">At this point you will see:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

<span data-ttu-id="9941b-153">DetailsView にバインドされたデータは、経由でアクセスできる、`DataItem`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="9941b-153">The data bound to the DetailsView can be accessed via the `DataItem` property.</span></span> <span data-ttu-id="9941b-154">厳密に型指定された DataRow インスタンスのコレクションから成るは厳密に型指定された DataTable に、コントロールのバインドはおことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9941b-154">Recall that we are binding our controls to a strongly-typed DataTable, which is composed of a collection of strongly-typed DataRow instances.</span></span> <span data-ttu-id="9941b-155">DetailsView の DataTable 内の最初の DataRow が割り当てられている DataTable が DetailsView にバインドされると、`DataItem`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="9941b-155">When the DataTable is bound to the DetailsView, the first DataRow in the DataTable is assigned to the DetailsView's `DataItem` property.</span></span> <span data-ttu-id="9941b-156">具体的には、`DataItem`プロパティが割り当てられている、`DataRowView`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="9941b-156">Specifically, the `DataItem` property is assigned a `DataRowView` object.</span></span> <span data-ttu-id="9941b-157">使用して、`DataRowView`の`Row`これは、基になる DataRow オブジェクトへのアクセスを取得するプロパティが、実際に`ProductsRow`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="9941b-157">We can use the `DataRowView`'s `Row` property to get access to the underlying DataRow object, which is actually a `ProductsRow` instance.</span></span> <span data-ttu-id="9941b-158">これを取得したら`ProductsRow`インスタンス オブジェクトのプロパティの値を検査するだけで決定することできます。</span><span class="sxs-lookup"><span data-stu-id="9941b-158">Once we have this `ProductsRow` instance we can make our decision by simply inspecting the object's property values.</span></span>

<span data-ttu-id="9941b-159">次のコード例を確認するかどうか、 `UnitPrice` DetailsView コントロールにバインドされている値 75.00 ドルを超えています。</span><span class="sxs-lookup"><span data-stu-id="9941b-159">The following code illustrates how to determine whether the `UnitPrice` value bound to the DetailsView control is greater than $75.00:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="9941b-160">`UnitPrice`持つことができます、`NULL`データベース内の値は、最初に確認で扱っていないかどうかを確認する、`NULL`値にアクセスする前に、`ProductsRow`の`UnitPrice`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="9941b-160">Since `UnitPrice` can have a `NULL` value in the database, we first check to make sure that we're not dealing with a `NULL` value before accessing the `ProductsRow`'s `UnitPrice` property.</span></span> <span data-ttu-id="9941b-161">このチェックは重要なためにアクセスしようとしている場合、`UnitPrice`プロパティにある場合に、`NULL`値、`ProductsRow`オブジェクトがスローされます、 [StrongTypingException 例外](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="9941b-161">This check is important because if we attempt to access the `UnitPrice` property when it has a `NULL` value the `ProductsRow` object will throw a [StrongTypingException exception](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).</span></span>


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a><span data-ttu-id="9941b-162">手順 3: DetailsView で UnitPrice の値の書式設定</span><span class="sxs-lookup"><span data-stu-id="9941b-162">Step 3: Formatting the UnitPrice Value in the DetailsView</span></span>

<span data-ttu-id="9941b-163">この時点で判断できるかどうか、 `UnitPrice` DetailsView にバインドされる値を超える $75.00、値が DetailsView をプログラムで調整する方法についての形式に応じてまだ示しました。</span><span class="sxs-lookup"><span data-stu-id="9941b-163">At this point we can determine whether the `UnitPrice` value bound to the DetailsView has a value that exceeds $75.00, but we've yet to see how to programmatically adjust the DetailsView's formatting accordingly.</span></span> <span data-ttu-id="9941b-164">DetailsView で行全体の書式設定を変更するプログラムでアクセスを使用して行`DetailsViewID.Rows[index]`へのアクセスを使用して、特定のセルを変更する;`DetailsViewID.Rows[index].Cells[index]`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-164">To modify the formatting of an entire row in the DetailsView, programmatically access the row using `DetailsViewID.Rows[index]`; to modify a particular cell, access use `DetailsViewID.Rows[index].Cells[index]`.</span></span> <span data-ttu-id="9941b-165">行またはセルの外観のスタイル関連プロパティを設定し、調整できますへの参照を作成したらです。</span><span class="sxs-lookup"><span data-stu-id="9941b-165">Once we have a reference to the row or cell we can then adjust its appearance by setting its style-related properties.</span></span>

<span data-ttu-id="9941b-166">行をプログラムでアクセスするには、0 から始まる行のインデックスがわかっている必要があります。</span><span class="sxs-lookup"><span data-stu-id="9941b-166">Accessing a row programmatically requires that you know the row's index, which starts at 0.</span></span> <span data-ttu-id="9941b-167">`UnitPrice`行が 4 のインデックスを付けることと、プログラムでアクセスできるように、DetailsView の 5 番目の行を使用して`ExpensiveProductsPriceInBoldItalic.Rows[4]`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-167">The `UnitPrice` row is the fifth row in the DetailsView, giving it an index of 4 and making it programmatically accessible using `ExpensiveProductsPriceInBoldItalic.Rows[4]`.</span></span> <span data-ttu-id="9941b-168">この時点で、次のコードを使用して、太字、斜体のフォントで表示される行全体のコンテンツがあります。</span><span class="sxs-lookup"><span data-stu-id="9941b-168">At this point we could have the entire row's content displayed in a bold, italic font by using the following code:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

<span data-ttu-id="9941b-169">ただし、これにより、*両方*ラベル (価格) と太字と斜体の値。</span><span class="sxs-lookup"><span data-stu-id="9941b-169">However, this will make *both* the label (Price) and the value bold and italic.</span></span> <span data-ttu-id="9941b-170">値だけ太字と斜体いただくために設定する行で、これは、次を使用して実現できます 2 番目のセルを書式設定を確認します。 場合</span><span class="sxs-lookup"><span data-stu-id="9941b-170">If we want to make just the value bold and italic we need to apply this formatting to the second cell in the row, which can be accomplished using the following:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

<span data-ttu-id="9941b-171">これまでのチュートリアルは、レンダリングされるマークアップとスタイルに関連する情報の間で明確に分離を維持するためにスタイル シートを使用して、以降は、上記のようにしてみましょう代わりに特定のスタイル プロパティの設定ではなく、CSS クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="9941b-171">Since our tutorials thus far have used stylesheets to maintain a clean separation between the rendered markup and style-related information, rather than setting the specific style properties as shown above let's instead use a CSS class.</span></span> <span data-ttu-id="9941b-172">開く、 `Styles.css` stylesheet という名前の新しい CSS クラスを追加および`ExpensivePriceEmphasis`を次の定義。</span><span class="sxs-lookup"><span data-stu-id="9941b-172">Open the `Styles.css` stylesheet and add a new CSS class named `ExpensivePriceEmphasis` with the following definition:</span></span>


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

<span data-ttu-id="9941b-173">次に、 `DataBound` 、イベント ハンドラーを設定、セルの`CssClass`プロパティを`ExpensivePriceEmphasis`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-173">Then, in the `DataBound` event handler, set the cell's `CssClass` property to `ExpensivePriceEmphasis`.</span></span> <span data-ttu-id="9941b-174">次のコードは、`DataBound`全体のイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="9941b-174">The following code shows the `DataBound` event handler in its entirety:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

<span data-ttu-id="9941b-175">通常フォントで価格が表示されます、Chai 75.00 ドル未満のコスト、これを表示するときに (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="9941b-175">When viewing Chai, which costs less than $75.00, the price is displayed in a normal font (see Figure 4).</span></span> <span data-ttu-id="9941b-176">ただし、97.00 ドルの価格を持つ、Mishi 日本の神戸 Niku を表示するときに、価格が表示されます、太字、斜体のフォントで (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="9941b-176">However, when viewing Mishi Kobe Niku, which has a price of $97.00, the price is displayed in a bold, italic font (see Figure 5).</span></span>


<span data-ttu-id="9941b-177">[![通常フォントで $75.00 より低い価格が表示されます。](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-177">[![Prices Less than $75.00 are Displayed in a Normal Font](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)</span></span>

<span data-ttu-id="9941b-178">**図 4**: 標準フォントで $75.00 より低い価格が表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-178">**Figure 4**: Prices Less than $75.00 are Displayed in a Normal Font ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="9941b-179">[![負荷の高い製品の価格が太字、斜体のフォントで表示されます。](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-179">[![Expensive Products' Prices are Displayed in a Bold, Italic Font](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="9941b-180">**図 5**: 高価な製品の価格が太字、斜体のフォントで表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-cs/_static/image13.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-180">**Figure 5**: Expensive Products' Prices are Displayed in a Bold, Italic Font ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image13.png))</span></span>


## <a name="using-the-formview-controlsdataboundevent-handler"></a><span data-ttu-id="9941b-181">FormView のコントロールを使用して`DataBound`イベント ハンドラー</span><span class="sxs-lookup"><span data-stu-id="9941b-181">Using the FormView Control's`DataBound`Event Handler</span></span>

<span data-ttu-id="9941b-182">フォーム ビューにバインドされている基になるデータを決定する手順は、DetailsView を作成するものと同じ、 `DataBound` 、イベント ハンドラーのキャスト、`DataItem`プロパティを適切なオブジェクト型が、コントロールにバインドされ、続行する方法を決定します。</span><span class="sxs-lookup"><span data-stu-id="9941b-182">The steps for determining the underlying data bound to a FormView are identical to those for a DetailsView create a `DataBound` event handler, cast the `DataItem` property to the appropriate object type bound to the control, and determine how to proceed.</span></span> <span data-ttu-id="9941b-183">FormView DetailsView が違う、ただし、独自のユーザー インターフェイスの外観を更新する方法です。</span><span class="sxs-lookup"><span data-stu-id="9941b-183">The FormView and DetailsView differ, however, in how their user interface's appearance is updated.</span></span>

<span data-ttu-id="9941b-184">FormView 任意 BoundFields が含まれていないとがないため、`Rows`コレクション。</span><span class="sxs-lookup"><span data-stu-id="9941b-184">The FormView does not contain any BoundFields and therefore lacks the `Rows` collection.</span></span> <span data-ttu-id="9941b-185">静的な HTML の組み合わせを含めることができる、テンプレートのフォーム ビューを構成する代わりに、Web コントロール、およびデータ バインド構文です。</span><span class="sxs-lookup"><span data-stu-id="9941b-185">Instead, a FormView is composed of templates, which can contain a mix of static HTML, Web controls, and databinding syntax.</span></span> <span data-ttu-id="9941b-186">通常、フォーム ビューのスタイルを調整するには、Web コントロールをフォーム ビューのテンプレート内での 1 つ以上のスタイルを調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9941b-186">Adjusting the style of a FormView typically involves adjusting the style of one or more of the Web controls within the FormView's templates.</span></span>

<span data-ttu-id="9941b-187">これを示すためには、みましょうみましょうなど、前の例はこの時点で一覧の製品をフォーム ビューを使用して表示製品名とユニットのみ在庫商品の在庫が 10 以下である場合は、赤いフォントで表示される数にします。</span><span class="sxs-lookup"><span data-stu-id="9941b-187">To illustrate this, let's use a FormView to list products like in the previous example, but this time let's display just the product name and units in stock with the units in stock displayed in a red font if it is less than or equal to 10.</span></span>

## <a name="step-4-displaying-the-product-information-in-a-formview"></a><span data-ttu-id="9941b-188">手順 4: FormView で製品情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="9941b-188">Step 4: Displaying the Product Information in a FormView</span></span>

<span data-ttu-id="9941b-189">フォーム ビューの追加、 `CustomColors.aspx` DetailsView やセットの下のページ、`ID`プロパティを`LowStockedProductsInRed`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-189">Add a FormView to the `CustomColors.aspx` page beneath the DetailsView and set its `ID` property to `LowStockedProductsInRed`.</span></span> <span data-ttu-id="9941b-190">FormView を前の手順で作成された ObjectDataSource コントロールにバインドします。</span><span class="sxs-lookup"><span data-stu-id="9941b-190">Bind the FormView to the ObjectDataSource control created from the previous step.</span></span> <span data-ttu-id="9941b-191">これが作成されます、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`FormView 用です。</span><span class="sxs-lookup"><span data-stu-id="9941b-191">This will create an `ItemTemplate`, `EditItemTemplate`, and `InsertItemTemplate` for the FormView.</span></span> <span data-ttu-id="9941b-192">削除、`EditItemTemplate`と`InsertItemTemplate`および簡略化、`ItemTemplate`に含めるだけ`ProductName`と`UnitsInStock`独自の適切な名前のラベル コントロールでは、それぞれの値します。</span><span class="sxs-lookup"><span data-stu-id="9941b-192">Remove the `EditItemTemplate` and `InsertItemTemplate` and simplify the `ItemTemplate` to include just the `ProductName` and `UnitsInStock` values, each in their own appropriately-named Label controls.</span></span> <span data-ttu-id="9941b-193">同様に、前の例から DetailsView も FormView のスマート タグの表示でページングを有効にするチェック ボックスを確認します。</span><span class="sxs-lookup"><span data-stu-id="9941b-193">As with the DetailsView from the earlier example, also check the Enable Paging checkbox in the FormView's smart tag.</span></span>

<span data-ttu-id="9941b-194">これらの編集後に、フォーム ビューのマークアップを次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9941b-194">After these edits your FormView's markup should look similar to the following:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

<span data-ttu-id="9941b-195">なお、`ItemTemplate`が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9941b-195">Note that the `ItemTemplate` contains:</span></span>

- <span data-ttu-id="9941b-196">**静的な HTML**テキスト"製品:"と"在庫:"と共に、`<br />`と`<b>`要素。</span><span class="sxs-lookup"><span data-stu-id="9941b-196">**Static HTML** the text "Product:" and "Units In Stock:" along with the `<br />` and `<b>` elements.</span></span>
- <span data-ttu-id="9941b-197">**Web コントロール**2 つのラベル コントロール`ProductNameLabel`と`UnitsInStockLabel`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-197">**Web controls** the two Label controls, `ProductNameLabel` and `UnitsInStockLabel`.</span></span>
- <span data-ttu-id="9941b-198">**データ バインド構文**、`<%# Bind("ProductName") %>`と`<%# Bind("UnitsInStock") %>`構文は、これらのフィールドからラベル コントロールの値を割り当てます`Text`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="9941b-198">**Databinding syntax** the `<%# Bind("ProductName") %>` and `<%# Bind("UnitsInStock") %>` syntax, which assigns the values from these fields to the Label controls' `Text` properties.</span></span>

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a><span data-ttu-id="9941b-199">手順 5: プログラムによって、データ バインドされたイベント ハンドラー内のデータの値の決定</span><span class="sxs-lookup"><span data-stu-id="9941b-199">Step 5: Programmatically Determining the Value of the Data in the DataBound Event Handler</span></span>

<span data-ttu-id="9941b-200">FormView のマークアップが完了したら、次の手順がどうかをプログラムで判断は、`UnitsInStock`値が 10 未満です。</span><span class="sxs-lookup"><span data-stu-id="9941b-200">With the FormView's markup complete, the next step is to programmatically determine if the `UnitsInStock` value is less than or equal to 10.</span></span> <span data-ttu-id="9941b-201">DetailsView でが FormView でまったく同じ方法で行われます。</span><span class="sxs-lookup"><span data-stu-id="9941b-201">This is accomplished in the exact same manner with the FormView as it was with the DetailsView.</span></span> <span data-ttu-id="9941b-202">FormView のイベント ハンドラーを作成して開始`DataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="9941b-202">Start by creating an event handler for the FormView's `DataBound` event.</span></span>


![データ バインドされたイベント ハンドラーを作成します。](custom-formatting-based-upon-data-cs/_static/image14.png)

<span data-ttu-id="9941b-204">**図 6**: 作成、`DataBound`イベント ハンドラー</span><span class="sxs-lookup"><span data-stu-id="9941b-204">**Figure 6**: Create the `DataBound` Event Handler</span></span>


<span data-ttu-id="9941b-205">イベント ハンドラーにキャスト FormView の`DataItem`プロパティを`ProductsRow`インスタンス化し、確認するかどうか、`UnitsInPrice`値のある赤いフォントで表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9941b-205">In the event handler cast the FormView's `DataItem` property to a `ProductsRow` instance and determine whether the `UnitsInPrice` value is such that we need to display it in a red font.</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a><span data-ttu-id="9941b-206">手順 6: FormView の ItemTemplate の UnitsInStockLabel ラベル コントロールの書式設定</span><span class="sxs-lookup"><span data-stu-id="9941b-206">Step 6: Formatting the UnitsInStockLabel Label Control in the FormView's ItemTemplate</span></span>

<span data-ttu-id="9941b-207">最後の手順は、表示されている書式設定を`UnitsInStock`赤いフォントで値の場合は、値は 10 個以下です。</span><span class="sxs-lookup"><span data-stu-id="9941b-207">The final step is to format the displayed `UnitsInStock` value in a red font if the value is 10 or less.</span></span> <span data-ttu-id="9941b-208">プログラムにアクセスする必要があります。 これを実現する、`UnitsInStockLabel`内の制御、`ItemTemplate`し、そのテキストが赤で表示されるように、そのスタイル プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="9941b-208">To accomplish this we need to programmatically access the `UnitsInStockLabel` control in the `ItemTemplate` and set its style properties so that its text is displayed in red.</span></span> <span data-ttu-id="9941b-209">テンプレート内の Web コントロールにアクセスするには、使用、`FindControl("controlID")`次のようなメソッド。</span><span class="sxs-lookup"><span data-stu-id="9941b-209">To access a Web control in a template, use the `FindControl("controlID")` method like this:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

<span data-ttu-id="9941b-210">いるコントロールのラベルにアクセスする例`ID`値は`UnitsInStockLabel`のでを使用します。</span><span class="sxs-lookup"><span data-stu-id="9941b-210">For our example we want to access a Label control whose `ID` value is `UnitsInStockLabel`, so we'd use:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

<span data-ttu-id="9941b-211">Web コントロールへのプログラムによる参照を作成したら、必要に応じてそのスタイル関連プロパティ変更できます。</span><span class="sxs-lookup"><span data-stu-id="9941b-211">Once we have a programmatic reference to the Web control, we can modify its style-related properties as needed.</span></span> <span data-ttu-id="9941b-212">前の例では、内の CSS クラスを作成しました、`Styles.css`という`LowUnitsInStockEmphasis`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-212">As with the earlier example, I've created a CSS class in `Styles.css` named `LowUnitsInStockEmphasis`.</span></span> <span data-ttu-id="9941b-213">ラベルの Web コントロールには、このスタイルを適用するに次のように設定します。 その`CssClass`プロパティに応じて。</span><span class="sxs-lookup"><span data-stu-id="9941b-213">To apply this style to the Label Web control, set its `CssClass` property accordingly.</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="9941b-214">プログラムによって、コントロールを使用して Web にアクセスするテンプレートを書式設定するための構文`FindControl("controlID")`し、そのプロパティを設定スタイルに関連することもできますを使用する場合と[TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView または GridView制御します。</span><span class="sxs-lookup"><span data-stu-id="9941b-214">The syntax for formatting a template programmatically accessing the Web control using `FindControl("controlID")` and then setting its style-related properties can also be used when using [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) in the DetailsView or GridView controls.</span></span> <span data-ttu-id="9941b-215">[次へ]、チュートリアルで TemplateFields について確認します。</span><span class="sxs-lookup"><span data-stu-id="9941b-215">We'll examine TemplateFields in our next tutorial.</span></span>


<span data-ttu-id="9941b-216">製品を表示するときに、図 7 は FormView を示しています。 が`UnitsInStock`図 8 に製品が、値が 10 未満の値は 10 より大きくします。</span><span class="sxs-lookup"><span data-stu-id="9941b-216">Figures 7 shows the FormView when viewing a product whose `UnitsInStock` value is greater than 10, while the product in Figure 8 has its value less than 10.</span></span>


<span data-ttu-id="9941b-217">[![製品と、十分に大きな Units In Stock、カスタム書式が適用されます。](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-217">[![For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)</span></span>

<span data-ttu-id="9941b-218">**図 7**: の製品では十分に大きな Units In Stock、カスタム書式の適用 ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-cs/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-218">**Figure 7**: For Products With a Sufficiently Large Units In Stock, No Custom Formatting is Applied ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image17.png))</span></span>


<span data-ttu-id="9941b-219">[![在庫数の単位は、製品の値を 10 以下の赤で表示されます。](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-219">[![The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)</span></span>

<span data-ttu-id="9941b-220">**図 8**: 在庫数のユニットが、それらの製品での値が 10 以下赤で表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-cs/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-220">**Figure 8**: The Units in Stock Number is Shown in Red for Those Products With Values of 10 or Less ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image20.png))</span></span>


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a><span data-ttu-id="9941b-221">GridView の書式が設定`RowDataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="9941b-221">Formatting with the GridView's`RowDataBound`Event</span></span>

<span data-ttu-id="9941b-222">以前 DetailsView の手順の順序を調べたりお、FormView では、データ バインド中に使用して進行状況を制御します。</span><span class="sxs-lookup"><span data-stu-id="9941b-222">Earlier we examined the sequence of steps the DetailsView and FormView controls progress through during databinding.</span></span> <span data-ttu-id="9941b-223">見てみましょうこれらの手順をもう一度リフレッシャーとして。</span><span class="sxs-lookup"><span data-stu-id="9941b-223">Let's look over these steps once again as a refresher.</span></span>

1. <span data-ttu-id="9941b-224">データ Web コントロールの`DataBinding`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-224">The data Web control's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="9941b-225">データは、Web コントロールのデータにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="9941b-225">The data is bound to the data Web control.</span></span>
3. <span data-ttu-id="9941b-226">データ Web コントロールの`DataBound`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-226">The data Web control's `DataBound` event fires.</span></span>

<span data-ttu-id="9941b-227">次の 3 つの単純な手順は、単一のレコードだけを表示するので、DetailsView と FormView 十分です。</span><span class="sxs-lookup"><span data-stu-id="9941b-227">These three simple steps are sufficient for the DetailsView and FormView because they display only a single record.</span></span> <span data-ttu-id="9941b-228">表示する GridView の*すべて*バインドされているレコードに (だけでなく、最初の)、手順 2 がもう少し複雑です。</span><span class="sxs-lookup"><span data-stu-id="9941b-228">For the GridView, which displays *all* records bound to it (not just the first), step 2 is a bit more involved.</span></span>

<span data-ttu-id="9941b-229">手順 2 GridView は、データ ソースを列挙し、各レコード作成で、`GridViewRow`をインスタンス化し、現在のレコードをバインドします。</span><span class="sxs-lookup"><span data-stu-id="9941b-229">In step 2 the GridView enumerates the data source and, for each record, creates a `GridViewRow` instance and binds the current record to it.</span></span> <span data-ttu-id="9941b-230">各`GridViewRow`GridView に追加された、2 つのイベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-230">For each `GridViewRow` added to the GridView, two events are raised:</span></span>

- <span data-ttu-id="9941b-231">**`RowCreated`** 後に起動、`GridViewRow`が作成されました</span><span class="sxs-lookup"><span data-stu-id="9941b-231">**`RowCreated`** fires after the `GridViewRow` has been created</span></span>
- <span data-ttu-id="9941b-232">**`RowDataBound`** 現在のレコードにバインドされた後に発生、`GridViewRow`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-232">**`RowDataBound`** fires after the current record has been bound to the `GridViewRow`.</span></span>

<span data-ttu-id="9941b-233">GridView し、データ バインディングがより正確に記載されている、次の一連の手順で。</span><span class="sxs-lookup"><span data-stu-id="9941b-233">For the GridView, then, data binding is more accurately described by the following sequence of steps:</span></span>

1. <span data-ttu-id="9941b-234">GridView の`DataBinding`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-234">The GridView's `DataBinding` event fires.</span></span>
2. <span data-ttu-id="9941b-235">データが GridView にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="9941b-235">The data is bound to the GridView.</span></span>   
  
   <span data-ttu-id="9941b-236">データ ソース内の各レコード</span><span class="sxs-lookup"><span data-stu-id="9941b-236">For each record in the data source</span></span> 

    1. <span data-ttu-id="9941b-237">作成、`GridViewRow`オブジェクト</span><span class="sxs-lookup"><span data-stu-id="9941b-237">Create a `GridViewRow` object</span></span>
    2. <span data-ttu-id="9941b-238">Fire、`RowCreated`イベント</span><span class="sxs-lookup"><span data-stu-id="9941b-238">Fire the `RowCreated` event</span></span>
    3. <span data-ttu-id="9941b-239">レコードにバインドします `GridViewRow`</span><span class="sxs-lookup"><span data-stu-id="9941b-239">Bind the record to the `GridViewRow`</span></span>
    4. <span data-ttu-id="9941b-240">Fire、`RowDataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="9941b-240">Fire the `RowDataBound` event</span></span>
    5. <span data-ttu-id="9941b-241">追加、`GridViewRow`を`Rows`コレクション</span><span class="sxs-lookup"><span data-stu-id="9941b-241">Add the `GridViewRow` to the `Rows` collection</span></span>
3. <span data-ttu-id="9941b-242">GridView の`DataBound`イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-242">The GridView's `DataBound` event fires.</span></span>

<span data-ttu-id="9941b-243">GridView の個々 のレコードの形式をカスタマイズするには、次に、必要がありますのイベント ハンドラーを作成する、`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="9941b-243">To customize the format of the GridView's individual records, then, we need to create an event handler for the `RowDataBound` event.</span></span> <span data-ttu-id="9941b-244">これを示すためには、追加する GridView、`CustomColors.aspx`名前、カテゴリ、および各製品の価格がより小さい $10.00 を黄色の背景色では、これらの製品を強調表示価格を一覧するページ。</span><span class="sxs-lookup"><span data-stu-id="9941b-244">To illustrate this, let's add a GridView to the `CustomColors.aspx` page that lists the name, category, and price for each product, highlighting those products whose price is less than $10.00 with a yellow background color.</span></span>

## <a name="step-7-displaying-product-information-in-a-gridview"></a><span data-ttu-id="9941b-245">手順 7: GridView で製品情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="9941b-245">Step 7: Displaying Product Information in a GridView</span></span>

<span data-ttu-id="9941b-246">前の例から、フォーム ビューの下にある GridView を追加し、設定、`ID`プロパティを`HighlightCheapProducts`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-246">Add a GridView beneath the FormView from the previous example and set its `ID` property to `HighlightCheapProducts`.</span></span> <span data-ttu-id="9941b-247">ページ上のすべての製品を返す ObjectDataSource が既にある、ために、GridView をバインドします。</span><span class="sxs-lookup"><span data-stu-id="9941b-247">Since we already have an ObjectDataSource that returns all products on the page, bind the GridView to that.</span></span> <span data-ttu-id="9941b-248">最後に、製品の名前、カテゴリ、および価格だけを含める GridView の BoundFields を編集します。</span><span class="sxs-lookup"><span data-stu-id="9941b-248">Finally, edit the GridView's BoundFields to include just the products' names, categories, and prices.</span></span> <span data-ttu-id="9941b-249">これらの編集した後、GridView のマークアップをようになります。</span><span class="sxs-lookup"><span data-stu-id="9941b-249">After these edits the GridView's markup should look like:</span></span>


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

<span data-ttu-id="9941b-250">図 9 では、ブラウザーで表示したときに、このポイントに作業の進行状況を示します。</span><span class="sxs-lookup"><span data-stu-id="9941b-250">Figure 9 shows our progress to this point when viewed through a browser.</span></span>


<span data-ttu-id="9941b-251">[![GridView の名前、カテゴリ、および各製品の価格を一覧表示します。](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-251">[![The GridView Lists the Name, Category, and Price For Each Product](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)</span></span>

<span data-ttu-id="9941b-252">**図 9**: GridView リスト名、カテゴリ、および各製品の価格 ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-cs/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-252">**Figure 9**: The GridView Lists the Name, Category, and Price For Each Product ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image23.png))</span></span>


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a><span data-ttu-id="9941b-253">手順 8: プログラムによって、RowDataBound イベント ハンドラー内のデータの値の決定</span><span class="sxs-lookup"><span data-stu-id="9941b-253">Step 8: Programmatically Determining the Value of the Data in the RowDataBound Event Handler</span></span>

<span data-ttu-id="9941b-254">ときに、 `ProductsDataTable` GridView にバインドされてその`ProductsRow`インスタンスが列挙され、各`ProductsRow`、`GridViewRow`を作成します。</span><span class="sxs-lookup"><span data-stu-id="9941b-254">When the `ProductsDataTable` is bound to the GridView its `ProductsRow` instances are enumerated and for each `ProductsRow` a `GridViewRow` is created.</span></span> <span data-ttu-id="9941b-255">`GridViewRow`の`DataItem`特定するプロパティが割り当てられている`ProductRow`、その後、GridView の`RowDataBound`イベント ハンドラーが呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="9941b-255">The `GridViewRow`'s `DataItem` property is assigned to the particular `ProductRow`, after which the GridView's `RowDataBound` event handler is raised.</span></span> <span data-ttu-id="9941b-256">決定する、`UnitPrice`製品ごとの値は、GridView にバインドし、gridview のイベント ハンドラーを作成する必要があります`RowDataBound`イベント。</span><span class="sxs-lookup"><span data-stu-id="9941b-256">To determine the `UnitPrice` value for each product bound to the GridView, then, we need to create an event handler for the GridView's `RowDataBound` event.</span></span> <span data-ttu-id="9941b-257">このイベント ハンドラーでおを検査できる、`UnitPrice`現在の値`GridViewRow`その行の書式設定に決定します。</span><span class="sxs-lookup"><span data-stu-id="9941b-257">In this event handler we can inspect the `UnitPrice` value for the current `GridViewRow` and make a formatting decision for that row.</span></span>

<span data-ttu-id="9941b-258">このイベント ハンドラーは、フォーム ビューと DetailsView で同じ一連の手順としてを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="9941b-258">This event handler can be created using the same series of steps as with the FormView and DetailsView.</span></span>


![GridView の RowDataBound イベントのイベント ハンドラーを作成します。](custom-formatting-based-upon-data-cs/_static/image24.png)

<span data-ttu-id="9941b-260">**図 10**: gridview のイベント ハンドラーを作成する`RowDataBound`イベント</span><span class="sxs-lookup"><span data-stu-id="9941b-260">**Figure 10**: Create an Event Handler for the GridView's `RowDataBound` Event</span></span>


<span data-ttu-id="9941b-261">この方法でイベント ハンドラーを作成すると、ASP.NET ページのコードの部分に自動的に追加するのには、次のコードが発生します。</span><span class="sxs-lookup"><span data-stu-id="9941b-261">Creating the event hander in this manner will cause the following code to be automatically added to the ASP.NET page's code portion:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

<span data-ttu-id="9941b-262">ときに、`RowDataBound`イベントの起動、イベント ハンドラーとして渡される 2 番目のパラメーター型のオブジェクト`GridViewRowEventArgs`、という名前のプロパティを持つ`Row`します。</span><span class="sxs-lookup"><span data-stu-id="9941b-262">When the `RowDataBound` event fires, the event handler is passed as its second parameter an object of type `GridViewRowEventArgs`, which has a property named `Row`.</span></span> <span data-ttu-id="9941b-263">このプロパティへの参照を返します、`GridViewRow`バインドされたデータだけでした。</span><span class="sxs-lookup"><span data-stu-id="9941b-263">This property returns a reference to the `GridViewRow` that was just data bound.</span></span> <span data-ttu-id="9941b-264">アクセスする、`ProductsRow`インスタンスにバインド、`GridViewRow`使用して、`DataItem`プロパティ次のようにします。</span><span class="sxs-lookup"><span data-stu-id="9941b-264">To access the `ProductsRow` instance bound to the `GridViewRow` we use the `DataItem` property like so:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

<span data-ttu-id="9941b-265">使用する場合、`RowDataBound`イベント ハンドラーが GridView は、さまざまな種類の行で構成される、このイベントが発生したことに注意する重要*すべて*行の型。</span><span class="sxs-lookup"><span data-stu-id="9941b-265">When working with the `RowDataBound` event handler it is important to keep in mind that the GridView is composed of different types of rows and that this event is fired for *all* row types.</span></span> <span data-ttu-id="9941b-266">A`GridViewRow`の型によって決定できる、`RowType`プロパティ、有効な値のいずれかを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="9941b-266">A `GridViewRow`'s type can be determined by its `RowType` property, and can have one of the possible values:</span></span>

- <span data-ttu-id="9941b-267">`DataRow` GridView からのレコードにバインドされている行 `DataSource`</span><span class="sxs-lookup"><span data-stu-id="9941b-267">`DataRow` a row that is bound to a record from the GridView's `DataSource`</span></span>
- <span data-ttu-id="9941b-268">`EmptyDataRow` 表示される場合、行、GridView の`DataSource`が空です</span><span class="sxs-lookup"><span data-stu-id="9941b-268">`EmptyDataRow` the row displayed if the GridView's `DataSource` is empty</span></span>
- <span data-ttu-id="9941b-269">`Footer` フッター行です。表示されている場合、GridView の`ShowFooter`プロパティに設定 `true`</span><span class="sxs-lookup"><span data-stu-id="9941b-269">`Footer` the footer row; shown if the GridView's `ShowFooter` property is set to `true`</span></span>
- <span data-ttu-id="9941b-270">`Header` ヘッダー行です。GridView の ShowHeader プロパティが に設定されているかどうかを示す`true`(既定)</span><span class="sxs-lookup"><span data-stu-id="9941b-270">`Header` the header row; shown if the GridView's ShowHeader property is set to `true` (the default)</span></span>
- <span data-ttu-id="9941b-271">`Pager` ページング インターフェイスを表示する行のページングを実装する gridview</span><span class="sxs-lookup"><span data-stu-id="9941b-271">`Pager` for GridView's that implement paging, the row that displays the paging interface</span></span>
- <span data-ttu-id="9941b-272">`Separator` GridView は使用されませんで使用される、 `RowType` DataList およびリピータのプロパティを制御する 2 つのデータについて説明します将来的にチュートリアルの Web コントロール</span><span class="sxs-lookup"><span data-stu-id="9941b-272">`Separator` not used for the GridView, but used by the `RowType` properties for the DataList and Repeater controls, two data Web controls we'll discuss in future tutorials</span></span>

<span data-ttu-id="9941b-273">`EmptyDataRow`、 `Header`、 `Footer`、および`Pager`にない行が関連付けられている、`DataSource`レコード、これらが常に、`null`値をその`DataItem`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="9941b-273">Since the `EmptyDataRow`, `Header`, `Footer`, and `Pager` rows aren't associated with a `DataSource` record, they will always have a `null` value for their `DataItem` property.</span></span> <span data-ttu-id="9941b-274">このため、現在の作業を試みる前に`GridViewRow`の`DataItem`プロパティ、お最初を確認してくださいを扱っている、`DataRow`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-274">For this reason, before attempting to work with the current `GridViewRow`'s `DataItem` property, we first must make sure that we're dealing with a `DataRow`.</span></span> <span data-ttu-id="9941b-275">これには、チェックして、`GridViewRow`の`RowType`プロパティ次のようにします。</span><span class="sxs-lookup"><span data-stu-id="9941b-275">This can be accomplished by checking the `GridViewRow`'s `RowType` property like so:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a><span data-ttu-id="9941b-276">手順 9: が $10.00 未満には、行黄色と、UnitPrice の値を強調表示</span><span class="sxs-lookup"><span data-stu-id="9941b-276">Step 9: Highlighting the Row Yellow When the UnitPrice Value is Less than $10.00</span></span>

<span data-ttu-id="9941b-277">最後の手順ではプログラム全体を強調表示を`GridViewRow`場合、`UnitPrice`行が $10.00 未満の値します。</span><span class="sxs-lookup"><span data-stu-id="9941b-277">The last step is to programmatically highlight the entire `GridViewRow` if the `UnitPrice` value for that row is less than $10.00.</span></span> <span data-ttu-id="9941b-278">GridView の行またはセルへのアクセスの構文が DetailsView の場合と同じ`GridViewID.Rows[index]`全体の行にアクセスする`GridViewID.Rows[index].Cells[index]`特定のセルにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="9941b-278">The syntax for accessing a GridView's rows or cells is the same as with the DetailsView `GridViewID.Rows[index]` to access the entire row, `GridViewID.Rows[index].Cells[index]` to access a particular cell.</span></span> <span data-ttu-id="9941b-279">ただし、ときに、`RowDataBound`イベント ハンドラーがデータ バインドを発生させる`GridViewRow`GridView に追加するのにはまだ`Rows`コレクション。</span><span class="sxs-lookup"><span data-stu-id="9941b-279">However, when the `RowDataBound` event handler fires the data bound `GridViewRow` has yet to be added to the GridView's `Rows` collection.</span></span> <span data-ttu-id="9941b-280">そのため、現在にアクセスできない`GridViewRow`インスタンスから、`RowDataBound`行のコレクションを使用してイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="9941b-280">Therefore you cannot access the current `GridViewRow` instance from the `RowDataBound` event handler using the Rows collection.</span></span>

<span data-ttu-id="9941b-281">代わりに`GridViewID.Rows[index]`、現在は参照できます`GridViewRow`インスタンス、`RowDataBound`イベント ハンドラーを使用して`e.Row`です。</span><span class="sxs-lookup"><span data-stu-id="9941b-281">Instead of `GridViewID.Rows[index]`, we can reference the current `GridViewRow` instance in the `RowDataBound` event handler using `e.Row`.</span></span> <span data-ttu-id="9941b-282">現在強調表示するには、`GridViewRow`インスタンスから、`RowDataBound`イベント ハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="9941b-282">That is, in order to highlight the current `GridViewRow` instance from the `RowDataBound` event handler we would use:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

<span data-ttu-id="9941b-283">設定ではなく、`GridViewRow`の`BackColor`プロパティを直接だけ作業 CSS クラスを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="9941b-283">Rather than set the `GridViewRow`'s `BackColor` property directly, let's stick with using CSS classes.</span></span> <span data-ttu-id="9941b-284">名前付き CSS クラスを作成した`AffordablePriceEmphasis`背景色を黄色に設定します。</span><span class="sxs-lookup"><span data-stu-id="9941b-284">I've created a CSS class named `AffordablePriceEmphasis` that sets the background color to yellow.</span></span> <span data-ttu-id="9941b-285">完成した`RowDataBound`イベント ハンドラーがフォローします。</span><span class="sxs-lookup"><span data-stu-id="9941b-285">The completed `RowDataBound` event handler follows:</span></span>


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


<span data-ttu-id="9941b-286">[![最も低コスト製品が黄色が強調表示されます。](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="9941b-286">[![The Most Affordable Products are Highlighted Yellow](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)</span></span>

<span data-ttu-id="9941b-287">**図 11**: 最も低コスト製品は黄色が強調表示されます ([フルサイズのイメージを表示するをクリックして](custom-formatting-based-upon-data-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="9941b-287">**Figure 11**: The Most Affordable Products are Highlighted Yellow ([Click to view full-size image](custom-formatting-based-upon-data-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="9941b-288">まとめ</span><span class="sxs-lookup"><span data-stu-id="9941b-288">Summary</span></span>

<span data-ttu-id="9941b-289">このチュートリアルでは、GridView、DetailsView、およびコントロールにバインドされたデータに基づくフォーム ビューの書式を設定する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="9941b-289">In this tutorial we saw how to format the GridView, DetailsView, and FormView based on the data bound to the control.</span></span> <span data-ttu-id="9941b-290">イベント ハンドラーを作成してこれを実現する、`DataBound`または`RowDataBound`イベント、必要な場合は書式設定の変更と共にに検査が基になるデータの場所。</span><span class="sxs-lookup"><span data-stu-id="9941b-290">To accomplish this we created an event handler for the `DataBound` or `RowDataBound` events, where the underlying data was examined along with a formatting change, if needed.</span></span> <span data-ttu-id="9941b-291">DetailsView または FormView にバインドされたデータにアクセスするを使用、`DataItem`プロパティに、`DataBound`イベント ハンドラー以外の場合は、GridView の各`GridViewRow`インスタンスの`DataItem`プロパティには、で使用可能な行にバインドされたデータが含まれています。`RowDataBound`イベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="9941b-291">To access the data bound to a DetailsView or FormView, we use the `DataItem` property in the `DataBound` event handler; for a GridView, each `GridViewRow` instance's `DataItem` property contains the data bound to that row, which is available in the `RowDataBound` event handler.</span></span>

<span data-ttu-id="9941b-292">プログラムによってデータ Web コントロールの書式設定を調整するための構文は、Web コントロールと書式設定するデータの表示方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="9941b-292">The syntax for programmatically adjusting the data Web control's formatting depends upon the Web control and how the data to be formatted is displayed.</span></span> <span data-ttu-id="9941b-293">DetailsView および GridView には、コントロール、行およびセルを序数に基づくインデックスによってアクセスできることができます。</span><span class="sxs-lookup"><span data-stu-id="9941b-293">For DetailsView and GridView controls, the rows and cells can be accessed by an ordinal index.</span></span> <span data-ttu-id="9941b-294">テンプレートを使用して、フォーム ビューの`FindControl("controlID")`メソッドは、通常、テンプレート内から Web コントロールの検索に使用します。</span><span class="sxs-lookup"><span data-stu-id="9941b-294">For the FormView, which uses templates, the `FindControl("controlID")` method is commonly used to locate a Web control from within the template.</span></span>

<span data-ttu-id="9941b-295">次のチュートリアルでは、GridView と DetailsView でテンプレートを使用する方法について見ていきます。</span><span class="sxs-lookup"><span data-stu-id="9941b-295">In the next tutorial we'll look at how to use templates with the GridView and DetailsView.</span></span> <span data-ttu-id="9941b-296">さらに、基になるデータに基づく書式をカスタマイズするためのもう 1 つの方法を会いしましょう。</span><span class="sxs-lookup"><span data-stu-id="9941b-296">Additionally, we'll see another technique for customizing the formatting based on the underlying data.</span></span>

<span data-ttu-id="9941b-297">満足プログラミング!</span><span class="sxs-lookup"><span data-stu-id="9941b-297">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="9941b-298">作成者について</span><span class="sxs-lookup"><span data-stu-id="9941b-298">About the Author</span></span>

<span data-ttu-id="9941b-299">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。</span><span class="sxs-lookup"><span data-stu-id="9941b-299">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="9941b-300">Scott は、コンサルタント、トレーナー、ライターとして機能します。</span><span class="sxs-lookup"><span data-stu-id="9941b-300">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="9941b-301">最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。</span><span class="sxs-lookup"><span data-stu-id="9941b-301">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="9941b-302">彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。</span><span class="sxs-lookup"><span data-stu-id="9941b-302">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="9941b-303">感謝の特別な</span><span class="sxs-lookup"><span data-stu-id="9941b-303">Special Thanks To</span></span>

<span data-ttu-id="9941b-304">このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。</span><span class="sxs-lookup"><span data-stu-id="9941b-304">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="9941b-305">このチュートリアルの潜在顧客レビュー担当者が E.R.</span><span class="sxs-lookup"><span data-stu-id="9941b-305">Lead reviewers for this tutorial were E.R.</span></span> <span data-ttu-id="9941b-306">Gilmore、Dennis Patterson と Dan Jagers です。</span><span class="sxs-lookup"><span data-stu-id="9941b-306">Gilmore, Dennis Patterson, and Dan Jagers.</span></span> <span data-ttu-id="9941b-307">今後、MSDN の記事を確認することに関心のあるですか。</span><span class="sxs-lookup"><span data-stu-id="9941b-307">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="9941b-308">場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="9941b-308">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9941b-309">次へ</span><span class="sxs-lookup"><span data-stu-id="9941b-309">Next</span></span>](using-templatefields-in-the-gridview-control-cs.md)
