---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Web サービスのバックエンド (VB) で、数値のアップダウン コントロールを作成する |Microsoft ドキュメント
author: wenz
description: チェック ボックスに値を入力するユーザーを待つ代わりに、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在する場合) はより多くの c を生じる可能性があります.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 690fd89c552407ec5d77419aae2488e4832efe44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="5d128-103">Web サービス バックエンド (VB) と数値のアップダウン コントロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d128-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="5d128-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5d128-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5d128-105">[コードをダウンロードする](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5d128-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="5d128-106">チェック ボックスに値を入力するユーザーを待つ代わりに、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在する場合) を生じる可能性が増えるにつれて快適です。</span><span class="sxs-lookup"><span data-stu-id="5d128-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="5d128-107">既定では、NumericUpDown コントロール常にだけ増加または減少値 1 が web サービスの証明をより柔軟です。</span><span class="sxs-lookup"><span data-stu-id="5d128-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="5d128-108">概要</span><span class="sxs-lookup"><span data-stu-id="5d128-108">Overview</span></span>

<span data-ttu-id="5d128-109">チェック ボックスに値を入力するユーザーを待つ代わりに、数値のアップダウン コントロール (Windows と他のオペレーティング システムに存在する場合) を生じる可能性が増えるにつれて快適です。</span><span class="sxs-lookup"><span data-stu-id="5d128-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="5d128-110">既定では、`NumericUpDown`柔軟性を証明する web サービスが、常にコントロールを拡大または値を 1 ずつ減少します。</span><span class="sxs-lookup"><span data-stu-id="5d128-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="5d128-111">手順</span><span class="sxs-lookup"><span data-stu-id="5d128-111">Steps</span></span>

<span data-ttu-id="5d128-112">ASP.NET AJAX コントロールのツールキットに含まれています、`NumericUpDown`テキスト ボックスに 2 つのボタンが自動的に追加されるエクステンダー: を減らしたりのいずれかの値を増やすことのいずれか。</span><span class="sxs-lookup"><span data-stu-id="5d128-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="5d128-113">ただし、コントロールは、web サービスの呼び出し (またはページのメソッドの呼び出し) をもサポートします。</span><span class="sxs-lookup"><span data-stu-id="5d128-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="5d128-114">ときに、または下のボタンをクリックすると、JavaScript コードは、web サーバーに接続して、あるメソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5d128-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="5d128-115">メソッドのシグネチャは、次のいずれかを示します。</span><span class="sxs-lookup"><span data-stu-id="5d128-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="5d128-116">`current`引数は、現在の値、テキスト ボックスに、`tag`属性のプロパティとして設定できる追加のコンテキスト データ、`NumericUpDown`エクステンダー (ただし、必須ではありません)。</span><span class="sxs-lookup"><span data-stu-id="5d128-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="5d128-117">このサンプルでは、数値のアップダウン コントロールいてはいけないのみを許可する 2 の累乗値: 1、2、4、8、16、32、64、およびなどです。</span><span class="sxs-lookup"><span data-stu-id="5d128-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="5d128-118">したがって、ユーザーが値を大きくときに実行されるメソッドには、倍精度浮動小数点、古い値です。 必要があります。その他のメソッドは、2 つのによって値を分割する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d128-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="5d128-119">したがって、完全な web サービスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5d128-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="5d128-120">最後に、新しい ASP.NET ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="5d128-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="5d128-121">必要がある通常どおり、`ScriptManager`コントロール、`TextBox`コントロールと`NumericUpDownExtender`コントロール。</span><span class="sxs-lookup"><span data-stu-id="5d128-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="5d128-122">後者には web サービス情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d128-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="5d128-123">`ServiceDownMethod` web メソッドまたはメソッドのページを下の名前</span><span class="sxs-lookup"><span data-stu-id="5d128-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="5d128-124">`ServiceDownPath` 下向きサービス メソッドを使用して web サービスへのパスページ メソッドを使用している場合は、省略します。</span><span class="sxs-lookup"><span data-stu-id="5d128-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="5d128-125">`ServiceUpMethod` web メソッドまたはメソッドをページ上の名前</span><span class="sxs-lookup"><span data-stu-id="5d128-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="5d128-126">`ServiceUpPath` サービス メソッドを使用して web サービスへのパスページ メソッドを使用している場合は、省略します。</span><span class="sxs-lookup"><span data-stu-id="5d128-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="5d128-127">ページの完全なマークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5d128-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="5d128-128">ページを実行する場合は、どのようにテキスト ボックスに値常に 2 倍に上のボタンをクリックして、下のボタンをクリックすると、半分になりますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5d128-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="5d128-129">[![2 の累乗の数値のみが表示されます。](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5d128-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="5d128-130">2 の累乗の数値のみが表示されます ([フルサイズのイメージを表示するをクリックして](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5d128-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5d128-131">前へ</span><span class="sxs-lookup"><span data-stu-id="5d128-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
