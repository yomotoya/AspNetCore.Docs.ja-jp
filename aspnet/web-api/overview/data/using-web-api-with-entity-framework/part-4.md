---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: エンティティのリレーションシップの処理 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872690"
---
<a name="handling-entity-relations"></a>処理のエンティティ関係
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、EF が、関連するエンティティを読み込む方法と、モデル クラスでの循環のナビゲーション プロパティを処理する方法の詳細の一部について説明します。 (このセクションでは、背景知識を提供し、チュートリアルを完了する必要はありません。 場合は、スキップ[パート 5](part-5.md)。)。

## <a name="eager-loading-versus-lazy-loading"></a>Eager 遅延読み込みと読み込み

EF とリレーショナル データベースを使用する場合は、EF が関連するデータを読み込む方法を理解する必要があります。

EF を生成する SQL クエリを確認するのに便利です。 SQL をトレースするには、次のコードの行を追加、`BookServiceContext`コンス トラクター。

[!code-csharp[Main](part-4/samples/sample1.cs)]

/Api/books に GET 要求を送信する場合は、次のような JSON が返されます。

[!code-console[Main](part-4/samples/sample2.cmd)]

ブックが有効な著者の Id を含んでいるにもかかわらず Author プロパティが null であることがわかります。 EF が関連の作成者エンティティを読み込んでいないためにです。 SQL クエリのトレース ログは、これを確認します。

[!code-console[Main](part-4/samples/sample3.sql)]

SELECT ステートメントでは、ブックのテーブルから受け取るし、作成者の表を参照していません。

リファレンスについては、メソッドでは、ここで、`BooksController`書籍の一覧を表すクラス。

[!code-csharp[Main](part-4/samples/sample4.cs)]

方法の作成者は、JSON データの一部として返すことができますを見てみましょう。 Entity Framework に関連するデータを読み込む 3 つの方法: 一括読み込み、遅延読み込み、および明示的な読み込みします。 各手法でのトレードオフがあるため、これらの動作を理解することが重要です。

### <a name="eager-loading"></a>一括読み込み

*一括読み込み*EF は、データベースの最初のクエリの一部として関連エンティティを読み込みます。 一括読み込みを実行するのには、使用、 **System.Data.Entity.Include**拡張メソッド。

[!code-csharp[Main](part-4/samples/sample5.cs)]

これは、クエリに含めるデータを作成、EF を示しています。 この変更を行うし、アプリを実行すると場合、JSON データは、次のようになります。

[!code-console[Main](part-4/samples/sample6.cmd)]

トレース ログは、EF が Book と作成者のテーブルに結合を実行することを示します。

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>遅延読み込み

遅延読み込みで、EF では、そのエンティティのナビゲーション プロパティが逆参照と関連するエンティティが自動的に読み込みます。 遅延読み込みを有効にするには、仮想ナビゲーション プロパティを確認します。 たとえば、Book クラスで。

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

次のコードについて考えてみましょう。

[!code-csharp[Main](part-4/samples/sample9.cs)]

遅延読み込みを有効にするへのアクセス、`Author`プロパティ`books[0]`EF 作成者のデータベースを照会すると、します。

遅延読み込みでは、EF は関連エンティティを取得するたびにクエリを送信するために、データベースの複数のトリップが必要です。 一般に、遅延読み込みをシリアル化するオブジェクトを無効にします。 シリアライザーでは、すべてのプロパティを関連エンティティの読み込みをトリガーすると、モデルの読み取り。 EF 遅延読み込みで有効になっている書籍の一覧をシリアル化時に、SQL クエリをここではなどです。 EF が 3 つの作成者の 3 つの独立したクエリを行うことを確認できます。

[!code-console[Main](part-4/samples/sample10.sql)]

遅延読み込みを使用する場合はまだあります. 一括読み込みでは、EF 非常に複雑な結合を生成する可能性があります。 または、関連エンティティは、データの小さなサブセットにする必要があり、遅延読み込みを効率的になります。

シリアル化の問題を回避する方法の 1 つのエンティティ オブジェクトの代わりにデータ転送オブジェクト (Dto) をシリアル化を開始します。 記事の後半でこの方法が紹介します。

### <a name="explicit-loading"></a>明示的な読み込み

明示的な読み込み似ていますが、遅延読み込みをコードで明示的に、関連するデータを取得します。ナビゲーション プロパティにアクセスするときに自動的に発生しません。 明示的な読み込みでは、詳細な制御を関連するデータを読み込むときに利用できますが、余分なコードが必要です。 明示的な読み込みの詳細については、次を参照してください。[関連エンティティの読み込み](https://msdn.microsoft.com/data/jj574232#explicit)です。

## <a name="navigation-properties-and-circular-references"></a>ナビゲーション プロパティと循環参照

ナビゲーション プロパティを定義した Book と作成者のモデルを定義したときに、`Book`ブックの作成者のリレーションシップのクラスが、逆方向にナビゲーション プロパティが定義されてはいなかった。

対応するナビゲーション プロパティを追加する場合の結果、`Author`クラスしますか?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

残念ながら、モデルをシリアル化する際に問題が発生します。 関連するデータを読み込む場合は、オブジェクトの循環グラフが作成されます。

![](part-4/_static/image1.png)

JSON または XML フォーマッタは、グラフをシリアル化する際に、例外がスローされます。 2 つのフォーマッタは、別の例外のメッセージをスローします。 JSON のフォーマッタの使用例を次に示します。

[!code-console[Main](part-4/samples/sample12.cmd)]

XML フォーマッタを次に示します。

[!code-xml[Main](part-4/samples/sample13.xml)]

1 つのソリューションでは、次のセクションで説明する DTOs を使用します。 また、グラフのサイクルを処理する JSON および XML フォーマッタを構成できます。 詳細については、次を参照してください。[循環オブジェクト参照の処理](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)です。

このチュートリアルでは、する必要はありません、`Author.Book`ナビゲーション プロパティ、ようにおくことができます。

> [!div class="step-by-step"]
> [前へ](part-3.md)
> [次へ](part-5.md)
