---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: "複数のアニメーションを実行する、同時に (VB) |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 これによりに落としたを実行しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 8461f5ea303a9e1166f694d4039d4c1aedd1caa8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-at-the-same-time-vb"></a>(VB) 同時に複数のアニメーションを実行します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 並列的に複数のアニメーションを実行できます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 並列的に複数のアニメーションを実行できます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 一般に、`<OnLoad>`使用できるアニメーションの 1 つだけです。 アニメーションのフレームワークでは、1 つを使用してにいくつかのアニメーションに参加することができます、`<Parallel>`要素。 内のすべてのアニメーション`<Parallel>`同時に実行されます。

ここでは、可能なマークアップを`AnimationExtender`フェードアウトと同時に、パネルのサイズを変更するときに、コントロール。

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

実際に: このスクリプトを実行すると、パネル が表示されたら、サイズ変更 (threefolding より幅と halfing height)、同時にオンにするとします。


[![パネルがフェードアウトと、(ブラウザーのレンダリング エンジンにより、そのコンテンツを含む) のサイズ変更](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

パネルがフェードアウトと、(ブラウザーのレンダリング エンジンにより、そのコンテンツを含む) のサイズ変更 ([フルサイズのイメージを表示するをクリックして](executing-several-animations-at-the-same-time-vb/_static/image3.png))

>[!div class="step-by-step"]
[前へ](adding-animation-to-a-control-vb.md)
[次へ](executing-several-animations-after-each-other-vb.md)
