---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: "Repeater コントロール (c#) で ModalPopup の使用 |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 この contr. を使用することも."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: d945b8decf4debbcbf415163e486e2bce8097701
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>リピータ コントロール (c#) で ModalPopup の使い方
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 リピータ内でこのコントロールを使用することもできます。


## <a name="overview"></a>概要

AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 リピータ内でこのコントロールを使用することもできます。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?表示 = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。 このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。 セットアップが異なっている場合は、データベースの接続情報を調整する必要です。 ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

次に、ページに、データ ソースを追加します。 限られた量のデータを使用するのにはのみ、AdventureWorks データベースの仕入先テーブルの最初の 5 つのエントリを選択します。 データ ソースを作成する Visual Studio のアシスタントを使用している場合、バグを現在のバージョンでは、テーブル名のプレフィックスにしないことに注意してください (`Vendor`) と`Purchasing`です。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

次に、モーダル ポップアップとして機能するパネルを追加します。 含まれている、`Button`コントロールをもう一度、ポップアップ ウィンドウを閉じます。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

リピータ内での作業のポップアップを作成するために、`ModalPopupExtender`内にコントロールを配置する必要があります、`<ItemTemplate>`リピータのセクションでします。 パネルがリピータ、範囲外ですが、extender が内部です。 リピータのマークアップを次に示します。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

次に、データ ソース内のすべての項目がモーダル ポップアップをトリガーする、その横のボタンに表示されます。


[![すべてのデータ ソースのエントリのモーダル ポップアップをトリガーできます。](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

すべてのデータ ソースのエントリのモーダル ポップアップをトリガーできます ([フルサイズのイメージを表示するをクリックして](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[前へ](launching-a-modal-popup-window-from-server-code-cs.md)
[次へ](handling-postbacks-from-a-modalpopup-cs.md)
