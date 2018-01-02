---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: "AJAX コントロール Toolkit コントロールおよびコントロール エクステンダー (VB) を使用して |Microsoft ドキュメント"
author: microsoft
description: "ASP.NET ページにコントロールの AJAX コントロール ツールキットおよびエクステンダーを追加する方法を説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>AJAX コントロール Toolkit コントロールおよびコントロール エクステンダー (VB) を使用してください。
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET ページにコントロールの AJAX コントロール ツールキットおよびエクステンダーを追加する方法を説明します。


AJAX コントロール Toolkit には、コントロールおよびコントロール エクステンダーのセットが含まれています。 この簡単なチュートリアルでは、ASP.NET ページにコントロールおよびコントロール エクステンダーの両方を追加する方法を学習します。

> [!NOTE] 
> 
> AJAX コントロール Toolkit をインストールして、AJAX コントロール Toolkit を Visual Studio または Visual Web Developer ツールボックスに追加する手順については、チュートリアルを参照してください[AJAX コントロール ツールキットを使用して開始](get-started-with-the-ajax-control-toolkit-vb.md)です。


## <a name="using-ajax-control-toolkit-controls"></a>AJAX コントロール Toolkit コントロールを使用します。

AJAX コントロール Toolkit コントロールは、通常の ASP.NET コントロールと同じように動作します。 ASP.NET ページには、ツールボックスからコントロールをドラッグすることができます。 デザイン ビューまたはソース ビュー内のページにコントロールを追加できます。

1 つの特別な要件がある、AJAX コントロール Toolkit からコントロールを使用する場合。 ページには、ScriptManager コントロールを含める必要があります。 ScriptManager コントロールは、AJAX コントロール Toolkit コントロールに必要なために必要な JavaScript のすべてを含む担当します。

たとえば、AJAX コントロール ツールキット タブには、エディター コントロールをという名前のコントロールが含まれています。 このコントロールには、豊富な HTML エディターが表示されます。 エディター コントロールをページに追加する手順に従います。

1. ShowEditor.aspx をという名前の新しい ASP.NET ページを作成します。
2. ツールボックスで、[AJAX Extensions] タブの下から ScriptManager コントロールを選択し、コントロールをページにドラッグします。
3. ツールボックスで、AJAX コントロール ツールキット タブの下からエディター コントロールを選択し、コントロールをページにドラッグ (図 1 を参照してください)。 デザイナーは、図 2 のようになります。
4. メニュー オプションを選択して、web サイトを実行**デバッグ、デバッグの開始** F5 キーを押すか。
5. 図 3 にページが表示されます。


[![HTML エディター コントロールを選択します。](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**図 01**: HTML エディター コントロールを選択すると ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![ScriptManager およびエディット コントロールで visual Studio デザイナー](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**図 02**: ScriptManager およびエディット コントロールで Visual Studio デザイナー ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![DisplayEditor.aspx ページ](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**図 03**:「DisplayEditor.aspx ページ ([フルサイズのイメージを表示するには、をクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>AJAX コントロール Toolkit コントロール エクステンダーを使用してください。

AJAX コントロール Toolkit には、コントロール エクステンダーも含まれています。 その名前からわかるように、コントロール エクステンダーは既存のコントロールの機能を拡張します。 たとえば、ConfirmButton コントロール エクステンダーは、標準の ASP.NET ボタン コントロールを拡張します。 Extender は、ボタンがクリックすると、確認のダイアログ ボックスを表示できるように、ボタン コントロールの動作を変更します。

AJAX コントロール Toolkit コントロールと同じように、コントロールの拡張には、ScriptManager コントロールが必要です。 ページでのコントロール エクステンダーの使用を開始する前に、ページに ScriptManager コントロールを追加する必要があります。

ConfirmButton コントロール エクステンダーを使用して次の手順に従います。

1. ShowConfirmButton.aspx をという名前の新しい ASP.NET ページを作成します。
2. [AJAX Extensions] タブの下からページにコントロールをドラッグして、ページに ScriptManager コントロールを追加します。
3. デザイナー画面には、ツールボックスで、[標準] タブの下からボタンをドラッグして、ページに標準のボタン コントロールを追加します。
4. クリックして、 **Extender の追加**オプションのタスク (図 4 を参照してください)。
5. エクステンダーの選択 ダイアログ ボックスで選択 ConfirmButtonExtender (図 5 を参照してください)、ok ボタンをクリックします。
6. デザイナーでボタン コントロールを選択し、Extender、Button1 を展開\_ConfirmButtonExtender ノード プロパティ ウィンドウ (図 6 を参照してください)。 値を割り当てる*本当に?* ConfirmText プロパティにします。
7. メニュー オプションを選択して、ページの実行**デバッグ、デバッグの開始**か、F5 キーを押すとします。


[![Extender の追加タスク オプション](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**図 04**: Extender の追加タスク オプション ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![ConfirmButton コントロール エクステンダーを選択します。](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**図 05**: ConfirmButton コントロール エクステンダーを選択すると ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![ConfirmButton プロパティの設定](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**図 06**: ConfirmButton プロパティの設定 ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


ページが開いたら、ボタンが表示されます。 ボタンをクリックすると、図 7 に確認のダイアログ ボックスを取得します。


[![確認のダイアログ ボックスを表示します。](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**図 07**: 確認のダイアログ ボックスを表示する ([フルサイズのイメージを表示するをクリックして](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


通常ドラッグしないコントロール エクステンダーをページに注意してください。 代わりに、使用する、 **Extender の追加**エクステンダーをページに既に追加したコントロールに追加するオプションのタスクです。 さらに、あるプロパティを設定するコントロール エクステンダー拡張されるコントロールのプロパティ シートを開いてに注意してください。

1 つの ASP.NET コントロールは、複数のコントロール エクステンダーによって拡張できます。 拡張されるコントロールのプロパティ シートでは、コントロールに関連付けられたコントロール エクステンダーのすべてを一覧表示します。

>[!div class="step-by-step"]
[前へ](get-started-with-the-ajax-control-toolkit-vb.md)
[次へ](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
