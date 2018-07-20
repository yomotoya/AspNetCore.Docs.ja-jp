---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: ルーティング規則では、ASP.NET Web API 2 Odata |Microsoft Docs
author: MikeWasson
description: この記事では、Web API は OData エンドポイントを使用しているルーティング表記規則について説明します。
ms.author: aspnetcontent
ms.date: 07/31/2013
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 645e820d3b03d1e3d2ac088973f6296efa5c2f40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818266"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>ルーティング規則では、ASP.NET Web API 2 Odata
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> この記事では、Web API は OData エンドポイントを使用しているルーティング表記規則について説明します。


Web API OData の要求を取得、コント ローラー名とアクション名を要求がマップされます。 マッピングは、HTTP メソッドと URI に基づきます。 たとえば、`GET /odata/Products(1)`マップ`ProductsController.GetProduct`します。

この記事のパート 1 で、組み込みの OData ルーティング規約を取り上げます。 これらの規則は、OData エンドポイントの向けに設計し、既定の Web API ルーティング システムを置換します。 (呼び出した場合、置換の出力**MapODataRoute**)。

第 2 部では、カスタム ルーティング規則を追加する方法を紹介します。 現在、組み込みの規則には、OData Uri の全体範囲は取り上げませんが、その他の問題を処理する際に拡張することができます。

- [組み込みのルーティング規約](#conventions)
- [カスタム ルーティング規約](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>組み込みのルーティング規約

Web API での OData ルーティング規約を説明する前に、OData Uri を理解することをお勧めします。 [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/)で構成されます。

- サービス ルート
- リソース パス
- クエリ オプション

![](odata-routing-conventions/_static/image1.png)

ルーティングの重要な部分は、リソース パスです。 リソース パスは、セグメントに分割されます。 たとえば、`/Products(1)/Supplier`が 3 つのセグメント。

- `Products` "Products"という名前のエンティティ セットを指します。
- `1` セットから 1 つのエンティティを選択すると、エンティティ キー。
- `Supplier` 関連エンティティを選択するナビゲーション プロパティです。

したがってこのパスは、1 の製品の仕入先を取得します。

> [!NOTE]
> OData パス セグメントが URI セグメントを常に対応していません。 たとえば、「1」は、パス セグメントと見なされます。


**コント ローラーの名前。** コント ローラー名は常にリソース パスのルートにある設定エンティティから派生します。 たとえば、リソース パスが`/Products(1)/Supplier`、Web API がという名前のコント ローラーの検索`ProductsController`します。

**アクション名。** アクション名は、次の表に記載されている、パス セグメントと、entity data model (EDM) から派生されます。 場合によってには、アクション名の 2 つの選択肢があります。 たとえば、"Get"または&quot;GetProducts&quot;します。

**エンティティを照会します。**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| GET /entityset | /製品 | GetEntitySet または Get | GetProducts |
| GET /entityset(key) | /Products(1) | GetEntityType または Get | GetProduct |
| GET /entityset(key)/cast | /(1) の製品/Models.Book | GetEntityType または Get | GetBook |

詳細については、次を参照してください。[読み取り専用 OData エンドポイントを作成](odata-v3/creating-an-odata-endpoint.md)です。

**作成、更新、およびエンティティの削除**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| /Entityset を投稿します。 | /製品 | PostEntityType または Post | PostProduct |
| PUT /entityset(key) | /Products(1) | PutEntityType または Put | PutProduct |
| /Entityset (キー) を配置/キャスト | /(1) の製品/Models.Book | PutEntityType または Put | PutBook |
| PATCH /entityset(key) | /Products(1) | PatchEntityType または修正プログラム | PatchProduct |
| PATCH /entityset(key)/cast | /(1) の製品/Models.Book | PatchEntityType または修正プログラム | PatchBook |
| DELETE /entityset(key) | /Products(1) | DeleteEntityType または削除 | DeleteProduct |
| DELETE /entityset(key)/cast | /(1) の製品/Models.Book | DeleteEntityType または削除 | DeleteBook |

**ナビゲーション プロパティのクエリを実行します。**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| GET /entityset(key)/navigation | /製品 (1) Supplier/ | GetNavigationFromEntityType または GetNavigation | GetSupplierFromProduct |
| キャスト/ナビゲーション/entityset (キー) を取得します。 | /(1) の製品/Models.Book/Author | GetNavigationFromEntityType または GetNavigation | GetAuthorFromBook |

詳細については、次を参照してください。[操作エンティティ関係](odata-v3/working-with-entity-relations.md)します。

**作成およびリンクを削除します。**

| 要求 | URI の例 | アクション名 |
| --- | --- | --- |
| POST /entityset(key)/$links/navigation | /製品 (1)/$ リンク/仕入先 | CreateLink |
| PUT/entityset (キー)/$links/ナビゲーション | /製品 (1)/$ リンク/仕入先 | CreateLink |
| DELETE /entityset(key)/$links/navigation | /製品 (1)/$ リンク/仕入先 | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers(1) | DeleteLink |

詳細については、次を参照してください。[操作エンティティ関係](odata-v3/working-with-entity-relations.md)します。

**Properties**

*Web API 2 が必要です。*

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| GET /entityset(key)/property | /製品 (1)/名前 | GetPropertyFromEntityType または GetProperty | GetNameFromProduct |
| GET /entityset(key)/cast/property | /(1) の製品/Models.Book/Author | GetPropertyFromEntityType または GetProperty | GetTitleFromBook |

**アクション**

| 要求 | URI の例 | アクション名 | アクションの例 |
| --- | --- | --- | --- |
| POST /entityset(key)/action | /製品 (1) レート/ | ActionNameOnEntityType またはアクション名 | RateOnProduct |
| 事後/entityset (キー) キャスト/アクション | /(1) の製品/Models.Book/CheckOut | ActionNameOnEntityType またはアクション名 | CheckOutOnBook |

詳細については、次を参照してください。 [OData アクション](odata-v3/odata-actions.md)します。

**メソッド シグネチャ**

メソッド シグネチャの一部のルールを次に示します。

- パスにキーが含まれている場合、アクションが必要という名前のパラメーター*キー*します。
- 場合は、パスには、ナビゲーション プロパティにキーが含まれている、アクションがという名前のパラメーターを必要*relatedKey*します。
- 装飾*キー*と*relatedKey*を持つパラメーター、 **[FromODataUri]** パラメーター。
- POST および PUT 要求は、エンティティ型のパラメーターを受け取る。
- PATCH 要求は、型のパラメーターを受け取る**デルタ&lt;T&gt;** ここで、 *T*は、エンティティ型です。

リファレンスについては、すべて組み込みの OData ルーティング規約のメソッド シグネチャを示す例を示します。

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>カスタム ルーティング規約

現在、組み込みの規約ではすべての可能な OData Uri は含まれません。 新しい規則を追加するには、実装、 **IODataRoutingConvention**インターフェイス。 このインターフェイスでは、2 つの方法があります。

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController**コント ローラーの名前を返します。
- **SelectAction**アクションの名前を返します。

両方の方法のその要求には、規則が適用されない場合、メソッドは null を返します。

**ODataPath**パラメーターが解析された OData リソース パスを表します。 一覧が含まれている**[した ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** リソース パスのセグメントごとに 1 つのインスタンスします。 **した ODataPathSegment** 、抽象クラスです。 各セグメントの種類がから派生したクラスによって表される**した ODataPathSegment**します。

**ODataPath.TemplatePath**プロパティは、文字列を連結したものを表すすべてのパス セグメント。 たとえば、URI は、 `/Products(1)/Supplier`、パス テンプレートが&quot;~/entityset/key/navigation&quot;します。 セグメントが URI セグメントに直接対応していないことに注意してください。 たとえば、エンティティ キー (1) は、それ自体として表されます。**した ODataPathSegment**します。

実装では通常、 **IODataRoutingConvention**は次の処理します。

1. 現在の要求にこの規則が適用されるかどうかをパス テンプレートを比較します。 適用されない場合は、null を返します。
2. プロパティを使用して、規則が適用される場合、**した ODataPathSegment**インスタンス コント ローラーとアクションの名前を作成します。
3. 操作については、任意の値をアクション パラメーター (通常はエンティティ キー) にバインドする必要がありますルート ディクショナリに追加します。

具体的な例を見てみましょう。 組み込みのルーティング規則では、ナビゲーション コレクションにインデックスを作成することはできません。 つまり、次のような Uri の規則はありません。

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

この種類のクエリを処理する場合は、カスタム ルーティング規則を次に示します。

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

メモ:

1. 派生した**EntitySetRoutingConvention**ため、 **SelectController**そのクラスのメソッドは、この新しいルーティング規則の適切な。 つまり、再実装する必要はありません**SelectController**します。
2. パス テンプレートが場合にのみ、GET 要求にのみ、規則が適用される&quot;~/entityset/key/navigation/key&quot;します。
3. アクション名が&quot;取得 {entitytype}&quot;ここで、 *{entitytype}* ナビゲーション コレクションの種類です。 たとえば、 &quot;GetSupplier&quot;します。 任意の名前付け規則を使用する&#8212;必ずコント ローラーのアクションと一致します。
4. アクションという 2 つのパラメーターは、*キー*と*relatedKey*します。 (一部の定義済みのパラメーター名の一覧は、次を参照してください[ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx)。)。

次の手順では、新しい規則をルーティング規則の一覧に追加します。 これは、次のコードに示すように場合に、構成中に発生します。

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

いくつかその他のサンプル ルーティング規約を調べるに便利なを次に示します。

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Web API 自体はオープン ソースの表示できるようにして、[ソース コード](http://aspnetwebstack.codeplex.com/)組み込みのルーティング規則の詳細について。 これらで定義されます、 **System.Web.Http.OData.Routing.Conventions**名前空間。
