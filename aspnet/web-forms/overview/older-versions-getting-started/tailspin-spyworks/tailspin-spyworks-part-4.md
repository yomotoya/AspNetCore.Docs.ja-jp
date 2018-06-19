---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'パート 4: 製品のリスト |Microsoft ドキュメント'
author: JoeStagner
description: このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 4 カバーが GridView contr. で製品を一覧表示する.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881065"
---
<a name="part-4-listing-products"></a><span data-ttu-id="8658a-104">パート 4: 一覧の製品</span><span class="sxs-lookup"><span data-stu-id="8658a-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="8658a-105">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="8658a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="8658a-106">Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。</span><span class="sxs-lookup"><span data-stu-id="8658a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="8658a-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8658a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="8658a-108">このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="8658a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="8658a-109">パート 4 では、GridView コントロールの一覧の製品について説明します。</span><span class="sxs-lookup"><span data-stu-id="8658a-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="8658a-110">GridView コントロールでの製品を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="8658a-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="8658a-111">"Add"および「新しい項目の」をクリックし、ソリューションでは、「を右クリック」、ProductsList.aspx ページの実装を開始してみましょう。</span><span class="sxs-lookup"><span data-stu-id="8658a-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="8658a-112">「Web フォームを使用してマスター ページ」を選択し、ProductsList.aspx のページ名を入力してください"です。</span><span class="sxs-lookup"><span data-stu-id="8658a-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="8658a-113">[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8658a-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="8658a-114">次 Site.Master ページを配置お「スタイル」フォルダーを選択し、「フォルダーの内容」ウィンドウから選択します。</span><span class="sxs-lookup"><span data-stu-id="8658a-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="8658a-115">"Ok"ページを作成する をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8658a-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="8658a-116">データベースには、次のように製品のデータが格納されます。</span><span class="sxs-lookup"><span data-stu-id="8658a-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="8658a-117">ページを作成した後エンティティ データ ソースを使用して、その製品データにアクセスをもう一度おがこのインスタンスで製品のエンティティを選択する必要があり、選択したカテゴリのものだけに返される項目を制限する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8658a-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="8658a-118">これを実現するには、WHERE 句を自動生成する EntityDataSource を説明し、WhereParameter を指定します。</span><span class="sxs-lookup"><span data-stu-id="8658a-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="8658a-119">この「製品カテゴリ メニュー」で、メニュー項目を作成したときに動的に構築リンク リンクごとに、クエリ文字列を CatagoryID を追加することで説明します。</span><span class="sxs-lookup"><span data-stu-id="8658a-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="8658a-120">クエリ文字列パラメーターから WHERE パラメーターを派生するエンティティのデータ ソースお知らせします。</span><span class="sxs-lookup"><span data-stu-id="8658a-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="8658a-121">次に、製品の一覧を表示するリスト ビュー コントロールを構成します。</span><span class="sxs-lookup"><span data-stu-id="8658a-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="8658a-122">ショッピング最適なエクスペリエンスを作成するには、ListVew に表示される個々 の製品にいくつかの簡潔な機能を縮小します。</span><span class="sxs-lookup"><span data-stu-id="8658a-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="8658a-123">製品名、製品の詳細ビューへのリンクになります。</span><span class="sxs-lookup"><span data-stu-id="8658a-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="8658a-124">製品の価格が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8658a-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="8658a-125">製品の画像が表示され、アプリケーションでは、カタログ イメージ ディレクトリからイメージを選択おを動的にします。</span><span class="sxs-lookup"><span data-stu-id="8658a-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="8658a-126">すぐに特定の製品をショッピング カートに追加へのリンクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8658a-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="8658a-127">ListView コントロール インスタンスのマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="8658a-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="8658a-128">表示されている各製品のいくつかのリンクを動的に構築おされます。</span><span class="sxs-lookup"><span data-stu-id="8658a-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="8658a-129">また、独自の新しいページをテストする前に、ディレクトリ構造、製品のカタログのイメージを次のように作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8658a-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="8658a-130">当社製品の画像がアクセス可能な製品リスト ページをテストします。</span><span class="sxs-lookup"><span data-stu-id="8658a-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="8658a-131">サイトのホーム ページでは、カテゴリの一覧のリンクのいずれかをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8658a-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="8658a-132">ProductDetials.apsx ページと AddToCart 機能を実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8658a-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="8658a-133">ファイルを使用して&gt;を以前と同様に、サイトのマスター ページを使用して ProductDetails.aspx ページ名を作成します。</span><span class="sxs-lookup"><span data-stu-id="8658a-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="8658a-134">EntityDataSource コントロールを使用して、データベース内の特定の製品レコードにアクセスする、もう一度おと、次のように、製品データを表示する ASP.NET FormView コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="8658a-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="8658a-135">書式設定するビットおもしろい見える場合、心配しないでください。</span><span class="sxs-lookup"><span data-stu-id="8658a-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="8658a-136">上記のマークアップの余地が画面のレイアウトで、いくつかの機能を後で実装します。</span><span class="sxs-lookup"><span data-stu-id="8658a-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="8658a-137">ショッピング カートは、アプリケーションではより複雑なロジックを表します。</span><span class="sxs-lookup"><span data-stu-id="8658a-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="8658a-138">、作業を開始するには、次を使用してファイル-&gt;MyShoppingCart.aspx をという名前のページを作成します。</span><span class="sxs-lookup"><span data-stu-id="8658a-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="8658a-139">後の名前を選択していないいるおことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8658a-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="8658a-140">データベースには、"ShoppingCart"という名前のテーブルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8658a-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="8658a-141">Entity Data Model を生成したとき、クラスは、データベース内の各テーブルに対して作成されました。</span><span class="sxs-lookup"><span data-stu-id="8658a-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="8658a-142">そのため、エンティティ データ モデルでは、"ShoppingCart"という名前のエンティティ クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="8658a-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="8658a-143">競合を回避する名前を選択するだけに代わりに登録されますが、ショッピング カート実装するためには、その名前を使用または、ニーズの拡張おでしたように、そのモデルを編集お可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8658a-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="8658a-144">簡単なショッピング カートを作成しているに留意とがショッピング カート表示に、ショッピング カート ロジックを埋め込み価値もです。</span><span class="sxs-lookup"><span data-stu-id="8658a-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="8658a-145">完全に独立したビジネス層に、ショッピング カートを実装する場合がありますも選択します。</span><span class="sxs-lookup"><span data-stu-id="8658a-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8658a-146">[前へ](tailspin-spyworks-part-3.md)
> [次へ](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="8658a-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
