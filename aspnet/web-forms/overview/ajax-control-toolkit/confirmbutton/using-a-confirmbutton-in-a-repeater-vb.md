---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: (VB) Repeater で ConfirmButton を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で ConfirmButton エクステンダーは、[はい] を作成します]、[ユーザーがボタンをクリックするとポップアップなどありません LinkButton コントロール。 場合にのみが [はい] がしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 72c9493582f7b952d1efcb45d3bc95bc130a38e1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826947"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a><span data-ttu-id="837e8-104">(VB) Repeater で ConfirmButton を使用します。</span><span class="sxs-lookup"><span data-stu-id="837e8-104">Using a ConfirmButton In a Repeater (VB)</span></span>
====================
<span data-ttu-id="837e8-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="837e8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="837e8-106">[コードのダウンロード](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="837e8-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span></span>

> <span data-ttu-id="837e8-107">AJAX Control Toolkit で ConfirmButton エクステンダーは、[はい] を作成します]、[ユーザーがボタンをクリックするとポップアップなどありません LinkButton コントロール。</span><span class="sxs-lookup"><span data-stu-id="837e8-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="837e8-108">のみ [はい] がクリックされたボタンのアクションが実行、それ以外の場合はキャンセルされます。</span><span class="sxs-lookup"><span data-stu-id="837e8-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="837e8-109">これは、repeater で可能性があります。</span><span class="sxs-lookup"><span data-stu-id="837e8-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="837e8-110">概要</span><span class="sxs-lookup"><span data-stu-id="837e8-110">Overview</span></span>

<span data-ttu-id="837e8-111">AJAX Control Toolkit で ConfirmButton エクステンダーは、[はい] を作成します]、[ユーザーがボタンをクリックするとポップアップなどありません LinkButton コントロール。</span><span class="sxs-lookup"><span data-stu-id="837e8-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="837e8-112">のみ [はい] がクリックされたボタンのアクションが実行、それ以外の場合はキャンセルされます。</span><span class="sxs-lookup"><span data-stu-id="837e8-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="837e8-113">これは、repeater で可能性があります。</span><span class="sxs-lookup"><span data-stu-id="837e8-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="837e8-114">手順</span><span class="sxs-lookup"><span data-stu-id="837e8-114">Steps</span></span>

<span data-ttu-id="837e8-115">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="837e8-115">First of all, a data source is required.</span></span> <span data-ttu-id="837e8-116">このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="837e8-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="837e8-117">データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。</span><span class="sxs-lookup"><span data-stu-id="837e8-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="837e8-118">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="837e8-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="837e8-119">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="837e8-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="837e8-120">このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。</span><span class="sxs-lookup"><span data-stu-id="837e8-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="837e8-121">場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="837e8-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="837e8-122">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="837e8-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

<span data-ttu-id="837e8-123">次に、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="837e8-123">Then, a data source is required.</span></span> <span data-ttu-id="837e8-124">わかりやすく、AdventureWorks の仕入先のテーブル内の最初の 5 つのエントリのみが取得されます。</span><span class="sxs-lookup"><span data-stu-id="837e8-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="837e8-125">Visual Studio のウィザードを使用して、データ ソース、テーブル名を作成するときに注意してください (`Vendors`) は現在正しくプレフィックスが付いていない`Purchasing`します。</span><span class="sxs-lookup"><span data-stu-id="837e8-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="837e8-126">次のマークアップは、しいものを示します。</span><span class="sxs-lookup"><span data-stu-id="837e8-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

<span data-ttu-id="837e8-127">このデータ ソースは、repeater 内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="837e8-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="837e8-128">通常どおり、`DataBinder.Eval()`メソッドは、データ ソースからデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="837e8-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="837e8-129">`ConfirmButtonExtender`内でコントロールを配置し、必要があります、`<ItemTemplate>`データ ソース内のエントリごとに表示されるように、repeater のセクション。</span><span class="sxs-lookup"><span data-stu-id="837e8-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


<span data-ttu-id="837e8-130">[![データ ソースからの各エントリの横にある [確認] ボタンが表示されます。](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="837e8-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span></span>

<span data-ttu-id="837e8-131">データ ソースからの各エントリの横にある [確認] ボタンが表示されます ([フルサイズの画像を表示する をクリックします](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="837e8-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="837e8-132">前へ</span><span class="sxs-lookup"><span data-stu-id="837e8-132">Previous</span></span>](using-a-confirmbutton-in-a-repeater-cs.md)
