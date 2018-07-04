---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: (VB) FormView で TextBoxWatermark を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で TextBoxWatermark コントロールでは、テキスト ボックスを拡張するので、ボックス内でテキストが表示されます。 ボックスに、ユーザーがクリックしたときに.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1bb3ad274ba6c8617643fa4d7894a73de4b5cdff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396186"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a>(VB) FormView で TextBoxWatermark を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> AJAX Control Toolkit で TextBoxWatermark コントロールでは、テキスト ボックスを拡張するので、ボックス内でテキストが表示されます。 ボックスに、ユーザーがクリックするは空になります。 ユーザーがテキストを入力しなくても、ボックスのまま、指定済みのテキストが表示されます。 これは、FormView コントロール内で可能性があります。


## <a name="overview"></a>概要

`TextBoxWatermark`ボックス内でテキストが表示されるように、AJAX Control Toolkit のコントロールがテキスト ボックスを拡張します。 ボックスに、ユーザーがクリックするは空になります。 ユーザーがテキストを入力しなくても、ボックスのまま、指定済みのテキストが表示されます。 これはでも、`FormView`コントロール。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。

このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

次に、サポートするページにデータ ソースを追加、 `DELETE`、`INSERT`と`UPDATE`SQL ステートメント。 データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。 次のマークアップは、正しい構文を示しています。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

名前に注意してください (`ID`) で使用されるため、データ ソースの`DataSourceID`のプロパティ、`FormView`コントロール。 `<InsertItemTemplate>`のセクション、`FormView`はによって拡張されたテキスト ボックスが含まれています、`TextBoxWatermarkExtender`コントロール。 エクステンダーには、テンプレート内で、それ以外が置かれていることを確認します。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

挿入モードに、ユーザーが変更されたときに今すぐ、`FormView`制御の場合に感謝します。 新しい仕入先が事前入力のテキスト フィールド、`TextBoxWatermarkExtender`コントロール。 テキスト ボックス内をクリックには、充てんテキストが表示されなくなることができます。


[![フィールド内のウォーターマークに由来エクステンダー](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

フィールドに透かしエクステンダーに由来 ([フルサイズの画像を表示する をクリックします](using-textboxwatermark-in-a-formview-vb/_static/image3.png))。

> [!div class="step-by-step"]
> [前へ](using-textboxwatermark-with-validation-controls-cs.md)
> [次へ](using-textboxwatermark-with-validation-controls-vb.md)
