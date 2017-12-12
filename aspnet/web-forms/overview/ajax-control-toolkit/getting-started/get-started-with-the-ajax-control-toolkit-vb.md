---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: "AJAX コントロール ツールキット (VB) を使用して作業を開始 |Microsoft ドキュメント"
author: microsoft
description: "AJAX コントロール ツールキットを使用して作業を開始するために知っておく必要がありますすべてを説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 0bbf6dc0be8a96ecd47b8620a6ba3220b50f10d4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>AJAX コントロール Toolkit (VB) の概要します。
====================
によって[Microsoft](https://github.com/microsoft)

> AJAX コントロール ツールキットを使用して作業を開始するために知っておく必要がありますすべてを説明します。


AJAX コントロール Toolkit には、ASP.NET アプリケーションで使用できる 30 以上の空きコントロールが含まれます。 このチュートリアルでは、AJAX コントロール ツールキットをダウンロードして、toolkit のコントロールを Visual Studio または Visual Web Developer Express ツールボックスに追加する方法を学習します。

## <a name="downloading-the-ajax-control-toolkit"></a>AJAX コントロール Toolkit のダウンロード

[AJAX コントロール Toolkit](http://devexpress.com/act)は、ASP.NET コミュニティや ASP.NET のチームのメンバーによって開発されたオープン ソース プロジェクトです。


[![AJAX コントロール Toolkit のダウンロード](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**図 01**: AJAX コントロール Toolkit のダウンロード ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


ファイルをダウンロードした後は、ファイルのブロックを解除する必要があります。 ファイルを右クリックし、プロパティを選択し、クリックして、**ブロックを解除する**(図 2 を参照してください) ボタンをクリックします。


[![AJAX コントロール Toolkit の ZIP ファイルのブロックを解除](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**図 02**: AJAX コントロール Toolkit の ZIP ファイルのブロックを解除 ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


ファイルのブロックを解除すると後、は、ファイルを解凍することができます: ファイルを右クリックして、**すべて展開**メニュー オプション。 ここで、このツールキットを Visual Studio または Visual Web Developer ツールボックスに追加する準備が整いました。

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>ツールボックスへの AJAX コントロール Toolkit の追加

AJAX コントロール ツールキットを使用する最も簡単な方法は、Visual Studio または Visual Web Developer ツールボックスにツールキットを追加する (図 3 を参照してください)。 こうすれば、単にドラッグできます toolkit コントロール ページにそれを使用する場合。


[![AJAX コントロール Toolkit ツールボックスに表示されます。](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**図 03**: AJAX コントロール Toolkit ツールボックスに表示されます ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


最初に、AJAX コントロール ツールキット タブがツールボックスに追加する必要があります。 これらの手順に従います。

1. 新しい web サイト メニュー オプション、ファイルを選択して、新しい ASP.NET web サイトを作成します。 エディターでファイルを開くソリューション エクスプ ローラー ウィンドウで、Default.aspx をダブルクリックします。
2. [全般] タブの下にあるツールボックスを右クリックし、メニュー オプションを選択**タブの追加**(図 4 を参照してください)。
3. AJAX コントロール ツールキットをという名前の新しいタブを入力します。


[![新しいタブを追加します。](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**図 04**: 新しいタブの追加 ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


次に、新しいタブに AJAX コントロール Toolkit コントロールを追加する必要があります。この場合は、以下の手順に従ってください。

- AJAX コントロール ツールキット] タブの下を右クリックし、メニュー オプションを選択**[アイテムの選択 (図 5 を参照してください)**です。
- AJAX コントロール ツールキットを解凍し、選択 AjaxControlToolkit.dll アセンブリの場所を参照します。


[![ツールボックスに追加する項目を選択します。](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**図 05**: ツールボックスに追加する項目の選択 ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


これらの手順を実行するツールキット コントロールのすべて、ツールボックスに表示されます。

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>ツールキットの新しいバージョンにアップグレードします。

ツールキットの古いリリースを使用していたしてに移動する必要があります、以降のバージョンは、ここは推奨される手順を示します。

- バイナリには、web サイトの Bin フォルダーから AjaxControlToolkit.dll アセンブリの古いバージョンを削除します。
- ツールボックス項目では、AJAX コントロール ツールキット タブを削除し、AjaxControlToolkit.dll アセンブリの新しいバージョンで、タブの再作成に上記の手順を実行します。

>[!div class="step-by-step"]
[前へ](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[次へ](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
