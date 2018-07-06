---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: (VB) FormView で TextBoxWatermark を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で TextBoxWatermark コントロールでは、テキスト ボックスを拡張するので、ボックス内でテキストが表示されます。 ボックスに、ユーザーがクリックしたときに.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 5f401b98adf0b07e8a0c6e2f1e078832d4918332
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840633"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a><span data-ttu-id="2065f-104">(VB) FormView で TextBoxWatermark を使用します。</span><span class="sxs-lookup"><span data-stu-id="2065f-104">Using TextBoxWatermark in a FormView (VB)</span></span>
====================
<span data-ttu-id="2065f-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2065f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2065f-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2065f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)</span></span>

> <span data-ttu-id="2065f-107">AJAX Control Toolkit で TextBoxWatermark コントロールでは、テキスト ボックスを拡張するので、ボックス内でテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2065f-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="2065f-108">ボックスに、ユーザーがクリックするは空になります。</span><span class="sxs-lookup"><span data-stu-id="2065f-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="2065f-109">ユーザーがテキストを入力しなくても、ボックスのまま、指定済みのテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2065f-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="2065f-110">これは、FormView コントロール内で可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2065f-110">This is also possible within a FormView control.</span></span>


## <a name="overview"></a><span data-ttu-id="2065f-111">概要</span><span class="sxs-lookup"><span data-stu-id="2065f-111">Overview</span></span>

<span data-ttu-id="2065f-112">`TextBoxWatermark`ボックス内でテキストが表示されるように、AJAX Control Toolkit のコントロールがテキスト ボックスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="2065f-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="2065f-113">ボックスに、ユーザーがクリックするは空になります。</span><span class="sxs-lookup"><span data-stu-id="2065f-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="2065f-114">ユーザーがテキストを入力しなくても、ボックスのまま、指定済みのテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2065f-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="2065f-115">これはでも、`FormView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="2065f-115">This is also possible within a `FormView` control.</span></span>

## <a name="steps"></a><span data-ttu-id="2065f-116">手順</span><span class="sxs-lookup"><span data-stu-id="2065f-116">Steps</span></span>

<span data-ttu-id="2065f-117">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="2065f-117">First of all, a data source is required.</span></span> <span data-ttu-id="2065f-118">このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="2065f-118">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="2065f-119">データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。</span><span class="sxs-lookup"><span data-stu-id="2065f-119">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="2065f-120">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="2065f-120">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="2065f-121">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="2065f-121">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="2065f-122">このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。</span><span class="sxs-lookup"><span data-stu-id="2065f-122">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="2065f-123">場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2065f-123">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="2065f-124">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="2065f-124">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

<span data-ttu-id="2065f-125">次に、サポートするページにデータ ソースを追加、 `DELETE`、`INSERT`と`UPDATE`SQL ステートメント。</span><span class="sxs-lookup"><span data-stu-id="2065f-125">Then, add a data source to the page which supports the `DELETE`, `INSERT` and `UPDATE` SQL statements.</span></span> <span data-ttu-id="2065f-126">データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。</span><span class="sxs-lookup"><span data-stu-id="2065f-126">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="2065f-127">次のマークアップは、正しい構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="2065f-127">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

<span data-ttu-id="2065f-128">名前に注意してください (`ID`) で使用されるため、データ ソースの`DataSourceID`のプロパティ、`FormView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="2065f-128">Remember the name (`ID`) of the data source, since it will be used in the `DataSourceID` property of the `FormView` control.</span></span> <span data-ttu-id="2065f-129">`<InsertItemTemplate>`のセクション、`FormView`はによって拡張されたテキスト ボックスが含まれています、`TextBoxWatermarkExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="2065f-129">The `<InsertItemTemplate>` section of the `FormView` contains a textbox which is extended by the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="2065f-130">エクステンダーには、テンプレート内で、それ以外が置かれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2065f-130">Make sure that the extender resides within the template and not outside of it.</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

<span data-ttu-id="2065f-131">挿入モードに、ユーザーが変更されたときに今すぐ、`FormView`制御の場合に感謝します。 新しい仕入先が事前入力のテキスト フィールド、`TextBoxWatermarkExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="2065f-131">Now when the user changes into the insert mode of the `FormView` control, the text field for the new vendor is prefilled thanks to the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="2065f-132">テキスト ボックス内をクリックには、充てんテキストが表示されなくなることができます。</span><span class="sxs-lookup"><span data-stu-id="2065f-132">A click inside the textbox lets the filler text disappear.</span></span>


<span data-ttu-id="2065f-133">[![フィールド内のウォーターマークに由来エクステンダー](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2065f-133">[![The watermark in the field comes from the extender](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)</span></span>

<span data-ttu-id="2065f-134">フィールドに透かしエクステンダーに由来 ([フルサイズの画像を表示する をクリックします](using-textboxwatermark-in-a-formview-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="2065f-134">The watermark in the field comes from the extender ([Click to view full-size image](using-textboxwatermark-in-a-formview-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2065f-135">[前へ](using-textboxwatermark-with-validation-controls-cs.md)
> [次へ](using-textboxwatermark-with-validation-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2065f-135">[Previous](using-textboxwatermark-with-validation-controls-cs.md)
[Next](using-textboxwatermark-with-validation-controls-vb.md)</span></span>
