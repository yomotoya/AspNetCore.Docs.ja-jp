---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: 条件 (VB) に基づくアニメーション |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションがかどうか.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: be43b68ce77819cdcd09d8e875604db90e8a8d96
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830339"
---
<a name="animation-depending-on-a-condition-vb"></a>条件 (VB) に基づくアニメーション
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションを実行するかどうかはできるいくつかの JavaScript コードの形式での条件によっても異なります。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションを実行するかどうかはできるいくつかの JavaScript コードの形式での条件によっても異なります。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

次のようなテキストのパネルに、アニメーションが適用されます。

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 通常のアニメーションの 1 つではなく、`<Condition>`要素が関与します。 値として提供される JavaScript コード、`ConditionScript`属性は、実行時に実行されます。 場合は true と評価、アニメーションは実行、それ以外の場合。 次のマークアップは、それぞれのケースをランダムの 50% で実行されている 2 つのアニメーションを提供します。 のみがあるので内の 1 つのアニメーション`<OnLoad>`、2 つ`<Condition>`を使用してアニメーションが参加している、`<Sequence>`要素。

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

なお、小なり記号 (`<`) で、`ConditionScript`属性は () をエスケープする必要があります。 ときにないアニメーション実行される、このスクリプトを実行するまたは、2 つのいずれかまたは両方の操作を行います。


[![パネルがフェードアウト、サイズを変更せず 1 つ目、2 番目のアニメーション実行していないため](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

パネルがフェードアウト、サイズを変更せず 1 つ目、2 番目のアニメーション実行していないため ([フルサイズの画像を表示する をクリックします](animation-depending-on-a-condition-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](executing-several-animations-after-each-other-vb.md)
> [次へ](picking-one-animation-out-of-a-list-vb.md)
