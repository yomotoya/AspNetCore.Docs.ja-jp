---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: (C#) Repeater コントロールで HoverMenu を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で HoverMenu コントロールには、単純なポップアップの効果が用意されています要素の上にマウス ポインターを移動する姿にポップアップが表示されます。 しています.。
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f3135eb52bab1550b1c89dd6ce62044640e80198
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831998"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a>(C#) Repeater コントロールで HoverMenu を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> AJAX Control Toolkit で HoverMenu コントロールには、単純なポップアップの効果が用意されています。 指定位置にある要素の上にマウス ポインターを合わせたときにポップアップが表示されます。 Repeater 内でこのコントロールを使用することもできます。


## <a name="overview"></a>概要

`HoverMenu` AJAX Control Toolkit でコントロールには、単純なポップアップの効果が用意されています。 指定位置にある要素の上にマウス ポインターを合わせたときにポップアップが表示されます。 Repeater 内でこのコントロールを使用することもできます。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。

このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

次に、ページにデータ ソースを追加します。 少量のデータを使用するためにはのみ、AdventureWorks データベースの Vendor テーブルの最初の 5 つのエントリを選択します。 データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

次に、モーダル ポップアップとして機能するパネルを追加します。

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

ここで、`HoverMenuExtender`が関与します。 Repeater の内、エクステンダーを配置する必要があります、独自のポップアップを取得するデータ ソース内のすべての要素、ように`<ItemTemplate>`セクション。 マークアップを次に示します。

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

データ ソース内のすべての項目をポップアップを右側に表示されます (`PopupPosition`属性) に 50 ミリ秒の遅延の後 (`PopDelay`属性)。


[![Repeater 内の各項目の横にあるホバー メニューが表示されます。](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

Repeater 内の各項目の横にあるホバー メニューが表示されます ([フルサイズの画像を表示する をクリックします](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [次へ](using-hovermenu-with-a-repeater-control-vb.md)
