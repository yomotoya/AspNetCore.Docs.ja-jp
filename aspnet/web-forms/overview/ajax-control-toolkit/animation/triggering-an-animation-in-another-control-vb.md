---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: アニメーションをトリガーするもう 1 つコントロール (VB) |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 一般に、起動する.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d9ef7d35e34bd4f2b1433f7fb02d0c48834b2357
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804893"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="4758f-104">アニメーションをトリガーするもう 1 つコントロール (VB)</span><span class="sxs-lookup"><span data-stu-id="4758f-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="4758f-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4758f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4758f-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4758f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="4758f-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="4758f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4758f-108">一般に、アニメーションの起動は、同じコントロールでのユーザー操作によってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="4758f-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="4758f-109">ただしも別のコントロールを 1 つのコントロールと、アニメーションと対話することです。</span><span class="sxs-lookup"><span data-stu-id="4758f-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="4758f-110">概要</span><span class="sxs-lookup"><span data-stu-id="4758f-110">Overview</span></span>

<span data-ttu-id="4758f-111">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="4758f-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4758f-112">一般に、アニメーションの起動は、同じコントロールでのユーザー操作によってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="4758f-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="4758f-113">ただしも別のコントロールを 1 つのコントロールと、アニメーションと対話することです。</span><span class="sxs-lookup"><span data-stu-id="4758f-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="4758f-114">手順</span><span class="sxs-lookup"><span data-stu-id="4758f-114">Steps</span></span>

<span data-ttu-id="4758f-115">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="4758f-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="4758f-116">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="4758f-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="4758f-117">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="4758f-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="4758f-118">パネルをアニメーション化を開始するには、HTML ボタンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4758f-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="4758f-119">なお`<input type="button" />`をお勧め`<asp:Button />`ためたくないポストバック、ユーザーがそのボタンをクリックするとにします。</span><span class="sxs-lookup"><span data-stu-id="4758f-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="4758f-120">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`。</span><span class="sxs-lookup"><span data-stu-id="4758f-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="4758f-121">設定することが重要`TargetControlID`ボタン (アニメーションをトリガーする要素) の ID に、パネル (アニメーション化されている要素) の ID にありません</span><span class="sxs-lookup"><span data-stu-id="4758f-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="4758f-122">内で、`<Animations>`ノードで、通常どおりの場所のアニメーション。</span><span class="sxs-lookup"><span data-stu-id="4758f-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="4758f-123">パネルを変更するには、するには、ボタンの設定、`AnimationTarget`内のすべてのアニメーション要素の属性`AnimationExtender`します。</span><span class="sxs-lookup"><span data-stu-id="4758f-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="4758f-124">値は、`AnimationTarget`パネルの ID をもちろんです。</span><span class="sxs-lookup"><span data-stu-id="4758f-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="4758f-125">そうすること、アニメーションにトリガーを起動するボタンではなく、パネルで発生します。</span><span class="sxs-lookup"><span data-stu-id="4758f-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="4758f-126">ここでは、`AnimationExtender`このシナリオのマークアップ。</span><span class="sxs-lookup"><span data-stu-id="4758f-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="4758f-127">個々 のアニメーションを表示する特別な順序に注意してください。</span><span class="sxs-lookup"><span data-stu-id="4758f-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="4758f-128">まず、アニメーションの実行後に、ボタンが非アクティブ化を取得します。</span><span class="sxs-lookup"><span data-stu-id="4758f-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="4758f-129">あるためありません`AnimationTarget`属性、`<EnableAction>`要素をこのアニメーションは、元のコントロールに適用される: ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="4758f-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="4758f-130">次の 2 つのアニメーションの手順は実行 parallelly (`<Parallel>`要素)。</span><span class="sxs-lookup"><span data-stu-id="4758f-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="4758f-131">両方が、`AnimationTarget`属性に設定`"Panel1"`、そのため、パネル、[not] ボタンをアニメーション化します。</span><span class="sxs-lookup"><span data-stu-id="4758f-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="4758f-132">[![パネル アニメーションを開始するボタンにマウスのクリック](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4758f-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="4758f-133">ボタンをマウスのクリックは、パネルのアニメーションを開始 ([フルサイズの画像を表示する をクリックします](triggering-an-animation-in-another-control-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="4758f-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4758f-134">[前へ](disabling-actions-during-animation-vb.md)
> [次へ](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4758f-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
