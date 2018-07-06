---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Web API 2 OData v3 のエンティティ関係サポート |Microsoft Docs
author: MikeWasson
description: ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。 OData を使用して、クライアントは、経由で移動することができます.
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: b24e3ca4e3d39b424bec6bb408bb0f85825c6761
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810745"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Web API 2 OData v3 のエンティティ関係をサポート
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> ほとんどのデータ セットは、エンティティ間のリレーションシップを定義します。 顧客が設定されている順序。ブックの「複数の作者により;製品では、仕入先があります。 クライアントは OData を使用して、エンティティ関係を移動できます。 製品を指定するには、業者を検索できます。 作成またはのリレーションシップを削除することもできます。 たとえば、製品の仕入先を設定できます。
> 
> このチュートリアルでは、ASP.NET Web API でこれらの操作をサポートする方法を示します。 チュートリアルは、チュートリアルに基づいて[Web API 2 OData v3 エンドポイントを作成する](creating-an-odata-endpoint.md)します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API 2
> - OData バージョン 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Supplier エンティティを追加します。

まず、OData フィードに新しいエンティティ型を追加する必要があります。 追加、`Supplier`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

このクラスは、エンティティ キーの文字列を使用します。 実際には、整数型のキーを使用するよりも少なくする必要があります。 その価値は OData で整数以外のキーの型を処理する方法を確認します。

次に、リレーションシップを追加することで作成します、`Supplier`プロパティを`Product`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

新しい追加**DbSet**を`ProductServiceContext`クラス、Entity Framework が含まれるように、`Supplier`データベース内のテーブル。

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

WebApiConfig.cs では、EDM モデルに"Suppliers"エンティティを追加します。

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>ナビゲーション プロパティ

供給業者の製品を取得するには、クライアントは、GET 要求を送信します。

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

ここで、ナビゲーション プロパティはで、「業者」、`Product`型。 この場合、`Supplier`は 1 つの項目がナビゲーション プロパティも (一対多または多対多のリレーションシップ) のコレクションを返します。

この要求をサポートするには、次のメソッドを追加、`ProductsController`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*キー*パラメーターは、製品のキー。 メソッドは、関連するエンティティ & #8212 をここでは、返します、`Supplier`インスタンス。 パラメーター名とメソッドの名前は、どちらも重要です。 一般に、ナビゲーション プロパティは、"X"という名前が場合、は、「getx メソッド」という名前のメソッドを追加する必要があります。 メソッドは、という名前のパラメーターを受け取る必要があります"*キー*"親のキーのデータ型と一致します。

必ず、 **[FromOdataUri]** 属性、*キー*パラメーター。 この属性では、Web API 要求 URI からキーを解析するときに、OData 構文の規則を使用するように指示します。

## <a name="creating-and-deleting-links"></a>作成およびリンクを削除します。

OData では、2 つのエンティティ間のリレーションシップを作成または削除をサポートします。 OData 用語で「リンク」とは関係 各リンクは、フォームを使用して URI*エンティティ*/$links/*エンティティ*します。 たとえば、製品から業者へのリンクは、ようになります。

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

新しいリンクを作成するには、クライアントは、リンク URI に POST 要求を送信します。 要求の本文は、ターゲット エンティティの URI です。 たとえば、"CTSO"キーを使用して仕入先があるとします。 "Product(1)"から"Supplier('CTSO')"へのリンクを作成するには、クライアントは、次のような要求を送信します。

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

リンクを削除するには、クライアントは、リンク URI に DELETE 要求を送信します。

**リンクを作成します。**

供給業者の製品リンクを作成するクライアントを有効にするには、次のコードを追加、`ProductsController`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

このメソッドは、3 つのパラメーターを受け取ります。

- *キー*: 親エンティティ (製品) にキー
- *navigationProperty*: ナビゲーション プロパティの名前。 この例では、唯一の有効なナビゲーション プロパティは、「サプライヤー」です。
- *リンク*: 関連エンティティの OData URI。 この値は、要求本文から取得されます。 などのリンク URI があります"`http://localhost/odata/Suppliers('CTSO')`、つまり、仕入先 ID = 'CTSO'。

メソッドは、業者を検索する、リンクを使用します。 場合は、一致する業者が見つかると、設定、`Product.Supplier`プロパティと、データベースに結果を保存します。

最も難しい部分は、リンク URI を解析しています。 基本的に、その URI に GET 要求を送信する結果をシミュレートする必要があります。 次のヘルパー メソッドは、これを行う方法を示します。 メソッドは、Web API ルーティング プロセスを起動し、失意、 **ODataPath**解析された OData パスを表すインスタンス。 リンク URI では、エンティティ キー、セグメントの 1 つがあります。 (ない場合、クライアントが不適切な URI を送信します。)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**リンクの削除**

リンクを削除するには、次のコードを追加、`ProductsController`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

この例では、ナビゲーション プロパティは、1 つ`Supplier`エンティティ。 ナビゲーション プロパティがコレクションである場合は、リンクを削除する URI は、関連エンティティのキーを含める必要があります。 例えば:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

この要求は、1 顧客から注文 1 を削除します。 この場合、DeleteLink メソッドは、次のシグネチャが完成します。

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey*パラメーターは、関連するエンティティのキー。 `DeleteLink`メソッドによって、プライマリ エンティティを参照してください、*キー*パラメーターによって、関連するエンティティを検索、 *relatedKey*パラメーター、および [削除] の関連付け。 データ モデルによっては、両方のバージョンを実装する必要あります`DeleteLink`します。 Web API では、要求 URI に基づく適切なバージョンを呼び出します。
