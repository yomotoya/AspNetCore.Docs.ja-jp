---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: ColorPicker コントロール エクステンダー (c#) を使用して |Microsoft Docs
author: microsoft
description: ColorPicker では、popup コントロールの UI を使用したクライアント側の色を選択機能を提供する ASP.NET AJAX エクステンダーです。 任意の ASP.NET にアタッチできます.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 58b8d581ed426227ed77435e22c84e9ea5d62ebe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833423"
---
<a name="using-the-colorpicker-control-extender-c"></a>ColorPicker コントロール エクステンダー (c#) を使用します。
====================
によって[Microsoft](https://github.com/microsoft)

> ColorPicker では、popup コントロールの UI を使用したクライアント側の色を選択機能を提供する ASP.NET AJAX エクステンダーです。 ASP.NET の TextBox コントロールにアタッチできます。 です。


このチュートリアルの目的では、AJAX Control Toolkit ColorPicker コントロール エクステンダーを使用する方法について説明します。 ColorPicker コントロール エクステンダーでは、色を選択することができます、ポップアップ ダイアログが表示されます。 ColorPicker は、ユーザーが色を選択するための直感的なユーザー インターフェイスを提供したい場合に便利です。

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>ColorPicker コントロール エクステンダーを TextBox コントロールを拡張します。

たとえば、ユーザーがカスタマイズされたビジネス カードを作成できるようにする web サイトを作成することに想像してください。 訪問者は、名刺のテキストを入力し、色を選択できます。 リスト 1 での ASP.NET ページには、txtCardText txtCardColor という 2 つのテキスト ボックス コントロールが含まれています。 フォームを送信すると、選択した値が表示されます (図 1 参照)。


[![ビジネスのカードを作成するための簡単なフォーム](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**図 01**: ビジネス カードを作成するための簡単なフォーム ([フルサイズの画像を表示する をクリックします](using-the-colorpicker-control-extender-cs/_static/image2.png))。


**1 - CreateCard.aspx を一覧表示します。**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

リスト 1 の動作がフォームでは、優れたユーザー エクスペリエンスは提供されません。 ユーザーは、テキスト ボックスに色を入力します。 場合は、ユーザーが特殊な色のなど pea 緑 - は、ユーザーの適切な網掛けだけが手助けがなくて HTML カラー コードを算出する必要があります。

ColorPicker コントロール エクステンダーを使用して、優れたユーザー エクスペリエンスを作成することができます。 TextBox コントロールにフォーカスを移動すると、ColorPicker に色のダイアログが表示されます (図 2 参照)。


[![ColorPicker コントロール エクステンダー](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**図 02**: The ColorPicker コントロール エクステンダー ([フルサイズの画像を表示する をクリックします](using-the-colorpicker-control-extender-cs/_static/image4.png))。


ColorPicker コントロール エクステンダーを使用して、リスト 1 でフォームを 2 つの手順を完了する必要があります。

1. ScriptManager コントロールをページに追加します。
2. ColorPicker コントロール エクステンダーをページに追加します。

ColorPicker を使用する前に、ページに、ScriptManager を追加する必要があります。 Scriptmanager コントロールを追加する点としては、開始のサーバー側のすぐ下&lt;フォーム&gt;タグ。 ([AJAX Extensions] タブでは、scriptmanager コントロールがあります)、ツールボックスから、ページ上に、scriptmanager コントロールをドラッグできます。 または、開始のサーバー側のフォーム タグの下に、ソース ビューに、次のタグを入力できます。

&lt;asp ScriptManager: ID ="ScriptManager1"runat ="server"/&gt;

ColorPicker コントロール エクステンダーをページに追加する最も簡単な方法は、デザイン ビューでです。 TxtCardColor テキスト ボックスの上にマウスを移動する場合は、スマート タスク オプションは、可能で表示されます。 エクステンダーを追加する (図 3 を参照してください)。 このオプションを選択する場合、Extender ウィザードでは、(図 4 参照) が表示されます。


[![Extender の追加](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**図 03**: extender の追加 ([フルサイズの画像を表示する をクリックします](using-the-colorpicker-control-extender-cs/_static/image6.png))。


[![エクステンダーのウィザードを使用してコントロール エクステンダーを選択します。](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**図 04**: エクステンダー ウィザードを使用してコントロール エクステンダーを選択すると ([フルサイズの画像を表示する をクリックします](using-the-colorpicker-control-extender-cs/_static/image8.png))。


ColorPicker エクステンダー txtCardColor テキスト ボックスを拡張する ColorPicker エクステンダーを選択することができます。 ダイアログ ボックスを閉じるには、[ok] をクリックします。

これらの変更を行った後、ページのソースは、リスト 2 のようになります。

2 - (ColorPicker) を含む CreateCard.aspx を一覧表示します。

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

ページに今すぐ txtCardColor TextBox コントロールのすぐ下に表示される ColorPickerExtender コントロールが含まれていることを確認します。 ColorPickerExtender コントロールは、カラー ピッカー ダイアログを表示するように、txtCardColor コントロールを拡張します。

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>カラー ピッカー ダイアログ ボックスを起動するのにボタンの使用

ColorPicker エクステンダーには、次のプロパティがサポートされています。

- PopupButtonId - カラー ピッカー ダイアログを表示すると、ページ上のボタンの ID。
- PopupPosition - カラー ピッカー ダイアログ ボックスのターゲット コントロールに対する相対的な位置。 使用可能な値は、Absolute、Center、斜め、BottomRight、左、右、右、左 (既定では斜め) です。
- SampleControlId - 選択した色を表示するコントロールの ID。
- SelectedColor - 初期の色、ColorPicker で選択されています。

これらのプロパティを使用して、カラー ピッカー ダイアログ ボックスを表示する方法と、選択した色の表示方法をカスタマイズすることができます。 リスト 3 のページでは、これらのプロパティのいくつかの使用方法を示します。

**3 - CreateCardButton.aspx を一覧表示します。**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

リスト 3 のページには、色選択にはが含まれています (図 5 を参照してください) ボタンをクリックします。 このボタンをクリックすると、テキスト ボックス上にカラー ピッカー ダイアログ ボックスが表示されます。 ダイアログ ボックスから色を選択する場合は、lblSample ラベル コントロールの背景色として、選択した色が表示されます。

ColorPicker PopupButtonID プロパティは、色の選択 ボタンを関連付ける ColorPicker エクステンダーに使用されます。 PopupButtonID プロパティの値を指定するときに、カラー ピッカー ダイアログ不要になった、ターゲット コントロールにフォーカスがある場合が表示されます。 ダイアログ ボックスを表示するボタンをクリックする必要があります。

SampleControlID プロパティは、関連付ける、ColorPicker で選択した色を表示するコントロールに使用されます。 ColorPicker は、現在選択されている色にこのコントロールの背景色を変更します。


[![ボタンのカラー ピッカー ダイアログを表示します。](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**図 05**: ボタン、カラー ピッカー ダイアログを表示する ([フルサイズの画像を表示する をクリックします](using-the-colorpicker-control-extender-cs/_static/image10.png))。


## <a name="summary"></a>まとめ

このチュートリアルでは、ColorPicker コントロール エクステンダーを使用して、ポップアップ カラー ピッカー ダイアログを表示する方法について説明しました。 最初に、TextBox コントロールにフォーカスが移動すると、ダイアログ ボックスを表示する方法について確認しました。 次に、ボタンがクリックされたときに、カラー ピッカー ダイアログ ボックスを表示するボタンを作成する方法を学習しました。

> [!div class="step-by-step"]
> [次へ](using-the-colorpicker-control-extender-vb.md)
