---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: アニメーションをコントロール (VB) に追加する |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 このチュートリアルではどのようにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3da98e478c45213875b3829e51351d03571a05b8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="248a7-104">アニメーションをコントロール (VB) に追加します。</span><span class="sxs-lookup"><span data-stu-id="248a7-104">Adding Animation to a Control (VB)</span></span>
====================
<span data-ttu-id="248a7-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="248a7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="248a7-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="248a7-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="248a7-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="248a7-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="248a7-108">このチュートリアルでは、このようなアニメーションを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="248a7-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="248a7-109">概要</span><span class="sxs-lookup"><span data-stu-id="248a7-109">Overview</span></span>

<span data-ttu-id="248a7-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="248a7-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="248a7-111">このチュートリアルでは、このようなアニメーションを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="248a7-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="248a7-112">手順</span><span class="sxs-lookup"><span data-stu-id="248a7-112">Steps</span></span>

<span data-ttu-id="248a7-113">含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="248a7-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="248a7-114">このシナリオでのアニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="248a7-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="248a7-115">パネルの関連付けられている CSS クラスには、背景色と幅を定義します。</span><span class="sxs-lookup"><span data-stu-id="248a7-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="248a7-116">次に、必要があります、`AnimationExtender`です。</span><span class="sxs-lookup"><span data-stu-id="248a7-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="248a7-117">入力した後、 `ID` 、通常、 `runat="server"`、`TargetControlID`属性は、ここでは、パネル アニメーション化するコントロールに設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="248a7-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="248a7-118">全体のアニメーションが適用されたは、残念ながら完全には現在サポートされている Visual Studio の IntelliSense によって、XML 構文を使用して宣言します。</span><span class="sxs-lookup"><span data-stu-id="248a7-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="248a7-119">ルート ノードは`<Animations>;`アニメーションが場所を take(s) ときを特定するこのノード内で複数のイベントが許可されます。</span><span class="sxs-lookup"><span data-stu-id="248a7-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="248a7-120">`OnClick` (マウス クリック)</span><span class="sxs-lookup"><span data-stu-id="248a7-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="248a7-121">`OnHoverOut` (ときにマウスがコントロールを離れる)</span><span class="sxs-lookup"><span data-stu-id="248a7-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="248a7-122">`OnHoverOver` (マウスがコントロール上に置いたときに、停止、`OnHoverOut`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="248a7-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="248a7-123">`OnLoad` (ページの読み込み時)</span><span class="sxs-lookup"><span data-stu-id="248a7-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="248a7-124">`OnMouseOut` (ときにマウスがコントロールを離れる)</span><span class="sxs-lookup"><span data-stu-id="248a7-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="248a7-125">`OnMouseOver` (マウスがコントロール上に置いたときに停止はありません、`OnMouseOut`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="248a7-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="248a7-126">フレームワークは、アニメーションでは、独自の XML 要素によって表される各 1 つのセットが付属します。</span><span class="sxs-lookup"><span data-stu-id="248a7-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="248a7-127">選択範囲を次に示します。</span><span class="sxs-lookup"><span data-stu-id="248a7-127">Here is a selection:</span></span>

- <span data-ttu-id="248a7-128">`<Color>` (色を変更する)</span><span class="sxs-lookup"><span data-stu-id="248a7-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="248a7-129">`<FadeIn>` (フェードイン)</span><span class="sxs-lookup"><span data-stu-id="248a7-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="248a7-130">`<FadeOut>` (フェードアウト)</span><span class="sxs-lookup"><span data-stu-id="248a7-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="248a7-131">`<Property>` (コントロールのプロパティを変更する)</span><span class="sxs-lookup"><span data-stu-id="248a7-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="248a7-132">`<Pulse>` (いた)</span><span class="sxs-lookup"><span data-stu-id="248a7-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="248a7-133">`<Resize>` (サイズを変更する)</span><span class="sxs-lookup"><span data-stu-id="248a7-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="248a7-134">`<Scale>` (サイズを比例的に変更する)</span><span class="sxs-lookup"><span data-stu-id="248a7-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="248a7-135">この例では、パネルはフェードアウトします。アニメーションは 1.5 秒を受け取ります (`Duration`属性)、24 (アニメーション手順) 秒あたりのフレームを表示する (`Fps` attributs)。</span><span class="sxs-lookup"><span data-stu-id="248a7-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="248a7-136">ここでは、完全なマークアップを`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="248a7-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="248a7-137">このスクリプトを実行すると、パネルが表示され、1.5 秒単位でフェードアウトします。</span><span class="sxs-lookup"><span data-stu-id="248a7-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="248a7-138">[![パネルがフェードアウトします。](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="248a7-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="248a7-139">パネルがフェードアウト ([フルサイズのイメージを表示するをクリックして](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="248a7-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="248a7-140">[前へ](dynamically-controlling-updatepanel-animations-cs.md)
> [次へ](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="248a7-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
