---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: UpdatePanel (c#) なしのポップアップ コントロールからポストバックを処理する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 Su でポストバックが発生する.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 108595a37d6c15a4bd6ddee365ca718969ca9c8c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804036"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>UpdatePanel (c#) なしのポップアップ コントロールからポストバックを処理します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 このようなパネルで、ポストバックが発生して、ページ上のパネルがある場合は、パネルがクリックしてされたかを判断する困難です。


## <a name="overview"></a>概要

AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 このようなパネルで、ポストバックが発生して、ページ上のパネルがある場合は、パネルがクリックしてされたかを判断する困難です。

## <a name="steps"></a>手順

使用する場合、 `PopupControl` 、ポストバックがしなくても、 `UpdatePanel`  ページで、Control Toolkit は提供していませんクライアント要素がさらに、ポストバックの原因となったポップアップをトリガーを決定する方法。 ただし小規模なトリックは、このシナリオの回避策を提供します。

まず、基本的なセットアップを示します。 どちらも、同じポップアップ カレンダーをトリガーする 2 つのテキスト ボックス。 2 つ`PopupControlExtenders`テキスト ボックスとポップアップをまとめて表示します。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

基本的な考え方が隠しフォーム フィールドを追加するには、 &lt; `form` &gt;起動ポップアップ テキスト ボックスを保持する要素。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

ページが読み込まれると、JavaScript コードは両方のテキスト ボックスにイベント ハンドラーを追加します。 その名前が隠しフォーム フィールドに書き込まれたユーザーがテキスト ボックスをクリックすると。

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

サーバー側のコードでは、非表示フィールドの値を読み取る必要があります。 隠しフォーム フィールドは、操作は簡単であるために、非表示の値の検証にホワイト リスト アプローチが必要です。 適切なテキスト ボックスが識別されると、カレンダーから日付がそこに書き込まれます。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズの画像を表示する をクリックします](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))。


[![テキスト ボックス内に配置する日付をクリックすると](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

テキスト ボックス内に配置する日付をクリックすると ([フルサイズの画像を表示する をクリックします](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))。

> [!div class="step-by-step"]
> [前へ](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [次へ](using-multiple-popup-controls-vb.md)
