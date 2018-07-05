---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: (C#)、UpdatePanel のポップアップ コントロールからポストバックを処理する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、する必要があります.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9fdf2821e005bfcb691134bb6994c01fa72b7abd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815383"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>(C#)、UpdatePanel のポップアップ コントロールからポストバックを処理します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、このようなポップアップ内ポストバックが発生した場合に実行する必要があります。


## <a name="overview"></a>概要

AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、このようなポップアップ内ポストバックが発生した場合に実行する必要があります。

## <a name="steps"></a>手順

使用する場合、 `PopupControl` 、ポストバックに、`UpdatePanel`ポストバックの原因となったページの更新を防ぐことができます。 次のマークアップでは、いくつかの重要な要素を定義します。

- A `ScriptManager` ASP.NET AJAX Control Toolkit が動作するように制御
- 2 つ`TextBox`ポップアップをトリガーする両方のコントロール
- A`Panel`ポップアップとして機能するコントロール
- パネル内で、`Calendar`内でコントロールが埋め込まれている、`UpdatePanel`コントロール
- 2 つ`PopupControlExtender`コントロール、テキスト ボックスに、パネルを割り当てる

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

なお、`OnSelectionChanged`の属性、`Calendar`コントロールを設定します。 ポストバックが発生したときに、ユーザーは、カレンダーで日付を選択するようにし、サーバー側メソッド`c1_SelectionChanged()`を実行します。 メソッド内には、現在の日付を取得し、テキスト ボックスに書き戻されるしてする必要があります。

そのための構文は次のように、: すべてのプロキシの最初のオブジェクト、 `PopupControlExtender`  ページを生成する必要があります。 ASP.NET AJAX Control Toolkit の提供、`GetProxyForCurrentPopup()`メソッド。 このメソッドが返すオブジェクトのサポート、`Commit()`メソッド (コントロールではなく、メソッドの呼び出しをトリガーした!) ポップアップをトリガーしたコントロールに値を送信します。 次のコードは、選択した日付を引数として、`Commit()`いるため、テキスト ボックスに、選択した日付を書き戻すコード メソッド。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

カレンダーの日付では、関連付けられているテキスト ボックスで、選択した日付が表示されます をクリックするたびに日付の選択コントロールを作成することができます現在入手多くの web サイト。


[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズの画像を表示する をクリックします](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))。


[![テキスト ボックス内に配置する日付をクリックすると](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

テキスト ボックス内に配置する日付をクリックすると ([フルサイズの画像を表示する をクリックします](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))。

> [!div class="step-by-step"]
> [前へ](using-multiple-popup-controls-cs.md)
> [次へ](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
