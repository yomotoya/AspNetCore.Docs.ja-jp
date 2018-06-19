---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: UpdatePanel (VB) でのポップアップ コントロールからのポストバックを処理する |Microsoft ドキュメント
author: wenz
description: AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、する必要があります.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 312927e01911ea705d0614eac462cd034820c7b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868478"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="4aa2f-104">UpdatePanel (VB) でのポップアップ コントロールからのポストバックを処理します。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="4aa2f-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4aa2f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4aa2f-106">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4aa2f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="4aa2f-107">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="4aa2f-108">特別な注意は、このようなポップアップでポストバックが発生したときにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="4aa2f-109">概要</span><span class="sxs-lookup"><span data-stu-id="4aa2f-109">Overview</span></span>

<span data-ttu-id="4aa2f-110">AJAX コントロールのツールキットで PopupControl extender には、他のコントロールがアクティブになったときに、ポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="4aa2f-111">特別な注意は、このようなポップアップでポストバックが発生したときにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="4aa2f-112">手順</span><span class="sxs-lookup"><span data-stu-id="4aa2f-112">Steps</span></span>

<span data-ttu-id="4aa2f-113">使用する場合、 `PopupControl` 、ポストバック時に、`UpdatePanel`ポストバックによるページの更新を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="4aa2f-114">次のマークアップでは、いくつかの重要な要素を定義します。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="4aa2f-115">A `ScriptManager` ASP.NET AJAX コントロール Toolkit が動作するように制御</span><span class="sxs-lookup"><span data-stu-id="4aa2f-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="4aa2f-116">2 つ`TextBox`ポップアップがトリガーされます両方を管理します</span><span class="sxs-lookup"><span data-stu-id="4aa2f-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="4aa2f-117">A`Panel`ポップアップ画面として使用するコントロール</span><span class="sxs-lookup"><span data-stu-id="4aa2f-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="4aa2f-118">パネル内で、`Calendar`内でコントロールが埋め込まれている、`UpdatePanel`コントロール</span><span class="sxs-lookup"><span data-stu-id="4aa2f-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="4aa2f-119">2 つ`PopupControlExtender`コントロールのテキスト ボックスに、パネルを割り当てる</span><span class="sxs-lookup"><span data-stu-id="4aa2f-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="4aa2f-120">なお、`OnSelectionChanged`の属性、`Calendar`コントロールが設定されています。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="4aa2f-121">ポストバックが発生した、ユーザーは、カレンダーで日付を選択するとき、およびサーバー側メソッド`c1_SelectionChanged()`を実行します。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="4aa2f-122">このメソッドは、内には、現在の日付を取得し、テキスト ボックスに書き戻さして必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="4aa2f-123">そのための構文は次のように、: まず、プロキシ オブジェクトを`PopupControlExtender` ページを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="4aa2f-124">ASP.NET AJAX コントロール Toolkit には、`GetProxyForCurrentPopup()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="4aa2f-125">このメソッドが返すオブジェクトをサポートしている、`Commit()`ポップアップ (コントロールではなく、メソッドの呼び出しをトリガーした!) をトリガーしたコントロールに値を送信するメソッド。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="4aa2f-126">次のコードは、選択した日付を引数として、`Commit()`メソッド、コードのテキスト ボックスに、選択した日付を書き戻しがします。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="4aa2f-127">今すぐ、関連付けられているテキスト ボックスに、選択した日付が表示されます、カレンダーの日付をクリックするたびに日付の選択コントロールを作成することができます現在に見つかりません多くの web サイト。</span><span class="sxs-lookup"><span data-stu-id="4aa2f-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="4aa2f-128">[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4aa2f-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="4aa2f-129">テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4aa2f-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="4aa2f-130">[![日付をクリックすると、テキスト ボックスに配置します。](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4aa2f-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="4aa2f-131">日付をクリックすると、テキスト ボックスに配置 ([フルサイズのイメージを表示するをクリックして](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4aa2f-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4aa2f-132">[前へ](using-multiple-popup-controls-vb.md)
> [次へ](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4aa2f-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
