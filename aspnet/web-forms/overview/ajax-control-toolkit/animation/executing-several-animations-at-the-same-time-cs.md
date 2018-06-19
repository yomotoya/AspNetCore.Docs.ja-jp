---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: 複数のアニメーションを実行する、同時に (c#) |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 これによりに落としたを実行しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ecd822f7fa00a24e97b9aa14888798825624617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869362"
---
<a name="executing-several-animations-at-the-same-time-c"></a>(C#) 同時に複数のアニメーションを実行します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 並列的に複数のアニメーションを実行できます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 並列的に複数のアニメーションを実行できます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 一般に、`<OnLoad>`使用できるアニメーションの 1 つだけです。 アニメーションのフレームワークでは、1 つを使用してにいくつかのアニメーションに参加することができます、`<Parallel>`要素。 内のすべてのアニメーション`<Parallel>`同時に実行されます。

ここでは、可能なマークアップを`AnimationExtender`フェードアウトと同時に、パネルのサイズを変更するときに、コントロール。

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

実際に: このスクリプトを実行すると、パネル が表示されたら、サイズ変更 (threefolding より幅と halfing height)、同時にオンにするとします。


[![パネルがフェードアウトと、(ブラウザーのレンダリング エンジンにより、そのコンテンツを含む) のサイズ変更](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

パネルがフェードアウトと、(ブラウザーのレンダリング エンジンにより、そのコンテンツを含む) のサイズ変更 ([フルサイズのイメージを表示するをクリックして](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](adding-animation-to-a-control-cs.md)
> [次へ](executing-several-animations-after-each-other-cs.md)
