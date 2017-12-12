---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: "CascadingDropDown (c#) を使用して、リストを入力 |Microsoft ドキュメント"
author: wenz
description: "CascadingDropDown コントロール、AJAX コントロール Toolkit でコントロールを拡張する DropDownList anoth 内の値が 1 つの DropDownList 負荷の変更に関連付けられているようにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: e5e0f11a815632aff9e17dc0f783f7eba2753995
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="e4f31-103">CascadingDropDown (c#) を使用して、リストを入力</span><span class="sxs-lookup"><span data-stu-id="e4f31-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="e4f31-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e4f31-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e4f31-105">[コードをダウンロードする](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e4f31-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="e4f31-106">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="e4f31-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e4f31-107">(たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。解決するために最初のチャレンジが実際にこのコントロールを使用して、ドロップダウン リストです。</span><span class="sxs-lookup"><span data-stu-id="e4f31-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="e4f31-108">概要</span><span class="sxs-lookup"><span data-stu-id="e4f31-108">Overview</span></span>

<span data-ttu-id="e4f31-109">AJAX コントロールのツールキットで CascadingDropDown コントロールは、別の DropDownList の値が 1 つの DropDownList 負荷の変更に関連付けられているように DropDownList コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="e4f31-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e4f31-110">(たとえば、1 つのリストが状態、私たちの一覧を提供し、[次へ] の一覧は状態にある主要都市で埋められます)。解決するために最初のチャレンジが実際にこのコントロールを使用して、ドロップダウン リストです。</span><span class="sxs-lookup"><span data-stu-id="e4f31-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="e4f31-111">手順</span><span class="sxs-lookup"><span data-stu-id="e4f31-111">Steps</span></span>

<span data-ttu-id="e4f31-112">ASP.NET AJAX とコントロール Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールを任意の場所 ページで配置する必要があります (ただし内、`<form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="e4f31-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="e4f31-113">その後、DropDownList コントロールが必要です。</span><span class="sxs-lookup"><span data-stu-id="e4f31-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="e4f31-114">この一覧 CascadingDropDown extender が追加されます。</span><span class="sxs-lookup"><span data-stu-id="e4f31-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="e4f31-115">一覧に表示されるエントリの一覧を返すし、web サービスへの非同期要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e4f31-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="e4f31-116">これを行うには、次 CascadingDropDown 属性を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e4f31-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="e4f31-117">`ServicePath`: 一覧のエントリを提供する web サービスの URL</span><span class="sxs-lookup"><span data-stu-id="e4f31-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="e4f31-118">`ServiceMethod`: Web メソッドの一覧のエントリを提供します。</span><span class="sxs-lookup"><span data-stu-id="e4f31-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="e4f31-119">`TargetControlID`。 ドロップダウン リストの ID</span><span class="sxs-lookup"><span data-stu-id="e4f31-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="e4f31-120">`Category`: カテゴリについては、web メソッドが呼び出されたときに送信されます。</span><span class="sxs-lookup"><span data-stu-id="e4f31-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="e4f31-121">`PromptText`: サーバーから非同期的にリストのデータを読み込むときに表示されるテキスト</span><span class="sxs-lookup"><span data-stu-id="e4f31-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="e4f31-122">ここでは、マークアップを`CascadingDropDown`要素。</span><span class="sxs-lookup"><span data-stu-id="e4f31-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="e4f31-123">C# および VB の唯一の違いは、関連付けられている web サービスの名前を示します。</span><span class="sxs-lookup"><span data-stu-id="e4f31-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="e4f31-124">JavaScript コード、 `CascadingDropDown` extender は、次のシグネチャを持つ web サービス メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4f31-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="e4f31-125">重要な側面は、メソッドの型の配列を返す必要があるように`CascadingDropDownNameValue`(ASP.NET AJAX コントロール Toolkit により定義) です。</span><span class="sxs-lookup"><span data-stu-id="e4f31-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="e4f31-126">`CascadingDropDownNameValue`一覧のエントリのテキストとし、その値を指定する必要があります、最初のコンス トラクターと同様に`<option value="VALUE">NAME</option>`HTML での操作とします。</span><span class="sxs-lookup"><span data-stu-id="e4f31-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="e4f31-127">サンプル データの一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e4f31-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="e4f31-128">ブラウザーでページの読み込みと、次の 3 つの仕入先を格納するリストがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="e4f31-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="e4f31-129">[![一覧が自動的に入力します。](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e4f31-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="e4f31-130">一覧が自動的に入力 ([フルサイズのイメージを表示するをクリックして](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e4f31-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e4f31-131">次へ</span><span class="sxs-lookup"><span data-stu-id="e4f31-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
