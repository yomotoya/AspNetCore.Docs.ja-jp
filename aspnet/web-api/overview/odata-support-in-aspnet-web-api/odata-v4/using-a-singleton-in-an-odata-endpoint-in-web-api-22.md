---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Web API 2.2 を使用して OData v4 でシングルトンを作成する |Microsoft Docs
author: rick-anderson
description: このトピックでは、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7562a90ae34b216dca2dd3cf541d086585735212
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833306"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="bb225-103">Web API 2.2 を使用して OData v4 でシングルトンを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb225-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="bb225-104">Zoe ルオ語で</span><span class="sxs-lookup"><span data-stu-id="bb225-104">by Zoe Luo</span></span>

> <span data-ttu-id="bb225-105">これまでは、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="bb225-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="bb225-106">OData v4、シングルトンおよびコンテインメント、web Api 2.2 をサポートする 2 つの追加オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="bb225-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="bb225-107">この記事では、Web API 2.2 で OData エンドポイントでシングルトンを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bb225-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="bb225-108">どのようなシングルトン、およびこれを使用する利点については、次を参照してください。[シングルトンを使用して、特別なエンティティを定義する](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="bb225-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="bb225-109">Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用して ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)します。</span><span class="sxs-lookup"><span data-stu-id="bb225-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="bb225-110">次のデータ モデルを使用して Web API プロジェクトでシングルトンを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb225-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![データ モデル](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="bb225-112">という名前の単一`Umbrella`の種類に基づいて定義されます`Company`、およびエンティティ セットの名前付き`Employees`の種類に基づいて定義されます`Employee`します。</span><span class="sxs-lookup"><span data-stu-id="bb225-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="bb225-113">このチュートリアルで使用するソリューションをからダウンロードできます[CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)します。</span><span class="sxs-lookup"><span data-stu-id="bb225-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="bb225-114">データ モデルを定義する</span><span class="sxs-lookup"><span data-stu-id="bb225-114">Define the data model</span></span>

1. <span data-ttu-id="bb225-115">CLR 型を定義します。</span><span class="sxs-lookup"><span data-stu-id="bb225-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="bb225-116">CLR 型に基づく EDM モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="bb225-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="bb225-117">ここでは、`builder.Singleton<Company>("Umbrella")`という名前のシングルトンを作成するモデル ビルダーに指示`Umbrella`EDM モデル。</span><span class="sxs-lookup"><span data-stu-id="bb225-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="bb225-118">生成されるメタデータは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="bb225-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="bb225-119">メタデータから確認できます、ナビゲーション プロパティ`Company`で、`Employees`エンティティ セットは、シングルトンにバインドされて`Umbrella`します。</span><span class="sxs-lookup"><span data-stu-id="bb225-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="bb225-120">バインディングはによって自動的に行われます`ODataConventionModelBuilder`、のでのみ`Umbrella`が、`Company`型。</span><span class="sxs-lookup"><span data-stu-id="bb225-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="bb225-121">使用することができます、モデル内であいまいさがある場合`HasSingletonBinding`シングルトン; にナビゲーション プロパティを明示的にバインドするには`HasSingletonBinding`を使用して同じ効果があります、 `Singleton` CLR 型の定義内の属性。</span><span class="sxs-lookup"><span data-stu-id="bb225-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="bb225-122">単一のコント ローラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="bb225-122">Define the singleton controller</span></span>

<span data-ttu-id="bb225-123">単一のコント ローラーが継承、EntitySet のコント ローラーのような`ODataController`、単一のコント ローラー名を指定する必要があります`[singletonName]Controller`します。</span><span class="sxs-lookup"><span data-stu-id="bb225-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="bb225-124">さまざまな種類の要求を処理するためには、アクションをコント ローラーに事前定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb225-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="bb225-125">**属性ルーティング**web Api 2.2 で既定で有効です。</span><span class="sxs-lookup"><span data-stu-id="bb225-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="bb225-126">たとえば、処理クエリを実行するアクションを定義する`Revenue`から`Company`属性ルーティングを使用して、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="bb225-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="bb225-127">各アクションの属性を定義する場合は、次の操作の定義だけ[OData ルーティング規約](../odata-routing-conventions.md)します。</span><span class="sxs-lookup"><span data-stu-id="bb225-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="bb225-128">キーが、単一のクエリを実行するために必要ないため、単一のコント ローラーで定義されたアクションは entityset コント ローラーで定義されたアクションとは若干異なります。</span><span class="sxs-lookup"><span data-stu-id="bb225-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="bb225-129">リファレンスについては、単一のコント ローラーのすべてのアクション定義のメソッド シグネチャは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="bb225-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="bb225-130">基本的に、これはサービス側で行う必要があるすべてです。</span><span class="sxs-lookup"><span data-stu-id="bb225-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="bb225-131">[サンプル プロジェクト](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)すべてのソリューションおよびシングルトンを使用する方法を示す OData クライアント コードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bb225-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="bb225-132">クライアントは次の手順に従って構築[OData v4 クライアント アプリを作成する](create-an-odata-v4-client-app.md)します。</span><span class="sxs-lookup"><span data-stu-id="bb225-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="bb225-133">.</span><span class="sxs-lookup"><span data-stu-id="bb225-133">.</span></span> 

<span data-ttu-id="bb225-134">*Leo Hu に改訂された記事については、元のコンテンツいただきありがとうございます。*</span><span class="sxs-lookup"><span data-stu-id="bb225-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
