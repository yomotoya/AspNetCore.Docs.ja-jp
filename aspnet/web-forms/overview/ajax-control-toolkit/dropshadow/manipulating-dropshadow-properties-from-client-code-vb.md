---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: (VB) のクライアント コードから DropShadow プロパティを操作する |Microsoft Docs
author: wenz
description: DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。 クライアント JavaScrip を使用して、このエクステンダーのプロパティを変更することもしています.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b8330174f6f49e96c42a0e15eeaf934bf24f87
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836661"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="89ac9-104">(VB) のクライアント コードから DropShadow プロパティを操作します。</span><span class="sxs-lookup"><span data-stu-id="89ac9-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="89ac9-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="89ac9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="89ac9-106">[コードのダウンロード](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="89ac9-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="89ac9-107">DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="89ac9-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="89ac9-108">クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="89ac9-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="89ac9-109">概要</span><span class="sxs-lookup"><span data-stu-id="89ac9-109">Overview</span></span>

<span data-ttu-id="89ac9-110">DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="89ac9-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="89ac9-111">クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="89ac9-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="89ac9-112">手順</span><span class="sxs-lookup"><span data-stu-id="89ac9-112">Steps</span></span>

<span data-ttu-id="89ac9-113">コードは、いくつかの行のテキストを格納しているパネルで始まります。</span><span class="sxs-lookup"><span data-stu-id="89ac9-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="89ac9-114">関連付けられている CSS クラスは、便利な背景色をパネルになります。</span><span class="sxs-lookup"><span data-stu-id="89ac9-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="89ac9-115">`DropShadowExtender`不透明度が 50% に設定、ドロップ シャドウ効果があるパネルの拡張に追加されます。</span><span class="sxs-lookup"><span data-stu-id="89ac9-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="89ac9-116">ASP.NET AJAX では、`ScriptManager`コントロールが動作する Control Toolkit を使用します。</span><span class="sxs-lookup"><span data-stu-id="89ac9-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="89ac9-117">別のパネルにドロップ シャドウの不透明度を設定するための 2 つの JavaScript リンクが含まれています: 負符号のリンクが低下シャドウの不透明度、プラスのリンクでが増加します。</span><span class="sxs-lookup"><span data-stu-id="89ac9-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="89ac9-118">JavaScript 関数`changeOpacity()`まず見つける必要がありますし、`DropShadowExtender`ページ上のコントロール。</span><span class="sxs-lookup"><span data-stu-id="89ac9-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="89ac9-119">ASP.NET AJAX の定義、`$find()`正確にタスクのメソッド。</span><span class="sxs-lookup"><span data-stu-id="89ac9-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="89ac9-120">次に、`get_Opacity()`メソッドは現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。</span><span class="sxs-lookup"><span data-stu-id="89ac9-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="89ac9-121">JavaScript コードは、現在の不透明度値を代入するし、`<label>`要素。</span><span class="sxs-lookup"><span data-stu-id="89ac9-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="89ac9-122">[![クライアント側で不透明度が変更されました。](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="89ac9-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="89ac9-123">クライアント側で不透明度が変更された ([フルサイズの画像を表示する をクリックします](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="89ac9-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="89ac9-124">前へ</span><span class="sxs-lookup"><span data-stu-id="89ac9-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
