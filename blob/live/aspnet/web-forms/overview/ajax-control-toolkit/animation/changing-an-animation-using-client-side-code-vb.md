---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: "クライアント側コード (VB) を使用してアニメーションを変更する |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションこともできます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 83d1a21fba37d8807be467d02b5550dc7d096e6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="55d2d-104">クライアント側コード (VB) を使用してアニメーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="55d2d-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="55d2d-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="55d2d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="55d2d-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="55d2d-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="55d2d-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="55d2d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="55d2d-108">カスタムのクライアント側 JavaScript コードを使用して、アニメーションを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="55d2d-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="55d2d-109">概要</span><span class="sxs-lookup"><span data-stu-id="55d2d-109">Overview</span></span>

<span data-ttu-id="55d2d-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="55d2d-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="55d2d-111">カスタムのクライアント側 JavaScript コードを使用して、アニメーションを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="55d2d-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="55d2d-112">手順</span><span class="sxs-lookup"><span data-stu-id="55d2d-112">Steps</span></span>

<span data-ttu-id="55d2d-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="55d2d-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="55d2d-114">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="55d2d-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="55d2d-115">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="55d2d-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="55d2d-116">実際のアニメーションは、HTML ボタンによって起動します。</span><span class="sxs-lookup"><span data-stu-id="55d2d-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="55d2d-117">次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="55d2d-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="55d2d-118">あることに注意してくださいありません`<Animations>`内のノード、`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="55d2d-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="55d2d-119">カスタムの JavaScript コードを使用すると、コントロールで使用するアニメーションを提供します。</span><span class="sxs-lookup"><span data-stu-id="55d2d-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="55d2d-120">サーバー API と同様に`AnimationExtender`、まだ、extender にアニメーションを代入する簡単な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="55d2d-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="55d2d-121">ただし、extender のアニメーションを読み書きするいくつかのメソッドは公開さまざまなイベントに登録されている (`OnClick`、`OnLoad`など)。</span><span class="sxs-lookup"><span data-stu-id="55d2d-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="55d2d-122">次にいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="55d2d-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="55d2d-123">戻り値の形式、`get_*()`関数とする引数の形式、`set_*()`関数は、XML マークアップは何でしょうのオブジェクト表現を提供する、JSON 文字列。</span><span class="sxs-lookup"><span data-stu-id="55d2d-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="55d2d-124">現時点に、オブジェクトを渡すことはありませんが、特定のアニメーションからのオブジェクトの読み取り可能であれば (`get_OnXXXBehavior()`メソッド)。</span><span class="sxs-lookup"><span data-stu-id="55d2d-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="55d2d-125">ここでは、JSON 文字列 (引用符を区切りし、適切な形式)、ボタンによってトリガーされるアニメーションを表すが、パネルのサイズを変更して、同時にフェードアウトしてアニメーション化します。</span><span class="sxs-lookup"><span data-stu-id="55d2d-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="55d2d-126">次の JavaScript コードでは、この JSON descripting に割り当てられます、`OnClick`現在エクステンダーのアニメーションして実行します。</span><span class="sxs-lookup"><span data-stu-id="55d2d-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="55d2d-127">[![マウスのクリックせず (およびごくわずかなマークアップを含む) に、アニメーションがすぐに、実行されます。](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="55d2d-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="55d2d-128">マウス クリックしてせず (およびほとんどのマークアップを含む) に、アニメーションがすぐに、実行されます ([フルサイズのイメージを表示するをクリックして](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="55d2d-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="55d2d-129">[前へ](executing-animations-using-client-side-code-vb.md)
[次へ](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="55d2d-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
