---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: CascadingDropDown (VB) を使用して、リストを入力 |Microsoft ドキュメント
author: wenz
description: CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e488a30443970d5e2ce825abd96d8e4a027585d1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="0e3be-103">CascadingDropDown (VB) を使用して、リストを入力</span><span class="sxs-lookup"><span data-stu-id="0e3be-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="0e3be-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0e3be-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0e3be-105">[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0e3be-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="0e3be-106">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="0e3be-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="0e3be-107">(たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。解決するために最初のチャレンジが実際にこのコントロールを使用して、ドロップダウン リストです。</span><span class="sxs-lookup"><span data-stu-id="0e3be-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="0e3be-108">概要</span><span class="sxs-lookup"><span data-stu-id="0e3be-108">Overview</span></span>

<span data-ttu-id="0e3be-109">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="0e3be-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="0e3be-110">(たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。解決するために最初のチャレンジが実際にこのコントロールを使用して、ドロップダウン リストです。</span><span class="sxs-lookup"><span data-stu-id="0e3be-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="0e3be-111">手順</span><span class="sxs-lookup"><span data-stu-id="0e3be-111">Steps</span></span>

<span data-ttu-id="0e3be-112">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="0e3be-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="0e3be-113">その後、DropDownList コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="0e3be-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="0e3be-114">この一覧 CascadingDropDown extender が追加されます。</span><span class="sxs-lookup"><span data-stu-id="0e3be-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="0e3be-115">一覧に表示されるエントリの一覧を返すし、web サービスへの非同期要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="0e3be-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="0e3be-116">これを行うには、次 CascadingDropDown 属性を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e3be-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="0e3be-117">`ServicePath`: 一覧のエントリを提供する web サービスの URL</span><span class="sxs-lookup"><span data-stu-id="0e3be-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="0e3be-118">`ServiceMethod`: Web メソッドの一覧のエントリを提供します。</span><span class="sxs-lookup"><span data-stu-id="0e3be-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="0e3be-119">`TargetControlID`。 ドロップダウン リストの ID</span><span class="sxs-lookup"><span data-stu-id="0e3be-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="0e3be-120">`Category`: カテゴリについては、web メソッドが呼び出されたときに送信されます。</span><span class="sxs-lookup"><span data-stu-id="0e3be-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="0e3be-121">`PromptText`: サーバーから非同期的にリストのデータを読み込むときに表示されるテキスト</span><span class="sxs-lookup"><span data-stu-id="0e3be-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="0e3be-122">ここでは、マークアップを`CascadingDropDown`要素。</span><span class="sxs-lookup"><span data-stu-id="0e3be-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="0e3be-123">C# および VB の唯一の違いは、関連付けられている web サービスの名前を示します。</span><span class="sxs-lookup"><span data-stu-id="0e3be-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="0e3be-124">JavaScript コード、 `CascadingDropDown` extender は、次のシグネチャを持つ web サービス メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0e3be-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="0e3be-125">重要な側面は、メソッドの型の配列を返す必要があるように`CascadingDropDownNameValue`(ASP.NET AJAX コントロール Toolkit により定義) です。</span><span class="sxs-lookup"><span data-stu-id="0e3be-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="0e3be-126">`CascadingDropDownNameValue`一覧のエントリのテキストとし、その値を指定する必要があります、最初のコンス トラクターと同様に`<option value="VALUE">NAME</option>`HTML での操作とします。</span><span class="sxs-lookup"><span data-stu-id="0e3be-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="0e3be-127">サンプル データの一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0e3be-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="0e3be-128">ブラウザーでページの読み込みと、次の 3 つの仕入先を格納するリストがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="0e3be-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="0e3be-129">[![一覧が自動的に入力します。](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0e3be-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="0e3be-130">一覧が自動的に入力 ([フルサイズのイメージを表示するをクリックして](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0e3be-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0e3be-131">[前へ](using-auto-postback-with-cascadingdropdown-cs.md)
> [次へ](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0e3be-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
