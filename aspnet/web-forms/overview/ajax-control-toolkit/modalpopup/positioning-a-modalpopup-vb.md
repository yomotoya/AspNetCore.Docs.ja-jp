---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: ModalPopup の位置 (VB) |Microsoft Docs
author: wenz
description: AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし、制御は提供されませんをしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1578a1bd6d5e3b595eba526552b8723daefb2ba1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387833"
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="f5810-104">ModalPopup の位置 (VB)</span><span class="sxs-lookup"><span data-stu-id="f5810-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="f5810-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f5810-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f5810-106">[コードのダウンロード](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f5810-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="f5810-107">AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="f5810-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f5810-108">ただし、コントロールは、ポップアップを配置する組み込みの機能を提供していません。</span><span class="sxs-lookup"><span data-stu-id="f5810-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="f5810-109">概要</span><span class="sxs-lookup"><span data-stu-id="f5810-109">Overview</span></span>

<span data-ttu-id="f5810-110">AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="f5810-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f5810-111">ただし、コントロールは、ポップアップを配置する組み込みの機能を提供していません。</span><span class="sxs-lookup"><span data-stu-id="f5810-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="f5810-112">手順</span><span class="sxs-lookup"><span data-stu-id="f5810-112">Steps</span></span>

<span data-ttu-id="f5810-113">ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`します。</span><span class="sxs-lookup"><span data-stu-id="f5810-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="f5810-114">コントロールは、ページのどこでも配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="f5810-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="f5810-115">次に、モーダル ポップアップとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="f5810-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="f5810-116">ボタンを使用して、ポップアップを閉じます。</span><span class="sxs-lookup"><span data-stu-id="f5810-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="f5810-117">ポップアップが表示されるたびには、特定の場所 ページでに配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5810-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="f5810-118">このタスクでは、クライアント側の JavaScript 関数が作成されます。</span><span class="sxs-lookup"><span data-stu-id="f5810-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="f5810-119">まず、パネルにアクセスしようとします。</span><span class="sxs-lookup"><span data-stu-id="f5810-119">It first tries to access the panel.</span></span> <span data-ttu-id="f5810-120">成功すると、パネルの位置が CSS および JavaScript (でポップアップの位置は変更) を使用して設定されます。</span><span class="sxs-lookup"><span data-stu-id="f5810-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="f5810-121">ただし、`ModalPopupExtender`コントロールもポップアップの配置を試みます。</span><span class="sxs-lookup"><span data-stu-id="f5810-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="f5810-122">そのため、JavaScript コードは、繰り返しの秒部分の 10 分ごと、ポップアップを配置します。</span><span class="sxs-lookup"><span data-stu-id="f5810-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="f5810-123">ご覧のとおりの戻り値、 `setTimeout()` JavaScript メソッドは、グローバル変数に保存されます。</span><span class="sxs-lookup"><span data-stu-id="f5810-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="f5810-124">これにより、停止、オンデマンドでポップアップの繰り返しの配置を使用して、`clearTimeout()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="f5810-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="f5810-125">残りの作業は適切であれば、これらの関数を呼び出し、ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="f5810-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="f5810-126">`movePanel()`パネルをトリガーするボタンがクリックされたときに、JavaScript 関数を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5810-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="f5810-127">および`stopMoving()`関数には、ポップアップには、このことができますが閉じられたときにトリガー、`ModalPopupExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="f5810-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="f5810-128">[![指定した位置にモーダル ポップアップが表示されます。](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f5810-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="f5810-129">指定した位置にモーダル ポップアップが表示されます ([フルサイズの画像を表示する をクリックします](positioning-a-modalpopup-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="f5810-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f5810-130">前へ</span><span class="sxs-lookup"><span data-stu-id="f5810-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
