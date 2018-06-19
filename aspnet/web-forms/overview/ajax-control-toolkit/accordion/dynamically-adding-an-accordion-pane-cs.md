---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: アコーディオンのウィンドウ (c#) を動的に追加する |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常、w を宣言しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ad2fc6ea3d527215c0226f3f594d781163d538b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879466"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>アコーディオンのウィンドウ (c#) を動的に追加します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常ページ自体内で宣言されていますが、サーバー側のコードは、同じ結果を実現するために使用できます。


## <a name="overview"></a>概要

AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常ページ自体内で宣言されていますが、サーバー側のコードは、同じ結果を実現するために使用できます。

## <a name="steps"></a>手順

アコーディオン コントロールでは、サーバー側のコードをすべての重要なプロパティを公開します。 特に、`Panes`プロパティは、アコーディオンを構成するペインのコレクションへのアクセスを付与します。 型があるすべてのウィンドウ`AccordionPane`します。 つまり、このようなウィンドウを作成する単純です。

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer`プロパティの`AccordionPane`;、ウィンドウのヘッダー セクション内の ASP.NET コントロールにアクセスできるように、`ContentContainer`のプロパティ`AccordionPane`ウィンドウの [コンテンツ] に対しても同様です。 これにより、ASP.NET コード ペインにコンテンツを追加します。

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Pane(s) を最後に、追加する必要があります、`Panes`アコーディオンのコレクション。

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

アコーディオン コントロールに 2 つのペインを追加する完全なサーバー側コードを次に示します。

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

不足している唯一の要素は、アコーディオン自体には、ASP.NET の存在に依存する`ScriptManager`コントロール。

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

この例を完了するには、アコーディオン コントロールで参照されている 2 つの CSS クラスは、ブラウザーのスタイル情報を提供します。

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![アコーディオン内のデータはサーバー側コードによって動的に追加されました](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

アコーディオン内のデータはサーバー側コードによって動的に追加されました ([フルサイズのイメージを表示するをクリックして](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](databinding-to-an-accordion-cs.md)
> [次へ](databinding-to-an-accordion-vb.md)
