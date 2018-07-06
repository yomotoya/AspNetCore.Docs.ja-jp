---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: スライダー コントロールを使用すると自動ポストバック (c#) |Microsoft Docs
author: wenz
description: スライダー コントロール、AJAX Control Toolkit では、マウスを使用して制御できるグラフィカルなスライダーを提供します。 スライダー自動転記を作成することはしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cb3c041f2a8a499d27cbcbc2f8975eedcac12e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834152"
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="8bfcb-104">自動ポストバック (c#) でスライダー コントロールの使用</span><span class="sxs-lookup"><span data-stu-id="8bfcb-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="8bfcb-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8bfcb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8bfcb-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8bfcb-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="8bfcb-107">スライダー コントロール、AJAX Control Toolkit では、マウスを使用して制御できるグラフィカルなスライダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8bfcb-108">ようにスライダー autopostback 1 回、値が変化することになります。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="8bfcb-109">概要</span><span class="sxs-lookup"><span data-stu-id="8bfcb-109">Overview</span></span>

<span data-ttu-id="8bfcb-110">スライダー コントロール、AJAX Control Toolkit では、マウスを使用して制御できるグラフィカルなスライダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="8bfcb-111">ようにスライダー autopostback 1 回、値が変化することになります。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="8bfcb-112">手順</span><span class="sxs-lookup"><span data-stu-id="8bfcb-112">Steps</span></span>

<span data-ttu-id="8bfcb-113">両方のテキスト ボックス、スライダーを変更すると自動的にポストバックするには、するには、属性が必要な`AutoPostBack="true"`: テキスト ボックス自体には、スライダーになると、スライダーの位置を保持するテキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="8bfcb-114">そのための必要なマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="8bfcb-115">`SliderExtender` ASP.NET AJAX Control toolkit コントロールが 2 つのテキスト ボックスに、スライダーの機能を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="8bfcb-116">追加のラベル要素が、ポストバックのユーザーに通知するために後で使用されます。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="8bfcb-117">最後に、 `ScriptManager` ASP.NET AJAX のコントロールが動作する Control Toolkit の必要な JavaScript を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="8bfcb-118">これで、スライダーがポスト バック;サーバー側で、このイベントをキャッチして処理して可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="8bfcb-119">[![ポストバックをトリガーするスライダーの移動](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8bfcb-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="8bfcb-120">ポストバックをトリガーするスライダーの移動 ([フルサイズの画像を表示する をクリックします](using-the-slider-control-with-auto-postback-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="8bfcb-121">[![その後、この変更の日付は、ラベルに書き込まれます](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8bfcb-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="8bfcb-122">その後、この変更の日付は、ラベルに書き込まれます ([フルサイズの画像を表示する をクリックします](using-the-slider-control-with-auto-postback-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="8bfcb-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8bfcb-123">次へ</span><span class="sxs-lookup"><span data-stu-id="8bfcb-123">Next</span></span>](databinding-the-slider-control-cs.md)
