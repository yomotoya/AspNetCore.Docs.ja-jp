---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: "サーバー側 (VB) からのアニメーションを変更する |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションも可能性があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: c5b23cce529be24157a8a3f9136de7ad7bafc1ea
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-vb"></a>サーバー側 (VB) からのアニメーションを変更します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 サーバー側でのアニメーションを変更することも


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 サーバー側でのアニメーションを変更することも

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

コードの残りの部分がサーバー側で実行し、マークアップ; を使用しません代わりに、作成するコードを使用して、`AnimationExtender`コントロール。

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

ただし、コントロール Toolkit 現在は提供されません API アクセスを個々 のアニメーションを作成します。 ただし、これを設定すること、`AnimationExtender`のアニメーション プロパティを文字列には、アニメーションを宣言して割り当てる場合に使用する XML マークアップを含んでいます。 含めることはできませんが、XML を作成するために、 `<Animations>` .NET Framework の XML を使用する可能性があります要素は、サポートまたは、次のコードのようにだけを指定して、文字列。

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

最後に、追加、`AnimationExtender`内で、現在のページを制御、`<form runat="server">`要素、アニメーションが含まれるして実行されることを確認します。

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![サーバー側の C #/vb のコードを使用して、アニメーションを作成します。](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

サーバー側の C #/vb のコードを使用して、アニメーションを作成 ([フルサイズのイメージを表示するをクリックして](modifying-animations-from-the-server-side-vb/_static/image3.png))

>[!div class="step-by-step"]
[前へ](triggering-an-animation-in-another-control-vb.md)
[次へ](executing-animations-using-client-side-code-vb.md)
