---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: 変更 (c#)、サーバー側からアニメーション |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションも可能性があります.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869011"
---
<a name="modifying-animations-from-the-server-side-c"></a>(C#)、サーバー側からのアニメーションを変更します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 サーバー側でのアニメーションを変更することも


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 サーバー側でのアニメーションを変更することも

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

アニメーションは、次のようなテキストのパネルに適用されます。

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

コードの残りの部分がサーバー側で実行し、マークアップ; を使用しません代わりに、作成するコードを使用して、`AnimationExtender`コントロール。

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

ただし、コントロール Toolkit 現在は提供されません API アクセスを個々 のアニメーションを作成します。 ただし、これを設定すること、`AnimationExtender`のアニメーション プロパティを文字列には、アニメーションを宣言して割り当てる場合に使用する XML マークアップを含んでいます。 含めることはできませんが、XML を作成するために、 `<Animations>` .NET Framework の XML を使用する可能性があります要素は、サポートまたは、次のコードのようにだけを指定して、文字列。

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

最後に、追加、`AnimationExtender`内で、現在のページを制御、`<form runat="server">`要素、アニメーションが含まれるして実行されることを確認します。

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![サーバー側の C #/vb のコードを使用して、アニメーションを作成します。](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

サーバー側の C #/vb のコードを使用して、アニメーションを作成 ([フルサイズのイメージを表示するをクリックして](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](triggering-an-animation-in-another-control-cs.md)
> [次へ](executing-animations-using-client-side-code-cs.md)
