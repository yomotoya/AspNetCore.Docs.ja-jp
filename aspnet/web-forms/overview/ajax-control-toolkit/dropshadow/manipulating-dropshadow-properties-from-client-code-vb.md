---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: (VB) のクライアント コードから DropShadow プロパティを操作する |Microsoft Docs
author: wenz
description: DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 クライアント JavaScrip を使用して、このエクステンダーのプロパティを変更することもしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b9220eecca224c2b1e0f19c972c6c2a4dc9c7d35
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841410"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>(VB) のクライアント コードから DropShadow プロパティを操作します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。


## <a name="overview"></a>概要

DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。

## <a name="steps"></a>手順

コードは、いくつかの行のテキストを格納しているパネルで始まります。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

関連付けられている CSS クラスは、便利な背景色をパネルになります。

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender`不透明度が 50% に設定、ドロップ シャドウ効果があるパネルの拡張に追加されます。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

ASP.NET AJAX では、`ScriptManager`コントロールが動作する Control Toolkit を使用します。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

別のパネルにドロップ シャドウの不透明度を設定するための 2 つの JavaScript リンクが含まれています: 負符号のリンクが低下シャドウの不透明度、プラスのリンクでが増加します。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

JavaScript 関数`changeOpacity()`まず見つける必要がありますし、`DropShadowExtender`ページ上のコントロール。 ASP.NET AJAX の定義、`$find()`正確にタスクのメソッド。 次に、`get_Opacity()`メソッドは現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。 JavaScript コードは、現在の不透明度値を代入するし、`<label>`要素。

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![クライアント側で不透明度が変更されました。](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

クライアント側で不透明度が変更された ([フルサイズの画像を表示する をクリックします](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](adjusting-the-z-index-of-a-dropshadow-vb.md)
