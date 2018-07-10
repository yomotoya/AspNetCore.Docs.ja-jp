---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: AJAX Control Toolkit のコントロールとコントロール エクステンダー (c#) を使用して |Microsoft Docs
author: microsoft
description: ASP.NET ページに AJAX Control Toolkit のコントロールとエクステンダーを追加する方法について説明します。
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c978bec3780ef2e83aee32a084d1baf5066e0e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817227"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>AJAX Control Toolkit のコントロールとコントロール エクステンダー (c#) を使用
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET ページに AJAX Control Toolkit のコントロールとエクステンダーを追加する方法について説明します。


AJAX Control Toolkit には、コントロールとコントロール エクステンダーのセットが含まれています。 この簡単なチュートリアルでは、ASP.NET ページにコントロールとコントロール エクステンダーの両方を追加する方法について説明します。

> [!NOTE] 
> 
> AJAX Control Toolkit をインストールして、AJAX Control Toolkit を Visual Studio または Visual Web Developer ツールボックスに追加する手順については、チュートリアルを参照してください。 [、AJAX Control Toolkit の概要](get-started-with-the-ajax-control-toolkit-cs.md)します。


## <a name="using-ajax-control-toolkit-controls"></a>AJAX Control Toolkit のコントロールを使用します。

通常の ASP.NET コントロールと同様、AJAX Control Toolkit コントロールが動作します。 ASP.NET ページには、ツールボックスからコントロールをドラッグすることができます。 デザイン ビューまたはソース ビュー内のページにコントロールを追加することができます。

1 つの特別な要件がある、AJAX Control Toolkit のコントロールを使用する場合。 ページには、ScriptManager コントロールを含める必要があります。 ScriptManager コントロールは、AJAX Control Toolkit コントロールに必要なために必要な JavaScript のすべてを含む責任を負います。

たとえば、AJAX Control Toolkit タブには、エディター コントロールをという名前のコントロールが含まれています。 このコントロールには、リッチ HTML エディターが表示されます。 エディター コントロールをページに追加するのに次の手順に従います。

1. ShowEditor.aspx をという名前の新しい ASP.NET ページを作成します。
2. ツールボックスで、[AJAX Extensions] タブの下から、ScriptManager コントロールを選択し、ページにコントロールをドラッグします。
3. ツールボックスで、AJAX Control Toolkit タブの下からエディター コントロールを選択し、ページ上にコントロールをドラッグします (図 1 参照)。 デザイナーは、図 2 のようになります。
4. メニュー オプションを選択して、web サイトを実行**デバッグ、デバッグの開始**または F5 キーを押します。
5. 図 3 のページが表示されます。


[![HTML エディター コントロールの選択](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**図 01**: HTML エディター コントロールを選択 ([フルサイズの画像を表示する をクリックします](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))。


[![Visual Studio のデザイナーで編集して ScriptManager コントロール](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**図 02**: 編集して ScriptManager コントロールでの Visual Studio デザイナー ([フルサイズの画像を表示する をクリックします](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))。


[![DisplayEditor.aspx ページ](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**図 03**: The DisplayEditor.aspx ページ ([フルサイズの画像を表示する をクリックします](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))。


## <a name="using-ajax-control-toolkit-control-extenders"></a>AJAX Control Toolkit コントロール エクステンダーを使用します。

AJAX Control Toolkit には、コントロール エクステンダーも含まれています。 その名前からわかるように、コントロール エクステンダーは、既存のコントロールの機能を拡張します。 たとえば、ConfirmButton コントロール エクステンダーは、標準の ASP.NET のボタン コントロールを拡張します。 エクステンダーは、ボタンのコントロールの動作を変更して、ボタンをクリックすると、確認のダイアログ ボックスを表示するようにします。

AJAX Control Toolkit コントロールと同様、コントロールのエクステンダーでは、ScriptManager コントロールが必要です。 ページ コントロール エクステンダーを使用する前に、ページに ScriptManager コントロールを追加する必要があります。

ConfirmButton コントロール エクステンダーを使用して、次の手順に従います。

1. ShowConfirmButton.aspx をという名前の新しい ASP.NET ページを作成します。
2. AJAX Extensions タブの下からページにコントロールをドラッグして、ページに ScriptManager コントロールを追加します。
3. デザイナー画面には、ツールボックスの [標準] タブの下から、ボタンをドラッグして、ページに標準のボタン コントロールを追加します。
4. をクリックして、 **Extender の追加**タスク オプション (図 4 参照)。
5. エクステンダーの選択 ダイアログ ボックスで、ConfirmButtonExtender を選択します (図 5 を参照してください)、ok ボタンをクリックします。
6. デザイナーでボタン コントロールを選択し、エクステンダー、Button1 の展開\_ConfirmButtonExtender ノードで、[プロパティ] ウィンドウ (図 6 参照)。 値を割り当てる*本当ですか?* ConfirmText プロパティにします。
7. メニュー オプションを選択して、ページの実行**デバッグ、デバッグの開始**または F5 キーを押します。


[![Extender の追加のタスク オプション](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**図 04**: Extender の追加タスク オプション ([フルサイズの画像を表示する をクリックします](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))。


[![ConfirmButton コントロール エクステンダーを選択します。](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**図 05**: ConfirmButton コントロール エクステンダーを選択すると ([フルサイズの画像を表示する をクリックします](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))。


[![ConfirmButton プロパティの設定](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**図 06**: ConfirmButton プロパティの設定 ([フルサイズの画像を表示する をクリックします](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))。


ページが開いたら、ボタンが表示されます。 ボタンをクリックすると、図 7 確認のダイアログ ボックスを取得します。


[![確認のダイアログ ボックスを表示します。](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**図 07**: 確認のダイアログ ボックスを表示する ([フルサイズの画像を表示する をクリックします](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))。


通常ドラッグしないコントロール エクステンダーをページに注意してください。 代わりに、使用、 **Extender の追加**タスク ページに既に追加したコントロールにエクステンダーを追加するオプション。 さらに、設定することコントロール エクステンダー プロパティを開いて拡張されるコントロールのプロパティ シートに注意してください。

1 つの ASP.NET コントロールは、複数のコントロール エクステンダーによって拡張できます。 拡張されるコントロールのプロパティ シートでは、コントロールに関連付けられたコントロール エクステンダーのすべてを一覧表示します。

> [!div class="step-by-step"]
> [前へ](get-started-with-the-ajax-control-toolkit-cs.md)
> [次へ](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
