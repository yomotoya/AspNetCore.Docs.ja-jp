---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: ASP.NET Web API 2 OData で $select, $expand, および $value を使用する |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: c5c0d374a918706e65254121ba5fa1516afdaf75
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814460"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>ASP.NET Web API 2 OData で $select, $expand, および $value を使用する
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

Web API 2 では、OData における $select, $expand, および $value オプションのサポートを追加しています。 これらのオプションは、サーバーから返された形式を制御することをクライアントに許可します。

- **$expand** は、応答にインラインで含まれる関連するエンティティを発生させます。
- **$select**は、応答に含めるプロパティのサブセットを選択します。
- **$value**は、プロパティの未加工の値を取得します。

## <a name="example-schema"></a>スキーマの例

この記事では、次の 3 つのエンティティを定義する OData サービスを使用します。 製品、仕入先、およびカテゴリ。 各製品には、1 つのカテゴリと仕入先の 1 つがあります。

![](using-select-expand-and-value/_static/image1.png)

エンティティ モデルを定義する c# クラスを次に示します。

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

注意、`Product`クラスのナビゲーション プロパティを定義、`Supplier`と`Category`します。 `Category`クラスは、各カテゴリの製品のナビゲーション プロパティを定義します。

このスキーマの OData エンドポイントを作成するには、Visual Studio 2013 のスキャフォールディングを」の説明に従って使用[で ASP.NET Web API OData エンドポイントを作成する](odata-v3/creating-an-odata-endpoint.md)します。 製品、カテゴリ、および仕入先の別のコント ローラーを追加します。

## <a name="enabling-expand-and-select"></a>$ の有効化を展開し、$select

Visual Studio 2013 では、Web API OData のスキャフォールディングは、その $ の展開を自動的にサポートおよび $select にコント ローラーを作成します。 リファレンスについては、ここでは $ をサポートするための要件を展開し、コント ローラーの $select します。

コレクションの場合、コント ローラーの`Get`メソッドが返す必要があります、 **IQueryable**します。

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

単一のエンティティを返す、 **SingleResult&lt;T&gt;**、T は、 **IQueryable** 0 または 1 つのエンティティを格納しています。

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

また、装飾、`Get`メソッド、 **[Queryable]** 属性は、前のコード スニペットで示すように。 代わりに、呼び出す**EnableQuerySupport**上、 **HttpConfiguration**スタートアップ時のオブジェクト。 (詳細については、次を参照してください[OData クエリ オプションを有効にする](supporting-odata-query-options.md#enable)。)。

## <a name="using-expand"></a>$ を使用して展開

OData エンティティまたはコレクションを照会するときに、既定の応答に関連するエンティティは含まれません。 たとえば、カテゴリ エンティティ セットの既定の応答を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

ご覧のように、応答は含まれません任意の製品では、Category エンティティは製品のナビゲーション リンク。 ただし、クライアントは $ を使用できる展開すると、各カテゴリの製品の一覧を取得します。 $Expand オプションを展開し、要求のクエリ文字列には。

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

これで、サーバー カテゴリごとに、インラインで、カテゴリの製品が含まれます。 応答ペイロードを次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

"Value"配列内の各エントリに製品一覧が含まれていることを確認します。

$ では、展開するナビゲーション プロパティのオプションはコンマ区切りの一覧を展開します。 次の要求は、カテゴリと製品の仕入先の両方を展開します。

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

ナビゲーション プロパティの 1 つ以上のレベルを展開することができます。 次の例には、カテゴリのすべての製品と各製品の仕入先が含まれます。

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

既定では、Web API は、2、最大の深さを制限します。 ような複雑な要求を送信することから、クライアントを妨げる`$expand=Orders/OrderDetails/Product/Supplier/Region`、効率的にクエリを実行し、大規模な応答を作成するがあります。 既定値を上書きするには、設定、 **MaxExpansionDepth**プロパティを **[Queryable]** 属性。

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

$ の詳細については、オプションを展開するを参照してください。 [Expand システム クエリ オプション ($ の展開)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) 、公式の OData ドキュメント。

## <a name="using-select"></a>$Select を使用します。

$Select オプションでは、応答本文に含めるプロパティのサブセットを指定します。 たとえば、名前と各製品の価格を取得するには、次のクエリを使用します。

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

$Select を組み合わせることができ、$ が同じクエリ内で展開します。 $Select オプションに展開されたプロパティを含めるに確認します。 たとえば、次の要求は、業者と製品の名前を取得します。

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

展開のプロパティ内のプロパティを選択することもできます。 次の要求では、製品を展開し、カテゴリ名と製品名を選択します。

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

$Select オプションの詳細については、次を参照してください。 [Select システム クエリ オプション ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) 、公式の OData ドキュメント。

## <a name="getting-individual-properties-of-an-entity-value"></a>エンティティ ($value) の個々 のプロパティを取得します。

エンティティから個々 のプロパティを取得する OData クライアントの 2 つの方法はあります。 クライアントでは、OData の形式で値を取得することまたは、プロパティの生の値を取得できます。

次の要求は、OData 形式でプロパティを取得します。

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

JSON 形式で応答の例を示します。

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

プロパティの生の値を取得するには、URI に $value を追加します。

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

応答を次に示します。 コンテンツの種類が"text/plain"、JSON に注意してください。

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

という名前のメソッドを追加すると、OData コント ローラーでこれらのクエリをサポートするために`GetProperty`ここで、`Property`プロパティの名前を指定します。 たとえば、Name プロパティを取得するメソッドを名前は`GetName`します。 メソッドは、そのプロパティの値を返す必要があります。

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
