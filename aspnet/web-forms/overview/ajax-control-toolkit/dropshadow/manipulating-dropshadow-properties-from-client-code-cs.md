---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: クライアント コード (c#) から DropShadow プロパティを操作する |Microsoft Docs
author: wenz
description: DataList の編集インターフェイスをカスタマイズします。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e3166b9da97a0f4097566b62ba52b6d672eab78f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377286"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="05da9-103">クライアント コード (c#) から DropShadow プロパティを操作します。</span><span class="sxs-lookup"><span data-stu-id="05da9-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="05da9-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="05da9-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="05da9-105">[コードのダウンロード](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="05da9-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="05da9-106">DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="05da9-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="05da9-107">クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="05da9-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="05da9-108">概要</span><span class="sxs-lookup"><span data-stu-id="05da9-108">Overview</span></span>

<span data-ttu-id="05da9-109">DropShadow コントロール、AJAX Control Toolkit では、影付きのパネルを拡張します。</span><span class="sxs-lookup"><span data-stu-id="05da9-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="05da9-110">クライアントの JavaScript コードを使用して、このエクステンダーのプロパティを変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="05da9-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="05da9-111">手順</span><span class="sxs-lookup"><span data-stu-id="05da9-111">Steps</span></span>

<span data-ttu-id="05da9-112">コードは、いくつかの行のテキストを格納しているパネルで始まります。</span><span class="sxs-lookup"><span data-stu-id="05da9-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="05da9-113">関連付けられている CSS クラスは、便利な背景色をパネルになります。</span><span class="sxs-lookup"><span data-stu-id="05da9-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="05da9-114">`DropShadowExtender`不透明度が 50% に設定、ドロップ シャドウ効果があるパネルの拡張に追加されます。</span><span class="sxs-lookup"><span data-stu-id="05da9-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="05da9-115">ASP.NET AJAX では、`ScriptManager`コントロールが動作する Control Toolkit を使用します。</span><span class="sxs-lookup"><span data-stu-id="05da9-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="05da9-116">別のパネルにドロップ シャドウの不透明度を設定するための 2 つの JavaScript リンクが含まれています: 負符号のリンクが低下シャドウの不透明度、プラスのリンクでが増加します。</span><span class="sxs-lookup"><span data-stu-id="05da9-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="05da9-117">JavaScript 関数`changeOpacity()`まず見つける必要がありますし、`DropShadowExtender`ページ上のコントロール。</span><span class="sxs-lookup"><span data-stu-id="05da9-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="05da9-118">ASP.NET AJAX の定義、`$find()`正確にタスクのメソッド。</span><span class="sxs-lookup"><span data-stu-id="05da9-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="05da9-119">次に、`get_Opacity()`メソッドは現在の不透明度を取得、`set_Opacity()`メソッドはそれを設定します。</span><span class="sxs-lookup"><span data-stu-id="05da9-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="05da9-120">JavaScript コードは、現在の不透明度値を代入するし、`<label>`要素。</span><span class="sxs-lookup"><span data-stu-id="05da9-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="05da9-121">[![クライアント側で不透明度が変更されました。](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="05da9-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="05da9-122">クライアント側で不透明度が変更された ([フルサイズの画像を表示する をクリックします](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="05da9-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="05da9-123">[前へ](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [次へ](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="05da9-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
