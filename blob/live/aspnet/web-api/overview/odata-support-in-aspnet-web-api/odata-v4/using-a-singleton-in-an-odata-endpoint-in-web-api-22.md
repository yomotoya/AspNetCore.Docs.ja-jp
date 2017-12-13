---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "Web API 2.2 を使用して OData v4 のシングルトンを作成 |Microsoft ドキュメント"
author: rick-anderson
description: "このトピックでは、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="a32a7-103">Web API 2.2 を使用して OData v4 のシングルトンを作成します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="a32a7-104">ゾーイ Luo によって</span><span class="sxs-lookup"><span data-stu-id="a32a7-104">by Zoe Luo</span></span>

> <span data-ttu-id="a32a7-105">従来、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセス可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a32a7-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="a32a7-106">OData v4 がシングルトンや含有関係、WebAPI 2.2 をどちらもサポートしている 2 つの追加オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="a32a7-107">この記事では、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="a32a7-108">どのようなシングルトンし、を使用する利点については、次を参照してください。[シングルトンを使用して、特殊なエンティティを定義する](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="a32a7-109">Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用する ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="a32a7-110">次のデータ モデルを使用して、Web API プロジェクトのシングルトンを作成します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![データ モデル](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="a32a7-112">という名前の単一`Umbrella`が定義される種類に基づいて`Company`、および名前付きセットのエンティティ`Employees`の種類に基づいて定義されます`Employee`です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="a32a7-113">このチュートリアルで使用されるソリューションからダウンロードできます。 [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="a32a7-114">データ モデルを定義する</span><span class="sxs-lookup"><span data-stu-id="a32a7-114">Define the data model</span></span>

1. <span data-ttu-id="a32a7-115">CLR 型を定義します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="a32a7-116">CLR 型に基づき、EDM モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="a32a7-117">ここでは、`builder.Singleton<Company>("Umbrella")`シングルトンという名前を作成するモデル ビルダーに指示`Umbrella`EDM モデル。</span><span class="sxs-lookup"><span data-stu-id="a32a7-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="a32a7-118">生成されたメタデータは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a32a7-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="a32a7-119">メタデータからことが分かりますナビゲーション プロパティ`Company`で、`Employees`エンティティ セットは、シングルトンにバインドされて`Umbrella`です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="a32a7-120">バインディングはによって自動的に行われます`ODataConventionModelBuilder`、のでのみ`Umbrella`が、`Company`型です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="a32a7-121">使用することができます、モデルにあいまいさがある場合`HasSingletonBinding`; シングルトンにナビゲーション プロパティを明示的にバインドするには`HasSingletonBinding`を使用すると同じ効果を持つ、 `Singleton` CLR 型の定義内の属性。</span><span class="sxs-lookup"><span data-stu-id="a32a7-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="a32a7-122">単一のコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-122">Define the singleton controller</span></span>

<span data-ttu-id="a32a7-123">EntitySet コント ローラーと同様に、単一コント ローラーがから継承`ODataController`、単一コント ローラー名を指定する必要があります`[singletonName]Controller`です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="a32a7-124">さまざまな種類の要求を処理するために、コント ローラーで事前に定義するアクションが必要です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="a32a7-125">**属性のルーティング**WebApi 2.2 で既定で有効にします。</span><span class="sxs-lookup"><span data-stu-id="a32a7-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="a32a7-126">たとえば、クエリを処理するアクションを定義する`Revenue`から`Company`属性がルーティングを使用して、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="a32a7-127">各アクションの属性を定義する場合は、次の操作を定義するだけ[OData ルーティング規約](../odata-routing-conventions.md)です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="a32a7-128">キーがシングルトン クエリを実行するために必要ないため、単一コント ローラーで定義されたアクションは entityset コント ローラーで定義されたアクションとは若干異なります。</span><span class="sxs-lookup"><span data-stu-id="a32a7-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="a32a7-129">リファレンスについては、単一コント ローラー アクション定義のそれぞれのメソッドのシグネチャは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="a32a7-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="a32a7-130">基本的には、これは、サービス側で実行する必要がありますすべてです。</span><span class="sxs-lookup"><span data-stu-id="a32a7-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="a32a7-131">[サンプル プロジェクト](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)のすべてのソリューションと、シングルトンを使用する方法を示す OData クライアント コードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a32a7-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="a32a7-132">次の手順に従って、クライアントが組み込まれて[OData v4 クライアント アプリを作成する](create-an-odata-v4-client-app.md)です。</span><span class="sxs-lookup"><span data-stu-id="a32a7-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="a32a7-133">。</span><span class="sxs-lookup"><span data-stu-id="a32a7-133">.</span></span> 

<span data-ttu-id="a32a7-134">*この記事の元のコンテンツ Leo %hu に感謝します。*</span><span class="sxs-lookup"><span data-stu-id="a32a7-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
