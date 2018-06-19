---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: クライアント側のコード (c#) を使用してアニメーションを実行 |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションの実行.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870740"
---
<a name="executing-animations-using-client-side-code-c"></a>クライアント側のコード (c#) を使用して実行中のアニメーション
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションの実行をトリガーも可能性があります。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 カスタムのクライアント側 JavaScript コードを使用して、アニメーションの実行をトリガーも可能性があります。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

内で、`<Animations>`ノードを使用して`<OnClick>`パネルに 1 回、ユーザーのアニメーションの実行をクリックします。 Parallelly 実行される 2 つのアニメーションを追加します。

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

デモについては、ここではこのアニメーション (および管理ツールキットを使用して作成されたその他のすべてのアニメーション) では、ページの実行後に、JavaScript コードを使用してが実行されます。 すべてのまずへのアクセス、`AnimationExtender`コントロール。 ASP.NET AJAX ライブラリにより、`$find()`このタスクに対する関数。

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender`コントロールは、XML マークアップで使用されるイベント ハンドラーと同じ名前のメソッドなど、豊富な API を公開: `OnClick()`、`OnLoad()`のようにします。 呼び出しなど、`OnClick()`メソッド内でアニメーションの実行、`<OnClick>`の要素、`AnimationExtender`コントロール。

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

ここでは、クライアント側 JavaScript コード全体のページが完全に読み込まれた後に [パネル] をクリックをエミュレートすることに注意して、 `pageLoad()` 1 回、ページの ASP.NET AJAX によって呼び出される関数の名前が使用して、JavaScript ライブラリであったすべて含まれます読み込まれます。

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![アニメーションのマウス クリックせず、すぐに実行します。](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

マウスのクリックしてせずに、アニメーションがすぐに、実行されます ([フルサイズのイメージを表示するをクリックして](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](modifying-animations-from-the-server-side-cs.md)
> [次へ](changing-an-animation-using-client-side-code-cs.md)
