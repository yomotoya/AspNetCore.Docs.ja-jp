---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: UpdatePanel アニメーション (c#) を動的に制御する |Microsoft Docs
author: wenz
description: アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。 内容として、.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: c55381548b00866c4e4fc9244392c00af201f62a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822604"
---
<a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="8b354-104">UpdatePanel アニメーション (c#) を動的に制御します。</span><span class="sxs-lookup"><span data-stu-id="8b354-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>
====================
<span data-ttu-id="8b354-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8b354-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8b354-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8b354-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="8b354-107">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="8b354-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b354-108">UpdatePanel の内容は、特別なエクステンダーが存在アニメーション フレームワークに大きく依存している: UpdatePanelAnimation します。</span><span class="sxs-lookup"><span data-stu-id="8b354-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="8b354-109">これは UpdatePanel トリガーと共にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="8b354-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="8b354-110">概要</span><span class="sxs-lookup"><span data-stu-id="8b354-110">Overview</span></span>

<span data-ttu-id="8b354-111">アニメーション コントロール、ASP.NET AJAX Control Toolkit ではなくコントロールだけをコントロールにアニメーションを追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="8b354-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b354-112">内容として、 `UpdatePanel`、アニメーション フレームワークに大きく依存している特別なエクステンダーが存在します:`UpdatePanelAnimation`します。</span><span class="sxs-lookup"><span data-stu-id="8b354-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="8b354-113">でも機能と共に`UpdatePanel`トリガーします。</span><span class="sxs-lookup"><span data-stu-id="8b354-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="8b354-114">手順</span><span class="sxs-lookup"><span data-stu-id="8b354-114">Steps</span></span>

<span data-ttu-id="8b354-115">最初の手順は、通常どおりに含める、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、Control Toolkit を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="8b354-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="8b354-116">このシナリオでは、アニメーションは、現在の時刻の表示に適用されます。</span><span class="sxs-lookup"><span data-stu-id="8b354-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="8b354-117">この情報を使用して、ラベルに書き込まれることができます、`Page_Load()`メソッド、または次のインライン コードの使用 (わかりやすくする)。</span><span class="sxs-lookup"><span data-stu-id="8b354-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="8b354-118">また、時間の更新をトリガーするボタンが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8b354-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="8b354-119">このコードに配置し、`<ContentTemplate>`のセクション、`UpdatePanel`要素。</span><span class="sxs-lookup"><span data-stu-id="8b354-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="8b354-120">パネルの`UpdateMode`に属性を設定する必要があります`"Conditional"`、トリガーのみがパネルの内容を更新します。</span><span class="sxs-lookup"><span data-stu-id="8b354-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="8b354-121">`<Triggers>`のセクション、 `UpdatePanel`、非同期ポストバックのトリガーが作成されに関連付けられている、`Click`ボタンのイベント。</span><span class="sxs-lookup"><span data-stu-id="8b354-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="8b354-122">したがって、ユーザーが、ボタンをクリックした場合、`UpdatePanel`が更新されます。</span><span class="sxs-lookup"><span data-stu-id="8b354-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="8b354-123">次のマークアップに示します、`UpdatePanel`コントロール。</span><span class="sxs-lookup"><span data-stu-id="8b354-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="8b354-124">最後に、`UpdatePanelAnimationExtender`構成する必要があります。 設定、 `TargetControlID` 、パネルの id 属性と、エクステンダー内でアニメーションを定義します。</span><span class="sxs-lookup"><span data-stu-id="8b354-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="8b354-125">フェードはある意味、更新された時間に優れた visual 重点を置いたを作成します。</span><span class="sxs-lookup"><span data-stu-id="8b354-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="8b354-126">エクステンダーのマークアップには可能性がありますし、このようになります。</span><span class="sxs-lookup"><span data-stu-id="8b354-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="8b354-127">ブラウザーでファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="8b354-127">Run the file in the browser.</span></span> <span data-ttu-id="8b354-128">ボタンをクリックしたときに、常に 1 秒の間フェードのパネルで、現在の時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8b354-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="8b354-129">[![現在の時刻がフェードインします。](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8b354-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="8b354-130">現在の時刻がフェードイン ([フルサイズの画像を表示する をクリックします](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="8b354-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8b354-131">[前へ](animating-an-updatepanel-control-cs.md)
> [次へ](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8b354-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
