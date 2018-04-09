---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: 複数のポップアップ コントロール (c#) を使用して |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 M を使用することもしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 7acd1b53e1b3e3e0d09d248b68941b166da3e81e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-multiple-popup-controls-c"></a>複数のポップアップ コントロール (c#) を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 1 ページに 1 つ以上のポップアップ コントロールを使用することもできます。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 1 ページに 1 つ以上のポップアップ コントロールを使用することもできます。

## <a name="steps"></a>手順

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

次に、ポップアップ ウィンドウとして機能するパネルを追加します。 現在のシナリオで、パネルが含まれます、`Calendar`コントロール。 内のパネルを配置するカレンダーのポストバックによるページの更新を回避するために、`UpdatePanel`コントロール。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

ページには、2 つのテキスト ボックスも含まれています。 各テキスト ボックスのカレンダー ポップアップ表示される、テキスト ボックスがアクティブ化されます。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

使用して 2 つのテキスト ボックスの各を拡張するようになりました、`PopupControlExtender`です。 `TargetControlID`属性が、extender に関連付けられているコントロールの ID を提供します。 `PopupControlID`属性は、[ポップアップ] パネルの ID を格納します。 この場合、両方エクステンダーが、同じパネルを表示するが、さまざまなパネルは、可能であれば、同様です。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

今すぐテキスト フィールド内をクリックするたびにカレンダーが表示されます、フィールドの下の日付を選択することができます。 (テキスト ボックスに、選択した日付を取得については、説明別のチュートリアルです。)


[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズのイメージを表示するをクリックして](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [次へ](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
