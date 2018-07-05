---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: 複数のアニメーションを実行すると同時に (c#) |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 これにより、落としたを実行する.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 017a37533ab055ad149cf0de3a5892ae92741b84
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816851"
---
<a name="executing-several-animations-at-the-same-time-c"></a>(C#) と同時に複数のアニメーションを実行
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 並列的に複数のアニメーションを実行できます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 並列的に複数のアニメーションを実行できます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

次のようなテキストのパネルに、アニメーションが適用されます。

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 一般に、`<OnLoad>`アニメーションを 1 つのみを受け入れます。 アニメーション フレームワークを使用すると、複数のアニメーションを使用して 1 つに参加させる、`<Parallel>`要素。 内のすべてのアニメーション`<Parallel>`と同時に実行されます。

ここでは、可能なマークアップを`AnimationExtender`フェードアウトし、同時に、パネルのサイズを変更するときに、コントロール。

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

実際: パネルが表示されたら、サイズが変更されますこのスクリプトを実行すると (threefolding よりも詳細の幅と halfing 高さ) と同時にフェードアウトします。


[![パネルをフェードアウトし、(ブラウザーのレンダリング エンジンに協力してくれた、そのコンテンツを含む) のサイズ変更](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

パネルをフェードアウトし、(ブラウザーのレンダリング エンジンに協力してくれた、そのコンテンツを含む) のサイズ変更 ([フルサイズの画像を表示する をクリックします](executing-several-animations-at-the-same-time-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](adding-animation-to-a-control-cs.md)
> [次へ](executing-several-animations-after-each-other-cs.md)
