---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: ASP.NET Web API 2.2 を使用して OData v4 のエンティティ関係 |Microsoft Docs
author: MikeWasson
description: ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。 OData を使用して、クライアントは、経由で移動することができます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 56cadbabaa7ca64ba39232495e25178849d5e3c2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377647"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>ASP.NET Web API 2.2 を使用して OData v4 のエンティティ関係
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。 クライアントは OData を使用して、エンティティ関係を移動できます。 製品を指定するには、業者を検索できます。 作成またはのリレーションシップを削除することもできます。 たとえば、製品の仕入先を設定できます。
> 
> このチュートリアルでは、ASP.NET Web API を使用しての OData v4 のこれらの操作をサポートする方法を示します。 チュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
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
> OData バージョン 3 では、次を参照してください。 [OData v3 のエンティティ関係をサポートしている](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations)します。


## <a name="add-a-supplier-entity"></a>Supplier エンティティを追加します。

> [!NOTE]
> チュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)です。


最初に、関連エンティティ必要があります。 という名前のクラスを追加`Supplier`Models フォルダーにします。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

ナビゲーション プロパティを追加、`Product`クラス。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

新しい追加**DbSet**を`ProductsContext`クラス、Entity Framework は、データベースの仕入先のテーブルを含めるようにします。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

WebApiConfig.cs で追加、 &quot;Suppliers&quot;エンティティがエンティティ データ モデルに設定。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>仕入先のコント ローラーを追加します。

追加、`SuppliersController`コント ローラーのフォルダーにクラス。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

このコント ローラーに対する CRUD 操作を追加する方法は表示されません。 手順は、Products コント ローラーの場合と同じ (を参照してください[OData v4 エンドポイントを作成](create-an-odata-v4-endpoint.md))。

## <a name="getting-related-entities"></a>関連エンティティの取得

供給業者の製品を取得するには、クライアントは、GET 要求を送信します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

この要求をサポートするには、次のメソッドを追加、`ProductsController`クラス。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

このメソッドは、既定の名前付け規則を使用します。

- メソッド名: getx メソッド、X はナビゲーション プロパティ。
- パラメーター名:*キー*

この名前付け規則に従うと、コント ローラー メソッドに HTTP 要求は、Web API によって自動的にマップされます。

要求の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

応答の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>関連コレクションを取得します。

前の例では、製品には、1 つの仕入先があります。 ナビゲーション プロパティでは、コレクションを返すこともできます。 次のコードでは、サプライヤーの製品を取得します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

この場合、メソッドが返されます、 **IQueryable**の代わりに、 **SingleResult&lt;T&gt;**

要求の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

応答の例 HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>エンティティ間のリレーションシップを作成します。

OData では、作成または既存の 2 つのエンティティ間のリレーションシップの削除をサポートします。 OData v4 用語では、リレーションシップは、&quot;参照&quot;します。 (OData v3 の場合は、リレーションシップが呼び出された、*リンク*します。 プロトコルの相違点は関係ありませんチュートリアルではこの。)

参照がフォームに、独自の URI`/Entity/NavigationProperty/$ref`します。 たとえば、製品とその供給業者間の参照に対応する URI を次に示します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

リレーションシップを追加するには、クライアントは、このアドレスに POST または PUT 要求を送信します。

- ナビゲーション プロパティが 1 つのエンティティなどに設定されている場合に PUT`Product.Supplier`します。
- ナビゲーション プロパティが、コレクションなどに設定されている場合に投稿`Supplier.Products`します。

要求の本文には、リレーションシップのもう一方のエンティティの URI が含まれています。 要求の例を次に示します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

この例では、クライアントが送信する PUT 要求を`/Products(6)/Supplier/$ref`、$ref URI である、 `Supplier` ID を持つ製品の 6 を =。 要求が成功すると、204 (No Content) 応答がサーバーに送信します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

リレーションシップを追加するコント ローラー メソッドを次に示します、 `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty*パラメーターを設定するには、どのリレーションシップを指定します。 (エンティティには、複数のナビゲーション プロパティがある場合は、詳細を追加`case`ステートメントです)。

*リンク*パラメーターには、サプライヤーの URI が含まれています。 Web API には、このパラメーターの値を取得する要求本文に自動的に解析します。

供給業者を検索する必要があります ID (またはキー) の一部では、*リンク*パラメーター。 これを行うには、次のヘルパー メソッドを使用します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

基本的には、このメソッドでは、OData ライブラリを使用して、URI のパスをセグメントに分割、キーを含むセグメントを見つける、キーに適切な型変換をします。

## <a name="deleting-a-relationship-between-entities"></a>エンティティ間のリレーションシップを削除します。

リレーションシップを削除するには、クライアントは $ref URI に HTTP DELETE 要求を送信します。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

製品と仕入先の間のリレーションシップを削除するコント ローラー メソッドを次に示します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

この場合、`Product.Supplier`は、 &quot;1&quot;設定するだけで、リレーションシップを削除できるように 1 対多のリレーションシップの終了`Product.Supplier`に`null`します。

&quot;多く&quot;を削除するエンティティに関連するリレーションシップ、クライアントの最後を指定する必要があります。 これを行うには、クライアントは、要求のクエリ文字列で、関連するエンティティの URI を送信します。 たとえば、「製品 1」から削除する"1" 仕入先。

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Web API でこれをサポートする必要がありますで追加のパラメーターを含める、`DeleteRef`メソッド。 製品を削除するコント ローラー メソッドを次に示します、`Supplier.Products`関係します。

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*キー*パラメーターは、業者のキーと*relatedKey*パラメーターは、製品から削除するキー、`Products`リレーションシップ。 Web API が、クエリ文字列から、キーを自動的に取得することに注意してください。
