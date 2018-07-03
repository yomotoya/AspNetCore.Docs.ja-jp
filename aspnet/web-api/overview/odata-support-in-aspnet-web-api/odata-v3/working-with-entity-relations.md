---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Web API 2 OData v3 のエンティティ関係サポート |Microsoft Docs
author: MikeWasson
description: ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。 OData を使用して、クライアントは、経由で移動することができます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 311e84a2beb3ec7661fd650b277f23458bcb0cb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377478"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="4b959-104">Web API 2 OData v3 のエンティティ関係をサポート</span><span class="sxs-lookup"><span data-stu-id="4b959-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="4b959-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4b959-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4b959-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="4b959-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="4b959-107">ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。</span><span class="sxs-lookup"><span data-stu-id="4b959-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="4b959-108">クライアントは OData を使用して、エンティティ関係を移動できます。</span><span class="sxs-lookup"><span data-stu-id="4b959-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="4b959-109">製品を指定するには、業者を検索できます。</span><span class="sxs-lookup"><span data-stu-id="4b959-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="4b959-110">作成またはのリレーションシップを削除することもできます。</span><span class="sxs-lookup"><span data-stu-id="4b959-110">You can also create or remove relationships.</span></span> <span data-ttu-id="4b959-111">たとえば、製品の仕入先を設定できます。</span><span class="sxs-lookup"><span data-stu-id="4b959-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="4b959-112">このチュートリアルでは、ASP.NET Web API でこれらの操作をサポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4b959-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="4b959-113">チュートリアルは、チュートリアルに基づいて[Web API 2 OData v3 エンドポイントを作成する](creating-an-odata-endpoint.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b959-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4b959-114">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="4b959-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4b959-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="4b959-115">Web API 2</span></span>
> - <span data-ttu-id="4b959-116">OData バージョン 3</span><span class="sxs-lookup"><span data-stu-id="4b959-116">OData Version 3</span></span>
> - <span data-ttu-id="4b959-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4b959-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="4b959-118">Supplier エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="4b959-118">Add a Supplier Entity</span></span>

<span data-ttu-id="4b959-119">まず、OData フィードに新しいエンティティ型を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b959-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="4b959-120">追加、`Supplier`クラス。</span><span class="sxs-lookup"><span data-stu-id="4b959-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="4b959-121">このクラスは、エンティティ キーの文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="4b959-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="4b959-122">実際には、整数型のキーを使用するよりも少なくする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b959-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="4b959-123">その価値は OData で整数以外のキーの型を処理する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="4b959-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="4b959-124">次に、リレーションシップを追加することで作成します、`Supplier`プロパティを`Product`クラス。</span><span class="sxs-lookup"><span data-stu-id="4b959-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="4b959-125">新しい追加**DbSet**を`ProductServiceContext`クラス、Entity Framework が含まれるように、`Supplier`データベース内のテーブル。</span><span class="sxs-lookup"><span data-stu-id="4b959-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="4b959-126">WebApiConfig.cs では、EDM モデルに"Suppliers"エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="4b959-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="4b959-127">ナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="4b959-127">Navigation Properties</span></span>

<span data-ttu-id="4b959-128">供給業者の製品を取得するには、クライアントは、GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="4b959-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="4b959-129">ここで、ナビゲーション プロパティはで、「業者」、`Product`型。</span><span class="sxs-lookup"><span data-stu-id="4b959-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="4b959-130">この場合、`Supplier`は 1 つの項目がナビゲーション プロパティも (一対多または多対多のリレーションシップ) のコレクションを返します。</span><span class="sxs-lookup"><span data-stu-id="4b959-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="4b959-131">この要求をサポートするには、次のメソッドを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="4b959-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="4b959-132">*キー*パラメーターは、製品のキー。</span><span class="sxs-lookup"><span data-stu-id="4b959-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="4b959-133">メソッドは、関連するエンティティ & #8212 をここでは、返します、`Supplier`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="4b959-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="4b959-134">パラメーター名とメソッドの名前は、どちらも重要です。</span><span class="sxs-lookup"><span data-stu-id="4b959-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="4b959-135">一般に、ナビゲーション プロパティは、"X"という名前が場合、は、「getx メソッド」という名前のメソッドを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b959-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="4b959-136">メソッドは、という名前のパラメーターを受け取る必要があります"*キー*"親のキーのデータ型と一致します。</span><span class="sxs-lookup"><span data-stu-id="4b959-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="4b959-137">必ず、 **[FromOdataUri]** 属性、*キー*パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4b959-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="4b959-138">この属性では、Web API 要求 URI からキーを解析するときに、OData 構文の規則を使用するように指示します。</span><span class="sxs-lookup"><span data-stu-id="4b959-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="4b959-139">作成およびリンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="4b959-139">Creating and Deleting Links</span></span>

<span data-ttu-id="4b959-140">OData では、2 つのエンティティ間のリレーションシップを作成または削除をサポートします。</span><span class="sxs-lookup"><span data-stu-id="4b959-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="4b959-141">OData 用語で「リンク」とは関係</span><span class="sxs-lookup"><span data-stu-id="4b959-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="4b959-142">各リンクは、フォームを使用して URI*エンティティ*/$links/*エンティティ*します。</span><span class="sxs-lookup"><span data-stu-id="4b959-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="4b959-143">たとえば、製品から業者へのリンクは、ようになります。</span><span class="sxs-lookup"><span data-stu-id="4b959-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="4b959-144">新しいリンクを作成するには、クライアントは、リンク URI に POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="4b959-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="4b959-145">要求の本文は、ターゲット エンティティの URI です。</span><span class="sxs-lookup"><span data-stu-id="4b959-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="4b959-146">たとえば、"CTSO"キーを使用して仕入先があるとします。</span><span class="sxs-lookup"><span data-stu-id="4b959-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="4b959-147">"Product(1)"から"Supplier('CTSO')"へのリンクを作成するには、クライアントは、次のような要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="4b959-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="4b959-148">リンクを削除するには、クライアントは、リンク URI に DELETE 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="4b959-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="4b959-149">**リンクを作成します。**</span><span class="sxs-lookup"><span data-stu-id="4b959-149">**Creating Links**</span></span>

<span data-ttu-id="4b959-150">供給業者の製品リンクを作成するクライアントを有効にするには、次のコードを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="4b959-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="4b959-151">このメソッドは、3 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="4b959-151">This method takes three parameters:</span></span>

- <span data-ttu-id="4b959-152">*キー*: 親エンティティ (製品) にキー</span><span class="sxs-lookup"><span data-stu-id="4b959-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="4b959-153">*navigationProperty*: ナビゲーション プロパティの名前。</span><span class="sxs-lookup"><span data-stu-id="4b959-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="4b959-154">この例では、唯一の有効なナビゲーション プロパティは、「サプライヤー」です。</span><span class="sxs-lookup"><span data-stu-id="4b959-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="4b959-155">*リンク*: 関連エンティティの OData URI。</span><span class="sxs-lookup"><span data-stu-id="4b959-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="4b959-156">この値は、要求本文から取得されます。</span><span class="sxs-lookup"><span data-stu-id="4b959-156">This value is taken from the request body.</span></span> <span data-ttu-id="4b959-157">などのリンク URI があります"`http://localhost/odata/Suppliers('CTSO')`、つまり、仕入先 ID = 'CTSO'。</span><span class="sxs-lookup"><span data-stu-id="4b959-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="4b959-158">メソッドは、業者を検索する、リンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="4b959-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="4b959-159">場合は、一致する業者が見つかると、設定、`Product.Supplier`プロパティと、データベースに結果を保存します。</span><span class="sxs-lookup"><span data-stu-id="4b959-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="4b959-160">最も難しい部分は、リンク URI を解析しています。</span><span class="sxs-lookup"><span data-stu-id="4b959-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="4b959-161">基本的に、その URI に GET 要求を送信する結果をシミュレートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b959-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="4b959-162">次のヘルパー メソッドは、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4b959-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="4b959-163">メソッドは、Web API ルーティング プロセスを起動し、失意、 **ODataPath**解析された OData パスを表すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="4b959-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="4b959-164">リンク URI では、エンティティ キー、セグメントの 1 つがあります。</span><span class="sxs-lookup"><span data-stu-id="4b959-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="4b959-165">(ない場合、クライアントが不適切な URI を送信します。)</span><span class="sxs-lookup"><span data-stu-id="4b959-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="4b959-166">**リンクの削除**</span><span class="sxs-lookup"><span data-stu-id="4b959-166">**Deleting Links**</span></span>

<span data-ttu-id="4b959-167">リンクを削除するには、次のコードを追加、`ProductsController`クラス。</span><span class="sxs-lookup"><span data-stu-id="4b959-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="4b959-168">この例では、ナビゲーション プロパティは、1 つ`Supplier`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="4b959-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="4b959-169">ナビゲーション プロパティがコレクションである場合は、リンクを削除する URI は、関連エンティティのキーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b959-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="4b959-170">例えば:</span><span class="sxs-lookup"><span data-stu-id="4b959-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="4b959-171">この要求は、1 顧客から注文 1 を削除します。</span><span class="sxs-lookup"><span data-stu-id="4b959-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="4b959-172">この場合、DeleteLink メソッドは、次のシグネチャが完成します。</span><span class="sxs-lookup"><span data-stu-id="4b959-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="4b959-173">*RelatedKey*パラメーターは、関連するエンティティのキー。</span><span class="sxs-lookup"><span data-stu-id="4b959-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="4b959-174">`DeleteLink`メソッドによって、プライマリ エンティティを参照してください、*キー*パラメーターによって、関連するエンティティを検索、 *relatedKey*パラメーター、および [削除] の関連付け。</span><span class="sxs-lookup"><span data-stu-id="4b959-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="4b959-175">データ モデルによっては、両方のバージョンを実装する必要あります`DeleteLink`します。</span><span class="sxs-lookup"><span data-stu-id="4b959-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="4b959-176">Web API では、要求 URI に基づく適切なバージョンを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4b959-176">Web API will call the correct version based on the request URI.</span></span>
