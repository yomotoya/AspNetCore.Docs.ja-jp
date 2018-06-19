---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Web API 2 OData v3 のエンティティ関係をサポートする |Microsoft ドキュメント
author: MikeWasson
description: ほとんどのデータ セットは、エンティティ間のリレーションを定義します。 お客様が設定されている順序です。ブックがある作成者です。製品では、仕入先があります。 OData を使用すると、クライアントは、経由で移動することができます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508411"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Web API 2 OData v3 のエンティティ関係をサポートします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> ほとんどのデータ セットは、エンティティ間のリレーションを定義します。 お客様が設定されている順序です。ブックがある作成者です。製品では、仕入先があります。 クライアントは OData を使用すると、エンティティ関係経由で移動できます。 製品を指定するには、供給業者を検索できます。 作成または、リレーションシップを削除することができますも。 たとえば、製品の仕入先を設定できます。
> 
> このチュートリアルでは、ASP.NET Web API でこれらの操作をサポートする方法を示します。 このチュートリアルは、チュートリアルに基づいて[Web API 2 OData v3 エンドポイントを作成する](creating-an-odata-endpoint.md)です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API 2
> - OData バージョン 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Supplier エンティティを追加します。

まず、OData フィードに新しいエンティティの種類を追加する必要があります。 追加、`Supplier`クラスです。

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

このクラスは、エンティティ キーの文字列を使用します。 実際には、整数キーを使用するよりも少なくする必要があります。 ですが、OData で整数以外の他の重要な型を処理する方法が表示される価値があります。

次に、追加することでリレーションシップを作成します、`Supplier`プロパティを`Product`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

新しい**DbSet**を`ProductServiceContext`クラス、Entity Framework が含まれるように、`Supplier`データベース内のテーブルです。

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

WebApiConfig.cs では、EDM モデルを"Suppliers"エンティティを追加します。

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>ナビゲーション プロパティ

に、製品の供給業者を取得するには、は、クライアントは、GET 要求を送信します。

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

ここで「業者」は、ナビゲーション プロパティで、`Product`型です。 この場合、`Supplier`参照の単一の項目がナビゲーション プロパティもコレクションを取得できます (一対多または多対多のリレーションシップ)。

この要求をサポートするには、次のメソッドを追加、`ProductsController`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*キー*パラメーターは、製品のキー。 メソッドを返します、関連するエンティティ & #8212 ここで、`Supplier`インスタンス。 メソッド名とパラメーター名はどちらも重要です。 一般に、ナビゲーション プロパティが"X"をという名前の場合は、「getx メソッド」という名前のメソッドを追加する必要があります。 メソッドは、という名前のパラメーターを受け取る必要があります"*キー*"親のキーのデータ型に一致します。

含める必要も、 **[FromOdataUri]** 属性、*キー*パラメーター。 この属性では、Web API 要求 URI のキーを解析する場合は、OData 構文の規則を使用するように指示します。

## <a name="creating-and-deleting-links"></a>作成して、リンクを削除します。

OData は、2 つのエンティティ間のリレーションシップを作成または削除をサポートします。 OData の用語で、リレーションシップは「リンク」です。 各リンクには、フォームを使用して URI*エンティティ*/$links/*エンティティ*です。 たとえば、製品から業者へのリンクは、このようになります。

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

新しいリンクを作成するには、クライアントは、リンク URI に POST 要求を送信します。 要求の本文は、ターゲット エンティティの URI です。 たとえば、"CTSO"キーを使用して仕入先があるとします。 "Product(1)"から"Supplier('CTSO')"へのリンクを作成するには、クライアントは、次のように要求を送信します。

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

リンクを削除するのには、クライアントは、リンク URI に DELETE 要求を送信します。

**リンクを作成します。**

製品業者へのリンクを作成するクライアントを有効にする次のコードを追加、`ProductsController`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

このメソッドは、次の 3 つのパラメーターを受け取ります。

- *キー*: 親エンティティ (製品) にキー
- *navigationProperty*: ナビゲーション プロパティの名前。 この例では、唯一の有効なナビゲーション プロパティは、「サプライヤー」です。
- *リンク*: 関連エンティティの OData URI。 この値は、要求本文から取得されます。 たとえば、リンク URI があります"`http://localhost/odata/Suppliers('CTSO')`、つまり仕入先 ID の 'CTSO' を = です。

メソッドでは、リンクを使用して、供給業者を検索します。 一致する業者が見つかった場合、メソッドを設定、`Product.Supplier`プロパティされ、データベースに結果を保存します。

最も難しい部分は、リンク URI を解析しています。 基本的には、その URI に GET 要求を送信の結果をシミュレートする必要があります。 次のヘルパー メソッドは、これを行う方法を示します。 メソッドは、Web API ルーティング プロセスを起動し、返される、 **ODataPath**解析された OData パスを表すインスタンス。 リンク URI には、エンティティ キー セグメントの 1 つがあります。 (いない場合、クライアントが無効な URI を送信します。)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**リンクを削除します。**

リンクを削除するには、次のコードを追加、`ProductsController`クラス。

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

この例では、ナビゲーション プロパティは、1 つ`Supplier`エンティティです。 ナビゲーション プロパティがコレクションの場合は、リンクを削除する URI は、この関連エンティティのキーを含める必要があります。 例:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

この要求は、1 の顧客から注文 1 を削除します。 この場合、DeleteLink メソッドは、次のシグネチャが得られます。

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey*パラメーターは、この関連エンティティのキー。 `DeleteLink`メソッド、によってプライマリ エンティティを検索、*キー*パラメーター、によって、この関連エンティティを検索、 *relatedKey*パラメーター、および関連付けを削除します。 によっては、データ モデルの両方のバージョンを実装する必要があります`DeleteLink`です。 Web API では、要求 URI に基づく適切なバージョンを呼び出します。
