---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: ASP.NET Web API 2.2 を使用して OData v4 のエンティティ関係 |Microsoft Docs
author: MikeWasson
description: ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。 OData を使用して、クライアントは、経由で移動することができます.
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d07ddab83462ee1bc84ba8ab15fe906937f506e6
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795393"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="3f993-104">ASP.NET Web API 2.2 を使用して OData v4 のエンティティ関係</span><span class="sxs-lookup"><span data-stu-id="3f993-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="3f993-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3f993-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3f993-106">ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。</span><span class="sxs-lookup"><span data-stu-id="3f993-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="3f993-107">クライアントは OData を使用して、エンティティ関係を移動できます。</span><span class="sxs-lookup"><span data-stu-id="3f993-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="3f993-108">製品を指定するには、業者を検索できます。</span><span class="sxs-lookup"><span data-stu-id="3f993-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="3f993-109">作成またはのリレーションシップを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="3f993-109">You can also create or remove relationships.</span></span> <span data-ttu-id="3f993-110">たとえば、製品の仕入先を設定できます。</span><span class="sxs-lookup"><span data-stu-id="3f993-110">For example, you can set the supplier for a product.</span></span>
>
> <span data-ttu-id="3f993-111">このチュートリアルでは、ASP.NET Web API を使用しての OData v4 のこれらの操作をサポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3f993-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="3f993-112">チュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)です。</span><span class="sxs-lookup"><span data-stu-id="3f993-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3f993-113">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="3f993-113">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="3f993-114">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="3f993-114">Web API 2.1</span></span>
> - <span data-ttu-id="3f993-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="3f993-115">OData v4</span></span>
> - <span data-ttu-id="3f993-116">Visual Studio 2013 (Visual Studio 2017 ダウンロード[ここ](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="3f993-116">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="3f993-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3f993-117">Entity Framework 6</span></span>
> - <span data-ttu-id="3f993-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3f993-118">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="3f993-119">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="3f993-119">Tutorial versions</span></span>
>
> <span data-ttu-id="3f993-120">OData バージョン 3 では、次を参照してください。 [OData v3 のエンティティ関係をサポートしている](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations)します。</span><span class="sxs-lookup"><span data-stu-id="3f993-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="3f993-121">Supplier エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="3f993-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="3f993-122">チュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)です。</span><span class="sxs-lookup"><span data-stu-id="3f993-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="3f993-123">最初に、関連エンティティ必要があります。</span><span class="sxs-lookup"><span data-stu-id="3f993-123">First, we need a related entity.</span></span> <span data-ttu-id="3f993-124">という名前のクラスを追加`Supplier`Models フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="3f993-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="3f993-125">ナビゲーション プロパティを追加、`Product`クラス。</span><span class="sxs-lookup"><span data-stu-id="3f993-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="3f993-126">新しい追加**DbSet**を`ProductsContext`クラス、Entity Framework は、データベースの仕入先のテーブルを含めるようにします。</span><span class="sxs-lookup"><span data-stu-id="3f993-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="3f993-127">WebApiConfig.cs で追加、 &quot;Suppliers&quot;エンティティがエンティティ データ モデルに設定。</span><span class="sxs-lookup"><span data-stu-id="3f993-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="3f993-128">仕入先のコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3f993-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="3f993-129">追加、`SuppliersController`コント ローラーのフォルダーにクラス。</span><span class="sxs-lookup"><span data-stu-id="3f993-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="3f993-130">このコント ローラーに対する CRUD 操作を追加する方法は表示されません。</span><span class="sxs-lookup"><span data-stu-id="3f993-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="3f993-131">手順は、Products コント ローラーの場合と同じ (を参照してください[OData v4 エンドポイントを作成](create-an-odata-v4-endpoint.md))。</span><span class="sxs-lookup"><span data-stu-id="3f993-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="3f993-132">関連エンティティの取得</span><span class="sxs-lookup"><span data-stu-id="3f993-132">Getting Related Entities</span></span>

<span data-ttu-id="3f993-133">供給業者の製品を取得するには、クライアントは、GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="3f993-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="3f993-134">この要求をサポートするには、次のメソッドを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="3f993-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="3f993-135">このメソッドは、既定の名前付け規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="3f993-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="3f993-136">メソッド名: getx メソッド、X はナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="3f993-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="3f993-137">パラメーター名:*キー*</span><span class="sxs-lookup"><span data-stu-id="3f993-137">Parameter name: *key*</span></span>

<span data-ttu-id="3f993-138">この名前付け規則に従うと、コント ローラー メソッドに HTTP 要求は、Web API によって自動的にマップされます。</span><span class="sxs-lookup"><span data-stu-id="3f993-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="3f993-139">要求の例 HTTP:</span><span class="sxs-lookup"><span data-stu-id="3f993-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="3f993-140">応答の例 HTTP:</span><span class="sxs-lookup"><span data-stu-id="3f993-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="3f993-141">関連コレクションを取得します。</span><span class="sxs-lookup"><span data-stu-id="3f993-141">Getting a related collection</span></span>

<span data-ttu-id="3f993-142">前の例では、製品には、1 つの仕入先があります。</span><span class="sxs-lookup"><span data-stu-id="3f993-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="3f993-143">ナビゲーション プロパティでは、コレクションを返すこともできます。</span><span class="sxs-lookup"><span data-stu-id="3f993-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="3f993-144">次のコードでは、サプライヤーの製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="3f993-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="3f993-145">この場合、メソッドが返されます、 **IQueryable**の代わりに、 **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="3f993-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="3f993-146">要求の例 HTTP:</span><span class="sxs-lookup"><span data-stu-id="3f993-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="3f993-147">応答の例 HTTP:</span><span class="sxs-lookup"><span data-stu-id="3f993-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="3f993-148">エンティティ間のリレーションシップを作成します。</span><span class="sxs-lookup"><span data-stu-id="3f993-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="3f993-149">OData では、作成または既存の 2 つのエンティティ間のリレーションシップの削除をサポートします。</span><span class="sxs-lookup"><span data-stu-id="3f993-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="3f993-150">OData v4 用語では、リレーションシップは、&quot;参照&quot;します。</span><span class="sxs-lookup"><span data-stu-id="3f993-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="3f993-151">(OData v3 の場合は、リレーションシップが呼び出された、*リンク*します。</span><span class="sxs-lookup"><span data-stu-id="3f993-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="3f993-152">プロトコルの相違点は関係ありませんチュートリアルではこの。)</span><span class="sxs-lookup"><span data-stu-id="3f993-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="3f993-153">参照がフォームに、独自の URI`/Entity/NavigationProperty/$ref`します。</span><span class="sxs-lookup"><span data-stu-id="3f993-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="3f993-154">たとえば、製品とその供給業者間の参照に対応する URI を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3f993-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="3f993-155">リレーションシップを追加するには、クライアントは、このアドレスに POST または PUT 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="3f993-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="3f993-156">ナビゲーション プロパティが 1 つのエンティティなどに設定されている場合に PUT`Product.Supplier`します。</span><span class="sxs-lookup"><span data-stu-id="3f993-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="3f993-157">ナビゲーション プロパティが、コレクションなどに設定されている場合に投稿`Supplier.Products`します。</span><span class="sxs-lookup"><span data-stu-id="3f993-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="3f993-158">要求の本文には、リレーションシップのもう一方のエンティティの URI が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3f993-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="3f993-159">要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3f993-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="3f993-160">この例では、クライアントが送信する PUT 要求を`/Products(6)/Supplier/$ref`、$ref URI である、 `Supplier` ID を持つ製品の 6 を =。</span><span class="sxs-lookup"><span data-stu-id="3f993-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="3f993-161">要求が成功すると、204 (No Content) 応答がサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="3f993-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="3f993-162">リレーションシップを追加するコント ローラー メソッドを次に示します、 `Product`:</span><span class="sxs-lookup"><span data-stu-id="3f993-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="3f993-163">*NavigationProperty*パラメーターを設定するには、どのリレーションシップを指定します。</span><span class="sxs-lookup"><span data-stu-id="3f993-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="3f993-164">(エンティティには、複数のナビゲーション プロパティがある場合は、詳細を追加`case`ステートメントです)。</span><span class="sxs-lookup"><span data-stu-id="3f993-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="3f993-165">*リンク*パラメーターには、サプライヤーの URI が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3f993-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="3f993-166">Web API には、このパラメーターの値を取得する要求本文に自動的に解析します。</span><span class="sxs-lookup"><span data-stu-id="3f993-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="3f993-167">供給業者を検索する必要があります ID (またはキー) の一部では、*リンク*パラメーター。</span><span class="sxs-lookup"><span data-stu-id="3f993-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="3f993-168">これを行うには、次のヘルパー メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3f993-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="3f993-169">基本的には、このメソッドでは、OData ライブラリを使用して、URI のパスをセグメントに分割、キーを含むセグメントを見つける、キーに適切な型変換をします。</span><span class="sxs-lookup"><span data-stu-id="3f993-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="3f993-170">エンティティ間のリレーションシップを削除します。</span><span class="sxs-lookup"><span data-stu-id="3f993-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="3f993-171">リレーションシップを削除するには、クライアントは $ref URI に HTTP DELETE 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="3f993-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="3f993-172">製品と仕入先の間のリレーションシップを削除するコント ローラー メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3f993-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="3f993-173">この場合、`Product.Supplier`は、 &quot;1&quot;設定するだけで、リレーションシップを削除できるように 1 対多のリレーションシップの終了`Product.Supplier`に`null`します。</span><span class="sxs-lookup"><span data-stu-id="3f993-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="3f993-174">&quot;多く&quot;を削除するエンティティに関連するリレーションシップ、クライアントの最後を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3f993-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="3f993-175">これを行うには、クライアントは、要求のクエリ文字列で、関連するエンティティの URI を送信します。</span><span class="sxs-lookup"><span data-stu-id="3f993-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="3f993-176">たとえば、「製品 1」から削除する"1" 仕入先。</span><span class="sxs-lookup"><span data-stu-id="3f993-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="3f993-177">Web API でこれをサポートする必要がありますで追加のパラメーターを含める、`DeleteRef`メソッド。</span><span class="sxs-lookup"><span data-stu-id="3f993-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="3f993-178">製品を削除するコント ローラー メソッドを次に示します、`Supplier.Products`関係します。</span><span class="sxs-lookup"><span data-stu-id="3f993-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="3f993-179">*キー*パラメーターは、業者のキーと*relatedKey*パラメーターは、製品から削除するキー、`Products`リレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="3f993-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="3f993-180">Web API が、クエリ文字列から、キーを自動的に取得することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3f993-180">Note that Web API automatically gets the key from the query string.</span></span>
