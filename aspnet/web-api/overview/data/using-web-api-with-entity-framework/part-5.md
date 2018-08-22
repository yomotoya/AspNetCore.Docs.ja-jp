---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: データ転送オブジェクト (Dto) の作成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 95075dd748f0fe4eb6d1c52d6bfe4a4576653b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826797"
---
<a name="create-data-transfer-objects-dtos"></a>データ転送オブジェクト (Dto) を作成します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

権利、web API、クライアントにデータベースのエンティティを公開します。 クライアントは、データベース テーブルに直接マップされるデータを受け取ります。 ただし、ない常にお勧めします。 クライアントに送信するデータの形状を変更することがあります。 たとえば、次の操作を行います。

- (前のセクションを参照してください)、循環参照を削除します。
- 表示するのには、クライアントは想定されていない特定のプロパティを非表示にします。
- ペイロードのサイズを軽減するために、いくつかのプロパティを省略します。
- クライアントの方が便利にする、入れ子になったオブジェクトを含むオブジェクト グラフを平坦化します。
- 「オーバーポスティング」脆弱性を回避します。 (を参照してください[モデルの検証](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)オーバーポスティング攻撃の詳細についてはします)。
- データベース層から、サービス層を分離します。

定義することがこれを実現する、*データ転送オブジェクト*(DTO)。 DTO は、ネットワーク経由でデータを送信する方法を定義するオブジェクトです。 書籍エンティティとそのしくみを見てみましょう。 Models フォルダーでは、2 つの DTO クラスを追加します。

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO`クラスには、すべての点を除いて、書籍モデルのプロパティが含まれています`AuthorName`作成者の名前を保持する文字列です。 `BookDTO`クラスにはからプロパティのサブセットが含まれています`BookDetailDTO`します。

2 つの GET メソッドを次に、置換、 `BooksController` Dto を返すバージョンのクラス。 LINQ を使用します**選択**書籍エンティティから Dto に変換するステートメント。

[!code-csharp[Main](part-5/samples/sample2.cs)]

ここで、新しいによって生成された SQL は、`GetBooks`メソッド。 EF が、LINQ を変換することを確認できます**選択**SQL SELECT ステートメントにします。

[!code-sql[Main](part-5/samples/sample3.sql)]

最後に、変更、 `PostBook` DTO を返すメソッド。

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> このチュートリアルで変換しています Dto を手動でコード。 ようなライブラリを使用することも[AutoMapper](http://automapper.org/)変換を自動的に処理します。
> 
> [!div class="step-by-step"]
> [前へ](part-4.md)
> [次へ](part-6.md)
