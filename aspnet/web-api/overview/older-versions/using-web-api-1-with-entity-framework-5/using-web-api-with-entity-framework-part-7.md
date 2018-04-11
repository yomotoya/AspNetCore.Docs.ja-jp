---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: '手順 7: 作成、メイン ページ |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="34a3e-102">手順 7: 作成、メイン ページ</span><span class="sxs-lookup"><span data-stu-id="34a3e-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="34a3e-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="34a3e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="34a3e-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="34a3e-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="34a3e-105">メインの作成] ページ</span><span class="sxs-lookup"><span data-stu-id="34a3e-105">Creating the Main Page</span></span>

<span data-ttu-id="34a3e-106">このセクションでは、アプリケーションのメイン ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="34a3e-107">このページは [管理] ページより複雑になるため、いくつかの手順でアプローチしましょう。</span><span class="sxs-lookup"><span data-stu-id="34a3e-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="34a3e-108">その過程より高度な Knockout.js テクニックをいくつかが表示されます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="34a3e-109">ページの基本的なレイアウトを次に示します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="34a3e-110">"Products"では、製品の配列を保持します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="34a3e-111">"Cart"では、製品と数量の配列を保持します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="34a3e-112">"Add to Cart"をクリックすると、カートを更新します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="34a3e-113">"Orders"では、注文 Id の配列を保持します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="34a3e-114">"Details"は、項目 (数量が製品) の配列は、注文の詳細を保持しています。</span><span class="sxs-lookup"><span data-stu-id="34a3e-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="34a3e-115">データ バインディングやスクリプトなしで、HTML では、いくつかの基本的なレイアウトを定義することで始めます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="34a3e-116">Views/Home/Index.cshtml ファイルを開き、次のすべての内容に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="34a3e-117">次に、スクリプトのセクションを追加し、空のビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="34a3e-118">以前スケッチした設計に基づき、当社ビュー モデル必要があります観測可能なオブジェクトの製品、カート、orders、および詳細。</span><span class="sxs-lookup"><span data-stu-id="34a3e-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="34a3e-119">次の変数を追加して、`AppViewModel`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="34a3e-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="34a3e-120">ユーザーでは、製品のリストから、カートにアイテムを追加でき、カートから項目を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="34a3e-121">これらの関数をカプセル化するには、製品を表す別のビュー モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="34a3e-122">`AppViewModel` に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="34a3e-123">`ProductViewModel`クラスにして、カートに製品を移動するために使用する 2 つの関数が含まれています: `addItemToCart` 、カートに製品の 1 つの単位を追加し、`removeAllFromCart`製品のすべての数量を削除します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="34a3e-124">ユーザーは、既存の注文を選択し、注文の詳細を取得します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="34a3e-125">私たちは、別のビュー モデルには、この機能をカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="34a3e-126">`OrderDetailsViewModel`は順序が初期化されて、サーバーに AJAX 要求を送信することによって、注文の詳細をフェッチとします。</span><span class="sxs-lookup"><span data-stu-id="34a3e-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="34a3e-127">また、`total`プロパティを`OrderDetailsViewModel`です。</span><span class="sxs-lookup"><span data-stu-id="34a3e-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="34a3e-128">このプロパティは、監視対象と呼ばれる特殊な[観測可能なオブジェクトの計算](http://knockoutjs.com/documentation/computedObservables.html)です。</span><span class="sxs-lookup"><span data-stu-id="34a3e-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="34a3e-129">名前が示すように、計算の監視対象することができます、計算された値へのデータ バインド&#8212;ここでは、注文のコストの合計。</span><span class="sxs-lookup"><span data-stu-id="34a3e-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="34a3e-130">これらの関数を次に、追加`AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="34a3e-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="34a3e-131">`resetCart` カートからすべての項目を削除します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="34a3e-132">`getDetails` 注文の詳細を取得 (によって、新しい pusing`OrderDetailsViewModel`上に、`details`リスト)。</span><span class="sxs-lookup"><span data-stu-id="34a3e-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="34a3e-133">`createOrder` 新しい注文を作成し、カートを空にします。</span><span class="sxs-lookup"><span data-stu-id="34a3e-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="34a3e-134">最後に、製品と注文の AJAX 要求をすることで、ビュー モデルを初期化します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="34a3e-135">[Ok]、多くのコードですが、作成したを詳細な手順を願っていますので設計はクリアされます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="34a3e-136">HTML に一部の Knockout.js バインドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="34a3e-137">**製品**</span><span class="sxs-lookup"><span data-stu-id="34a3e-137">**Products**</span></span>

<span data-ttu-id="34a3e-138">製品の一覧については、バインディングを次に示します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="34a3e-139">これにより、製品の配列を反復処理し、名前と価格を表示します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="34a3e-140">"Order に追加"ボタンは、ユーザーがログインした場合にのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="34a3e-141">"Order に追加"ボタン呼び出し`addItemToCart`上、`ProductViewModel`製品のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="34a3e-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="34a3e-142">Knockout.js の優れた機能を示しますこの: ビュー モデルには、その他のビュー モデルが含まれている、内部のモデルにバインドを適用できます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="34a3e-143">この例では、内のバインディングでは、`foreach`のごとに適用されます、`ProductViewModel`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="34a3e-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="34a3e-144">この方法は、1 つのビュー モデルにすべての機能を配置することよりもかなりクリーナーです。</span><span class="sxs-lookup"><span data-stu-id="34a3e-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="34a3e-145">**カート**</span><span class="sxs-lookup"><span data-stu-id="34a3e-145">**Cart**</span></span>

<span data-ttu-id="34a3e-146">次に、カートのバインドを示します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="34a3e-147">これにより、買い物カゴの配列を反復処理し、名前、価格、および数量を表示します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="34a3e-148">削除」のリンクと、"Create Order"ボタンがビュー モデル関数にバインドされていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="34a3e-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="34a3e-149">**注文**</span><span class="sxs-lookup"><span data-stu-id="34a3e-149">**Orders**</span></span>

<span data-ttu-id="34a3e-150">注文の一覧については、バインディングを次に示します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="34a3e-151">これは、注文を反復処理し、注文 ID を示しています。</span><span class="sxs-lookup"><span data-stu-id="34a3e-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="34a3e-152">リンクをクリックしてイベントがバインドされている、`getDetails`関数。</span><span class="sxs-lookup"><span data-stu-id="34a3e-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="34a3e-153">**注文の詳細**</span><span class="sxs-lookup"><span data-stu-id="34a3e-153">**Order Details**</span></span>

<span data-ttu-id="34a3e-154">次に、注文の詳細のバインドを示します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="34a3e-155">これは、注文の品目を反復処理し、製品、価格、数量が表示されます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="34a3e-156">周囲の div は、詳細配列には、1 つまたは複数の項目が含まれている場合にのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="34a3e-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="34a3e-157">まとめ</span><span class="sxs-lookup"><span data-stu-id="34a3e-157">Conclusion</span></span>

<span data-ttu-id="34a3e-158">このチュートリアルでは、データベース、およびデータ層の上に公開されたインターフェイスを提供する ASP.NET Web API と通信するために Entity Framework を使用するアプリケーションを作成しました。</span><span class="sxs-lookup"><span data-stu-id="34a3e-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="34a3e-159">ページの再読み込みせずの動的な対話を提供するのに HTML ページ、および Knockout.js と jQuery を表示するために ASP.NET MVC 4 を使用します。</span><span class="sxs-lookup"><span data-stu-id="34a3e-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="34a3e-160">その他のリソース:</span><span class="sxs-lookup"><span data-stu-id="34a3e-160">Additional resources:</span></span>

- [<span data-ttu-id="34a3e-161">ASP.NET データ アクセス コンテンツ マップ</span><span class="sxs-lookup"><span data-stu-id="34a3e-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="34a3e-162">Entity Framework デベロッパー センター</span><span class="sxs-lookup"><span data-stu-id="34a3e-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="34a3e-163">前へ</span><span class="sxs-lookup"><span data-stu-id="34a3e-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
