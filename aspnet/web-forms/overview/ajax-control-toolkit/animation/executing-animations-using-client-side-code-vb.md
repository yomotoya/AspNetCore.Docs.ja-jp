---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "クライアント側コード (VB) を使用してアニメーションを実行 |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションの実行."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a>クライアント側コード (VB) を使用して実行中のアニメーション
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションの実行をトリガーも可能性があります。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションの実行をトリガーも可能性があります。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnClick>`パネルに 1 回、ユーザーのアニメーションの実行をクリックします。 Parallelly 実行される 2 つのアニメーションを追加します。

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

デモについては、ここではこのアニメーション (および管理ツールキットを使用して作成されたその他のすべてのアニメーション) では、ページの実行後に、JavaScript コードを使用してが実行されます。 すべてのまずへのアクセス、`AnimationExtender`コントロール。 ASP.NET AJAX ライブラリにより、`$find()`このタスクに対する関数。

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

`AnimationExtender`コントロールは、XML マークアップで使用されるイベント ハンドラーと同じ名前のメソッドなど、豊富な API を公開: `OnClick()`、`OnLoad()`のようにします。 呼び出しなど、`OnClick()`メソッド内でアニメーションの実行、`<OnClick>`の要素、`AnimationExtender`コントロール。

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

ここでは、クライアント側 JavaScript コード全体のページが完全に読み込まれた後に [パネル] をクリックをエミュレートすることに注意して、 `pageLoad()` 1 回、ページの ASP.NET AJAX によって呼び出される関数の名前が使用して、JavaScript ライブラリであったすべて含まれます読み込まれます。

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![アニメーションのマウス クリックせず、すぐに実行します。](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

マウスのクリックしてせずに、アニメーションがすぐに、実行されます ([フルサイズのイメージを表示するをクリックして](executing-animations-using-client-side-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[前へ](modifying-animations-from-the-server-side-vb.md)
[次へ](changing-an-animation-using-client-side-code-vb.md)
