---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: "ユーザーの操作 (c#) への応答でのアニメーション化 |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションがスターできます."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: efb9c34c317ec56b43c498f40a857a9b47fa50b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-c"></a>ユーザーの操作 (c#) への応答をアニメーション化します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションは、自動的に開始できます。 または、マウスでクリックなど、ユーザーとの対話によってトリガー可能性があります。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションは、自動的に開始できます。 または、マウスでクリックなど、ユーザーとの対話によってトリガー可能性があります。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

内で、`<Animations>`ノードはユーザーの操作を使用してアニメーションを開始する 5 つの方法があります (不足している要素は`<OnLoad>`ページ全体が完全に読み込まれた後に実行される)。

- `<OnClick>`(マウスでクリック コントロール)
- `<OnHoverOut>`(マウスがコントロールを離れる)
- `<OnHoverOver>`(マウスが停止する、コントロールの上、`<OnHoverOut>`アニメーション)
- `<OnMouseOut>`(マウスがコントロールを離れる)
- `<OnMouseOver>`(マウスがないを停止する、コントロールの上、`<OnMouseOut>`アニメーション)

このシナリオで`<OnClick>`を使用します。 パネルにユーザーがクリックすると、サイズが変更され、同時にフェードアウトします。

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![マウスのクリック、アニメーションを開始します。](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

マウスのクリックがアニメーションを開始 ([フルサイズのイメージを表示するをクリックして](animating-in-response-to-user-interaction-cs/_static/image3.png))

>[!div class="step-by-step"]
[前へ](picking-one-animation-out-of-a-list-cs.md)
[次へ](disabling-actions-during-animation-cs.md)
