---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: ドラッグ アンド ドロップ ReorderList (VB) を使用して |Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 12c5aa60aa6bb71c99e267a1a71b40ed718df8cf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835311"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="9aff4-103">ドラッグ アンド ドロップで ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="9aff4-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="9aff4-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9aff4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9aff4-105">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9aff4-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="9aff4-106">ReorderList コントロール、AJAX Control Toolkit では、ユーザーがドラッグ アンド ドロップを使用して並べ替えることができる一覧を提供します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="9aff4-107">リストの現在の順序は、サーバーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="9aff4-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="9aff4-108">概要</span><span class="sxs-lookup"><span data-stu-id="9aff4-108">Overview</span></span>

<span data-ttu-id="9aff4-109">`ReorderList` AJAX Control Toolkit でコントロールには、ユーザーがドラッグ アンド ドロップを使用して並べ替えることができる一覧が用意されています。</span><span class="sxs-lookup"><span data-stu-id="9aff4-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="9aff4-110">リストの現在の順序は、サーバーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="9aff4-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="9aff4-111">手順</span><span class="sxs-lookup"><span data-stu-id="9aff4-111">Steps</span></span>

<span data-ttu-id="9aff4-112">`ReorderList`コントロールが一覧に、データベースからデータをバインディングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="9aff4-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="9aff4-113">何よりすばらしいは、データ ストアにリストの要素の順序の変更の書き込みもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="9aff4-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="9aff4-114">このサンプルでは、データ ストアとして Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="9aff4-115">データベースは、express edition を含め、Visual Studio のインストールのオプション (および無料) 部分です。</span><span class="sxs-lookup"><span data-stu-id="9aff4-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="9aff4-116">個別のダウンロードとして入手[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="9aff4-117">このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。</span><span class="sxs-lookup"><span data-stu-id="9aff4-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="9aff4-118">場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9aff4-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="9aff4-119">データベースを設定する最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。</span><span class="sxs-lookup"><span data-stu-id="9aff4-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="9aff4-120">サーバーへの接続をダブルクリックして`Databases`新しいデータベースを作成し、(右クリックし、 `New Database`) と呼ばれる`Tutorials`します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="9aff4-121">という新しいテーブルを作成すると、このデータベースに`AJAX`次の 4 つの列を含む。</span><span class="sxs-lookup"><span data-stu-id="9aff4-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="9aff4-122">`id` (プライマリ キー、整数、identity、いない NULL)</span><span class="sxs-lookup"><span data-stu-id="9aff4-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="9aff4-123">`char` (char (1)、NULL)</span><span class="sxs-lookup"><span data-stu-id="9aff4-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="9aff4-124">`description` (varchar (50)、NULL)</span><span class="sxs-lookup"><span data-stu-id="9aff4-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="9aff4-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="9aff4-125">`position` (int, NULL)</span></span>


<span data-ttu-id="9aff4-126">[![AJAX のテーブルのレイアウト](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9aff4-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="9aff4-127">AJAX のテーブルのレイアウト ([フルサイズの画像を表示する をクリックします](drag-and-drop-via-reorderlist-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="9aff4-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="9aff4-128">次に、表に、いくつかの値を設定します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="9aff4-129">なお、`position`列は、要素の並べ替え順序を保持します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="9aff4-130">[![AJAX の表に、初期データ](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9aff4-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="9aff4-131">AJAX の表に、初期データ ([フルサイズの画像を表示する をクリックします](drag-and-drop-via-reorderlist-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="9aff4-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="9aff4-132">次の手順を生成する必要があります、`SqlDataSource`新しいデータベースとそのテーブルでの通信を制御します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="9aff4-133">データ ソースをサポートする必要があります、`SELECT`と`UPDATE`SQL コマンド。</span><span class="sxs-lookup"><span data-stu-id="9aff4-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="9aff4-134">リストの要素の順序が変更された後で、ときに、`ReorderList`コントロールは、データ ソースの 2 つの値を自動的に送信する`Update`コマンド: 新しい位置と、要素の ID。</span><span class="sxs-lookup"><span data-stu-id="9aff4-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="9aff4-135">そのため、データ ソースのニーズ、`<UpdateParameters>`これら 2 つの値のセクション。</span><span class="sxs-lookup"><span data-stu-id="9aff4-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="9aff4-136">`ReorderList`コントロールは、次の属性を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9aff4-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="9aff4-137">`AllowReorder`: かどうか、リスト項目を並べ替えることができます</span><span class="sxs-lookup"><span data-stu-id="9aff4-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="9aff4-138">`DataSourceID`: データ ソースの ID</span><span class="sxs-lookup"><span data-stu-id="9aff4-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="9aff4-139">`DataKeyField`: データ ソースに主キー列の名前</span><span class="sxs-lookup"><span data-stu-id="9aff4-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="9aff4-140">`SortOrderField`リスト項目の並べ替え順序を提供するデータ ソース列</span><span class="sxs-lookup"><span data-stu-id="9aff4-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="9aff4-141">`<DragHandleTemplate>`と`<ItemTemplate>`セクションでは、リストのレイアウトを微調整することができます。</span><span class="sxs-lookup"><span data-stu-id="9aff4-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="9aff4-142">また、データ バインドが可能なを使用して、`Eval()`メソッド、ご覧のとおり。</span><span class="sxs-lookup"><span data-stu-id="9aff4-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="9aff4-143">次の CSS スタイル情報 (で参照されている、`<DragHandleTemplate>`のセクション、`ReorderList`コントロール) により、マウス ポインターをドラッグ ハンドルに置いたときに適切に変更します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="9aff4-144">最後に、`ScriptManager`コントロールがページの ASP.NET AJAX を初期化します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="9aff4-145">ブラウザーでこの例を実行し、リスト項目を少し変更します。</span><span class="sxs-lookup"><span data-stu-id="9aff4-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="9aff4-146">次に、ページを再度読み込んでか、データベースを見てください。</span><span class="sxs-lookup"><span data-stu-id="9aff4-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="9aff4-147">変更後の位置が保持されているし、は、値でも反映されます、`position`データベース内の列のマークアップを使用して、任意のコードは一切だけです。</span><span class="sxs-lookup"><span data-stu-id="9aff4-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="9aff4-148">[![新しい一覧項目の順序に従って、データベースで変更データ](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9aff4-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="9aff4-149">新しい一覧に従って、データベースで変更データ項目の順序 ([フルサイズの画像を表示する をクリックします](drag-and-drop-via-reorderlist-vb/_static/image9.png))。</span><span class="sxs-lookup"><span data-stu-id="9aff4-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9aff4-150">前へ</span><span class="sxs-lookup"><span data-stu-id="9aff4-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
