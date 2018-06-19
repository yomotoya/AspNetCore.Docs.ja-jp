---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: アニメーション別コントロール (VB) をトリガーする |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 一般を起動する、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 262a17e7521a8ea16c81e8dfdc6d3b6614c18eea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878998"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="f9d2d-104">アニメーション別コントロール (VB) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="f9d2d-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f9d2d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f9d2d-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f9d2d-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="f9d2d-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f9d2d-108">一般に、アニメーションの起動が同じコントロールでのユーザー操作によってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="f9d2d-109">ただしも別のコントロールを 1 つのコントロールとし、アニメーションと対話することです。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="f9d2d-110">概要</span><span class="sxs-lookup"><span data-stu-id="f9d2d-110">Overview</span></span>

<span data-ttu-id="f9d2d-111">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f9d2d-112">一般に、アニメーションの起動が同じコントロールでのユーザー操作によってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="f9d2d-113">ただしも別のコントロールを 1 つのコントロールとし、アニメーションと対話することです。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="f9d2d-114">手順</span><span class="sxs-lookup"><span data-stu-id="f9d2d-114">Steps</span></span>

<span data-ttu-id="f9d2d-115">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f9d2d-116">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f9d2d-117">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="f9d2d-118">パネルをアニメーション化を開始するために、HTML のボタンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="f9d2d-119">なお`<input type="button" />`をお勧め`<asp:Button />`たくないポストバック、ユーザーがそのボタンをクリックするためです。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="f9d2d-120">次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`です。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="f9d2d-121">設定することが重要`TargetControlID`ボタン (要素をアニメーションをトリガーする、) の ID に、パネル (アニメーション化されている要素) の ID にありません</span><span class="sxs-lookup"><span data-stu-id="f9d2d-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="f9d2d-122">内で、`<Animations>`ノード、通常どおりアニメーションを配置します。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="f9d2d-123">パネルを変更するようにするには、ボタンを設定、`AnimationTarget`内のすべてのアニメーション要素の属性`AnimationExtender`です。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="f9d2d-124">値は、`AnimationTarget`もちろん、パネルの ID です。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="f9d2d-125">このように、アニメーションは、パネルで、トリガーを起動するボタンではなく、発生します。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="f9d2d-126">ここでは、`AnimationExtender`このシナリオのマークアップ。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="f9d2d-127">個々 のアニメーションを表示する特別な順序に注意してください。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="f9d2d-128">最初に、アニメーションの実行後に、ボタンが非アクティブ化を取得します。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="f9d2d-129">存在しないためありません`AnimationTarget`属性、`<EnableAction>`要素をこのアニメーション コントロールに適用する、発信元: ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="f9d2d-130">次の 2 つのアニメーションのステップは実行 parallelly (`<Parallel>`要素)。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="f9d2d-131">両方とも、`AnimationTarget`属性に設定`"Panel1"`、したがってパネル、not ボタンをアニメーション化します。</span><span class="sxs-lookup"><span data-stu-id="f9d2d-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="f9d2d-132">[![マウスでクリック ボタン パネル アニメーションを開始します。](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f9d2d-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f9d2d-133">マウスでクリック ボタン パネル アニメーションを開始する ([フルサイズのイメージを表示するをクリックして](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f9d2d-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f9d2d-134">[前へ](disabling-actions-during-animation-vb.md)
> [次へ](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f9d2d-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
