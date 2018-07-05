---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: アニメーション (c#) 中の操作を無効にする |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アクションもサポートしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 396411332735503d1781e171bead8ab6aaed6efa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828196"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="6d2ca-104">アニメーション (c#) 中の操作を無効にします。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="6d2ca-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6d2ca-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6d2ca-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6d2ca-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="6d2ca-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6d2ca-108">マウス クリックしてなどの操作もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="6d2ca-109">ただしマウスのクリックでは、アニメーションを起動するときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="6d2ca-110">概要</span><span class="sxs-lookup"><span data-stu-id="6d2ca-110">Overview</span></span>

<span data-ttu-id="6d2ca-111">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6d2ca-112">マウス クリックしてなどの操作もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="6d2ca-113">ただしマウスのクリックでは、アニメーションを起動するときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="6d2ca-114">手順</span><span class="sxs-lookup"><span data-stu-id="6d2ca-114">Steps</span></span>

<span data-ttu-id="6d2ca-115">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="6d2ca-116">アニメーションは、次のように、HTML ボタンに適用されます。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="6d2ca-117">ポストバック; を作成するボタンしたくないので、Web コントロールではなく、HTML コントロールを使用することに注意してください。私たちにとって、クライアント側のアニメーションを起動しはだけです。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="6d2ca-118">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="6d2ca-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="6d2ca-119">内で、`<Animations>`ノード、`<OnClick>`マウス クリックを処理するために適切な要素です。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="6d2ca-120">ただし、アニメーションの中に、ボタンをクリックする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="6d2ca-121">`<EnableAction>`要素を処理できます。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="6d2ca-122">設定`Enabled="false"`アニメーションの一部として、ボタンを無効にします。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="6d2ca-123">(詳しくは、ボタンと実際のアニメーションを無効にする、) 複数の個別のアニメーションを使用しているため、 `<Parallel>` 1 つにまとめて 1 つのアニメーションを連結する要素が必要です。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="6d2ca-124">ここでは、完全なマークアップを`AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="6d2ca-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="6d2ca-125">リストの末尾に次の XML 要素を使用して、アニメーションの後のボタンを再度有効にすることもなります。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="6d2ca-126">ただし特定のシナリオでこれは無意味になるため、ボタンはフェードアウトし、は、アニメーションの最後に表示されません。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="6d2ca-127">[![アニメーションを実行するとすぐに、ボタンが無効になっています](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6d2ca-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="6d2ca-128">アニメーションを実行するとすぐに、ボタンは無効 ([フルサイズの画像を表示する をクリックします](disabling-actions-during-animation-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="6d2ca-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6d2ca-129">[前へ](animating-in-response-to-user-interaction-cs.md)
> [次へ](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6d2ca-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
