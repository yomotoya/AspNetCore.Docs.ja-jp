---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: アコーディオン (VB) へのデータ バインド |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルには、w を宣言は、通常は.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: bc11aa96577919c2ca5dfca4ffc6301da67645ea
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377429"
---
<a name="databinding-to-an-accordion-vb"></a><span data-ttu-id="a1e53-104">アコーディオン (VB) へのデータ バインド</span><span class="sxs-lookup"><span data-stu-id="a1e53-104">Databinding to an Accordion (VB)</span></span>
====================
<span data-ttu-id="a1e53-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a1e53-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a1e53-106">[コードのダウンロード](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a1e53-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span></span>

> <span data-ttu-id="a1e53-107">AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。</span><span class="sxs-lookup"><span data-stu-id="a1e53-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="a1e53-108">パネルは通常、ページ自体内で宣言が、データ ソースへのバインドは柔軟性を提供します。</span><span class="sxs-lookup"><span data-stu-id="a1e53-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="a1e53-109">概要</span><span class="sxs-lookup"><span data-stu-id="a1e53-109">Overview</span></span>

<span data-ttu-id="a1e53-110">AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。</span><span class="sxs-lookup"><span data-stu-id="a1e53-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="a1e53-111">パネルは通常、ページ自体内で宣言が、データ ソースへのバインドは柔軟性を提供します。</span><span class="sxs-lookup"><span data-stu-id="a1e53-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="a1e53-112">手順</span><span class="sxs-lookup"><span data-stu-id="a1e53-112">Steps</span></span>

<span data-ttu-id="a1e53-113">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="a1e53-113">First of all, a data source is required.</span></span> <span data-ttu-id="a1e53-114">このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1e53-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="a1e53-115">データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。</span><span class="sxs-lookup"><span data-stu-id="a1e53-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="a1e53-116">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="a1e53-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="a1e53-117">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="a1e53-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="a1e53-118">このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。</span><span class="sxs-lookup"><span data-stu-id="a1e53-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="a1e53-119">場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1e53-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="a1e53-120">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="a1e53-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

<span data-ttu-id="a1e53-121">次に、ページにデータ ソースを追加します。</span><span class="sxs-lookup"><span data-stu-id="a1e53-121">Then, add a data source to the page.</span></span> <span data-ttu-id="a1e53-122">少量のデータを使用するためにはのみ、AdventureWorks データベースの Vendor テーブルの最初の 5 つのエントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="a1e53-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="a1e53-123">データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。</span><span class="sxs-lookup"><span data-stu-id="a1e53-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="a1e53-124">次のマークアップは、正しい構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="a1e53-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

<span data-ttu-id="a1e53-125">データ ソースの名前 (ID) に注意してください。</span><span class="sxs-lookup"><span data-stu-id="a1e53-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="a1e53-126">この非常に id で使用する必要があります、`DataSourceID`アコーディオン コントロールのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="a1e53-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

<span data-ttu-id="a1e53-127">アコーディオン コントロール内では、さまざまなパーツのヘッダーを含む、コントロールのテンプレートを行うことができます (`<HeaderTemplate>`) とコンテンツ (`<ContentTemplate>`)。</span><span class="sxs-lookup"><span data-stu-id="a1e53-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="a1e53-128">これらの要素内でデータからデータを出力だけを使用して、ソース、`DataBinder.Eval()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="a1e53-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

<span data-ttu-id="a1e53-129">ページが読み込まれるときに、このサーバー側コードとアコーディオンにデータ ソースをバインドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1e53-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

<span data-ttu-id="a1e53-130">このサンプルを完了するまでにはアコーディオン コントロールで参照されている 2 つの CSS クラスを定義する必要があります (のプロパティで`HeaderCssClass`と`ContentCssClass`)。</span><span class="sxs-lookup"><span data-stu-id="a1e53-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="a1e53-131">次のマークアップを格納、`<head>`ページのセクション。</span><span class="sxs-lookup"><span data-stu-id="a1e53-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


<span data-ttu-id="a1e53-132">[![アコーディオンにデータがデータ ソースから直接取得されます。](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1e53-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span></span>

<span data-ttu-id="a1e53-133">アコーディオンにデータがデータ ソースから直接取得されます ([フルサイズの画像を表示する をクリックします](databinding-to-an-accordion-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="a1e53-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1e53-134">[前へ](dynamically-adding-an-accordion-pane-cs.md)
> [次へ](dynamically-adding-an-accordion-pane-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a1e53-134">[Previous](dynamically-adding-an-accordion-pane-cs.md)
[Next](dynamically-adding-an-accordion-pane-vb.md)</span></span>
