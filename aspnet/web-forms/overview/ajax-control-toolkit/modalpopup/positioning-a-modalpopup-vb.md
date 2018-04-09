---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: 配置 ModalPopup (VB) |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、コントロールは提供しません、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d20888674dfedee7a7af85efd8df118c8394c6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="c155c-104">ModalPopup (VB) の配置</span><span class="sxs-lookup"><span data-stu-id="c155c-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="c155c-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c155c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c155c-106">[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c155c-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="c155c-107">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="c155c-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c155c-108">ただし、コントロールでは、ポップアップを配置する組み込みの機能を提供していません。</span><span class="sxs-lookup"><span data-stu-id="c155c-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="c155c-109">概要</span><span class="sxs-lookup"><span data-stu-id="c155c-109">Overview</span></span>

<span data-ttu-id="c155c-110">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="c155c-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c155c-111">ただし、コントロールでは、ポップアップを配置する組み込みの機能を提供していません。</span><span class="sxs-lookup"><span data-stu-id="c155c-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="c155c-112">手順</span><span class="sxs-lookup"><span data-stu-id="c155c-112">Steps</span></span>

<span data-ttu-id="c155c-113">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`です。</span><span class="sxs-lookup"><span data-stu-id="c155c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="c155c-114">コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="c155c-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="c155c-115">次に、モーダル ポップアップとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="c155c-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="c155c-116">ボタンを使用して、ポップアップ ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="c155c-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="c155c-117">ポップアップが表示されるたびに、特定の場所 ページでに配置するものとします。</span><span class="sxs-lookup"><span data-stu-id="c155c-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="c155c-118">このタスクでは、クライアント側 JavaScript 関数が作成されます。</span><span class="sxs-lookup"><span data-stu-id="c155c-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="c155c-119">まず、パネルにアクセスしようとします。</span><span class="sxs-lookup"><span data-stu-id="c155c-119">It first tries to access the panel.</span></span> <span data-ttu-id="c155c-120">成功した場合、パネルの位置は CSS および JavaScript (でポップアップの位置は変更) を使用して設定されます。</span><span class="sxs-lookup"><span data-stu-id="c155c-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="c155c-121">ただし、`ModalPopupExtender`コントロールもしようとする、ポップアップを配置します。</span><span class="sxs-lookup"><span data-stu-id="c155c-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="c155c-122">そのため、JavaScript コード、ポップアップの秒部分の 10 分ごとに繰り返し配置します。</span><span class="sxs-lookup"><span data-stu-id="c155c-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="c155c-123">ご覧の戻り値として、 `setTimeout()` JavaScript メソッドは、グローバル変数に保存します。</span><span class="sxs-lookup"><span data-stu-id="c155c-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="c155c-124">これにより、必要に応じて、ポップアップの繰り返しの配置を停止するを使用して、`clearTimeout()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="c155c-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="c155c-125">今すぐために残されている、必要に応じてこれらの関数を呼び出すブラウザーです。</span><span class="sxs-lookup"><span data-stu-id="c155c-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="c155c-126">`movePanel()`パネルをトリガーするボタンがクリックされたときに、JavaScript 関数を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="c155c-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="c155c-127">および`stopMoving()`関数活躍、ポップアップ画面には、このことができますが閉じられたときにトリガーされる、`ModalPopupExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="c155c-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="c155c-128">[![指定した位置にモーダル ポップアップが表示されます。](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c155c-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="c155c-129">指定した位置にモーダル ポップアップが表示されます ([フルサイズのイメージを表示するをクリックして](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c155c-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c155c-130">前へ</span><span class="sxs-lookup"><span data-stu-id="c155c-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
