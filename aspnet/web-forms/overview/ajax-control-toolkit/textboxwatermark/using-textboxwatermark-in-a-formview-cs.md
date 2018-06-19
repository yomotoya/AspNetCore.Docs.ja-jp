---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: TextBoxWatermark を使用して、フォーム ビュー (c#) |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで TextBoxWatermark コントロールは、ボックス内でテキストが表示されないようにテキスト ボックスを拡張します。 ボックスに、ユーザーがクリックしたときに.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 90e532d33756a995a85994254c48889517c5bb8d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870779"
---
<a name="using-textboxwatermark-in-a-formview-c"></a>TextBoxWatermark を使用して、フォーム ビュー (c#)
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> AJAX コントロールのツールキットで TextBoxWatermark コントロールは、ボックス内でテキストが表示されないようにテキスト ボックスを拡張します。 ボックスに、ユーザーがクリックするは空になります。 ユーザーがテキストを入力することがなく、ボックスのまま、指定済みのテキストが表示されます。 フォーム ビュー コントロール内でこのも。


## <a name="overview"></a>概要

`TextBoxWatermark`ボックス内でテキストが表示されるように、AJAX コントロール Toolkit でコントロールがテキスト ボックスを拡張します。 ボックスに、ユーザーがクリックするは空になります。 ユーザーがテキストを入力することがなく、ボックスのまま、指定済みのテキストが表示されます。 これはでも、`FormView`コントロール。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。

このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。 セットアップが異なっている場合は、データベースの接続情報を調整する必要です。

ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

次に、サポートするページにデータ ソースを追加、 `DELETE`、`INSERT`と`UPDATE`SQL ステートメント。 データ ソースを作成する Visual Studio のアシスタントを使用している場合、バグを現在のバージョンでは、テーブル名のプレフィックスにしないことに注意してください (`Vendor`) と`Purchasing`です。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

名前に注意してください (`ID`) で使用されるため、データ ソースの`DataSourceID`のプロパティ、`FormView`コントロール。 `<InsertItemTemplate>`のセクションで、`FormView`によって拡張されたテキスト ボックスが含まれています、`TextBoxWatermarkExtender`コントロール。 テンプレート内と、それ以外は、extender が存在することを確認します。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

ユーザーが挿入モードの変更と今すぐ、`FormView`制御の場合は、新しい仕入先に感謝事前入力用テキスト フィールド、`TextBoxWatermarkExtender`コントロール。 テキスト ボックス内をクリックでは、充てんテキストが消えることができます。


[![フィールド レベルのウォーターマークは、extender から取得します。](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

フィールド レベルのウォーターマークが、extender を起源 ([フルサイズのイメージを表示するをクリックして](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [次へ](using-textboxwatermark-with-validation-controls-cs.md)
