---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: クライアント コード (c#) から DropShadow プロパティの操作 |Microsoft ドキュメント
author: wenz
description: DataList の編集インターフェイスのカスタマイズ
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870337"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>クライアント コード (c#) から DropShadow プロパティを操作します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。 クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。


## <a name="overview"></a>概要

AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。 クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。

## <a name="steps"></a>手順

コードは、いくつかの行のテキストを含むパネルで始まります。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

関連付けられている CSS クラスには、パネル nice の背景色が与えられます。

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

`DropShadowExtender`パネルの ドロップ シャドウ効果、不透明度の 50% に設定と拡張に追加されます。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

ASP.NET AJAX ではその後、`ScriptManager`コントロール動作を制御ツールキットを有効にします。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

別のパネルには、影の不透明度を設定するための 2 つの JavaScript リンクが含まれています。 負符号のリンクは、影の不透明度を減少、正符号のリンクでが増加します。

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

JavaScript 関数`changeOpacity()`し、最初に検索する必要があります、`DropShadowExtender`ページ上のコントロールです。 ASP.NET AJAX を定義、`$find()`正確にそのタスクのメソッドです。 次に、`get_Opacity()`メソッドは、現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。 JavaScript コードが現在の不透明度の値、`<label>`要素。

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![クライアント側の不透明度を変更します。](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

クライアント側の不透明度を変更 ([フルサイズのイメージを表示するをクリックして](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [次へ](adjusting-the-z-index-of-a-dropshadow-vb.md)
