---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: データ転送オブジェクト (Dto) を作成 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a>データ転送オブジェクト (Dto) を作成します。
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

右側、web API、クライアントへのデータベース エンティティを公開します。 クライアントは、データベース テーブルに直接マップされるデータを受け取ります。 ただし、れていない常にことをお勧めします。 クライアントに送信されるデータの形状を変更する場合があります。 たとえば、次の操作を行います。

- (前のセクションを参照してください) の循環参照を削除します。
- 表示するのには、クライアントは想定されていない特定のプロパティを非表示にします。
- ペイロードのサイズを軽減するためにいくつかのプロパティを省略します。
- クライアントの方が便利にする、入れ子になったオブジェクトを格納するオブジェクト グラフを平坦化します。
- "過剰ポスティングの脆弱性を回避します。 (を参照してください[モデルの検証](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md)過剰ポスティングのディスカッションのです)。
- データベース層から、サービス層を分離します。

これを実現することができますを定義する、*データ転送オブジェクト*(DTO)。 DTO は、ネットワーク経由でデータを送信する方法を定義するオブジェクトです。 Book エンティティで動作する方法を見てみましょう。 Models フォルダーには、2 つの DTO クラスを追加します。

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO`クラスには、すべての点を除いて、Book モデルからのプロパティが含まれます`AuthorName`作成者の名前を保持する文字列です。 `BookDTO`クラスからプロパティのサブセットに含まれる`BookDetailDTO`です。

2 つの GET メソッドを次に、置換、 `BooksController` dtos の使用を返すバージョンのクラスです。 LINQ を使用して**選択**ステートメントを Book エンティティからの dtos の使用に変換します。

[!code-csharp[Main](part-5/samples/sample2.cs)]

ここでは、新しいによって生成された SQL`GetBooks`メソッドです。 EF に LINQ が変換を参照してください**選択**SQL SELECT ステートメントにします。

[!code-sql[Main](part-5/samples/sample3.sql)]

最後に、変更、 `PostBook` DTO を返します。

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> このチュートリアルではお変換 dtos の使用を手動でのコードにします。 別のオプションはのようなライブラリを使用する[AutoMapper](http://automapper.org/)変換を自動的に処理します。
> 
> [!div class="step-by-step"]
> [前へ](part-4.md)
> [次へ](part-6.md)
