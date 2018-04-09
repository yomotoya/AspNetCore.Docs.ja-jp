---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: アニメーションをコントロール (c#) に追加する |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 このチュートリアルではどのようにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="ddfe4-104">アニメーションをコントロール (c#) に追加します。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="ddfe4-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ddfe4-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="ddfe4-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ddfe4-108">このチュートリアルでは、このようなアニメーションを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="ddfe4-109">概要</span><span class="sxs-lookup"><span data-stu-id="ddfe4-109">Overview</span></span>

<span data-ttu-id="ddfe4-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ddfe4-111">このチュートリアルでは、このようなアニメーションを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="ddfe4-112">手順</span><span class="sxs-lookup"><span data-stu-id="ddfe4-112">Steps</span></span>

<span data-ttu-id="ddfe4-113">含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="ddfe4-114">このシナリオでのアニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="ddfe4-115">パネルの関連付けられている CSS クラスには、背景色と幅を定義します。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="ddfe4-116">次に、必要があります、`AnimationExtender`です。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="ddfe4-117">入力した後、 `ID` 、通常、 `runat="server"`、`TargetControlID`属性は、ここでは、パネル アニメーション化するコントロールに設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="ddfe4-118">全体のアニメーションが適用されたは、残念ながら完全には現在サポートされている Visual Studio の IntelliSense によって、XML 構文を使用して宣言します。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="ddfe4-119">ルート ノードは`<Animations>;`アニメーションが場所を take(s) ときを特定するこのノード内で複数のイベントが許可されます。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="ddfe4-120">`OnClick` (マウス クリック)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="ddfe4-121">`OnHoverOut` (ときにマウスがコントロールを離れる)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ddfe4-122">`OnHoverOver` (マウスがコントロール上に置いたときに、停止、`OnHoverOut`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="ddfe4-123">`OnLoad` (ページの読み込み時)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="ddfe4-124">`OnMouseOut` (ときにマウスがコントロールを離れる)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ddfe4-125">`OnMouseOver` (マウスがコントロール上に置いたときに停止はありません、`OnMouseOut`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="ddfe4-126">フレームワークは、アニメーションでは、独自の XML 要素によって表される各 1 つのセットが付属します。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="ddfe4-127">選択範囲を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-127">Here is a selection:</span></span>

- <span data-ttu-id="ddfe4-128">`<Color>` (色を変更する)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="ddfe4-129">`<FadeIn>` (フェードイン)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="ddfe4-130">`<FadeOut>` (フェードアウト)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="ddfe4-131">`<Property>` (コントロールのプロパティを変更する)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="ddfe4-132">`<Pulse>` (いた)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="ddfe4-133">`<Resize>` (サイズを変更する)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="ddfe4-134">`<Scale>` (サイズを比例的に変更する)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="ddfe4-135">この例では、パネルはフェードアウトします。アニメーションは 1.5 秒を受け取ります (`Duration`属性)、24 (アニメーション手順) 秒あたりのフレームを表示する (`Fps` attributs)。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="ddfe4-136">ここでは、完全なマークアップを`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="ddfe4-137">このスクリプトを実行すると、パネルが表示され、1.5 秒単位でフェードアウトします。</span><span class="sxs-lookup"><span data-stu-id="ddfe4-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="ddfe4-138">[![パネルがフェードアウトします。](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ddfe4-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="ddfe4-139">パネルがフェードアウト ([フルサイズのイメージを表示するをクリックして](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ddfe4-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ddfe4-140">次へ</span><span class="sxs-lookup"><span data-stu-id="ddfe4-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
