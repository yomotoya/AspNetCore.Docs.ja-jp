---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: 一覧 (c#) からアニメーションを 1 つ選択 |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 フレームワークも許可する.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 7ef2c5d37c32150d17b798e22290f33b5619a14c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834673"
---
<a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="f33c3-104">一覧 (c#) からアニメーションを 1 つ選択します。</span><span class="sxs-lookup"><span data-stu-id="f33c3-104">Picking One Animation Out Of a List (C#)</span></span>
====================
<span data-ttu-id="f33c3-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f33c3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f33c3-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f33c3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="f33c3-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="f33c3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f33c3-108">フレームワークでは、いくつかの JavaScript コードの評価によって、アニメーションの一覧からアニメーションを 1 つを選択するプログラマもできます。</span><span class="sxs-lookup"><span data-stu-id="f33c3-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="f33c3-109">概要</span><span class="sxs-lookup"><span data-stu-id="f33c3-109">Overview</span></span>

<span data-ttu-id="f33c3-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="f33c3-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f33c3-111">フレームワークでは、いくつかの JavaScript コードの評価によって、アニメーションの一覧からアニメーションを 1 つを選択するプログラマもできます。</span><span class="sxs-lookup"><span data-stu-id="f33c3-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="f33c3-112">手順</span><span class="sxs-lookup"><span data-stu-id="f33c3-112">Steps</span></span>

<span data-ttu-id="f33c3-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="f33c3-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="f33c3-114">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="f33c3-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="f33c3-115">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="f33c3-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="f33c3-116">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="f33c3-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="f33c3-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f33c3-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="f33c3-118">通常のアニメーションの 1 つではなく、`<Case>`要素が関与します。</span><span class="sxs-lookup"><span data-stu-id="f33c3-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="f33c3-119">その SelectScript 属性の値が評価されます。戻り値は数値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="f33c3-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="f33c3-120">内でサブアニメーションのいずれか、この数によっては&lt;ケース&gt;を実行します。</span><span class="sxs-lookup"><span data-stu-id="f33c3-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="f33c3-121">たとえば、SelectScript は 2 に評価されると、Control Toolkit は 内で 3 番目のアニメーションを実行&lt;ケース&gt;(カウント 0 から始まる)。</span><span class="sxs-lookup"><span data-stu-id="f33c3-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="f33c3-122">次のマークアップは次の 3 つのサブアニメーションを定義します。 幅をサイズ変更、高さのサイズを変更およびフェードアウトします。JavaScript コード (`Math.floor(3 * Math.random())`) 3 つのアニメーションのいずれかが実行されるため、0 ~ 2 の数値を取得します。</span><span class="sxs-lookup"><span data-stu-id="f33c3-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


<span data-ttu-id="f33c3-123">[![考えられる 3 つのアニメーションのいずれか: パネルが広くなります。](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f33c3-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="f33c3-124">考えられる 3 つのアニメーションのいずれか: パネルが広くなる ([フルサイズの画像を表示する をクリックします](picking-one-animation-out-of-a-list-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="f33c3-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f33c3-125">[前へ](animation-depending-on-a-condition-cs.md)
> [次へ](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f33c3-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
