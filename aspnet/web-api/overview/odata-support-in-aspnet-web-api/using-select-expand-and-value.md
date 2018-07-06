---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: ASP.NET Web API 2 OData で $select, $expand, および $value を使用する |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: c5c0d374a918706e65254121ba5fa1516afdaf75
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814460"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="7c15e-102">ASP.NET Web API 2 OData で $select, $expand, および $value を使用する</span><span class="sxs-lookup"><span data-stu-id="7c15e-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="7c15e-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7c15e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7c15e-104">Web API 2 では、OData における $select, $expand, および $value オプションのサポートを追加しています。</span><span class="sxs-lookup"><span data-stu-id="7c15e-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="7c15e-105">これらのオプションは、サーバーから返された形式を制御することをクライアントに許可します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="7c15e-106">**$expand** は、応答にインラインで含まれる関連するエンティティを発生させます。</span><span class="sxs-lookup"><span data-stu-id="7c15e-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="7c15e-107">**$select**は、応答に含めるプロパティのサブセットを選択します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="7c15e-108">**$value**は、プロパティの未加工の値を取得します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="7c15e-109">スキーマの例</span><span class="sxs-lookup"><span data-stu-id="7c15e-109">Example Schema</span></span>

<span data-ttu-id="7c15e-110">この記事では、次の 3 つのエンティティを定義する OData サービスを使用します。 製品、仕入先、およびカテゴリ。</span><span class="sxs-lookup"><span data-stu-id="7c15e-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="7c15e-111">各製品には、1 つのカテゴリと仕入先の 1 つがあります。</span><span class="sxs-lookup"><span data-stu-id="7c15e-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="7c15e-112">エンティティ モデルを定義する c# クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="7c15e-113">注意、`Product`クラスのナビゲーション プロパティを定義、`Supplier`と`Category`します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="7c15e-114">`Category`クラスは、各カテゴリの製品のナビゲーション プロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="7c15e-115">このスキーマの OData エンドポイントを作成するには、Visual Studio 2013 のスキャフォールディングを」の説明に従って使用[で ASP.NET Web API OData エンドポイントを作成する](odata-v3/creating-an-odata-endpoint.md)します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="7c15e-116">製品、カテゴリ、および仕入先の別のコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="7c15e-117">$ の有効化を展開し、$select</span><span class="sxs-lookup"><span data-stu-id="7c15e-117">Enabling $expand and $select</span></span>

<span data-ttu-id="7c15e-118">Visual Studio 2013 では、Web API OData のスキャフォールディングは、その $ の展開を自動的にサポートおよび $select にコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="7c15e-119">リファレンスについては、ここでは $ をサポートするための要件を展開し、コント ローラーの $select します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="7c15e-120">コレクションの場合、コント ローラーの`Get`メソッドが返す必要があります、 **IQueryable**します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="7c15e-121">単一のエンティティを返す、 **SingleResult&lt;T&gt;**、T は、 **IQueryable** 0 または 1 つのエンティティを格納しています。</span><span class="sxs-lookup"><span data-stu-id="7c15e-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="7c15e-122">また、装飾、`Get`メソッド、 **[Queryable]** 属性は、前のコード スニペットで示すように。</span><span class="sxs-lookup"><span data-stu-id="7c15e-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="7c15e-123">代わりに、呼び出す**EnableQuerySupport**上、 **HttpConfiguration**スタートアップ時のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7c15e-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="7c15e-124">(詳細については、次を参照してください[OData クエリ オプションを有効にする](supporting-odata-query-options.md#enable)。)。</span><span class="sxs-lookup"><span data-stu-id="7c15e-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="7c15e-125">$ を使用して展開</span><span class="sxs-lookup"><span data-stu-id="7c15e-125">Using $expand</span></span>

<span data-ttu-id="7c15e-126">OData エンティティまたはコレクションを照会するときに、既定の応答に関連するエンティティは含まれません。</span><span class="sxs-lookup"><span data-stu-id="7c15e-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="7c15e-127">たとえば、カテゴリ エンティティ セットの既定の応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="7c15e-128">ご覧のように、応答は含まれません任意の製品では、Category エンティティは製品のナビゲーション リンク。</span><span class="sxs-lookup"><span data-stu-id="7c15e-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="7c15e-129">ただし、クライアントは $ を使用できる展開すると、各カテゴリの製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="7c15e-130">$Expand オプションを展開し、要求のクエリ文字列には。</span><span class="sxs-lookup"><span data-stu-id="7c15e-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="7c15e-131">これで、サーバー カテゴリごとに、インラインで、カテゴリの製品が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7c15e-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="7c15e-132">応答ペイロードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="7c15e-133">"Value"配列内の各エントリに製品一覧が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="7c15e-134">$ では、展開するナビゲーション プロパティのオプションはコンマ区切りの一覧を展開します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="7c15e-135">次の要求は、カテゴリと製品の仕入先の両方を展開します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="7c15e-136">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="7c15e-137">ナビゲーション プロパティの 1 つ以上のレベルを展開することができます。</span><span class="sxs-lookup"><span data-stu-id="7c15e-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="7c15e-138">次の例には、カテゴリのすべての製品と各製品の仕入先が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7c15e-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="7c15e-139">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="7c15e-140">既定では、Web API は、2、最大の深さを制限します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="7c15e-141">ような複雑な要求を送信することから、クライアントを妨げる`$expand=Orders/OrderDetails/Product/Supplier/Region`、効率的にクエリを実行し、大規模な応答を作成するがあります。</span><span class="sxs-lookup"><span data-stu-id="7c15e-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="7c15e-142">既定値を上書きするには、設定、 **MaxExpansionDepth**プロパティを **[Queryable]** 属性。</span><span class="sxs-lookup"><span data-stu-id="7c15e-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="7c15e-143">$ の詳細については、オプションを展開するを参照してください。 [Expand システム クエリ オプション ($ の展開)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) 、公式の OData ドキュメント。</span><span class="sxs-lookup"><span data-stu-id="7c15e-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="7c15e-144">$Select を使用します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-144">Using $select</span></span>

<span data-ttu-id="7c15e-145">$Select オプションでは、応答本文に含めるプロパティのサブセットを指定します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="7c15e-146">たとえば、名前と各製品の価格を取得するには、次のクエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="7c15e-147">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="7c15e-148">$Select を組み合わせることができ、$ が同じクエリ内で展開します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="7c15e-149">$Select オプションに展開されたプロパティを含めるに確認します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="7c15e-150">たとえば、次の要求は、業者と製品の名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="7c15e-151">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="7c15e-152">展開のプロパティ内のプロパティを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="7c15e-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="7c15e-153">次の要求では、製品を展開し、カテゴリ名と製品名を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="7c15e-154">応答本文を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="7c15e-155">$Select オプションの詳細については、次を参照してください。 [Select システム クエリ オプション ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) 、公式の OData ドキュメント。</span><span class="sxs-lookup"><span data-stu-id="7c15e-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="7c15e-156">エンティティ ($value) の個々 のプロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="7c15e-157">エンティティから個々 のプロパティを取得する OData クライアントの 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="7c15e-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="7c15e-158">クライアントでは、OData の形式で値を取得することまたは、プロパティの生の値を取得できます。</span><span class="sxs-lookup"><span data-stu-id="7c15e-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="7c15e-159">次の要求は、OData 形式でプロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="7c15e-160">JSON 形式で応答の例を示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="7c15e-161">プロパティの生の値を取得するには、URI に $value を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="7c15e-162">応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-162">Here is the response.</span></span> <span data-ttu-id="7c15e-163">コンテンツの種類が"text/plain"、JSON に注意してください。</span><span class="sxs-lookup"><span data-stu-id="7c15e-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="7c15e-164">という名前のメソッドを追加すると、OData コント ローラーでこれらのクエリをサポートするために`GetProperty`ここで、`Property`プロパティの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="7c15e-165">たとえば、Name プロパティを取得するメソッドを名前は`GetName`します。</span><span class="sxs-lookup"><span data-stu-id="7c15e-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="7c15e-166">メソッドは、そのプロパティの値を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="7c15e-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
