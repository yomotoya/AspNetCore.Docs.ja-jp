---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: "アニメーション (c#) 中のアクションを無効化 |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アクションもサポートしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 875c6be5e190c31fac030fc17ef040a934233f16
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="5c105-104">アニメーション (c#) 中のアクションを無効にします。</span><span class="sxs-lookup"><span data-stu-id="5c105-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="5c105-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5c105-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5c105-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5c105-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="5c105-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="5c105-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5c105-108">マウス クリックしてなどの操作もサポートします。</span><span class="sxs-lookup"><span data-stu-id="5c105-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="5c105-109">ただし、マウスのクリックでは、アニメーションを開始、ときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5c105-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="5c105-110">概要</span><span class="sxs-lookup"><span data-stu-id="5c105-110">Overview</span></span>

<span data-ttu-id="5c105-111">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="5c105-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5c105-112">マウス クリックしてなどの操作もサポートします。</span><span class="sxs-lookup"><span data-stu-id="5c105-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="5c105-113">ただし、マウスのクリックでは、アニメーションを開始、ときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5c105-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="5c105-114">手順</span><span class="sxs-lookup"><span data-stu-id="5c105-114">Steps</span></span>

<span data-ttu-id="5c105-115">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="5c105-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="5c105-116">アニメーションは、次のように HTML ボタンに適用されます。</span><span class="sxs-lookup"><span data-stu-id="5c105-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="5c105-117">ポストバック; を作成するボタンたくないので、Web コントロールではなく、HTML コントロールを使用することに注意してください。クライアント側のアニメーションを起動、いてはいけないだけです。</span><span class="sxs-lookup"><span data-stu-id="5c105-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="5c105-118">次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="5c105-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="5c105-119">内で、`<Animations>`ノード、`<OnClick>`は、適切な要素をマウスのクリックを処理します。</span><span class="sxs-lookup"><span data-stu-id="5c105-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="5c105-120">ただし、アニメーションの中に、ボタンをクリックする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5c105-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="5c105-121">`<EnableAction>`その要素が注意することができます。</span><span class="sxs-lookup"><span data-stu-id="5c105-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="5c105-122">設定`Enabled="false"`アニメーションの一部として、ボタンを無効にします。</span><span class="sxs-lookup"><span data-stu-id="5c105-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="5c105-123">(ボタンおよび実際のアニメーションを無効にする、) 複数の個別のアニメーションを使用しているため、`<Parallel>`要素が 1 つにまとめて 1 つのアニメーションを接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5c105-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="5c105-124">ここでは、完全なマークアップを`AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="5c105-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="5c105-125">リストの末尾に次の XML 要素を使用して、アニメーションの終了後 ボタンを再度有効にすることもできます。</span><span class="sxs-lookup"><span data-stu-id="5c105-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="5c105-126">ただし特定のシナリオでこのが役に立ちませんボタンからフェードアウトになり、アニメーションの最後に表示されていません。</span><span class="sxs-lookup"><span data-stu-id="5c105-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="5c105-127">[![アニメーションを実行するとすぐに、ボタンが無効になっています](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5c105-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="5c105-128">アニメーションを実行するとすぐに、ボタンは無効になります ([フルサイズのイメージを表示するをクリックして](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5c105-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5c105-129">[前へ](animating-in-response-to-user-interaction-cs.md)
[次へ](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5c105-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
