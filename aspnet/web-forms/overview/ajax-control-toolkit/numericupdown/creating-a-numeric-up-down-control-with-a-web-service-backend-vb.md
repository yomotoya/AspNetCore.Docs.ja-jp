---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Web サービス バックエンド (VB) で、数値のアップダウン コントロールを作成する |Microsoft Docs
author: wenz
description: チェック ボックスに値を入力するだけでなく、数値を上げ下げするコントロール (Windows と他のオペレーティング システムに存在) はより多くの c を生じる可能性があります.
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bf69b3e6932df528e8ccd2348ffa6f13132f99f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835274"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="11f8a-103">Web サービス バックエンド (VB) で数値を上げ下げするコントロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="11f8a-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="11f8a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="11f8a-105">[コードのダウンロード](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="11f8a-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="11f8a-106">チェック ボックスに値を入力するだけでなく、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在) がより多く生じる快適です。</span><span class="sxs-lookup"><span data-stu-id="11f8a-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="11f8a-107">既定では、NumericUpDown コントロールは常に増加または値を 1 ずつ減少しますが、web サービスは柔軟性を証明します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="11f8a-108">概要</span><span class="sxs-lookup"><span data-stu-id="11f8a-108">Overview</span></span>

<span data-ttu-id="11f8a-109">チェック ボックスに値を入力するだけでなく、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在) がより多く生じる快適です。</span><span class="sxs-lookup"><span data-stu-id="11f8a-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="11f8a-110">既定で、`NumericUpDown`柔軟性を証明する web サービスが、コントロールが常に増加または値を 1 ずつ減少します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="11f8a-111">手順</span><span class="sxs-lookup"><span data-stu-id="11f8a-111">Steps</span></span>

<span data-ttu-id="11f8a-112">ASP.NET AJAX Control Toolkit に含まれています、`NumericUpDown`テキスト ボックスに、2 つのボタンを自動的に追加するエクステンダー: 1 つずつ減少し、その値を増やすことの 1 つ。</span><span class="sxs-lookup"><span data-stu-id="11f8a-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="11f8a-113">ただし、コントロールは、web サービスの呼び出し (またはページ メソッドの呼び出し) をもサポートします。</span><span class="sxs-lookup"><span data-stu-id="11f8a-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="11f8a-114">たびに、上または下向きの矢印ボタンがクリックすると、JavaScript コードは、web サーバーに接続して、あるメソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="11f8a-115">メソッドのシグネチャは、次のいずれかを示します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="11f8a-116">`current`引数は、テキスト ボックスには、現在の値、`tag`属性は、追加のコンテキスト データのプロパティとして設定できる、`NumericUpDown`エクステンダー (ただしは必要ありません)。</span><span class="sxs-lookup"><span data-stu-id="11f8a-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="11f8a-117">このサンプルでは、数値のアップダウン コントロールものとのみを許可する 2 の累乗値: 1、2、4、8、16、32、64、およびなど。</span><span class="sxs-lookup"><span data-stu-id="11f8a-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="11f8a-118">したがって、ユーザーが値を大きくときに実行されるメソッドには、古い値の倍精度浮動小数点; 必要があります。その他のメソッドは、値を 2 で除算する必要があります。</span><span class="sxs-lookup"><span data-stu-id="11f8a-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="11f8a-119">したがって、完全な web サービスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="11f8a-120">最後に、新しい ASP.NET ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="11f8a-121">通常どおり、必要があります、`ScriptManager`コントロール、`TextBox`コントロールと`NumericUpDownExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="11f8a-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="11f8a-122">後者の場合、web サービスの情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="11f8a-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="11f8a-123">`ServiceDownMethod` web メソッドまたはメソッドのページ、リストの名前</span><span class="sxs-lookup"><span data-stu-id="11f8a-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="11f8a-124">`ServiceDownPath` 下向きのサービス メソッドを使用して web サービスへのパスページ メソッドを使用している場合は、省略します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="11f8a-125">`ServiceUpMethod` web メソッドまたはメソッドがページ上の名前</span><span class="sxs-lookup"><span data-stu-id="11f8a-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="11f8a-126">`ServiceUpPath` サービス メソッドを使用して web サービスへのパスページ メソッドを使用している場合は、省略します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="11f8a-127">ページの完全なマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="11f8a-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="11f8a-128">ページを実行する場合は、どのようにテキスト ボックスに値常に 2 倍に上のボタンをクリックして、下のボタンをクリックするとは半分にするとに注意してください。</span><span class="sxs-lookup"><span data-stu-id="11f8a-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="11f8a-129">[![2 の累乗の数値のみが表示されます。](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11f8a-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="11f8a-130">2 の累乗の数値のみが表示されます ([フルサイズの画像を表示する をクリックします](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="11f8a-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="11f8a-131">前へ</span><span class="sxs-lookup"><span data-stu-id="11f8a-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
