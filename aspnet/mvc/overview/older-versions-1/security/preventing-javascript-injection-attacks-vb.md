---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: JavaScript インジェクション攻撃 (VB) |Microsoft ドキュメント
author: StephenWalther
description: JavaScript インジェクション攻撃やクロスサイト スクリプティング攻撃が発生しないようにします。 このチュートリアルでは、Stephen Walther は、する方法に簡単に de について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="preventing-javascript-injection-attacks-vb"></a><span data-ttu-id="bdcab-104">JavaScript インジェクション攻撃 (VB) の防止</span><span class="sxs-lookup"><span data-stu-id="bdcab-104">Preventing JavaScript Injection Attacks (VB)</span></span>
====================
<span data-ttu-id="bdcab-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="bdcab-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="bdcab-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> <span data-ttu-id="bdcab-107">JavaScript インジェクション攻撃やクロスサイト スクリプティング攻撃が発生しないようにします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-107">Prevent JavaScript Injection Attacks and Cross-Site Scripting Attacks from happening to you.</span></span> <span data-ttu-id="bdcab-108">このチュートリアルでは、Stephen Walther は、この種の攻撃、html エンコード、コンテンツを簡単に低下する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-108">In this tutorial, Stephen Walther explains how you can easily defeat these types of attacks by HTML encoding your content.</span></span>


<span data-ttu-id="bdcab-109">このチュートリアルの目的では、ASP.NET MVC アプリケーションの JavaScript インジェクション攻撃を防止する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-109">The goal of this tutorial is to explain how you can prevent JavaScript injection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="bdcab-110">このチュートリアルでは、JavaScript インジェクション攻撃から web サイトを保護することに 2 つの方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-110">This tutorial discusses two approaches to defending your website against a JavaScript injection attack.</span></span> <span data-ttu-id="bdcab-111">表示するデータをエンコードすることによって JavaScript インジェクション攻撃を防止する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-111">You learn how to prevent JavaScript injection attacks by encoding the data that you display.</span></span> <span data-ttu-id="bdcab-112">使用すること、データをエンコードすることによって JavaScript インジェクション攻撃を防止する方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-112">You also learn how to prevent JavaScript injection attacks by encoding the data that you accept.</span></span>

## <a name="what-is-a-javascript-injection-attack"></a><span data-ttu-id="bdcab-113">JavaScript インジェクション攻撃とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="bdcab-113">What is a JavaScript Injection Attack?</span></span>

<span data-ttu-id="bdcab-114">ユーザー入力をそのまま使用し、ユーザー入力を再表示すると、JavaScript インジェクション攻撃に対して、web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-114">Whenever you accept user input and redisplay the user input, you open your website to JavaScript injection attacks.</span></span> <span data-ttu-id="bdcab-115">JavaScript インジェクション攻撃にさらされる具体的なアプリケーションを調べてみましょう。</span><span class="sxs-lookup"><span data-stu-id="bdcab-115">Let's examine a concrete application that is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="bdcab-116">カスタマー フィードバックの web サイトが作成されたことを想像してください (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="bdcab-116">Imagine that you have created a customer feedback website (see Figure 1).</span></span> <span data-ttu-id="bdcab-117">顧客では、web サイトにアクセスでき、製品を使用しての経験に関するフィードバックを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-117">Customers can visit the website and enter feedback on their experience using your products.</span></span> <span data-ttu-id="bdcab-118">お客様のフィードバックを送信すると、フィードバックはフィードバック ページに再表示されます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-118">When a customer submits their feedback, the feedback is redisplayed on the feedback page.</span></span>


<span data-ttu-id="bdcab-119">[![カスタマー フィードバックの web サイト](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bdcab-119">[![Customer Feedback Website](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)</span></span>

<span data-ttu-id="bdcab-120">**図 01**: カスタマー フィードバックの web サイト ([フルサイズのイメージを表示するをクリックして](preventing-javascript-injection-attacks-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bdcab-120">**Figure 01**: Customer Feedback Website ([Click to view full-size image](preventing-javascript-injection-attacks-vb/_static/image3.png))</span></span>


<span data-ttu-id="bdcab-121">カスタマー フィードバックの web サイトを使用して、`controller`リスト 1 にします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-121">The customer feedback website uses the `controller` in Listing 1.</span></span> <span data-ttu-id="bdcab-122">これは、`controller`という 2 つのアクションを含む`Index()`と`Create()`です。</span><span class="sxs-lookup"><span data-stu-id="bdcab-122">This `controller` contains two actions named `Index()` and `Create()`.</span></span>

<span data-ttu-id="bdcab-123">**1 – を一覧表示します。 `HomeController.vb`**</span><span class="sxs-lookup"><span data-stu-id="bdcab-123">**Listing 1 – `HomeController.vb`**</span></span>

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

<span data-ttu-id="bdcab-124">`Index()`メソッドが表示されます、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="bdcab-124">The `Index()` method displays the `Index` view.</span></span> <span data-ttu-id="bdcab-125">このメソッドはすべてに、お客様からのフィードバックの前の`Index`(LINQ to SQL クエリを使用して) データベースからのフィードバックを取得して表示します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-125">This method passes all of the previous customer feedback to the `Index` view by retrieving the feedback from the database (using a LINQ to SQL query).</span></span>

<span data-ttu-id="bdcab-126">`Create()`メソッドは、新しい項目のフィードバックを作成し、データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-126">The `Create()` method creates a new Feedback item and adds it to the database.</span></span> <span data-ttu-id="bdcab-127">顧客がフォームに入力するメッセージが渡される、`Create()`メッセージ パラメーターのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="bdcab-127">The message that the customer enters in the form is passed to the `Create()` method in the message parameter.</span></span> <span data-ttu-id="bdcab-128">フィードバック項目が作成され、メッセージは、フィードバック項目に割り当てられる`Message`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="bdcab-128">A Feedback item is created and the message is assigned to the Feedback item's `Message` property.</span></span> <span data-ttu-id="bdcab-129">フィードバック項目がデータベースに送信される、`DataContext.SubmitChanges()`メソッドの呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="bdcab-129">The Feedback item is submitted to the database with the `DataContext.SubmitChanges()` method call.</span></span> <span data-ttu-id="bdcab-130">訪問者が最後に、リダイレクトは、`Index`ビューのすべてのフィードバックが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-130">Finally, the visitor is redirected back to the `Index` view where all of the feedback is displayed.</span></span>

<span data-ttu-id="bdcab-131">`Index`ビューが一覧表示する 2 に含まれています。</span><span class="sxs-lookup"><span data-stu-id="bdcab-131">The `Index` view is contained in Listing 2.</span></span>

<span data-ttu-id="bdcab-132">**2 – を一覧表示します。 `Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="bdcab-132">**Listing 2 – `Index.aspx`**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

<span data-ttu-id="bdcab-133">`Index`ビューには 2 つのセクションです。</span><span class="sxs-lookup"><span data-stu-id="bdcab-133">The `Index` view has two sections.</span></span> <span data-ttu-id="bdcab-134">上部のセクションには、実際のお客様のフィードバック フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bdcab-134">The top section contains the actual customer feedback form.</span></span> <span data-ttu-id="bdcab-135">下部のセクションには、For が含まれています.すべての以前の顧客からのフィードバック項目をループし、フィードバックの各項目に対して EntryDate とメッセージ プロパティを表示する各ループします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-135">The bottom section contains a For..Each loop that loops through all of the previous customer feedback items and displays the EntryDate and Message properties for each feedback item.</span></span>

<span data-ttu-id="bdcab-136">カスタマー フィードバックの web サイトは、単純な web サイトです。</span><span class="sxs-lookup"><span data-stu-id="bdcab-136">The customer feedback website is a simple website.</span></span> <span data-ttu-id="bdcab-137">残念ながら、web サイトは、JavaScript インジェクション攻撃にさらさです。</span><span class="sxs-lookup"><span data-stu-id="bdcab-137">Unfortunately, the website is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="bdcab-138">顧客フィードバック フォームに次のテキストを入力することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="bdcab-138">Imagine that you enter the following text into the customer feedback form:</span></span>

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

<span data-ttu-id="bdcab-139">このテキストは、警告メッセージ ボックスを表示する JavaScript のスクリプトを表します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-139">This text represents a JavaScript script that displays an alert message box.</span></span> <span data-ttu-id="bdcab-140">このスクリプトを送信するフィードバックにユーザーが後のフォームのメッセージ<em>Boo!</em>すべてのユーザー、顧客フィードバック web サイトにアクセス、将来 (図 2 を参照) するたびに表示されます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-140">After someone submits this script into the feedback form, the message <em>Boo!</em>will appear whenever anyone visits the customer feedback website in the future (see Figure 2).</span></span>


<span data-ttu-id="bdcab-141">[![JavaScript インジェクション](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="bdcab-141">[![JavaScript Injection](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)</span></span>

<span data-ttu-id="bdcab-142">**図 02**: JavaScript インジェクション ([フルサイズのイメージを表示するをクリックして](preventing-javascript-injection-attacks-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bdcab-142">**Figure 02**: JavaScript Injection ([Click to view full-size image](preventing-javascript-injection-attacks-vb/_static/image6.png))</span></span>


<span data-ttu-id="bdcab-143">ここで、JavaScript インジェクション攻撃への初期応答には、apathy 可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bdcab-143">Now, your initial response to JavaScript injection attacks might be apathy.</span></span> <span data-ttu-id="bdcab-144">JavaScript インジェクション攻撃がの型だけであると思われる場合があります*改変*攻撃です。</span><span class="sxs-lookup"><span data-stu-id="bdcab-144">You might think that JavaScript injection attacks are simply a type of *defacement* attack.</span></span> <span data-ttu-id="bdcab-145">いる誰は何でも実行できます evil 本当に JavaScript インジェクション攻撃をコミットすることによってと思われる場合があります。</span><span class="sxs-lookup"><span data-stu-id="bdcab-145">You might believe that no one can do anything truly evil by committing a JavaScript injection attack.</span></span>

<span data-ttu-id="bdcab-146">残念ながら、ハッカー行えるいくつか実際には、web サイトに JavaScript を挿入し、実に悪質なものです。</span><span class="sxs-lookup"><span data-stu-id="bdcab-146">Unfortunately, a hacker can do some really, really evil things by injecting JavaScript into a website.</span></span> <span data-ttu-id="bdcab-147">JavaScript インジェクション攻撃を使用して、クロスサイト スクリプト (XSS) 攻撃を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-147">You can use a JavaScript injection attack to perform a Cross-Site Scripting (XSS) attack.</span></span> <span data-ttu-id="bdcab-148">クロスサイト スクリプティング攻撃では、ユーザーの機密情報を盗むし、別の web サイトに情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-148">In a Cross-Site Scripting attack, you steal confidential user information and send the information to another website.</span></span>

<span data-ttu-id="bdcab-149">たとえば、ハッカーは、他のユーザーからブラウザーの cookie の値を盗むために JavaScript インジェクション攻撃を使用できます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-149">For example, a hacker can use a JavaScript injection attack to steal the values of browser cookies from other users.</span></span> <span data-ttu-id="bdcab-150">-パスワード、クレジット_カード番号、または – の社会保障番号などの機密情報がブラウザーの cookie に格納されている場合、ハッカーはこの情報を盗むために JavaScript インジェクション攻撃を使用できます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-150">If sensitive information -- such as passwords, credit card numbers, or social security numbers – is stored in the browser cookies, then a hacker can use a JavaScript injection attack to steal this information.</span></span> <span data-ttu-id="bdcab-151">または、ユーザーが JavaScript 攻撃では、侵害された場合、ページ内にハッカーは、挿入された JavaScript を使用してフォーム データを取得し、別の web サイトに送信し、フォーム フィールドの機密情報を入力した場合。</span><span class="sxs-lookup"><span data-stu-id="bdcab-151">Or, if a user enters sensitive information in a form field contained in a page that has been compromised with a JavaScript attack, then the hacker can use the injected JavaScript to grab the form data and send it to another website.</span></span>

<span data-ttu-id="bdcab-152">*心配お答えください*です。</span><span class="sxs-lookup"><span data-stu-id="bdcab-152">*Please be scared*.</span></span> <span data-ttu-id="bdcab-153">JavaScript インジェクション攻撃を真剣にして、ユーザーの機密情報を保護します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-153">Take JavaScript injection attacks seriously and protect your user's confidential information.</span></span> <span data-ttu-id="bdcab-154">次の 2 つのセクションでは、ASP.NET MVC アプリケーションの JavaScript インジェクション攻撃を防御するために使用できる 2 つの方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-154">In the next two sections, we discuss two techniques that you can use to defend your ASP.NET MVC applications from JavaScript injection attacks.</span></span>

## <a name="approach-1-html-encode-in-the-view"></a><span data-ttu-id="bdcab-155">アプローチ 1: ビューでの HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-155">Approach #1: HTML Encode in the View</span></span>

<span data-ttu-id="bdcab-156">1 つの JavaScript インジェクション攻撃を回避するという簡単な方法を HTML には、ビュー内のデータを再表示するときに、web サイトのユーザーが入力した任意のデータをエンコードします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-156">One easy method of preventing JavaScript injection attacks is to HTML encode any data entered by website users when you redisplay the data in a view.</span></span> <span data-ttu-id="bdcab-157">更新された`Index`ビューを一覧表示する 3 でこの方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-157">The updated `Index` view in Listing 3 follows this approach.</span></span>

<span data-ttu-id="bdcab-158">**3 – を一覧表示する`Index.aspx`(HTML エンコード)**</span><span class="sxs-lookup"><span data-stu-id="bdcab-158">**Listing 3 – `Index.aspx` (HTML Encoded)**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

<span data-ttu-id="bdcab-159">注意して、値の`feedback.Message`HTML エンコードされる値が次のコードに表示される前に。</span><span class="sxs-lookup"><span data-stu-id="bdcab-159">Notice that the value of `feedback.Message` is HTML encoded before the value is displayed with the following code:</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

<span data-ttu-id="bdcab-160">新機能の平均を HTML エンコード文字列ですか?</span><span class="sxs-lookup"><span data-stu-id="bdcab-160">What does it mean to HTML encode a string?</span></span> <span data-ttu-id="bdcab-161">文字列をエンコードする HTML と危険ななどの文字`<`と`>`など、HTML エンティティの参照に置き換え`&lt;`と`&gt;`です。</span><span class="sxs-lookup"><span data-stu-id="bdcab-161">When you HTML encode a string, dangerous characters such as `<` and `>` are replaced by HTML entity references such as `&lt;` and `&gt;`.</span></span> <span data-ttu-id="bdcab-162">ときに、文字列`<script>alert("Boo!")</script>`html エンコードされる場合に変換を取得`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`です。</span><span class="sxs-lookup"><span data-stu-id="bdcab-162">So when the string `<script>alert("Boo!")</script>` is HTML encoded, it gets converted to `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`.</span></span> <span data-ttu-id="bdcab-163">エンコードされた文字列は、ブラウザーによって解釈される場合は、JavaScript スクリプトとして実行されなくなります。</span><span class="sxs-lookup"><span data-stu-id="bdcab-163">The encoded string no longer executes as a JavaScript script when interpreted by a browser.</span></span> <span data-ttu-id="bdcab-164">代わりに、図 3 に無害なページを取得します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-164">Instead, you get the harmless page in Figure 3.</span></span>


<span data-ttu-id="bdcab-165">[![敗北 JavaScript 攻撃](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="bdcab-165">[![Defeated JavaScript Attack](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)</span></span>

<span data-ttu-id="bdcab-166">**図 03**: 敗北 JavaScript 攻撃 ([フルサイズのイメージを表示するをクリックして](preventing-javascript-injection-attacks-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="bdcab-166">**Figure 03**: Defeated JavaScript Attack ([Click to view full-size image](preventing-javascript-injection-attacks-vb/_static/image9.png))</span></span>


<span data-ttu-id="bdcab-167">注意、`Index`リスト 3 での値のみを表示します。`feedback.Message`はエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-167">Notice that in the `Index` view in Listing 3 only the value of `feedback.Message` is encoded.</span></span> <span data-ttu-id="bdcab-168">値`feedback.EntryDate`がエンコードされていません。</span><span class="sxs-lookup"><span data-stu-id="bdcab-168">The value of `feedback.EntryDate` is not encoded.</span></span> <span data-ttu-id="bdcab-169">のみ、ユーザーが入力したデータをエンコードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="bdcab-169">You only need to encode data entered by a user.</span></span> <span data-ttu-id="bdcab-170">EntryDate の値が生成されるため、コント ローラーで、しない必要がありますを HTML エンコードしてこの値。</span><span class="sxs-lookup"><span data-stu-id="bdcab-170">Because the value of EntryDate was generated in the controller, you don't need to HTML encode this value.</span></span>

## <a name="approach-2-html-encode-in-the-controller"></a><span data-ttu-id="bdcab-171">方法 2: コント ローラー内の HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-171">Approach #2: HTML Encode in the Controller</span></span>

<span data-ttu-id="bdcab-172">HTML のできる HTML ビューで、データを表示するには、データをエンコード、代わりに、データベースへのデータを送信する直前にデータをエンコードします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-172">Instead of HTML encoding data when you display the data in a view, you can HTML encode the data just before you submit the data to the database.</span></span> <span data-ttu-id="bdcab-173">場合、この 2 番目の方法が実行される、 `controller` 4 の一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="bdcab-173">This second approach is taken in the case of the `controller` in Listing 4.</span></span>

<span data-ttu-id="bdcab-174">**4 – を一覧表示する`HomeController.cs`(HTML エンコード)**</span><span class="sxs-lookup"><span data-stu-id="bdcab-174">**Listing 4 – `HomeController.cs` (HTML Encoded)**</span></span>

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

<span data-ttu-id="bdcab-175">メッセージの値が HTML エンコードされる値は内でデータベースに送信する前に、`Create()`アクション。</span><span class="sxs-lookup"><span data-stu-id="bdcab-175">Notice that the value of Message is HTML encoded before the value is submitted to the database within the `Create()` action.</span></span> <span data-ttu-id="bdcab-176">ビューでは、メッセージが再表示されますと、メッセージが HTML でエンコードされたメッセージに挿入された任意の JavaScript は実行されません。</span><span class="sxs-lookup"><span data-stu-id="bdcab-176">When the Message is redisplayed in the view, the Message is HTML encoded and any JavaScript injected in the Message is not executed.</span></span>

<span data-ttu-id="bdcab-177">通常、この 2 番目の方法でこのチュートリアルで説明した最初の方法をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bdcab-177">Typically, you should favor the first approach discussed in this tutorial over this second approach.</span></span> <span data-ttu-id="bdcab-178">この 2 つ目の方法を使った問題は、最終的に HTML エンコードされたデータ、データベース内です。</span><span class="sxs-lookup"><span data-stu-id="bdcab-178">The problem with this second approach is that you end up with HTML encoded data in your database.</span></span> <span data-ttu-id="bdcab-179">つまり、データベースのデータは奇妙な探し求めている文字ダーティになった。</span><span class="sxs-lookup"><span data-stu-id="bdcab-179">In other words, your database data is dirtied with funny looking characters.</span></span>

<span data-ttu-id="bdcab-180">なぜこれが正しくないか。</span><span class="sxs-lookup"><span data-stu-id="bdcab-180">Why is this bad?</span></span> <span data-ttu-id="bdcab-181">Web ページ以外のもので、データベースのデータを表示する必要が生じた場合は、問題があります。</span><span class="sxs-lookup"><span data-stu-id="bdcab-181">If you ever need to display the database data in something other than a web page, then you will have problems.</span></span> <span data-ttu-id="bdcab-182">たとえば、データには、Windows フォーム アプリケーションで不要になった簡単に表示できます。</span><span class="sxs-lookup"><span data-stu-id="bdcab-182">For example, you can no longer easily display the data in a Windows Forms application.</span></span>

## <a name="summary"></a><span data-ttu-id="bdcab-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="bdcab-183">Summary</span></span>

<span data-ttu-id="bdcab-184">このチュートリアルの目的は、JavaScript インジェクション攻撃の見込顧客のコンピューターを保護するにはでした。</span><span class="sxs-lookup"><span data-stu-id="bdcab-184">The purpose of this tutorial was to scare you about the prospect of a JavaScript injection attack.</span></span> <span data-ttu-id="bdcab-185">このチュートリアルには、JavaScript インジェクション攻撃に対して、ASP.NET MVC アプリケーションを守るための 2 つの方法が説明されている: いずれかの HTML を作成することができます送信されたユーザーをエンコードするか、ビュー内のデータを HTML 送信されたユーザーをエンコード コント ローラー内のデータ。</span><span class="sxs-lookup"><span data-stu-id="bdcab-185">This tutorial discussed two approaches for defending your ASP.NET MVC applications against JavaScript injection attacks: you can either HTML encode user submitted data in the view or you can HTML encode user submitted data in the controller.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bdcab-186">前へ</span><span class="sxs-lookup"><span data-stu-id="bdcab-186">Previous</span></span>](authenticating-users-with-windows-authentication-vb.md)
