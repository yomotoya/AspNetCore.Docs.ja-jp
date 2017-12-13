---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: "コントロール (VB) を動的に作成する |Microsoft ドキュメント"
author: wenz
description: "ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、t の対象のコントロールに、結果の値を設定しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ec0b6d429f3eb4a7243201c2a91adde462cf6345
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="f3727-103">コントロール (VB) を動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="f3727-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="f3727-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f3727-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f3727-105">[コードをダウンロードする](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f3727-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="f3727-106">ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、ターゲット コントロール ページで、ページの更新せずに、結果の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="f3727-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="f3727-107">概要</span><span class="sxs-lookup"><span data-stu-id="f3727-107">Overview</span></span>

<span data-ttu-id="f3727-108">`DynamicPopulate` ASP.NET AJAX コントロール Toolkit でコントロールのメソッドを呼び出す web サービス (ページ) と [ページの更新なし] ページでターゲット コントロールに、結果の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="f3727-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f3727-109">このチュートリアルでは、これを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f3727-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="f3727-110">手順</span><span class="sxs-lookup"><span data-stu-id="f3727-110">Steps</span></span>

<span data-ttu-id="f3727-111">まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります`DynamicPopulate`です。</span><span class="sxs-lookup"><span data-stu-id="f3727-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="f3727-112">Web サービス クラスに必要な`ScriptService`内で定義されている属性`Microsoft.Web.Script.Services`です。 それ以外の場合 ASP.NET AJAX は順番に必要な web サービスのクライアント側の JavaScript プロキシを作成できません`DynamicPopulate`です。</span><span class="sxs-lookup"><span data-stu-id="f3727-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="f3727-113">Web メソッドと呼ばれる、文字列型の 1 つの引数を期待する必要があります`contextKey`、ので、`DynamicPopulate`コントロールは、web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="f3727-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="f3727-114">次の web サービスでは、現在の日付を返しますで表される形式で、`contextKey`引数。</span><span class="sxs-lookup"><span data-stu-id="f3727-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f3727-115">Web サービスとして保存し、`DynamicPopulate.vb.asmx`です。</span><span class="sxs-lookup"><span data-stu-id="f3727-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="f3727-116">また、実装する可能性があります、`getDate()`メソッドを使用して実際の ASP.NET ページ内でページ メソッドとして、`DynamicPopulate`コントロール。</span><span class="sxs-lookup"><span data-stu-id="f3727-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="f3727-117">次の手順では、新しい ASP.NET ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f3727-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="f3727-118">最初の手順を含めるには常に、よう、 `ScriptManager` ASP.NET AJAX ライブラリを読み込むとコントロール Toolkit 動作させるには、現在のページで。</span><span class="sxs-lookup"><span data-stu-id="f3727-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f3727-119">ラベル コントロールを追加し、(たとえば、同じ名前の HTML コントロールを使用して、または&lt; `asp:Label`  / &gt; web コントロール) web サービス呼び出しの結果は後で説明します。</span><span class="sxs-lookup"><span data-stu-id="f3727-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="f3727-120">(、HTML コントロールとして、サーバーへのポストバックが必要ないため)、HTML のボタンは、カタログを動的に作成をトリガーする使用されます。</span><span class="sxs-lookup"><span data-stu-id="f3727-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="f3727-121">最後に、必要があります、`DynamicPopulateExtender`作業のネットワーク上のコントロールです。</span><span class="sxs-lookup"><span data-stu-id="f3727-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="f3727-122">次の属性が設定されます (とは別に、自明`ID`と`runat` = `"server"`)。</span><span class="sxs-lookup"><span data-stu-id="f3727-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="f3727-123">`TargetControlID`web サービスの呼び出しから結果を配置します。</span><span class="sxs-lookup"><span data-stu-id="f3727-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="f3727-124">`ServicePath`web サービスへのパス (ページ メソッドを使用する場合は省略)</span><span class="sxs-lookup"><span data-stu-id="f3727-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="f3727-125">`ServiceMethod`web メソッドまたはページのメソッドの名前</span><span class="sxs-lookup"><span data-stu-id="f3727-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="f3727-126">`ContextKey`web サービスに送信されるコンテキスト情報</span><span class="sxs-lookup"><span data-stu-id="f3727-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="f3727-127">`PopulateTriggerControlID`web サービスの呼び出しをトリガーする要素</span><span class="sxs-lookup"><span data-stu-id="f3727-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="f3727-128">`ClearContentsDuringUpdate`web サービスの呼び出し中にターゲット要素を空にするかどうか</span><span class="sxs-lookup"><span data-stu-id="f3727-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="f3727-129">ご覧のように、コントロールには、いくつかの情報が必要なが所定の位置に配置することで、すべて非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="f3727-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="f3727-130">ここでは、マークアップを`DynamicPopulateExtender`現在のシナリオでの制御。</span><span class="sxs-lookup"><span data-stu-id="f3727-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="f3727-131">ASP.NET ページをブラウザーで実行し、をクリックします。現在の日付が月-日-年の形式で表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3727-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="f3727-132">[![ボタンをクリックして、サーバーから日付を取得します。](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f3727-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f3727-133">ボタンをクリックして、サーバーから日付を取得する ([フルサイズのイメージを表示するをクリックして](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f3727-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f3727-134">[前へ](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[次へ](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f3727-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
