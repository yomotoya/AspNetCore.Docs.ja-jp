---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'パート 2: ドメイン モデルの作成 |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: cb98f42df411a7ba12ff4566c30ddfbf253253d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835306"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="552a3-102">パート 2: ドメイン モデルの作成</span><span class="sxs-lookup"><span data-stu-id="552a3-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="552a3-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="552a3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="552a3-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="552a3-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="552a3-105">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="552a3-105">Add Models</span></span>

<span data-ttu-id="552a3-106">Entity Framework のアプローチに 3 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="552a3-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="552a3-107">データベース ファースト: データベースで開始して、Entity Framework には、コードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="552a3-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="552a3-108">モデル ファースト: ビジュアルのモデルから開始して、Entity Framework には、データベースとコードの両方が生成されます。</span><span class="sxs-lookup"><span data-stu-id="552a3-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="552a3-109">Code first: コードを開始して、Entity Framework には、データベースが生成されます。</span><span class="sxs-lookup"><span data-stu-id="552a3-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="552a3-110">Poco (plain-old CLR object) としてのドメイン オブジェクトを定義することで開始 code first アプローチを使用しています。</span><span class="sxs-lookup"><span data-stu-id="552a3-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="552a3-111">Code first アプローチでは、ドメイン オブジェクトは、トランザクションや永続化など、データベース層をサポートするために余分なコードを必要はありません。</span><span class="sxs-lookup"><span data-stu-id="552a3-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="552a3-112">(具体的には、それらから継承する必要はありません、 [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx)クラスです)。Entity Framework がデータベース スキーマを作成する方法を制御するのに、データ注釈を引き続き使用できます。</span><span class="sxs-lookup"><span data-stu-id="552a3-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="552a3-113">Poco を記述する追加のプロパティを搬送しないため、[データベース状態](https://msdn.microsoft.com/library/system.data.entitystate.aspx)、JSON または XML にシリアルできます化簡単にします。</span><span class="sxs-lookup"><span data-stu-id="552a3-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="552a3-114">ただし、わけには、クライアントでは、直接、Entity Framework モデルを常に公開する必要があります、チュートリアルの後半で表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="552a3-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="552a3-115">次の Poco を作成します。</span><span class="sxs-lookup"><span data-stu-id="552a3-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="552a3-116">製品</span><span class="sxs-lookup"><span data-stu-id="552a3-116">Product</span></span>
- <span data-ttu-id="552a3-117">順番</span><span class="sxs-lookup"><span data-stu-id="552a3-117">Order</span></span>
- <span data-ttu-id="552a3-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="552a3-118">OrderDetail</span></span>

<span data-ttu-id="552a3-119">各クラスを作成するには、ソリューション エクスプ ローラーで Models フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="552a3-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="552a3-120">コンテキスト メニューでは、次のように選択します。**追加**選び**クラス。**</span><span class="sxs-lookup"><span data-stu-id="552a3-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="552a3-121">追加、`Product`次の実装クラス。</span><span class="sxs-lookup"><span data-stu-id="552a3-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="552a3-122">慣例により、Entity Framework を使用して、`Id`プロパティを主キーとして、データベース テーブルに id 列にマップします。</span><span class="sxs-lookup"><span data-stu-id="552a3-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="552a3-123">新規に作成するときに`Product`インスタンスの値を設定しません`Id`データベースの値を生成するためです。</span><span class="sxs-lookup"><span data-stu-id="552a3-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="552a3-124">**ScaffoldColumn**属性に指示をスキップする ASP.NET MVC、`Id`プロパティ エディターのフォームを生成するときにします。</span><span class="sxs-lookup"><span data-stu-id="552a3-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="552a3-125">**必要**属性は、モデルの検証に使用します。</span><span class="sxs-lookup"><span data-stu-id="552a3-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="552a3-126">指定します、`Name`プロパティは空でない文字列である必要があります。</span><span class="sxs-lookup"><span data-stu-id="552a3-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="552a3-127">追加、`Order`クラス。</span><span class="sxs-lookup"><span data-stu-id="552a3-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="552a3-128">追加、`OrderDetail`クラス。</span><span class="sxs-lookup"><span data-stu-id="552a3-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="552a3-129">外部キーの関係</span><span class="sxs-lookup"><span data-stu-id="552a3-129">Foreign Key Relations</span></span>

<span data-ttu-id="552a3-130">注文は多くの注文の詳細を含み、各注文の詳細は、1 つの製品を参照します。</span><span class="sxs-lookup"><span data-stu-id="552a3-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="552a3-131">これらのリレーションを表す、`OrderDetail`クラスという名前のプロパティを定義する`OrderId`と`ProductId`します。</span><span class="sxs-lookup"><span data-stu-id="552a3-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="552a3-132">これらのプロパティが外部キーを表すし、データベースに foreign key 制約を追加、entity Framework は推論します。</span><span class="sxs-lookup"><span data-stu-id="552a3-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="552a3-133">`Order`と`OrderDetail`クラスでは、関連オブジェクトへの参照を含む「ナビゲーション」プロパティも含まれます。</span><span class="sxs-lookup"><span data-stu-id="552a3-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="552a3-134">順序を指定すると、ナビゲーション プロパティを次の順序で製品に移動できます。</span><span class="sxs-lookup"><span data-stu-id="552a3-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="552a3-135">これで、プロジェクトをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="552a3-135">Compile the project now.</span></span> <span data-ttu-id="552a3-136">Entity Framework では、リフレクションを使用して、コンパイル済みのアセンブリ、データベース スキーマを作成する必要がありますので、モデルのプロパティを検出します。</span><span class="sxs-lookup"><span data-stu-id="552a3-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="552a3-137">メディア タイプ フォーマッタを構成します。</span><span class="sxs-lookup"><span data-stu-id="552a3-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="552a3-138">A[メディア タイプ フォーマッタ](../../formats-and-model-binding/media-formatters.md)は Web API は HTTP 応答の本文を書き込むときに、データをシリアル化するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="552a3-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="552a3-139">組み込みのフォーマッタは、JSON および XML の出力をサポートします。</span><span class="sxs-lookup"><span data-stu-id="552a3-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="552a3-140">既定で、すべてのオブジェクトを値別これらフォーマッタのどちらもシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="552a3-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="552a3-141">値によるシリアル化は、オブジェクト グラフに循環参照が含まれている場合、問題を作成します。</span><span class="sxs-lookup"><span data-stu-id="552a3-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="552a3-142">これは、場合で正確には、`Order`と`OrderDetail`クラスをそれぞれに、もう一方への参照が格納されているためです。</span><span class="sxs-lookup"><span data-stu-id="552a3-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="552a3-143">フォーマッタは以下の参照、値によって各オブジェクトを記述し、円で移動します。</span><span class="sxs-lookup"><span data-stu-id="552a3-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="552a3-144">そのため、既定の動作を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="552a3-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="552a3-145">ソリューション エクスプ ローラーでアプリケーションを拡張して\_フォルダーを起動し、WebApiConfig.cs という名前のファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="552a3-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="552a3-146">`WebApiConfig` クラスに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="552a3-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="552a3-147">このコードでは、オブジェクトの参照を保持するために、JSON フォーマッタを設定し、パイプラインから完全 XML フォーマッタを削除します。</span><span class="sxs-lookup"><span data-stu-id="552a3-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="552a3-148">(オブジェクトの参照を保持するために、XML フォーマッタを構成することができますが、もう少しの作業が、このアプリケーションの JSON のみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="552a3-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="552a3-149">詳細については、次を参照してください[循環オブジェクト参照の処理](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。)。</span><span class="sxs-lookup"><span data-stu-id="552a3-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="552a3-150">[前へ](using-web-api-with-entity-framework-part-1.md)
> [次へ](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="552a3-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
