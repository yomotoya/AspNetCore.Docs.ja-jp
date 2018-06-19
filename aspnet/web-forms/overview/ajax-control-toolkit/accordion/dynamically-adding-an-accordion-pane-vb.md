---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: アコーディオン ペイン (VB) を動的に追加する |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常、w を宣言しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868725"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>アコーディオン ペイン (VB) を動的に追加します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常ページ自体内で宣言されていますが、サーバー側のコードは、同じ結果を実現するために使用できます。


## <a name="overview"></a>概要

AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常ページ自体内で宣言されていますが、サーバー側のコードは、同じ結果を実現するために使用できます。

## <a name="steps"></a>手順

アコーディオン コントロールでは、サーバー側のコードをすべての重要なプロパティを公開します。 特に、`Panes`プロパティは、アコーディオンを構成するペインのコレクションへのアクセスを付与します。 型があるすべてのウィンドウ`AccordionPane`します。 つまり、このようなウィンドウを作成する単純です。

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer`プロパティの`AccordionPane`;、ウィンドウのヘッダー セクション内の ASP.NET コントロールにアクセスできるように、`ContentContainer`のプロパティ`AccordionPane`ウィンドウの [コンテンツ] に対しても同様です。 これにより、ASP.NET コード ペインにコンテンツを追加します。

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Pane(s) を最後に、追加する必要があります、`Panes`アコーディオンのコレクション。

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

アコーディオン コントロールに 2 つのペインを追加する完全なサーバー側コードを次に示します。

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

不足している唯一の要素は、アコーディオン自体には、ASP.NET の存在に依存する`ScriptManager`コントロール。

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

この例を完了するには、アコーディオン コントロールで参照されている 2 つの CSS クラスは、ブラウザーのスタイル情報を提供します。

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![アコーディオン内のデータはサーバー側コードによって動的に追加されました](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

アコーディオン内のデータはサーバー側コードによって動的に追加されました ([フルサイズのイメージを表示するをクリックして](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](databinding-to-an-accordion-vb.md)
