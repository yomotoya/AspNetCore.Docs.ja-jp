---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: UpdatePanel アニメーション (VB) を動的に制御する |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 内容を.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871546"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="3539f-104">UpdatePanel アニメーション (VB) を動的に制御します。</span><span class="sxs-lookup"><span data-stu-id="3539f-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="3539f-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3539f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3539f-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3539f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="3539f-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="3539f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3539f-108">UpdatePanel の内容は、の特別な extender が存在し、アニメーション、フレームワークに大きく依存して: UpdatePanelAnimation です。</span><span class="sxs-lookup"><span data-stu-id="3539f-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="3539f-109">UpdatePanel トリガーと共にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="3539f-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="3539f-110">概要</span><span class="sxs-lookup"><span data-stu-id="3539f-110">Overview</span></span>

<span data-ttu-id="3539f-111">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="3539f-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3539f-112">内容を`UpdatePanel`、特別な extender が存在するアニメーション フレームワークに大きく依存している:`UpdatePanelAnimation`です。</span><span class="sxs-lookup"><span data-stu-id="3539f-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="3539f-113">と共に使用するも`UpdatePanel`トリガーします。</span><span class="sxs-lookup"><span data-stu-id="3539f-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="3539f-114">手順</span><span class="sxs-lookup"><span data-stu-id="3539f-114">Steps</span></span>

<span data-ttu-id="3539f-115">含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3539f-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="3539f-116">このシナリオでは、アニメーションは、現在の時刻の表示に適用されます。</span><span class="sxs-lookup"><span data-stu-id="3539f-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="3539f-117">この情報を使用してラベルに書き込まれることができます、`Page_Load()`メソッド、または次のインライン コードの使用 (ここではわかりやすくするため)。</span><span class="sxs-lookup"><span data-stu-id="3539f-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="3539f-118">また、時刻が更新をトリガーするボタンが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3539f-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="3539f-119">このコードに格納し、`<ContentTemplate>`のセクションで、`UpdatePanel`要素。</span><span class="sxs-lookup"><span data-stu-id="3539f-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="3539f-120">パネルの`UpdateMode`に属性を設定する必要があります`"Conditional"`トリガーのみはパネルの内容を更新するため、します。</span><span class="sxs-lookup"><span data-stu-id="3539f-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="3539f-121">`<Triggers>`のセクション、 `UpdatePanel`、非同期ポストバック トリガーが作成されに関連付けられている、`Click`ボタンのイベントです。</span><span class="sxs-lookup"><span data-stu-id="3539f-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="3539f-122">したがって、ユーザーが、ボタンをクリックした場合、`UpdatePanel`は更新します。</span><span class="sxs-lookup"><span data-stu-id="3539f-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="3539f-123">ここでは、マークアップを`UpdatePanel`コントロール。</span><span class="sxs-lookup"><span data-stu-id="3539f-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="3539f-124">最後に、`UpdatePanelAnimationExtender`構成する必要があります。 設定、`TargetControlID`属性パネルの ID をおよびエクステンダー内でアニメーションを定義します。</span><span class="sxs-lookup"><span data-stu-id="3539f-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="3539f-125">フェードする意味で、優れた視覚的に重点を置いてが更新された時間を作成します。</span><span class="sxs-lookup"><span data-stu-id="3539f-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="3539f-126">Extender マークアップは可能性がありますし、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3539f-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="3539f-127">ブラウザーで、ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="3539f-127">Run the file in the browser.</span></span> <span data-ttu-id="3539f-128">ボタンをクリックするたびに常に 1 秒の間のフェードインのパネルで、現在の時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3539f-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="3539f-129">[![現在の時刻がフェードインします。](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3539f-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="3539f-130">現在の時刻がフェードイン ([フルサイズのイメージを表示するをクリックして](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3539f-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3539f-131">前へ</span><span class="sxs-lookup"><span data-stu-id="3539f-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
