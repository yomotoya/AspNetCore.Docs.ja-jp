---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: "ASP.NET Web API 2.2 を使用して OData v4 でのエンティティ関係 |Microsoft ドキュメント"
author: MikeWasson
description: "ほとんどのデータ セットは、エンティティ間のリレーションを定義します。 お客様が設定されている順序です。ブックがある作成者です。製品では、仕入先があります。 OData を使用すると、クライアントは、経由で移動することができます."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>ASP.NET Web API 2.2 を使用して OData v4 でのエンティティ関係
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> ほとんどのデータ セットは、エンティティ間のリレーションを定義します。 お客様が設定されている順序です。ブックがある作成者です。製品では、仕入先があります。 クライアントは OData を使用すると、エンティティ関係経由で移動できます。 製品を指定するには、供給業者を検索できます。 作成または、リレーションシップを削除することができますも。 たとえば、製品の仕入先を設定できます。
> 
> このチュートリアルでは、ASP.NET Web API を使用しての OData v4 でこれらの操作をサポートする方法を示します。 このチュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用する ASP.NET Web API 2 を作成する](create-an-odata-v4-endpoint.md)です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API 2.1
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
> 
> OData バージョン 3 を参照してください。 [OData v3 のエンティティ関係をサポートする](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations)です。


## <a name="add-a-supplier-entity"></a>Supplier エンティティを追加します。

> [!NOTE]
> このチュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用する ASP.NET Web API 2 を作成する](create-an-odata-v4-endpoint.md)です。


最初に、関連エンティティ必要があります。 という名前のクラスを追加`Supplier`Models フォルダーにします。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

ナビゲーション プロパティを追加、`Product`クラス。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

新しい**DbSet**を`ProductsContext`クラス、Entity Framework は、データベースに仕入先テーブルを含めるようにします。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

WebApiConfig.cs で追加、 &quot;Suppliers&quot;エンティティがエンティティ データ モデルに設定します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Suppliers コント ローラーを追加します。

追加、`SuppliersController`コント ローラーのフォルダーへのクラスです。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

このコント ローラーの CRUD 操作を追加する方法を表示しません。 手順は、Products コント ローラーと同じ (を参照してください[OData v4 エンドポイントを作成する](create-an-odata-v4-endpoint.md))。

## <a name="getting-related-entities"></a>関連エンティティの取得

に、製品の供給業者を取得するには、は、クライアントは、GET 要求を送信します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

この要求をサポートするには、次のメソッドを追加、`ProductsController`クラス。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

このメソッドは、既定の名前付け規則を使用します。

- メソッド名: getx メソッド、X はナビゲーション プロパティ。
- パラメーター名:*キー*

この名前付け規則に従うと、Web API はコント ローラー メソッドへの HTTP 要求を自動的にマップします。

要求の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

応答の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>関連するコレクションを取得します。

前の例では、製品は、1 つの仕入先がします。 ナビゲーション プロパティでは、コレクションを返すこともできます。 次のコードは、サプライヤーの製品を取得します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

この場合、メソッドを返します、 **IQueryable**の代わりに、 **SingleResult&lt;T&gt;**

要求の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

応答の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>エンティティ間のリレーションシップを作成します。

OData には、作成または既存の 2 つのエンティティ間のリレーションシップを削除するがサポートしています。 OData v4 の用語では、リレーションシップは、&quot;参照&quot;です。 (OData v3 のリレーションシップが呼び出された、*リンク*です。 プロトコルの相違点は関係ありませんチュートリアルではこの。)

参照が、独自の URI は、フォームの`/Entity/NavigationProperty/$ref`します。 たとえば、製品とその仕入先の間に参照のアドレスへの URI を次に示します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

リレーションシップを追加するには、クライアントは、このアドレスに POST または PUT 要求を送信します。

- ナビゲーション プロパティが 1 つのエンティティなどに設定されている場合に PUT`Product.Supplier`です。
- ナビゲーション プロパティが、コレクションなどに設定されている場合に投稿`Supplier.Products`です。

要求の本文には、リレーションシップ内の他のエンティティの URI が含まれています。 要求の例を次に示します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

この例では、クライアントに PUT 要求を送信`/Products(6)/Supplier/$ref`の $ref URI は、`Supplier`プロダクトの product ID = 6。 要求が成功した場合、サーバーは、204 (No Content) 応答を送信します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

ここでは、コント ローラーを追加するメソッドへのリレーションシップ、 `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty*パラメーターを設定するには、どのリレーションシップを指定します。 (1 つ以上のナビゲーション プロパティがエンティティを使用する場合は、他の追加できます`case`ステートメントです)。

*リンク*パラメーターには、業者の URI が含まれています。 Web API は、自動的にこのパラメーターの値を取得する要求本文を解析します。

供給業者を調べる必要があります ID (またはキー) の一部では、*リンク*パラメーター。 これを行うには、次のヘルパー メソッドを使用します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

基本的には、このメソッドは、URI のパスをセグメントに分割をキーを含むセグメントを見つけて、キーを正しい型に変換する OData ライブラリを使用します。

## <a name="deleting-a-relationship-between-entities"></a>エンティティ間のリレーションシップを削除します。

リレーションシップを削除するには、は、クライアントは、$ref URI に HTTP DELETE 要求を送信します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

製品と納入業者間のリレーションシップを削除するコント ローラーのメソッドを次に示します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

この場合、`Product.Supplier`は、 &quot;1&quot; 1 対多リレーションを設定するだけで、リレーションシップを削除するためのエンド`Product.Supplier`に`null`です。

&quot;多く&quot;クライアント、リレーションシップの end がエンティティを削除する関連を指定する必要があります。 これを行うには、クライアントは、要求のクエリ文字列に関連エンティティの URI を送信します。 たとえば、「製品 1」から削除するため「業者 1"。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

これをサポートする Web API で、含める必要は追加のパラメーターで、`DeleteRef`メソッドです。 製品を削除するコント ローラーのメソッドをここでは、`Supplier.Products`関係します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*キー*パラメーターは、業者のキーと*relatedKey*パラメーターは、製品から削除するキー、`Products`リレーションシップです。 Web API が、クエリ文字列から、キーを自動的に取得することに注意してください。
