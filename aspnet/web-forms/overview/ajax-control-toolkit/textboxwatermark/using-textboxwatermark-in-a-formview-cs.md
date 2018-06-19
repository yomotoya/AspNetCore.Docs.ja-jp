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
<a name="using-textboxwatermark-in-a-formview-c"></a><span data-ttu-id="46e8d-104">TextBoxWatermark を使用して、フォーム ビュー (c#)</span><span class="sxs-lookup"><span data-stu-id="46e8d-104">Using TextBoxWatermark in a FormView (C#)</span></span>
====================
<span data-ttu-id="46e8d-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="46e8d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="46e8d-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="46e8d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)</span></span>

> <span data-ttu-id="46e8d-107">AJAX コントロールのツールキットで TextBoxWatermark コントロールは、ボックス内でテキストが表示されないようにテキスト ボックスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="46e8d-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="46e8d-108">ボックスに、ユーザーがクリックするは空になります。</span><span class="sxs-lookup"><span data-stu-id="46e8d-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="46e8d-109">ユーザーがテキストを入力することがなく、ボックスのまま、指定済みのテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="46e8d-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="46e8d-110">フォーム ビュー コントロール内でこのも。</span><span class="sxs-lookup"><span data-stu-id="46e8d-110">This is also possible within a FormView control.</span></span>


## <a name="overview"></a><span data-ttu-id="46e8d-111">概要</span><span class="sxs-lookup"><span data-stu-id="46e8d-111">Overview</span></span>

<span data-ttu-id="46e8d-112">`TextBoxWatermark`ボックス内でテキストが表示されるように、AJAX コントロール Toolkit でコントロールがテキスト ボックスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="46e8d-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="46e8d-113">ボックスに、ユーザーがクリックするは空になります。</span><span class="sxs-lookup"><span data-stu-id="46e8d-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="46e8d-114">ユーザーがテキストを入力することがなく、ボックスのまま、指定済みのテキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="46e8d-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="46e8d-115">これはでも、`FormView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="46e8d-115">This is also possible within a `FormView` control.</span></span>

## <a name="steps"></a><span data-ttu-id="46e8d-116">手順</span><span class="sxs-lookup"><span data-stu-id="46e8d-116">Steps</span></span>

<span data-ttu-id="46e8d-117">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="46e8d-117">First of all, a data source is required.</span></span> <span data-ttu-id="46e8d-118">このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="46e8d-118">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="46e8d-119">データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。</span><span class="sxs-lookup"><span data-stu-id="46e8d-119">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="46e8d-120">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="46e8d-120">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="46e8d-121">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="46e8d-121">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="46e8d-122">このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。</span><span class="sxs-lookup"><span data-stu-id="46e8d-122">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="46e8d-123">セットアップが異なっている場合は、データベースの接続情報を調整する必要です。</span><span class="sxs-lookup"><span data-stu-id="46e8d-123">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="46e8d-124">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="46e8d-124">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

<span data-ttu-id="46e8d-125">次に、サポートするページにデータ ソースを追加、 `DELETE`、`INSERT`と`UPDATE`SQL ステートメント。</span><span class="sxs-lookup"><span data-stu-id="46e8d-125">Then, add a data source to the page which supports the `DELETE`, `INSERT` and `UPDATE` SQL statements.</span></span> <span data-ttu-id="46e8d-126">データ ソースを作成する Visual Studio のアシスタントを使用している場合、バグを現在のバージョンでは、テーブル名のプレフィックスにしないことに注意してください (`Vendor`) と`Purchasing`です。</span><span class="sxs-lookup"><span data-stu-id="46e8d-126">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="46e8d-127">次のマークアップは、正しい構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="46e8d-127">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

<span data-ttu-id="46e8d-128">名前に注意してください (`ID`) で使用されるため、データ ソースの`DataSourceID`のプロパティ、`FormView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="46e8d-128">Remember the name (`ID`) of the data source, since it will be used in the `DataSourceID` property of the `FormView` control.</span></span> <span data-ttu-id="46e8d-129">`<InsertItemTemplate>`のセクションで、`FormView`によって拡張されたテキスト ボックスが含まれています、`TextBoxWatermarkExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="46e8d-129">The `<InsertItemTemplate>` section of the `FormView` contains a textbox which is extended by the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="46e8d-130">テンプレート内と、それ以外は、extender が存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="46e8d-130">Make sure that the extender resides within the template and not outside of it.</span></span>

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

<span data-ttu-id="46e8d-131">ユーザーが挿入モードの変更と今すぐ、`FormView`制御の場合は、新しい仕入先に感謝事前入力用テキスト フィールド、`TextBoxWatermarkExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="46e8d-131">Now when the user changes into the insert mode of the `FormView` control, the text field for the new vendor is prefilled thanks to the `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="46e8d-132">テキスト ボックス内をクリックでは、充てんテキストが消えることができます。</span><span class="sxs-lookup"><span data-stu-id="46e8d-132">A click inside the textbox lets the filler text disappear.</span></span>


<span data-ttu-id="46e8d-133">[![フィールド レベルのウォーターマークは、extender から取得します。](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="46e8d-133">[![The watermark in the field comes from the extender](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)</span></span>

<span data-ttu-id="46e8d-134">フィールド レベルのウォーターマークが、extender を起源 ([フルサイズのイメージを表示するをクリックして](using-textboxwatermark-in-a-formview-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="46e8d-134">The watermark in the field comes from the extender ([Click to view full-size image](using-textboxwatermark-in-a-formview-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="46e8d-135">次へ</span><span class="sxs-lookup"><span data-stu-id="46e8d-135">Next</span></span>](using-textboxwatermark-with-validation-controls-cs.md)
