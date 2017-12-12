---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: "クライアント側のコード (c#) を使用してアニメーションを変更する |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションこともできます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6dcaeac073f54b0804fe3acf7ec22491b1cbbba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-c"></a>クライアント側のコード (c#) を使用してアニメーションを変更します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションを変更することもできます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションを変更することもできます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

実際のアニメーションは、HTML ボタンによって起動します。

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

あることに注意してくださいありません`<Animations>`内のノード、`AnimationExtender`コントロール。 カスタムの JavaScript コードを使用すると、コントロールで使用するアニメーションを提供します。

サーバー API と同様に`AnimationExtender`、まだ、extender にアニメーションを代入する簡単な方法はありません。 ただし、extender のアニメーションを読み書きするいくつかのメソッドは公開さまざまなイベントに登録されている (`OnClick`、`OnLoad`など)。 次にいくつかの例を示します。

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

戻り値の形式、`get_*()`関数とする引数の形式、`set_*()`関数は、XML マークアップは何でしょうのオブジェクト表現を提供する、JSON 文字列。 現時点に、オブジェクトを渡すことはありませんが、特定のアニメーションからのオブジェクトの読み取り可能であれば (`get_OnXXXBehavior()`メソッド)。

ここでは、JSON 文字列 (引用符を区切りし、適切な形式)、ボタンによってトリガーされるアニメーションを表すが、パネルのサイズを変更して、同時にフェードアウトしてアニメーション化します。

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

次の JavaScript コードでは、この JSON descripting に割り当てられます、`OnClick`現在エクステンダーのアニメーションして実行します。

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![マウスのクリックせず (およびごくわずかなマークアップを含む) に、アニメーションがすぐに、実行されます。](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

マウス クリックしてせず (およびほとんどのマークアップを含む) に、アニメーションがすぐに、実行されます ([フルサイズのイメージを表示するをクリックして](changing-an-animation-using-client-side-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[前へ](executing-animations-using-client-side-code-cs.md)
[次へ](animating-an-updatepanel-control-cs.md)
