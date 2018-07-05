---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: サーバー コード (c#) からモーダル ポップアップ ウィンドウの起動 |Microsoft Docs
author: wenz
description: AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオでは、その t が必要としています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: a13915f5748f8ad9b79fb787cc5e0682faa19388
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374092"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>サーバー コード (c#) からモーダル ポップアップ ウィンドウを起動します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオにモーダル ポップアップを開くが、サーバー側でトリガーされる必要があります。


## <a name="overview"></a>概要

AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオにモーダル ポップアップを開くが、サーバー側でトリガーされる必要があります。

## <a name="steps"></a>手順

まず、ModalPopup コントロールの動作方法を示すために、ボタンの ASP.NET web コントロールが必要です。 内でこのようなボタンを追加、&lt;フォーム&gt;新しいページの要素。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

次に、ポップアップを作成するマークアップが必要です。 定義として、`<asp:Panel>`を制御し、ボタン コントロールが含まれているかどうかを確認します。 ModalPopup コントロールがこのようなボタン閉じるのポップアップを作成するための機能を提供していますそれ以外の場合、消えるようにする簡単な方法はありません。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

次のページに ASP.NET AJAX Toolkit の ModalPopup コントロールを追加します。 ボタン コントロールに読み込み、非表示になります ボタン、および実際のポップアップの ID のプロパティを設定します。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

ASP.NET AJAX; に基づいてすべての web ページと同様別のターゲットのブラウザーのために必要な JavaScript ライブラリを読み込むスクリプト マネージャーが必要です。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

ブラウザーでの例を実行します。 ボタンをクリックすると、モーダル ポップアップが表示されます。 サーバー側コードを使用して同じ効果を実現するために新しいボタンが必要です。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

ボタンをクリックしますがポストバックを生成して、実行、ご覧のとおり、`ServerButton_Click()`サーバー上のメソッド。 このメソッドは、JavaScript 関数が呼び出された`launchModal()`実行は正確では、ページが読み込まれると、JavaScript 関数が実行されます。

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

ジョブの`launchModal()`ModalPopup を表示することです。 `launchModal()`関数が実行されるは、完全な HTML ページが読み込まれるとします。 その瞬間、ただし、ASP.NET AJAX フレームワークが完全にまだ読み込まれていません。 そのため、`launchModal()`関数設定変数を後で、ModalPopup コントロールを表示する必要があります。

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

`pageLoad()` JavaScript 関数は、ASP.NET AJAX が完全に読み込まれた後に実行される特殊な関数です。 そのため、ModalPopup コントロールが場合にのみを表示するには、この関数にコードを追加しました`launchModal()`が前に呼び出されました。

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

`$find()`関数はページ上の名前付き要素を探してでサーバー側の ID をパラメーターとして必要があります。 そのため、 `$find("mpe")` ModalPopup コントロールのクライアントの表現を返します。 その`show()`メソッドにより、ポップアップが表示されます。


[![モーダル ポップアップを表示するときに、ボタンのいずれかがクリックされます。](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

モーダル ポップアップを表示するときに、ボタンのいずれかがクリックされた ([フルサイズの画像を表示する をクリックします](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [次へ](using-modalpopup-with-a-repeater-control-cs.md)
