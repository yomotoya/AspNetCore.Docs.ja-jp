---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: (C#)、UpdatePanel のポップアップ コントロールからポストバックを処理する |Microsoft Docs
author: wenz
description: AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。 特別な注意は、する必要があります.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f886ba0f3e79bc6d5daf193eaedfd627bc9b937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825930"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="cb93e-104">(C#)、UpdatePanel のポップアップ コントロールからポストバックを処理します。</span><span class="sxs-lookup"><span data-stu-id="cb93e-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="cb93e-105">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="cb93e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="cb93e-106">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="cb93e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="cb93e-107">AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="cb93e-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="cb93e-108">特別な注意は、このようなポップアップ内ポストバックが発生した場合に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cb93e-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="cb93e-109">概要</span><span class="sxs-lookup"><span data-stu-id="cb93e-109">Overview</span></span>

<span data-ttu-id="cb93e-110">AJAX Control Toolkit で PopupControl エクステンダーには、その他のコントロールがアクティブになったときにポップアップをトリガーする簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="cb93e-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="cb93e-111">特別な注意は、このようなポップアップ内ポストバックが発生した場合に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cb93e-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="cb93e-112">手順</span><span class="sxs-lookup"><span data-stu-id="cb93e-112">Steps</span></span>

<span data-ttu-id="cb93e-113">使用する場合、 `PopupControl` 、ポストバックに、`UpdatePanel`ポストバックの原因となったページの更新を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="cb93e-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="cb93e-114">次のマークアップでは、いくつかの重要な要素を定義します。</span><span class="sxs-lookup"><span data-stu-id="cb93e-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="cb93e-115">A `ScriptManager` ASP.NET AJAX Control Toolkit が動作するように制御</span><span class="sxs-lookup"><span data-stu-id="cb93e-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="cb93e-116">2 つ`TextBox`ポップアップをトリガーする両方のコントロール</span><span class="sxs-lookup"><span data-stu-id="cb93e-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="cb93e-117">A`Panel`ポップアップとして機能するコントロール</span><span class="sxs-lookup"><span data-stu-id="cb93e-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="cb93e-118">パネル内で、`Calendar`内でコントロールが埋め込まれている、`UpdatePanel`コントロール</span><span class="sxs-lookup"><span data-stu-id="cb93e-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="cb93e-119">2 つ`PopupControlExtender`コントロール、テキスト ボックスに、パネルを割り当てる</span><span class="sxs-lookup"><span data-stu-id="cb93e-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="cb93e-120">なお、`OnSelectionChanged`の属性、`Calendar`コントロールを設定します。</span><span class="sxs-lookup"><span data-stu-id="cb93e-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="cb93e-121">ポストバックが発生したときに、ユーザーは、カレンダーで日付を選択するようにし、サーバー側メソッド`c1_SelectionChanged()`を実行します。</span><span class="sxs-lookup"><span data-stu-id="cb93e-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="cb93e-122">メソッド内には、現在の日付を取得し、テキスト ボックスに書き戻されるしてする必要があります。</span><span class="sxs-lookup"><span data-stu-id="cb93e-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="cb93e-123">そのための構文は次のように、: すべてのプロキシの最初のオブジェクト、 `PopupControlExtender`  ページを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cb93e-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="cb93e-124">ASP.NET AJAX Control Toolkit の提供、`GetProxyForCurrentPopup()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="cb93e-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="cb93e-125">このメソッドが返すオブジェクトのサポート、`Commit()`メソッド (コントロールではなく、メソッドの呼び出しをトリガーした!) ポップアップをトリガーしたコントロールに値を送信します。</span><span class="sxs-lookup"><span data-stu-id="cb93e-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="cb93e-126">次のコードは、選択した日付を引数として、`Commit()`いるため、テキスト ボックスに、選択した日付を書き戻すコード メソッド。</span><span class="sxs-lookup"><span data-stu-id="cb93e-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="cb93e-127">カレンダーの日付では、関連付けられているテキスト ボックスで、選択した日付が表示されます をクリックするたびに日付の選択コントロールを作成することができます現在入手多くの web サイト。</span><span class="sxs-lookup"><span data-stu-id="cb93e-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="cb93e-128">[![カレンダーは、テキスト ボックスに、ユーザーがクリックしたときに表示されます。](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cb93e-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="cb93e-129">テキスト ボックスに、ユーザーがクリックしたときに、カレンダーが表示されます ([フルサイズの画像を表示する をクリックします](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="cb93e-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="cb93e-130">[![テキスト ボックス内に配置する日付をクリックすると](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="cb93e-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="cb93e-131">テキスト ボックス内に配置する日付をクリックすると ([フルサイズの画像を表示する をクリックします](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="cb93e-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cb93e-132">[前へ](using-multiple-popup-controls-cs.md)
> [次へ](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="cb93e-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
