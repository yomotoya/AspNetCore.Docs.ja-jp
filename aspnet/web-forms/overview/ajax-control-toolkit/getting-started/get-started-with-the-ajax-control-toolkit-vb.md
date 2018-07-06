---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: AJAX Control Toolkit (VB) の概要 |Microsoft Docs
author: microsoft
description: AJAX Control Toolkit の使用を開始するために必要なについて説明します。
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 65743c9eb8eb1b02c6cf87da53222af1312f4445
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822080"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>AJAX Control Toolkit (VB) の概要します。
====================
によって[Microsoft](https://github.com/microsoft)

> AJAX Control Toolkit の使用を開始するために必要なについて説明します。


AJAX Control Toolkit には、30 を超える無料コントロール、ASP.NET アプリケーションで使用できるが含まれています。 このチュートリアルでは、AJAX Control Toolkit をダウンロードして、toolkit のコントロールを Visual Studio または Visual Web Developer Express ツールボックスに追加する方法について説明します。

## <a name="downloading-the-ajax-control-toolkit"></a>AJAX Control Toolkit のダウンロード

[AJAX Control Toolkit](http://devexpress.com/act)は、ASP.NET コミュニティと ASP.NET チームのメンバーによって開発されたオープン ソース プロジェクトです。


[![AJAX Control Toolkit のダウンロード](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**図 01**: AJAX Control Toolkit のダウンロード ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))。


ファイルをダウンロードした後、ファイルのブロックを解除する必要があります。 ファイルを右クリックし、プロパティを選択してをクリックして、**ブロック解除**(図 2 参照) ボタンをクリックします。


[![AJAX Control Toolkit の ZIP ファイルをブロック解除](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**図 02**: AJAX Control Toolkit の ZIP ファイルをブロック解除 ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))。


ファイルのブロックを解除すると後、は、ファイルを解凍できます: ファイルを右クリックして、**すべて展開**メニュー オプション。 ここで、Visual Studio または Visual Web Developer ツールボックスにこのツールキットを追加する準備が整いました。

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>AJAX Control Toolkit をツールボックスに追加します。

AJAX Control Toolkit を使用する最も簡単な方法は、Visual Studio または Visual Web Developer ツールボックスにこのツールキットを追加する、(図 3 を参照してください)。 そうすることだけドラッグできます toolkit のコントロールをページにそれを使用する場合。


[![AJAX Control Toolkit がツールボックスに表示されます。](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**図 03**: AJAX Control Toolkit がツールボックスに表示されます ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))。


最初に、AJAX Control Toolkit の タブをツールボックスに追加する必要があります。 これらの手順に従います。

1. メニュー オプション ファイルを新しい web サイトを選択して、新しい ASP.NET web サイトを作成します。 ソリューション エクスプ ローラー ウィンドウで、エディターでファイルを開くの Default.aspx をダブルクリックします。
2. [全般] タブの下にあるツールボックスを右クリックし、メニュー オプションを選択**タブの追加**(図 4 参照)。
3. AJAX Control Toolkit という名前の新しいタブを入力します。


[![新しいタブを追加します。](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**図 04**: 新しいタブの追加 ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))。


次に、新しいタブに、AJAX Control Toolkit コントロールを追加する必要があります。この場合は、以下の手順に従ってください。

- AJAX Control Toolkit] タブの下を右クリックし、メニュー オプションを選択 **[アイテムの選択 (図 5 参照)** します。
- AJAX Control Toolkit を解凍するを AjaxControlToolkit.dll アセンブリを選択する場所に移動します。


[![ツールボックスに追加する項目を選択します。](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**図 05**: ツールボックスに追加する項目を選択 ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))。


次の手順を完了するは、ツールボックスに toolkit のコントロールのすべて表示されます。

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>ツールキットの新しいバージョンにアップグレードします。

移動する必要があり、旧バージョンのツールキットを使用していた場合、以降のバージョンとここでは推奨される手順を示します。

- バイナリには、web サイトの Bin フォルダーから AjaxControlToolkit.dll アセンブリの古いバージョンを削除します。
- ツールボックス項目には、AJAX Control Toolkit タブを削除し、AjaxControlToolkit.dll アセンブリの新しいバージョンで、タブの再作成に上記の手順を実行します。

> [!div class="step-by-step"]
> [前へ](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [次へ](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
