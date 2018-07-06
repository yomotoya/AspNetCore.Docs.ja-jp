---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: (C#) DropShadow の Z インデックスを調整 |Microsoft Docs
author: wenz
description: DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 ただし場合がありますこのシャドウがインストールの他のコントロールと競合しています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 2470972e038b0bb58601e100dd568a17281e2abe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827438"
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>(C#) DropShadow の Z インデックスを調整します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 ただしこのシャドウは、他のコントロールでは、ASP.NET の Menu コントロールのインスタンスとも競合しています。 ときにメニュー エントリが表示されます、ドロップ シャドウの背後に表示されます。


## <a name="overview"></a>概要

DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 ただしこのシャドウは、他のコントロールでは、ASP.NET の Menu コントロールのインスタンスとも競合しています。 ときにメニュー エントリが表示されます、ドロップ シャドウの背後に表示されます。

## <a name="steps"></a>手順

コードは、パネルが表示される効果のための十分なテキストが含まれるように、十分なテキストを含む、パネル自体には、開始します。

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

別のパネルが直前に配置されて、`panelShadow`パネル。 上メニュー エントリが表示されるように水平方向のメニューが含まれている (または: )、`dropShadow`パネル)。

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

次に、`DropShadowExtender`拡張に追加されます、`panelShadow`ドロップ シャドウ効果を使用したパネル。

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

ASP.NET AJAX では最後に、`ScriptManager`コントロールが動作する Control Toolkit を使用します。

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

このスクリプトを実行すると、パネルの下にあるメニュー エントリが表示されます。 ただし、メニューが CSS クラスを使用して`panel`だけがある他のパネルの前に表示される要素を次の 2 つを定義します。

- 相対配置
- 正の z インデックス

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

次に、 `DropShadowExtender` Menu コントロールとコントロールが不要に競合しません。


[![前: に、メニュー項目が表示されていません。](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Before: メニューのエントリが表示されていない ([フルサイズの画像を表示する をクリックします](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))。


[![メニュー エントリの表示後。](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

処理の後: メニュー項目が表示されます ([フルサイズの画像を表示する をクリックします](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))。

> [!div class="step-by-step"]
> [次へ](manipulating-dropshadow-properties-from-client-code-cs.md)
