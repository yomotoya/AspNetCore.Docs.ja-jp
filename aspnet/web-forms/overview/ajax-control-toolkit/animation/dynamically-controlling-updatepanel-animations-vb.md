---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: "UpdatePanel アニメーション (VB) を動的に制御する |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 内容を."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b1f169724d8d3f8bcbc46198d320eeb8e7260c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>UpdatePanel アニメーション (VB) を動的に制御します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 UpdatePanel の内容は、の特別な extender が存在し、アニメーション、フレームワークに大きく依存して: UpdatePanelAnimation です。 UpdatePanel トリガーと共にも使用できます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 内容を`UpdatePanel`、特別な extender が存在するアニメーション フレームワークに大きく依存している:`UpdatePanelAnimation`です。 と共に使用するも`UpdatePanel`トリガーします。

## <a name="steps"></a>手順

含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

このシナリオでは、アニメーションは、現在の時刻の表示に適用されます。 この情報を使用してラベルに書き込まれることができます、`Page_Load()`メソッド、または次のインライン コードの使用 (ここではわかりやすくするため)。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

また、時刻が更新をトリガーするボタンが作成されます。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

このコードに格納し、`<ContentTemplate>`のセクションで、`UpdatePanel`要素。 パネルの`UpdateMode`に属性を設定する必要があります`"Conditional"`トリガーのみはパネルの内容を更新するため、します。 `<Triggers>`のセクション、 `UpdatePanel`、非同期ポストバック トリガーが作成されに関連付けられている、`Click`ボタンのイベントです。 したがって、ユーザーが、ボタンをクリックした場合、`UpdatePanel`は更新します。 ここでは、マークアップを`UpdatePanel`コントロール。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

最後に、`UpdatePanelAnimationExtender`構成する必要があります。 設定、`TargetControlID`属性パネルの ID をおよびエクステンダー内でアニメーションを定義します。 フェードする意味で、優れた視覚的に重点を置いてが更新された時間を作成します。 Extender マークアップは可能性がありますし、次のようになります。


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

ブラウザーで、ファイルを実行します。 ボタンをクリックするたびに常に 1 秒の間のフェードインのパネルで、現在の時刻が表示されます。


[![現在の時刻がフェードインします。](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

現在の時刻がフェードイン ([フルサイズのイメージを表示するをクリックして](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

>[!div class="step-by-step"]
[前へ](animating-an-updatepanel-control-vb.md)
