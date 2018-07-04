---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: UpdatePanel コントロール (VB) をアニメーション化 |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 内容として、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5f14a3297c2f3ddd6b0817cc8e46ac23b3a06c0b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369717"
---
<a name="animating-an-updatepanel-control-vb"></a>UpdatePanel コントロールをアニメーション (VB)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 UpdatePanel の内容は、特別なエクステンダーが存在アニメーション フレームワークに大きく依存している: UpdatePanelAnimation します。 このチュートリアルでは、UpdatePanel のようなアニメーションを設定する方法を示します。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 内容として、 `UpdatePanel`、アニメーション フレームワークに大きく依存している特別なエクステンダーが存在します:`UpdatePanelAnimation`します。 このチュートリアルのようなアニメーションを設定する方法を示しています、`UpdatePanel`します。

## <a name="steps"></a>手順

最初の手順は、通常どおりに含める、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、Control Toolkit を使用できるようにします。

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

このシナリオでは、アニメーションは、ASP.NET に適用される`Wizard`内に存在する web コントロール、`UpdatePanel`します。 (任意) の 3 つの手順では、ポストバックをトリガーするための十分なオプションを提供します。

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

マークアップのために必要な`UpdatePanelAnimationExtender`コントロールを使用するマークアップを非常に似ていますが、`AnimationExtender`します。 `TargetControlID`属性を提供しています、`ID`の`UpdatePanel`; をアニメーション化する内で、`UpdatePanelAnimationExtender`コントロール、`<Animations>`要素は、単位の XML マークアップを保持します。 1 つの違い: イベントとイベント ハンドラーの量が比較する限られた`AnimationExtender`します。 `UpdatePanels`、2 つだけに存在します。

- `<OnUpdated>` UpdatePanel が更新されたとき
- `<OnUpdating>` UpdatePanel が更新を開始する場合

このシナリオでは、新しいコンテンツでは、 `UpdatePanel` (ポストバック) の後、フェードインものとします。 これは、そのために必要なマークアップです。

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

これで、ポストバック、UpdatePanel 内で発生するたびに、パネルの新しい内容はスムーズ フェードインします。


[![ウィザードの次の手順がフェードインします。](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

ウィザードの次の手順がフェードイン ([フルサイズの画像を表示する をクリックします](animating-an-updatepanel-control-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](changing-an-animation-using-client-side-code-vb.md)
> [次へ](dynamically-controlling-updatepanel-animations-vb.md)
