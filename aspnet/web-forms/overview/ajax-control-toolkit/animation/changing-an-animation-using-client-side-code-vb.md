---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: クライアント側コード (VB) を使用してアニメーションを変更する |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションこともできます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8ca2c962c5ebe5e0c45d5b575031ada3e64acd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386748"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>クライアント側コード (VB) を使用してアニメーションを変更します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 カスタムのクライアント側の JavaScript コードを使用してアニメーションを変更することもできます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 カスタムのクライアント側の JavaScript コードを使用してアニメーションを変更することもできます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

次のようなテキストのパネルに、アニメーションが適用されます。

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

実際のアニメーションは、HTML ボタンによって起動されます。

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

あることに注意してくださいありません`<Animations>`内のノード、`AnimationExtender`コントロール。 カスタム JavaScript コードは、コントロールで使用するアニメーションの提供に使用されます。

サーバー API と同様`AnimationExtender`アニメーションをエクステンダーにまだ割り当てする簡単な方法はありません。 さまざまなイベントに登録されているエクステンダーはアニメーションを読み書きするいくつかのメソッドを公開するただし (`OnClick`、`OnLoad`など)。 次にいくつかの例を示します。

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

戻り値の形式、`get_*()`関数と引数の形式、`set_*()`関数は、JSON 文字列は、XML マークアップのオブジェクト表現を提供します。 現時点で、オブジェクトを渡す方法はありませんが、特定のアニメーションからオブジェクトを読み取ることが (`get_OnXXXBehavior()`メソッド)。

JSON 文字列を次に示します (引用符を区切り記号と適切に書式設定)、ボタンによってトリガーされるアニメーションを表すが、パネルのサイズを変更して同時にフェードアウトしてアニメーション化します。

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

次の JavaScript コードでは、この JSON descripting に割り当てられます、`OnClick`現在エクステンダーのアニメーションが実行されるとします。

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![アニメーションのマウス クリックせず (およびほとんどのマークアップ)、すぐに実行します。](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

アニメーションのマウス クリックしてせず (およびほとんどのマークアップ)、すぐに実行 ([フルサイズの画像を表示する をクリックします](changing-an-animation-using-client-side-code-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](executing-animations-using-client-side-code-vb.md)
> [次へ](animating-an-updatepanel-control-vb.md)
