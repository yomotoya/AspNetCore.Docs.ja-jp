---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: UpdatePanel コントロール (VB) をアニメーション化 |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 内容を.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873158"
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="2bcdc-104">UpdatePanel コントロール (VB) をアニメーション化</span><span class="sxs-lookup"><span data-stu-id="2bcdc-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="2bcdc-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2bcdc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2bcdc-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2bcdc-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="2bcdc-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2bcdc-108">UpdatePanel の内容は、の特別な extender が存在し、アニメーション、フレームワークに大きく依存して: UpdatePanelAnimation です。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="2bcdc-109">このチュートリアルでは、UpdatePanel のようなアニメーションを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="2bcdc-110">概要</span><span class="sxs-lookup"><span data-stu-id="2bcdc-110">Overview</span></span>

<span data-ttu-id="2bcdc-111">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2bcdc-112">内容を`UpdatePanel`、特別な extender が存在するアニメーション フレームワークに大きく依存している:`UpdatePanelAnimation`です。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="2bcdc-113">このチュートリアルでは、このようなアニメーションを設定する方法、`UpdatePanel`です。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="2bcdc-114">手順</span><span class="sxs-lookup"><span data-stu-id="2bcdc-114">Steps</span></span>

<span data-ttu-id="2bcdc-115">含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2bcdc-116">このシナリオでのアニメーションが ASP.NET に適用される`Wizard`web コントロール内に存在する、`UpdatePanel`です。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="2bcdc-117">(任意) の 3 つの手順は、ポストバックをトリガーするための十分なオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2bcdc-118">マークアップのために必要な`UpdatePanelAnimationExtender`コントロールを使用するマークアップを非常に似ていますが、`AnimationExtender`です。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="2bcdc-119">`TargetControlID`属性を指定した、`ID`の`UpdatePanel`; アニメーション化する内で、`UpdatePanelAnimationExtender`コントロール、`<Animations>`要素は、アニメーションの XML マークアップを保持します。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="2bcdc-120">1 つの違い: イベントとイベント ハンドラーの量は、比較する限られた`AnimationExtender`です。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="2bcdc-121">`UpdatePanels`、2 つだけに存在します。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="2bcdc-122">`<OnUpdated>` UpdatePanel が更新されたとき</span><span class="sxs-lookup"><span data-stu-id="2bcdc-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="2bcdc-123">`<OnUpdating>` UpdatePanel が更新を開始する場合</span><span class="sxs-lookup"><span data-stu-id="2bcdc-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="2bcdc-124">このシナリオでは、新しい内容で、 `UpdatePanel` (ポストバック) の後のフェードインいてはいけない。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="2bcdc-125">必要なマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="2bcdc-126">これで、UpdatePanel 内のポストバックが発生すると、パネルの新しい内容がフェードイン スムーズに動作します。</span><span class="sxs-lookup"><span data-stu-id="2bcdc-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="2bcdc-127">[![ウィザードの次の手順がフェードインします。](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2bcdc-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2bcdc-128">ウィザードの次の手順がフェードイン ([フルサイズのイメージを表示するをクリックして](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2bcdc-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2bcdc-129">[前へ](changing-an-animation-using-client-side-code-vb.md)
> [次へ](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2bcdc-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
