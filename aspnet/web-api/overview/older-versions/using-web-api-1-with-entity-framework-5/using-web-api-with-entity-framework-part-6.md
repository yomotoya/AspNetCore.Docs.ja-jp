---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'パート 6: 製品と Order コント ローラーを作成する |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831314"
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="ebba2-102">パート 6: 製品の作成と Order コント ローラー</span><span class="sxs-lookup"><span data-stu-id="ebba2-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="ebba2-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ebba2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ebba2-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="ebba2-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="ebba2-105">製品のコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-105">Add a Products Controller</span></span>

<span data-ttu-id="ebba2-106">管理者コント ローラーは管理者特権を持つユーザーです。</span><span class="sxs-lookup"><span data-stu-id="ebba2-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="ebba2-107">お客様は、その一方で、ことができます製品の表示ことはできませんを作成、更新、またはそれらを削除します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="ebba2-108">Get メソッドを開いたまま、Post、Put、および Delete メソッドを簡単にアクセスが制限できます。</span><span class="sxs-lookup"><span data-stu-id="ebba2-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="ebba2-109">製品に対して返されるデータになります。</span><span class="sxs-lookup"><span data-stu-id="ebba2-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="ebba2-110">`ActualCost`プロパティを顧客に表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="ebba2-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="ebba2-111">ソリューションは、定義する、*データ転送オブジェクト*(DTO) を含む顧客に表示されるプロパティのサブセットです。</span><span class="sxs-lookup"><span data-stu-id="ebba2-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="ebba2-112">LINQ を使用してプロジェクト`Product`インスタンス`ProductDTO`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="ebba2-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="ebba2-113">という名前のクラスを追加`ProductDTO`Models フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="ebba2-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="ebba2-114">コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-114">Now add the controller.</span></span> <span data-ttu-id="ebba2-115">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="ebba2-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="ebba2-116">選択**追加**を選択し、**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="ebba2-117">**コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー &quot;ProductsController&quot;します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ebba2-118">**テンプレート**、**空の API コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="ebba2-119">次のコードでは、ソース ファイル内のすべてを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ebba2-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="ebba2-120">コント ローラーを引き続き使用、`OrdersContext`データベース クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="ebba2-121">返すのではなく`Product`インスタンスを直接呼び出して`MapProducts`をプロジェクトに`ProductDTO`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="ebba2-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="ebba2-122">`MapProducts`メソッドが返す、 **IQueryable**ので、他のクエリ パラメーターと結果を作成します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="ebba2-123">これで確認できます、`GetProduct`メソッドで、追加、**場所**句をクエリ。</span><span class="sxs-lookup"><span data-stu-id="ebba2-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="ebba2-124">注文のコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-124">Add an Orders Controller</span></span>

<span data-ttu-id="ebba2-125">次に、ユーザーの作成し、注文を表示できるようにするコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="ebba2-126">別の DTO を始めます。</span><span class="sxs-lookup"><span data-stu-id="ebba2-126">We'll start with another DTO.</span></span> <span data-ttu-id="ebba2-127">ソリューション エクスプ ローラーで、Models フォルダーを右クリックし、という名前のクラスを追加`OrderDTO`次の実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="ebba2-128">コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-128">Now add the controller.</span></span> <span data-ttu-id="ebba2-129">ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="ebba2-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="ebba2-130">選択**追加**を選択し、**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="ebba2-131">**コント ローラーの追加**ダイアログ ボックスで、次のオプションを設定します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="ebba2-132">**コント ローラー名**、"OrdersController"を入力します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="ebba2-133">**テンプレート**選択「を Entity Framework を使用して、読み取り/書き込みアクションがある API コント ローラー」。</span><span class="sxs-lookup"><span data-stu-id="ebba2-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="ebba2-134">**モデル クラス**、&quot;順序 (ProductStore.Models)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="ebba2-135">**データ コンテキスト クラス**、 &quot;OrdersContext (ProductStore.Models)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="ebba2-136">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ebba2-136">Click **Add**.</span></span> <span data-ttu-id="ebba2-137">これには、OrdersController.cs という名前のファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="ebba2-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="ebba2-138">次に、コント ローラーの既定の実装を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebba2-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="ebba2-139">最初に、削除、`PutOrder`と`DeleteOrder`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ebba2-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="ebba2-140">このサンプルでは、顧客は変更または既存の注文を削除できません。</span><span class="sxs-lookup"><span data-stu-id="ebba2-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="ebba2-141">実際のアプリケーションでは、このような場合を処理するためにバックエンド ロジックの多くする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebba2-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="ebba2-142">(たとえば、注文既に出荷でしょうか。)</span><span class="sxs-lookup"><span data-stu-id="ebba2-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="ebba2-143">変更、`GetOrders`をユーザーに属する注文だけを返すメソッド。</span><span class="sxs-lookup"><span data-stu-id="ebba2-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="ebba2-144">変更、`GetOrder`メソッドとして、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ebba2-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="ebba2-145">変更するためのメソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="ebba2-146">戻り値は、`OrderDTO`インスタンスの代わりに、`Order`します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="ebba2-147">使用して、注文データベース クエリを実行、ときに、 [DbQuery.Include](https://msdn.microsoft.com/library/gg696395)関連をフェッチするメソッド`OrderDetail`と`Product`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="ebba2-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="ebba2-148">射影を使用して結果をフラット化しました。</span><span class="sxs-lookup"><span data-stu-id="ebba2-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="ebba2-149">HTTP 応答の数量と製品の配列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ebba2-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="ebba2-150">この形式は、クライアントが元のオブジェクトのグラフは、入れ子になったエンティティ (注文、詳細、および製品) が含まれるよりも使用できるは簡単です。</span><span class="sxs-lookup"><span data-stu-id="ebba2-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="ebba2-151">最後のメソッドが考慮すべき`PostOrder`します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="ebba2-152">現在のところ、このメソッドは、`Order`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="ebba2-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="ebba2-153">何が起きるが、クライアントは、このような要求の本文を送信した場合。</span><span class="sxs-lookup"><span data-stu-id="ebba2-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="ebba2-154">これは、適切に構成された順序では、および Entity Framework では、データベースに挿入さいわいです。</span><span class="sxs-lookup"><span data-stu-id="ebba2-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="ebba2-155">以前は存在しなかった Product エンティティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ebba2-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="ebba2-156">クライアントは、新しい製品を作成したデータベースに!</span><span class="sxs-lookup"><span data-stu-id="ebba2-156">The client just created a new product in our database!</span></span> <span data-ttu-id="ebba2-157">クマの注文を表示するときは、順序、フルフィルメント業務部門に驚くことになります。</span><span class="sxs-lookup"><span data-stu-id="ebba2-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="ebba2-158">使用は、POST または PUT 要求でそのまま使用するデータについて本当に注意してください。</span><span class="sxs-lookup"><span data-stu-id="ebba2-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="ebba2-159">この問題を回避するには、変更、`PostOrder`メソッドを`OrderDTO`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="ebba2-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="ebba2-160">使用して、`OrderDTO`を作成する、`Order`します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="ebba2-161">使用に注意してください、`ProductID`と`Quantity`プロパティ、および製品名または価格のいずれかのクライアントが送信されるすべての値を無視します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="ebba2-162">製品 ID が有効でない場合は、データベースの外部キー制約に違反することとは、挿入は失敗します。</span><span class="sxs-lookup"><span data-stu-id="ebba2-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="ebba2-163">ここでは、完全な`PostOrder`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ebba2-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="ebba2-164">最後に、追加、 **Authorize**属性をコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="ebba2-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="ebba2-165">今すぐ登録されたユーザーのみは作成または注文を表示できます。</span><span class="sxs-lookup"><span data-stu-id="ebba2-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ebba2-166">[前へ](using-web-api-with-entity-framework-part-5.md)
> [次へ](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="ebba2-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
