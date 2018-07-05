---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: アニメーション コントロールに追加する (VB) |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 このチュートリアルではどのようにしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ae2fd6c680ed89022772c62bb6148808d2f4daf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818127"
---
<a name="adding-animation-to-a-control-vb"></a>アニメーション コントロールに追加する (VB)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 このチュートリアルでは、このようなアニメーションを設定する方法を示します。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 このチュートリアルでは、このようなアニメーションを設定する方法を示します。

## <a name="steps"></a>手順

最初の手順は、通常どおりに含める、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、Control Toolkit を使用できるようにします。

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

このシナリオでは、アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラスは、背景色と幅を定義します。

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

次に、必要があります、`AnimationExtender`します。 指定したら、`ID`と、通常`runat="server"`、`TargetControlID`属性は、ここでは、パネルをアニメーション化するコントロールに設定する必要があります。

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

全体のアニメーションが適用されるは、残念ながら Visual Studio の IntelliSense によって完全には現在サポート、XML の構文を使用して宣言します。 ルート ノードは`<Animations>;`と場所の take(s) 単位を決定します。 このノード内で複数のイベントが許可されます。

- `OnClick` (マウス クリック)
- `OnHoverOut` (マウスから離したときにコントロールを)
- `OnHoverOver` (コントロールの上にマウスを重ねると停止、`OnHoverOut`アニメーション)
- `OnLoad` (ページの読み込み時)
- `OnMouseOut` (マウスから離したときにコントロールを)
- `OnMouseOver` (停止しない、コントロールの上にマウスを重ねると、`OnMouseOut`アニメーション)

フレームワークのアニメーション、独自の XML 要素によって表されるそれぞれのセットが付属します。 選択範囲を次に示します。

- `<Color>` (色を変更する)
- `<FadeIn>` (フェードイン)
- `<FadeOut>` (フェードアウト)
- `<Property>` (コントロールのプロパティを変更する)
- `<Pulse>` (いた)
- `<Resize>` (サイズを変更する)
- `<Scale>` (それに比例してサイズを変更する)

この例で、パネルをフェードアウトものとします。アニメーションが 1.5 秒かかるものと (`Duration`属性)、1 秒あたり 24 フレーム (アニメーション手順) を表示する (`Fps` attributs)。 完全なマークアップを次に示します、`AnimationExtender`コントロール。

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

このスクリプトを実行すると、パネルが表示され、1.5 秒にフェードアウトします。


[![パネルをフェードアウトします。](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

パネルをフェードアウト ([フルサイズの画像を表示する をクリックします](adding-animation-to-a-control-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](dynamically-controlling-updatepanel-animations-cs.md)
> [次へ](executing-several-animations-at-the-same-time-vb.md)
