---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: クライアント側コード (VB) を使用してアニメーションを実行する |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 アニメーションの実行.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 08cba7fa04249da4f0c7baa8e730ac75489e0efc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826309"
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="1df8a-104">クライアント側コード (VB) を使用してアニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="1df8a-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="1df8a-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1df8a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1df8a-106">[コードのダウンロード](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1df8a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="1df8a-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="1df8a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1df8a-108">カスタムのクライアント側の JavaScript コードを使用して、アニメーションの実行をトリガーも可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1df8a-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="1df8a-109">概要</span><span class="sxs-lookup"><span data-stu-id="1df8a-109">Overview</span></span>

<span data-ttu-id="1df8a-110">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="1df8a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1df8a-111">カスタムのクライアント側の JavaScript コードを使用して、アニメーションの実行をトリガーも可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1df8a-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="1df8a-112">手順</span><span class="sxs-lookup"><span data-stu-id="1df8a-112">Steps</span></span>

<span data-ttu-id="1df8a-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれる Control Toolkit を使用すること。</span><span class="sxs-lookup"><span data-stu-id="1df8a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="1df8a-114">次のようなテキストのパネルに、アニメーションが適用されます。</span><span class="sxs-lookup"><span data-stu-id="1df8a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="1df8a-115">パネルの関連付けられている CSS クラス、便利な背景色を定義し、パネルの固定幅の設定も。</span><span class="sxs-lookup"><span data-stu-id="1df8a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="1df8a-116">次に、追加、 `AnimationExtender` 、ページを提供する、 `ID`、`TargetControlID`属性と、変更を加える`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="1df8a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="1df8a-117">内で、`<Animations>`ノードを使用して`<OnClick>`パネルがクリック 1 回、ユーザーのアニメーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="1df8a-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="1df8a-118">Parallelly 実行される 2 つのアニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="1df8a-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="1df8a-119">このアニメーション (およびその他のすべてのアニメーション Control Toolkit を使用して作成) には、デモンストレーションのために、ページの実行後に、JavaScript コードを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="1df8a-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="1df8a-120">最初にすべてのアクセスする必要があります、`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="1df8a-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="1df8a-121">ASP.NET AJAX ライブラリには、`$find()`このタスクの関数。</span><span class="sxs-lookup"><span data-stu-id="1df8a-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="1df8a-122">`AnimationExtender`コントロールは、XML マークアップで使用されるイベント ハンドラーと同じ名前のメソッドを含む豊富な API を公開します: `OnClick()`、`OnLoad()`など。</span><span class="sxs-lookup"><span data-stu-id="1df8a-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="1df8a-123">呼び出しなど、`OnClick()`メソッド内でアニメーションの実行、`<OnClick>`の要素、`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="1df8a-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="1df8a-124">ここでは、ページが完全に読み込まれた後に [パネル] をクリックをエミュレートする完全なクライアント側の JavaScript コードは、注意、 `pageLoad()` 1 回、ページの ASP.NET AJAX によって呼び出される関数の名前が使用され、JavaScript ライブラリであったすべて含まれます読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="1df8a-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="1df8a-125">[![アニメーションのマウス クリックせず、すぐに実行します。](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1df8a-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="1df8a-126">アニメーションのマウス クリックしてせず、すぐに実行 ([フルサイズの画像を表示する をクリックします](executing-animations-using-client-side-code-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="1df8a-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1df8a-127">[前へ](modifying-animations-from-the-server-side-vb.md)
> [次へ](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1df8a-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
