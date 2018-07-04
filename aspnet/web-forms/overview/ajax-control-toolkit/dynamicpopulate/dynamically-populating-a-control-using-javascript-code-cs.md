---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: JavaScript コード (c#) を使用してコントロールを動的に作成する |Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、t のターゲット コントロールに、結果の値を入力しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6999dee71966c4362254d9f601e55fdb75d0b292
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394075"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="73110-103">JavaScript コード (c#) を使用してコントロールを動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="73110-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>
====================
<span data-ttu-id="73110-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="73110-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="73110-105">[コードのダウンロード](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="73110-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="73110-106">ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、ページで、ページを更新せず、ターゲット コントロールに、結果の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="73110-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="73110-107">カスタムのクライアント側の JavaScript コードを使用して、カタログの作成をトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="73110-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="73110-108">概要</span><span class="sxs-lookup"><span data-stu-id="73110-108">Overview</span></span>

<span data-ttu-id="73110-109">`DynamicPopulate` ASP.NET AJAX Control Toolkit のコントロールが web サービス (またはページ メソッド) を呼び出し、ターゲット コントロール ページで、ページ更新せずに、結果の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="73110-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="73110-110">カスタムのクライアント側の JavaScript コードを使用して、カタログの作成をトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="73110-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="73110-111">手順</span><span class="sxs-lookup"><span data-stu-id="73110-111">Steps</span></span>

<span data-ttu-id="73110-112">まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります、`DynamicPopulateExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="73110-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="73110-113">Web サービス メソッドを実装する`getDate()`と呼ばれる、文字列型の 1 つの引数が予想される`contextKey`、ので、`DynamicPopulate`コントロールが web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="73110-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="73110-114">コードを次に示します (ファイル`DynamicPopulate.cs.asmx`) 3 つの形式のいずれかで現在の日付を取得します。</span><span class="sxs-lookup"><span data-stu-id="73110-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="73110-115">次の手順では、新しい ASP.NET サイトを作成し、ASP.NET AJAX ScriptManager コントロールに起動します。</span><span class="sxs-lookup"><span data-stu-id="73110-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="73110-116">ラベル コントロールを追加し、(たとえば、同じ名前の HTML コントロールを使用して、または`<asp:Label />`web コントロール)、web サービス呼び出しの結果は後で説明します。</span><span class="sxs-lookup"><span data-stu-id="73110-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="73110-117">次に、含める、`DynamicPopulateExtender`制御し、対象のコントロールがカスタム JavaScript を使用して後で、この実行は作成をトリガーするコントロールの名前ではなく、web サービス情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="73110-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="73110-118">JavaScript の部分するようになりました。</span><span class="sxs-lookup"><span data-stu-id="73110-118">Now to the JavaScript part.</span></span> <span data-ttu-id="73110-119">`$find()` 、ASP.NET AJAX ライブラリで定義されている、関数など、ASP.NET AJAX Control Toolkit のサーバー側のオブジェクトへの参照を返します`DynamicPopulateExtender`します。</span><span class="sxs-lookup"><span data-stu-id="73110-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="73110-120">現在のファイルで`$find("dpe")`ものへの参照を返します`DynamicPopulateExtender`ページ内のコントロール。</span><span class="sxs-lookup"><span data-stu-id="73110-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="73110-121">呼び出されるメソッドを公開`populate()`動的作成プロセスをトリガーします。</span><span class="sxs-lookup"><span data-stu-id="73110-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="73110-122">`populate()`メソッドが 1 つの引数が必要です。 コンテキスト キーへの引数として機能、 `getDate()` web メソッド。</span><span class="sxs-lookup"><span data-stu-id="73110-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="73110-123">たとえば、`$find("dpe").populate("format1")`月-日-年の形式で現在の日付のラベルに表示します。</span><span class="sxs-lookup"><span data-stu-id="73110-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="73110-124">サンプルをさらに柔軟性にするには、するには、ユーザーがいくつかの日付形式の間は選択ようになりました。</span><span class="sxs-lookup"><span data-stu-id="73110-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="73110-125">それぞれのラジオ ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="73110-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="73110-126">1 回オプション ボタンでユーザーが、JavaScript コード動的に設定を選択した日付の形式でラベル。</span><span class="sxs-lookup"><span data-stu-id="73110-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="73110-127">これらのオプション ボタンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="73110-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="73110-128">ラジオ ボタン、JavaScript の式のコンテキスト内でなお`this.value`はまったく同じ情報を使用する場合は、現在のボタンの値を示します、`getDate()`メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="73110-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="73110-129">[![ボタンをクリックして指定された形式で、サーバーから日付を取得します。](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="73110-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="73110-130">ボタンをクリックしますが、指定された形式でサーバーから日付を取得 ([フルサイズの画像を表示する をクリックします](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="73110-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="73110-131">[前へ](dynamically-populating-a-control-cs.md)
> [次へ](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="73110-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
