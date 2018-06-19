---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'パート 2: ドメイン モデルの作成 |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878582"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="df20b-102">パート 2: ドメイン モデルの作成</span><span class="sxs-lookup"><span data-stu-id="df20b-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="df20b-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="df20b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="df20b-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="df20b-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="df20b-105">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="df20b-105">Add Models</span></span>

<span data-ttu-id="df20b-106">これには Entity Framework のアプローチを次の 3 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="df20b-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="df20b-107">データベース優先: データベースを起動して、Entity Framework には、コードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="df20b-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="df20b-108">モデル優先: ビジュアル モデルから起動して、Entity Framework では、データベースとコードの両方が生成されます。</span><span class="sxs-lookup"><span data-stu-id="df20b-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="df20b-109">コード優先: コードでは、起動して、Entity Framework には、データベースが生成されます。</span><span class="sxs-lookup"><span data-stu-id="df20b-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="df20b-110">コード優先のアプローチは使用した poco から (従来の CLR オブジェクト) として、ドメイン オブジェクトを定義することで開始するためです。</span><span class="sxs-lookup"><span data-stu-id="df20b-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="df20b-111">コード優先のアプローチでは、ドメイン オブジェクトはトランザクション処理や永続化など、データベース層をサポートするために余分なコードを必要ありません。</span><span class="sxs-lookup"><span data-stu-id="df20b-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="df20b-112">(具体的には、それらから継承する必要はありません、 [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx)クラスです)。データ注釈を使用して、Entity Framework が、データベース スキーマを作成する方法を制御することができますも。</span><span class="sxs-lookup"><span data-stu-id="df20b-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="df20b-113">した poco からに説明する追加のプロパティを使用することはありませんので[データベース状態](https://msdn.microsoft.com/library/system.data.entitystate.aspx)、JSON または XML に簡単にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="df20b-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="df20b-114">ただしをないわけでは常に、クライアントに直接、Entity Framework モデルを公開する必要があります、チュートリアルで後ほどおわかりのようです。</span><span class="sxs-lookup"><span data-stu-id="df20b-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="df20b-115">次のした poco からを作成されます。</span><span class="sxs-lookup"><span data-stu-id="df20b-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="df20b-116">製品</span><span class="sxs-lookup"><span data-stu-id="df20b-116">Product</span></span>
- <span data-ttu-id="df20b-117">順番</span><span class="sxs-lookup"><span data-stu-id="df20b-117">Order</span></span>
- <span data-ttu-id="df20b-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="df20b-118">OrderDetail</span></span>

<span data-ttu-id="df20b-119">各クラスを作成するには、ソリューション エクスプ ローラーで、[モデル] フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="df20b-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="df20b-120">コンテキスト メニューから選択**追加**し、**クラスです。**</span><span class="sxs-lookup"><span data-stu-id="df20b-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="df20b-121">追加、`Product`次の実装を持つクラス。</span><span class="sxs-lookup"><span data-stu-id="df20b-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="df20b-122">慣例により、Entity Framework を使用して、`Id`主キーとしてのプロパティであり、データベース テーブルに id 列にマップします。</span><span class="sxs-lookup"><span data-stu-id="df20b-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="df20b-123">新規に作成するときに`Product`、インスタンスの値を設定しない`Id`データベースには、値がによって生成されるため、します。</span><span class="sxs-lookup"><span data-stu-id="df20b-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="df20b-124">**ScaffoldColumn**属性に指示をスキップする ASP.NET MVC、`Id`プロパティ エディター フォームを生成するときにします。</span><span class="sxs-lookup"><span data-stu-id="df20b-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="df20b-125">**必要**属性は、モデルの検証に使用します。</span><span class="sxs-lookup"><span data-stu-id="df20b-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="df20b-126">指定する、`Name`プロパティが空でない文字列にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="df20b-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="df20b-127">追加、`Order`クラス。</span><span class="sxs-lookup"><span data-stu-id="df20b-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="df20b-128">追加、`OrderDetail`クラス。</span><span class="sxs-lookup"><span data-stu-id="df20b-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="df20b-129">外部キー関係</span><span class="sxs-lookup"><span data-stu-id="df20b-129">Foreign Key Relations</span></span>

<span data-ttu-id="df20b-130">注文には、多くの注文の詳細が含まれています。 し、各注文の詳細は 1 つの製品を参照します。</span><span class="sxs-lookup"><span data-stu-id="df20b-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="df20b-131">これらのリレーションを表す、`OrderDetail`クラスという名前のプロパティを定義する`OrderId`と`ProductId`です。</span><span class="sxs-lookup"><span data-stu-id="df20b-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="df20b-132">これらのプロパティが外部キーを表すし、データベースに foreign key 制約を追加、entity Framework は推論されます。</span><span class="sxs-lookup"><span data-stu-id="df20b-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="df20b-133">`Order`と`OrderDetail`クラスも含まれて「ナビゲーション」のプロパティは、関連するオブジェクトへの参照が含まれています。</span><span class="sxs-lookup"><span data-stu-id="df20b-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="df20b-134">順序を指定するに移動できます順序では、製品によって次のナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="df20b-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="df20b-135">これで、プロジェクトをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="df20b-135">Compile the project now.</span></span> <span data-ttu-id="df20b-136">Entity Framework では、リフレクションを使用して、コンパイルされたアセンブリをデータベース スキーマを作成する必要がありますので、モデルのプロパティを検出します。</span><span class="sxs-lookup"><span data-stu-id="df20b-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="df20b-137">メディア タイプ フォーマッタを構成します。</span><span class="sxs-lookup"><span data-stu-id="df20b-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="df20b-138">A[メディア タイプ フォーマッタ](../../formats-and-model-binding/media-formatters.md)Web API は、HTTP 応答の本文を書き込むときに、データをシリアル化するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="df20b-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="df20b-139">組み込みのフォーマッタは、JSON と XML の出力をサポートします。</span><span class="sxs-lookup"><span data-stu-id="df20b-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="df20b-140">既定では、これらフォーマッタの両方をシリアル化のすべてのオブジェクト値。</span><span class="sxs-lookup"><span data-stu-id="df20b-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="df20b-141">値によるシリアル化は、オブジェクト グラフに循環参照が含まれている場合、問題を作成します。</span><span class="sxs-lookup"><span data-stu-id="df20b-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="df20b-142">場合と正確には、`Order`と`OrderDetail`クラス、それぞれに他方への参照が格納されているためです。</span><span class="sxs-lookup"><span data-stu-id="df20b-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="df20b-143">フォーマッタでは、次の参照、値によって、各オブジェクトの作成し、堂々巡りに移動します。</span><span class="sxs-lookup"><span data-stu-id="df20b-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="df20b-144">したがって、既定の動作を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="df20b-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="df20b-145">ソリューション エクスプ ローラーで、アプリを展開して\_フォルダーを起動し、WebApiConfig.cs をという名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="df20b-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="df20b-146">`WebApiConfig` クラスに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="df20b-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="df20b-147">このコードでは、オブジェクト参照を保持するために JSON フォーマッタを設定し、パイプラインから完全 XML フォーマッタを削除します。</span><span class="sxs-lookup"><span data-stu-id="df20b-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="df20b-148">(オブジェクト参照を保持する XML フォーマッタを構成することができますが、少しの作業は、このアプリケーションの JSON のみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="df20b-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="df20b-149">詳細については、次を参照してください[循環オブジェクト参照の処理](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。)。</span><span class="sxs-lookup"><span data-stu-id="df20b-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df20b-150">[前へ](using-web-api-with-entity-framework-part-1.md)
> [次へ](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="df20b-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
