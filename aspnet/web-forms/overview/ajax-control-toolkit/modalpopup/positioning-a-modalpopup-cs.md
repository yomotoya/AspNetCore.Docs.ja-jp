---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: ModalPopup の位置 (c#) |Microsoft Docs
author: wenz
description: AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、制御は提供されませんをしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f94c0a473b88c0155847d700c48faa34b84c940
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807065"
---
<a name="positioning-a-modalpopup-c"></a>ModalPopup の位置 (c#)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、コントロールは、ポップアップを配置する組み込みの機能を提供していません。


## <a name="overview"></a>概要

AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、コントロールは、ポップアップを配置する組み込みの機能を提供していません。

## <a name="steps"></a>手順

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`します。 コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

次に、モーダル ポップアップとして機能するパネルを追加します。 ボタンを使用して、ポップアップを閉じます。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

ポップアップが表示されるたびには、特定の場所 ページでに配置する必要があります。 このタスクでは、クライアント側の JavaScript 関数が作成されます。 まず、パネルにアクセスしようとします。 成功すると、パネルの位置が CSS および JavaScript (でポップアップの位置は変更) を使用して設定されます。 ただし、`ModalPopupExtender`コントロールもポップアップの配置を試みます。 そのため、JavaScript コードは、繰り返しの秒部分の 10 分ごと、ポップアップを配置します。

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

ご覧のとおりの戻り値、 `setTimeout()` JavaScript メソッドは、グローバル変数に保存されます。 これにより、停止、オンデマンドでポップアップの繰り返しの配置を使用して、`clearTimeout()`メソッド。

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

残りの作業は適切であれば、これらの関数を呼び出し、ブラウザーにします。 `movePanel()`パネルをトリガーするボタンがクリックされたときに、JavaScript 関数を呼び出す必要があります。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

および`stopMoving()`関数には、ポップアップには、このことができますが閉じられたときにトリガー、`ModalPopupExtender`コントロール。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![指定した位置にモーダル ポップアップが表示されます。](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

指定した位置にモーダル ポップアップが表示されます ([フルサイズの画像を表示する をクリックします](positioning-a-modalpopup-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](handling-postbacks-from-a-modalpopup-cs.md)
> [次へ](launching-a-modal-popup-window-from-server-code-vb.md)
