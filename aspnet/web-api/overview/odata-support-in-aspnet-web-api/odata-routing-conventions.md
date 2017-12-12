---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: "ルーティング規約の ASP.NET Web API 2 Odata |Microsoft ドキュメント"
author: MikeWasson
description: "この記事では、OData エンドポイントの Web API を使用するルーティング規則について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: cd24a85a05e427f83d28cae876431d04cc295f17
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>ルーティング規約の ASP.NET Web API 2 Odata
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> この記事では、OData エンドポイントの Web API を使用するルーティング規則について説明します。


OData 要求を取得すると、Web API コント ローラー名とアクション名に要求をマップします。 マッピングは、HTTP メソッドと URI に基づいています。 たとえば、`GET /odata/Products(1)`にマップ`ProductsController.GetProduct`です。

この記事のパート 1 では、組み込みの OData ルーティング規約を説明します。 これらの規則は、OData エンドポイントの専用に設計され、既定の Web API ルーティング システムを置換します。 (を呼び出す場合、置換**MapODataRoute**)。

第 2 部では、カスタムのルーティング規則を追加する方法を説明します。 現在、組み込みの規則には、OData Uri の全体範囲は取り上げませんが、その他の問題を処理することを拡張することができます。

- [組み込みのルーティング規約](#conventions)
- [カスタム ルーティング規約](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>組み込みのルーティング規約

Web API の OData ルーティング規約を説明する前に、OData の Uri を理解することをお勧めします。 [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/)で構成されます。

- サービス ルート
- リソースのパス
- クエリ オプション

![](odata-routing-conventions/_static/image1.png)

ルーティングの重要な部分は、リソース パスです。 リソース パスは、セグメントに分割されます。 たとえば、`/Products(1)/Supplier`が 3 つのセグメント。

- `Products`"Products"という名前のエンティティ セットを指します。
- `1`セットから 1 つのエンティティを選択すると、エンティティ キー。
- `Supplier`関連エンティティを選択するナビゲーション プロパティです。

したがってこのパスは、製品 1 の提供元を取得します。

> [!NOTE]
> OData パス セグメントが URI セグメントを常に対応していません。 たとえば、「1」は、パス セグメントと見なされます。


**コント ローラー名。** コント ローラーの名前は常に、エンティティ セット リソースのパスのルートに由来します。 たとえば、次のリソース パスは`/Products(1)/Supplier`、Web API コント ローラーがという名前を探します`ProductsController`です。

**アクション名。** アクション名は、次の表に記載されているパス セグメントと、entity data model (EDM) から派生します。 場合によってには、アクション名の 2 つの選択肢があります。 たとえば、"Get"または&quot;GetProducts&quot;です。

**エンティティを照会します。**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| /Entityset を取得します。 | /製品 | GetEntitySet または Get | GetProducts |
| /Entityset(key) を取得します。 | /Products(1) | GetEntityType または Get | GetProduct |
| /Entityset (キー) を取得/キャスト | /(1) の製品/Models.Book | GetEntityType または Get | GetBook |

詳細については、次を参照してください。[読み取り専用 OData エンドポイントを作成](odata-v3/creating-an-odata-endpoint.md)です。

**作成、更新、およびエンティティを削除します。**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| /Entityset を投稿します。 | /製品 | PostEntityType または Post | PostProduct |
| /Entityset(key) を配置します。 | /Products(1) | PutEntityType または Put | PutProduct |
| /Entityset (キー) を配置/キャスト | /(1) の製品/Models.Book | PutEntityType または Put | PutBook |
| 修正プログラム/entityset(key) | /Products(1) | PatchEntityType または修正プログラム | PatchProduct |
| /Entityset (キー) をパッチ/キャスト | /(1) の製品/Models.Book | PatchEntityType または修正プログラム | PatchBook |
| /Entityset(key) を削除します。 | /Products(1) | DeleteEntityType または削除 | DeleteProduct |
| /Entityset (キー) を削除/キャスト | /(1) の製品/Models.Book | DeleteEntityType または削除 | DeleteBook |

**ナビゲーション プロパティのクエリを実行します。**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| GET/entityset (キー)/ナビゲーション | /製品 (1)/仕入先 | GetNavigationFromEntityType または GetNavigation | GetSupplierFromProduct |
| /Entityset (キー)/キャスト/ナビゲーションを取得します。 | /(1) の製品/Models.Book/Author | GetNavigationFromEntityType または GetNavigation | GetAuthorFromBook |

詳細については、次を参照してください。[のエンティティ関係作業](odata-v3/working-with-entity-relations.md)です。

**作成して、リンクを削除します。**

| 要求 | URI の例 | アクション名 |
| --- | --- | --- |
| POST/entityset (キー)/$links/ナビゲーション | /製品 (1)/$ リンク/仕入先 | CreateLink |
| PUT/entityset (キー)/$links/ナビゲーション | /製品 (1)/$ リンク/仕入先 | CreateLink |
| DELETE/entityset (キー)/$links/ナビゲーション | /製品 (1)/$ リンク/仕入先 | DeleteLink |
| /Entityset(key)/$links/navigation(relatedKey) を削除します。 | /Products/(1)/$links/Suppliers(1) | DeleteLink |

詳細については、次を参照してください。[のエンティティ関係作業](odata-v3/working-with-entity-relations.md)です。

**プロパティ**

*Web API 2 が必要です。*

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| GET/entityset (キー)/プロパティ | /製品 (1)/名前 | GetPropertyFromEntityType または GetProperty | GetNameFromProduct |
| /Entityset (キー) キャスト/プロパティを取得します。 | /(1) の製品/Models.Book/Author | GetPropertyFromEntityType または GetProperty | GetTitleFromBook |

**アクション**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| POST/entityset (キー)/操作 | /製品 (1) レート/ | ActionNameOnEntityType またはアクション名 | RateOnProduct |
| /Entityset (キー)/キャスト/アクションを投稿します。 | /(1) の製品/Models.Book/CheckOut | ActionNameOnEntityType またはアクション名 | CheckOutOnBook |

詳細については、次を参照してください。 [OData アクション](odata-v3/odata-actions.md)です。

**メソッドのシグネチャ**

メソッド シグネチャの一部のルールを次に示します。

- アクションがという名前のパラメーターを持つ場合は、パスには、キーが含まれています、*キー*です。
- アクションがという名前のパラメーターを持つ場合は、パスには、ナビゲーション プロパティにキーが含まれています、 *relatedKey*です。
- 装飾*キー*と*relatedKey*を持つパラメーター、 **[FromODataUri]**パラメーター。
- POST および PUT 要求は、エンティティ型のパラメーターを受け取ります。
- PATCH 要求は、型のパラメーターを受け取る**デルタ&lt;T&gt;**ここで、 *T*は、エンティティ型です。

リファレンスについては、組み込みの OData ルーティング規約ですべてのメソッドのシグネチャを説明する例を次に示します。

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>カスタム ルーティング規約

現在、組み込みの規則では、すべての可能な OData Uri は対応していません。 新しい規則を追加するには実装することによって、 **IODataRoutingConvention**インターフェイスです。 このインターフェイスでは、次の 2 つの方法があります。

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController**コント ローラーの名前を返します。
- **SelectAction**アクションの名前を返します。

両方の方法、その要求に規則が適用されない場合、メソッド必要があります null を返します。

**ODataPath**パラメーターが解析された OData リソース パスを表します。 一覧が含まれている **[ODataPathSegment](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatapathsegment.aspx)** リソース パスのセグメントごとに 1 つのインスタンスします。 **ODataPathSegment** 、抽象クラスです。 各セグメントの種類がから派生したクラスによって表される**ODataPathSegment**です。

**ODataPath.TemplatePath**プロパティは、文字列を連結したものを表すすべてのパス セグメント。 たとえば、次の URI が`/Products(1)/Supplier`、パス テンプレートは&quot;~/entityset/key/navigation&quot;です。 セグメントが URI セグメントに直接対応していないことに注意してください。 たとえば、エンティティ キー (1) は、独自として表されます。 **ODataPathSegment**です。

実装では通常、 **IODataRoutingConvention**は次の実行します。

1. 現在の要求にこの規則が適用される場合に表示するパス テンプレートを比較します。 適用されない場合は、null を返します。
2. 規則を適用する場合は、プロパティを使用して、 **ODataPathSegment**コント ローラーとアクションの名前を派生するインスタンス。
3. アクションのアクション パラメーター (通常はエンティティ キー) にバインドする必要がありますルート ディクショナリを任意の値を追加します。

具体的な例を見てみましょう。 組み込みのルーティング規則では、ナビゲーション コレクションにインデックスを作成することはできません。 つまり、次のような Uri の規則ではありません。

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

この種類のクエリを処理するカスタム ルーティング規則を次に示します。

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

メモ:

1. 派生した**EntitySetRoutingConvention**であるため、 **SelectController**そのクラスのメソッドは、この新しいルーティング規約の適切な。 つまり、再実装する必要はありません**SelectController**です。
2. GET 要求にのみ、およびパス テンプレートが場合にのみ、規則が適用される&quot;~/entityset/key/navigation/key&quot;です。
3. アクション名が&quot;{EntityType} を取得&quot;ここで、 *{EntityType}*ナビゲーション コレクションの種類です。 たとえば、 &quot;GetSupplier&quot;です。 任意の名前付け規則を &#8212; を使用することができます。確認、コント ローラーのアクションと一致します。
4. アクションという 2 つのパラメーターは、*キー*と*relatedKey*です。 (一部の定義済みのパラメーター名の一覧は、次を参照してください[ODataRouteConstants](https://msdn.microsoft.com/en-us/library/system.web.http.odata.routing.odatarouteconstants.aspx)。)。

次の手順では、ルーティング規則の一覧に新しい規則を追加します。 これは、次のコードに示すように場合に、構成時に発生します。

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

ここでは、いくつか他のサンプル ルーティング規則を調査すると便利です。

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

もちろん Web API 自体は、オープン ソースで確認できるようにし、[ソース コード](http://aspnetwebstack.codeplex.com/)組み込みルーティング規約のです。 これらで定義されます、 **System.Web.Http.OData.Routing.Conventions**名前空間。
