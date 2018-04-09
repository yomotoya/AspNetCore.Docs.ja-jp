---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: データ バインド スライダー コントロール (VB) |Microsoft ドキュメント
author: wenz
description: AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。 現在の positio をバインドすることはしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="401e2-104">スライダー コントロール (VB) のデータ バインド</span><span class="sxs-lookup"><span data-stu-id="401e2-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="401e2-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="401e2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="401e2-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="401e2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="401e2-107">AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="401e2-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="401e2-108">スライダーの現在の位置を別の ASP.NET コントロールにバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="401e2-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="401e2-109">概要</span><span class="sxs-lookup"><span data-stu-id="401e2-109">Overview</span></span>

<span data-ttu-id="401e2-110">AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="401e2-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="401e2-111">スライダーの現在の位置を別の ASP.NET コントロールにバインドすることができます。</span><span class="sxs-lookup"><span data-stu-id="401e2-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="401e2-112">手順</span><span class="sxs-lookup"><span data-stu-id="401e2-112">Steps</span></span>

<span data-ttu-id="401e2-113">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="401e2-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="401e2-114">次に、2 つ追加`TextBox`ページへのコントロールです。</span><span class="sxs-lookup"><span data-stu-id="401e2-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="401e2-115">1 つは、グラフィカルなスライダーに変換され、もう 1 つは、スライダーの位置を保持します。</span><span class="sxs-lookup"><span data-stu-id="401e2-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="401e2-116">次の手順は、最後の手順では既にです。</span><span class="sxs-lookup"><span data-stu-id="401e2-116">The next step is already the final step.</span></span> <span data-ttu-id="401e2-117">`SliderExtender` ASP.NET AJAX コントロール Toolkit からコントロールをスライダー最初のテキスト ボックスの外になり、スライダーの位置が変わったときに、2 つ目のテキスト ボックスが自動的に更新します。</span><span class="sxs-lookup"><span data-stu-id="401e2-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="401e2-118">を使用するためには、`SliderExtender`の`TargetControlID`属性は、最初のテキスト ボックスの ID に設定する必要があります、`BoundControlID`属性は、2 つ目のテキスト ボックスの ID に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="401e2-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="401e2-119">データ バインディングの双方向のしくみ、ブラウザーで示すように、: スライダーの位置を更新して、テキスト ボックスに新しい値を入力します。</span><span class="sxs-lookup"><span data-stu-id="401e2-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="401e2-120">行う場合、2 つ目のテキスト ボックスは読み取り専用、そこに値を手動で更新するユーザーが困難であるように、テキスト フィールドに弱い保護を追加することがあります。</span><span class="sxs-lookup"><span data-stu-id="401e2-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="401e2-121">[![スライダーとテキスト ボックスが同期](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="401e2-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="401e2-122">スライダーとテキスト ボックスが同期 ([フルサイズのイメージを表示するをクリックして](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="401e2-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="401e2-123">前へ</span><span class="sxs-lookup"><span data-stu-id="401e2-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
