---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: データ バインド、スライダー コントロール (c#) |Microsoft Docs
author: wenz
description: スライダー コントロール、AJAX Control Toolkit では、マウスを使用して制御できるグラフィカルなスライダーを提供します。 現在の positio をバインドすることはしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: b7aebb8dd180113b011ac038e8da4a3baa701485
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818664"
---
<a name="databinding-the-slider-control-c"></a>スライダー コントロール (c#) をデータ バインド
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> スライダー コントロール、AJAX Control Toolkit では、マウスを使用して制御できるグラフィカルなスライダーを提供します。 スライダーの現在の位置を別の ASP.NET コントロールにバインドすることになります。


## <a name="overview"></a>概要

スライダー コントロール、AJAX Control Toolkit では、マウスを使用して制御できるグラフィカルなスライダーを提供します。 スライダーの現在の位置を別の ASP.NET コントロールにバインドすることになります。

## <a name="steps"></a>手順

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

次に、2 つ追加`TextBox`ページにコントロール。 1 つは、グラフィカルなスライダーに変換され、もう 1 つは、スライダーの位置を保持します。

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

次の手順は、最後の手順では既にいます。 `SliderExtender` ASP.NET AJAX Control toolkit のコントロールの最初のテキスト ボックスから、スライダーし、スライダーの位置が変わったときに、2 つ目のテキスト ボックスを自動的に更新します。 作業をするためには、`SliderExtender`の`TargetControlID`属性は、最初のテキスト ボックスの ID に設定する必要があります、`BoundControlID`属性は、2 つ目のテキスト ボックスの ID に設定する必要があります。

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

双方向でのデータ バインドの動作、ブラウザーでご覧のとおり: スライダーの位置を更新して、テキスト ボックスに新しい値を入力します。 2 番目のテキスト ボックスは読み取り専用にする場合は、そこに値を手動で更新するユーザーを複雑になるように、テキスト フィールドに簡単な保護を追加できます。


[![スライダーとテキスト ボックスが同期](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

スライダーとテキスト ボックスが同期 ([フルサイズの画像を表示する をクリックします](databinding-the-slider-control-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](using-the-slider-control-with-auto-postback-cs.md)
> [次へ](using-the-slider-control-with-auto-postback-vb.md)
