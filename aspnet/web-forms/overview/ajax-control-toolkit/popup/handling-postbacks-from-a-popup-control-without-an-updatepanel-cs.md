---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: UpdatePanel (c#) なしのポップアップ コントロールからのポストバックを処理する |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 Su でポストバックが発生する.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 59ffa05945289de6e01e2c21dd5a0f82ca1fa374
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879544"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="fdf8c-104">UpdatePanel (c#) なしのポップアップ コントロールからのポストバックを処理します。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="fdf8c-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fdf8c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fdf8c-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fdf8c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="fdf8c-107">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="fdf8c-108">このようなパネルのポストバックが発生するし、ページ上のパネルがあることを決定するパネルがクリックしてされた困難です。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="fdf8c-109">概要</span><span class="sxs-lookup"><span data-stu-id="fdf8c-109">Overview</span></span>

<span data-ttu-id="fdf8c-110">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="fdf8c-111">このようなパネルのポストバックが発生するし、ページ上のパネルがあることを決定するパネルがクリックしてされた困難です。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="fdf8c-112">手順</span><span class="sxs-lookup"><span data-stu-id="fdf8c-112">Steps</span></span>

<span data-ttu-id="fdf8c-113">使用する場合、 `PopupControl` 、ポストバックがしなくても、 `UpdatePanel`  ページで、コントロール ツールキットは提供しませんクライアント要素の順番のポストバックの原因となったポップアップがトリガーを決定する方法です。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="fdf8c-114">ただしトリックを小さなは、このシナリオの回避策を提供します。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="fdf8c-115">まず、基本設定を次に示します: 2 つのテキスト ボックスがどちらも、同じポップアップ カレンダーをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="fdf8c-116">2 つ`PopupControlExtenders`テキスト ボックスとポップアップをまとめます。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="fdf8c-117">非表示のフォーム フィールドを追加するのには、基本的な概念、 &lt; `form` &gt;ポップアップ画面を起動するテキスト ボックスを保持する要素。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="fdf8c-118">ページが読み込まれると、JavaScript コードは両方のテキスト ボックスにイベント ハンドラーを追加します。 その名前が隠しフォーム フィールドに書き込まれるテキスト ボックスに、ユーザーがクリックすると、ときに。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="fdf8c-119">サーバー側のコードでは、非表示フィールドの値を読み取る必要があります。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="fdf8c-120">隠しフォーム フィールドが操作する単純なため、非表示の値を検証するホワイト リストのアプローチが必要です。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="fdf8c-121">正しいテキスト ボックスを特定したら、カレンダーから日付がそこに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="fdf8c-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="fdf8c-122">[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fdf8c-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="fdf8c-123">テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fdf8c-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="fdf8c-124">[![日付をクリックすると、テキスト ボックスに配置します。](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fdf8c-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="fdf8c-125">日付をクリックすると、テキスト ボックスに配置 ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fdf8c-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fdf8c-126">[前へ](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [次へ](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fdf8c-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
