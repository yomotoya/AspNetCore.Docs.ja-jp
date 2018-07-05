---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: UpdatePanel アニメーション (VB) を動的に制御する |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 内容として、.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: b93ed1c7994c7561396298d876af213ae872f787
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803656"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>UpdatePanel アニメーション (VB) を動的に制御します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 UpdatePanel の内容は、特別なエクステンダーが存在アニメーション フレームワークに大きく依存している: UpdatePanelAnimation します。 これは UpdatePanel トリガーと共にも使用できます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 内容として、 `UpdatePanel`、アニメーション フレームワークに大きく依存している特別なエクステンダーが存在します:`UpdatePanelAnimation`します。 でも機能と共に`UpdatePanel`トリガーします。

## <a name="steps"></a>手順

最初の手順は、通常どおりに含める、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、Control Toolkit を使用できるようにします。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

このシナリオでは、アニメーションは、現在の時刻の表示に適用されます。 この情報を使用して、ラベルに書き込まれることができます、`Page_Load()`メソッド、または次のインライン コードの使用 (わかりやすくする)。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

また、時間の更新をトリガーするボタンが作成されます。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

このコードに配置し、`<ContentTemplate>`のセクション、`UpdatePanel`要素。 パネルの`UpdateMode`に属性を設定する必要があります`"Conditional"`、トリガーのみがパネルの内容を更新します。 `<Triggers>`のセクション、 `UpdatePanel`、非同期ポストバックのトリガーが作成されに関連付けられている、`Click`ボタンのイベント。 したがって、ユーザーが、ボタンをクリックした場合、`UpdatePanel`が更新されます。 次のマークアップに示します、`UpdatePanel`コントロール。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

最後に、`UpdatePanelAnimationExtender`構成する必要があります。 設定、 `TargetControlID` 、パネルの id 属性と、エクステンダー内でアニメーションを定義します。 フェードはある意味、更新された時間に優れた visual 重点を置いたを作成します。 エクステンダーのマークアップには可能性がありますし、このようになります。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

ブラウザーでファイルを実行します。 ボタンをクリックしたときに、常に 1 秒の間フェードのパネルで、現在の時刻が表示されます。


[![現在の時刻がフェードインします。](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

現在の時刻がフェードイン ([フルサイズの画像を表示する をクリックします](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](animating-an-updatepanel-control-vb.md)
