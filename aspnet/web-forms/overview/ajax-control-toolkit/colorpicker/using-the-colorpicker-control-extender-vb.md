---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: ColorPicker コントロール エクステンダー (VB) を使用して |Microsoft ドキュメント
author: microsoft
description: ColorPicker は、popup コントロールの UI を使用したクライアント側の色を選択機能を提供する ASP.NET AJAX エクステンダーです。 すべての ASP.NET にアタッチできます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-vb"></a>ColorPicker コントロール エクステンダー (VB) を使用します。
====================
によって[Microsoft](https://github.com/microsoft)

> ColorPicker は、popup コントロールの UI を使用したクライアント側の色を選択機能を提供する ASP.NET AJAX エクステンダーです。 これは、ASP.NET のテキスト ボックス コントロールにアタッチできます。 です。


このチュートリアルの目的では、AJAX コントロール Toolkit ColorPicker コントロール エクステンダーの使用方法について説明します。 ColorPicker コントロール エクステンダーは、色を選択することができますポップアップ ダイアログ ボックスを表示します。 ColorPicker は、色を選択するユーザーの直感的なユーザー インターフェイスを提供する場合に便利です。

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>ColorPicker コントロール エクステンダーを持つテキスト ボックス コントロールを拡張します。

たとえば、ユーザーがカスタマイズされたビジネス カードを作成できるようにする web サイトを作成することに想像してください。 閲覧者は、名刺のテキストを入力し、色を選択します。 リスト 1 の ASP.NET ページには、2 つのテキスト ボックス コントロールが txtCardText および txtCardColor という名前が含まれています。 フォームを送信すると、選択した値が表示されます (図 1 を参照してください)。


[![名刺を作成するための簡単なフォーム](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**図 01**: 名刺を作成するための単純な形式 ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image2.png))


**1 - CreateCard.aspx を一覧表示します。**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

フォーム リスト 1 動作しますが、これには、優れたユーザー エクスペリエンスを提供しません。 テキスト ボックスに、色を入力するユーザーがいます。 ユーザーが特殊な色の場合など、pea 緑では、ユーザーの右側の網かけだけ考える必要がありますのヘルプを表示しない HTML 色コード。

ColorPicker コントロール エクステンダーを使用するより優れたユーザー エクスペリエンスを提供します。 テキスト ボックス コントロールにフォーカスを移動すると、ColorPicker に色のダイアログが表示されます (図 2 を参照してください)。


[![ColorPicker コントロール エクステンダー](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**図 02**:「ColorPicker コントロール エクステンダー ([フルサイズ イメージを表示するに、をクリックして](using-the-colorpicker-control-extender-vb/_static/image4.png))


1 の一覧で、フォームを ColorPicker コントロール エクステンダーを使用する 2 つの手順を完了する必要があります。

1. ページに ScriptManager コントロールを追加します。
2. ColorPicker コントロール エクステンダーをページに追加します。

ColorPicker を使用するには、ページに ScriptManager を追加する必要があります。 開くサーバー側のすぐ下は、ScriptManager を追加するに適して&lt;フォーム&gt;タグ。 ([AJAX Extensions] タブでは、ScriptManager があります)、ツールボックスから、ScriptManager をページにドラッグできます。 また、開始サーバー側 form タグの下にソース ビューに、次のタグを入力することができます。

&lt;asp ScriptManager: ID ="ScriptManager1"runat ="server"/&gt;

ColorPicker コントロール エクステンダーをページに追加する最も簡単な方法は、デザイン ビューでです。 マウス txtCardColor ボックスにポインターを移動する場合は、スマート タスク オプションは、ではで表示されます。 extender を追加できます (図 3 を参照してください)。 このオプションを選択した場合、Extender ウィザードでは、(図 4 を参照) が表示されます。


[![Extender の追加](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**図 03**: extender の追加 ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Extender のウィザードを使用してコントロール エクステンダーを選択します。](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**図 04**: Extender ウィザードを使用してコントロール エクステンダーを選択すると ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image8.png))


ColorPicker エクステンダー txtCardColor テキスト ボックスを拡張する ColorPicker extender を選択することができます。 ダイアログ ボックスを閉じるには、[ok] をクリックします。

これらの変更を加えた後、ページのソースを一覧表示する 2 に似ています。

**2 - (ColorPicker) を持つ CreateCard.aspx を一覧表示します。**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

ページに今すぐ txtCardColor TextBox コントロールのすぐ下に表示されている ColorPickerExtender コントロールが含まれていることを確認します。 ColorPickerExtender コントロールは、カラー ピッカー ダイアログ ボックスを表示するように、txtCardColor コントロールを拡張します。

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>カラー ピッカー ダイアログを起動するボタンを使用してください。

ColorPicker extender は、次のプロパティをサポートします。

- PopupButtonId - カラー ピッカー ダイアログを表示すると、ページ上のボタンの ID。
- PopupPosition - カラー ピッカー ダイアログ ボックスのターゲット コントロールに対する相対的な位置です。 使用可能な値は絶対パス、Center、斜め、BottomRight、左上端、TopRight、右、および左 (既定では斜め) です。
- SampleControlId - 選択した色を表示するコントロールの ID。
- SelectedColor -、ColorPicker が選択した最初の色。

これらのプロパティを使用して、カラー ピッカー ダイアログ ボックスを表示する方法と、選択した色の表示方法をカスタマイズすることができます。 ページ 3 の一覧表示するのには、これらのプロパティのいくつかを使用する方法を示しています。

**3 - CreateCardButton.aspx を一覧表示します。**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

一覧表示する 3 ページには、色選択にはが含まれています (図 5 を参照してください) ボタンをクリックします。 このボタンをクリックすると、テキスト ボックスの上カラー ピッカー ダイアログ ボックスが表示されます。 ダイアログ ボックスで色を選択する場合は、lblSample ラベル コントロールの背景色として選択した色が表示されます。

ColorPicker PopupButtonID プロパティを使用して、色の選択 ボタンを関連付ける ColorPicker extender を使用します。 PopupButtonID プロパティの値を指定するときに、カラー ピッカー ダイアログは表示されなくなりますターゲット コントロールにフォーカスがある場合。 ダイアログ ボックスを表示するボタンをクリックする必要があります。

SampleControlID プロパティは、ColorPicker で選択した色を表示するコントロールを関連付けるために使用します。 ColorPicker は、現在選択されている色に、このコントロールの背景色を変更します。


[![カラー ピッカー ダイアログ ボタンを表示します。](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**図 05**: カラー ピッカー ダイアログ ボタンを表示する ([フルサイズのイメージを表示するをクリックして](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>まとめ

このチュートリアルでは、ColorPicker コントロール エクステンダーを使用して、ポップアップ カラー ピッカー ダイアログ ボックスを表示する方法を学習します。 最初に、テキスト ボックス コントロールにフォーカスを移動、ダイアログ ボックスを表示する方法調べられます。 次に、ボタンがクリックされたときに、カラー ピッカー ダイアログ ボックスを表示するボタンを作成する方法を学習します。

> [!div class="step-by-step"]
> [前へ](using-the-colorpicker-control-extender-cs.md)
