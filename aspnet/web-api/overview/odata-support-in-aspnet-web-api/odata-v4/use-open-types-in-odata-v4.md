---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: ASP.NET Web API を使用した OData v4 の種類を開く |Microsoft Docs
author: microsoft
description: OData v4 のオープン型は、型定義で宣言されている任意のプロパティだけでなく、動的プロパティを含む stuctured 型です。 開く...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 77771d85532b8b622c2ad4ca219a38990e474c9c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823755"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="c1611-104">ASP.NET Web API を使用した OData v4 の種類を開く</span><span class="sxs-lookup"><span data-stu-id="c1611-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="c1611-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c1611-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c1611-106">OData v4 の*オープン型*は型定義で宣言されている任意のプロパティだけでなく、動的プロパティを含む stuctured 型です。</span><span class="sxs-lookup"><span data-stu-id="c1611-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="c1611-107">オープン型では、データ モデルに柔軟性を追加できます。</span><span class="sxs-lookup"><span data-stu-id="c1611-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="c1611-108">このチュートリアルでは、ASP.NET Web API OData でオープン型を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c1611-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="c1611-109">このチュートリアルでは、ASP.NET Web API で OData エンドポイントを作成する方法を既に把握している前提としています。</span><span class="sxs-lookup"><span data-stu-id="c1611-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="c1611-110">そうでない場合は、を読み取ることによって開始[OData v4 エンドポイントを作成](create-an-odata-v4-endpoint.md)最初。</span><span class="sxs-lookup"><span data-stu-id="c1611-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c1611-111">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="c1611-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c1611-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="c1611-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="c1611-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="c1611-113">OData v4</span></span>


<span data-ttu-id="c1611-114">まず、OData の用語。</span><span class="sxs-lookup"><span data-stu-id="c1611-114">First, some OData terminology:</span></span>

- <span data-ttu-id="c1611-115">エンティティの種類: キーを持つ構造化型。</span><span class="sxs-lookup"><span data-stu-id="c1611-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="c1611-116">複合型: キーのない構造化型。</span><span class="sxs-lookup"><span data-stu-id="c1611-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="c1611-117">オープン型: 動的プロパティを持つ型。</span><span class="sxs-lookup"><span data-stu-id="c1611-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="c1611-118">エンティティ型と複合型の両方を開くことができます。</span><span class="sxs-lookup"><span data-stu-id="c1611-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="c1611-119">動的なプロパティの値は、プリミティブ型、複合型、または列挙型を指定できます。または、それらの型のいずれかのコレクション。</span><span class="sxs-lookup"><span data-stu-id="c1611-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="c1611-120">オープン型の詳細については、次を参照してください。、 [OData v4 仕様](http://www.odata.org/documentation/odata-version-4-0/)します。</span><span class="sxs-lookup"><span data-stu-id="c1611-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="c1611-121">Web の OData ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c1611-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="c1611-122">NuGet パッケージ マネージャーを使用して、最新の Web API OData ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c1611-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="c1611-123">[パッケージ マネージャー コンソール] ウィンドウで。</span><span class="sxs-lookup"><span data-stu-id="c1611-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="c1611-124">CLR 型を定義します。</span><span class="sxs-lookup"><span data-stu-id="c1611-124">Define the CLR Types</span></span>

<span data-ttu-id="c1611-125">CLR 型として EDM モデルを定義することで開始します。</span><span class="sxs-lookup"><span data-stu-id="c1611-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="c1611-126">Entity Data Model (EDM) の作成時に</span><span class="sxs-lookup"><span data-stu-id="c1611-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="c1611-127">`Category` 列挙型です。</span><span class="sxs-lookup"><span data-stu-id="c1611-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="c1611-128">`Address` 複合型。</span><span class="sxs-lookup"><span data-stu-id="c1611-128">`Address` is a complex type.</span></span> <span data-ttu-id="c1611-129">(がない、キー、ため、エンティティ型ではありません。)</span><span class="sxs-lookup"><span data-stu-id="c1611-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="c1611-130">`Customer` エンティティ型。</span><span class="sxs-lookup"><span data-stu-id="c1611-130">`Customer` is an entity type.</span></span> <span data-ttu-id="c1611-131">(キーを持つ)。</span><span class="sxs-lookup"><span data-stu-id="c1611-131">(It has a key.)</span></span>
- <span data-ttu-id="c1611-132">`Press` 開いている複雑な型です。</span><span class="sxs-lookup"><span data-stu-id="c1611-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="c1611-133">`Book` オープンなエンティティ型。</span><span class="sxs-lookup"><span data-stu-id="c1611-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="c1611-134">オープン型を作成するには、CLR 型が型のプロパティをいる必要があります`IDictionary<string, object>`、動的なプロパティが格納されます。</span><span class="sxs-lookup"><span data-stu-id="c1611-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="c1611-135">EDM モデルを構築します。</span><span class="sxs-lookup"><span data-stu-id="c1611-135">Build the EDM Model</span></span>

<span data-ttu-id="c1611-136">使用する場合**ODataConventionModelBuilder** 、EDM を作成する`Press`と`Book`の有無に基づいて、オープン型として自動的に追加されますが、`IDictionary<string, object>`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1611-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="c1611-137">ビルドすることも、EDM に明示的を使用して**に**します。</span><span class="sxs-lookup"><span data-stu-id="c1611-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="c1611-138">OData コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="c1611-138">Add an OData Controller</span></span>

<span data-ttu-id="c1611-139">次に、OData コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="c1611-139">Next, add an OData controller.</span></span> <span data-ttu-id="c1611-140">このチュートリアルでは、だけでサポートを取得および POST 要求、しするエンティティを格納するメモリ内のリストを使用して簡略化されたコント ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c1611-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="c1611-141">いることを確認最初`Book`インスタンスに動的プロパティがありません。</span><span class="sxs-lookup"><span data-stu-id="c1611-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="c1611-142">2 番目の`Book`インスタンスには、次の動的プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1611-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="c1611-143">「発行」: プリミティブ型</span><span class="sxs-lookup"><span data-stu-id="c1611-143">"Published": Primitive type</span></span>
- <span data-ttu-id="c1611-144">"Authors": プリミティブ型のコレクション</span><span class="sxs-lookup"><span data-stu-id="c1611-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="c1611-145">「カテゴリー」: 列挙型のコレクション。</span><span class="sxs-lookup"><span data-stu-id="c1611-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="c1611-146">また、`Press`プロパティを`Book`インスタンスには、次の動的プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1611-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="c1611-147">「ブログ」: プリミティブ型</span><span class="sxs-lookup"><span data-stu-id="c1611-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="c1611-148">"Address": Complex type</span><span class="sxs-lookup"><span data-stu-id="c1611-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="c1611-149">メタデータをクエリします。</span><span class="sxs-lookup"><span data-stu-id="c1611-149">Query the Metadata</span></span>

<span data-ttu-id="c1611-150">OData メタデータ ドキュメントを取得する GET 要求を送信`~/$metadata`します。</span><span class="sxs-lookup"><span data-stu-id="c1611-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="c1611-151">応答本文は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c1611-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="c1611-152">メタデータ ドキュメントをことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="c1611-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="c1611-153">`Book`と`Press`の値の型、`OpenType`属性は true。</span><span class="sxs-lookup"><span data-stu-id="c1611-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="c1611-154">`Customer`と`Address`型は、この属性を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c1611-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="c1611-155">`Book`エンティティ型が宣言された 3 つのプロパティ: ISBN、タイトル、およびキーを押します。</span><span class="sxs-lookup"><span data-stu-id="c1611-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="c1611-156">OData メタデータは含まれません、 `Book.Properties` CLR クラスからプロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1611-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="c1611-157">同様に、`Press`複合型が宣言された 2 つのプロパティ: 名前とカテゴリ。</span><span class="sxs-lookup"><span data-stu-id="c1611-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="c1611-158">メタデータは含まれません、 `Press.DynamicProperties` CLR クラスからプロパティ。</span><span class="sxs-lookup"><span data-stu-id="c1611-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="c1611-159">エンティティを照会します。</span><span class="sxs-lookup"><span data-stu-id="c1611-159">Query an Entity</span></span>

<span data-ttu-id="c1611-160">書籍と ISBN と等しい「978-0-7356-7942-9」を取得するに GET 要求を送信`~/Books('978-0-7356-7942-9')`します。</span><span class="sxs-lookup"><span data-stu-id="c1611-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="c1611-161">応答本文は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c1611-161">The response body should look similar to the following.</span></span> <span data-ttu-id="c1611-162">(読みやすくするためにインデントします。)</span><span class="sxs-lookup"><span data-stu-id="c1611-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="c1611-163">動的プロパティはインラインで宣言されたプロパティであることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="c1611-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="c1611-164">エンティティを投稿します。</span><span class="sxs-lookup"><span data-stu-id="c1611-164">POST an Entity</span></span>

<span data-ttu-id="c1611-165">書籍エンティティを追加するに POST 要求を送信`~/Books`します。</span><span class="sxs-lookup"><span data-stu-id="c1611-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="c1611-166">クライアントは、要求ペイロード内の動的プロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="c1611-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="c1611-167">要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c1611-167">Here is an example request.</span></span> <span data-ttu-id="c1611-168">"Price"と「発行済み」のプロパティに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c1611-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="c1611-169">コント ローラー メソッドにブレークポイントを設定する場合、Web API がこれらのプロパティを追加することを確認できます、`Properties`ディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="c1611-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="c1611-170">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="c1611-170">Additional Resources</span></span>

[<span data-ttu-id="c1611-171">OData のオープン型のサンプル</span><span class="sxs-lookup"><span data-stu-id="c1611-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
