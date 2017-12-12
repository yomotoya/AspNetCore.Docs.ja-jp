---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: "自動ポストバック (VB) でスライダー コントロールの使用 |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。 スライダーを自動転記を作成しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: fedd3ff947c6f5d5d4d00791087e9fd825fdf9d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>自動ポストバック (VB) でスライダー コントロールの使用
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。 することがスライダー autopostback 1 回値が変化します。


## <a name="overview"></a>概要

AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。 することがスライダー autopostback 1 回値が変化します。

## <a name="steps"></a>手順

両方のテキスト ボックス、スライダーを変更されたときにポストバックが自動的に行うために、属性が必要な`AutoPostBack="true"`: 自体、スライダーとなるテキスト ボックスとは、スライダーの位置を保持するテキスト ボックス。 その必要なマークアップを次に示します。

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` ASP.NET AJAX コントロール Toolkit からコントロールが 2 つのテキスト ボックスにスライダー機能を割り当てます。

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

追加のラベル要素は、ポストバックのユーザーを通知するために後で使用されます。

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

最後に、 `ScriptManager` ASP.NET AJAX のコントロールが動作するコントロール ツールキットの必要な JavaScript を読み込みます。

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

これで、スライダーがポスト バックです。サーバー側では、このイベントをキャッチし、処理する可能性があります。

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![ポストバックをトリガーするスライダーの移動](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

ポストバックをトリガーするスライダーの移動 ([フルサイズのイメージを表示するをクリックして](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![この変更の日付がラベルで記述された後、](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

その後、この変更の日付は、ラベルに書き込まれます ([フルサイズのイメージを表示するをクリックして](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

>[!div class="step-by-step"]
[前へ](databinding-the-slider-control-cs.md)
[次へ](databinding-the-slider-control-vb.md)
