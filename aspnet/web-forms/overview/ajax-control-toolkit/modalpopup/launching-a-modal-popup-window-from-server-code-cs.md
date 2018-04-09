---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: サーバー コード (c#) からのモーダル ポップアップ ウィンドウを起動 |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオでは、その t が必要としています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 04bb056ee29065a472a70d480568b897a789ae59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="ec16b-104">サーバー コード (c#) からのモーダル ポップアップ ウィンドウを起動します。</span><span class="sxs-lookup"><span data-stu-id="ec16b-104">Launching a Modal Popup Window from Server Code (C#)</span></span>
====================
<span data-ttu-id="ec16b-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ec16b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ec16b-106">[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ec16b-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="ec16b-107">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="ec16b-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ec16b-108">ただし一部のシナリオでは、サーバー側でモーダル ポップアップの開始がトリガーされることが必要です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="ec16b-109">概要</span><span class="sxs-lookup"><span data-stu-id="ec16b-109">Overview</span></span>

<span data-ttu-id="ec16b-110">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="ec16b-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ec16b-111">ただし一部のシナリオでは、サーバー側でモーダル ポップアップの開始がトリガーされることが必要です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="ec16b-112">手順</span><span class="sxs-lookup"><span data-stu-id="ec16b-112">Steps</span></span>

<span data-ttu-id="ec16b-113">まず、ModalPopup コントロールの動作方法を示すために、ASP.NET ボタン web コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="ec16b-114">内でこのようなボタンを追加、&lt;フォーム&gt;の新しいページの要素。</span><span class="sxs-lookup"><span data-stu-id="ec16b-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="ec16b-115">次に、ポップアップを作成するマークアップが必要です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="ec16b-116">として定義、`<asp:Panel>`を制御し、ボタン コントロールが含まれているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="ec16b-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="ec16b-117">ModalPopup コントロールがポップアップ ウィンドウです。 このようなボタン閉じるを作成するための機能を提供していますそれ以外の場合が消えることができるようにする簡単な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="ec16b-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="ec16b-118">次のページに、ASP.NET AJAX Toolkit から ModalPopup コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="ec16b-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="ec16b-119">ボタン コントロールに読み込み、非表示になります ボタン、および実際のポップアップの ID のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="ec16b-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="ec16b-120">ASP.NET AJAX; に基づいてすべての web ページと同様Script Manager が、別のターゲットのブラウザーのために必要な JavaScript ライブラリを読み込む必要です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="ec16b-121">ブラウザーの例を実行します。</span><span class="sxs-lookup"><span data-stu-id="ec16b-121">Run the example in the browser.</span></span> <span data-ttu-id="ec16b-122">ボタンをクリックすると、モーダル ポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ec16b-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="ec16b-123">サーバー側コードを使用して、同じ効果を実現するために、新しいボタンが必要です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="ec16b-124">ボタンをクリックしてがポストバックを生成し、実行が表示されるよう、`ServerButton_Click()`サーバー上のメソッドです。</span><span class="sxs-lookup"><span data-stu-id="ec16b-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="ec16b-125">このメソッドでは、JavaScript 関数が呼び出された`launchModal()`が実行される正確に言うと、ページが読み込まれる JavaScript 関数が実行されます。</span><span class="sxs-lookup"><span data-stu-id="ec16b-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="ec16b-126">ジョブの`launchModal()`ModalPopup を表示することです。</span><span class="sxs-lookup"><span data-stu-id="ec16b-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="ec16b-127">`launchModal()`関数が実行されるは、完全な HTML ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="ec16b-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="ec16b-128">その時点では、ただし、ASP.NET AJAX framework が完全にまだ読み込まれていません。</span><span class="sxs-lookup"><span data-stu-id="ec16b-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="ec16b-129">したがって、`launchModal()`関数だけ ModalPopup コントロールを後で示す必要があります、変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="ec16b-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="ec16b-130">`pageLoad()` JavaScript 関数は、ASP.NET AJAX が完全に読み込まれた後に実行される特殊な関数です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="ec16b-131">そのため、場合に限り、ModalPopup コントロールを表示するには、この関数にコードを追加お`launchModal()`が前に呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="ec16b-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="ec16b-132">`$find()`関数がページ上の名前付きの要素を探して、パラメーターとして、サーバー側で ID が必要です。</span><span class="sxs-lookup"><span data-stu-id="ec16b-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="ec16b-133">したがって、 `$find("mpe")` ModalPopup コントロールのクライアントの表現を返します以外の場合はその`show()`メソッドにより、ポップアップ画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ec16b-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="ec16b-134">[![モーダル ポップアップを表示するにはボタンのいずれかがクリックされました。](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ec16b-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="ec16b-135">モーダル ポップアップを表示するにはボタンのいずれかがクリックされた ([フルサイズのイメージを表示する をクリックします](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="ec16b-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ec16b-136">次へ</span><span class="sxs-lookup"><span data-stu-id="ec16b-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
