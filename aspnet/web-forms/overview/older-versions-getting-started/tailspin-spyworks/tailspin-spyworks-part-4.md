---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'パート 4: 製品を一覧表示 |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、GridView contr. で製品のリスティングについて説明します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 929d88d747c25ca7c4f6f991421cb3aa9456aa45
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368810"
---
<a name="part-4-listing-products"></a><span data-ttu-id="f4615-104">パート 4: 製品のリスティング</span><span class="sxs-lookup"><span data-stu-id="f4615-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="f4615-105">によって[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="f4615-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="f4615-106">Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。</span><span class="sxs-lookup"><span data-stu-id="f4615-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="f4615-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f4615-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="f4615-108">このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="f4615-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="f4615-109">パート 4 では、GridView コントロールと製品のリスティングについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f4615-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="f4615-110">GridView コントロールを備えた製品を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="f4615-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="f4615-111">"Add"および"新しい項目 を選択して、ソリューションでは、「右クリック」ProductsList.aspx、ページの実装を開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="f4615-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="f4615-112">「Web フォームを使用してマスター ページ」を選択し、ProductsList.aspx のページ名を入力してください"です。</span><span class="sxs-lookup"><span data-stu-id="f4615-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="f4615-113">[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f4615-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="f4615-114">次に Site.Master ページを配置しました [スタイル] フォルダーを選択し、「フォルダーの内容」ウィンドウから選択します。</span><span class="sxs-lookup"><span data-stu-id="f4615-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="f4615-115">ページを作成するには [Ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f4615-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="f4615-116">次に示すように、データベースには製品データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f4615-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="f4615-117">ページを作成した後、製品データにアクセスするエンティティのデータ ソースをもう一度使用しますが、このインスタンスで製品エンティティを選択してだけ選択したカテゴリに返される項目を制限する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4615-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="f4615-118">これを実現するには、WHERE 句の自動生成 EntityDataSource を説明し、WhereParameter を指定します。</span><span class="sxs-lookup"><span data-stu-id="f4615-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="f4615-119">[製品カテゴリ メニュー] で、メニュー項目を作成したときに動的に構築、リンク、CatagoryID を各リンクのクエリ文字列に追加することで。 思い出してください。</span><span class="sxs-lookup"><span data-stu-id="f4615-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="f4615-120">場所のパラメーターをクエリ文字列パラメーターから派生するエンティティのデータ ソースをお知らせします。</span><span class="sxs-lookup"><span data-stu-id="f4615-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="f4615-121">次に、製品の一覧を表示する ListView コントロールを構成します。</span><span class="sxs-lookup"><span data-stu-id="f4615-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="f4615-122">最適なショッピング エクスペリエンスを作成するには、ListVew に表示される各製品にいくつかの簡潔な機能を縮小します。</span><span class="sxs-lookup"><span data-stu-id="f4615-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="f4615-123">製品名は、製品の詳細ビューへのリンクになります。</span><span class="sxs-lookup"><span data-stu-id="f4615-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="f4615-124">製品の価格が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f4615-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="f4615-125">製品の画像が表示され、アプリケーションでは、カタログ イメージのディレクトリからイメージを選択しましたが動的にします。</span><span class="sxs-lookup"><span data-stu-id="f4615-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="f4615-126">すぐに特定の製品をショッピング カートに追加するリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f4615-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="f4615-127">ListView コントロール インスタンスのマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f4615-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="f4615-128">動的には表示されている各製品のいくつかのリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="f4615-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="f4615-129">また、独自の新しいページをテストする前に、製品のディレクトリ構造にカタログのイメージを次のように作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4615-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="f4615-130">マイクロソフト製品の画像がアクセス可能な製品リスト ページをテストできます。</span><span class="sxs-lookup"><span data-stu-id="f4615-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="f4615-131">サイトのホーム ページで、カテゴリの一覧のリンクのいずれかをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f4615-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="f4615-132">ProductDetials.apsx ページと AddToCart 機能を実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f4615-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="f4615-133">ファイルを使用して、&gt;ProductDetails.aspx 以前に行ったように、サイトのマスター ページを使用してページ名を作成します。</span><span class="sxs-lookup"><span data-stu-id="f4615-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="f4615-134">EntityDataSource コントロールを使用して、データベース内の特定の製品レコードへのアクセスにはもう一度おと ASP.NET FormView コントロールを使用して次のように、製品データを表示します。</span><span class="sxs-lookup"><span data-stu-id="f4615-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="f4615-135">少しおかしいは書式設定をする場合、心配しないでください。</span><span class="sxs-lookup"><span data-stu-id="f4615-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="f4615-136">上記のマークアップの余地が画面のレイアウトで、2 つの機能を後で実装します。</span><span class="sxs-lookup"><span data-stu-id="f4615-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="f4615-137">ショッピング カートは、アプリケーションではより複雑なロジックを表します。</span><span class="sxs-lookup"><span data-stu-id="f4615-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="f4615-138">最初に、使用してファイルの&gt;MyShoppingCart.aspx と呼ばれるページを作成します。</span><span class="sxs-lookup"><span data-stu-id="f4615-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="f4615-139">名の後選択しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f4615-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="f4615-140">データベースには、"ShoppingCart"という名前のテーブルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f4615-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="f4615-141">Entity Data Model を生成したときに、クラスは、データベース内の各テーブルに対して作成されました。</span><span class="sxs-lookup"><span data-stu-id="f4615-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="f4615-142">そのため、エンティティ データ モデルでは、"ShoppingCart"という名前のエンティティ クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f4615-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="f4615-143">行わないことに代わりに単純に現在名前の競合を回避するには、ショッピング カートの実装には、その名前を使用したり、ニーズに応じて、拡張されるように、モデルを編集しましたでした。</span><span class="sxs-lookup"><span data-stu-id="f4615-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="f4615-144">簡単なショッピング カートを作成したことに注意してください。 および、ショッピング カートの表示でショッピング カート ロジックの埋め込み価値もです。</span><span class="sxs-lookup"><span data-stu-id="f4615-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="f4615-145">また、ショッピング カートを完全に独立したビジネス層で実装に選択する場合があります。</span><span class="sxs-lookup"><span data-stu-id="f4615-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f4615-146">[前へ](tailspin-spyworks-part-3.md)
> [次へ](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="f4615-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
