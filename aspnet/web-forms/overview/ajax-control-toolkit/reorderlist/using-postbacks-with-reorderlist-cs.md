---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: ポストバックの併用 ReorderList (c#) |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで ReorderList コントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。 一覧の順序が変更されるたびに、po しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed01c30c0721c8f1cd8ccb3fea0735ea8fa4f0a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="a7fe2-104">ポストバックの併用 ReorderList (c#)</span><span class="sxs-lookup"><span data-stu-id="a7fe2-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="a7fe2-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a7fe2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a7fe2-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a7fe2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="a7fe2-107">AJAX コントロールのツールキットで ReorderList コントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="a7fe2-108">一覧の順序が変更されるたびにポストバックの変更のサーバーに通知するものとします。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="a7fe2-109">概要</span><span class="sxs-lookup"><span data-stu-id="a7fe2-109">Overview</span></span>

<span data-ttu-id="a7fe2-110">`ReorderList` AJAX コントロール Toolkit でコントロール一覧に表示される、ユーザーがドラッグ アンド ドロップで並べ替えることができます。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="a7fe2-111">一覧の順序が変更されるたびにポストバックの変更のサーバーに通知するものとします。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="a7fe2-112">手順</span><span class="sxs-lookup"><span data-stu-id="a7fe2-112">Steps</span></span>

<span data-ttu-id="a7fe2-113">いくつかの可能なデータ ソースが、`ReorderList`コントロール。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="a7fe2-114">使用する 1 つは、`XmlDataSource`コントロール。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="a7fe2-115">この XML をバインドするために、`ReorderList`コントロールと有効にするポストバックで、次の属性を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="a7fe2-116">`DataSourceID`: データ ソースの ID</span><span class="sxs-lookup"><span data-stu-id="a7fe2-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="a7fe2-117">`SortOrderField`: プロパティを並べ替える</span><span class="sxs-lookup"><span data-stu-id="a7fe2-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="a7fe2-118">`AllowReorder`: リストの要素の順序を変更するユーザーを許可するか</span><span class="sxs-lookup"><span data-stu-id="a7fe2-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="a7fe2-119">`PostBackOnReorder`: かどうかを、リストが再配置されるたびに、ポストバックを作成するには</span><span class="sxs-lookup"><span data-stu-id="a7fe2-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="a7fe2-120">コントロールの適切なマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="a7fe2-121">内で、`ReorderList`を使用してコントロール、データ ソースから特定のデータをバインドすることがあります、`Eval()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="a7fe2-122">ページで任意の位置にあるラベルは最後の順番変更が発生したときに情報を保持します。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="a7fe2-123">このラベルは、ポストバックを処理、サーバー側コード内のテキストが入力されます。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="a7fe2-124">最後に、ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`ページにコントロールを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a7fe2-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="a7fe2-125">[![ポストバックをトリガーするそれぞれの並べ替え](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a7fe2-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="a7fe2-126">ポストバックをトリガーするそれぞれの並べ替え ([フルサイズのイメージを表示するをクリックして](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a7fe2-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a7fe2-127">次へ</span><span class="sxs-lookup"><span data-stu-id="a7fe2-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
