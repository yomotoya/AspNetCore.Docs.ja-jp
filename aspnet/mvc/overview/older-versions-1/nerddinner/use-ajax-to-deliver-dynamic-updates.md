---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: "AJAX を使用して、動的な更新プログラムを配信 |Microsoft ドキュメント"
author: microsoft
description: "RSVP のログイン ユーザーの該当する dinner 詳細内に組み込まれて Ajax ベースのアプローチを使用して、夕食への参加をサポートして手順 10. を実装しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7b75f8c6cf08112eb77d1a9a40222ed1425ef3a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="bf69d-103">AJAX を使用して、動的更新を配信するには</span><span class="sxs-lookup"><span data-stu-id="bf69d-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="bf69d-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bf69d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="bf69d-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="bf69d-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="bf69d-106">これは、無料の手順 10. ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="bf69d-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="bf69d-107">RSVP のログイン ユーザーの dinner の詳細 ページ内に組み込まれて Ajax ベースのアプローチを使用して、夕食への参加の目的をサポートして手順 10. を実装します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="bf69d-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="bf69d-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="bf69d-109">NerdDinner 手順 10: RSVPs を有効にする AJAX を受け入れます</span><span class="sxs-lookup"><span data-stu-id="bf69d-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="bf69d-110">RSVP、夕食への参加の目的にログインしているユーザー向けのサポートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="bf69d-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="bf69d-111">有効にしますこの dinner の詳細 ページ内に組み込まれて AJAX ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="bf69d-112">ユーザーが RSVP'd かどうかを示す</span><span class="sxs-lookup"><span data-stu-id="bf69d-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="bf69d-113">ユーザーがアクセスできる、 */Dinners/詳細/[id*] 特定 dinner に関する詳細を表示する URL。</span><span class="sxs-lookup"><span data-stu-id="bf69d-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="bf69d-114">アクション メソッドが実装されている Details() 次のようにします。</span><span class="sxs-lookup"><span data-stu-id="bf69d-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="bf69d-115">RSVP のサポートを実装する、最初の手順は、Dinner オブジェクト (前に作成した Dinner.cs 部分クラス) 内に"IsUserRegistered(username)"ヘルパー メソッドを追加するされます。</span><span class="sxs-lookup"><span data-stu-id="bf69d-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="bf69d-116">このヘルパー メソッドには、true またはユーザーが現在 Dinner の RSVP'd かどうかに応じて false が返されます。</span><span class="sxs-lookup"><span data-stu-id="bf69d-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="bf69d-117">Details.aspx ビュー テンプレートで、ユーザーが登録されているかどうかを示す、適切なメッセージを表示するにまたはイベントではなく、次のコードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="bf69d-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="bf69d-118">今すぐに登録されている夕食にアクセスするユーザーが表示されますこのメッセージ。</span><span class="sxs-lookup"><span data-stu-id="bf69d-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="bf69d-119">それらが登録されていませんが表示されます Dinner を訪問するときに、メッセージの下。</span><span class="sxs-lookup"><span data-stu-id="bf69d-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="bf69d-120">登録アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="bf69d-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="bf69d-121">今すぐ RSVP な夕食の詳細 ページからユーザーを有効にする必要な機能を追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="bf69d-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="bf69d-122">これを実装する、\Controllers ディレクトリを右クリックして、追加の を選択して、新しい"RSVPController"クラスを作成しますお&gt;コント ローラーのメニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="bf69d-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="bf69d-123">引数として Dinner の id を受け取り、適切な夕食オブジェクト、ログイン ユーザーが、登録したユーザーの一覧で、現在あり場合を確認を取得する新しい RSVPController クラス内で"Register"アクション メソッドを実装しますそれらの RSVP オブジェクトを追加できません。</span><span class="sxs-lookup"><span data-stu-id="bf69d-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="bf69d-124">単純な文字列を返すアクション メソッドの出力としてお方法の上に注意してください。</span><span class="sxs-lookup"><span data-stu-id="bf69d-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="bf69d-125">おでしたが埋め込まれているビュー テンプレート – 内でこのメッセージが小さいためであるために使用 Content() ヘルパー メソッドのコント ローラーの基本クラスと上と同様に、文字列メッセージの戻り値で。</span><span class="sxs-lookup"><span data-stu-id="bf69d-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="bf69d-126">AJAX を使用して RSVPForEvent アクション メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="bf69d-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="bf69d-127">この詳細ビューから登録アクション メソッドを呼び出すに AJAX が使用されます。</span><span class="sxs-lookup"><span data-stu-id="bf69d-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="bf69d-128">この実装は非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="bf69d-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="bf69d-129">まず 2 つのスクリプト ライブラリの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="bf69d-130">最初のライブラリは、コア ASP.NET AJAX クライアント側スクリプト ライブラリを参照します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="bf69d-131">このファイル (圧縮) サイズの約 24 k は、中核となるクライアント側の AJAX 機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bf69d-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="bf69d-132">2 番目のライブラリには、ASP.NET MVC の組み込み AJAX ヘルパー メソッド (間もなくが使用されます) と統合されるユーティリティ関数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bf69d-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="bf69d-133">更新プログラム テンプレート コードの表示「が登録されていないこのイベントの」メッセージ、した出力ではなく代わりにレンダリングのリンクをプッシュされたときになるように追加しましたが、RSVP コント ローラーで、RSVPForEvent アクション メソッドを呼び出す AJAX 呼び出しを実行し、ごことができます。ユーザーを RSVPs:</span><span class="sxs-lookup"><span data-stu-id="bf69d-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="bf69d-134">上記 Ajax.ActionLink() ヘルパー メソッドは ASP.NET MVC に組み込まれ Html.ActionLink() ヘルパー メソッドと似ていますなる標準的なナビゲーションを実行する代わりに、アクション メソッドへの AJAX 呼び出しのリンクがクリックされたときにします。</span><span class="sxs-lookup"><span data-stu-id="bf69d-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="bf69d-135">上記の"RSVP"コント ローラーの「登録」アクション メソッドを呼び出すと、"id"パラメーターとして、DinnerID を渡すことをおはします。</span><span class="sxs-lookup"><span data-stu-id="bf69d-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="bf69d-136">最後の AjaxOptions パラメーターを渡しているは、アクション メソッドから返されるコンテンツを行い、HTML を更新することを示します&lt;div&gt;の id を持つ"rsvpmsg"ページの要素。</span><span class="sxs-lookup"><span data-stu-id="bf69d-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="bf69d-137">およびようになりました、夕食にユーザーが参照するときに登録されていませんまだ、その RSVP へのリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bf69d-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="bf69d-138">RSVP コント ローラーで、登録アクション メソッドへの AJAX 呼び出しを行うします"RSVP このイベントの"リンクをクリックした場合と、完了時に更新されたメッセージが表示されます以下のような。</span><span class="sxs-lookup"><span data-stu-id="bf69d-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="bf69d-139">ネットワーク帯域幅とこの AJAX 呼び出しを行うときに関係するトラフィックは非常に軽量です。</span><span class="sxs-lookup"><span data-stu-id="bf69d-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="bf69d-140">小規模の HTTP POST ネットワーク要求が行われる、ユーザーは、"このイベントの RSVP"リンクをクリックすると、 */Dinners/Register/1* URL をネットワーク上で次のようになります。</span><span class="sxs-lookup"><span data-stu-id="bf69d-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="bf69d-141">単に、登録アクション メソッドからの応答。</span><span class="sxs-lookup"><span data-stu-id="bf69d-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="bf69d-142">この軽量コールは高速であり、でも、低速ネットワーク経由で動作します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="bf69d-143">JQuery アニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="bf69d-144">実装しました AJAX 機能は、正しいで高速に動作します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="bf69d-145">場合があります、起こります速すぎてただし、ことユーザーつかない可能性があります RSVP のリンクが新しいテキストに置き換えられています。</span><span class="sxs-lookup"><span data-stu-id="bf69d-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="bf69d-146">結果を少しより明確に更新メッセージに注目させる単純なアニメーションを追加できます。</span><span class="sxs-lookup"><span data-stu-id="bf69d-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="bf69d-147">既定 ASP.NET MVC プロジェクト テンプレートにはには、jQuery – Microsoft によってサポートされても優れた (と非常によく使用される) のオープン ソースの JavaScript ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bf69d-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="bf69d-148">jQuery では、いくつかの便利な HTML DOM の選択と効果ライブラリを含む機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="bf69d-149">JQuery を使用するには、へのスクリプト参照を最初に追加されます。</span><span class="sxs-lookup"><span data-stu-id="bf69d-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="bf69d-150">スクリプト参照のすべてのページが使用できるように、サイト内でさまざまな場所内での jQuery を使用して行う、ため、Site.master マスター ページ ファイル内を追加します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="bf69d-151">*ヒント: VS 2008 sp1 を JavaScript ファイル (jQuery を含む) の豊富な intellisense サポートを有効にする JavaScript intellisense の修正プログラムがインストールされていることを確認します。ダウンロードすることができます: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="bf69d-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="bf69d-152">多くの場合、JQuery を使用して記述されたコードは、グローバル「$ ()」を使用して、CSS セレクターを使用して 1 つまたは複数の HTML 要素を取得する JavaScript メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bf69d-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="bf69d-153">たとえば、 *$("#rsvpmsg")* rsvpmsg の id を持つ任意の HTML 要素の選択中に*$(".something")* 「何か」CSS ですべての要素を選択クラス名。</span><span class="sxs-lookup"><span data-stu-id="bf69d-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="bf69d-154">「すべてのチェックされているオプション ボタンを返す」などのより高度なクエリを記述することもできます。 のような選択クエリを使用して: *$("の入力 [@typeラジオを =] [@checked]")*です。</span><span class="sxs-lookup"><span data-stu-id="bf69d-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="bf69d-155">要素を選択したら、それらを非表示にするように、アクションを実行するためにメソッドを呼び出すことができます: *$("#rsvpmsg").hide() です。*</span><span class="sxs-lookup"><span data-stu-id="bf69d-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="bf69d-156">RSVP シナリオでは、"rsvpmsg"を選択する"AnimateRSVPMessage"をという名前の単純な JavaScript 関数を定義してあります&lt;div&gt;とテキスト コンテンツのサイズをアニメーション化します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="bf69d-157">コードの下、小さなテキストと開始し、原因、400 ミリ秒の期間の経過と共に増加します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="bf69d-158">おできますし、ワイヤ アップ、Ajax.ActionLink() ヘルパー メソッドにその名前を渡すことによって、AJAX 呼び出しが正常に完了した後に呼び出される JavaScript 関数 (AjaxOptions"OnSuccess"を使用してイベント プロパティ)。</span><span class="sxs-lookup"><span data-stu-id="bf69d-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="bf69d-159">これで、および"RSVP このイベントの"リンクをクリックし、AJAX 呼び出しが正常に完了、内容のメッセージが送信されるときにバックアップがアニメーション化サイズが大きくなります。</span><span class="sxs-lookup"><span data-stu-id="bf69d-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="bf69d-160">「成功」イベントを提供するだけでなくは、AjaxOptions オブジェクトは、(と共に、さまざまな他のプロパティと便利なオプション) を処理できる OnBegin、OnFailure、および OnComplete イベントを表示します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="bf69d-161">RSVP の部分ビューをリファクタリングのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="bf69d-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="bf69d-162">この詳細ビュー テンプレートはどの超過が難しく、少し理解する少し長くなるを開始します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="bf69d-163">コードの読みやすさを向上させるには、終了 RSVP コードの表示、詳細ページ用のすべてをカプセル化する部分ビュー – RSVPStatus.ascx – を作成します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="bf69d-164">\Views\Dinners フォルダーを右クリックし、追加 - この作業を行うことができます&gt;メニュー コマンドを表示します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="bf69d-165">必要がある、厳密に型指定された ViewModel として Dinner オブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="bf69d-166">おできますし、コピー/貼り付け RSVP コンテンツに、Details.aspx ビューからです。</span><span class="sxs-lookup"><span data-stu-id="bf69d-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="bf69d-167">処理が完了したら、みましょうもビューを作成別部分 – EditAndDeleteLinks.ascx - 編集および削除リンク表示コードをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="bf69d-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="bf69d-168">その厳密に型指定された ViewModel として Dinner オブジェクトを取得し、コピー/貼り付けを Details.aspx ビューから編集および削除ロジックは、それを必要もあります。</span><span class="sxs-lookup"><span data-stu-id="bf69d-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="bf69d-169">詳細は表示テンプレートはし、下部にある 2 つの Html.RenderPartial() メソッドの呼び出しを含めるだけ。</span><span class="sxs-lookup"><span data-stu-id="bf69d-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="bf69d-170">これにより、コードの読み取りおよびメンテナンス クリーナーです。</span><span class="sxs-lookup"><span data-stu-id="bf69d-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="bf69d-171">次の手順</span><span class="sxs-lookup"><span data-stu-id="bf69d-171">Next Step</span></span>

<span data-ttu-id="bf69d-172">これで、AJAX を使用して、さらにし、対話型のマッピングのサポートをアプリケーションに追加して方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="bf69d-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bf69d-173">[前へ](secure-applications-using-authentication-and-authorization.md)
[次へ](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="bf69d-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
