---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: (VB) の一覧からアニメーションを 1 つ選択 |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 フレームワークも許可する.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c60d7cff7c841d23185fbdf07abf0e894b21cf5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835799"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>(VB) の一覧からアニメーションを 1 つ選択します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 フレームワークでは、いくつかの JavaScript コードの評価によって、アニメーションの一覧からアニメーションを 1 つを選択するプログラマもできます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 フレームワークでは、いくつかの JavaScript コードの評価によって、アニメーションの一覧からアニメーションを 1 つを選択するプログラマもできます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

次のようなテキストのパネルに、アニメーションが適用されます。

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 通常のアニメーションの 1 つではなく、`<Case>`要素が関与します。 その SelectScript 属性の値が評価されます。戻り値は数値である必要があります。 内でサブアニメーションのいずれか、この数によっては&lt;ケース&gt;を実行します。 たとえば、SelectScript は 2 に評価されると、Control Toolkit は 内で 3 番目のアニメーションを実行&lt;ケース&gt;(カウント 0 から始まる)。

次のマークアップは次の 3 つのサブアニメーションを定義します。 幅をサイズ変更、高さのサイズを変更およびフェードアウトします。JavaScript コード (`Math.floor(3 * Math.random())`) 3 つのアニメーションのいずれかが実行されるため、0 ~ 2 の数値を取得します。

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![考えられる 3 つのアニメーションのいずれか: パネルが広くなります。](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

考えられる 3 つのアニメーションのいずれか: パネルが広くなる ([フルサイズの画像を表示する をクリックします](picking-one-animation-out-of-a-list-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](animation-depending-on-a-condition-vb.md)
> [次へ](animating-in-response-to-user-interaction-vb.md)
