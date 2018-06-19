---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Web API 2.2 を使用して OData v4 でコンテインメント |Microsoft ドキュメント
author: rick-anderson
description: 従来、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセス可能性があります。 OData v4 には、単一予測クエリおよび Con 2 つの追加オプションを提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508001"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="be52c-104">Web API 2.2 を使用して OData v4 の包含</span><span class="sxs-lookup"><span data-stu-id="be52c-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="be52c-105">Jinfu tan をクリックして</span><span class="sxs-lookup"><span data-stu-id="be52c-105">by Jinfu Tan</span></span>

> <span data-ttu-id="be52c-106">従来、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセス可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be52c-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="be52c-107">OData v4 がシングルトンや含有関係、WebAPI 2.2 をどちらもサポートしている 2 つの追加オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="be52c-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="be52c-108">このトピックでは、WebApi 2.2 で OData エンドポイントでコンテインメントを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="be52c-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="be52c-109">コンテインメントの詳細については、次を参照してください。 [OData v4 に今後予定されているコンテインメント](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="be52c-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="be52c-110">Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用する ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)です。</span><span class="sxs-lookup"><span data-stu-id="be52c-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="be52c-111">最初は、モデルを作成コンテインメント ドメイン OData サービスでこのデータ モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="be52c-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![データ モデル](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="be52c-113">アカウントには、多く PaymentInstruments (PI) が含まれていますが、エンティティ PI のセットを定義していません。</span><span class="sxs-lookup"><span data-stu-id="be52c-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="be52c-114">代わりに、Pi は、アカウントを介してのみアクセスできることができます。</span><span class="sxs-lookup"><span data-stu-id="be52c-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="be52c-115">このトピックで使用されるソリューションをダウンロードする[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)です。</span><span class="sxs-lookup"><span data-stu-id="be52c-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="be52c-116">データ モデルを定義します。</span><span class="sxs-lookup"><span data-stu-id="be52c-116">Defining the data model</span></span>

1. <span data-ttu-id="be52c-117">CLR 型を定義します。</span><span class="sxs-lookup"><span data-stu-id="be52c-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="be52c-118">`Contained`コンテインメント ナビゲーション プロパティの属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="be52c-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="be52c-119">CLR 型に基づき、EDM モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="be52c-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="be52c-120">`ODataConventionModelBuilder` EDM モデルを構築する場合を処理する、`Contained`属性が、対応するナビゲーション プロパティに追加します。</span><span class="sxs-lookup"><span data-stu-id="be52c-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="be52c-121">プロパティがコレクション型の場合、`GetCount(string NameContains)`関数も作成されます。</span><span class="sxs-lookup"><span data-stu-id="be52c-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="be52c-122">生成されたメタデータは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="be52c-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="be52c-123">`ContainsTarget`属性の場合、ナビゲーション プロパティが、包含であることを示します。</span><span class="sxs-lookup"><span data-stu-id="be52c-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="be52c-124">含むエンティティ セットのコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="be52c-124">Define the containing entity set controller</span></span>

<span data-ttu-id="be52c-125">含まれているエンティティは、独自のコント ローラー; がありません。アクションが含まれるエンティティ セットのコント ローラーで定義されます。</span><span class="sxs-lookup"><span data-stu-id="be52c-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="be52c-126">このサンプルでは、AccountsController がない PaymentInstrumentsController です。</span><span class="sxs-lookup"><span data-stu-id="be52c-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="be52c-127">OData パスが 4 つ以上のセグメントである場合は、専用の属性がルーティングのしくみなど`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上のコント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="be52c-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="be52c-128">それ以外の場合、属性と従来のルーティングの両方の動作: たとえば、`GetPayInPIs(int key)`と一致する`GET ~/Accounts(1)/PayinPIs`です。</span><span class="sxs-lookup"><span data-stu-id="be52c-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="be52c-129">*この記事の元のコンテンツ Leo %hu に感謝します。*</span><span class="sxs-lookup"><span data-stu-id="be52c-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
