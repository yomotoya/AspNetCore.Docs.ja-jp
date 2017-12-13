---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: "ユーザー コントロールと JavaScript (VB) DynamicPopulate を使って |Microsoft ドキュメント"
author: wenz
description: "ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、t の対象のコントロールに、結果の値を設定しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 69066edc18b58cc3148a738fe8dd48cb92a84f11
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="d1899-103">ユーザー コントロールおよび JavaScript (VB) で DynamicPopulate の使用</span><span class="sxs-lookup"><span data-stu-id="d1899-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="d1899-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d1899-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d1899-105">[コードをダウンロードする](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1899-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="d1899-106">ASP.NET AJAX コントロールのツールキットで DynamicPopulate コントロールは、メソッドを呼び出す web サービス (ページ) と、ターゲット コントロール ページで、ページの更新せずに、結果の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="d1899-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="d1899-107">カスタムのクライアント側 JavaScript コードを使用してカタログの作成をトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="d1899-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="d1899-108">ただし、extender は、ユーザー コントロールに存在する場合に実行される特別な注意があります。</span><span class="sxs-lookup"><span data-stu-id="d1899-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="d1899-109">概要</span><span class="sxs-lookup"><span data-stu-id="d1899-109">Overview</span></span>

<span data-ttu-id="d1899-110">`DynamicPopulate` ASP.NET AJAX コントロール Toolkit でコントロールのメソッドを呼び出す web サービス (ページ) と [ページの更新なし] ページでターゲット コントロールに、結果の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="d1899-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="d1899-111">カスタムのクライアント側 JavaScript コードを使用してカタログの作成をトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="d1899-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="d1899-112">ただし、extender は、ユーザー コントロールに存在する場合に実行される特別な注意があります。</span><span class="sxs-lookup"><span data-stu-id="d1899-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="d1899-113">手順</span><span class="sxs-lookup"><span data-stu-id="d1899-113">Steps</span></span>

<span data-ttu-id="d1899-114">まず、ASP.NET Web サービスによって呼び出されるメソッドを実装する必要があります、`DynamicPopulateExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="d1899-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="d1899-115">Web サービス メソッドを実装する`getDate()`と呼ばれる、文字列型の引数を 1 つ指定する必要が`contextKey`、ので、`DynamicPopulate`コントロールは、web サービスの呼び出しごとに 2 つのコンテキスト情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="d1899-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="d1899-116">次のコードは (ファイル`DynamicPopulate.vb.asmx`) 3 つの形式のいずれかで現在の日付を取得します。</span><span class="sxs-lookup"><span data-stu-id="d1899-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="d1899-117">次の手順で、新しいユーザー コントロールを作成します (`.ascx`ファイル)、最初の行で、次の宣言を表します。</span><span class="sxs-lookup"><span data-stu-id="d1899-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="d1899-118">A &lt; `label` &gt;サーバーから取得したデータを表示する要素が使用されます。</span><span class="sxs-lookup"><span data-stu-id="d1899-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="d1899-119">また、ユーザー コントロール ファイル内を使用します次の 3 つのオプション ボタン、web サービスによってサポートされる 3 つの可能な日付形式のいずれかを表すごとに 1 つ。</span><span class="sxs-lookup"><span data-stu-id="d1899-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="d1899-120">ラジオ ボタンのいずれかで、ユーザーがクリックすると、ブラウザーは次のような JavaScript コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="d1899-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="d1899-121">このコードにアクセスする、 `DynamicPopulateExtender` (奇妙な ID を心配しないでまだ、これについては、後で) し、データを動的な作成をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="d1899-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="d1899-122">現在のラジオ ボタンのコンテキストで`this.value`はその値を指す`format1`、`format2`または`format3`正確にどのような web メソッドが必要です。</span><span class="sxs-lookup"><span data-stu-id="d1899-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="d1899-123">ユーザー コントロールにまだないですだけが、`DynamicPopulateExtender`オプション ボタン web サービスからへのリンク コントロール。</span><span class="sxs-lookup"><span data-stu-id="d1899-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="d1899-124">コントロールで使用される奇妙な ID を再度確認可能性があります:`mcd1$myDate`の代わりに`myDate`です。</span><span class="sxs-lookup"><span data-stu-id="d1899-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="d1899-125">以前は、JavaScript コード`mcd1_dpe1`にアクセスする、`DynamicPopulateExtender`の代わりに`dpe1`です。この名前付け方法を使用する場合に特別な要件を`DynamicPopulateExtender`ユーザー コントロール内で。</span><span class="sxs-lookup"><span data-stu-id="d1899-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="d1899-126">さらに、正常に動作させる特定の方法でユーザー コントロールを埋め込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1899-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="d1899-127">新しい ASP.NET ページを作成し、実装した、ユーザー コントロールのタグ プレフィックスを登録します。</span><span class="sxs-lookup"><span data-stu-id="d1899-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="d1899-128">次に、ASP.NET AJAX を含めます`ScriptManager`新しいページ上のコントロール。</span><span class="sxs-lookup"><span data-stu-id="d1899-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="d1899-129">最後に、ユーザー コントロールをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d1899-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="d1899-130">のみを設定する必要があるその`ID`属性 (および`runat="server"`もちろん、) も、特定の名前に設定する必要があるが、:`mcd1`ので、これは JavaScript を使用してそれにアクセスするユーザー コントロール内で使用されるプレフィックス。</span><span class="sxs-lookup"><span data-stu-id="d1899-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="d1899-131">以上です。</span><span class="sxs-lookup"><span data-stu-id="d1899-131">And that's it!</span></span> <span data-ttu-id="d1899-132">ページが期待どおりに動作します。 ユーザーがクリックでのラジオ ボタンの、、、Toolkit でコントロールが web サービスを呼び出すと、目的の形式で、現在の日付を表示します。</span><span class="sxs-lookup"><span data-stu-id="d1899-132">The page behaves as expected: A user clicks on on of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="d1899-133">[![ユーザー コントロールでのラジオ ボタンが存在します。](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1899-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="d1899-134">ユーザー コントロールでのラジオ ボタンが存在する ([フルサイズのイメージを表示するをクリックして](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d1899-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d1899-135">前へ</span><span class="sxs-lookup"><span data-stu-id="d1899-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
