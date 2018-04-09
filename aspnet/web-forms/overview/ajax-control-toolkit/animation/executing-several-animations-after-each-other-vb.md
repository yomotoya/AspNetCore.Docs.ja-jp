---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: 複数のアニメーションを実行する相互 (VB) |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 これによりに落としたを実行しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 700946b9f32c5ed2dcb8586e7c0e84d2238ff103
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="85d1a-104">他の (VB) の後にいくつかのアニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="85d1a-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="85d1a-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="85d1a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="85d1a-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="85d1a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="85d1a-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="85d1a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="85d1a-108">これにより、後に、他のいくつかのアニメーションの 1 つを実行できます。</span><span class="sxs-lookup"><span data-stu-id="85d1a-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="85d1a-109">概要</span><span class="sxs-lookup"><span data-stu-id="85d1a-109">Overview</span></span>

<span data-ttu-id="85d1a-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="85d1a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="85d1a-111">これにより、後に、他のいくつかのアニメーションの 1 つを実行できます。</span><span class="sxs-lookup"><span data-stu-id="85d1a-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="85d1a-112">手順</span><span class="sxs-lookup"><span data-stu-id="85d1a-112">Steps</span></span>

<span data-ttu-id="85d1a-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="85d1a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="85d1a-114">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="85d1a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="85d1a-115">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="85d1a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="85d1a-116">次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意 `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="85d1a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="85d1a-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="85d1a-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="85d1a-118">一般に、`<OnLoad>`使用できるアニメーションの 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="85d1a-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="85d1a-119">アニメーションのフレームワークでは、1 つを使用してにいくつかのアニメーションに参加することができます、`<Sequence>`要素。</span><span class="sxs-lookup"><span data-stu-id="85d1a-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="85d1a-120">内のすべてのアニメーション`<Sequence>`後にもう 1 つずつ実行されます。</span><span class="sxs-lookup"><span data-stu-id="85d1a-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="85d1a-121">ここでは、可能なマークアップを`AnimationExtender`コントロール パネルの幅を作成して、高さを小さきます。</span><span class="sxs-lookup"><span data-stu-id="85d1a-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="85d1a-122">実行すると、パネルは、このスクリプトも縦としより小さい最初を取得します。</span><span class="sxs-lookup"><span data-stu-id="85d1a-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="85d1a-123">[![最初の幅が増加しました](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="85d1a-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="85d1a-124">最初の幅が増加しました ([フルサイズのイメージを表示するをクリックして](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="85d1a-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="85d1a-125">[![高さの減少し、](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="85d1a-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="85d1a-126">高さの減少し、([フルサイズのイメージを表示するをクリックして](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="85d1a-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85d1a-127">[前へ](executing-several-animations-at-the-same-time-vb.md)
> [次へ](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="85d1a-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
