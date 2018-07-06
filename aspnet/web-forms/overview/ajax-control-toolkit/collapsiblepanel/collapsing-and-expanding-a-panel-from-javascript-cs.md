---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: 折りたたみと展開 (c#) の JavaScript からパネル |Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit の CollapsiblePanel コントロール パネルの拡張し、その内容を折りたたむし、展開する機能が提供されますをしています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c7fc2e2f5e4bc74efb7bf22afbc776cd6aee2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813153"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="f6056-103">折りたたみと展開 (c#) の JavaScript からパネル</span><span class="sxs-lookup"><span data-stu-id="f6056-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>
====================
<span data-ttu-id="f6056-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f6056-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f6056-105">[コードのダウンロード](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f6056-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="f6056-106">ASP.NET AJAX Control Toolkit の CollapsiblePanel コントロールはパネルを拡張し、その内容を折りたたむし、もう一度展開する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="f6056-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="f6056-107">これら 2 つのアクションは、カスタム JavaScript コードからトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="f6056-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="f6056-108">概要</span><span class="sxs-lookup"><span data-stu-id="f6056-108">Overview</span></span>

<span data-ttu-id="f6056-109">ASP.NET AJAX Control Toolkit の CollapsiblePanel コントロールはパネルを拡張し、その内容を折りたたむし、もう一度展開する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="f6056-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="f6056-110">これら 2 つのアクションは、カスタム JavaScript コードからトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="f6056-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="f6056-111">手順</span><span class="sxs-lookup"><span data-stu-id="f6056-111">Steps</span></span>

<span data-ttu-id="f6056-112">まず、新しい ASP.NET ページを作成し、含める、`ScriptManager`内、`<form>`要素。</span><span class="sxs-lookup"><span data-stu-id="f6056-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="f6056-113">これは、Control Toolkit に必要な ASP.NET AJAX ライブラリを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="f6056-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="f6056-114">次に、折りたたみ/展開効果がわかるようにテキスト パネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f6056-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="f6056-115">ご覧のように、パネルは、CSS クラスを次に示します (と基本的に背景色とパネルの幅を定義します) を参照します。</span><span class="sxs-lookup"><span data-stu-id="f6056-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="f6056-116">`CollapsiblePanelExtender`コントロールに必要な`TargetControlID`ツールキットは、どのパネルを折りたたむか、要求時に展開を認識できるように属性します。</span><span class="sxs-lookup"><span data-stu-id="f6056-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="f6056-117">残念ながら、エクステンダー現在公開しません特定の API、パネルの展開または折りたたみが一部の文書化されていないメソッドが操作を行います。</span><span class="sxs-lookup"><span data-stu-id="f6056-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="f6056-118">まず、クライアント側の JavaScript を折りたたんだり展開したり、パネルの内容をトリガーし、ページに 3 つの HTML ボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="f6056-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="f6056-119">クライアント側の JavaScript コード (を使い始める`<script type="text/javascript">`)、`$find()`メソッドを使用する必要がありますにアクセスする、`CollapsiblePanelExtender`します。</span><span class="sxs-lookup"><span data-stu-id="f6056-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="f6056-120">`$find("cpe")` 参照を返します。</span><span class="sxs-lookup"><span data-stu-id="f6056-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="f6056-121">そこから、特定のメソッドは、手元のタスクを解決します。</span><span class="sxs-lookup"><span data-stu-id="f6056-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="f6056-122">メソッド (拡大) を開くため、パネルが呼び出されます`_doOpen()`; 次のコードで実装、`doOpen()`最初のボタンがクリックされたときに、関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f6056-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="f6056-123">閉じる、または、パネルを折りたたみ、`_doClose()`メソッドを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f6056-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="f6056-124">したがって、ユーザーは、2 番目のボタンをクリックすると、次の JavaScript コードが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f6056-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="f6056-125">3 番目のボタンは、パネルの状態を切り替えます。 から折りたたまれているを展開すると、その逆。</span><span class="sxs-lookup"><span data-stu-id="f6056-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="f6056-126">`CollapsiblePanelExtender`公開、`toggle()`はまさにメソッド: パネルの状態を反転させます。</span><span class="sxs-lookup"><span data-stu-id="f6056-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="f6056-127">別の方法もあります (によって内部的に使用される、`toggle()`メソッド):`get_Collapsed()`のメソッド、`CollapsiblePanelExtender()`パネルが折りたたまれているかどうかどうかがわかります。</span><span class="sxs-lookup"><span data-stu-id="f6056-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="f6056-128">いずれかを展開し、この関数の戻り値の値に応じて、パネルは、(`_doOpen()`メソッド)、または折りたたまれている (`_doClose()`) メソッド。</span><span class="sxs-lookup"><span data-stu-id="f6056-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


<span data-ttu-id="f6056-129">[![3 番目のボタンは、パネルの状態を変更しますから一番前と展開のために折りたたむ。](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f6056-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="f6056-130">3 番目のボタンは、パネルの状態を変更: から一番前と展開のために折りたたむ ([フルサイズの画像を表示する をクリックします](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="f6056-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f6056-131">次へ</span><span class="sxs-lookup"><span data-stu-id="f6056-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
