---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: "アコーディオン ペイン (VB) を動的に追加する |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。 パネルは通常、w を宣言しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adc0b7985e5ba3da1684b645ae1b71b5112594a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="df2cf-104">アコーディオン ペイン (VB) を動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="df2cf-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="df2cf-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="df2cf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="df2cf-106">[コードをダウンロードする](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="df2cf-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="df2cf-107">AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="df2cf-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="df2cf-108">パネルは通常ページ自体内で宣言されていますが、サーバー側のコードは、同じ結果を実現するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="df2cf-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="df2cf-109">概要</span><span class="sxs-lookup"><span data-stu-id="df2cf-109">Overview</span></span>

<span data-ttu-id="df2cf-110">AJAX コントロールのツールキットでアコーディオン コントロールは、複数のペインがあり、時にそれらのいずれかを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="df2cf-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="df2cf-111">パネルは通常ページ自体内で宣言されていますが、サーバー側のコードは、同じ結果を実現するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="df2cf-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="df2cf-112">手順</span><span class="sxs-lookup"><span data-stu-id="df2cf-112">Steps</span></span>

<span data-ttu-id="df2cf-113">アコーディオン コントロールでは、サーバー側のコードをすべての重要なプロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="df2cf-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="df2cf-114">特に、`Panes`プロパティは、アコーディオンを構成するペインのコレクションへのアクセスを付与します。</span><span class="sxs-lookup"><span data-stu-id="df2cf-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="df2cf-115">型があるすべてのウィンドウ`AccordionPane`します。</span><span class="sxs-lookup"><span data-stu-id="df2cf-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="df2cf-116">つまり、このようなウィンドウを作成する単純です。</span><span class="sxs-lookup"><span data-stu-id="df2cf-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="df2cf-117">`HeaderContainer`プロパティの`AccordionPane`;、ウィンドウのヘッダー セクション内の ASP.NET コントロールにアクセスできるように、`ContentContainer`のプロパティ`AccordionPane`ウィンドウの [コンテンツ] に対しても同様です。</span><span class="sxs-lookup"><span data-stu-id="df2cf-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="df2cf-118">これにより、ASP.NET コード ペインにコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="df2cf-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="df2cf-119">Pane(s) を最後に、追加する必要があります、`Panes`アコーディオンのコレクション。</span><span class="sxs-lookup"><span data-stu-id="df2cf-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="df2cf-120">アコーディオン コントロールに 2 つのペインを追加する完全なサーバー側コードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="df2cf-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="df2cf-121">不足している唯一の要素は、アコーディオン自体には、ASP.NET の存在に依存する`ScriptManager`コントロール。</span><span class="sxs-lookup"><span data-stu-id="df2cf-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="df2cf-122">この例を完了するには、アコーディオン コントロールで参照されている 2 つの CSS クラスは、ブラウザーのスタイル情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="df2cf-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="df2cf-123">[![アコーディオン内のデータはサーバー側コードによって動的に追加されました](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df2cf-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="df2cf-124">アコーディオン内のデータはサーバー側コードによって動的に追加されました ([フルサイズのイメージを表示するをクリックして](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="df2cf-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="df2cf-125">前へ</span><span class="sxs-lookup"><span data-stu-id="df2cf-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
