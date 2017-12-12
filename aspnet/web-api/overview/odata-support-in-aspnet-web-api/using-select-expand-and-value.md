---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "$Select を使用すると、$を展開し、ASP.NET Web API 2 OData $value |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="63888-102">$Select を使用すると、$を展開し、ASP.NET Web API 2 OData $value</span><span class="sxs-lookup"><span data-stu-id="63888-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="63888-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="63888-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="63888-104">Web API 2 は、$ の展開、$select、および OData $value オプションのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="63888-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="63888-105">これらのオプションは、サーバーから返される形式を制御するクライアントを許可します。</span><span class="sxs-lookup"><span data-stu-id="63888-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="63888-106">**$展開**と、関連するエンティティの応答にインラインで含まれます。</span><span class="sxs-lookup"><span data-stu-id="63888-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="63888-107">**$select**応答に含めるプロパティのサブセットを選択します。</span><span class="sxs-lookup"><span data-stu-id="63888-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="63888-108">**$value**プロパティの生の値を取得します。</span><span class="sxs-lookup"><span data-stu-id="63888-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="63888-109">例のスキーマ</span><span class="sxs-lookup"><span data-stu-id="63888-109">Example Schema</span></span>

<span data-ttu-id="63888-110">この記事では、次の 3 つのエンティティを定義する OData サービスを使用します。 商品、供給業者、およびカテゴリ。</span><span class="sxs-lookup"><span data-stu-id="63888-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="63888-111">各製品には、1 つのカテゴリと 1 つの仕入先があります。</span><span class="sxs-lookup"><span data-stu-id="63888-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="63888-112">エンティティ モデルを定義する c# クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="63888-113">注意して、`Product`クラスのナビゲーション プロパティを定義、`Supplier`と`Category`です。</span><span class="sxs-lookup"><span data-stu-id="63888-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="63888-114">`Category`クラスは、各カテゴリの製品のナビゲーション プロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="63888-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="63888-115">このスキーマの OData エンドポイントを作成するには、Visual Studio 2013 のスキャフォールディングを」の説明に従って使用[ASP.NET Web API で OData エンドポイントを作成する](odata-v3/creating-an-odata-endpoint.md)です。</span><span class="sxs-lookup"><span data-stu-id="63888-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="63888-116">製品、カテゴリ、および仕入先の別のコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="63888-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="63888-117">$ の有効化を展開し、$select</span><span class="sxs-lookup"><span data-stu-id="63888-117">Enabling $expand and $select</span></span>

<span data-ttu-id="63888-118">Visual Studio 2013 では、Web API OData のスキャフォールディングは、その自動的にサポートしている $展開と $select にコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="63888-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="63888-119">リファレンスについては、ここでは、$ をサポートするための要件を展開し、コント ローラーの $select します。</span><span class="sxs-lookup"><span data-stu-id="63888-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="63888-120">コレクションの場合、コント ローラーの`Get`メソッドが返す必要があります、 **IQueryable**です。</span><span class="sxs-lookup"><span data-stu-id="63888-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="63888-121">単一のエンティティを返す、 **SingleResult&lt;T&gt;**ここで T は、 **IQueryable** 0 または 1 つのエンティティを格納しています。</span><span class="sxs-lookup"><span data-stu-id="63888-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="63888-122">装飾することも、`Get`メソッド、 **[Queryable]**属性、前のコード スニペットで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="63888-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="63888-123">代わりに、呼び出す**EnableQuerySupport**上、 **HttpConfiguration**スタートアップ時のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="63888-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="63888-124">(詳細については、次を参照してください[OData クエリ オプションを有効にすると](supporting-odata-query-options.md#enable)。)。</span><span class="sxs-lookup"><span data-stu-id="63888-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="63888-125">$ を使用して展開</span><span class="sxs-lookup"><span data-stu-id="63888-125">Using $expand</span></span>

<span data-ttu-id="63888-126">OData エンティティまたはコレクションのクエリを実行する場合、既定の応答には関連エンティティが含まれません。</span><span class="sxs-lookup"><span data-stu-id="63888-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="63888-127">たとえば、カテゴリ エンティティ セットの既定の応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="63888-128">ご覧のように、応答は含まれません、製品、Category エンティティが製品にナビゲーション リンクを持っていて。</span><span class="sxs-lookup"><span data-stu-id="63888-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="63888-129">ただし、クライアントが $ を使用して展開すると、各カテゴリの製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="63888-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="63888-130">$ オプションを展開し、要求のクエリ文字列には。</span><span class="sxs-lookup"><span data-stu-id="63888-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="63888-131">これで、サーバー カテゴリごとに、インラインでカテゴリの製品が含まれます。</span><span class="sxs-lookup"><span data-stu-id="63888-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="63888-132">応答ペイロードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="63888-133">"Value"配列の各エントリに製品の一覧が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="63888-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="63888-134">$ は、展開するナビゲーション プロパティのオプションは、コンマで区切られた一覧を展開します。</span><span class="sxs-lookup"><span data-stu-id="63888-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="63888-135">次の要求は、カテゴリ、製品の供給業者の両方を展開します。</span><span class="sxs-lookup"><span data-stu-id="63888-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="63888-136">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="63888-137">ナビゲーション プロパティの 1 つ以上のレベルを展開することができます。</span><span class="sxs-lookup"><span data-stu-id="63888-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="63888-138">次の例では、カテゴリのすべての製品と製品ごとのサプライヤーのも含まれています。</span><span class="sxs-lookup"><span data-stu-id="63888-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="63888-139">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="63888-140">既定では、Web API は、2 に展開の最大の深さを制限します。</span><span class="sxs-lookup"><span data-stu-id="63888-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="63888-141">妨げているクライアントと同様に複雑な要求を送信する`$expand=Orders/OrderDetails/Product/Supplier/Region`、されない可能性があるクエリを実行し、大規模な応答を作成する効率的です。</span><span class="sxs-lookup"><span data-stu-id="63888-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="63888-142">既定をオーバーライドするには設定、 **MaxExpansionDepth**プロパティを**[Queryable]**属性。</span><span class="sxs-lookup"><span data-stu-id="63888-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="63888-143">$ の詳細については、オプションを展開しを参照してください[Expand システム クエリ オプション ($展開)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand)公式の OData ドキュメントにします。</span><span class="sxs-lookup"><span data-stu-id="63888-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="63888-144">$Select を使用します。</span><span class="sxs-lookup"><span data-stu-id="63888-144">Using $select</span></span>

<span data-ttu-id="63888-145">$Select オプションでは、応答本文に含めるプロパティのサブセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="63888-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="63888-146">たとえば、名前と各製品の価格だけを取得するには、次のクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="63888-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="63888-147">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="63888-148">$Select を組み合わせることができ、同じクエリで $ を展開します。</span><span class="sxs-lookup"><span data-stu-id="63888-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="63888-149">$Select オプションに展開されたプロパティを含めることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="63888-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="63888-150">たとえば、次の要求は、業者と製品の名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="63888-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="63888-151">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="63888-152">展開されたプロパティ内のプロパティを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="63888-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="63888-153">次の要求は、製品を展開し、カテゴリ名と製品名を選択します。</span><span class="sxs-lookup"><span data-stu-id="63888-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="63888-154">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="63888-155">$Select オプションの詳細については、次を参照してください。[選択システム クエリ オプション ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select)公式の OData ドキュメントにします。</span><span class="sxs-lookup"><span data-stu-id="63888-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="63888-156">エンティティ ($value) の個々 のプロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="63888-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="63888-157">エンティティから個々 のプロパティを取得する OData クライアントの 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="63888-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="63888-158">クライアントでは、OData 形式で値を取得することまたは、プロパティの生の値を取得できます。</span><span class="sxs-lookup"><span data-stu-id="63888-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="63888-159">次の要求は、OData 形式でプロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="63888-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="63888-160">JSON 形式で応答の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="63888-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="63888-161">プロパティの生の値を取得するには、URI に $value を追加します。</span><span class="sxs-lookup"><span data-stu-id="63888-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="63888-162">ここでは応答です。</span><span class="sxs-lookup"><span data-stu-id="63888-162">Here is the response.</span></span> <span data-ttu-id="63888-163">コンテンツの種類が「テキスト/プレーン」されない JSON に注意してください。</span><span class="sxs-lookup"><span data-stu-id="63888-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="63888-164">これらのクエリ、OData コント ローラーをサポートするには、という名前のメソッドを追加`GetProperty`ここで、`Property`プロパティの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="63888-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="63888-165">たとえば、Name プロパティを取得するメソッドの名前は`GetName`します。</span><span class="sxs-lookup"><span data-stu-id="63888-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="63888-166">メソッドは、そのプロパティの値を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="63888-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
