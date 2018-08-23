---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: アニメーション コントロールに追加する (c#) |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 このチュートリアルではどのようにしています.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aa977883af931bb74b791104cf4ee3212079e43a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834768"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="ae658-104">アニメーション コントロールに追加する (c#)</span><span class="sxs-lookup"><span data-stu-id="ae658-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="ae658-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ae658-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ae658-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ae658-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="ae658-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ae658-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ae658-108">このチュートリアルでは、このようなアニメーションを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ae658-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="ae658-109">概要</span><span class="sxs-lookup"><span data-stu-id="ae658-109">Overview</span></span>

<span data-ttu-id="ae658-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ae658-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ae658-111">このチュートリアルでは、このようなアニメーションを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ae658-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="ae658-112">手順</span><span class="sxs-lookup"><span data-stu-id="ae658-112">Steps</span></span>

<span data-ttu-id="ae658-113">最初の手順は、通常どおりに含める、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、Control Toolkit を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ae658-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="ae658-114">このシナリオでは、アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="ae658-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="ae658-115">パネルの関連付けられている CSS クラスは、背景色と幅を定義します。</span><span class="sxs-lookup"><span data-stu-id="ae658-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="ae658-116">次に、必要があります、`AnimationExtender`します。</span><span class="sxs-lookup"><span data-stu-id="ae658-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="ae658-117">指定したら、`ID`と、通常`runat="server"`、`TargetControlID`属性は、ここでは、パネルをアニメーション化するコントロールに設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ae658-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="ae658-118">全体のアニメーションが適用されるは、残念ながら Visual Studio の IntelliSense によって完全には現在サポート、XML の構文を使用して宣言します。</span><span class="sxs-lookup"><span data-stu-id="ae658-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="ae658-119">ルート ノードは`<Animations>;`と場所の take(s) 単位を決定します。 このノード内で複数のイベントが許可されます。</span><span class="sxs-lookup"><span data-stu-id="ae658-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="ae658-120">`OnClick` (マウス クリック)</span><span class="sxs-lookup"><span data-stu-id="ae658-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="ae658-121">`OnHoverOut` (マウスから離したときにコントロールを)</span><span class="sxs-lookup"><span data-stu-id="ae658-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ae658-122">`OnHoverOver` (コントロールの上にマウスを重ねると停止、`OnHoverOut`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="ae658-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="ae658-123">`OnLoad` (ページの読み込み時)</span><span class="sxs-lookup"><span data-stu-id="ae658-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="ae658-124">`OnMouseOut` (マウスから離したときにコントロールを)</span><span class="sxs-lookup"><span data-stu-id="ae658-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ae658-125">`OnMouseOver` (停止しない、コントロールの上にマウスを重ねると、`OnMouseOut`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="ae658-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="ae658-126">フレームワークのアニメーション、独自の XML 要素によって表されるそれぞれのセットが付属します。</span><span class="sxs-lookup"><span data-stu-id="ae658-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="ae658-127">選択範囲を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ae658-127">Here is a selection:</span></span>

- <span data-ttu-id="ae658-128">`<Color>` (色を変更する)</span><span class="sxs-lookup"><span data-stu-id="ae658-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="ae658-129">`<FadeIn>` (フェードイン)</span><span class="sxs-lookup"><span data-stu-id="ae658-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="ae658-130">`<FadeOut>` (フェードアウト)</span><span class="sxs-lookup"><span data-stu-id="ae658-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="ae658-131">`<Property>` (コントロールのプロパティを変更する)</span><span class="sxs-lookup"><span data-stu-id="ae658-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="ae658-132">`<Pulse>` (いた)</span><span class="sxs-lookup"><span data-stu-id="ae658-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="ae658-133">`<Resize>` (サイズを変更する)</span><span class="sxs-lookup"><span data-stu-id="ae658-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="ae658-134">`<Scale>` (それに比例してサイズを変更する)</span><span class="sxs-lookup"><span data-stu-id="ae658-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="ae658-135">この例で、パネルをフェードアウトものとします。アニメーションが 1.5 秒かかるものと (`Duration`属性)、1 秒あたり 24 フレーム (アニメーション手順) を表示する (`Fps` attributs)。</span><span class="sxs-lookup"><span data-stu-id="ae658-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="ae658-136">完全なマークアップを次に示します、`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="ae658-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="ae658-137">このスクリプトを実行すると、パネルが表示され、1.5 秒にフェードアウトします。</span><span class="sxs-lookup"><span data-stu-id="ae658-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="ae658-138">[![パネルをフェードアウトします。](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ae658-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="ae658-139">パネルをフェードアウト ([フルサイズの画像を表示する をクリックします](adding-animation-to-a-control-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="ae658-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ae658-140">次へ</span><span class="sxs-lookup"><span data-stu-id="ae658-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
