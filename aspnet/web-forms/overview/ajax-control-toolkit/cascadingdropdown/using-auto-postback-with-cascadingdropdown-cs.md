---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: CascadingDropDown (c#) での自動ポストバックの使用 |Microsoft ドキュメント
author: wenz
description: CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 04e0914dd1057f9ce490f68ae3fa9c56766beafb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="d1531-103">CascadingDropDown (c#) での自動ポストバックの使用</span><span class="sxs-lookup"><span data-stu-id="d1531-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="d1531-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d1531-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d1531-105">[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1531-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="d1531-106">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="d1531-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="d1531-107">ただし、ASP CascadingDropDown コントロールを使用する場合。NET の DropDownList のコントロールの AutoPostBack 機能は動作しません、自体 (不要な) のポストバックを生成、一覧に非同期的にデータを読み込むためです。</span><span class="sxs-lookup"><span data-stu-id="d1531-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="d1531-108">一部の JavaScript コードでは、この特殊効果を回避できます。</span><span class="sxs-lookup"><span data-stu-id="d1531-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="d1531-109">概要</span><span class="sxs-lookup"><span data-stu-id="d1531-109">Overview</span></span>

<span data-ttu-id="d1531-110">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="d1531-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="d1531-111">(たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。ただし、ASP CascadingDropDown コントロールを使用する場合。NET の DropDownList のコントロールの AutoPostBack 機能は動作しません、自体 (不要な) のポストバックを生成、一覧に非同期的にデータを読み込むためです。</span><span class="sxs-lookup"><span data-stu-id="d1531-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="d1531-112">一部の JavaScript コードでは、この特殊効果を回避できます。</span><span class="sxs-lookup"><span data-stu-id="d1531-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="d1531-113">手順</span><span class="sxs-lookup"><span data-stu-id="d1531-113">Steps</span></span>

<span data-ttu-id="d1531-114">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、 &lt; `form` &gt;要素)。</span><span class="sxs-lookup"><span data-stu-id="d1531-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="d1531-115">その後、DropDownList コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="d1531-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="d1531-116">このリストの CascadingDropDown extender が追加され、web サービスの URL とメソッドの情報。</span><span class="sxs-lookup"><span data-stu-id="d1531-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="d1531-117">CascadingDropDown extender し、非同期的に呼び出します次のメソッド シグネチャを持つ web サービス。</span><span class="sxs-lookup"><span data-stu-id="d1531-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="d1531-118">メソッドは、型 CascadingDropDown 値の配列を返します。</span><span class="sxs-lookup"><span data-stu-id="d1531-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="d1531-119">リスト項目のキャプションとし、値型のコンス トラクターが最初に必要です (HTML`value`属性)。</span><span class="sxs-lookup"><span data-stu-id="d1531-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="d1531-120">ブラウザーでページの読み込みがいっぱいに 3 つのベンダーと、ドロップダウン リスト、もう 1 つが既に選択されています。</span><span class="sxs-lookup"><span data-stu-id="d1531-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="d1531-121">また、ASP.NET、定義、 `__doPostBack()` JavaScript メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d1531-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="d1531-122">ページが読み込まれると、この JavaScript の呼び出しがのみ内にある要素には、ドロップダウン リストに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d1531-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="d1531-123">リスト内の要素がない場合、コントロール ツールキットが現在を読み込んで、JavaScript コードが、タイムアウト時間を使用し、0.5 秒でもう一度試みます。</span><span class="sxs-lookup"><span data-stu-id="d1531-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="d1531-124">これにより、ポストバックは、一覧内に実際に要素が、ユーザーはエントリを選択するときにのみ実行されます。</span><span class="sxs-lookup"><span data-stu-id="d1531-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="d1531-125">[![ポストバック リスト要素を選択すると、します。](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1531-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="d1531-126">ポストバック リスト要素を選択すると、([フルサイズのイメージを表示するをクリックして](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d1531-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1531-127">[前へ](presetting-list-entries-with-cascadingdropdown-cs.md)
> [次へ](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d1531-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
