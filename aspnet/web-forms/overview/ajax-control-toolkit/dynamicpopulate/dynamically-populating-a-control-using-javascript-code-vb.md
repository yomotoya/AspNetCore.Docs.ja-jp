---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: JavaScript コード (VB) を使用してコントロールを動的に作成する |Microsoft ドキュメント
author: wenz
description: ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、t の対象のコントロールに、結果の値を設定しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 04bbc6fca839c2b1ed5cafd3a4411604b98e187d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="3326d-103">JavaScript コード (VB) を使用してコントロールを動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="3326d-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="3326d-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3326d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3326d-105">[コードをダウンロードする](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3326d-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="3326d-106">ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、ターゲット コントロール ページで、ページの更新せずに、結果の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="3326d-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="3326d-107">カスタムのクライアント側 JavaScript コードを使用してカタログの作成をトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="3326d-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="3326d-108">概要</span><span class="sxs-lookup"><span data-stu-id="3326d-108">Overview</span></span>

<span data-ttu-id="3326d-109">`DynamicPopulate` ASP.NET AJAX コントロール Toolkit でコントロールのメソッドを呼び出す web サービス (ページ) と [ページの更新なし] ページでターゲット コントロールに、結果の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="3326d-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="3326d-110">カスタムのクライアント側 JavaScript コードを使用してカタログの作成をトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="3326d-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="3326d-111">手順</span><span class="sxs-lookup"><span data-stu-id="3326d-111">Steps</span></span>

<span data-ttu-id="3326d-112">まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります、`DynamicPopulateExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="3326d-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="3326d-113">Web サービス メソッドを実装する`getDate()`と呼ばれる、文字列型の引数を 1 つ指定する必要が`contextKey`、ので、`DynamicPopulate`コントロールは、web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="3326d-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="3326d-114">次のコードは (ファイル`DynamicPopulate.vb.asmx`) 3 つの形式のいずれかで現在の日付を取得します。</span><span class="sxs-lookup"><span data-stu-id="3326d-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="3326d-115">次の手順では、新しい ASP.NET サイトを作成し、ASP.NET AJAX ScriptManager コントロールと起動します。</span><span class="sxs-lookup"><span data-stu-id="3326d-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="3326d-116">ラベル コントロールを追加し、(たとえば、同じ名前の HTML コントロールを使用して、または`<asp:Label />`web コントロール) web サービス呼び出しの結果は後で説明します。</span><span class="sxs-lookup"><span data-stu-id="3326d-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="3326d-117">次に、含める、`DynamicPopulateExtender`制御し、web サービス情報、対象のコントロールがカスタム JavaScript を使用して、後で実行されます作成をトリガーするコントロールの名前ではなくを提供します。</span><span class="sxs-lookup"><span data-stu-id="3326d-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="3326d-118">JavaScript の部分するようになりました。</span><span class="sxs-lookup"><span data-stu-id="3326d-118">Now to the JavaScript part.</span></span> <span data-ttu-id="3326d-119">`$find()` ASP.NET AJAX ライブラリによって定義された関数がなど、ASP.NET AJAX コントロール Toolkit のサーバー側オブジェクトへの参照を返します`DynamicPopulateExtender`です。</span><span class="sxs-lookup"><span data-stu-id="3326d-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="3326d-120">現在のファイル、`$find("dpe")`ものへの参照を返します`DynamicPopulateExtender`ページ内のコントロールです。</span><span class="sxs-lookup"><span data-stu-id="3326d-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="3326d-121">呼び出されるメソッドを公開`populate()`動的カタログの作成処理をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="3326d-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="3326d-122">`populate()`メソッドには、1 つの引数が必要です。 コンテキスト キーへの引数となる、 `getDate()` web メソッド。</span><span class="sxs-lookup"><span data-stu-id="3326d-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="3326d-123">たとえば、`$find("dpe").populate("format1")`月-日-年の形式で現在の日付でラベルに表示します。</span><span class="sxs-lookup"><span data-stu-id="3326d-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="3326d-124">サンプルをより柔軟に行うには、いくつかの日付形式の間で、ユーザーが今すぐを選択します。</span><span class="sxs-lookup"><span data-stu-id="3326d-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="3326d-125">それぞれのうち、ラジオ ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3326d-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="3326d-126">1 回クリックするオプション ボタンで、JavaScript コードが動的に追加、選択した日付の書式付きのラベル。</span><span class="sxs-lookup"><span data-stu-id="3326d-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="3326d-127">これらのオプション ボタンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3326d-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="3326d-128">ラジオ ボタン、JavaScript 式のコンテキスト内でなお`this.value`まったく同じ情報である場合は、現在のボタンの値を参照、`getDate()`メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="3326d-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="3326d-129">[![ボタンをクリックしてからサーバーの指定された形式で日付を取得します。](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3326d-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="3326d-130">ボタンをクリックしてからサーバーの指定された形式で日付を取得する ([フルサイズのイメージを表示するをクリックして](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3326d-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3326d-131">[前へ](dynamically-populating-a-control-vb.md)
> [次へ](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3326d-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
