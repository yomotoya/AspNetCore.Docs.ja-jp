---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: 評価コントロール (VB) を作成する |Microsoft Docs
author: wenz
description: 多くの web サイトから e コマース、コミュニティ サイトにユーザーは、レート記事や項目を提供します。 これは、通常コーディング上の工夫が必要ですが、ので、.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b229d0155fbab0437c644b41424164e4c655f5e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831322"
---
<a name="creating-a-rating-control-vb"></a>評価コントロール (VB) を作成します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> 多くの web サイトから e コマース、コミュニティ サイトにユーザーは、レート記事や項目を提供します。 通常はコーディング上の工夫が必要ですが、Control Toolkit、破棄する必要は。


## <a name="overview"></a>概要

多くの web サイトから e コマース、コミュニティ サイトにユーザーは、レート記事や項目を提供します。 通常はコーディング上の工夫が必要ですが、Control Toolkit、破棄する必要は。

## <a name="steps"></a>手順

最初に、すべての必要な (少なくとも) 2 つのイメージ: 塗りつぶしのいずれかの項目の評価、および評価が空の項目に 1 つ。 評価の項目は、通常、星または、スマイリーです。 このシナリオでは、このチュートリアルでは、ソース コードのダウンロードの一部として 3 つのファイル、smiley.png と empty.png およびスマイリー done.png を検索します。

次に、新しい ASP.NET ファイルを作成し、追加の開始、`ScriptManager`コントロールを。

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

次に、追加、 `Rating` ASP.NET AJAX Control toolkit コントロール。 次の属性は、この例で設定する必要があります。

- `CurrentRating` 使用する初期の評価
- `MaxRating` 最大年齢区分
- `EmptyStarCssClass` 評価のアイテム (star) は空にするときに使用する CSS クラス
- `FilledStarCssClass` 評価の項目 (star) は、入力時に使用する CSS クラス
- `StarCssClass` 表示される統計に使用する CSS クラス
- `WaitingStarCssClass` 星の評価がサーバーに送信されるときに使用する CSS クラス

5 つで評価コントロールが作成されるマークアップは、次を [なし] が記入されます最初の項目 (スマイリー)。

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

参照されている 3 つの CSS クラス必要があります、適切なイメージ ファイルを表示するのには簡単に CSS を使用して実行できます。

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

3 つのイメージの高さと幅を指定することは、それ以外の場合、表示を少し messed なります確認します。

最後に、評価の結果する必要がある、ユーザーに表示されます (または、少なくともデータベースに保存)。 その評価フォームをサーバーにポストバックするためには、テキスト メッセージと送信ボタンの出力のラベルを追加します。

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

サーバー側のコードでアクセスを使用して評価コントロール、`ID`し、アクセス、 `CurrentRating` 0 ~ 5 の値の例では、評価を選択した項目の数であるプロパティ。

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

ページを保存し、お使いのブラウザーに読み込みます。 (最初は空) の評価の項目をポイントすると、JavaScript の効果が発生します。 評価の変更。 一連の星をクリックすると、現在の評価が保持されます。 最後に、フォームを送信するときに、サーバー側コードは、選択した評価を出力します。


[![最小限のコードで評価システムを作成します。](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

最小限のコードで評価システムを作成する ([フルサイズの画像を表示する をクリックします](creating-a-rating-control-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](creating-a-rating-control-cs.md)
