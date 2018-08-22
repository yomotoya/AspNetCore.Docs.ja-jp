---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Web API 2.2 を使用して OData v4 のコンテインメイト |Microsoft Docs
author: rick-anderson
description: これまでは、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセスできます。 OData v4 でシングルトンおよび Con 2 つの追加オプションを提供しています.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: aca263a04df25ca241bc0b9798b3a0b588d4cae8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826343"
---
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="27fe1-104">Web API 2.2 を使用して OData v4 のコンテインメイト</span><span class="sxs-lookup"><span data-stu-id="27fe1-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="27fe1-105">によって Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="27fe1-105">by Jinfu Tan</span></span>

> <span data-ttu-id="27fe1-106">これまでは、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="27fe1-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="27fe1-107">OData v4、シングルトンおよびコンテインメント、web Api 2.2 をサポートする 2 つの追加オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="27fe1-108">このトピックでは、web Api 2.2 で OData エンドポイントでコンテインメントを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="27fe1-109">コンテインメントの詳細については、次を参照してください。 [OData v4 で予定されている包含](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="27fe1-110">Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用して ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="27fe1-111">最初に、作成しますコンテインメントのドメイン モデル、OData サービスでこのデータ モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![データ モデル](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="27fe1-113">アカウントには多く PaymentInstruments (PI) が含まれていますが、エンティティ、PI のセットを定義していません。</span><span class="sxs-lookup"><span data-stu-id="27fe1-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="27fe1-114">代わりに、Pi は、アカウントを介してのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="27fe1-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="27fe1-115">このトピックで使用するソリューションをダウンロードする[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="27fe1-116">データ モデルを定義します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-116">Defining the data model</span></span>

1. <span data-ttu-id="27fe1-117">CLR 型を定義します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="27fe1-118">`Contained`コンテインメント ナビゲーション プロパティに属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="27fe1-119">CLR 型に基づく EDM モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="27fe1-120">`ODataConventionModelBuilder` EDM モデルを構築する場合を処理する、`Contained`属性は、対応するナビゲーション プロパティに追加されます。</span><span class="sxs-lookup"><span data-stu-id="27fe1-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="27fe1-121">プロパティがコレクション型の場合、`GetCount(string NameContains)`関数も作成されます。</span><span class="sxs-lookup"><span data-stu-id="27fe1-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="27fe1-122">生成されるメタデータは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="27fe1-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="27fe1-123">`ContainsTarget`属性は、ナビゲーション プロパティは、包含であることを示します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="27fe1-124">親エンティティ セットのコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-124">Define the containing entity set controller</span></span>

<span data-ttu-id="27fe1-125">含まれているエンティティは、独自のコント ローラーは必要はありません。アクションは、親エンティティ セットのコント ローラーで定義されます。</span><span class="sxs-lookup"><span data-stu-id="27fe1-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="27fe1-126">このサンプルでは、AccountsController が PaymentInstrumentsController しません。</span><span class="sxs-lookup"><span data-stu-id="27fe1-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="27fe1-127">OData パスが 4 つ以上のセグメントである場合は、専用の属性がルーティングのしくみなど`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上記のコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="27fe1-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="27fe1-128">それ以外の場合、属性と従来のルーティングの動作: たとえば、`GetPayInPIs(int key)`と一致する`GET ~/Accounts(1)/PayinPIs`します。</span><span class="sxs-lookup"><span data-stu-id="27fe1-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="27fe1-129">*Leo Hu に改訂された記事については、元のコンテンツいただきありがとうございます。*</span><span class="sxs-lookup"><span data-stu-id="27fe1-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
