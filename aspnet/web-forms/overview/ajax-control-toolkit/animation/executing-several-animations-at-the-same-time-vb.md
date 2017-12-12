---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: "複数のアニメーションを実行する、同時に (VB) |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 これによりに落としたを実行しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 8461f5ea303a9e1166f694d4039d4c1aedd1caa8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="dea18-104">(VB) 同時に複数のアニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dea18-104">Executing Several Animations at The Same Time (VB)</span></span>
====================
<span data-ttu-id="dea18-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dea18-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dea18-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="dea18-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="dea18-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="dea18-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dea18-108">並列的に複数のアニメーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="dea18-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="dea18-109">概要</span><span class="sxs-lookup"><span data-stu-id="dea18-109">Overview</span></span>

<span data-ttu-id="dea18-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="dea18-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dea18-111">並列的に複数のアニメーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="dea18-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="dea18-112">手順</span><span class="sxs-lookup"><span data-stu-id="dea18-112">Steps</span></span>

<span data-ttu-id="dea18-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="dea18-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="dea18-114">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="dea18-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="dea18-115">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="dea18-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="dea18-116">次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="dea18-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="dea18-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dea18-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="dea18-118">一般に、`<OnLoad>`使用できるアニメーションの 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="dea18-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="dea18-119">アニメーションのフレームワークでは、1 つを使用してにいくつかのアニメーションに参加することができます、`<Parallel>`要素。</span><span class="sxs-lookup"><span data-stu-id="dea18-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="dea18-120">内のすべてのアニメーション`<Parallel>`同時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="dea18-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="dea18-121">ここでは、可能なマークアップを`AnimationExtender`フェードアウトと同時に、パネルのサイズを変更するときに、コントロール。</span><span class="sxs-lookup"><span data-stu-id="dea18-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="dea18-122">実際に: このスクリプトを実行すると、パネル が表示されたら、サイズ変更 (threefolding より幅と halfing height)、同時にオンにするとします。</span><span class="sxs-lookup"><span data-stu-id="dea18-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="dea18-123">[![パネルがフェードアウトと、(ブラウザーのレンダリング エンジンにより、そのコンテンツを含む) のサイズ変更](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dea18-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="dea18-124">パネルがフェードアウトと、(ブラウザーのレンダリング エンジンにより、そのコンテンツを含む) のサイズ変更 ([フルサイズのイメージを表示するをクリックして](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dea18-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dea18-125">[前へ](adding-animation-to-a-control-vb.md)
[次へ](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dea18-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
