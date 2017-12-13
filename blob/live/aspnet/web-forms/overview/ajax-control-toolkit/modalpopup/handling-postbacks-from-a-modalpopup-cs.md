---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: "処理 (c#)、ModalPopup からのポストバック |Microsoft ドキュメント"
author: wenz
description: "AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。 特別な注意が必要と pos しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 096c32d6004474d3d5952ce12d7349b9ebda6003
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="6f4bf-104">(C#)、ModalPopup からのポストバックを処理します。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="6f4bf-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6f4bf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6f4bf-106">[コードをダウンロードする](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6f4bf-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="6f4bf-107">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="6f4bf-108">特別な注意が必要、ポストバックがからのポップアップで作成されたときにします。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="6f4bf-109">概要</span><span class="sxs-lookup"><span data-stu-id="6f4bf-109">Overview</span></span>

<span data-ttu-id="6f4bf-110">AJAX コントロールのツールキットで ModalPopup コントロールには、クライアント側の手段を使用してモーダル ポップアップを作成する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="6f4bf-111">特別な注意が必要、ポストバックがからのポップアップで作成されたときにします。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="6f4bf-112">手順</span><span class="sxs-lookup"><span data-stu-id="6f4bf-112">Steps</span></span>

<span data-ttu-id="6f4bf-113">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="6f4bf-114">次に、モーダル ポップアップとして機能するパネルを追加します。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="6f4bf-115">ユーザーは、名前と電子メール アドレスを入力できます。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="6f4bf-116">ボタンは、ポップアップを閉じ、情報の保存に使用されます。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="6f4bf-117">なお、`OnClick`属性が設定されているため、このボタンがクリックされたときに、ポストバックが発生します。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="6f4bf-118">ページ自体は、まったく同じ情報の 2 つのラベルで構成されています。 名前と電子メール アドレス。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="6f4bf-119">ボタンを使用して、モーダル ポップアップをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="6f4bf-120">ポップアップを表示するために、追加、`ModalPopupExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="6f4bf-121">設定、`PopupControlID`属性パネルの ID をおよび`TargetControlID`ボタンの ID に。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="6f4bf-122">今すぐされるたびに、`Save`のモーダル ポップアップでボタンがクリックされた、サーバー側`SaveData()`メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="6f4bf-123">データ ストアに入力されたデータを保存する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="6f4bf-124">わかりやすくするため、新しいデータのラベルにだけ出力します。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="6f4bf-125">また、現在の名前と電子メールを使用のモーダル ポップアップで、テキスト ボックス コントロールを入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="6f4bf-126">ただしこれは必要な場合にのみポストバックは発生しません。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="6f4bf-127">ポストバックがある場合、ASP.NET の viewstate 機能は、テキスト ボックスに適切な値を自動的に入力します。</span><span class="sxs-lookup"><span data-stu-id="6f4bf-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="6f4bf-128">[![モーダル ポップアップがポストバックを発生させる](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6f4bf-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="6f4bf-129">モーダル ポップアップがポストバックを発生させる ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6f4bf-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6f4bf-130">[前へ](using-modalpopup-with-a-repeater-control-cs.md)
[次へ](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6f4bf-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
