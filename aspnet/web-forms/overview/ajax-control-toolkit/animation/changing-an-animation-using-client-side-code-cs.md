---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: クライアント側コード (c#) を使用してアニメーションを変更する |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションこともできます.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 9711e93ee6f119ec1825e32bdad64535435970c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808957"
---
<a name="changing-an-animation-using-client-side-code-c"></a>クライアント側コード (c#) を使用してアニメーションを変更します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 カスタムのクライアント側の JavaScript コードを使用してアニメーションを変更することもできます。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 カスタムのクライアント側の JavaScript コードを使用してアニメーションを変更することもできます。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

次のようなテキストのパネルに、アニメーションが適用されます。

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

実際のアニメーションは、HTML ボタンによって起動されます。

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

あることに注意してくださいありません`<Animations>`内のノード、`AnimationExtender`コントロール。 カスタム JavaScript コードは、コントロールで使用するアニメーションの提供に使用されます。

サーバー API と同様`AnimationExtender`アニメーションをエクステンダーにまだ割り当てする簡単な方法はありません。 さまざまなイベントに登録されているエクステンダーはアニメーションを読み書きするいくつかのメソッドを公開するただし (`OnClick`、`OnLoad`など)。 次にいくつかの例を示します。

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

戻り値の形式、`get_*()`関数と引数の形式、`set_*()`関数は、JSON 文字列は、XML マークアップのオブジェクト表現を提供します。 現時点で、オブジェクトを渡す方法はありませんが、特定のアニメーションからオブジェクトを読み取ることが (`get_OnXXXBehavior()`メソッド)。

JSON 文字列を次に示します (引用符を区切り記号と適切に書式設定)、ボタンによってトリガーされるアニメーションを表すが、パネルのサイズを変更して同時にフェードアウトしてアニメーション化します。

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

次の JavaScript コードでは、この JSON descripting に割り当てられます、`OnClick`現在エクステンダーのアニメーションが実行されるとします。

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![アニメーションのマウス クリックせず (およびほとんどのマークアップ)、すぐに実行します。](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

アニメーションのマウス クリックしてせず (およびほとんどのマークアップ)、すぐに実行 ([フルサイズの画像を表示する をクリックします](changing-an-animation-using-client-side-code-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](executing-animations-using-client-side-code-cs.md)
> [次へ](animating-an-updatepanel-control-cs.md)
