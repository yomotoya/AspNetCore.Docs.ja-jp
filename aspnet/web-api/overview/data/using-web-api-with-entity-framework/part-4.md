---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: エンティティ関係の処理 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 5d05a2e5d4380a15078317545325bd20fde3f83c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826576"
---
<a name="handling-entity-relations"></a>エンティティ関係の処理
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、EF が関連エンティティを読み込む方法と、モデル クラス内で循環ナビゲーション プロパティを処理する方法のいくつかの詳細について説明します。 (ここでは、バック グラウンドの知識を提供し、チュートリアルを完了する必要はありません。 場合は、必要に応じて[パート 5](part-5.md)。)。

## <a name="eager-loading-versus-lazy-loading"></a>一括読み込みと遅延読み込み

リレーショナル データベースでの EF を使用する場合は、EF が関連するデータを読み込む方法を理解する必要があります。

EF によって生成される SQL クエリを表示する便利です。 SQL をトレースするには、次のコードの行を追加、`BookServiceContext`コンス トラクター。

[!code-csharp[Main](part-4/samples/sample1.cs)]

/Api/books に GET 要求を送信する場合は、次のような JSON が返されます。

[!code-console[Main](part-4/samples/sample2.cmd)]

書籍が有効な著者の Id を含んでいるにもかかわらず Author プロパティが null の場合、ことを確認できます。 EF が作成者の関連エンティティを読み込んでいないためにです。 SQL クエリのトレース ログは、これを確認します。

[!code-console[Main](part-4/samples/sample3.sql)]

SELECT ステートメントは、ブックの「テーブルから取得し作成者表を参照していますいません。

リファレンスについては、メソッドでは、ここで、`BooksController`書籍の一覧を返すクラス。

[!code-csharp[Main](part-4/samples/sample4.cs)]

JSON データの一部として、作成者取得する方法を見てみましょう。 Entity Framework に関連するデータを読み込む 3 つの方法: 一括読み込み、遅延読み込み、および明示的な読み込み。 各手法では、上のトレードオフがあるため、そのしくみを理解することが重要です。

### <a name="eager-loading"></a>一括読み込み

*一括読み込み*EF は、最初のデータベース クエリの一部として関連エンティティを読み込みます。 一括読み込みを実行するのには、使用、 **System.Data.Entity.Include**拡張メソッド。

[!code-csharp[Main](part-4/samples/sample5.cs)]

これは、クエリに含めるデータを作成、EF に指示します。 この変更を行うアプリを実行すると、JSON データは、次のようになります。

[!code-console[Main](part-4/samples/sample6.cmd)]

トレース ログは、EF が書籍と著者のテーブルの結合を実行することを示します。

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>遅延読み込み

遅延読み込み、EF では、そのエンティティのナビゲーション プロパティが逆参照されるときに、関連エンティティが自動的に読み込みます。 遅延読み込みを有効にするには、仮想ナビゲーション プロパティを確認します。 たとえば、Book クラス: で

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

次のコードについて考えてみましょう。

[!code-csharp[Main](part-4/samples/sample9.cs)]

遅延読み込みを有効にするへのアクセス、`Author`プロパティを`books[0]`作成者のデータベースのクエリに EF を発生します。

遅延読み込みでは、EF は、関連エンティティを取得するたびにクエリを送信するために、データベースの複数のトリップが必要です。 一般に、遅延読み込みのシリアル化するオブジェクトを無効にします。 シリアライザーは、すべての関連エンティティの読み込みをトリガーすると、モデルのプロパティを読み取る必要があります。 EF は、遅延読み込みを有効になっている書籍の一覧をシリアル化時に、SQL クエリをここではたとえばです。 EF では、次の 3 つの分離したクエリを次の 3 つの作成者に確認できます。

[!code-console[Main](part-4/samples/sample10.sql)]

遅延読み込みを使用する場合は引き続きがあります。 一括読み込みでは、EF が非常に複雑な結合を生成する可能性があります。 または、関連エンティティは、データの小さなサブセットにする必要があり、遅延読み込みを効率的になります。

シリアル化の問題を回避する方法の 1 つでは、エンティティ オブジェクトではなく、データ転送オブジェクト (Dto) をシリアル化します。 記事の後半では、この方法を紹介します。

### <a name="explicit-loading"></a>明示的な読み込み

はコードで明示的に関連するデータを取得する点を除いては、明示的読み込みは、遅延読み込みに似ていますナビゲーション プロパティにアクセスするときに自動的に発生しません。 明示的読み込みは、関連データを読み込むときに細かく制御できますが、余分なコードが必要です。 明示的な読み込みの詳細については、次を参照してください。[関連エンティティの読み込み](https://msdn.microsoft.com/data/jj574232#explicit)します。

## <a name="navigation-properties-and-circular-references"></a>ナビゲーション プロパティと循環参照

ナビゲーション プロパティを定義した書籍と著者のモデルを定義したとき、 `Book` Book の Author リレーションシップ クラスが逆方向のナビゲーション プロパティを定義するでした。

対応するナビゲーション プロパティを追加する場合の結果、`Author`クラスか?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

残念ながら、モデルをシリアル化するときに問題が発生します。 関連データを読み込む場合は、循環オブジェクト グラフを作成します。

![](part-4/_static/image1.png)

JSON または XML フォーマッタは、グラフをシリアル化する際に、例外がスローされます。 2 つのフォーマッタは、別の例外メッセージをスローします。 JSON フォーマッタの例を次に示します。

[!code-console[Main](part-4/samples/sample12.cmd)]

XML フォーマッタを次に示します。

[!code-xml[Main](part-4/samples/sample13.xml)]

1 つのソリューションでは、次のセクションで説明する Dto を使用します。 また、グラフのサイクルの対応する JSON および XML フォーマッタを構成できます。 詳細については、次を参照してください。[循環オブジェクト参照の処理](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)します。

このチュートリアルで必要はありません、`Author.Book`ナビゲーション プロパティ、ため残しておくことができます。

> [!div class="step-by-step"]
> [前へ](part-3.md)
> [次へ](part-5.md)
