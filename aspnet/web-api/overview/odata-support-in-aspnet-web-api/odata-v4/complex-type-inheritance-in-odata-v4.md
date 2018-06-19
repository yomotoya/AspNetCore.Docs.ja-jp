---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET web API OData v4 の複合型の継承 |Microsoft ドキュメント
author: microsoft
description: OData v4 仕様では、複合型を別の複合型から継承できます。 (複合型は、キーがない場合は構造化型です)。Web API しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508421"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="19864-104">ASP.NET web API OData v4 の複合型の継承</span><span class="sxs-lookup"><span data-stu-id="19864-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="19864-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="19864-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="19864-106">OData v4 に従って[仕様](http://www.odata.org/documentation/odata-version-4-0/)、複合型が別の複合型から継承できます。</span><span class="sxs-lookup"><span data-stu-id="19864-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="19864-107">(A*複雑な*型は、キーのない構造化された型です)。Web API OData 5.3 は、複合型の継承をサポートします。</span><span class="sxs-lookup"><span data-stu-id="19864-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="19864-108">このトピックでは、複雑な継承の種類で entity data model (EDM) をビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="19864-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="19864-109">完全なソース コードでは、次を参照してください。[複合型の継承のサンプルの OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)です。</span><span class="sxs-lookup"><span data-stu-id="19864-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="19864-110">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="19864-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="19864-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="19864-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="19864-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="19864-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="19864-113">モデル階層</span><span class="sxs-lookup"><span data-stu-id="19864-113">Model Hierarchy</span></span>

<span data-ttu-id="19864-114">複合型の継承を示すためには、次のクラス階層を使用します。</span><span class="sxs-lookup"><span data-stu-id="19864-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="19864-115">`Shape`抽象の複合型。</span><span class="sxs-lookup"><span data-stu-id="19864-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="19864-116">`Rectangle`、 `Triangle`、および`Circle`複合型から派生`Shape`、および`RoundRectangle`から派生した`Rectangle`です。</span><span class="sxs-lookup"><span data-stu-id="19864-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="19864-117">`Window`エンティティ型は、含む、`Shape`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="19864-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="19864-118">ここでは、これらの型を定義する CLR クラスです。</span><span class="sxs-lookup"><span data-stu-id="19864-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="19864-119">EDM モデルを構築します。</span><span class="sxs-lookup"><span data-stu-id="19864-119">Build the EDM Model</span></span>

<span data-ttu-id="19864-120">使用することができます、EDM を作成する**ODataConventionModelBuilder**CLR 型から継承関係を推測します。</span><span class="sxs-lookup"><span data-stu-id="19864-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="19864-121">ビルドすることも、EDM、明示的に使用する**ため**です。</span><span class="sxs-lookup"><span data-stu-id="19864-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="19864-122">これにより、多くのコードを受け取りますが、EDM より詳細に制御できます。</span><span class="sxs-lookup"><span data-stu-id="19864-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="19864-123">これら 2 つの例では、同じ EDM スキーマを作成します。</span><span class="sxs-lookup"><span data-stu-id="19864-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="19864-124">メタデータ ドキュメント</span><span class="sxs-lookup"><span data-stu-id="19864-124">Metadata Document</span></span>

<span data-ttu-id="19864-125">ここでは、OData メタデータ ドキュメント、複合型の継承を示すです。</span><span class="sxs-lookup"><span data-stu-id="19864-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="19864-126">メタデータ ドキュメントからは、ことを確認できます。</span><span class="sxs-lookup"><span data-stu-id="19864-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="19864-127">`Shape`複合型は抽象クラスです。</span><span class="sxs-lookup"><span data-stu-id="19864-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="19864-128">`Rectangle`、 `Triangle`、および`Circle`複合型が基本型を含める`Shape`です。</span><span class="sxs-lookup"><span data-stu-id="19864-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="19864-129">`RoundRectangle`型に基本型が、`Rectangle`です。</span><span class="sxs-lookup"><span data-stu-id="19864-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="19864-130">複合型のキャスト</span><span class="sxs-lookup"><span data-stu-id="19864-130">Casting Complex Types</span></span>

<span data-ttu-id="19864-131">複合型のキャストはサポートされています。</span><span class="sxs-lookup"><span data-stu-id="19864-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="19864-132">たとえば、次のクエリのキャスト、`Shape`を`Rectangle`です。</span><span class="sxs-lookup"><span data-stu-id="19864-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="19864-133">応答ペイロードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="19864-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
