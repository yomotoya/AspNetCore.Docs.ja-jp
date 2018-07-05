---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: サーバー側 (c#) からアニメーションを変更する |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションも可能性があります.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 8954f025873ea553fde26c6e2330ce6e5be2b539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804055"
---
<a name="modifying-animations-from-the-server-side-c"></a>サーバー側 (c#) からアニメーションを変更します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 サーバー側で、アニメーションを変更することも


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 サーバー側で、アニメーションを変更することも

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

次のようなテキストのパネルに、アニメーションが適用されます。

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

コードの残りの部分がサーバー側で実行され、マークアップ; は使用しません代わりに、作成するコードを使用、`AnimationExtender`コントロール。

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

ただし、Control Toolkit 現在アクセスことはできません、API を個別にアニメーションを作成します。 ただしを設定すること、`AnimationExtender`アニメーションを宣言的に割り当てるときに使用する XML マークアップを含む文字列へのアニメーション プロパティ。 含めることはできませんが、XML を作成するには、 `<Animations>` .NET Framework の XML を使用する可能性があります要素は、サポートまたは、次のコードのように、文字列を指定だけです。

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

最後に、追加、`AnimationExtender`内で現在のページにコントロール、`<form runat="server">`要素、アニメーションが含まれており、実行されることを確認します。

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![サーバー側の C #/vb のコードを使用してアニメーションの作成します。](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

サーバー側の C #/vb のコードを使用してアニメーションの作成 ([フルサイズの画像を表示する をクリックします](modifying-animations-from-the-server-side-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](triggering-an-animation-in-another-control-cs.md)
> [次へ](executing-animations-using-client-side-code-cs.md)
