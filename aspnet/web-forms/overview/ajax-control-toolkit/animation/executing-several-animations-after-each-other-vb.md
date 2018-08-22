---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: 他の (VB) の後に複数のアニメーションを実行する |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 これにより、落としたを実行する.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 89412c078bbe40f06d31327d0a17bf3ea8bc8314
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826675"
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="ebb7c-104">複数のアニメーションを実行後に他の (VB)</span><span class="sxs-lookup"><span data-stu-id="ebb7c-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="ebb7c-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ebb7c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ebb7c-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ebb7c-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="ebb7c-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ebb7c-108">後、その他の複数のアニメーションを 1 つを実行できます。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="ebb7c-109">概要</span><span class="sxs-lookup"><span data-stu-id="ebb7c-109">Overview</span></span>

<span data-ttu-id="ebb7c-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ebb7c-111">後、その他の複数のアニメーションを 1 つを実行できます。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="ebb7c-112">手順</span><span class="sxs-lookup"><span data-stu-id="ebb7c-112">Steps</span></span>

<span data-ttu-id="ebb7c-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="ebb7c-114">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="ebb7c-115">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="ebb7c-116">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="ebb7c-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="ebb7c-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="ebb7c-118">一般に、`<OnLoad>`アニメーションを 1 つのみを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="ebb7c-119">アニメーション フレームワークを使用すると、複数のアニメーションを使用して 1 つに参加させる、`<Sequence>`要素。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="ebb7c-120">内のすべてのアニメーション`<Sequence>`後にもう 1 つずつ実行されます。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="ebb7c-121">ここでは、可能なマークアップを`AnimationExtender`コントロール、パネルの幅を作成して、高さを大ききます。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="ebb7c-122">ときに最初の取得も縦と横が小さくし、パネル、このスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="ebb7c-123">[![まず、幅が増加します。](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ebb7c-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="ebb7c-124">まず、幅が増加 ([フルサイズの画像を表示する をクリックします](executing-several-animations-after-each-other-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="ebb7c-125">[![高さの減少し、](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="ebb7c-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="ebb7c-126">高さの減少し、([フルサイズの画像を表示する をクリックします](executing-several-animations-after-each-other-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="ebb7c-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ebb7c-127">[前へ](executing-several-animations-at-the-same-time-vb.md)
> [次へ](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ebb7c-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
