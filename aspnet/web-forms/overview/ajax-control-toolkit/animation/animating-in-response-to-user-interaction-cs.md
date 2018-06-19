---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: ユーザーの操作 (c#) への応答でのアニメーション化 |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションがスターできます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 783563f4e33087e99a071cf829ca6bab246ba3b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870948"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="2ea07-104">ユーザーの操作 (c#) への応答をアニメーション化します。</span><span class="sxs-lookup"><span data-stu-id="2ea07-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="2ea07-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2ea07-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2ea07-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2ea07-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="2ea07-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2ea07-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ea07-108">アニメーションは、自動的に開始できます。 または、マウスでクリックなど、ユーザーとの対話によってトリガー可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2ea07-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="2ea07-109">概要</span><span class="sxs-lookup"><span data-stu-id="2ea07-109">Overview</span></span>

<span data-ttu-id="2ea07-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2ea07-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ea07-111">アニメーションは、自動的に開始できます。 または、マウスでクリックなど、ユーザーとの対話によってトリガー可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2ea07-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="2ea07-112">手順</span><span class="sxs-lookup"><span data-stu-id="2ea07-112">Steps</span></span>

<span data-ttu-id="2ea07-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2ea07-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="2ea07-114">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="2ea07-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="2ea07-115">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="2ea07-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="2ea07-116">次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="2ea07-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="2ea07-117">内で、`<Animations>`ノードはユーザーの操作を使用してアニメーションを開始する 5 つの方法があります (不足している要素は`<OnLoad>`ページ全体が完全に読み込まれた後に実行される)。</span><span class="sxs-lookup"><span data-stu-id="2ea07-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="2ea07-118">`<OnClick>` (マウスでクリック コントロール)</span><span class="sxs-lookup"><span data-stu-id="2ea07-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="2ea07-119">`<OnHoverOut>` (マウスがコントロールを離れる)</span><span class="sxs-lookup"><span data-stu-id="2ea07-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="2ea07-120">`<OnHoverOver>` (マウスが停止する、コントロールの上、`<OnHoverOut>`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="2ea07-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="2ea07-121">`<OnMouseOut>` (マウスがコントロールを離れる)</span><span class="sxs-lookup"><span data-stu-id="2ea07-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="2ea07-122">`<OnMouseOver>` (マウスがないを停止する、コントロールの上、`<OnMouseOut>`アニメーション)</span><span class="sxs-lookup"><span data-stu-id="2ea07-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="2ea07-123">このシナリオで`<OnClick>`を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ea07-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="2ea07-124">パネルにユーザーがクリックすると、サイズが変更され、同時にフェードアウトします。</span><span class="sxs-lookup"><span data-stu-id="2ea07-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="2ea07-125">[![マウスのクリック、アニメーションを開始します。](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2ea07-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="2ea07-126">マウスのクリックがアニメーションを開始 ([フルサイズのイメージを表示するをクリックして](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2ea07-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ea07-127">[前へ](picking-one-animation-out-of-a-list-cs.md)
> [次へ](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2ea07-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
