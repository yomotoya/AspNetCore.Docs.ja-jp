---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: (VB) DropShadow の Z インデックスを調整 |Microsoft Docs
author: wenz
description: DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 ただし場合がありますこのシャドウがインストールの他のコントロールと競合しています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 78697f51a09dfaad315255efa23120d4c456bfea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829545"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="8172f-104">(VB) DropShadow の Z インデックスを調整します。</span><span class="sxs-lookup"><span data-stu-id="8172f-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="8172f-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8172f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8172f-106">[コードのダウンロード](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8172f-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="8172f-107">DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="8172f-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8172f-108">ただしこのシャドウは、他のコントロールでは、ASP.NET の Menu コントロールのインスタンスとも競合しています。</span><span class="sxs-lookup"><span data-stu-id="8172f-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="8172f-109">ときにメニュー エントリが表示されます、ドロップ シャドウの背後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8172f-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="8172f-110">概要</span><span class="sxs-lookup"><span data-stu-id="8172f-110">Overview</span></span>

<span data-ttu-id="8172f-111">DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="8172f-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8172f-112">ただしこのシャドウは、他のコントロールでは、ASP.NET の Menu コントロールのインスタンスとも競合しています。</span><span class="sxs-lookup"><span data-stu-id="8172f-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="8172f-113">ときにメニュー エントリが表示されます、ドロップ シャドウの背後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8172f-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="8172f-114">手順</span><span class="sxs-lookup"><span data-stu-id="8172f-114">Steps</span></span>

<span data-ttu-id="8172f-115">コードは、パネルが表示される効果のための十分なテキストが含まれるように、十分なテキストを含む、パネル自体には、開始します。</span><span class="sxs-lookup"><span data-stu-id="8172f-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="8172f-116">別のパネルが直前に配置されて、`panelShadow`パネル。</span><span class="sxs-lookup"><span data-stu-id="8172f-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="8172f-117">上メニュー エントリが表示されるように水平方向のメニューが含まれている (または: )、`dropShadow`パネル)。</span><span class="sxs-lookup"><span data-stu-id="8172f-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="8172f-118">次に、`DropShadowExtender`拡張に追加されます、`panelShadow`ドロップ シャドウ効果を使用したパネル。</span><span class="sxs-lookup"><span data-stu-id="8172f-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="8172f-119">ASP.NET AJAX では最後に、`ScriptManager`コントロールが動作する Control Toolkit を使用します。</span><span class="sxs-lookup"><span data-stu-id="8172f-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="8172f-120">このスクリプトを実行すると、パネルの下にあるメニュー エントリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8172f-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="8172f-121">ただし、メニューが CSS クラスを使用して`panel`だけがある他のパネルの前に表示される要素を次の 2 つを定義します。</span><span class="sxs-lookup"><span data-stu-id="8172f-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="8172f-122">相対配置</span><span class="sxs-lookup"><span data-stu-id="8172f-122">Relative positioning</span></span>
- <span data-ttu-id="8172f-123">正の z インデックス</span><span class="sxs-lookup"><span data-stu-id="8172f-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="8172f-124">次に、 `DropShadowExtender` Menu コントロールとコントロールが不要に競合しません。</span><span class="sxs-lookup"><span data-stu-id="8172f-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="8172f-125">[![前: に、メニュー項目が表示されていません。](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8172f-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="8172f-126">Before: メニューのエントリが表示されていない ([フルサイズの画像を表示する をクリックします](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="8172f-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="8172f-127">[![メニュー エントリの表示後。](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8172f-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="8172f-128">処理の後: メニュー項目が表示されます ([フルサイズの画像を表示する をクリックします](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="8172f-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8172f-129">[前へ](manipulating-dropshadow-properties-from-client-code-cs.md)
> [次へ](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8172f-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
