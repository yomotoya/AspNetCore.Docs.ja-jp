---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: アニメーション (VB) 中の操作を無効にする |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アクションもサポートしています.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 811e1d75f79885f3f4c561d9211fec625fcf1807
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827623"
---
<a name="disabling-actions-during-animation-vb"></a>アニメーション (VB) 中の操作を無効にします。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 マウス クリックしてなどの操作もサポートしています。 ただしマウスのクリックでは、アニメーションを起動するときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 マウス クリックしてなどの操作もサポートしています。 ただしマウスのクリックでは、アニメーションを起動するときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

アニメーションは、次のように、HTML ボタンに適用されます。

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

ポストバック; を作成するボタンしたくないので、Web コントロールではなく、HTML コントロールを使用することに注意してください。私たちにとって、クライアント側のアニメーションを起動しはだけです。

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

内で、`<Animations>`ノード、`<OnClick>`マウス クリックを処理するために適切な要素です。 ただし、アニメーションの中に、ボタンをクリックする可能性があります。 `<EnableAction>`要素を処理できます。 設定`Enabled="false"`アニメーションの一部として、ボタンを無効にします。 (詳しくは、ボタンと実際のアニメーションを無効にする、) 複数の個別のアニメーションを使用しているため、 `<Parallel>` 1 つにまとめて 1 つのアニメーションを連結する要素が必要です。 ここでは、完全なマークアップを`AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

リストの末尾に次の XML 要素を使用して、アニメーションの後のボタンを再度有効にすることもなります。

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

ただし特定のシナリオでこれは無意味になるため、ボタンはフェードアウトし、は、アニメーションの最後に表示されません。


[![アニメーションを実行するとすぐに、ボタンが無効になっています](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

アニメーションを実行するとすぐに、ボタンは無効 ([フルサイズの画像を表示する をクリックします](disabling-actions-during-animation-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](animating-in-response-to-user-interaction-vb.md)
> [次へ](triggering-an-animation-in-another-control-vb.md)
