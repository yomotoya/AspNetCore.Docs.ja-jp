---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: クライアント コード (VB) から DropShadow プロパティの操作 |Microsoft ドキュメント
author: wenz
description: AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。 クライアント JavaScrip を使用しても、このエクステンダーのプロパティを変更できます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870909"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="9cb9f-104">クライアント コード (VB) から DropShadow プロパティを操作します。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="9cb9f-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9cb9f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9cb9f-106">[コードをダウンロードする](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9cb9f-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="9cb9f-107">AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="9cb9f-108">クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="9cb9f-109">概要</span><span class="sxs-lookup"><span data-stu-id="9cb9f-109">Overview</span></span>

<span data-ttu-id="9cb9f-110">AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="9cb9f-111">クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="9cb9f-112">手順</span><span class="sxs-lookup"><span data-stu-id="9cb9f-112">Steps</span></span>

<span data-ttu-id="9cb9f-113">コードは、いくつかの行のテキストを含むパネルで始まります。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="9cb9f-114">関連付けられている CSS クラスには、パネル nice の背景色が与えられます。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="9cb9f-115">`DropShadowExtender`パネルの ドロップ シャドウ効果、不透明度の 50% に設定と拡張に追加されます。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="9cb9f-116">ASP.NET AJAX ではその後、`ScriptManager`コントロール動作を制御ツールキットを有効にします。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="9cb9f-117">別のパネルには、影の不透明度を設定するための 2 つの JavaScript リンクが含まれています。 負符号のリンクは、影の不透明度を減少、正符号のリンクでが増加します。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="9cb9f-118">JavaScript 関数`changeOpacity()`し、最初に検索する必要があります、`DropShadowExtender`ページ上のコントロールです。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="9cb9f-119">ASP.NET AJAX を定義、`$find()`正確にそのタスクのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="9cb9f-120">次に、`get_Opacity()`メソッドは、現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="9cb9f-121">JavaScript コードが現在の不透明度の値、`<label>`要素。</span><span class="sxs-lookup"><span data-stu-id="9cb9f-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="9cb9f-122">[![クライアント側の不透明度を変更します。](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9cb9f-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="9cb9f-123">クライアント側の不透明度を変更 ([フルサイズのイメージを表示するをクリックして](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9cb9f-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9cb9f-124">前へ</span><span class="sxs-lookup"><span data-stu-id="9cb9f-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
