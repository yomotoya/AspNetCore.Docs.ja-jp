---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: マスター/詳細のフィルター処理 (c#) DropDownList を |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、DropDownLists を使用して、'master' のレコードと、DataList displ に表示する単一の web ページにマスター/詳細レポートを表示する方法を確認しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880675"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a><span data-ttu-id="9ada5-103">マスター/詳細 DropDownList (c#) によるフィルター処理</span><span class="sxs-lookup"><span data-stu-id="9ada5-103">Master/Detail Filtering With a DropDownList (C#)</span></span>
====================
<span data-ttu-id="9ada5-104">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="9ada5-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="9ada5-105">[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe)または[PDF のダウンロード](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="9ada5-105">[Download Sample App](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) or [Download PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)</span></span>

> <span data-ttu-id="9ada5-106">このチュートリアルでは、「マスター」のレコードと「詳細」を表示する DataList 表示 DropDownLists を使用して単一の web ページにマスター/詳細レポートを表示する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-106">In this tutorial we see how to display master/detail reports in a single web page using DropDownLists to display the "master" records and a DataList to display the "details".</span></span>


## <a name="introduction"></a><span data-ttu-id="9ada5-107">はじめに</span><span class="sxs-lookup"><span data-stu-id="9ada5-107">Introduction</span></span>

<span data-ttu-id="9ada5-108">GridView を使用する前に示したを最初に作成したマスター/詳細レポート[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアルでは、一部の「マスター」のレコードのセットを表示することによって開始します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-108">The master/detail report, which we first created using a GridView in the earlier [Master/Detail Filtering With a DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, begins by showing some set of "master" records.</span></span> <span data-ttu-id="9ada5-109">ユーザー、ドリルダウンできますマスターのレコードのいずれかにそれによってそのマスター レコードの"の詳細の表示"</span><span class="sxs-lookup"><span data-stu-id="9ada5-109">The user can then drill down into one of the master records, thereby viewing that master record's "details."</span></span> <span data-ttu-id="9ada5-110">マスター/詳細レポートは、一対多の関係を視覚化し、特に「幅」のテーブル (多数の列があるもの) の詳細な情報を表示するための最適な選択肢です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-110">Master/detail reports are an ideal choice for visualizing one-to-many relationships and for displaying detailed information from particularly "wide" tables (ones that have a lot of columns).</span></span> <span data-ttu-id="9ada5-111">前のチュートリアルで、GridView と DetailsView コントロールを使用してマスター/詳細レポートを実装する方法おについて説明してきました。</span><span class="sxs-lookup"><span data-stu-id="9ada5-111">We've explored how to implement master/detail reports using the GridView and DetailsView controls in previous tutorials.</span></span> <span data-ttu-id="9ada5-112">このチュートリアルを次の 2 つの場合は、DataList でフォーカスが、これらの概念を再確認しますおし Repeater を代わりに制御します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-112">In this tutorial and the next two, we'll reexamine these concepts, but focus on using DataList and Repeater controls instead.</span></span>

<span data-ttu-id="9ada5-113">このチュートリアルでは、DataList に表示される [詳細] のレコードを含む、「マスター」のレコードを格納して DropDownList を使用してに紹介します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-113">In this tutorial, we'll look at using a DropDownList to contain the "master" records, with the "details" records displayed in a DataList.</span></span>

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a><span data-ttu-id="9ada5-114">手順 1: マスター/詳細のチュートリアル Web ページの追加</span><span class="sxs-lookup"><span data-stu-id="9ada5-114">Step 1: Adding the Master/Detail Tutorial Web Pages</span></span>

<span data-ttu-id="9ada5-115">このチュートリアルを始める前にまずみましょうフォルダーおよびしなければならない、このチュートリアルを DataList およびリピータ コントロールを使用してマスター/詳細レポートを処理する次の 2 つの ASP.NET ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-115">Before we start this tutorial, let's first take a moment to add the folder and ASP.NET pages we'll need for this tutorial and the next two dealing with master/detail reports using the DataList and Repeater controls.</span></span> <span data-ttu-id="9ada5-116">という名前のプロジェクトに新しいフォルダーを作成して開始`DataListRepeaterFiltering`です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-116">Start by creating a new folder in the project named `DataListRepeaterFiltering`.</span></span> <span data-ttu-id="9ada5-117">次に、すべてのマスター ページを使用するように構成して、このフォルダーに次の 5 つの ASP.NET ページを追加`Site.master`:</span><span class="sxs-lookup"><span data-stu-id="9ada5-117">Next, add the following five ASP.NET pages to this folder, having all of them configured to use the master page `Site.master`:</span></span>

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![DataListRepeaterFiltering フォルダーを作成し、チュートリアルの ASP.NET ページを追加](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

<span data-ttu-id="9ada5-119">**図 1**: 作成、`DataListRepeaterFiltering`フォルダー チュートリアルの ASP.NET ページを追加</span><span class="sxs-lookup"><span data-stu-id="9ada5-119">**Figure 1**: Create a `DataListRepeaterFiltering` Folder and Add the Tutorial ASP.NET Pages</span></span>


<span data-ttu-id="9ada5-120">次に、開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン画面上のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-120">Next, open the `Default.aspx` page and drag the `SectionLevelTutorialListing.ascx` User Control from the `UserControls` folder onto the Design surface.</span></span> <span data-ttu-id="9ada5-121">作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、サイト マップを列挙し、箇条書きリストの現在のセクションのチュートリアルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9ada5-121">This User Control, which we created in the [Master Pages and Site Navigation](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumerates the site map and displays the tutorials from the current section in a bulleted list.</span></span>


<span data-ttu-id="9ada5-122">[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-122">[![Add the SectionLevelTutorialListing.ascx User Control to Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)</span></span>

<span data-ttu-id="9ada5-123">**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-123">**Figure 2**: Add the `SectionLevelTutorialListing.ascx` User Control to `Default.aspx` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))</span></span>


<span data-ttu-id="9ada5-124">箇条書き一覧表示するために作成する、マスター/詳細チュートリアルでは、必要がありますサイト マップに追加します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-124">In order to have the bulleted list display the master/detail tutorials we'll be creating, we need to add them to the site map.</span></span> <span data-ttu-id="9ada5-125">開く、`Web.sitemap`ファイルし、マークアップの後に「を表示するデータを DataList とリピータ」サイト マップ ノード、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-125">Open the `Web.sitemap` file and add the following markup after the "Displaying Data with the DataList and Repeater" site map node markup:</span></span>

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![マップを更新するサイトに新しい ASP.NET ページを含める](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

<span data-ttu-id="9ada5-127">**図 3**: 新規の ASP.NET ページを含むサイト マップを更新</span><span class="sxs-lookup"><span data-stu-id="9ada5-127">**Figure 3**: Update the Site Map to Include the New ASP.NET Pages</span></span>


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a><span data-ttu-id="9ada5-128">手順 2: DropDownList でカテゴリを表示します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-128">Step 2: Displaying the Categories in a DropDownList</span></span>

<span data-ttu-id="9ada5-129">マスター/詳細レポートは、選択されたリスト項目の製品が表示されると、DropDownList のカテゴリを一覧、DataList 内のページ上にします。</span><span class="sxs-lookup"><span data-stu-id="9ada5-129">Our master/detail report will list the categories in a DropDownList, with the selected list item's products displayed further down in the page in a DataList.</span></span> <span data-ttu-id="9ada5-130">Us, 事前最初のタスクは、DropDownList で表示されるカテゴリを次に、です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-130">The first task ahead of us, then, is to have the categories displayed in a DropDownList.</span></span> <span data-ttu-id="9ada5-131">開いて開始、 `FilterByDropDownList.aspx`  ページで、`DataListRepeaterFiltering`フォルダーと、ページのデザイナーには、ツールボックスからドラッグ DropDownList です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-131">Start by opening the `FilterByDropDownList.aspx` page in the `DataListRepeaterFiltering` folder and drag a DropDownList from the Toolbox onto the page's designer.</span></span> <span data-ttu-id="9ada5-132">次に、設定の DropDownList の`ID`プロパティを`Categories`です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-132">Next, set the DropDownList's `ID` property to `Categories`.</span></span> <span data-ttu-id="9ada5-133">DropDownList のスマート タグから データ ソースのリンクをクリックし、作成という名前の新しい ObjectDataSource`CategoriesDataSource`です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-133">Click on the Choose Data Source link from the DropDownList's smart tag and create a new ObjectDataSource named `CategoriesDataSource`.</span></span>


<span data-ttu-id="9ada5-134">[![CategoriesDataSource をという名前の新しい ObjectDataSource を追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-134">[![Add a New ObjectDataSource Named CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)</span></span>

<span data-ttu-id="9ada5-135">**図 4**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-135">**Figure 4**: Add a New ObjectDataSource Named `CategoriesDataSource` ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))</span></span>


<span data-ttu-id="9ada5-136">起動するように、新しい ObjectDataSource を構成、`CategoriesBLL`クラスの`GetCategories()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-136">Configure the new ObjectDataSource such that it invokes the `CategoriesBLL` class's `GetCategories()` method.</span></span> <span data-ttu-id="9ada5-137">DropDownList でどのようなデータ ソースのフィールドを表示するかと、これを指定する必要があります ObjectDataSource を構成した後は、各リスト項目の値として関連付けられている 1 つがあります。</span><span class="sxs-lookup"><span data-stu-id="9ada5-137">After configuring the ObjectDataSource we still need to specify what data source field should be displayed in the DropDownList and which one should be associated as the value for each list item.</span></span> <span data-ttu-id="9ada5-138">`CategoryName`ディスプレイとしてフィールドと`CategoryID`各リスト項目の値として。</span><span class="sxs-lookup"><span data-stu-id="9ada5-138">Have the `CategoryName` field as the display and `CategoryID` as the value for each list item.</span></span>


<span data-ttu-id="9ada5-139">[![値として使用 CategoryID と CategoryName フィールドの DropDownList の表示があります。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-139">[![Have the DropDownList Display the CategoryName Field and Use CategoryID as the Value](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)</span></span>

<span data-ttu-id="9ada5-140">**図 5**: DropDownList ディスプレイ、`CategoryName`フィールド`CategoryID`値として ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-140">**Figure 5**: Have the DropDownList Display the `CategoryName` Field and Use `CategoryID` as the Value ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))</span></span>


<span data-ttu-id="9ada5-141">レコードが挿入されます DropDownList コントロールがあるこの時点で、`Categories`テーブル (約 6 秒で行うすべて)。</span><span class="sxs-lookup"><span data-stu-id="9ada5-141">At this point we have a DropDownList control that's populated with the records from the `Categories` table (all accomplished in about six seconds).</span></span> <span data-ttu-id="9ada5-142">図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-142">Figure 6 shows our progress thus far when viewed through a browser.</span></span>


<span data-ttu-id="9ada5-143">[![ドロップダウン リストに現在のカテゴリを一覧表示します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-143">[![A Drop-Down Lists the Current Categories](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)</span></span>

<span data-ttu-id="9ada5-144">**図 6**: A ドロップダウン リスト、現在のカテゴリ ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-144">**Figure 6**: A Drop-Down Lists the Current Categories ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))</span></span>


## <a name="step-2-adding-the-products-datalist"></a><span data-ttu-id="9ada5-145">手順 2: 製品 DataList の追加</span><span class="sxs-lookup"><span data-stu-id="9ada5-145">Step 2: Adding the Products DataList</span></span>

<span data-ttu-id="9ada5-146">マスター/詳細レポートの最後の手順では、選択したカテゴリに関連付けられている製品の一覧です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-146">The last step in our master/detail report is to list the products associated with the selected category.</span></span> <span data-ttu-id="9ada5-147">これを実現する、DataList をページに追加し、という名前の新しい ObjectDataSource を作成する`ProductsByCategoryDataSource`です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-147">To accomplish this, add a DataList to the page and create a new ObjectDataSource named `ProductsByCategoryDataSource`.</span></span> <span data-ttu-id="9ada5-148">`ProductsByCategoryDataSource`コントロールからそのデータの取得、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-148">Have the `ProductsByCategoryDataSource` control retrieve its data from the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method.</span></span> <span data-ttu-id="9ada5-149">このマスター/詳細レポートが読み取り専用であるために、INSERT、UPDATE、および DELETE のタブのオプション (なし) を選択します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-149">Since this master/detail report is read-only, choose the (None) option in the INSERT, UPDATE, and DELETE tabs.</span></span>


<span data-ttu-id="9ada5-150">[![GetProductsByCategoryID(categoryID) 方法を選択します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-150">[![Select the GetProductsByCategoryID(categoryID) Method](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)</span></span>

<span data-ttu-id="9ada5-151">**図 7**: 選択、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-151">**Figure 7**: Select the `GetProductsByCategoryID(categoryID)` Method ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))</span></span>


<span data-ttu-id="9ada5-152">[次へ] をクリックすると、ObjectDataSource ウィザードの指示に従って us ソースの値の`GetProductsByCategoryID(categoryID)`メソッドの*`categoryID`* パラメーター。</span><span class="sxs-lookup"><span data-stu-id="9ada5-152">After clicking Next, the ObjectDataSource wizard prompts us for the source of the value for the `GetProductsByCategoryID(categoryID)` method's *`categoryID`* parameter.</span></span> <span data-ttu-id="9ada5-153">選択した値を使用する`categories`DropDownList 項目コントロールを処理するパラメーターのソースを設定する`Categories`です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-153">To use the value of the selected `categories` DropDownList item set the Parameter source to Control and the ControlID to `Categories`.</span></span>


<span data-ttu-id="9ada5-154">[![カテゴリの DropDownList の値に categoryID パラメーターを設定します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-154">[![Set the categoryID Parameter to the Value of the Categories DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)</span></span>

<span data-ttu-id="9ada5-155">**図 8**: 設定、 *`categoryID`* パラメーターの値を`Categories`DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-155">**Figure 8**: Set the *`categoryID`* Parameter to the Value of the `Categories` DropDownList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))</span></span>


<span data-ttu-id="9ada5-156">データ ソース構成ウィザードを完了すると、Visual Studio が自動的に生成、 `ItemTemplate` DataList の各データ フィールドの値と名前を表示するためです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-156">Upon completing the Configure Data Source wizard, Visual Studio will automatically generate an `ItemTemplate` for the DataList that displays the name and value of each data field.</span></span> <span data-ttu-id="9ada5-157">代わりに使用する DataList を強化してみましょう、`ItemTemplate`製品の名前、カテゴリ、供給業者、単位、およびと共に価格ごとの数だけを表示する、`SeparatorTemplate`挿入される、`<hr>`各項目の間の要素。</span><span class="sxs-lookup"><span data-stu-id="9ada5-157">Let's enhance the DataList to instead use an `ItemTemplate` that displays just the product's name, category, supplier, quantity per unit, and price along with a `SeparatorTemplate` that injects an `<hr>` element between each item.</span></span> <span data-ttu-id="9ada5-158">使用して、`ItemTemplate`の例から、 [DataList とリピータ コントロールのデータの表示](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md)が、チュートリアルでは、お気軽にどのようなテンプレートのマークアップ最も視覚に訴える検索を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-158">I'm going to use the `ItemTemplate` from an example in the [Displaying Data with the DataList and Repeater Controls](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) tutorial, but feel free to use whatever template markup you find most visually appealing.</span></span>

<span data-ttu-id="9ada5-159">DataList とその ObjectDataSource のマークアップがこれらの変更を行った後は、次のようにする必要がありますなります。</span><span class="sxs-lookup"><span data-stu-id="9ada5-159">After making these changes, your DataList and its ObjectDataSource's markup should look similar to the following:</span></span>

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

<span data-ttu-id="9ada5-160">すぐをブラウザーで作業の進行状況を確認します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-160">Take a moment to check out our progress in a browser.</span></span> <span data-ttu-id="9ada5-161">最初のページへのアクセス、ときに、選択したカテゴリ (飲み物) に属しているこれらの製品が表示されます (図 9) が DropDownList を変更すると、データが更新されません。</span><span class="sxs-lookup"><span data-stu-id="9ada5-161">When first visiting the page, those products belonging to the selected category (Beverages) are displayed (as shown in Figure 9), but changing the DropDownList doesn't update the data.</span></span> <span data-ttu-id="9ada5-162">これは、ため、更新する DataList ポストバックが発生したときです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-162">This is because a postback must occur for the DataList to update.</span></span> <span data-ttu-id="9ada5-163">か、これを実現する設定の DropDownList の`AutoPostBack`プロパティを`true`またはボタンの Web コントロールをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-163">To accomplish this we can either set the DropDownList's `AutoPostBack` property to `true` or add a Button Web control to the page.</span></span> <span data-ttu-id="9ada5-164">このチュートリアルでは、選択したの DropDownList を設定する`AutoPostBack`プロパティを`true`です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-164">For this tutorial, I've opted to set the DropDownList's `AutoPostBack` property to `true`.</span></span>

<span data-ttu-id="9ada5-165">図 9 と 10 は、マスター/詳細レポートの動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="9ada5-165">Figures 9 and 10 illustrate the master/detail report in action.</span></span>


<span data-ttu-id="9ada5-166">[![最初のページへのアクセス、飲み物の製品が表示されます。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-166">[![When First Visiting the Page, the Beverage Products are Displayed](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)</span></span>

<span data-ttu-id="9ada5-167">**図 9**: 最初のページへのアクセス、飲み物の製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-167">**Figure 9**: When First Visiting the Page, the Beverage Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))</span></span>


<span data-ttu-id="9ada5-168">[![DataList の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-168">[![Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)</span></span>

<span data-ttu-id="9ada5-169">**図 10**: DataList の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択する ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-169">**Figure 10**: Selecting a New Product (Produce) Automatically Causes a PostBack, Updating the DataList ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))</span></span>


## <a name="adding-a----choose-a-category----list-item"></a><span data-ttu-id="9ada5-170">「--カテゴリの選択-」リスト アイテムを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-170">Adding a "-- Choose a Category --" List Item</span></span>

<span data-ttu-id="9ada5-171">最初にアクセスすると、 `FilterByDropDownList.aspx` DropDownList の最初のリスト項目 (飲み物) が既定では、DataList で飲み物の製品が表示された選択したカテゴリのページです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-171">When first visiting the `FilterByDropDownList.aspx` page the categories DropDownList's first list item (Beverages) is selected by default, showing the beverage products in the DataList.</span></span> <span data-ttu-id="9ada5-172">*マスター/詳細のフィルター処理で、DropDownList* DropDownList が既定でオンにし、選択すると表示を「--カテゴリの選択-」オプションを追加しましたチュートリアル*すべて*のデータベース内の製品です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-172">In the *Master/Detail Filtering With a DropDownList* tutorial we added a "-- Choose a Category --" option to the DropDownList that was selected by default and, when selected, displayed *all* of the products in the database.</span></span> <span data-ttu-id="9ada5-173">このようなアプローチでした管理しやすい、GridView では、製品を一覧表示するときに各製品の行が画面領域の量が少ないをかかりました。</span><span class="sxs-lookup"><span data-stu-id="9ada5-173">Such an approach was manageable when listing the products in a GridView, as each product row took up a small amount of screen real estate.</span></span> <span data-ttu-id="9ada5-174">DataList でただし、各製品の情報を消費画面の容量より大きなチャンクです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-174">With the DataList, however, each product's information consumes a much larger chunk of the screen.</span></span> <span data-ttu-id="9ada5-175">まだみましょう「--カテゴリの選択-」オプションを追加して既定では、選択されていることがあるがすべての製品を表示することはなく選択すると、みましょう構成製品が表示されないように。</span><span class="sxs-lookup"><span data-stu-id="9ada5-175">Let's still add a "-- Choose a Category --" option and have it selected by default, but instead of having it show all products when selected, let's configure it so that it shows no products.</span></span>

<span data-ttu-id="9ada5-176">次のドロップダウン リストに新しいリスト アイテムを追加するに [プロパティ] ウィンドウに移動し、内の省略記号をクリックして、`Items`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-176">To add a new list item to the DropDownList, go to the Properties window and click on the ellipses in the `Items` property.</span></span> <span data-ttu-id="9ada5-177">新しい一覧項目を追加、 `Text` 「--カテゴリの選択-」、および`Value``0`です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-177">Add a new list item with the `Text` "-- Choose a Category --" and the `Value` `0`.</span></span>


![追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

<span data-ttu-id="9ada5-179">**図 11**:「--カテゴリの選択-」リスト アイテムを追加</span><span class="sxs-lookup"><span data-stu-id="9ada5-179">**Figure 11**: Add a "-- Choose a Category --" List Item</span></span>


<span data-ttu-id="9ada5-180">代わりに、次のドロップダウン リストに、次のマークアップを追加することでリスト項目を追加できます。</span><span class="sxs-lookup"><span data-stu-id="9ada5-180">Alternatively, you can add the list item by adding the following markup to the DropDownList:</span></span>

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

<span data-ttu-id="9ada5-181">さらの DropDownList の制御を設定する必要があります`AppendDataBoundItems`に`true`ために設定されている場合`false`(既定)、任意の手動で追加したリストを上書きしますカテゴリは、ObjectDataSource から DropDownList にバインドします。項目。</span><span class="sxs-lookup"><span data-stu-id="9ada5-181">Additionally, we need to set the DropDownList control's `AppendDataBoundItems` to `true` because if it's set to `false` (the default), when the categories are bound to the DropDownList from the ObjectDataSource they'll overwrite any manually-added list items.</span></span>


![AppendDataBoundItems プロパティを True に設定します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

<span data-ttu-id="9ada5-183">**図 12**: 設定、`AppendDataBoundItems`プロパティを True に</span><span class="sxs-lookup"><span data-stu-id="9ada5-183">**Figure 12**: Set the `AppendDataBoundItems` Property to True</span></span>


<span data-ttu-id="9ada5-184">値を選択した理由`0`「--カテゴリの選択-」リスト アイテムは、値は、システム内のカテゴリが存在しないため`0`、したがって製品レコードは返されません、"--カテゴリ--"リスト アイテムの選択 を選択するとします。</span><span class="sxs-lookup"><span data-stu-id="9ada5-184">The reason we chose the value `0` for the "-- Choose a Category --" list item is because there are no categories in the system with a value of `0`, hence no product records will be returned when the "-- Choose a Category --" list item is selected.</span></span> <span data-ttu-id="9ada5-185">これには、すぐをブラウザーでページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9ada5-185">To confirm this, take a moment to visit the page through a browser.</span></span> <span data-ttu-id="9ada5-186">図 13 に示す最初に、"--カテゴリ--"リスト アイテムの選択 が選択されているページを表示すると、製品は表示されません。</span><span class="sxs-lookup"><span data-stu-id="9ada5-186">As Figure 13 shows, when initially viewing the page the "-- Choose a Category --" list item is selected and no products are displayed.</span></span>


<span data-ttu-id="9ada5-187">[![ときに、](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="9ada5-187">[![When the](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)</span></span>

<span data-ttu-id="9ada5-188">**図 13**: いいえ製品が表示される、"--カテゴリ--"リスト アイテムの選択 を選択すると、([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))</span><span class="sxs-lookup"><span data-stu-id="9ada5-188">**Figure 13**: When the "-- Choose a Category --" List Item is Selected, No Products are Displayed ([Click to view full-size image](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))</span></span>


<span data-ttu-id="9ada5-189">表示ではなく場合*すべて*の製品の「--カテゴリの選択-」オプションを選択すると、値を使用して、`-1`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="9ada5-189">If you'd rather display *all* of the products when the "-- Choose a Category --" option is selected, use a value of `-1` instead.</span></span> <span data-ttu-id="9ada5-190">鋭いリーダーがそのチェックインを思い出してください、*マスター/詳細のフィルター処理で、DropDownList*に更新されたチュートリアル、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドように場合、 *`categoryID`* 値`-1`が渡され、すべての製品レコードが返されました。</span><span class="sxs-lookup"><span data-stu-id="9ada5-190">The astute reader will recall that back in the *Master/Detail Filtering With a DropDownList* tutorial we updated the `ProductsBLL` class's `GetProductsByCategoryID(categoryID)` method so that if a *`categoryID`* value of `-1` was passed in, all product records were returned.</span></span>

## <a name="summary"></a><span data-ttu-id="9ada5-191">まとめ</span><span class="sxs-lookup"><span data-stu-id="9ada5-191">Summary</span></span>

<span data-ttu-id="9ada5-192">階層的に関連するデータを表示するには、マスター/詳細レポート、元のユーザーは、階層の最上位からデータを参照するための開始し、詳細にドリル ダウンを使用してデータを表示する多くの場合、役立ちます。</span><span class="sxs-lookup"><span data-stu-id="9ada5-192">When displaying hierarchically-related data, it often helps to present the data using master/detail reports, from which the user can start perusing the data from the top of the hierarchy and drill down into details.</span></span> <span data-ttu-id="9ada5-193">このチュートリアルでは、選択したカテゴリの製品が表示された単純なマスター/詳細レポートを作成して調査します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-193">In this tutorial we examined building a simple master/detail report showing a selected category's products.</span></span> <span data-ttu-id="9ada5-194">これは、カテゴリと、選択したカテゴリに属する製品用 DataList の一覧については、DropDownList を使用して行われました。</span><span class="sxs-lookup"><span data-stu-id="9ada5-194">This was accomplished by using a DropDownList for the list of categories and a DataList for the products belonging to the selected category.</span></span>

<span data-ttu-id="9ada5-195">次のチュートリアルでは、次の 2 つのページにわたってマスターと詳細レコードを分離することで紹介します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-195">In the next tutorial we'll look at separating the master and details records across two pages.</span></span> <span data-ttu-id="9ada5-196">最初のページでは、「マスター」のレコードの一覧が表示されます、詳細を表示するリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-196">In the first page, a list of "master" records will be displayed, with a link to view the details.</span></span> <span data-ttu-id="9ada5-197">リンクをクリックすると、2 番目のページは、選択したマスター レコードの詳細を表示するユーザーは whisk します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-197">Clicking on the link will whisk the user to the second page, which will display the details for the selected master record.</span></span>

<span data-ttu-id="9ada5-198">満足プログラミング!</span><span class="sxs-lookup"><span data-stu-id="9ada5-198">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="9ada5-199">作成者について</span><span class="sxs-lookup"><span data-stu-id="9ada5-199">About the Author</span></span>

<span data-ttu-id="9ada5-200">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。</span><span class="sxs-lookup"><span data-stu-id="9ada5-200">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="9ada5-201">Scott は、コンサルタント、トレーナー、ライターとして機能します。</span><span class="sxs-lookup"><span data-stu-id="9ada5-201">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="9ada5-202">最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-202">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="9ada5-203">彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。</span><span class="sxs-lookup"><span data-stu-id="9ada5-203">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="9ada5-204">特別に感謝しています.</span><span class="sxs-lookup"><span data-stu-id="9ada5-204">Special Thanks To…</span></span>

<span data-ttu-id="9ada5-205">このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。</span><span class="sxs-lookup"><span data-stu-id="9ada5-205">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="9ada5-206">このチュートリアルのレビュー担当者の潜在顧客がものです。 Schmidt しました。</span><span class="sxs-lookup"><span data-stu-id="9ada5-206">Lead reviewer for this tutorial was Randy Schmidt.</span></span> <span data-ttu-id="9ada5-207">今後、MSDN の記事を確認することに関心のあるですか。</span><span class="sxs-lookup"><span data-stu-id="9ada5-207">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="9ada5-208">場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="9ada5-208">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9ada5-209">次へ</span><span class="sxs-lookup"><span data-stu-id="9ada5-209">Next</span></span>](master-detail-filtering-acess-two-pages-datalist-cs.md)
