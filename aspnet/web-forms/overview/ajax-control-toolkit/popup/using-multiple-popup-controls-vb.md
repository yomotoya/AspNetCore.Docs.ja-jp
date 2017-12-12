---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: "複数のポップアップ コントロール (VB) を使用して |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 M を使用することもしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 32e170ebd78a6f849004e789f53c03d9cd40be01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="58b33-104">複数のポップアップ コントロール (VB) を使用します。</span><span class="sxs-lookup"><span data-stu-id="58b33-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="58b33-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="58b33-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="58b33-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="58b33-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="58b33-107">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="58b33-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="58b33-108">1 ページに 1 つ以上のポップアップ コントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="58b33-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="58b33-109">概要</span><span class="sxs-lookup"><span data-stu-id="58b33-109">Overview</span></span>

<span data-ttu-id="58b33-110">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="58b33-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="58b33-111">1 ページに 1 つ以上のポップアップ コントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="58b33-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="58b33-112">手順</span><span class="sxs-lookup"><span data-stu-id="58b33-112">Steps</span></span>

<span data-ttu-id="58b33-113">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="58b33-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="58b33-114">次に、ポップアップ ウィンドウとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="58b33-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="58b33-115">現在のシナリオで、パネルが含まれます、`Calendar`コントロール。</span><span class="sxs-lookup"><span data-stu-id="58b33-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="58b33-116">内のパネルを配置するカレンダーのポストバックによるページの更新を回避するために、`UpdatePanel`コントロール。</span><span class="sxs-lookup"><span data-stu-id="58b33-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="58b33-117">ページには、2 つのテキスト ボックスも含まれています。</span><span class="sxs-lookup"><span data-stu-id="58b33-117">The page also contains two text boxes.</span></span> <span data-ttu-id="58b33-118">各テキスト ボックスのカレンダー ポップアップ表示される、テキスト ボックスがアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="58b33-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="58b33-119">使用して 2 つのテキスト ボックスの各を拡張するようになりました、`PopupControlExtender`です。</span><span class="sxs-lookup"><span data-stu-id="58b33-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="58b33-120">`TargetControlID`属性が、extender に関連付けられているコントロールの ID を提供します。</span><span class="sxs-lookup"><span data-stu-id="58b33-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="58b33-121">`PopupControlID`属性は、[ポップアップ] パネルの ID を格納します。</span><span class="sxs-lookup"><span data-stu-id="58b33-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="58b33-122">この場合、両方エクステンダーが、同じパネルを表示するが、さまざまなパネルは、可能であれば、同様です。</span><span class="sxs-lookup"><span data-stu-id="58b33-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="58b33-123">今すぐテキスト フィールド内をクリックするたびにカレンダーが表示されます、フィールドの下の日付を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="58b33-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="58b33-124">(テキスト ボックスに、選択した日付を取得については、説明別のチュートリアルです。)</span><span class="sxs-lookup"><span data-stu-id="58b33-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="58b33-125">[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="58b33-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="58b33-126">テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズのイメージを表示するをクリックして](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="58b33-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="58b33-127">[前へ](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[次へ](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="58b33-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
