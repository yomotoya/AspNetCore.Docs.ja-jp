---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: 複数のポップアップ コントロール (VB) を使用して |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 M を使用することもしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: accb9134cc355e0c042114873973bcbfe9a4a0a4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384562"
---
<a name="using-multiple-popup-controls-vb"></a>複数のポップアップ コントロール (VB) を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 1 ページには、複数のポップアップ コントロールを使用することもできます。


## <a name="overview"></a>概要

AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 1 ページには、複数のポップアップ コントロールを使用することもできます。

## <a name="steps"></a>手順

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

次に、ポップアップとして機能するパネルを追加します。 パネルに含まれる現在のシナリオでは、`Calendar`コントロール。 内のパネルを配置するカレンダーのポストバックの原因となったページ更新を回避するために、`UpdatePanel`コントロール。

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

ページには、2 つのテキスト ボックスも含まれています。 各テキスト ボックスのテキスト ボックスがアクティブ化される、カレンダーのポップアップは表示されます。

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

2 つのテキスト ボックスのそれぞれを拡張するようになりました、`PopupControlExtender`します。 `TargetControlID`属性は、extender に関連付けられているコントロールの ID を提供します。 `PopupControlID`属性には、ポップアップ パネルの ID が含まれています。 この場合は、両方のエクステンダーが同じのパネルを表示するが、さまざまなパネルをも可能であれば、します。

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

テキスト フィールド内でクリックしたときに、カレンダーが表示されます、フィールドの下の日付を選択することができます。 (テキスト ボックスに、選択した日付は戻ってきてで取り上げる別のチュートリアルです。)


[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズの画像を表示する をクリックします](using-multiple-popup-controls-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [次へ](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
