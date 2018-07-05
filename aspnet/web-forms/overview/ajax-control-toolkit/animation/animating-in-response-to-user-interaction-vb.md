---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: ユーザーの操作 (VB) への応答をアニメーション化 |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションが星できます.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b774a058e715e4a98e767daf92886f24e627822
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815526"
---
<a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="ecf20-104">ユーザーの操作 (VB) への応答をアニメーション化</span><span class="sxs-lookup"><span data-stu-id="ecf20-104">Animating in Response To User Interaction (VB)</span></span>
====================
<span data-ttu-id="ecf20-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ecf20-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ecf20-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ecf20-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="ecf20-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ecf20-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ecf20-108">アニメーションは、自動的に起動できるまたはをトリガーすると、ユーザーの操作など、マウスでクリックしています。</span><span class="sxs-lookup"><span data-stu-id="ecf20-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="ecf20-109">概要</span><span class="sxs-lookup"><span data-stu-id="ecf20-109">Overview</span></span>

<span data-ttu-id="ecf20-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ecf20-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ecf20-111">アニメーションは、自動的に起動できるまたはをトリガーすると、ユーザーの操作など、マウスでクリックしています。</span><span class="sxs-lookup"><span data-stu-id="ecf20-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="ecf20-112">手順</span><span class="sxs-lookup"><span data-stu-id="ecf20-112">Steps</span></span>

<span data-ttu-id="ecf20-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="ecf20-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="ecf20-114">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="ecf20-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="ecf20-115">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="ecf20-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="ecf20-116">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="ecf20-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="ecf20-117">内で、`<Animations>`ノードはユーザーの操作を使用してアニメーションを開始する 5 つの方法があります (不足している要素が`<OnLoad>`ページ全体が完全に読み込まれた後実行される)。</span><span class="sxs-lookup"><span data-stu-id="ecf20-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="ecf20-118">`<OnClick>` (マウス クリック コントロール)</span><span class="sxs-lookup"><span data-stu-id="ecf20-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="ecf20-119">`<OnHoverOut>` (マウスがコントロールを離れる)</span><span class="sxs-lookup"><span data-stu-id="ecf20-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="ecf20-120">`<OnHoverOver>` (マウス ホバーを停止する、コントロールの上、`<OnHoverOut>`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="ecf20-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="ecf20-121">`<OnMouseOut>` (マウスがコントロールのまま)</span><span class="sxs-lookup"><span data-stu-id="ecf20-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="ecf20-122">`<OnMouseOver>` (マウスを置くが停止しない、コントロール、`<OnMouseOut>`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="ecf20-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="ecf20-123">このシナリオで`<OnClick>`使用されます。</span><span class="sxs-lookup"><span data-stu-id="ecf20-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="ecf20-124">パネルに、ユーザーがクリックすると、サイズを変更し、同時にフェードアウトします。</span><span class="sxs-lookup"><span data-stu-id="ecf20-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


<span data-ttu-id="ecf20-125">[![マウスのクリックでアニメーションを開始します。](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ecf20-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="ecf20-126">マウスのクリックでアニメーションを開始 ([フルサイズの画像を表示する をクリックします](animating-in-response-to-user-interaction-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="ecf20-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ecf20-127">[前へ](picking-one-animation-out-of-a-list-vb.md)
> [次へ](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ecf20-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
