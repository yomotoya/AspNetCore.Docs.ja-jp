---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "ASP.NET Web API 2 OData で $select, $expand, および $value を使用する |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>$Select を使用すると、$を展開し、ASP.NET Web API 2 OData $value
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

Web API 2 では、OData における $select, $expand, および $value オプションのサポートを追加しています。これらのオプションは、サーバーから返された形式を制御することをクライアントに許可します。

- **$expand** は、応答にインラインで含まれる関連するエンティティを発生させます。
- **$select** は、応答に含めるプロパティのサブセットを選択します。
- **$value** は、プロパティの未加工の値を取得します。

## <a name="example-schema"></a>例のスキーマ

この記事では、次の 3 つのエンティティを定義する OData サービスを使用します。 商品、供給業者、およびカテゴリ。 各製品には、1 つのカテゴリと 1 つの仕入先があります。

![](using-select-expand-and-value/_static/image1.png)

エンティティ モデルを定義する c# クラスを次に示します。

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

注意して、`Product`クラスのナビゲーション プロパティを定義、`Supplier`と`Category`です。 `Category`クラスは、各カテゴリの製品のナビゲーション プロパティを定義します。

このスキーマの OData エンドポイントを作成するには、Visual Studio 2013 のスキャフォールディングを」の説明に従って使用[ASP.NET Web API で OData エンドポイントを作成する](odata-v3/creating-an-odata-endpoint.md)です。 製品、カテゴリ、および仕入先の別のコント ローラーを追加します。

## <a name="enabling-expand-and-select"></a>$ の有効化を展開し、$select

Visual Studio 2013 では、Web API OData のスキャフォールディングは、その自動的にサポートしている $展開と $select にコント ローラーを作成します。 リファレンスについては、ここでは、$ をサポートするための要件を展開し、コント ローラーの $select します。

コレクションの場合、コント ローラーの`Get`メソッドが返す必要があります、 **IQueryable**です。

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

単一のエンティティを返す、 **SingleResult&lt;T&gt;**ここで T は、 **IQueryable** 0 または 1 つのエンティティを格納しています。

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

装飾することも、`Get`メソッド、 **[Queryable]**属性、前のコード スニペットで示すようにします。 代わりに、呼び出す**EnableQuerySupport**上、 **HttpConfiguration**スタートアップ時のオブジェクト。 (詳細については、次を参照してください[OData クエリ オプションを有効にすると](supporting-odata-query-options.md#enable)。)。

## <a name="using-expand"></a>$ を使用して展開

OData エンティティまたはコレクションのクエリを実行する場合、既定の応答には関連エンティティが含まれません。 たとえば、カテゴリ エンティティ セットの既定の応答を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

ご覧のように、応答は含まれません、製品、Category エンティティが製品にナビゲーション リンクを持っていて。 ただし、クライアントが $ を使用して展開すると、各カテゴリの製品の一覧を取得します。 $ オプションを展開し、要求のクエリ文字列には。

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

これで、サーバー カテゴリごとに、インラインでカテゴリの製品が含まれます。 応答ペイロードを次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

"Value"配列の各エントリに製品の一覧が含まれていることを確認します。

$ は、展開するナビゲーション プロパティのオプションは、コンマで区切られた一覧を展開します。 次の要求は、カテゴリ、製品の供給業者の両方を展開します。

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

ナビゲーション プロパティの 1 つ以上のレベルを展開することができます。 次の例では、カテゴリのすべての製品と製品ごとのサプライヤーのも含まれています。

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

既定では、Web API は、2 に展開の最大の深さを制限します。 妨げているクライアントと同様に複雑な要求を送信する`$expand=Orders/OrderDetails/Product/Supplier/Region`、されない可能性があるクエリを実行し、大規模な応答を作成する効率的です。 既定をオーバーライドするには設定、 **MaxExpansionDepth**プロパティを**[Queryable]**属性。

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

$ の詳細については、オプションを展開しを参照してください[Expand システム クエリ オプション ($展開)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand)公式の OData ドキュメントにします。

## <a name="using-select"></a>$Select を使用します。

$Select オプションでは、応答本文に含めるプロパティのサブセットを指定します。 たとえば、名前と各製品の価格だけを取得するには、次のクエリを使用します。

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

$Select を組み合わせることができ、同じクエリで $ を展開します。 $Select オプションに展開されたプロパティを含めることを確認してください。 たとえば、次の要求は、業者と製品の名前を取得します。

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

展開されたプロパティ内のプロパティを選択することもできます。 次の要求は、製品を展開し、カテゴリ名と製品名を選択します。

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

応答本文を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

$Select オプションの詳細については、次を参照してください。[選択システム クエリ オプション ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select)公式の OData ドキュメントにします。

## <a name="getting-individual-properties-of-an-entity-value"></a>エンティティ ($value) の個々 のプロパティを取得します。

エンティティから個々 のプロパティを取得する OData クライアントの 2 つの方法はあります。 クライアントでは、OData 形式で値を取得することまたは、プロパティの生の値を取得できます。

次の要求は、OData 形式でプロパティを取得します。

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

JSON 形式で応答の例を次に示します。

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

プロパティの生の値を取得するには、URI に $value を追加します。

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

ここでは応答です。 コンテンツの種類が「テキスト/プレーン」されない JSON に注意してください。

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

これらのクエリ、OData コント ローラーをサポートするには、という名前のメソッドを追加`GetProperty`ここで、`Property`プロパティの名前を指定します。 たとえば、Name プロパティを取得するメソッドの名前は`GetName`します。 メソッドは、そのプロパティの値を返す必要があります。

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
