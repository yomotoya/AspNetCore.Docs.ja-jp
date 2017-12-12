---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: "処理 (c#)、ModalPopup からのポストバック |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 特別な注意が必要と pos しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 096c32d6004474d3d5952ce12d7349b9ebda6003
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>(C#)、ModalPopup からのポストバックを処理します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 特別な注意が必要、ポストバックがからのポップアップで作成されたときにします。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 特別な注意が必要、ポストバックがからのポップアップで作成されたときにします。

## <a name="steps"></a>手順

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

次に、モーダル ポップアップとして機能するパネルを追加します。 ユーザーは、名前と電子メール アドレスを入力できます。 ボタンは、ポップアップを閉じ、情報の保存に使用されます。 なお、`OnClick`属性が設定されているため、このボタンがクリックされたときに、ポストバックが発生します。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

ページ自体は、まったく同じ情報の 2 つのラベルで構成されています。 名前と電子メール アドレス。 ボタンを使用して、モーダル ポップアップをトリガーします。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

ポップアップを表示するために、追加、`ModalPopupExtender`コントロール。 設定、`PopupControlID`属性パネルの ID をおよび`TargetControlID`ボタンの ID に。

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

今すぐされるたびに、`Save`のモーダル ポップアップでボタンがクリックされた、サーバー側`SaveData()`メソッドを実行します。 データ ストアに入力されたデータを保存する可能性があります。 わかりやすくするため、新しいデータのラベルにだけ出力します。

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

また、現在の名前と電子メールを使用のモーダル ポップアップで、テキスト ボックス コントロールを入力する必要があります。 ただしこれは必要な場合にのみポストバックは発生しません。 ポストバックがある場合、ASP.NET の viewstate 機能は、テキスト ボックスに適切な値を自動的に入力します。

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![モーダル ポップアップがポストバックを発生させる](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

モーダル ポップアップがポストバックを発生させる ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

>[!div class="step-by-step"]
[前へ](using-modalpopup-with-a-repeater-control-cs.md)
[次へ](positioning-a-modalpopup-cs.md)
