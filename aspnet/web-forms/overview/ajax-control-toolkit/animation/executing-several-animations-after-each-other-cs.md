---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: 複数のアニメーションを実行した後 (c#) 相互 |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 これにより、落としたを実行する.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 2317a029d4b12ba66d2d06e5012bb0bf892086a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807604"
---
<a name="executing-several-animations-after-each-other-c"></a>(C#) の後に複数のアニメーションを実行
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 後、その他の複数のアニメーションを 1 つを実行できます。


アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 後、その他の複数のアニメーションを 1 つを実行できます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

次のようなテキストのパネルに、アニメーションが適用されます。

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 一般に、`<OnLoad>`アニメーションを 1 つのみを受け入れます。 アニメーション フレームワークを使用すると、複数のアニメーションを使用して 1 つに参加させる、`<Sequence>`要素。 内のすべてのアニメーション`<Sequence>`後にもう 1 つずつ実行されます。 ここでは、可能なマークアップを`AnimationExtender`コントロール、パネルの幅を作成して、高さを大ききます。

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

ときに最初の取得も縦と横が小さくし、パネル、このスクリプトを実行します。


[![まず、幅が増加します。](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

まず、幅が増加 ([フルサイズの画像を表示する をクリックします](executing-several-animations-after-each-other-cs/_static/image3.png))。


[![高さの減少し、](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

高さの減少し、([フルサイズの画像を表示する をクリックします](executing-several-animations-after-each-other-cs/_static/image6.png))。

> [!div class="step-by-step"]
> [前へ](executing-several-animations-at-the-same-time-cs.md)
> [次へ](animation-depending-on-a-condition-cs.md)
