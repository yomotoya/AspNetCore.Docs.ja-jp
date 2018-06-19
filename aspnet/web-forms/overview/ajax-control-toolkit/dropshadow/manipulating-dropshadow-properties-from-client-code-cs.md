---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: クライアント コード (c#) から DropShadow プロパティの操作 |Microsoft ドキュメント
author: wenz
description: DataList の編集インターフェイスのカスタマイズ
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870337"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="b188d-103">クライアント コード (c#) から DropShadow プロパティを操作します。</span><span class="sxs-lookup"><span data-stu-id="b188d-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="b188d-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b188d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b188d-105">[コードをダウンロードする](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b188d-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="b188d-106">AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="b188d-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b188d-107">クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="b188d-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="b188d-108">概要</span><span class="sxs-lookup"><span data-stu-id="b188d-108">Overview</span></span>

<span data-ttu-id="b188d-109">AJAX コントロール ツールキットの DropShadow コントロールは、ドロップ シャドウ付きパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="b188d-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="b188d-110">クライアントの JavaScript コードを使用してこのエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="b188d-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="b188d-111">手順</span><span class="sxs-lookup"><span data-stu-id="b188d-111">Steps</span></span>

<span data-ttu-id="b188d-112">コードは、いくつかの行のテキストを含むパネルで始まります。</span><span class="sxs-lookup"><span data-stu-id="b188d-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="b188d-113">関連付けられている CSS クラスには、パネル nice の背景色が与えられます。</span><span class="sxs-lookup"><span data-stu-id="b188d-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="b188d-114">`DropShadowExtender`パネルの ドロップ シャドウ効果、不透明度の 50% に設定と拡張に追加されます。</span><span class="sxs-lookup"><span data-stu-id="b188d-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="b188d-115">ASP.NET AJAX ではその後、`ScriptManager`コントロール動作を制御ツールキットを有効にします。</span><span class="sxs-lookup"><span data-stu-id="b188d-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="b188d-116">別のパネルには、影の不透明度を設定するための 2 つの JavaScript リンクが含まれています。 負符号のリンクは、影の不透明度を減少、正符号のリンクでが増加します。</span><span class="sxs-lookup"><span data-stu-id="b188d-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="b188d-117">JavaScript 関数`changeOpacity()`し、最初に検索する必要があります、`DropShadowExtender`ページ上のコントロールです。</span><span class="sxs-lookup"><span data-stu-id="b188d-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="b188d-118">ASP.NET AJAX を定義、`$find()`正確にそのタスクのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="b188d-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="b188d-119">次に、`get_Opacity()`メソッドは、現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。</span><span class="sxs-lookup"><span data-stu-id="b188d-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="b188d-120">JavaScript コードが現在の不透明度の値、`<label>`要素。</span><span class="sxs-lookup"><span data-stu-id="b188d-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="b188d-121">[![クライアント側の不透明度を変更します。](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b188d-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="b188d-122">クライアント側の不透明度を変更 ([フルサイズのイメージを表示するをクリックして](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b188d-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b188d-123">[前へ](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [次へ](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b188d-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
