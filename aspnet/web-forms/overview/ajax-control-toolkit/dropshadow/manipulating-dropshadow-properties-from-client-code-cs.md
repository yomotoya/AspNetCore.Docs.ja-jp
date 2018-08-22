---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: クライアント コード (c#) から DropShadow プロパティを操作する |Microsoft Docs
author: wenz
description: DataList の編集インターフェイスをカスタマイズします。
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: f751e2378621a6ab74f9f15fe51fd18bdd8b4070
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827158"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>クライアント コード (c#) から DropShadow プロパティを操作します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。


## <a name="overview"></a>概要

DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。

## <a name="steps"></a>手順

コードは、いくつかの行のテキストを格納しているパネルで始まります。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

関連付けられている CSS クラスは、便利な背景色をパネルになります。

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender`不透明度が 50% に設定、ドロップ シャドウ効果があるパネルの拡張に追加されます。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

ASP.NET AJAX では、`ScriptManager`コントロールが動作する Control Toolkit を使用します。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

別のパネルにドロップ シャドウの不透明度を設定するための 2 つの JavaScript リンクが含まれています: 負符号のリンクが低下シャドウの不透明度、プラスのリンクでが増加します。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

JavaScript 関数`changeOpacity()`まず見つける必要がありますし、`DropShadowExtender`ページ上のコントロール。 ASP.NET AJAX の定義、`$find()`正確にタスクのメソッド。 次に、`get_Opacity()`メソッドは現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。 JavaScript コードは、現在の不透明度値を代入するし、`<label>`要素。

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![クライアント側で不透明度が変更されました。](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

クライアント側で不透明度が変更された ([フルサイズの画像を表示する をクリックします](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [次へ](adjusting-the-z-index-of-a-dropshadow-vb.md)
