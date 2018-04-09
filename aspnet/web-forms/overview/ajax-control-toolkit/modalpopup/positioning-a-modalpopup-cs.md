---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: 配置、ModalPopup (c#) |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、コントロールは提供しません、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: bee5be84259231d8cd5efde74b610d72f5e250cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="positioning-a-modalpopup-c"></a>配置、ModalPopup (c#)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、コントロールでは、ポップアップを配置する組み込みの機能を提供していません。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、コントロールでは、ポップアップを配置する組み込みの機能を提供していません。

## <a name="steps"></a>手順

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`です。 コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

次に、モーダル ポップアップとして機能するパネルを追加します。 ボタンを使用して、ポップアップ ウィンドウを閉じます。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

ポップアップが表示されるたびに、特定の場所 ページでに配置するものとします。 このタスクでは、クライアント側 JavaScript 関数が作成されます。 まず、パネルにアクセスしようとします。 成功した場合、パネルの位置は CSS および JavaScript (でポップアップの位置は変更) を使用して設定されます。 ただし、`ModalPopupExtender`コントロールもしようとする、ポップアップを配置します。 そのため、JavaScript コード、ポップアップの秒部分の 10 分ごとに繰り返し配置します。

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

ご覧の戻り値として、 `setTimeout()` JavaScript メソッドは、グローバル変数に保存します。 これにより、必要に応じて、ポップアップの繰り返しの配置を停止するを使用して、`clearTimeout()`メソッド。

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

今すぐために残されている、必要に応じてこれらの関数を呼び出すブラウザーです。 `movePanel()`パネルをトリガーするボタンがクリックされたときに、JavaScript 関数を呼び出す必要があります。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

および`stopMoving()`関数活躍、ポップアップ画面には、このことができますが閉じられたときにトリガーされる、`ModalPopupExtender`コントロール。

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![指定した位置にモーダル ポップアップが表示されます。](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

指定した位置にモーダル ポップアップが表示されます ([フルサイズのイメージを表示するをクリックして](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](handling-postbacks-from-a-modalpopup-cs.md)
> [次へ](launching-a-modal-popup-window-from-server-code-vb.md)
