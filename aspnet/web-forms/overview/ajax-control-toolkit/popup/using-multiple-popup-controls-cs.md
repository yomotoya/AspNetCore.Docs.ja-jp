---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: 複数のポップアップ コントロール (c#) を使用して |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 M を使用することもしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b00f720b66e6826c29f51690ab3361958aa8677
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364816"
---
<a name="using-multiple-popup-controls-c"></a>複数のポップアップ コントロール (c#) を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 1 ページには、複数のポップアップ コントロールを使用することもできます。


## <a name="overview"></a>概要

AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 1 ページには、複数のポップアップ コントロールを使用することもできます。

## <a name="steps"></a>手順

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

次に、ポップアップとして機能するパネルを追加します。 パネルに含まれる現在のシナリオでは、`Calendar`コントロール。 内のパネルを配置するカレンダーのポストバックの原因となったページ更新を回避するために、`UpdatePanel`コントロール。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

ページには、2 つのテキスト ボックスも含まれています。 各テキスト ボックスのテキスト ボックスがアクティブ化される、カレンダーのポップアップは表示されます。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

2 つのテキスト ボックスのそれぞれを拡張するようになりました、`PopupControlExtender`します。 `TargetControlID`属性は、extender に関連付けられているコントロールの ID を提供します。 `PopupControlID`属性には、ポップアップ パネルの ID が含まれています。 この場合は、両方のエクステンダーが同じのパネルを表示するが、さまざまなパネルをも可能であれば、します。

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

テキスト フィールド内でクリックしたときに、カレンダーが表示されます、フィールドの下の日付を選択することができます。 (テキスト ボックスに、選択した日付は戻ってきてで取り上げる別のチュートリアルです。)


[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズの画像を表示する をクリックします](using-multiple-popup-controls-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [次へ](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
