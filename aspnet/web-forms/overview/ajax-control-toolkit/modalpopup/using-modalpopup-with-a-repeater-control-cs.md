---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: (C#) Repeater コントロールで ModalPopup を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 この contr. を使用することも.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 72d0f16d22911d867f9a91faf273e236453e7b3a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833625"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>(C#) Repeater コントロールで ModalPopup を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 Repeater 内でこのコントロールを使用することもできます。


## <a name="overview"></a>概要

AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 Repeater 内でこのコントロールを使用することもできます。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。 このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。 ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

次に、ページにデータ ソースを追加します。 少量のデータを使用するためにはのみ、AdventureWorks データベースの Vendor テーブルの最初の 5 つのエントリを選択します。 データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

次に、モーダル ポップアップとして機能するパネルを追加します。 含まれている、`Button`コントロールをもう一度、ポップアップを閉じます。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

リピータ内でポップアップを作成するには、`ModalPopupExtender`内でコントロールを配置する必要があります、 `<ItemTemplate>` repeater のセクション。 ように、パネルが、repeater 外ですが、エクステンダーが内部です。 リピータのマークアップを次に示します。

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

次に、モーダル ポップアップをトリガーするように、その横にあるボタンで、データ ソース内のすべての項目が表示されます。


[![データ ソースのエントリごとに、モーダル ポップアップをトリガーできます。](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

データ ソースのエントリごとに、モーダル ポップアップをトリガーできます ([フルサイズの画像を表示する をクリックします](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](launching-a-modal-popup-window-from-server-code-cs.md)
> [次へ](handling-postbacks-from-a-modalpopup-cs.md)
