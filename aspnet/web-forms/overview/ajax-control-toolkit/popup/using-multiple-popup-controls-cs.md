---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: 複数のポップアップ コントロール (c#) を使用して |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 M を使用することもしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b00f720b66e6826c29f51690ab3361958aa8677
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364816"
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="b9cf7-104">複数のポップアップ コントロール (c#) を使用します。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="b9cf7-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b9cf7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b9cf7-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b9cf7-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="b9cf7-107">AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b9cf7-108">1 ページには、複数のポップアップ コントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="b9cf7-109">概要</span><span class="sxs-lookup"><span data-stu-id="b9cf7-109">Overview</span></span>

<span data-ttu-id="b9cf7-110">AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b9cf7-111">1 ページには、複数のポップアップ コントロールを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="b9cf7-112">手順</span><span class="sxs-lookup"><span data-stu-id="b9cf7-112">Steps</span></span>

<span data-ttu-id="b9cf7-113">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="b9cf7-114">次に、ポップアップとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="b9cf7-115">パネルに含まれる現在のシナリオでは、`Calendar`コントロール。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="b9cf7-116">内のパネルを配置するカレンダーのポストバックの原因となったページ更新を回避するために、`UpdatePanel`コントロール。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="b9cf7-117">ページには、2 つのテキスト ボックスも含まれています。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-117">The page also contains two text boxes.</span></span> <span data-ttu-id="b9cf7-118">各テキスト ボックスのテキスト ボックスがアクティブ化される、カレンダーのポップアップは表示されます。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="b9cf7-119">2 つのテキスト ボックスのそれぞれを拡張するようになりました、`PopupControlExtender`します。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="b9cf7-120">`TargetControlID`属性は、extender に関連付けられているコントロールの ID を提供します。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="b9cf7-121">`PopupControlID`属性には、ポップアップ パネルの ID が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="b9cf7-122">この場合は、両方のエクステンダーが同じのパネルを表示するが、さまざまなパネルをも可能であれば、します。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="b9cf7-123">テキスト フィールド内でクリックしたときに、カレンダーが表示されます、フィールドの下の日付を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="b9cf7-124">(テキスト ボックスに、選択した日付は戻ってきてで取り上げる別のチュートリアルです。)</span><span class="sxs-lookup"><span data-stu-id="b9cf7-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="b9cf7-125">[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b9cf7-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="b9cf7-126">テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズの画像を表示する をクリックします](using-multiple-popup-controls-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="b9cf7-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b9cf7-127">次へ</span><span class="sxs-lookup"><span data-stu-id="b9cf7-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
