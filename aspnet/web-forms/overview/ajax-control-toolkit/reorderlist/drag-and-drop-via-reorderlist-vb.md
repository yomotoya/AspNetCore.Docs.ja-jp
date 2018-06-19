---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: ドラッグ アンド ドロップ ReorderList (VB) 経由で |Microsoft ドキュメント
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 99f47b969dc75efeec8485254d311c93dc0b5d35
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878790"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="ab320-103">ドラッグ アンド ドロップ ReorderList (VB) を使用して</span><span class="sxs-lookup"><span data-stu-id="ab320-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="ab320-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ab320-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ab320-105">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ab320-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="ab320-106">AJAX コントロールのツールキットで ReorderList コントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。</span><span class="sxs-lookup"><span data-stu-id="ab320-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="ab320-107">リストの現在の順序は、サーバー上に保持いてはいけない。</span><span class="sxs-lookup"><span data-stu-id="ab320-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="ab320-108">概要</span><span class="sxs-lookup"><span data-stu-id="ab320-108">Overview</span></span>

<span data-ttu-id="ab320-109">`ReorderList` AJAX コントロール Toolkit でコントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。</span><span class="sxs-lookup"><span data-stu-id="ab320-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="ab320-110">リストの現在の順序は、サーバー上に保持いてはいけない。</span><span class="sxs-lookup"><span data-stu-id="ab320-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="ab320-111">手順</span><span class="sxs-lookup"><span data-stu-id="ab320-111">Steps</span></span>

<span data-ttu-id="ab320-112">`ReorderList`コントロールは、データベースから、リストへのデータ バインディングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="ab320-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="ab320-113">何より、データ ストアに、リスト要素の順序変更を書き込んでいますもサポートします。</span><span class="sxs-lookup"><span data-stu-id="ab320-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="ab320-114">このサンプルでは、データ ストアとして Microsoft SQL Server 2005 Express Edition を使用します。</span><span class="sxs-lookup"><span data-stu-id="ab320-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="ab320-115">データベースは、express エディションを含む、Visual Studio のインストールの省略可能な (および空き) 部分です。</span><span class="sxs-lookup"><span data-stu-id="ab320-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="ab320-116">個別のダウンロードとして入手できますも[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)です。</span><span class="sxs-lookup"><span data-stu-id="ab320-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="ab320-117">このサンプルのものと、SQL Server 2005 Express Edition のインスタンスが呼び出される`SQLEXPRESS`web サーバーと同じコンピューター上に存在し、これは既定の設定にもします。</span><span class="sxs-lookup"><span data-stu-id="ab320-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="ab320-118">セットアップが異なっている場合は、データベースの接続情報を調整する必要です。</span><span class="sxs-lookup"><span data-stu-id="ab320-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="ab320-119">データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) )。</span><span class="sxs-lookup"><span data-stu-id="ab320-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="ab320-120">サーバーへの接続をダブルクリックして`Databases`新しいデータベースを作成し、(を右クリックして`New Database`) と呼ばれる`Tutorials`です。</span><span class="sxs-lookup"><span data-stu-id="ab320-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="ab320-121">このデータベースでと呼ばれる新しいテーブルを作成`AJAX`次の 4 つの列を含む。</span><span class="sxs-lookup"><span data-stu-id="ab320-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="ab320-122">`id` (主キー、整数、id は not NULL)</span><span class="sxs-lookup"><span data-stu-id="ab320-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="ab320-123">`char` (char (1)、NULL)</span><span class="sxs-lookup"><span data-stu-id="ab320-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="ab320-124">`description` (varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="ab320-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="ab320-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="ab320-125">`position` (int, NULL)</span></span>


<span data-ttu-id="ab320-126">[![AJAX のテーブルのレイアウト](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ab320-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="ab320-127">AJAX のテーブルのレイアウト ([フルサイズのイメージを表示するをクリックして](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ab320-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="ab320-128">次に、いくつかの値をテーブルを入力します。</span><span class="sxs-lookup"><span data-stu-id="ab320-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="ab320-129">なお、`position`列は、要素の並べ替え順序を保持します。</span><span class="sxs-lookup"><span data-stu-id="ab320-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="ab320-130">[![AJAX テーブル内の最初のデータ](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ab320-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="ab320-131">AJAX テーブル内の最初のデータ ([フルサイズのイメージを表示するをクリックして](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="ab320-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="ab320-132">次の手順を生成する必要があります、`SqlDataSource`新しいデータベースとそのテーブルとの通信を制御します。</span><span class="sxs-lookup"><span data-stu-id="ab320-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="ab320-133">データ ソースをサポートする必要があります、`SELECT`と`UPDATE`SQL コマンド。</span><span class="sxs-lookup"><span data-stu-id="ab320-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="ab320-134">リストの要素の順序が変更された後で、ときに、`ReorderList`コントロールは、データ ソースの 2 つの値を自動的に送信する`Update`コマンド: 新しい位置と、要素の ID。</span><span class="sxs-lookup"><span data-stu-id="ab320-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="ab320-135">そのため、データ ソースのニーズ、`<UpdateParameters>`これら 2 つの値についてのセクション。</span><span class="sxs-lookup"><span data-stu-id="ab320-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="ab320-136">`ReorderList`コントロールは、次の属性を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ab320-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="ab320-137">`AllowReorder`: かどうか、一覧の項目を並べ替えることができます</span><span class="sxs-lookup"><span data-stu-id="ab320-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="ab320-138">`DataSourceID`: データ ソースの ID</span><span class="sxs-lookup"><span data-stu-id="ab320-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="ab320-139">`DataKeyField`: データ ソースの主キー列の名前</span><span class="sxs-lookup"><span data-stu-id="ab320-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="ab320-140">`SortOrderField`: データ ソースを提供する列、リスト項目の並べ替え順序</span><span class="sxs-lookup"><span data-stu-id="ab320-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="ab320-141">`<DragHandleTemplate>`と`<ItemTemplate>`セクションでは、リストのレイアウトを微調整することができます。</span><span class="sxs-lookup"><span data-stu-id="ab320-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="ab320-142">また、データ バインドが可能なを使用して、`Eval()`メソッドを次に示すよう。</span><span class="sxs-lookup"><span data-stu-id="ab320-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="ab320-143">次の CSS スタイル情報 (で参照されている、`<DragHandleTemplate>`のセクションで、`ReorderList`コントロール)、マウス ポインターをドラッグ ハンドル上に置くことと適切に変わることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ab320-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="ab320-144">最後に、`ScriptManager`コントロール、ページを ASP.NET AJAX を初期化します。</span><span class="sxs-lookup"><span data-stu-id="ab320-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="ab320-145">ブラウザーでこの例を実行し、少しリスト項目を再配置します。</span><span class="sxs-lookup"><span data-stu-id="ab320-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="ab320-146">次に、ページの再読み込みや、データベースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ab320-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="ab320-147">変更後の位置が維持されているしの値でも反映されます、`position`ことと、データベース内の列せず、すべてのコードは、マークアップを使用してだけです。</span><span class="sxs-lookup"><span data-stu-id="ab320-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="ab320-148">[![新しい一覧項目の順序に従って、データベースで変更データ](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ab320-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="ab320-149">新しいリストに従って、データベースで変更データ項目の順序 ([フルサイズのイメージを表示するをクリックして](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="ab320-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab320-150">前へ</span><span class="sxs-lookup"><span data-stu-id="ab320-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
