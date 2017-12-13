---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: "UpdatePanel のアニメーション (c#) を動的に制御する |Microsoft ドキュメント"
author: wenz
description: "アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 内容を."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 0408556e322a46211388fbd557d7a967044129ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="32ae8-104">UpdatePanel のアニメーションに動的に制御する (c#)</span><span class="sxs-lookup"><span data-stu-id="32ae8-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>
====================
<span data-ttu-id="32ae8-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="32ae8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="32ae8-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="32ae8-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="32ae8-107">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="32ae8-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="32ae8-108">UpdatePanel の内容は、の特別な extender が存在し、アニメーション、フレームワークに大きく依存して: UpdatePanelAnimation です。</span><span class="sxs-lookup"><span data-stu-id="32ae8-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="32ae8-109">UpdatePanel トリガーと共にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="32ae8-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="32ae8-110">概要</span><span class="sxs-lookup"><span data-stu-id="32ae8-110">Overview</span></span>

<span data-ttu-id="32ae8-111">アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="32ae8-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="32ae8-112">内容を`UpdatePanel`、特別な extender が存在するアニメーション フレームワークに大きく依存している:`UpdatePanelAnimation`です。</span><span class="sxs-lookup"><span data-stu-id="32ae8-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="32ae8-113">と共に使用するも`UpdatePanel`トリガーします。</span><span class="sxs-lookup"><span data-stu-id="32ae8-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="32ae8-114">手順</span><span class="sxs-lookup"><span data-stu-id="32ae8-114">Steps</span></span>

<span data-ttu-id="32ae8-115">含めるには、まず通常どおり、 `ScriptManager`  ページで、ASP.NET AJAX ライブラリが読み込まれ、コントロール ツールキットを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="32ae8-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="32ae8-116">このシナリオでは、アニメーションは、現在の時刻の表示に適用されます。</span><span class="sxs-lookup"><span data-stu-id="32ae8-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="32ae8-117">この情報を使用してラベルに書き込まれることができます、`Page_Load()`メソッド、または次のインライン コードの使用 (ここではわかりやすくするため)。</span><span class="sxs-lookup"><span data-stu-id="32ae8-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="32ae8-118">また、時刻が更新をトリガーするボタンが作成されます。</span><span class="sxs-lookup"><span data-stu-id="32ae8-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="32ae8-119">このコードに格納し、`<ContentTemplate>`のセクションで、`UpdatePanel`要素。</span><span class="sxs-lookup"><span data-stu-id="32ae8-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="32ae8-120">パネルの`UpdateMode`に属性を設定する必要があります`"Conditional"`トリガーのみはパネルの内容を更新するため、します。</span><span class="sxs-lookup"><span data-stu-id="32ae8-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="32ae8-121">`<Triggers>`のセクション、 `UpdatePanel`、非同期ポストバック トリガーが作成されに関連付けられている、`Click`ボタンのイベントです。</span><span class="sxs-lookup"><span data-stu-id="32ae8-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="32ae8-122">したがって、ユーザーが、ボタンをクリックした場合、`UpdatePanel`は更新します。</span><span class="sxs-lookup"><span data-stu-id="32ae8-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="32ae8-123">ここでは、マークアップを`UpdatePanel`コントロール。</span><span class="sxs-lookup"><span data-stu-id="32ae8-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="32ae8-124">最後に、`UpdatePanelAnimationExtender`構成する必要があります。 設定、`TargetControlID`属性パネルの ID をおよびエクステンダー内でアニメーションを定義します。</span><span class="sxs-lookup"><span data-stu-id="32ae8-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="32ae8-125">フェードする意味で、優れた視覚的に重点を置いてが更新された時間を作成します。</span><span class="sxs-lookup"><span data-stu-id="32ae8-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="32ae8-126">Extender マークアップは可能性がありますし、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="32ae8-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="32ae8-127">ブラウザーで、ファイルを実行します。</span><span class="sxs-lookup"><span data-stu-id="32ae8-127">Run the file in the browser.</span></span> <span data-ttu-id="32ae8-128">ボタンをクリックするたびに常に 1 秒の間のフェードインのパネルで、現在の時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="32ae8-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="32ae8-129">[![現在の時刻がフェードインします。](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="32ae8-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="32ae8-130">現在の時刻がフェードイン ([フルサイズのイメージを表示するをクリックして](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="32ae8-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="32ae8-131">[前へ](animating-an-updatepanel-control-cs.md)
[次へ](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="32ae8-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
