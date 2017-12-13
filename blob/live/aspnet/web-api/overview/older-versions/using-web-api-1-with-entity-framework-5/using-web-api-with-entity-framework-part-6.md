---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "パート 6: 製品と順序コント ローラーの作成 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="9ac7b-102">パート 6: 製品の作成と順序コント ローラー</span><span class="sxs-lookup"><span data-stu-id="9ac7b-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="9ac7b-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9ac7b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9ac7b-104">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="9ac7b-105">製品のコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-105">Add a Products Controller</span></span>

<span data-ttu-id="9ac7b-106">管理コント ローラーは、管理者特権を持っているユーザーに対してです。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="9ac7b-107">お客様は、その一方で、できます製品の表示ことはできませんを作成、更新、またはそれらを削除します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="9ac7b-108">Get メソッドを開いたまま、Post、Put、および削除のメソッドへのアクセスを制限おできます簡単にします。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="9ac7b-109">しかし、製品の返されるデータを見てください。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="9ac7b-110">`ActualCost`プロパティを顧客に表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="9ac7b-111">ソリューションは、定義する、*データ転送オブジェクト*(DTO) が含まれている顧客に表示する必要のあるプロパティのサブセットにはです。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="9ac7b-112">LINQ を使用してプロジェクト`Product`にインスタンス`ProductDTO`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="9ac7b-113">という名前のクラスを追加`ProductDTO`Models フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="9ac7b-114">コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-114">Now add the controller.</span></span> <span data-ttu-id="9ac7b-115">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="9ac7b-116">選択**追加**選択してから、**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="9ac7b-117">**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー &quot;ProductsController&quot;です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="9ac7b-118">**テンプレート****空 API コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="9ac7b-119">次のコードでは、ソース ファイル内のすべてを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="9ac7b-120">コント ローラーが現在も使用して、`OrdersContext`データベースを照会します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="9ac7b-121">返すのではなく`Product`インスタンスを直接呼んで`MapProducts`上にそれらをプロジェクトに`ProductDTO`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="9ac7b-122">`MapProducts`メソッドを返します、 **IQueryable**ので、他のクエリ パラメーターと結果を作成します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="9ac7b-123">これで確認できます、`GetProduct`メソッドで、追加、**場所**クエリ句。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="9ac7b-124">注文のコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-124">Add an Orders Controller</span></span>

<span data-ttu-id="9ac7b-125">次に、ユーザーを作成し、注文を表示できるようにしているコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="9ac7b-126">別の DTO をまず確認します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-126">We'll start with another DTO.</span></span> <span data-ttu-id="9ac7b-127">ソリューション エクスプ ローラーで、Models フォルダーを右クリックし、という名前のクラスを追加`OrderDTO`次の実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="9ac7b-128">コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-128">Now add the controller.</span></span> <span data-ttu-id="9ac7b-129">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="9ac7b-130">選択**追加**選択してから、**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="9ac7b-131">**コント ローラーの追加**ダイアログ ボックスで、次のオプションを設定します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="9ac7b-132">**コント ローラー名**、"OrdersController"を入力します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="9ac7b-133">**テンプレート**「Entity Framework を使用して、読み取り/書き込みアクションがある API コント ローラー」です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="9ac7b-134">**モデル クラス**&quot;順序 (ProductStore.Models)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="9ac7b-135">**データ コンテキスト クラス** &quot;OrdersContext (ProductStore.Models)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="9ac7b-136">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-136">Click **Add**.</span></span> <span data-ttu-id="9ac7b-137">OrdersController.cs をという名前のファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="9ac7b-138">次に、コント ローラーの既定の実装を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="9ac7b-139">最初に、削除、`PutOrder`と`DeleteOrder`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="9ac7b-140">このサンプルでは、お客様は変更または既存の注文を削除できません。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="9ac7b-141">実際のアプリケーションでは、このような場合を処理するためのバックエンド ロジックの多くする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="9ac7b-142">(たとえばが、既に発送しますか?)</span><span class="sxs-lookup"><span data-stu-id="9ac7b-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="9ac7b-143">変更、`GetOrders`をユーザーに属する注文だけを返すメソッド。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="9ac7b-144">変更、`GetOrder`メソッドを次のようにします。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="9ac7b-145">メソッドに加え変更を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="9ac7b-146">戻り値は、 `OrderDTO` 、インスタンスの代わりに、`Order`です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="9ac7b-147">使用して、注文のデータベースのクエリを実行おとき、 [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395)関連をフェッチするメソッド`OrderDetail`と`Product`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="9ac7b-148">結果をフラット化私たちには、投影を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="9ac7b-149">HTTP 応答の数量が製品の配列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="9ac7b-150">この形式は、クライアントが元のオブジェクトのグラフは、入れ子になったエンティティ (注文、詳細、および製品) を含むより使用できるは簡単です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="9ac7b-151">これを考慮する最後のメソッド`PostOrder`です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="9ac7b-152">現在、このメソッドは、`Order`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="9ac7b-153">動作を検討してください。 クライアントは、次のように、要求本文を送信する場合。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="9ac7b-154">これは、適切に構成された順序であり、Entity Framework では、データベースに挿入問題なく。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="9ac7b-155">以前存在しなかった製品エンティティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="9ac7b-156">データベースに新しい製品を作成したクライアントです。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-156">The client just created a new product in our database!</span></span> <span data-ttu-id="9ac7b-157">クマの注文を確認したときに、注文 fullfilment 部門に驚くことになります。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="9ac7b-158">教訓、本当に POST または PUT 要求でそのまま使用するデータに関する注意してください。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="9ac7b-159">この問題を避けるためには、変更、`PostOrder`メソッドを`OrderDTO`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="9ac7b-160">使用して、`OrderDTO`を作成する、`Order`です。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="9ac7b-161">使用に注意してください、`ProductID`と`Quantity`プロパティ、およびおは、製品名または価格のいずれかのクライアントが送信したすべての値を無視します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="9ac7b-162">製品 ID が有効でない場合、データベースの外部キー制約に違反することし、は、挿入は失敗します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="9ac7b-163">ここでは、完全な`PostOrder`メソッド。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="9ac7b-164">最後に、追加、 **Authorize**コント ローラーに属性します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="9ac7b-165">これで登録されたユーザーのみを作成したり注文を表示します。</span><span class="sxs-lookup"><span data-stu-id="9ac7b-165">Now only registered users can create or view orders.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9ac7b-166">[前へ](using-web-api-with-entity-framework-part-5.md)
[次へ](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="9ac7b-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
