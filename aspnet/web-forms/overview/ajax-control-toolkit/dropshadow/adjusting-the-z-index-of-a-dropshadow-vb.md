---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: DropShadow (VB) の Z インデックスを調整する |Microsoft ドキュメント
author: wenz
description: AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。 ただしこのシャドウは、insta の他のコントロールを持つ場合があります競合しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871299"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="deed4-104">DropShadow (VB) の Z インデックスを調整します。</span><span class="sxs-lookup"><span data-stu-id="deed4-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="deed4-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="deed4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="deed4-106">[コードをダウンロードする](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="deed4-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="deed4-107">AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="deed4-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="deed4-108">ただしこのシャドウは、他のコントロールでは、ASP.NET メニュー コントロールのインスタンスとも競合しています。</span><span class="sxs-lookup"><span data-stu-id="deed4-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="deed4-109">ときにメニュー項目が表示されます、ドロップ シャドウの背後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="deed4-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="deed4-110">概要</span><span class="sxs-lookup"><span data-stu-id="deed4-110">Overview</span></span>

<span data-ttu-id="deed4-111">AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="deed4-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="deed4-112">ただしこのシャドウは、他のコントロールでは、ASP.NET メニュー コントロールのインスタンスとも競合しています。</span><span class="sxs-lookup"><span data-stu-id="deed4-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="deed4-113">ときにメニュー項目が表示されます、ドロップ シャドウの背後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="deed4-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="deed4-114">手順</span><span class="sxs-lookup"><span data-stu-id="deed4-114">Steps</span></span>

<span data-ttu-id="deed4-115">パネルが影響を表示するのに十分なテキストが含まれるように、十分なテキストを含む自体、パネルを使用したコードの開始します。</span><span class="sxs-lookup"><span data-stu-id="deed4-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="deed4-116">別のパネルが直前に配置されて、`panelShadow`パネルです。</span><span class="sxs-lookup"><span data-stu-id="deed4-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="deed4-117">含まれている水平方向のメニューにメニュー項目が表示されるように (または: の下)、`dropShadow`パネル)。</span><span class="sxs-lookup"><span data-stu-id="deed4-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="deed4-118">次に、`DropShadowExtender`を拡張する追加の`panelShadow`ドロップ シャドウ効果のあるパネル。</span><span class="sxs-lookup"><span data-stu-id="deed4-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="deed4-119">最後に、ASP.NET AJAX`ScriptManager`コントロール動作を制御ツールキットを有効にします。</span><span class="sxs-lookup"><span data-stu-id="deed4-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="deed4-120">このスクリプトを実行すると、パネルの下にあるメニュー項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="deed4-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="deed4-121">ただし、メニューは、CSS クラスを使用`panel`だけがある要素がもう一方のパネルの前に表示する 2 つの処理を定義します。</span><span class="sxs-lookup"><span data-stu-id="deed4-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="deed4-122">相対配置</span><span class="sxs-lookup"><span data-stu-id="deed4-122">Relative positioning</span></span>
- <span data-ttu-id="deed4-123">正の z インデックス</span><span class="sxs-lookup"><span data-stu-id="deed4-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="deed4-124">次に、`DropShadowExtender`コントロールはメニュー コントロールを使用できなくなった場合は競合しません。</span><span class="sxs-lookup"><span data-stu-id="deed4-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="deed4-125">[![前: に、メニュー項目が表示されていません。](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="deed4-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="deed4-126">前: に、メニュー エントリは表示されません ([フルサイズのイメージを表示するをクリックして](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="deed4-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="deed4-127">[![メニュー項目の表示後。](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="deed4-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="deed4-128">後: メニュー項目が表示されます ([フルサイズのイメージを表示するをクリックして](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="deed4-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="deed4-129">[前へ](manipulating-dropshadow-properties-from-client-code-cs.md)
> [次へ](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="deed4-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
