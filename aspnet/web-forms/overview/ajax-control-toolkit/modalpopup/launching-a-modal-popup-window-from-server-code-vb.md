---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: サーバー コード (VB) からのモーダル ポップアップ ウィンドウを起動 |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオでは、その t が必要としています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 46554dae60ad9cd13e97e5755e95cb2125d1fed9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872871"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>サーバー コード (VB) からのモーダル ポップアップ ウィンドウを起動します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオでは、サーバー側でモーダル ポップアップの開始がトリガーされることが必要です。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオでは、サーバー側でモーダル ポップアップの開始がトリガーされることが必要です。

## <a name="steps"></a>手順

まず、ModalPopup コントロールの動作方法を示すために、ASP.NET ボタン web コントロールが必要です。 内でこのようなボタンを追加、&lt;フォーム&gt;の新しいページの要素。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

次に、ポップアップを作成するマークアップが必要です。 として定義、`<asp:Panel>`を制御し、ボタン コントロールが含まれているかどうかを確認します。 ModalPopup コントロールがポップアップ ウィンドウです。 このようなボタン閉じるを作成するための機能を提供していますそれ以外の場合が消えることができるようにする簡単な方法はありません。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

次のページに、ASP.NET AJAX Toolkit から ModalPopup コントロールを追加します。 ボタン コントロールに読み込み、非表示になります ボタン、および実際のポップアップの ID のプロパティを設定します。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

ASP.NET AJAX; に基づいてすべての web ページと同様Script Manager が、別のターゲットのブラウザーのために必要な JavaScript ライブラリを読み込む必要です。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

ブラウザーの例を実行します。 ボタンをクリックすると、モーダル ポップアップが表示されます。 サーバー側コードを使用して、同じ効果を実現するために、新しいボタンが必要です。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

ボタンをクリックしてがポストバックを生成し、実行が表示されるよう、`ServerButton_Click()`サーバー上のメソッドです。 このメソッドでは、JavaScript 関数が呼び出された`launchModal()`が実行される正確に言うと、ページが読み込まれる JavaScript 関数が実行されます。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

ジョブの`launchModal()`ModalPopup を表示することです。 `launchModal()`関数が実行されるは、完全な HTML ページが読み込まれます。 その時点では、ただし、ASP.NET AJAX framework が完全にまだ読み込まれていません。 したがって、`launchModal()`関数だけ ModalPopup コントロールを後で示す必要があります、変数を設定します。

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` JavaScript 関数は、ASP.NET AJAX が完全に読み込まれた後に実行される特殊な関数です。 そのため、場合に限り、ModalPopup コントロールを表示するには、この関数にコードを追加お`launchModal()`が前に呼び出されました。

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()`関数がページ上の名前付きの要素を探して、パラメーターとして、サーバー側で ID が必要です。 したがって、 `$find("mpe")` ModalPopup コントロールのクライアントの表現を返します以外の場合はその`show()`メソッドにより、ポップアップ画面が表示されます。


[![モーダル ポップアップを表示するにはボタンのいずれかがクリックされました。](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

モーダル ポップアップを表示するにはボタンのいずれかがクリックされた ([フルサイズのイメージを表示する をクリックします](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](positioning-a-modalpopup-cs.md)
> [次へ](using-modalpopup-with-a-repeater-control-vb.md)
