---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET Web API を使用した OData v4 の複合型の継承 |Microsoft Docs
author: microsoft
description: OData v4 仕様では、複合型は、別の複合型から継承できます。 (複合型は、キーのない構造化型です)。Web API.
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 8dbf7dc4cfc70e1ea1ed6f72ffc0751a56809c09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823805"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="905b9-104">ASP.NET Web API を使用した OData v4 の複合型の継承</span><span class="sxs-lookup"><span data-stu-id="905b9-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="905b9-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="905b9-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="905b9-106">OData v4 に従って[仕様](http://www.odata.org/documentation/odata-version-4-0/)、複合型は、別の複合型から継承できます。</span><span class="sxs-lookup"><span data-stu-id="905b9-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="905b9-107">(A*複雑な*型はキーのない構造化された型です)。Web API OData 5.3 は、複合型の継承をサポートします。</span><span class="sxs-lookup"><span data-stu-id="905b9-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="905b9-108">このトピックでは、複雑な継承の種類で entity data model (EDM) を構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="905b9-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="905b9-109">完全なソース コードでは、次を参照してください。 [OData の複合型継承サンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)します。</span><span class="sxs-lookup"><span data-stu-id="905b9-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="905b9-110">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="905b9-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="905b9-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="905b9-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="905b9-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="905b9-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="905b9-113">モデル階層</span><span class="sxs-lookup"><span data-stu-id="905b9-113">Model Hierarchy</span></span>

<span data-ttu-id="905b9-114">複合型の継承を示すためには、次のクラス階層を使用します。</span><span class="sxs-lookup"><span data-stu-id="905b9-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="905b9-115">`Shape` 抽象複合型です。</span><span class="sxs-lookup"><span data-stu-id="905b9-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="905b9-116">`Rectangle`、 `Triangle`、および`Circle`複合型から派生`Shape`、および`RoundRectangle`から派生した`Rectangle`します。</span><span class="sxs-lookup"><span data-stu-id="905b9-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="905b9-117">`Window` エンティティ型は、含む、`Shape`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="905b9-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="905b9-118">これらの型を定義する CLR クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="905b9-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="905b9-119">EDM モデルを構築します。</span><span class="sxs-lookup"><span data-stu-id="905b9-119">Build the EDM Model</span></span>

<span data-ttu-id="905b9-120">使用することができます、EDM を作成する**ODataConventionModelBuilder**CLR 型からの継承関係を推論します。</span><span class="sxs-lookup"><span data-stu-id="905b9-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="905b9-121">ビルドすることも、EDM に明示的を使用して**に**します。</span><span class="sxs-lookup"><span data-stu-id="905b9-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="905b9-122">これにより、多くのコードを受け取るが、EDM より詳細に制御できます。</span><span class="sxs-lookup"><span data-stu-id="905b9-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="905b9-123">これら 2 つの例では、同じ EDM スキーマを作成します。</span><span class="sxs-lookup"><span data-stu-id="905b9-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="905b9-124">メタデータ ドキュメント</span><span class="sxs-lookup"><span data-stu-id="905b9-124">Metadata Document</span></span>

<span data-ttu-id="905b9-125">OData の複合型の継承を示すメタデータ ドキュメントを次に示します。</span><span class="sxs-lookup"><span data-stu-id="905b9-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="905b9-126">メタデータ ドキュメントをことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="905b9-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="905b9-127">`Shape`複雑な型が抽象型。</span><span class="sxs-lookup"><span data-stu-id="905b9-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="905b9-128">`Rectangle`、 `Triangle`、および`Circle`複合型が基本型である`Shape`します。</span><span class="sxs-lookup"><span data-stu-id="905b9-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="905b9-129">`RoundRectangle`型が基本型`Rectangle`します。</span><span class="sxs-lookup"><span data-stu-id="905b9-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="905b9-130">複合型のキャスト</span><span class="sxs-lookup"><span data-stu-id="905b9-130">Casting Complex Types</span></span>

<span data-ttu-id="905b9-131">複合型のキャストはサポートされています。</span><span class="sxs-lookup"><span data-stu-id="905b9-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="905b9-132">たとえば、次のクエリのキャスト、`Shape`を`Rectangle`します。</span><span class="sxs-lookup"><span data-stu-id="905b9-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="905b9-133">応答ペイロードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="905b9-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
