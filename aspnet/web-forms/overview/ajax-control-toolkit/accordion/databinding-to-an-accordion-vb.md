---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: アコーディオン (VB) へのデータ バインド |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルには、w を宣言は、通常は.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: 948741e3b8618a642c440a527fddef825faf595e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826570"
---
<a name="databinding-to-an-accordion-vb"></a>アコーディオン (VB) へのデータ バインド
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルは通常、ページ自体内で宣言が、データ ソースへのバインドは柔軟性を提供します。


## <a name="overview"></a>概要

AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルは通常、ページ自体内で宣言が、データ ソースへのバインドは柔軟性を提供します。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。

このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

次に、ページにデータ ソースを追加します。 少量のデータを使用するためにはのみ、AdventureWorks データベースの Vendor テーブルの最初の 5 つのエントリを選択します。 データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

データ ソースの名前 (ID) に注意してください。 この非常に id で使用する必要があります、`DataSourceID`アコーディオン コントロールのプロパティ。

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

アコーディオン コントロール内では、さまざまなパーツのヘッダーを含む、コントロールのテンプレートを行うことができます (`<HeaderTemplate>`) とコンテンツ (`<ContentTemplate>`)。 これらの要素内でデータからデータを出力だけを使用して、ソース、`DataBinder.Eval()`メソッド。

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

ページが読み込まれるときに、このサーバー側コードとアコーディオンにデータ ソースをバインドする必要があります。

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

このサンプルを完了するまでにはアコーディオン コントロールで参照されている 2 つの CSS クラスを定義する必要があります (のプロパティで`HeaderCssClass`と`ContentCssClass`)。 次のマークアップを格納、`<head>`ページのセクション。

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![アコーディオンにデータがデータ ソースから直接取得されます。](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

アコーディオンにデータがデータ ソースから直接取得されます ([フルサイズの画像を表示する をクリックします](databinding-to-an-accordion-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](dynamically-adding-an-accordion-pane-cs.md)
> [次へ](dynamically-adding-an-accordion-pane-vb.md)
