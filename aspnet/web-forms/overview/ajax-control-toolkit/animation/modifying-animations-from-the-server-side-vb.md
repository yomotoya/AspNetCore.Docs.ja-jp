---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: サーバー側 (VB) からのアニメーションを変更する |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションも可能性があります.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b9ce85fc5040b2318233b3c553c2cf53dd03555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="c9c57-104">サーバー側 (VB) からのアニメーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="c9c57-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="c9c57-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c9c57-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c9c57-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c9c57-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="c9c57-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="c9c57-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c9c57-108">サーバー側でのアニメーションを変更することも</span><span class="sxs-lookup"><span data-stu-id="c9c57-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="c9c57-109">概要</span><span class="sxs-lookup"><span data-stu-id="c9c57-109">Overview</span></span>

<span data-ttu-id="c9c57-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="c9c57-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c9c57-111">サーバー側でのアニメーションを変更することも</span><span class="sxs-lookup"><span data-stu-id="c9c57-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="c9c57-112">手順</span><span class="sxs-lookup"><span data-stu-id="c9c57-112">Steps</span></span>

<span data-ttu-id="c9c57-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="c9c57-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="c9c57-114">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="c9c57-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="c9c57-115">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="c9c57-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="c9c57-116">コードの残りの部分がサーバー側で実行し、マークアップ; を使用しません代わりに、作成するコードを使用して、`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c9c57-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="c9c57-117">ただし、コントロール Toolkit 現在は提供されません API アクセスを個々 のアニメーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c9c57-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="c9c57-118">ただし、これを設定すること、`AnimationExtender`のアニメーション プロパティを文字列には、アニメーションを宣言して割り当てる場合に使用する XML マークアップを含んでいます。</span><span class="sxs-lookup"><span data-stu-id="c9c57-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="c9c57-119">含めることはできませんが、XML を作成するために、 `<Animations>` .NET Framework の XML を使用する可能性があります要素は、サポートまたは、次のコードのようにだけを指定して、文字列。</span><span class="sxs-lookup"><span data-stu-id="c9c57-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="c9c57-120">最後に、追加、`AnimationExtender`内で、現在のページを制御、`<form runat="server">`要素、アニメーションが含まれるして実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c9c57-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="c9c57-121">[![サーバー側の C #/vb のコードを使用して、アニメーションを作成します。](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c9c57-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="c9c57-122">サーバー側の C #/vb のコードを使用して、アニメーションを作成 ([フルサイズのイメージを表示するをクリックして](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c9c57-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9c57-123">[前へ](triggering-an-animation-in-another-control-vb.md)
> [次へ](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c9c57-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
