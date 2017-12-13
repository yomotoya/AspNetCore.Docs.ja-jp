---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: "サーバー コード (VB) からのモーダル ポップアップ ウィンドウを起動 |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオでは、その t が必要としています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c4bcf3e32b3aa91bb73e01296bc1fc1a2e064711
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="b53a3-104">サーバー コード (VB) からのモーダル ポップアップ ウィンドウを起動します。</span><span class="sxs-lookup"><span data-stu-id="b53a3-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="b53a3-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b53a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b53a3-106">[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b53a3-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="b53a3-107">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="b53a3-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b53a3-108">ただし一部のシナリオでは、サーバー側でモーダル ポップアップの開始がトリガーされることが必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="b53a3-109">概要</span><span class="sxs-lookup"><span data-stu-id="b53a3-109">Overview</span></span>

<span data-ttu-id="b53a3-110">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="b53a3-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b53a3-111">ただし一部のシナリオでは、サーバー側でモーダル ポップアップの開始がトリガーされることが必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="b53a3-112">手順</span><span class="sxs-lookup"><span data-stu-id="b53a3-112">Steps</span></span>

<span data-ttu-id="b53a3-113">まず、ModalPopup コントロールの動作方法を示すために、ASP.NET ボタン web コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="b53a3-114">内でこのようなボタンを追加、&lt;フォーム&gt;の新しいページの要素。</span><span class="sxs-lookup"><span data-stu-id="b53a3-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="b53a3-115">次に、ポップアップを作成するマークアップが必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="b53a3-116">として定義、`<asp:Panel>`を制御し、ボタン コントロールが含まれているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="b53a3-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="b53a3-117">ModalPopup コントロールがポップアップ ウィンドウです。 このようなボタン閉じるを作成するための機能を提供していますそれ以外の場合が消えることができるようにする簡単な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="b53a3-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="b53a3-118">次のページに、ASP.NET AJAX Toolkit から ModalPopup コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="b53a3-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="b53a3-119">ボタン コントロールに読み込み、非表示になります ボタン、および実際のポップアップの ID のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="b53a3-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="b53a3-120">ASP.NET AJAX; に基づいてすべての web ページと同様Script Manager が、別のターゲットのブラウザーのために必要な JavaScript ライブラリを読み込む必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="b53a3-121">ブラウザーの例を実行します。</span><span class="sxs-lookup"><span data-stu-id="b53a3-121">Run the example in the browser.</span></span> <span data-ttu-id="b53a3-122">ボタンをクリックすると、モーダル ポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b53a3-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="b53a3-123">サーバー側コードを使用して、同じ効果を実現するために、新しいボタンが必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="b53a3-124">ボタンをクリックしてがポストバックを生成し、実行が表示されるよう、`ServerButton_Click()`サーバー上のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="b53a3-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="b53a3-125">このメソッドでは、JavaScript 関数が呼び出された`launchModal()`が実行される正確に言うと、ページが読み込まれる JavaScript 関数が実行されます。</span><span class="sxs-lookup"><span data-stu-id="b53a3-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="b53a3-126">ジョブの`launchModal()`ModalPopup を表示することです。</span><span class="sxs-lookup"><span data-stu-id="b53a3-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="b53a3-127">`launchModal()`関数が実行されるは、完全な HTML ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="b53a3-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="b53a3-128">その時点では、ただし、ASP.NET AJAX framework が完全にまだ読み込まれていません。</span><span class="sxs-lookup"><span data-stu-id="b53a3-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="b53a3-129">したがって、`launchModal()`関数だけ ModalPopup コントロールを後で示す必要があります、変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="b53a3-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="b53a3-130">`pageLoad()` JavaScript 関数は、ASP.NET AJAX が完全に読み込まれた後に実行される特殊な関数です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="b53a3-131">そのため、場合に限り、ModalPopup コントロールを表示するには、この関数にコードを追加お`launchModal()`が前に呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="b53a3-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="b53a3-132">`$find()`関数がページ上の名前付きの要素を探して、パラメーターとして、サーバー側で ID が必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a3-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="b53a3-133">したがって、 `$find("mpe")` ModalPopup コントロールのクライアントの表現を返します以外の場合はその`show()`メソッドにより、ポップアップ画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b53a3-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="b53a3-134">[![モーダル ポップアップを表示するにはボタンのいずれかがクリックされました。](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b53a3-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="b53a3-135">モーダル ポップアップを表示するにはボタンのいずれかがクリックされた ([フルサイズのイメージを表示する をクリックします](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="b53a3-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b53a3-136">[前へ](positioning-a-modalpopup-cs.md)
[次へ](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b53a3-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
