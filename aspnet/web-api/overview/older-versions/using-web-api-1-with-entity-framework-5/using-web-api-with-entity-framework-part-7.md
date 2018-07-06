---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'パート 7: 作成、メイン ページ |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: c5b6cb0f2e48cdea3d6cc5cde72d08a99126f05f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825636"
---
<a name="part-7-creating-the-main-page"></a>パート 7: 作成、メイン ページ
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>メインの作成 ページ

このセクションでは、アプリケーションのメイン ページを作成します。 このページは、いくつかの手順にアプローチしましょうように管理者 ページより複雑になります。 その過程では、高度な Knockout.js テクニックを確認します。 ページの基本的なレイアウトを次に示します。

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- 「製品」は、製品の配列を保持します。
- 「カート」は、製品と数量の配列を保持します。 カートの内容を更新"Add to Cart"をクリックします。
- "Orders"は、注文 Id の配列を保持します。
- 「詳細」(数量と製品) の項目の配列は、注文の詳細を保持します。

HTML では、データ バインディングやスクリプトではないいくつかの基本的なレイアウトを定義することで始めます。 Views/Home/Index.cshtml ファイルを開き、次のようにすべての内容を置き換えます。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

次に、スクリプトのセクションを追加し、空のビュー モデルを作成します。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

先ほどスケッチ設計に基づき、このビュー モデルが必要オブザーバブル製品、カート、注文、および詳細です。 次の変数を追加して、`AppViewModel`オブジェクト。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

ユーザーでは、製品のリストから、カートにアイテムを追加でき、カートから項目を削除することができます。 これらの関数をカプセル化するには、成果物を表す別のビュー モデル クラスを作成します。 `AppViewModel` に次のコードを追加します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel`クラスにはと、カートから製品の移動に使用される 2 つの関数が含まれています: `addItemToCart` 、カートに製品の 1 つのユニットを追加し、`removeAllFromCart`製品のすべての数量を削除します。

ユーザーは、既存の注文を選択し、注文の詳細を取得します。 この機能を別のビュー モデルにカプセル化します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel`注文を使用して初期化をサーバーに、AJAX 要求を送信することによって、注文の詳細をフェッチします。

また、`total`プロパティを`OrderDetailsViewModel`します。 このプロパティは、オブザーバブルと呼ばれる特殊な[観測可能なオブジェクトの計算](http://knockoutjs.com/documentation/computedObservables.html)します。 名前が示すように、計算された観測可能なオブジェクトを使用する計算値にデータをバインド&#8212;ここでは、注文のコストの合計。

これらの関数を次に、追加`AppViewModel`:

- `resetCart` カートの内容からすべての項目を削除します。
- `getDetails` 注文の詳細を取得します (新しい pusing によって`OrderDetailsViewModel`上に、`details`リスト)。
- `createOrder` 新しい注文を作成し、カートの内容を空にします。


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

最後に、製品と注文の AJAX 要求を行って、ビュー モデルを初期化します。

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

そうは多くのコードが構築しましたを順を追って、ことを願っています設計はクリアされます。 HTML に一部の Knockout.js バインドを追加できます。

**製品**

製品の一覧については、バインドを次に示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

これにより、製品の配列を反復処理し、名前と価格を表示します。 ユーザーがログインされる場合にのみ、"Order に追加"ボタンが表示されます。

"Order に追加"ボタン呼び出し`addItemToCart`上、`ProductViewModel`製品のインスタンス。 これは、Knockout.js の優れた機能を示して: ビュー モデルにその他のビュー モデルが含まれている場合は、内部のモデルに、バインドを適用できます。 この例では、内のバインディングでは、`foreach`のそれぞれに適用される、`ProductViewModel`インスタンス。 この方法は、1 つのビュー モデルを配置することで、すべての機能よりもはるかに簡潔です。

**カート**

カートのバインドを次に示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

これにより、カートの配列を反復処理し、名前、価格、および数量を表示します。 ビュー モデルの関数にリンクの「削除」と"Order の作成"ボタンがバインドされていることに注意してください。

**注文**

注文の一覧については、バインドを次に示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

これは、注文を反復処理し、注文 ID を示しています。 リンクをクリック イベントのバインド先、`getDetails`関数。

**注文の詳細**

注文の詳細のバインディングを次に示します。

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

これは、順序内の項目を反復処理し、製品、価格、数量が表示されます。 周囲の div は、詳細の配列には、1 つまたは複数の項目が含まれている場合にのみ表示されます。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、データベース、およびデータ層の上にパブリックに公開されたインターフェイスを提供する ASP.NET Web API と通信するために Entity Framework を使用するアプリケーションを作成します。 ASP.NET MVC 4 ページの再読み込みなしの動的な対話を提供するのに HTML ページ、および Knockout.js と jQuery を表示するために使用します。

その他のリソース:

- [ASP.NET データ アクセス コンテンツ マップ](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework デベロッパー センター](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-6.md)
