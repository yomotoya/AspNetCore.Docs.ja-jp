---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: "UpdatePanel (VB) でのポップアップ コントロールからのポストバックを処理する |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、する必要があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 4445437f25925429d240b7fe2d019855afc52fe0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>UpdatePanel (VB) でのポップアップ コントロールからのポストバックを処理します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、このようなポップアップでポストバックが発生したときにする必要があります。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、このようなポップアップでポストバックが発生したときにする必要があります。

## <a name="steps"></a>手順

使用する場合、 `PopupControl` 、ポストバック時に、`UpdatePanel`ポストバックによるページの更新を防ぐことができます。 次のマークアップでは、いくつかの重要な要素を定義します。

- A `ScriptManager` ASP.NET AJAX コントロール Toolkit が動作するように制御
- 2 つ`TextBox`ポップアップがトリガーされます両方を管理します
- A`Panel`ポップアップ画面として使用するコントロール
- パネル内で、`Calendar`内でコントロールが埋め込まれている、`UpdatePanel`コントロール
- 2 つ`PopupControlExtender`コントロールのテキスト ボックスに、パネルを割り当てる

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

なお、`OnSelectionChanged`の属性、`Calendar`コントロールが設定されています。 ポストバックが発生した、ユーザーは、カレンダーで日付を選択するとき、およびサーバー側メソッド`c1_SelectionChanged()`を実行します。 このメソッドは、内には、現在の日付を取得し、テキスト ボックスに書き戻さして必要があります。

そのための構文は次のように、: まず、プロキシ オブジェクトを`PopupControlExtender` ページを生成する必要があります。 ASP.NET AJAX コントロール Toolkit には、`GetProxyForCurrentPopup()`メソッドです。 このメソッドが返すオブジェクトをサポートしている、`Commit()`ポップアップ (コントロールではなく、メソッドの呼び出しをトリガーした!) をトリガーしたコントロールに値を送信するメソッド。 次のコードは、選択した日付を引数として、`Commit()`メソッド、コードのテキスト ボックスに、選択した日付を書き戻しがします。

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

今すぐ、関連付けられているテキスト ボックスに、選択した日付が表示されます、カレンダーの日付をクリックするたびに日付の選択コントロールを作成することができます現在に見つかりません多くの web サイト。


[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))


[![日付をクリックすると、テキスト ボックスに配置します。](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

日付をクリックすると、テキスト ボックスに配置 ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

>[!div class="step-by-step"]
[前へ](using-multiple-popup-controls-vb.md)
[次へ](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
