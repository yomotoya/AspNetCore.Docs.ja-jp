---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: (VB) Repeater で ConfirmButton を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で ConfirmButton エクステンダーは、[はい] を作成します]、[ユーザーがボタンをクリックするとポップアップなどありません LinkButton コントロール。 場合にのみが [はい] がしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: ce403e84766f586eca36ef6bc513d9fbf7bd1d40
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396420"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>(VB) Repeater で ConfirmButton を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> AJAX Control Toolkit で ConfirmButton エクステンダーは、[はい] を作成します]、[ユーザーがボタンをクリックするとポップアップなどありません LinkButton コントロール。 のみ [はい] がクリックされたボタンのアクションが実行、それ以外の場合はキャンセルされます。 これは、repeater で可能性があります。


## <a name="overview"></a>概要

AJAX Control Toolkit で ConfirmButton エクステンダーは、[はい] を作成します]、[ユーザーがボタンをクリックするとポップアップなどありません LinkButton コントロール。 のみ [はい] がクリックされたボタンのアクションが実行、それ以外の場合はキャンセルされます。 これは、repeater で可能性があります。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。

このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

次に、データ ソースが必要です。 わかりやすく、AdventureWorks の仕入先のテーブル内の最初の 5 つのエントリのみが取得されます。 Visual Studio のウィザードを使用して、データ ソース、テーブル名を作成するときに注意してください (`Vendors`) は現在正しくプレフィックスが付いていない`Purchasing`します。 次のマークアップは、しいものを示します。

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

このデータ ソースは、repeater 内で使用できます。 通常どおり、`DataBinder.Eval()`メソッドは、データ ソースからデータを取得します。 `ConfirmButtonExtender`内でコントロールを配置し、必要があります、`<ItemTemplate>`データ ソース内のエントリごとに表示されるように、repeater のセクション。

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![データ ソースからの各エントリの横にある [確認] ボタンが表示されます。](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

データ ソースからの各エントリの横にある [確認] ボタンが表示されます ([フルサイズの画像を表示する をクリックします](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](using-a-confirmbutton-in-a-repeater-cs.md)
