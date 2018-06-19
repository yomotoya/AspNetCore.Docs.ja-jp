---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: 条件 (VB) によってアニメーション |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションがかどうか.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a648ff8299c9720e9f34522f271595ab1b9bc9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868153"
---
<a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="74629-104">条件 (VB) によってアニメーション</span><span class="sxs-lookup"><span data-stu-id="74629-104">Animation Depending On a Condition (VB)</span></span>
====================
<span data-ttu-id="74629-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="74629-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="74629-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="74629-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="74629-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="74629-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="74629-108">JavaScript コードの形式での条件には、アニメーションを実行するかどうかは依存できますも。</span><span class="sxs-lookup"><span data-stu-id="74629-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="74629-109">概要</span><span class="sxs-lookup"><span data-stu-id="74629-109">Overview</span></span>

<span data-ttu-id="74629-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="74629-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="74629-111">JavaScript コードの形式での条件には、アニメーションを実行するかどうかは依存できますも。</span><span class="sxs-lookup"><span data-stu-id="74629-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="74629-112">手順</span><span class="sxs-lookup"><span data-stu-id="74629-112">Steps</span></span>

<span data-ttu-id="74629-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="74629-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="74629-114">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="74629-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="74629-115">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="74629-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="74629-116">次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意 `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="74629-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="74629-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="74629-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="74629-118">正規のアニメーションでは、いずれかではなく、`<Condition>`要素が関係します。</span><span class="sxs-lookup"><span data-stu-id="74629-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="74629-119">値として提供されている JavaScript コード、`ConditionScript`属性は、ランタイムで実行します。</span><span class="sxs-lookup"><span data-stu-id="74629-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="74629-120">場合は true と評価、アニメーション実行される、それ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="74629-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="74629-121">次のマークアップでは、それぞれの時にランダムなケースの 50% で実行されている 2 つのアニメーションを提供します。</span><span class="sxs-lookup"><span data-stu-id="74629-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="74629-122">のみがあるので内の 1 つのアニメーション`<OnLoad>`、2 つ`<Condition>`アニメーションで相互に参加している、`<Sequence>`要素。</span><span class="sxs-lookup"><span data-stu-id="74629-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="74629-123">なおが低い方 (`<`) で、`ConditionScript`属性は () をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="74629-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="74629-124">ときにかないアニメーションの実行は、このスクリプトを実行するかは、2 つのいずれかのか。</span><span class="sxs-lookup"><span data-stu-id="74629-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="74629-125">[![パネルがフェードアウト、サイズを変更せずため、最初の 1 つ、2 番目のアニメーション実行しませんでした。](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="74629-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="74629-126">パネルがフェードアウト サイズを変更することがなく 1 つ目、2 番目のアニメーション実行していないため ([フルサイズのイメージを表示するをクリックして](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="74629-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="74629-127">[前へ](executing-several-animations-after-each-other-vb.md)
> [次へ](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="74629-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
