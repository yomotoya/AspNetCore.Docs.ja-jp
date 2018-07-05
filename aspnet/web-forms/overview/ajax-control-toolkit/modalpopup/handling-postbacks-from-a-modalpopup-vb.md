---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: (VB) ModalPopup からポストバックを処理する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 Pos ときに、特別な注意を実行する必要が.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: c4c067401f88c0bba7269406d08b7b3857f022d6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804369"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>(VB) ModalPopup からポストバックを処理します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ポップアップでポストバックが作成されたときに、特別な注意を実行する必要があります。


## <a name="overview"></a>概要

AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ポップアップでポストバックが作成されたときに、特別な注意を実行する必要があります。

## <a name="steps"></a>手順

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

次に、モーダル ポップアップとして機能するパネルを追加します。 ユーザーは、名前と電子メール アドレスを入力できます。 ボタンは、ポップアップを閉じるし、情報の保存に使用されます。 なお、`OnClick`属性を設定すると、このボタンがクリックされたときにポストバックが発生するようにします。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

ページ自体は、まったく同じ情報の 2 つのラベルで構成されています。 名前と電子メール アドレス。 ボタンを使用して、モーダル ポップアップをトリガーします。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

ポップアップを表示するために、追加、`ModalPopupExtender`コントロール。 設定、`PopupControlID`パネルの id 属性と`TargetControlID`ボタンの ID に。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

今すぐたびに、`Save`モーダル ポップアップ内のボタンがクリックされた、サーバー側`SaveData()`メソッドが実行されます。 データ ストアに入力されたデータを保存する可能性があります。 わかりやすくする、新しいデータのラベルにだけ出力します。

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

また、現在の名前と電子メールを使用モーダル ポップアップ内のテキスト ボックス コントロールを入力する必要があります。 ただしこれは必要な場合にのみポストバックは発生しません。 ポストバックの場合、ASP.NET の viewstate の機能は自動的に、テキスト ボックスに適切な値を入力します。

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![モーダル ポップアップがポストバックを発生させる](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

モーダル ポップアップがポストバックを発生させる ([フルサイズの画像を表示する をクリックします](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](using-modalpopup-with-a-repeater-control-vb.md)
> [次へ](positioning-a-modalpopup-vb.md)
