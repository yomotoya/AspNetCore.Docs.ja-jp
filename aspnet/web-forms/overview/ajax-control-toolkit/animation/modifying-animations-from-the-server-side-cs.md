---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: "変更 (c#)、サーバー側からアニメーション |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アニメーションも可能性があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: e1f9bda03b3e3edf3bbdc591cde9858944d39321
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="dcccb-104">(C#)、サーバー側からのアニメーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="dcccb-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="dcccb-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dcccb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dcccb-106">[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="dcccb-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="dcccb-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="dcccb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dcccb-108">サーバー側でのアニメーションを変更することも</span><span class="sxs-lookup"><span data-stu-id="dcccb-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="dcccb-109">概要</span><span class="sxs-lookup"><span data-stu-id="dcccb-109">Overview</span></span>

<span data-ttu-id="dcccb-110">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="dcccb-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dcccb-111">サーバー側でのアニメーションを変更することも</span><span class="sxs-lookup"><span data-stu-id="dcccb-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="dcccb-112">手順</span><span class="sxs-lookup"><span data-stu-id="dcccb-112">Steps</span></span>

<span data-ttu-id="dcccb-113">まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="dcccb-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="dcccb-114">アニメーションは、次のようなテキストのパネルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="dcccb-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="dcccb-115">パネルの関連付けられている CSS クラス、nice の背景色を定義し、パネルの固定幅を設定します。</span><span class="sxs-lookup"><span data-stu-id="dcccb-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="dcccb-116">コードの残りの部分がサーバー側で実行し、マークアップ; を使用しません代わりに、作成するコードを使用して、`AnimationExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="dcccb-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="dcccb-117">ただし、コントロール Toolkit 現在は提供されません API アクセスを個々 のアニメーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="dcccb-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="dcccb-118">ただし、これを設定すること、`AnimationExtender`のアニメーション プロパティを文字列には、アニメーションを宣言して割り当てる場合に使用する XML マークアップを含んでいます。</span><span class="sxs-lookup"><span data-stu-id="dcccb-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="dcccb-119">含めることはできませんが、XML を作成するために、 `<Animations>` .NET Framework の XML を使用する可能性があります要素は、サポートまたは、次のコードのようにだけを指定して、文字列。</span><span class="sxs-lookup"><span data-stu-id="dcccb-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="dcccb-120">最後に、追加、`AnimationExtender`内で、現在のページを制御、`<form runat="server">`要素、アニメーションが含まれるして実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dcccb-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="dcccb-121">[![サーバー側の C #/vb のコードを使用して、アニメーションを作成します。](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dcccb-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="dcccb-122">サーバー側の C #/vb のコードを使用して、アニメーションを作成 ([フルサイズのイメージを表示するをクリックして](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dcccb-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dcccb-123">[前へ](triggering-an-animation-in-another-control-cs.md)
[次へ](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="dcccb-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
