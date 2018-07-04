---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: 条件 (VB) に基づくアニメーション |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションがかどうか.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 160b53519382c215db039fd6297bb907688b81c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368558"
---
<a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="3e8a4-104">条件 (VB) に基づくアニメーション</span><span class="sxs-lookup"><span data-stu-id="3e8a4-104">Animation Depending On a Condition (VB)</span></span>
====================
<span data-ttu-id="3e8a4-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3e8a4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3e8a4-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3e8a4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="3e8a4-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3e8a4-108">アニメーションを実行するかどうかはできるいくつかの JavaScript コードの形式での条件によっても異なります。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="3e8a4-109">概要</span><span class="sxs-lookup"><span data-stu-id="3e8a4-109">Overview</span></span>

<span data-ttu-id="3e8a4-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3e8a4-111">アニメーションを実行するかどうかはできるいくつかの JavaScript コードの形式での条件によっても異なります。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="3e8a4-112">手順</span><span class="sxs-lookup"><span data-stu-id="3e8a4-112">Steps</span></span>

<span data-ttu-id="3e8a4-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="3e8a4-114">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="3e8a4-115">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="3e8a4-116">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="3e8a4-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="3e8a4-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="3e8a4-118">通常のアニメーションの 1 つではなく、`<Condition>`要素が関与します。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="3e8a4-119">値として提供される JavaScript コード、`ConditionScript`属性は、実行時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="3e8a4-120">場合は true と評価、アニメーションは実行、それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="3e8a4-121">次のマークアップは、それぞれのケースをランダムの 50% で実行されている 2 つのアニメーションを提供します。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="3e8a4-122">のみがあるので内の 1 つのアニメーション`<OnLoad>`、2 つ`<Condition>`を使用してアニメーションが参加している、`<Sequence>`要素。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="3e8a4-123">なお、小なり記号 (`<`) で、`ConditionScript`属性は () をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="3e8a4-124">ときにないアニメーション実行される、このスクリプトを実行するまたは、2 つのいずれかまたは両方の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="3e8a4-125">[![パネルがフェードアウト、サイズを変更せず 1 つ目、2 番目のアニメーション実行していないため](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3e8a4-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="3e8a4-126">パネルがフェードアウト、サイズを変更せず 1 つ目、2 番目のアニメーション実行していないため ([フルサイズの画像を表示する をクリックします](animation-depending-on-a-condition-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="3e8a4-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e8a4-127">[前へ](executing-several-animations-after-each-other-vb.md)
> [次へ](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3e8a4-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
