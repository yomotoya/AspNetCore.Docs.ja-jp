---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: データ バインドする (c#)、アコーディオン |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常、w を宣言しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a3ec242c4d5312026ddbc8282ef1b4c3142915a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878933"
---
<a name="databinding-to-an-accordion-c"></a>(C#)、アコーディオンへのデータ バインド
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常ページ自体内で宣言されていますが、データ ソースへのバインディングが複数の柔軟性を提供します。


## <a name="overview"></a>概要

AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常ページ自体内で宣言されていますが、データ ソースへのバインディングが複数の柔軟性を提供します。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。

このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。 セットアップが異なっている場合は、データベースの接続情報を調整する必要です。

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

次に、ページに、データ ソースを追加します。 限られた量のデータを使用するのにはのみ、AdventureWorks データベースの仕入先テーブルの最初の 5 つのエントリを選択します。 データ ソースを作成する Visual Studio のアシスタントを使用している場合、バグを現在のバージョンでは、テーブル名のプレフィックスにしないことに注意してください (`Vendor`) と`Purchasing`です。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

データ ソースの名前 (ID) に注意してください。 非常にこの id を使用し、必要があります、`DataSourceID`アコーディオン コントロールのプロパティ。

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

アコーディオン コントロール内では、ヘッダーを含む、コントロールのさまざまな部分のテンプレートを提供できます (`<HeaderTemplate>`) とコンテンツ (`<ContentTemplate>`)。 これらの要素内のデータからデータを出力だけを使用して、ソース、`DataBinder.Eval()`メソッド。

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

ページが読み込まれるときに、このサーバー側コードで、アコーディオンにデータ ソースをバインドしてください。

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

このサンプルを完了するまでには、アコーディオン コントロールで参照されている 2 つの CSS クラスを定義する必要があります (そのプロパティで`HeaderCssClass`と`ContentCssClass`)。 次のマークアップを格納、`<head>`ページのセクション。

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![データ ソースから直接、アコーディオンのデータのソースします。](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

データ ソースから直接、アコーディオンのデータのソース ([フルサイズのイメージを表示するをクリックして](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [次へ](dynamically-adding-an-accordion-pane-cs.md)
