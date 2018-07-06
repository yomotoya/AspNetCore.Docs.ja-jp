---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: (VB) の一覧からアニメーションを 1 つ選択 |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 フレームワークも許可する.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 126f1b03897763f0619f893d23ab2e763206d08e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809888"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="78b8f-104">(VB) の一覧からアニメーションを 1 つ選択します。</span><span class="sxs-lookup"><span data-stu-id="78b8f-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="78b8f-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="78b8f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="78b8f-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="78b8f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="78b8f-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="78b8f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="78b8f-108">フレームワークでは、いくつかの JavaScript コードの評価によって、アニメーションの一覧からアニメーションを 1 つを選択するプログラマもできます。</span><span class="sxs-lookup"><span data-stu-id="78b8f-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="78b8f-109">概要</span><span class="sxs-lookup"><span data-stu-id="78b8f-109">Overview</span></span>

<span data-ttu-id="78b8f-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="78b8f-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="78b8f-111">フレームワークでは、いくつかの JavaScript コードの評価によって、アニメーションの一覧からアニメーションを 1 つを選択するプログラマもできます。</span><span class="sxs-lookup"><span data-stu-id="78b8f-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="78b8f-112">手順</span><span class="sxs-lookup"><span data-stu-id="78b8f-112">Steps</span></span>

<span data-ttu-id="78b8f-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="78b8f-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="78b8f-114">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="78b8f-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="78b8f-115">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="78b8f-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="78b8f-116">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="78b8f-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="78b8f-117">内で、`<Animations>`ノードを使用して`<OnLoad>`ページが完全に読み込まれた後に、アニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="78b8f-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="78b8f-118">通常のアニメーションの 1 つではなく、`<Case>`要素が関与します。</span><span class="sxs-lookup"><span data-stu-id="78b8f-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="78b8f-119">その SelectScript 属性の値が評価されます。戻り値は数値である必要があります。</span><span class="sxs-lookup"><span data-stu-id="78b8f-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="78b8f-120">内でサブアニメーションのいずれか、この数によっては&lt;ケース&gt;を実行します。</span><span class="sxs-lookup"><span data-stu-id="78b8f-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="78b8f-121">たとえば、SelectScript は 2 に評価されると、Control Toolkit は 内で 3 番目のアニメーションを実行&lt;ケース&gt;(カウント 0 から始まる)。</span><span class="sxs-lookup"><span data-stu-id="78b8f-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="78b8f-122">次のマークアップは次の 3 つのサブアニメーションを定義します。 幅をサイズ変更、高さのサイズを変更およびフェードアウトします。JavaScript コード (`Math.floor(3 * Math.random())`) 3 つのアニメーションのいずれかが実行されるため、0 ~ 2 の数値を取得します。</span><span class="sxs-lookup"><span data-stu-id="78b8f-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="78b8f-123">[![考えられる 3 つのアニメーションのいずれか: パネルが広くなります。](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="78b8f-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="78b8f-124">考えられる 3 つのアニメーションのいずれか: パネルが広くなる ([フルサイズの画像を表示する をクリックします](picking-one-animation-out-of-a-list-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="78b8f-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="78b8f-125">[前へ](animation-depending-on-a-condition-vb.md)
> [次へ](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="78b8f-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
