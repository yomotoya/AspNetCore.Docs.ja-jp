---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: "自動ポストバック (c#) でスライダー コントロールの使用 |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。 スライダーを自動転記を作成しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: f6291f4162b3069a6316a60b4b29f82f55121aac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="d41c3-104">自動ポストバック (c#) でスライダー コントロールの使用</span><span class="sxs-lookup"><span data-stu-id="d41c3-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="d41c3-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d41c3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d41c3-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d41c3-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="d41c3-107">AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="d41c3-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="d41c3-108">することがスライダー autopostback 1 回値が変化します。</span><span class="sxs-lookup"><span data-stu-id="d41c3-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="d41c3-109">概要</span><span class="sxs-lookup"><span data-stu-id="d41c3-109">Overview</span></span>

<span data-ttu-id="d41c3-110">AJAX コントロール Toolkit のスライダー コントロールは、マウスを使用して制御できるグラフィカルなスライダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="d41c3-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="d41c3-111">することがスライダー autopostback 1 回値が変化します。</span><span class="sxs-lookup"><span data-stu-id="d41c3-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="d41c3-112">手順</span><span class="sxs-lookup"><span data-stu-id="d41c3-112">Steps</span></span>

<span data-ttu-id="d41c3-113">両方のテキスト ボックス、スライダーを変更されたときにポストバックが自動的に行うために、属性が必要な`AutoPostBack="true"`: 自体、スライダーとなるテキスト ボックスとは、スライダーの位置を保持するテキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="d41c3-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="d41c3-114">その必要なマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d41c3-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="d41c3-115">`SliderExtender` ASP.NET AJAX コントロール Toolkit からコントロールが 2 つのテキスト ボックスにスライダー機能を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="d41c3-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="d41c3-116">追加のラベル要素は、ポストバックのユーザーを通知するために後で使用されます。</span><span class="sxs-lookup"><span data-stu-id="d41c3-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="d41c3-117">最後に、 `ScriptManager` ASP.NET AJAX のコントロールが動作するコントロール ツールキットの必要な JavaScript を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="d41c3-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="d41c3-118">これで、スライダーがポスト バックです。サーバー側では、このイベントをキャッチし、処理する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d41c3-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="d41c3-119">[![ポストバックをトリガーするスライダーの移動](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d41c3-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="d41c3-120">ポストバックをトリガーするスライダーの移動 ([フルサイズのイメージを表示するをクリックして](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d41c3-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="d41c3-121">[![この変更の日付がラベルで記述された後、](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d41c3-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="d41c3-122">その後、この変更の日付は、ラベルに書き込まれます ([フルサイズのイメージを表示するをクリックして](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d41c3-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d41c3-123">次へ</span><span class="sxs-lookup"><span data-stu-id="d41c3-123">Next</span></span>](databinding-the-slider-control-cs.md)
