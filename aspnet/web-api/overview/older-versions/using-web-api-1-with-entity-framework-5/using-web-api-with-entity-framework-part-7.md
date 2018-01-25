---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: "手順 7: 作成、メイン ページ |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 4d06e72bc664f707bbbe4603be41347158c58903
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="part-7-creating-the-main-page"></a>手順 7: 作成、メイン ページ
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>メインの作成 ページ

このセクションでは、アプリケーションのメイン ページを作成します。 このページは [管理] ページより複雑になるため、いくつかの手順でアプローチしましょう。 その過程より高度な Knockout.js テクニックをいくつかが表示されます。 ページの基本的なレイアウトを次に示します。

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Products"では、製品の配列を保持します。
- "Cart"では、製品と数量の配列を保持します。 "Add to Cart"をクリックすると、カートを更新します。
- "Orders"では、注文 Id の配列を保持します。
- "Details"は、項目 (数量が製品) の配列は、注文の詳細を保持しています。

データ バインディングやスクリプトなしで、HTML では、いくつかの基本的なレイアウトを定義することで始めます。 Views/Home/Index.cshtml ファイルを開き、次のすべての内容に置き換えます。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

次に、スクリプトのセクションを追加し、空のビュー モデルを作成します。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

以前スケッチした設計に基づき、当社ビュー モデル必要があります観測可能なオブジェクトの製品、カート、orders、および詳細。 次の変数を追加して、`AppViewModel`オブジェクト。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

ユーザーでは、製品のリストから、カートにアイテムを追加でき、カートから項目を削除することができます。 これらの関数をカプセル化するには、製品を表す別のビュー モデル クラスを作成します。 `AppViewModel` に次のコードを追加します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel`クラスにして、カートに製品を移動するために使用する 2 つの関数が含まれています: `addItemToCart` 、カートに製品の 1 つの単位を追加し、`removeAllFromCart`製品のすべての数量を削除します。

ユーザーは、既存の注文を選択し、注文の詳細を取得します。 私たちは、別のビュー モデルには、この機能をカプセル化します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel`は順序が初期化されて、サーバーに AJAX 要求を送信することによって、注文の詳細をフェッチとします。

また、`total`プロパティを`OrderDetailsViewModel`です。 このプロパティは、監視対象と呼ばれる特殊な[観測可能なオブジェクトの計算](http://knockoutjs.com/documentation/computedObservables.html)です。 名前が示すように、計算観測可能なオブジェクトできます計算値 &#8212;へのデータ バインド。 この場合、注文のコストの合計。

これらの関数を次に、追加`AppViewModel`:

- `resetCart`カートからすべての項目を削除します。
- `getDetails`注文の詳細を取得 (によって、新しい pusing`OrderDetailsViewModel`上に、`details`リスト)。
- `createOrder`新しい注文を作成し、カートを空にします。


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

最後に、製品と注文の AJAX 要求をすることで、ビュー モデルを初期化します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

[Ok]、多くのコードですが、作成したを詳細な手順を願っていますので設計はクリアされます。 HTML に一部の Knockout.js バインドを追加できます。

**製品**

製品の一覧については、バインディングを次に示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

これにより、製品の配列を反復処理し、名前と価格を表示します。 "Order に追加"ボタンは、ユーザーがログインした場合にのみ表示されます。

"Order に追加"ボタン呼び出し`addItemToCart`上、`ProductViewModel`製品のインスタンス。 Knockout.js の優れた機能を示しますこの: ビュー モデルには、その他のビュー モデルが含まれている、内部のモデルにバインドを適用できます。 この例では、内のバインディングでは、`foreach`のごとに適用されます、`ProductViewModel`インスタンス。 この方法は、1 つのビュー モデルにすべての機能を配置することよりもかなりクリーナーです。

**カート**

次に、カートのバインドを示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

これにより、買い物カゴの配列を反復処理し、名前、価格、および数量を表示します。 削除」のリンクと、"Create Order"ボタンがビュー モデル関数にバインドされていることに注意してください。

**注文**

注文の一覧については、バインディングを次に示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

これは、注文を反復処理し、注文 ID を示しています。 リンクをクリックしてイベントがバインドされている、`getDetails`関数。

**注文の詳細**

次に、注文の詳細のバインドを示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

これは、注文の品目を反復処理し、製品、価格、数量が表示されます。 周囲の div は、詳細配列には、1 つまたは複数の項目が含まれている場合にのみ表示されます。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、データベース、およびデータ層の上に公開されたインターフェイスを提供する ASP.NET Web API と通信するために Entity Framework を使用するアプリケーションを作成しました。 ページの再読み込みせずの動的な対話を提供するのに HTML ページ、および Knockout.js と jQuery を表示するために ASP.NET MVC 4 を使用します。

その他のリソース:

- [ASP.NET データ アクセス コンテンツ マップ](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework デベロッパー センター](https://msdn.microsoft.com/data/ef)

>[!div class="step-by-step"]
[前へ](using-web-api-with-entity-framework-part-6.md)
