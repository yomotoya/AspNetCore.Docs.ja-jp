---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: 複数のアニメーションを実行する (c#) 相互後 |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 これによりに落としたを実行しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 836f0bba890a03e74ae62c2df029b7525b34275c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871409"
---
<a name="executing-several-animations-after-each-other-c"></a>他の (c#) の後にいくつかのアニメーションを実行します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 これにより、後に、他のいくつかのアニメーションの 1 つを実行できます。


アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 これにより、後に、他のいくつかのアニメーションの 1 つを実行できます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意 `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 一般に、`<OnLoad>`使用できるアニメーションの 1 つだけです。 アニメーションのフレームワークでは、1 つを使用してにいくつかのアニメーションに参加することができます、`<Sequence>`要素。 内のすべてのアニメーション`<Sequence>`後にもう 1 つずつ実行されます。 ここでは、可能なマークアップを`AnimationExtender`コントロール パネルの幅を作成して、高さを小さきます。

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

実行すると、パネルは、このスクリプトも縦としより小さい最初を取得します。


[![最初の幅が増加しました](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

最初の幅が増加しました ([フルサイズのイメージを表示するをクリックして](executing-several-animations-after-each-other-cs/_static/image3.png))


[![高さの減少し、](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

高さの減少し、([フルサイズのイメージを表示するをクリックして](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [前へ](executing-several-animations-at-the-same-time-cs.md)
> [次へ](animation-depending-on-a-condition-cs.md)
