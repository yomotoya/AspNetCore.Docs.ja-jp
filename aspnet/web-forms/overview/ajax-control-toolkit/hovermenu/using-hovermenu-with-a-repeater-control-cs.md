---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: (C#) Repeater コントロールで HoverMenu を使用する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で HoverMenu コントロールには、単純なポップアップの効果が用意されています要素の上にマウス ポインターを移動する姿にポップアップが表示されます。 しています.。
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 33f65a132ea9f0a939ab70ac249b397ea883ac8c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829240"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a><span data-ttu-id="15133-103">(C#) Repeater コントロールで HoverMenu を使用します。</span><span class="sxs-lookup"><span data-stu-id="15133-103">Using HoverMenu with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="15133-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="15133-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="15133-105">[コードのダウンロード](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="15133-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span></span>

> <span data-ttu-id="15133-106">AJAX Control Toolkit で HoverMenu コントロールには、単純なポップアップの効果が用意されています。 指定位置にある要素の上にマウス ポインターを合わせたときにポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="15133-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="15133-107">Repeater 内でこのコントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="15133-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="15133-108">概要</span><span class="sxs-lookup"><span data-stu-id="15133-108">Overview</span></span>

<span data-ttu-id="15133-109">`HoverMenu` AJAX Control Toolkit でコントロールには、単純なポップアップの効果が用意されています。 指定位置にある要素の上にマウス ポインターを合わせたときにポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="15133-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="15133-110">Repeater 内でこのコントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="15133-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="15133-111">手順</span><span class="sxs-lookup"><span data-stu-id="15133-111">Steps</span></span>

<span data-ttu-id="15133-112">まず、データ ソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="15133-112">First of all, a data source is required.</span></span> <span data-ttu-id="15133-113">このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="15133-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="15133-114">データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。</span><span class="sxs-lookup"><span data-stu-id="15133-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="15133-115">AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="15133-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="15133-116">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="15133-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="15133-117">このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。</span><span class="sxs-lookup"><span data-stu-id="15133-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="15133-118">場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15133-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="15133-119">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="15133-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="15133-120">次に、ページにデータ ソースを追加します。</span><span class="sxs-lookup"><span data-stu-id="15133-120">Then, add a data source to the page.</span></span> <span data-ttu-id="15133-121">少量のデータを使用するためにはのみ、AdventureWorks データベースの Vendor テーブルの最初の 5 つのエントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="15133-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="15133-122">データ ソースを作成するには、Visual Studio アシスタントを使用する場合は、現在のバージョンのバグは、テーブル名のプレフィックスにしないことに注意してください。 (`Vendor`) と`Purchasing`します。</span><span class="sxs-lookup"><span data-stu-id="15133-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="15133-123">次のマークアップは、正しい構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="15133-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="15133-124">次に、モーダル ポップアップとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="15133-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="15133-125">ここで、`HoverMenuExtender`が関与します。</span><span class="sxs-lookup"><span data-stu-id="15133-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="15133-126">Repeater の内、エクステンダーを配置する必要があります、独自のポップアップを取得するデータ ソース内のすべての要素、ように`<ItemTemplate>`セクション。</span><span class="sxs-lookup"><span data-stu-id="15133-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="15133-127">マークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="15133-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="15133-128">データ ソース内のすべての項目をポップアップを右側に表示されます (`PopupPosition`属性) に 50 ミリ秒の遅延の後 (`PopDelay`属性)。</span><span class="sxs-lookup"><span data-stu-id="15133-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="15133-129">[![Repeater 内の各項目の横にあるホバー メニューが表示されます。](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="15133-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="15133-130">Repeater 内の各項目の横にあるホバー メニューが表示されます ([フルサイズの画像を表示する をクリックします](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="15133-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="15133-131">次へ</span><span class="sxs-lookup"><span data-stu-id="15133-131">Next</span></span>](using-hovermenu-with-a-repeater-control-vb.md)
