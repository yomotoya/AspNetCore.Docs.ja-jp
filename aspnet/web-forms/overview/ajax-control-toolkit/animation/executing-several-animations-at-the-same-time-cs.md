---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: 複数のアニメーションを実行すると同時に (c#) |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 これにより、落としたを実行する.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 500971bb1bad101a165c8dda6f5fa3d5cfe3aba4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382820"
---
<a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="21584-104">(C#) と同時に複数のアニメーションを実行</span><span class="sxs-lookup"><span data-stu-id="21584-104">Executing Several Animations at The Same Time (C#)</span></span>
====================
<span data-ttu-id="21584-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="21584-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="21584-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="21584-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="21584-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="21584-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="21584-108">並列的に複数のアニメーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="21584-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="21584-109">概要</span><span class="sxs-lookup"><span data-stu-id="21584-109">Overview</span></span>

<span data-ttu-id="21584-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="21584-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="21584-111">並列的に複数のアニメーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="21584-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="21584-112">手順</span><span class="sxs-lookup"><span data-stu-id="21584-112">Steps</span></span>

<span data-ttu-id="21584-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="21584-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="21584-114">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="21584-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="21584-115">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="21584-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="21584-116">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="21584-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="21584-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="21584-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="21584-118">一般に、`<OnLoad>`アニメーションを 1 つのみを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="21584-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="21584-119">アニメーション フレームワークを使用すると、複数のアニメーションを使用して 1 つに参加させる、`<Parallel>`要素。</span><span class="sxs-lookup"><span data-stu-id="21584-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="21584-120">内のすべてのアニメーション`<Parallel>`と同時に実行されます。</span><span class="sxs-lookup"><span data-stu-id="21584-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="21584-121">ここでは、可能なマークアップを`AnimationExtender`フェードアウトし、同時に、パネルのサイズを変更するときに、コントロール。</span><span class="sxs-lookup"><span data-stu-id="21584-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="21584-122">実際: パネルが表示されたら、サイズが変更されますこのスクリプトを実行すると (threefolding よりも詳細の幅と halfing 高さ) と同時にフェードアウトします。</span><span class="sxs-lookup"><span data-stu-id="21584-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="21584-123">[![パネルをフェードアウトし、(ブラウザーのレンダリング エンジンに協力してくれた、そのコンテンツを含む) のサイズ変更](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="21584-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="21584-124">パネルをフェードアウトし、(ブラウザーのレンダリング エンジンに協力してくれた、そのコンテンツを含む) のサイズ変更 ([フルサイズの画像を表示する をクリックします](executing-several-animations-at-the-same-time-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="21584-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21584-125">[前へ](adding-animation-to-a-control-cs.md)
> [次へ](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="21584-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
