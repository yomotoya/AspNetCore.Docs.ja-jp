---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: 条件 (VB) によってアニメーション |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションがかどうか.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a648ff8299c9720e9f34522f271595ab1b9bc9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868153"
---
<a name="animation-depending-on-a-condition-vb"></a>条件 (VB) によってアニメーション
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 JavaScript コードの形式での条件には、アニメーションを実行するかどうかは依存できますも。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 JavaScript コードの形式での条件には、アニメーションを実行するかどうかは依存できますも。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意 `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。 正規のアニメーションでは、いずれかではなく、`<Condition>`要素が関係します。 値として提供されている JavaScript コード、`ConditionScript`属性は、ランタイムで実行します。 場合は true と評価、アニメーション実行される、それ以外の場合。 次のマークアップでは、それぞれの時にランダムなケースの 50% で実行されている 2 つのアニメーションを提供します。 のみがあるので内の 1 つのアニメーション`<OnLoad>`、2 つ`<Condition>`アニメーションで相互に参加している、`<Sequence>`要素。

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

なおが低い方 (`<`) で、`ConditionScript`属性は () をエスケープする必要があります。 ときにかないアニメーションの実行は、このスクリプトを実行するかは、2 つのいずれかのか。


[![パネルがフェードアウト、サイズを変更せずため、最初の 1 つ、2 番目のアニメーション実行しませんでした。](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

パネルがフェードアウト サイズを変更することがなく 1 つ目、2 番目のアニメーション実行していないため ([フルサイズのイメージを表示するをクリックして](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](executing-several-animations-after-each-other-vb.md)
> [次へ](picking-one-animation-out-of-a-list-vb.md)
