---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: "UpdatePanel コントロール (c#) をアニメーション化 |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 内容を."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e6d8954d2ec886994cdd723121e540b471131f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-c"></a>UpdatePanel コントロール (c#) をアニメーション化
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 UpdatePanel の内容は、の特別な extender が存在し、アニメーション、フレームワークに大きく依存して: UpdatePanelAnimation です。 このチュートリアルでは、UpdatePanel のようなアニメーションを設定する方法を示します。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 内容を`UpdatePanel`、特別な extender が存在するアニメーション フレームワークに大きく依存している:`UpdatePanelAnimation`です。 このチュートリアルでは、このようなアニメーションを設定する方法、`UpdatePanel`です。

## <a name="steps"></a>手順

含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

このシナリオでのアニメーションが ASP.NET に適用される`Wizard`web コントロール内に存在する、`UpdatePanel`です。 (任意) の 3 つの手順は、ポストバックをトリガーするための十分なオプションを提供します。

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

マークアップのために必要な`UpdatePanelAnimationExtender`コントロールを使用するマークアップを非常に似ていますが、`AnimationExtender`です。 `TargetControlID`属性を指定した、`ID`の`UpdatePanel`; アニメーション化する内で、`UpdatePanelAnimationExtender`コントロール、`<Animations>`要素は、アニメーションの XML マークアップを保持します。 1 つの違い: イベントとイベント ハンドラーの量は、比較する限られた`AnimationExtender`です。 `UpdatePanels`、2 つだけに存在します。

- `<OnUpdated>`UpdatePanel が更新されたとき
- `<OnUpdating>`UpdatePanel が更新を開始する場合

このシナリオでは、新しい内容で、 `UpdatePanel` (ポストバック) の後のフェードインいてはいけない。 必要なマークアップを次に示します。

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

これで、UpdatePanel 内のポストバックが発生すると、パネルの新しい内容がフェードイン スムーズに動作します。


[![ウィザードの次の手順がフェードインします。](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

ウィザードの次の手順がフェードイン ([フルサイズのイメージを表示するをクリックして](animating-an-updatepanel-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[前へ](changing-an-animation-using-client-side-code-cs.md)
[次へ](dynamically-controlling-updatepanel-animations-cs.md)
