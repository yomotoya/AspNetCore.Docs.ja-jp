---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: "Repeater コントロール (VB) で ModalPopup の使用 |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 この contr. を使用することも."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a1e49fdebfb3ad62667ffd5a979d366730a097bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a><span data-ttu-id="afda6-104">リピータ コントロール (VB) で ModalPopup の使い方</span><span class="sxs-lookup"><span data-stu-id="afda6-104">Using ModalPopup with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="afda6-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="afda6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="afda6-106">[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="afda6-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span></span>

> <span data-ttu-id="afda6-107">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="afda6-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="afda6-108">リピータ内でこのコントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="afda6-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="afda6-109">概要</span><span class="sxs-lookup"><span data-stu-id="afda6-109">Overview</span></span>

<span data-ttu-id="afda6-110">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="afda6-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="afda6-111">リピータ内でこのコントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="afda6-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="afda6-112">手順</span><span class="sxs-lookup"><span data-stu-id="afda6-112">Steps</span></span>

<span data-ttu-id="afda6-113">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="afda6-113">First of all, a data source is required.</span></span> <span data-ttu-id="afda6-114">このサンプルでは、AdventureWorks データベースと、Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="afda6-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="afda6-115">データベース (express エディションを含む)、Visual Studio のインストールのオプションの一部であるし、下にある個別のダウンロードとしても利用[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。</span><span class="sxs-lookup"><span data-stu-id="afda6-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="afda6-116">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (でダウンロード[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="afda6-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="afda6-117">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?表示 = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) をアタッチし、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="afda6-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="afda6-118">このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。</span><span class="sxs-lookup"><span data-stu-id="afda6-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="afda6-119">セットアップが異なっている場合は、データベースの接続情報を調整する必要です。</span><span class="sxs-lookup"><span data-stu-id="afda6-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="afda6-120">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="afda6-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="afda6-121">次に、ページに、データ ソースを追加します。</span><span class="sxs-lookup"><span data-stu-id="afda6-121">Then, add a data source to the page.</span></span> <span data-ttu-id="afda6-122">限られた量のデータを使用するのにはのみ、AdventureWorks データベースの仕入先テーブルの最初の 5 つのエントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="afda6-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="afda6-123">データ ソースを作成する Visual Studio のアシスタントを使用している場合、バグを現在のバージョンでは、テーブル名のプレフィックスにしないことに注意してください (`Vendor`) と`Purchasing`です。</span><span class="sxs-lookup"><span data-stu-id="afda6-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="afda6-124">次のマークアップは、正しい構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="afda6-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="afda6-125">次に、モーダル ポップアップとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="afda6-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="afda6-126">含まれている、`Button`コントロールをもう一度、ポップアップ ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="afda6-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="afda6-127">リピータ内での作業のポップアップを作成するために、`ModalPopupExtender`内にコントロールを配置する必要があります、`<ItemTemplate>`リピータのセクションでします。</span><span class="sxs-lookup"><span data-stu-id="afda6-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="afda6-128">パネルがリピータ、範囲外ですが、extender が内部です。</span><span class="sxs-lookup"><span data-stu-id="afda6-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="afda6-129">リピータのマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="afda6-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="afda6-130">次に、データ ソース内のすべての項目がモーダル ポップアップをトリガーする、その横のボタンに表示されます。</span><span class="sxs-lookup"><span data-stu-id="afda6-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="afda6-131">[![すべてのデータ ソースのエントリのモーダル ポップアップをトリガーできます。](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="afda6-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="afda6-132">すべてのデータ ソースのエントリのモーダル ポップアップをトリガーできます ([フルサイズのイメージを表示するをクリックして](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="afda6-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="afda6-133">[前へ](launching-a-modal-popup-window-from-server-code-vb.md)
[次へ](handling-postbacks-from-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="afda6-133">[Previous](launching-a-modal-popup-window-from-server-code-vb.md)
[Next](handling-postbacks-from-a-modalpopup-vb.md)</span></span>
