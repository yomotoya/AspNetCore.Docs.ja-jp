---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: クライアント コード (VB) から DropShadow プロパティの操作 |Microsoft ドキュメント
author: wenz
description: AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。 クライアント JavaScrip を使用しても、このエクステンダーのプロパティを変更できます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>クライアント コード (VB) から DropShadow プロパティを操作します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。 クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。


## <a name="overview"></a>概要

AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。 クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。

## <a name="steps"></a>手順

コードは、いくつかの行のテキストを含むパネルで始まります。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

関連付けられている CSS クラスには、パネル nice の背景色が与えられます。

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

`DropShadowExtender`パネルの ドロップ シャドウ効果、不透明度の 50% に設定と拡張に追加されます。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

ASP.NET AJAX ではその後、`ScriptManager`コントロール動作を制御ツールキットを有効にします。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

別のパネルには、影の不透明度を設定するための 2 つの JavaScript リンクが含まれています。 負符号のリンクは、影の不透明度を減少、正符号のリンクでが増加します。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

JavaScript 関数`changeOpacity()`し、最初に検索する必要があります、`DropShadowExtender`ページ上のコントロールです。 ASP.NET AJAX を定義、`$find()`正確にそのタスクのメソッドです。 次に、`get_Opacity()`メソッドは、現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。 JavaScript コードが現在の不透明度の値、`<label>`要素。

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![クライアント側の不透明度を変更します。](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

クライアント側の不透明度を変更 ([フルサイズのイメージを表示するをクリックして](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](adjusting-the-z-index-of-a-dropshadow-vb.md)
