---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: 動的に追加する、アコーディオン ウィンドウ (c#) |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルには、w を宣言は、通常は.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 16cefadb7086a658b20558526757f9229a43fcc9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826119"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>アコーディオン ウィンドウでは (c#) を動的に追加します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルがページ自体には、通常は宣言されているが、サーバー側のコードは、同じ結果を実現するために使用できます。


## <a name="overview"></a>概要

AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルがページ自体には、通常は宣言されているが、サーバー側のコードは、同じ結果を実現するために使用できます。

## <a name="steps"></a>手順

アコーディオン コントロールは、サーバー側コードをすべての重要なプロパティを公開します。 その他のもの、`Panes`プロパティは、アコーディオンを構成するペインのコレクションへのアクセスを許可します。 型のあるすべてのウィンドウ`AccordionPane`します。 このようなウィンドウを作成する単純なしたがって。

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

`HeaderContainer`プロパティの`AccordionPane`; ウィンドウのヘッダー セクション内の ASP.NET コントロールへのアクセスを提供します、`ContentContainer`プロパティの`AccordionPane`ウィンドウのコンテンツ セクションに対しても同様です。 これにより、ASP.NET コード ウィンドウにコンテンツを追加します。

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

最後に、pane(s) に追加する必要があります、`Panes`アコーディオンのコレクション。

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

アコーディオン コントロールに 2 つのペインを追加します。 完全なサーバー側コードを次に示します。

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

唯一の要素がありませんが、ASP.NET の存在に依存する自体、アコーディオン`ScriptManager`コントロール。

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

例を完了するには、アコーディオン コントロールで参照されている 2 つの CSS クラスは、ブラウザーのスタイル情報を提供します。

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![アコーディオンにデータはサーバー側コードによって動的に追加されました](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

アコーディオンにデータはサーバー側コードによって動的に追加されました ([フルサイズの画像を表示する をクリックします](dynamically-adding-an-accordion-pane-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](databinding-to-an-accordion-cs.md)
> [次へ](databinding-to-an-accordion-vb.md)
