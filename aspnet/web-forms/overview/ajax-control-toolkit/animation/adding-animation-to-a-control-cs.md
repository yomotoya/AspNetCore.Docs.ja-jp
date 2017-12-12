---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: "アニメーションをコントロール (c#) に追加する |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 このチュートリアルではどのようにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7016ae3c92c665136579a8588818e6e4179a102a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-c"></a>アニメーションをコントロール (c#) に追加します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 このチュートリアルでは、このようなアニメーションを設定する方法を示します。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 このチュートリアルでは、このようなアニメーションを設定する方法を示します。

## <a name="steps"></a>手順

含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

このシナリオでのアニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラスには、背景色と幅を定義します。

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

次に、必要があります、`AnimationExtender`です。 入力した後、 `ID` 、通常、 `runat="server"`、`TargetControlID`属性は、ここでは、パネル アニメーション化するコントロールに設定する必要があります。

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

全体のアニメーションが適用されたは、残念ながら完全には現在サポートされている Visual Studio の IntelliSense によって、XML 構文を使用して宣言します。 ルート ノードは`<Animations>;`アニメーションが場所を take(s) ときを特定するこのノード内で複数のイベントが許可されます。

- `OnClick`(マウス クリック)
- `OnHoverOut`(ときにマウスがコントロールを離れる)
- `OnHoverOver`(マウスがコントロール上に置いたときに、停止、`OnHoverOut`アニメーション)
- `OnLoad`(ページの読み込み時)
- `OnMouseOut`(ときにマウスがコントロールを離れる)
- `OnMouseOver`(マウスがコントロール上に置いたときに停止はありません、`OnMouseOut`アニメーション)

フレームワークは、アニメーションでは、独自の XML 要素によって表される各 1 つのセットが付属します。 選択範囲を次に示します。

- `<Color>`(色を変更する)
- `<FadeIn>`(フェードイン)
- `<FadeOut>`(フェードアウト)
- `<Property>`(コントロールのプロパティを変更する)
- `<Pulse>`(いた)
- `<Resize>`(サイズを変更する)
- `<Scale>`(サイズを比例的に変更する)

この例では、パネルはフェードアウトします。アニメーションは 1.5 秒を受け取ります (`Duration`属性)、24 (アニメーション手順) 秒あたりのフレームを表示する (`Fps` attributs)。 ここでは、完全なマークアップを`AnimationExtender`コントロール。

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

このスクリプトを実行すると、パネルが表示され、1.5 秒単位でフェードアウトします。


[![パネルがフェードアウトします。](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

パネルがフェードアウト ([フルサイズのイメージを表示するをクリックして](adding-animation-to-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[次へ](executing-several-animations-at-the-same-time-cs.md)
