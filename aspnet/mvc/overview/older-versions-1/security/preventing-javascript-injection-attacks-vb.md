---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: JavaScript インジェクション攻撃を防ぐ (VB) |Microsoft Docs
author: StephenWalther
description: JavaScript インジェクション攻撃およびクロス サイト スクリプティング攻撃が発生しないようにします。 このチュートリアルでは、Stephen Walther は、方法を簡単に de について説明しています.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: c46b6e1ca13228feb764d9c660ad578576956970
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828234"
---
<a name="preventing-javascript-injection-attacks-vb"></a><span data-ttu-id="fe20c-104">JavaScript インジェクション攻撃を防ぐ (VB)</span><span class="sxs-lookup"><span data-stu-id="fe20c-104">Preventing JavaScript Injection Attacks (VB)</span></span>
====================
<span data-ttu-id="fe20c-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="fe20c-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="fe20c-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="fe20c-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> <span data-ttu-id="fe20c-107">JavaScript インジェクション攻撃およびクロス サイト スクリプティング攻撃が発生しないようにします。</span><span class="sxs-lookup"><span data-stu-id="fe20c-107">Prevent JavaScript Injection Attacks and Cross-Site Scripting Attacks from happening to you.</span></span> <span data-ttu-id="fe20c-108">このチュートリアルでは、Stephen Walther は、これらの種類の HTML コンテンツをエンコードして攻撃を簡単に倒す方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-108">In this tutorial, Stephen Walther explains how you can easily defeat these types of attacks by HTML encoding your content.</span></span>


<span data-ttu-id="fe20c-109">このチュートリアルの目的では、ASP.NET MVC アプリケーションで JavaScript インジェクション攻撃を防止する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-109">The goal of this tutorial is to explain how you can prevent JavaScript injection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="fe20c-110">このチュートリアルでは、web サイト、JavaScript インジェクション攻撃を防御する 2 つの方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-110">This tutorial discusses two approaches to defending your website against a JavaScript injection attack.</span></span> <span data-ttu-id="fe20c-111">表示データをエンコードすることによって JavaScript インジェクション攻撃を回避する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-111">You learn how to prevent JavaScript injection attacks by encoding the data that you display.</span></span> <span data-ttu-id="fe20c-112">そのまま使用するデータをエンコードすることによって JavaScript インジェクション攻撃を回避する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-112">You also learn how to prevent JavaScript injection attacks by encoding the data that you accept.</span></span>

## <a name="what-is-a-javascript-injection-attack"></a><span data-ttu-id="fe20c-113">JavaScript インジェクション攻撃を受けるとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="fe20c-113">What is a JavaScript Injection Attack?</span></span>

<span data-ttu-id="fe20c-114">ユーザー入力をそのまま使用し、ユーザー入力を再表示すると、JavaScript インジェクション攻撃、web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-114">Whenever you accept user input and redisplay the user input, you open your website to JavaScript injection attacks.</span></span> <span data-ttu-id="fe20c-115">JavaScript インジェクション攻撃にさらされる具体的なアプリケーションを調べてみましょう。</span><span class="sxs-lookup"><span data-stu-id="fe20c-115">Let's examine a concrete application that is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="fe20c-116">顧客フィードバック web サイトを作成することを想像してください (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="fe20c-116">Imagine that you have created a customer feedback website (see Figure 1).</span></span> <span data-ttu-id="fe20c-117">Web サイトにアクセスし、製品を使用した経験のフィードバックを入力できます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-117">Customers can visit the website and enter feedback on their experience using your products.</span></span> <span data-ttu-id="fe20c-118">顧客は、ユーザーのフィードバックを送信するときに、フィードバックはフィードバック ページに再表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-118">When a customer submits their feedback, the feedback is redisplayed on the feedback page.</span></span>


<span data-ttu-id="fe20c-119">[![カスタマー フィードバックの web サイト](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fe20c-119">[![Customer Feedback Website](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)</span></span>

<span data-ttu-id="fe20c-120">**図 01**: カスタマー フィードバックの web サイト ([フルサイズの画像を表示する をクリックします](preventing-javascript-injection-attacks-vb/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="fe20c-120">**Figure 01**: Customer Feedback Website ([Click to view full-size image](preventing-javascript-injection-attacks-vb/_static/image3.png))</span></span>


<span data-ttu-id="fe20c-121">顧客フィードバック web サイトを使用して、`controller`リスト 1 でします。</span><span class="sxs-lookup"><span data-stu-id="fe20c-121">The customer feedback website uses the `controller` in Listing 1.</span></span> <span data-ttu-id="fe20c-122">これは、`controller`という名前の 2 つのアクションを含む`Index()`と`Create()`します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-122">This `controller` contains two actions named `Index()` and `Create()`.</span></span>

<span data-ttu-id="fe20c-123">**1 – を一覧表示します。 `HomeController.vb`**</span><span class="sxs-lookup"><span data-stu-id="fe20c-123">**Listing 1 – `HomeController.vb`**</span></span>

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

<span data-ttu-id="fe20c-124">`Index()`メソッドが表示されます、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="fe20c-124">The `Index()` method displays the `Index` view.</span></span> <span data-ttu-id="fe20c-125">このメソッドは以前、お客様のフィードバックにはすべて、 `Index` (LINQ to SQL クエリを使用して) データベースからのフィードバックを取得して表示します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-125">This method passes all of the previous customer feedback to the `Index` view by retrieving the feedback from the database (using a LINQ to SQL query).</span></span>

<span data-ttu-id="fe20c-126">`Create()`メソッドは、新しいフィードバック項目を作成し、それをデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-126">The `Create()` method creates a new Feedback item and adds it to the database.</span></span> <span data-ttu-id="fe20c-127">渡される、顧客がフォームに入力したメッセージ、`Create()`メッセージ パラメーターのメソッド。</span><span class="sxs-lookup"><span data-stu-id="fe20c-127">The message that the customer enters in the form is passed to the `Create()` method in the message parameter.</span></span> <span data-ttu-id="fe20c-128">フィードバック項目が作成され、メッセージは、フィードバック項目に割り当てられる`Message`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="fe20c-128">A Feedback item is created and the message is assigned to the Feedback item's `Message` property.</span></span> <span data-ttu-id="fe20c-129">フィードバック項目が使用してデータベースに送信される、`DataContext.SubmitChanges()`メソッドの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="fe20c-129">The Feedback item is submitted to the database with the `DataContext.SubmitChanges()` method call.</span></span> <span data-ttu-id="fe20c-130">最後に、訪問者にリダイレクトされる、`Index`ビューのすべてのフィードバックが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-130">Finally, the visitor is redirected back to the `Index` view where all of the feedback is displayed.</span></span>

<span data-ttu-id="fe20c-131">`Index`リスト 2 でビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fe20c-131">The `Index` view is contained in Listing 2.</span></span>

<span data-ttu-id="fe20c-132">**2 – を一覧表示します。 `Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="fe20c-132">**Listing 2 – `Index.aspx`**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

<span data-ttu-id="fe20c-133">`Index`ビューでは 2 つのセクション。</span><span class="sxs-lookup"><span data-stu-id="fe20c-133">The `Index` view has two sections.</span></span> <span data-ttu-id="fe20c-134">最上部のセクションには、実際の顧客のフィードバック フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fe20c-134">The top section contains the actual customer feedback form.</span></span> <span data-ttu-id="fe20c-135">下部のセクションには、For が含まれています.ループをすべての以前の顧客フィードバック項目をループし、各フィードバック項目の EntryDate とメッセージのプロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-135">The bottom section contains a For..Each loop that loops through all of the previous customer feedback items and displays the EntryDate and Message properties for each feedback item.</span></span>

<span data-ttu-id="fe20c-136">顧客フィードバック web サイトは、単純な web サイトです。</span><span class="sxs-lookup"><span data-stu-id="fe20c-136">The customer feedback website is a simple website.</span></span> <span data-ttu-id="fe20c-137">残念ながら、web サイトは JavaScript インジェクション攻撃を開いています。</span><span class="sxs-lookup"><span data-stu-id="fe20c-137">Unfortunately, the website is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="fe20c-138">お客様のフィードバック フォームに次のテキストを入力することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="fe20c-138">Imagine that you enter the following text into the customer feedback form:</span></span>

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

<span data-ttu-id="fe20c-139">このテキストは、警告メッセージ ボックスを表示する JavaScript スクリプトを表します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-139">This text represents a JavaScript script that displays an alert message box.</span></span> <span data-ttu-id="fe20c-140">このスクリプトを送信するフィードバックにだれかが後のフォームのメッセージ<em>Boo!</em>すべてのユーザー アクセス、カスタマー フィードバックの web サイト、将来 (図 2 参照) されるたびに表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-140">After someone submits this script into the feedback form, the message <em>Boo!</em>will appear whenever anyone visits the customer feedback website in the future (see Figure 2).</span></span>


<span data-ttu-id="fe20c-141">[![JavaScript インジェクション](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fe20c-141">[![JavaScript Injection](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)</span></span>

<span data-ttu-id="fe20c-142">**図 02**: JavaScript インジェクション ([フルサイズの画像を表示する をクリックします](preventing-javascript-injection-attacks-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="fe20c-142">**Figure 02**: JavaScript Injection ([Click to view full-size image](preventing-javascript-injection-attacks-vb/_static/image6.png))</span></span>


<span data-ttu-id="fe20c-143">ここで、JavaScript インジェクション攻撃への初期応答には、無関心可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fe20c-143">Now, your initial response to JavaScript injection attacks might be apathy.</span></span> <span data-ttu-id="fe20c-144">JavaScript インジェクション攻撃がの型だけであると思うかもしれません*ねらい*攻撃です。</span><span class="sxs-lookup"><span data-stu-id="fe20c-144">You might think that JavaScript injection attacks are simply a type of *defacement* attack.</span></span> <span data-ttu-id="fe20c-145">JavaScript インジェクション攻撃を受けるをコミットすることによってはだれ何か本当に有害なことができますと思われる場合があります。</span><span class="sxs-lookup"><span data-stu-id="fe20c-145">You might believe that no one can do anything truly evil by committing a JavaScript injection attack.</span></span>

<span data-ttu-id="fe20c-146">残念ながら、ハッカーはいくつか実行実際には、web サイトに JavaScript を挿入し、実に悪質なものです。</span><span class="sxs-lookup"><span data-stu-id="fe20c-146">Unfortunately, a hacker can do some really, really evil things by injecting JavaScript into a website.</span></span> <span data-ttu-id="fe20c-147">JavaScript インジェクション攻撃を受けるを使用して、クロスサイト スクリプティング (XSS) 攻撃を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-147">You can use a JavaScript injection attack to perform a Cross-Site Scripting (XSS) attack.</span></span> <span data-ttu-id="fe20c-148">クロスサイト スクリプティング攻撃では、ユーザーの機密情報を盗むし、別の web サイトに情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-148">In a Cross-Site Scripting attack, you steal confidential user information and send the information to another website.</span></span>

<span data-ttu-id="fe20c-149">たとえば、ハッカーは、他のユーザーからのブラウザーの cookie の値を盗み出す JavaScript インジェクション攻撃を受けるを使用できます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-149">For example, a hacker can use a JavaScript injection attack to steal the values of browser cookies from other users.</span></span> <span data-ttu-id="fe20c-150">-パスワード、クレジット_カード番号、または – の社会保障番号などの機密情報がブラウザーの cookie に格納されている場合、ハッカーはこの情報の盗用、JavaScript インジェクション攻撃を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-150">If sensitive information -- such as passwords, credit card numbers, or social security numbers – is stored in the browser cookies, then a hacker can use a JavaScript injection attack to steal this information.</span></span> <span data-ttu-id="fe20c-151">または、ユーザーが JavaScript 攻撃では、侵害された場合のページに含まれる、ハッカーは、挿入された JavaScript を使用して、フォームのデータを取得し、別の web サイトに送信し、フォーム フィールドに機密情報を入力した場合。</span><span class="sxs-lookup"><span data-stu-id="fe20c-151">Or, if a user enters sensitive information in a form field contained in a page that has been compromised with a JavaScript attack, then the hacker can use the injected JavaScript to grab the form data and send it to another website.</span></span>

<span data-ttu-id="fe20c-152">*怖くてお答えください*します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-152">*Please be scared*.</span></span> <span data-ttu-id="fe20c-153">JavaScript インジェクション攻撃を真剣に受け止めてし、ユーザーの機密情報を保護します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-153">Take JavaScript injection attacks seriously and protect your user's confidential information.</span></span> <span data-ttu-id="fe20c-154">次の 2 つのセクションでは、JavaScript インジェクション攻撃から ASP.NET MVC アプリケーションを保護するために使用できる 2 つの手法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-154">In the next two sections, we discuss two techniques that you can use to defend your ASP.NET MVC applications from JavaScript injection attacks.</span></span>

## <a name="approach-1-html-encode-in-the-view"></a><span data-ttu-id="fe20c-155">方法 1: ビュー内の HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="fe20c-155">Approach #1: HTML Encode in the View</span></span>

<span data-ttu-id="fe20c-156">JavaScript インジェクション攻撃を防ぐの簡単な方法では HTML を 1 つは、ビュー内のデータを再表示するときに、web サイトのユーザーが入力した任意のデータをエンコードします。</span><span class="sxs-lookup"><span data-stu-id="fe20c-156">One easy method of preventing JavaScript injection attacks is to HTML encode any data entered by website users when you redisplay the data in a view.</span></span> <span data-ttu-id="fe20c-157">更新された`Index`リスト 3 でビューがこの方法に従います。</span><span class="sxs-lookup"><span data-stu-id="fe20c-157">The updated `Index` view in Listing 3 follows this approach.</span></span>

<span data-ttu-id="fe20c-158">**リスト 3. – `Index.aspx` (HTML でエンコードされた)**</span><span class="sxs-lookup"><span data-stu-id="fe20c-158">**Listing 3 – `Index.aspx` (HTML Encoded)**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

<span data-ttu-id="fe20c-159">注意の値`feedback.Message`html エンコード前に、次のコードで、値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-159">Notice that the value of `feedback.Message` is HTML encoded before the value is displayed with the following code:</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

<span data-ttu-id="fe20c-160">これは平均値を HTML 文字列をエンコードするでしょうか。</span><span class="sxs-lookup"><span data-stu-id="fe20c-160">What does it mean to HTML encode a string?</span></span> <span data-ttu-id="fe20c-161">文字列をエンコードする HTML と危険な文字など`<`と`>`など HTML エンティティ参照に置き換え`&lt;`と`&gt;`します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-161">When you HTML encode a string, dangerous characters such as `<` and `>` are replaced by HTML entity references such as `&lt;` and `&gt;`.</span></span> <span data-ttu-id="fe20c-162">したがって、文字列`<script>alert("Boo!")</script>`html エンコードされる場合に変換を取得`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-162">So when the string `<script>alert("Boo!")</script>` is HTML encoded, it gets converted to `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`.</span></span> <span data-ttu-id="fe20c-163">エンコードされた文字列は、ブラウザーによって解釈される場合は、JavaScript スクリプトとして実行されなくなります。</span><span class="sxs-lookup"><span data-stu-id="fe20c-163">The encoded string no longer executes as a JavaScript script when interpreted by a browser.</span></span> <span data-ttu-id="fe20c-164">代わりに、図 3 害のないページを取得します。</span><span class="sxs-lookup"><span data-stu-id="fe20c-164">Instead, you get the harmless page in Figure 3.</span></span>


<span data-ttu-id="fe20c-165">[![JavaScript の敗北攻撃](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="fe20c-165">[![Defeated JavaScript Attack](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)</span></span>

<span data-ttu-id="fe20c-166">**図 03**: 敗北 JavaScript 攻撃 ([フルサイズの画像を表示する をクリックします](preventing-javascript-injection-attacks-vb/_static/image9.png))。</span><span class="sxs-lookup"><span data-stu-id="fe20c-166">**Figure 03**: Defeated JavaScript Attack ([Click to view full-size image](preventing-javascript-injection-attacks-vb/_static/image9.png))</span></span>


<span data-ttu-id="fe20c-167">インシデントを`Index`リスト 3 での値のみを表示します。`feedback.Message`はエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="fe20c-167">Notice that in the `Index` view in Listing 3 only the value of `feedback.Message` is encoded.</span></span> <span data-ttu-id="fe20c-168">値`feedback.EntryDate`がエンコードされていません。</span><span class="sxs-lookup"><span data-stu-id="fe20c-168">The value of `feedback.EntryDate` is not encoded.</span></span> <span data-ttu-id="fe20c-169">のみ、ユーザーによって入力されたデータをエンコードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe20c-169">You only need to encode data entered by a user.</span></span> <span data-ttu-id="fe20c-170">EntryDate の値が、コント ローラーで生成されたためする必要がありますを HTML をエンコードしません。 この値。</span><span class="sxs-lookup"><span data-stu-id="fe20c-170">Because the value of EntryDate was generated in the controller, you don't need to HTML encode this value.</span></span>

## <a name="approach-2-html-encode-in-the-controller"></a><span data-ttu-id="fe20c-171">方法 2: コント ローラー内の HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="fe20c-171">Approach #2: HTML Encode in the Controller</span></span>

<span data-ttu-id="fe20c-172">HTML のできる HTML ビューでデータを表示すると、データをエンコードではなく、データベースにデータを送信する直前にデータをエンコードします。</span><span class="sxs-lookup"><span data-stu-id="fe20c-172">Instead of HTML encoding data when you display the data in a view, you can HTML encode the data just before you submit the data to the database.</span></span> <span data-ttu-id="fe20c-173">この 2 つ目のアプローチがの場合に行われています、`controller`リスト 4。</span><span class="sxs-lookup"><span data-stu-id="fe20c-173">This second approach is taken in the case of the `controller` in Listing 4.</span></span>

<span data-ttu-id="fe20c-174">**リスト 4 – `HomeController.cs` (HTML でエンコードされた)**</span><span class="sxs-lookup"><span data-stu-id="fe20c-174">**Listing 4 – `HomeController.cs` (HTML Encoded)**</span></span>

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

<span data-ttu-id="fe20c-175">メッセージの値が HTML エンコード値がデータベースに送信される前に、`Create()`アクション。</span><span class="sxs-lookup"><span data-stu-id="fe20c-175">Notice that the value of Message is HTML encoded before the value is submitted to the database within the `Create()` action.</span></span> <span data-ttu-id="fe20c-176">ビューでは、メッセージが再表示されますと、メッセージが HTML エンコードされたメッセージに挿入された任意の JavaScript は実行されません。</span><span class="sxs-lookup"><span data-stu-id="fe20c-176">When the Message is redisplayed in the view, the Message is HTML encoded and any JavaScript injected in the Message is not executed.</span></span>

<span data-ttu-id="fe20c-177">通常、この 2 つ目の方法でこのチュートリアルで説明した最初のアプローチのどちらを優先する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fe20c-177">Typically, you should favor the first approach discussed in this tutorial over this second approach.</span></span> <span data-ttu-id="fe20c-178">この 2 つ目のアプローチの問題は、最終的に HTML エンコードされたデータ、データベース内です。</span><span class="sxs-lookup"><span data-stu-id="fe20c-178">The problem with this second approach is that you end up with HTML encoded data in your database.</span></span> <span data-ttu-id="fe20c-179">つまり、データベースのデータには、奇妙な探し求めている文字ダーティになった。</span><span class="sxs-lookup"><span data-stu-id="fe20c-179">In other words, your database data is dirtied with funny looking characters.</span></span>

<span data-ttu-id="fe20c-180">なぜこれが正しくないでしょうか。</span><span class="sxs-lookup"><span data-stu-id="fe20c-180">Why is this bad?</span></span> <span data-ttu-id="fe20c-181">Web ページ以外の方法で、データベースのデータを表示する必要がある場合は、問題があります。</span><span class="sxs-lookup"><span data-stu-id="fe20c-181">If you ever need to display the database data in something other than a web page, then you will have problems.</span></span> <span data-ttu-id="fe20c-182">たとえば、Windows フォーム アプリケーションで、データを表示することができます簡単になります。</span><span class="sxs-lookup"><span data-stu-id="fe20c-182">For example, you can no longer easily display the data in a Windows Forms application.</span></span>

## <a name="summary"></a><span data-ttu-id="fe20c-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="fe20c-183">Summary</span></span>

<span data-ttu-id="fe20c-184">このチュートリアルの目的は、JavaScript インジェクション攻撃の取引関係に関する恐ろしいでした。</span><span class="sxs-lookup"><span data-stu-id="fe20c-184">The purpose of this tutorial was to scare you about the prospect of a JavaScript injection attack.</span></span> <span data-ttu-id="fe20c-185">このチュートリアルには、JavaScript インジェクション攻撃に対して、ASP.NET MVC アプリケーションを守るための 2 つの方法が説明されている: いずれかの HTML を実行できます送信されたユーザーをエンコードするか、ビュー内のデータは HTML ユーザー送信のエンコード、コント ローラー内のデータ。</span><span class="sxs-lookup"><span data-stu-id="fe20c-185">This tutorial discussed two approaches for defending your ASP.NET MVC applications against JavaScript injection attacks: you can either HTML encode user submitted data in the view or you can HTML encode user submitted data in the controller.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fe20c-186">前へ</span><span class="sxs-lookup"><span data-stu-id="fe20c-186">Previous</span></span>](authenticating-users-with-windows-authentication-vb.md)
