---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "複数のポップアップ コントロール (c#) を使用して |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 M を使用することもしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="cb067-104">複数のポップアップ コントロール (c#) を使用します。</span><span class="sxs-lookup"><span data-stu-id="cb067-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="cb067-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cb067-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cb067-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="cb067-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="cb067-107">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="cb067-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="cb067-108">1 ページに 1 つ以上のポップアップ コントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="cb067-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="cb067-109">概要</span><span class="sxs-lookup"><span data-stu-id="cb067-109">Overview</span></span>

<span data-ttu-id="cb067-110">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="cb067-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="cb067-111">1 ページに 1 つ以上のポップアップ コントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="cb067-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="cb067-112">手順</span><span class="sxs-lookup"><span data-stu-id="cb067-112">Steps</span></span>

<span data-ttu-id="cb067-113">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="cb067-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="cb067-114">次に、ポップアップ ウィンドウとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="cb067-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="cb067-115">現在のシナリオで、パネルが含まれます、`Calendar`コントロール。</span><span class="sxs-lookup"><span data-stu-id="cb067-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="cb067-116">内のパネルを配置するカレンダーのポストバックによるページの更新を回避するために、`UpdatePanel`コントロール。</span><span class="sxs-lookup"><span data-stu-id="cb067-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="cb067-117">ページには、2 つのテキスト ボックスも含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb067-117">The page also contains two text boxes.</span></span> <span data-ttu-id="cb067-118">各テキスト ボックスのカレンダー ポップアップ表示される、テキスト ボックスがアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="cb067-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="cb067-119">使用して 2 つのテキスト ボックスの各を拡張するようになりました、`PopupControlExtender`です。</span><span class="sxs-lookup"><span data-stu-id="cb067-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="cb067-120">`TargetControlID`属性が、extender に関連付けられているコントロールの ID を提供します。</span><span class="sxs-lookup"><span data-stu-id="cb067-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="cb067-121">`PopupControlID`属性は、[ポップアップ] パネルの ID を格納します。</span><span class="sxs-lookup"><span data-stu-id="cb067-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="cb067-122">この場合、両方エクステンダーが、同じパネルを表示するが、さまざまなパネルは、可能であれば、同様です。</span><span class="sxs-lookup"><span data-stu-id="cb067-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="cb067-123">今すぐテキスト フィールド内をクリックするたびにカレンダーが表示されます、フィールドの下の日付を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="cb067-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="cb067-124">(テキスト ボックスに、選択した日付を取得については、説明別のチュートリアルです。)</span><span class="sxs-lookup"><span data-stu-id="cb067-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="cb067-125">[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cb067-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="cb067-126">テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズのイメージを表示するをクリックして](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cb067-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="cb067-127">次へ</span><span class="sxs-lookup"><span data-stu-id="cb067-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
