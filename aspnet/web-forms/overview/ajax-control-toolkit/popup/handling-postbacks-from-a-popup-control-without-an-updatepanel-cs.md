---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: "UpdatePanel (c#) なしのポップアップ コントロールからのポストバックを処理する |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 Su でポストバックが発生する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>UpdatePanel (c#) なしのポップアップ コントロールからのポストバックを処理します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 このようなパネルのポストバックが発生するし、ページ上のパネルがあることを決定するパネルがクリックしてされた困難です。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 このようなパネルのポストバックが発生するし、ページ上のパネルがあることを決定するパネルがクリックしてされた困難です。

## <a name="steps"></a>手順

使用する場合、 `PopupControl` 、ポストバックがしなくても、 `UpdatePanel`  ページで、コントロール ツールキットは提供しませんクライアント要素の順番のポストバックの原因となったポップアップがトリガーを決定する方法です。 ただしトリックを小さなは、このシナリオの回避策を提供します。

まず、基本設定を次に示します: 2 つのテキスト ボックスがどちらも、同じポップアップ カレンダーをトリガーします。 2 つ`PopupControlExtenders`テキスト ボックスとポップアップをまとめます。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

非表示のフォーム フィールドを追加するのには、基本的な概念、 &lt; `form` &gt;ポップアップ画面を起動するテキスト ボックスを保持する要素。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

ページが読み込まれると、JavaScript コードは両方のテキスト ボックスにイベント ハンドラーを追加します。 その名前が隠しフォーム フィールドに書き込まれるテキスト ボックスに、ユーザーがクリックすると、ときに。

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

サーバー側のコードでは、非表示フィールドの値を読み取る必要があります。 隠しフォーム フィールドが操作する単純なため、非表示の値を検証するホワイト リストのアプローチが必要です。 正しいテキスト ボックスを特定したら、カレンダーから日付がそこに書き込まれます。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![日付をクリックすると、テキスト ボックスに配置します。](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

日付をクリックすると、テキスト ボックスに配置 ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[前へ](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[次へ](using-multiple-popup-controls-vb.md)
