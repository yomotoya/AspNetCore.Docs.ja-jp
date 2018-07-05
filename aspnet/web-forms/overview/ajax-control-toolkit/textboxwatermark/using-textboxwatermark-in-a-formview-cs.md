---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: (C#)、FormView で TextBoxWatermark を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で TextBoxWatermark コントロールでは、テキスト ボックスを拡張するので、ボックス内でテキストが表示されます。 ボックスに、ユーザーがクリックしたときに.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e646b685680071501d45f7454920def2b15a7db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817039"
---
<a name="using-textboxwatermark-in-a-formview-c"></a>(C#)、FormView で TextBoxWatermark を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> AJAX Control Toolkit で TextBoxWatermark コントロールでは、テキスト ボックスを拡張するので、ボックス内でテキストが表示されます。 ボックスに、ユーザーがクリックするは空になります。 ユーザーがテキストを入力しなくても、ボックスのまま、指定済みのテキストが表示されます。 これは、FormView コントロール内で可能性があります。


## <a name="overview"></a>概要

`TextBoxWatermark`ボックス内でテキストが表示されるように、AJAX Control Toolkit のコントロールがテキスト ボックスを拡張します。 ボックスに、ユーザーがクリックするは空になります。 ユーザーがテキストを入力しなくても、ボックスのまま、指定済みのテキストが表示されます。 これはでも、`FormView`コントロール。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。

このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

次に、サポートするページにデータ ソースを追加、 `DELETE`、`INSERT`と`UPDATE`SQL ステートメント。 データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

名前に注意してください (`ID`) で使用されるため、データ ソースの`DataSourceID`のプロパティ、`FormView`コントロール。 `<InsertItemTemplate>`のセクション、`FormView`はによって拡張されたテキスト ボックスが含まれています、`TextBoxWatermarkExtender`コントロール。 エクステンダーには、テンプレート内で、それ以外が置かれていることを確認します。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

挿入モードに、ユーザーが変更されたときに今すぐ、`FormView`制御の場合に感謝します。 新しい仕入先が事前入力のテキスト フィールド、`TextBoxWatermarkExtender`コントロール。 テキスト ボックス内をクリックには、充てんテキストが表示されなくなることができます。


[![フィールド内のウォーターマークに由来エクステンダー](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

フィールドに透かしエクステンダーに由来 ([フルサイズの画像を表示する をクリックします](using-textboxwatermark-in-a-formview-cs/_static/image3.png))。

> [!div class="step-by-step"]
> [次へ](using-textboxwatermark-with-validation-controls-cs.md)
