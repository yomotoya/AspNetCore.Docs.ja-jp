---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Web API 2 OData v3 のエンティティ関係をサポートする |Microsoft ドキュメント
author: MikeWasson
description: ほとんどのデータ セットは、エンティティ間のリレーションを定義します。 お客様が設定されている順序です。ブックがある作成者です。製品では、仕入先があります。 OData を使用すると、クライアントは、経由で移動することができます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="f7d83-104">Web API 2 OData v3 のエンティティ関係をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f7d83-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="f7d83-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f7d83-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f7d83-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f7d83-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="f7d83-107">ほとんどのデータ セットは、エンティティ間のリレーションを定義します。 お客様が設定されている順序です。ブックがある作成者です。製品では、仕入先があります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="f7d83-108">クライアントは OData を使用すると、エンティティ関係経由で移動できます。</span><span class="sxs-lookup"><span data-stu-id="f7d83-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="f7d83-109">製品を指定するには、供給業者を検索できます。</span><span class="sxs-lookup"><span data-stu-id="f7d83-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="f7d83-110">作成または、リレーションシップを削除することができますも。</span><span class="sxs-lookup"><span data-stu-id="f7d83-110">You can also create or remove relationships.</span></span> <span data-ttu-id="f7d83-111">たとえば、製品の仕入先を設定できます。</span><span class="sxs-lookup"><span data-stu-id="f7d83-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="f7d83-112">このチュートリアルでは、ASP.NET Web API でこれらの操作をサポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="f7d83-113">このチュートリアルは、チュートリアルに基づいて[Web API 2 OData v3 エンドポイントを作成する](creating-an-odata-endpoint.md)です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f7d83-114">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="f7d83-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f7d83-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="f7d83-115">Web API 2</span></span>
> - <span data-ttu-id="f7d83-116">OData バージョン 3</span><span class="sxs-lookup"><span data-stu-id="f7d83-116">OData Version 3</span></span>
> - <span data-ttu-id="f7d83-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f7d83-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="f7d83-118">Supplier エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-118">Add a Supplier Entity</span></span>

<span data-ttu-id="f7d83-119">まず、OData フィードに新しいエンティティの種類を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="f7d83-120">追加、`Supplier`クラスです。</span><span class="sxs-lookup"><span data-stu-id="f7d83-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="f7d83-121">このクラスは、エンティティ キーの文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="f7d83-122">実際には、整数キーを使用するよりも少なくする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="f7d83-123">ですが、OData で整数以外の他の重要な型を処理する方法が表示される価値があります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="f7d83-124">次に、追加することでリレーションシップを作成します、`Supplier`プロパティを`Product`クラス。</span><span class="sxs-lookup"><span data-stu-id="f7d83-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="f7d83-125">新しい**DbSet**を`ProductServiceContext`クラス、Entity Framework が含まれるように、`Supplier`データベース内のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="f7d83-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="f7d83-126">WebApiConfig.cs では、EDM モデルを"Suppliers"エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="f7d83-127">ナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="f7d83-127">Navigation Properties</span></span>

<span data-ttu-id="f7d83-128">に、製品の供給業者を取得するには、は、クライアントは、GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="f7d83-129">ここで「業者」は、ナビゲーション プロパティで、`Product`型です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="f7d83-130">この場合、`Supplier`参照の単一の項目がナビゲーション プロパティもコレクションを取得できます (一対多または多対多のリレーションシップ)。</span><span class="sxs-lookup"><span data-stu-id="f7d83-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="f7d83-131">この要求をサポートするには、次のメソッドを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="f7d83-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="f7d83-132">*キー*パラメーターは、製品のキー。</span><span class="sxs-lookup"><span data-stu-id="f7d83-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="f7d83-133">メソッドを返します、関連するエンティティ & #8212 ここで、`Supplier`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="f7d83-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="f7d83-134">メソッド名とパラメーター名はどちらも重要です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="f7d83-135">一般に、ナビゲーション プロパティが"X"をという名前の場合は、「getx メソッド」という名前のメソッドを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="f7d83-136">メソッドは、という名前のパラメーターを受け取る必要があります"*キー*"親のキーのデータ型に一致します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="f7d83-137">含める必要も、 **[FromOdataUri]** 属性、*キー*パラメーター。</span><span class="sxs-lookup"><span data-stu-id="f7d83-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="f7d83-138">この属性では、Web API 要求 URI のキーを解析する場合は、OData 構文の規則を使用するように指示します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="f7d83-139">作成して、リンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-139">Creating and Deleting Links</span></span>

<span data-ttu-id="f7d83-140">OData は、2 つのエンティティ間のリレーションシップを作成または削除をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f7d83-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="f7d83-141">OData の用語で、リレーションシップは「リンク」です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="f7d83-142">各リンクには、フォームを使用して URI*エンティティ*/$links/*エンティティ*です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="f7d83-143">たとえば、製品から業者へのリンクは、このようになります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="f7d83-144">新しいリンクを作成するには、クライアントは、リンク URI に POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="f7d83-145">要求の本文は、ターゲット エンティティの URI です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="f7d83-146">たとえば、"CTSO"キーを使用して仕入先があるとします。</span><span class="sxs-lookup"><span data-stu-id="f7d83-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="f7d83-147">"Product(1)"から"Supplier('CTSO')"へのリンクを作成するには、クライアントは、次のように要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="f7d83-148">リンクを削除するのには、クライアントは、リンク URI に DELETE 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="f7d83-149">**リンクを作成します。**</span><span class="sxs-lookup"><span data-stu-id="f7d83-149">**Creating Links**</span></span>

<span data-ttu-id="f7d83-150">製品業者へのリンクを作成するクライアントを有効にする次のコードを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="f7d83-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="f7d83-151">このメソッドは、次の 3 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-151">This method takes three parameters:</span></span>

- <span data-ttu-id="f7d83-152">*キー*: 親エンティティ (製品) にキー</span><span class="sxs-lookup"><span data-stu-id="f7d83-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="f7d83-153">*navigationProperty*: ナビゲーション プロパティの名前。</span><span class="sxs-lookup"><span data-stu-id="f7d83-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="f7d83-154">この例では、唯一の有効なナビゲーション プロパティは、「サプライヤー」です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="f7d83-155">*リンク*: 関連エンティティの OData URI。</span><span class="sxs-lookup"><span data-stu-id="f7d83-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="f7d83-156">この値は、要求本文から取得されます。</span><span class="sxs-lookup"><span data-stu-id="f7d83-156">This value is taken from the request body.</span></span> <span data-ttu-id="f7d83-157">たとえば、リンク URI があります"`http://localhost/odata/Suppliers('CTSO')`、つまり仕入先 ID の 'CTSO' を = です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="f7d83-158">メソッドでは、リンクを使用して、供給業者を検索します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="f7d83-159">一致する業者が見つかった場合、メソッドを設定、`Product.Supplier`プロパティされ、データベースに結果を保存します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="f7d83-160">最も難しい部分は、リンク URI を解析しています。</span><span class="sxs-lookup"><span data-stu-id="f7d83-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="f7d83-161">基本的には、その URI に GET 要求を送信の結果をシミュレートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="f7d83-162">次のヘルパー メソッドは、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="f7d83-163">メソッドは、Web API ルーティング プロセスを起動し、返される、 **ODataPath**解析された OData パスを表すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="f7d83-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="f7d83-164">リンク URI には、エンティティ キー セグメントの 1 つがあります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="f7d83-165">(いない場合、クライアントが無効な URI を送信します。)</span><span class="sxs-lookup"><span data-stu-id="f7d83-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="f7d83-166">**リンクを削除します。**</span><span class="sxs-lookup"><span data-stu-id="f7d83-166">**Deleting Links**</span></span>

<span data-ttu-id="f7d83-167">リンクを削除するには、次のコードを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="f7d83-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="f7d83-168">この例では、ナビゲーション プロパティは、1 つ`Supplier`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="f7d83-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="f7d83-169">ナビゲーション プロパティがコレクションの場合は、リンクを削除する URI は、この関連エンティティのキーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="f7d83-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="f7d83-170">例:</span><span class="sxs-lookup"><span data-stu-id="f7d83-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="f7d83-171">この要求は、1 の顧客から注文 1 を削除します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="f7d83-172">この場合、DeleteLink メソッドは、次のシグネチャが得られます。</span><span class="sxs-lookup"><span data-stu-id="f7d83-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="f7d83-173">*RelatedKey*パラメーターは、この関連エンティティのキー。</span><span class="sxs-lookup"><span data-stu-id="f7d83-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="f7d83-174">`DeleteLink`メソッド、によってプライマリ エンティティを検索、*キー*パラメーター、によって、この関連エンティティを検索、 *relatedKey*パラメーター、および関連付けを削除します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="f7d83-175">によっては、データ モデルの両方のバージョンを実装する必要があります`DeleteLink`です。</span><span class="sxs-lookup"><span data-stu-id="f7d83-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="f7d83-176">Web API では、要求 URI に基づく適切なバージョンを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f7d83-176">Web API will call the correct version based on the request URI.</span></span>
