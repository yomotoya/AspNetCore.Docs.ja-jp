---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: アコーディオン ウィンドウ (VB) を動的に追加する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。 パネルには、w を宣言は、通常は.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 1fdb95ee1ee93bc011a257e4e21c876dbbc7d2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820027"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="3d14a-104">アコーディオン ウィンドウ (VB) を動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="3d14a-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="3d14a-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3d14a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3d14a-106">[コードのダウンロード](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3d14a-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="3d14a-107">AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。</span><span class="sxs-lookup"><span data-stu-id="3d14a-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="3d14a-108">パネルがページ自体には、通常は宣言されているが、サーバー側のコードは、同じ結果を実現するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="3d14a-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="3d14a-109">概要</span><span class="sxs-lookup"><span data-stu-id="3d14a-109">Overview</span></span>

<span data-ttu-id="3d14a-110">AJAX Control Toolkit で、アコーディオン コントロールは、複数のペインを示し、うち 1 つを同時に表示できます。</span><span class="sxs-lookup"><span data-stu-id="3d14a-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="3d14a-111">パネルがページ自体には、通常は宣言されているが、サーバー側のコードは、同じ結果を実現するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="3d14a-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="3d14a-112">手順</span><span class="sxs-lookup"><span data-stu-id="3d14a-112">Steps</span></span>

<span data-ttu-id="3d14a-113">アコーディオン コントロールは、サーバー側コードをすべての重要なプロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="3d14a-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="3d14a-114">その他のもの、`Panes`プロパティは、アコーディオンを構成するペインのコレクションへのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="3d14a-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="3d14a-115">型のあるすべてのウィンドウ`AccordionPane`します。</span><span class="sxs-lookup"><span data-stu-id="3d14a-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="3d14a-116">このようなウィンドウを作成する単純なしたがって。</span><span class="sxs-lookup"><span data-stu-id="3d14a-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="3d14a-117">`HeaderContainer`プロパティの`AccordionPane`; ウィンドウのヘッダー セクション内の ASP.NET コントロールへのアクセスを提供します、`ContentContainer`プロパティの`AccordionPane`ウィンドウのコンテンツ セクションに対しても同様です。</span><span class="sxs-lookup"><span data-stu-id="3d14a-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="3d14a-118">これにより、ASP.NET コード ウィンドウにコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="3d14a-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="3d14a-119">最後に、pane(s) に追加する必要があります、`Panes`アコーディオンのコレクション。</span><span class="sxs-lookup"><span data-stu-id="3d14a-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="3d14a-120">アコーディオン コントロールに 2 つのペインを追加します。 完全なサーバー側コードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3d14a-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="3d14a-121">唯一の要素がありませんが、ASP.NET の存在に依存する自体、アコーディオン`ScriptManager`コントロール。</span><span class="sxs-lookup"><span data-stu-id="3d14a-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="3d14a-122">例を完了するには、アコーディオン コントロールで参照されている 2 つの CSS クラスは、ブラウザーのスタイル情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="3d14a-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="3d14a-123">[![アコーディオンにデータはサーバー側コードによって動的に追加されました](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3d14a-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="3d14a-124">アコーディオンにデータはサーバー側コードによって動的に追加されました ([フルサイズの画像を表示する をクリックします](dynamically-adding-an-accordion-pane-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="3d14a-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3d14a-125">前へ</span><span class="sxs-lookup"><span data-stu-id="3d14a-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
