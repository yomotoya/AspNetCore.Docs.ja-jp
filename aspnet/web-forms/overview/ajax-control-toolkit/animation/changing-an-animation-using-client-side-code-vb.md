---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: クライアント側コード (VB) を使用してアニメーションを変更する |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションこともできます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="changing-an-animation-using-client-side-code-vb"></a>クライアント側コード (VB) を使用してアニメーションを変更します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションを変更することもできます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションを変更することもできます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

実際のアニメーションは、HTML ボタンによって起動します。

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

あることに注意してくださいありません`<Animations>`内のノード、`AnimationExtender`コントロール。 カスタムの JavaScript コードを使用すると、コントロールで使用するアニメーションを提供します。

サーバー API と同様に`AnimationExtender`、まだ、extender にアニメーションを代入する簡単な方法はありません。 ただし、extender のアニメーションを読み書きするいくつかのメソッドは公開さまざまなイベントに登録されている (`OnClick`、`OnLoad`など)。 次にいくつかの例を示します。

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

戻り値の形式、`get_*()`関数とする引数の形式、`set_*()`関数は、XML マークアップは何でしょうのオブジェクト表現を提供する、JSON 文字列。 現時点に、オブジェクトを渡すことはありませんが、特定のアニメーションからのオブジェクトの読み取り可能であれば (`get_OnXXXBehavior()`メソッド)。

ここでは、JSON 文字列 (引用符を区切りし、適切な形式)、ボタンによってトリガーされるアニメーションを表すが、パネルのサイズを変更して、同時にフェードアウトしてアニメーション化します。

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

次の JavaScript コードでは、この JSON descripting に割り当てられます、`OnClick`現在エクステンダーのアニメーションして実行します。

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![マウスのクリックせず (およびごくわずかなマークアップを含む) に、アニメーションがすぐに、実行されます。](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

マウス クリックしてせず (およびほとんどのマークアップを含む) に、アニメーションがすぐに、実行されます ([フルサイズのイメージを表示するをクリックして](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](executing-animations-using-client-side-code-vb.md)
> [次へ](animating-an-updatepanel-control-vb.md)
