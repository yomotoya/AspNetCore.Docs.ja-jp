---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: (C#) コントロールを動的に作成する |Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、t のターゲット コントロールに、結果の値を入力しています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a78e2800b119db61965f9922ba99a2f90e6be948
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827328"
---
<a name="dynamically-populating-a-control-c"></a><span data-ttu-id="fc4c6-103">(C#) コントロールを動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-103">Dynamically Populating a Control (C#)</span></span>
====================
<span data-ttu-id="fc4c6-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fc4c6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fc4c6-105">[コードのダウンロード](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fc4c6-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="fc4c6-106">ASP.NET AJAX Control Toolkit で DynamicPopulate コントロールは、web サービス (またはページ メソッド) を呼び出すし、ページで、ページを更新せず、ターゲット コントロールに、結果の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="fc4c6-107">概要</span><span class="sxs-lookup"><span data-stu-id="fc4c6-107">Overview</span></span>

<span data-ttu-id="fc4c6-108">`DynamicPopulate` ASP.NET AJAX Control Toolkit のコントロールが web サービス (またはページ メソッド) を呼び出し、ターゲット コントロール ページで、ページ更新せずに、結果の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="fc4c6-109">このチュートリアルでは、これを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="fc4c6-110">手順</span><span class="sxs-lookup"><span data-stu-id="fc4c6-110">Steps</span></span>

<span data-ttu-id="fc4c6-111">まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります`DynamicPopulate`します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="fc4c6-112">Web サービス クラスに必要な`ScriptService`内で定義されている属性`Microsoft.Web.Script.Services`。 それ以外の場合、ASP.NET AJAX はにさらに必要な web サービスのクライアント側の JavaScript プロキシを作成することはできません`DynamicPopulate`します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="fc4c6-113">Web メソッドと呼ばれる、文字列型の 1 つの引数を想定する必要があります`contextKey`、ので、`DynamicPopulate`コントロールが web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="fc4c6-114">次の web サービスによって表される形式で現在の日付を返します、`contextKey`引数。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="fc4c6-115">Web サービスとして保存されます`DynamicPopulate.cs.asmx`します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="fc4c6-116">また、実装すること、`getDate()`メソッドでは、実際の ASP.NET ページ内でページ メソッドとして、`DynamicPopulate`コントロール。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="fc4c6-117">次の手順では、新しい ASP.NET ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="fc4c6-118">最初の手順を含めるには常に、よう、 `ScriptManager` Control Toolkit の作業を行うと ASP.NET AJAX ライブラリを読み込むには、現在のページで。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="fc4c6-119">ラベル コントロールを追加し、(たとえば、同じ名前の HTML コントロールを使用して、または&lt; `asp:Label`  / &gt; web コントロール)、web サービス呼び出しの結果は後で説明します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="fc4c6-120">(HTML コントロール、サーバーへのポストバックは必要ありませんので) として、HTML ボタンは、動的な作成をトリガーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="fc4c6-121">最後に、必要があります、`DynamicPopulateExtender`ワイヤ処理を制御します。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="fc4c6-122">次の属性が設定されます (、明らかなものとは別に`ID`と`runat` = `"server"`)。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="fc4c6-123">`TargetControlID` web サービスの呼び出しから結果を格納する場所</span><span class="sxs-lookup"><span data-stu-id="fc4c6-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="fc4c6-124">`ServicePath` web サービスへのパス (ページ メソッドを使用する場合は省略)</span><span class="sxs-lookup"><span data-stu-id="fc4c6-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="fc4c6-125">`ServiceMethod` web メソッドまたはページ メソッドの名前</span><span class="sxs-lookup"><span data-stu-id="fc4c6-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="fc4c6-126">`ContextKey` web サービスに送信されるコンテキスト情報</span><span class="sxs-lookup"><span data-stu-id="fc4c6-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="fc4c6-127">`PopulateTriggerControlID` web サービスの呼び出しをトリガーする要素</span><span class="sxs-lookup"><span data-stu-id="fc4c6-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="fc4c6-128">`ClearContentsDuringUpdate` web サービスの呼び出し中にターゲット要素を空にするかどうか</span><span class="sxs-lookup"><span data-stu-id="fc4c6-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="fc4c6-129">ご覧のように、コントロールには、いくつかの情報が必要なが非常に簡単には所定の位置に配置することで、すべてのもの。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="fc4c6-130">マークアップを次に示します、`DynamicPopulateExtender`現在のシナリオでのコントロール。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="fc4c6-131">ASP.NET ページをブラウザーで実行して、; ボタンをクリックします現在の日付が月-日-年の形式で表示されます。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="fc4c6-132">[![ボタンをクリックして、サーバーから日付を取得します。](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc4c6-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="fc4c6-133">ボタンをクリックしますが、サーバーから日付を取得します ([フルサイズの画像を表示する をクリックします](dynamically-populating-a-control-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="fc4c6-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fc4c6-134">次へ</span><span class="sxs-lookup"><span data-stu-id="fc4c6-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
