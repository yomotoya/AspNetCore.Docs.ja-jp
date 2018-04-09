---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Repeater コントロール (VB) で HoverMenu の使用 |Microsoft ドキュメント
author: wenz
description: 'AJAX コントロールのツールキットで HoverMenu コントロールには、単純なポップアップ効果が用意されています: マウス ポインターが要素上に置いた、におけるにポップアップが表示されますしています.。'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aa107a7483683d965f3a510e6a43f174a386da0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="ba5b3-103">リピータ コントロール (VB) で HoverMenu の使い方</span><span class="sxs-lookup"><span data-stu-id="ba5b3-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="ba5b3-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ba5b3-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ba5b3-105">[コードをダウンロードする](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ba5b3-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="ba5b3-106">AJAX コントロールのツールキットで HoverMenu コントロールには、単純なポップアップ効果が用意されています: マウス ポインターが要素上に置いたと指定した位置にあるポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="ba5b3-107">リピータ内でこのコントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="ba5b3-108">概要</span><span class="sxs-lookup"><span data-stu-id="ba5b3-108">Overview</span></span>

<span data-ttu-id="ba5b3-109">`HoverMenu` AJAX コントロール Toolkit でコントロールには、単純なポップアップ効果が用意されています: マウス ポインターが要素上に置いたと指定した位置にあるポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="ba5b3-110">リピータ内でこのコントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="ba5b3-111">手順</span><span class="sxs-lookup"><span data-stu-id="ba5b3-111">Steps</span></span>

<span data-ttu-id="ba5b3-112">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-112">First of all, a data source is required.</span></span> <span data-ttu-id="ba5b3-113">このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="ba5b3-114">データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="ba5b3-115">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="ba5b3-116">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="ba5b3-117">このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="ba5b3-118">セットアップが異なっている場合は、データベースの接続情報を調整する必要です。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="ba5b3-119">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="ba5b3-120">次に、ページに、データ ソースを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-120">Then, add a data source to the page.</span></span> <span data-ttu-id="ba5b3-121">限られた量のデータを使用するのにはのみ、AdventureWorks データベースの仕入先テーブルの最初の 5 つのエントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="ba5b3-122">データ ソースを作成する Visual Studio のアシスタントを使用している場合、バグを現在のバージョンでは、テーブル名のプレフィックスにしないことに注意してください (`Vendor`) と`Purchasing`です。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="ba5b3-123">次のマークアップは、正しい構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="ba5b3-124">次に、モーダル ポップアップとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="ba5b3-125">ここで、`HoverMenuExtender`活躍です。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="ba5b3-126">リピータの内、extender に配置する必要があります、データ ソース内のすべての要素が、独自のポップアップを取得できるように`<ItemTemplate>`セクションです。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="ba5b3-127">マークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="ba5b3-128">これで、データ ソース内のすべての項目をポップアップ表示 (`PopupPosition`属性) 50 ミリ秒の遅延の後 (`PopDelay`属性)。</span><span class="sxs-lookup"><span data-stu-id="ba5b3-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="ba5b3-129">[![リピータ内の各項目の横にあるホバー メニューが表示されます。](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ba5b3-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="ba5b3-130">リピータ内の各項目の横にあるホバー メニューが表示されます ([フルサイズのイメージを表示するをクリックして](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ba5b3-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ba5b3-131">前へ</span><span class="sxs-lookup"><span data-stu-id="ba5b3-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
