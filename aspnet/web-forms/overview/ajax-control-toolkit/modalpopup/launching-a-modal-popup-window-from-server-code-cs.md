---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: サーバー コード (c#) からモーダル ポップアップ ウィンドウの起動 |Microsoft Docs
author: wenz
description: AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 ただし一部のシナリオでは、その t が必要としています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b7b486c4b99e5ddcb9bc244a9c5dcf193d33b696
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815305"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="2bdd9-104">サーバー コード (c#) からモーダル ポップアップ ウィンドウを起動します。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-104">Launching a Modal Popup Window from Server Code (C#)</span></span>
====================
<span data-ttu-id="2bdd9-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2bdd9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2bdd9-106">[コードのダウンロード](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2bdd9-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="2bdd9-107">AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2bdd9-108">ただし一部のシナリオにモーダル ポップアップを開くが、サーバー側でトリガーされる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="2bdd9-109">概要</span><span class="sxs-lookup"><span data-stu-id="2bdd9-109">Overview</span></span>

<span data-ttu-id="2bdd9-110">AJAX Control Toolkit の ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2bdd9-111">ただし一部のシナリオにモーダル ポップアップを開くが、サーバー側でトリガーされる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="2bdd9-112">手順</span><span class="sxs-lookup"><span data-stu-id="2bdd9-112">Steps</span></span>

<span data-ttu-id="2bdd9-113">まず、ModalPopup コントロールの動作方法を示すために、ボタンの ASP.NET web コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="2bdd9-114">内でこのようなボタンを追加、&lt;フォーム&gt;新しいページの要素。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="2bdd9-115">次に、ポップアップを作成するマークアップが必要です。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="2bdd9-116">定義として、`<asp:Panel>`を制御し、ボタン コントロールが含まれているかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="2bdd9-117">ModalPopup コントロールがこのようなボタン閉じるのポップアップを作成するための機能を提供していますそれ以外の場合、消えるようにする簡単な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="2bdd9-118">次のページに ASP.NET AJAX Toolkit の ModalPopup コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="2bdd9-119">ボタン コントロールに読み込み、非表示になります ボタン、および実際のポップアップの ID のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="2bdd9-120">ASP.NET AJAX; に基づいてすべての web ページと同様別のターゲットのブラウザーのために必要な JavaScript ライブラリを読み込むスクリプト マネージャーが必要です。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="2bdd9-121">ブラウザーでの例を実行します。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-121">Run the example in the browser.</span></span> <span data-ttu-id="2bdd9-122">ボタンをクリックすると、モーダル ポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="2bdd9-123">サーバー側コードを使用して同じ効果を実現するために新しいボタンが必要です。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="2bdd9-124">ボタンをクリックしますがポストバックを生成して、実行、ご覧のとおり、`ServerButton_Click()`サーバー上のメソッド。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="2bdd9-125">このメソッドは、JavaScript 関数が呼び出された`launchModal()`実行は正確では、ページが読み込まれると、JavaScript 関数が実行されます。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="2bdd9-126">ジョブの`launchModal()`ModalPopup を表示することです。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="2bdd9-127">`launchModal()`関数が実行されるは、完全な HTML ページが読み込まれるとします。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="2bdd9-128">その瞬間、ただし、ASP.NET AJAX フレームワークが完全にまだ読み込まれていません。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="2bdd9-129">そのため、`launchModal()`関数設定変数を後で、ModalPopup コントロールを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="2bdd9-130">`pageLoad()` JavaScript 関数は、ASP.NET AJAX が完全に読み込まれた後に実行される特殊な関数です。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="2bdd9-131">そのため、ModalPopup コントロールが場合にのみを表示するには、この関数にコードを追加しました`launchModal()`が前に呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="2bdd9-132">`$find()`関数はページ上の名前付き要素を探してでサーバー側の ID をパラメーターとして必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="2bdd9-133">そのため、 `$find("mpe")` ModalPopup コントロールのクライアントの表現を返します。 その`show()`メソッドにより、ポップアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="2bdd9-134">[![モーダル ポップアップを表示するときに、ボタンのいずれかがクリックされます。](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2bdd9-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="2bdd9-135">モーダル ポップアップを表示するときに、ボタンのいずれかがクリックされた ([フルサイズの画像を表示する をクリックします](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="2bdd9-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2bdd9-136">次へ</span><span class="sxs-lookup"><span data-stu-id="2bdd9-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
