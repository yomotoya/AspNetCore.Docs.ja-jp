---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: 別のコントロール (c#) でのアニメーションをトリガーする |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 一般を起動する、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="triggering-an-animation-in-another-control-c"></a>別のコントロール (c#) でのアニメーションをトリガーします。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 一般に、アニメーションの起動が同じコントロールでのユーザー操作によってトリガーされます。 ただしも別のコントロールを 1 つのコントロールとし、アニメーションと対話することです。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 一般に、アニメーションの起動が同じコントロールでのユーザー操作によってトリガーされます。 ただしも別のコントロールを 1 つのコントロールとし、アニメーションと対話することです。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

パネルをアニメーション化を開始するために、HTML のボタンが使用されます。 なお`<input type="button" />`をお勧め`<asp:Button />`たくないポストバック、ユーザーがそのボタンをクリックするためです。

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`です。 設定することが重要`TargetControlID`ボタン (要素をアニメーションをトリガーする、) の ID に、パネル (アニメーション化されている要素) の ID にありません

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

内で、`<Animations>`ノード、通常どおりアニメーションを配置します。 パネルを変更するようにするには、ボタンを設定、`AnimationTarget`内のすべてのアニメーション要素の属性`AnimationExtender`です。 値は、`AnimationTarget`もちろん、パネルの ID です。 このように、アニメーションは、パネルで、トリガーを起動するボタンではなく、発生します。 ここでは、`AnimationExtender`このシナリオのマークアップ。

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

個々 のアニメーションを表示する特別な順序に注意してください。 最初に、アニメーションの実行後に、ボタンが非アクティブ化を取得します。 存在しないためありません`AnimationTarget`属性、`<EnableAction>`要素をこのアニメーション コントロールに適用する、発信元: ボタンをクリックします。 次の 2 つのアニメーションのステップは実行 parallelly (`<Parallel>`要素)。 両方とも、`AnimationTarget`属性に設定`"Panel1"`、したがってパネル、not ボタンをアニメーション化します。


[![マウスでクリック ボタン パネル アニメーションを開始します。](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

マウスでクリック ボタン パネル アニメーションを開始する ([フルサイズのイメージを表示するをクリックして](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](disabling-actions-during-animation-cs.md)
> [次へ](modifying-animations-from-the-server-side-cs.md)
