---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: アニメーション (c#) 中の操作を無効にする |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アクションもサポートしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: a82d46f47cf12b29284bf9211545f8984a586c03
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365582"
---
<a name="disabling-actions-during-animation-c"></a>アニメーション (c#) 中の操作を無効にします。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 マウス クリックしてなどの操作もサポートしています。 ただしマウスのクリックでは、アニメーションを起動するときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 マウス クリックしてなどの操作もサポートしています。 ただしマウスのクリックでは、アニメーションを起動するときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

アニメーションは、次のように、HTML ボタンに適用されます。

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

ポストバック; を作成するボタンしたくないので、Web コントロールではなく、HTML コントロールを使用することに注意してください。私たちにとって、クライアント側のアニメーションを起動しはだけです。

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

内で、`<Animations>`ノード、`<OnClick>`マウス クリックを処理するために適切な要素です。 ただし、アニメーションの中に、ボタンをクリックする可能性があります。 `<EnableAction>`要素を処理できます。 設定`Enabled="false"`アニメーションの一部として、ボタンを無効にします。 (詳しくは、ボタンと実際のアニメーションを無効にする、) 複数の個別のアニメーションを使用しているため、 `<Parallel>` 1 つにまとめて 1 つのアニメーションを連結する要素が必要です。 ここでは、完全なマークアップを`AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

リストの末尾に次の XML 要素を使用して、アニメーションの後のボタンを再度有効にすることもなります。

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

ただし特定のシナリオでこれは無意味になるため、ボタンはフェードアウトし、は、アニメーションの最後に表示されません。


[![アニメーションを実行するとすぐに、ボタンが無効になっています](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

アニメーションを実行するとすぐに、ボタンは無効 ([フルサイズの画像を表示する をクリックします](disabling-actions-during-animation-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](animating-in-response-to-user-interaction-cs.md)
> [次へ](triggering-an-animation-in-another-control-cs.md)
