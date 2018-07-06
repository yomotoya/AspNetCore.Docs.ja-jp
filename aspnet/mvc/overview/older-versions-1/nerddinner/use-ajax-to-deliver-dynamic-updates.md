---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: AJAX を使用して、動的更新を配信する |Microsoft Docs
author: microsoft
description: 手順 10 の実装は、RSVP にログインしているユーザーの dinner 詳細内で統合された Ajax ベースのアプローチを使用して、夕食に参加している関心をサポート.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825202"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="dcc73-103">AJAX を使用して、動的更新を配信するには</span><span class="sxs-lookup"><span data-stu-id="dcc73-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="dcc73-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="dcc73-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="dcc73-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="dcc73-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="dcc73-106">これは、無料の手順 10 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="dcc73-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="dcc73-107">手順 10 の実装では、dinner の詳細 ページ内で統合された Ajax ベースのアプローチを使用して、夕食に参加している関心 RSVP にログインしているユーザーのサポートします。</span><span class="sxs-lookup"><span data-stu-id="dcc73-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="dcc73-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="dcc73-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="dcc73-109">NerdDinner 手順 10: AJAX 予約の有効化を受け入れる</span><span class="sxs-lookup"><span data-stu-id="dcc73-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="dcc73-110">今すぐ RSVP を夕食に参加している関心にログインしているユーザー向けのサポートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="dcc73-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="dcc73-111">有効にしますこの dinner の詳細 ページ内で統合された AJAX ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="dcc73-112">ユーザーに対する RSVP があるかどうかを示す</span><span class="sxs-lookup"><span data-stu-id="dcc73-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="dcc73-113">ユーザーがアクセスできる、 */Dinners/詳細/[id*] 特定の dinner に関する詳細を表示する URL:</span><span class="sxs-lookup"><span data-stu-id="dcc73-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="dcc73-114">アクション メソッドが実装されている Details() ようになります。</span><span class="sxs-lookup"><span data-stu-id="dcc73-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="dcc73-115">Dinner オブジェクト (前に作成した Dinner.cs 部分クラス) 内"IsUserRegistered(username)"ヘルパー メソッドを追加する RSVP のサポートを実装する、最初のステップになります。</span><span class="sxs-lookup"><span data-stu-id="dcc73-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="dcc73-116">このヘルパー メソッドは、true または false かどうか、ユーザーが現在に対する RSVP、夕食によって返されます。</span><span class="sxs-lookup"><span data-stu-id="dcc73-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="dcc73-117">次のコードまたはイベントではなく、ユーザーが登録されているかどうかを示す適切なメッセージを表示する、Details.aspx ビュー テンプレートに追加できます。</span><span class="sxs-lookup"><span data-stu-id="dcc73-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="dcc73-118">これでユーザーに登録されている夕食にアクセスしたとき、メッセージが表示されますこの。</span><span class="sxs-lookup"><span data-stu-id="dcc73-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="dcc73-119">見られるは登録されません夕食を訪問するときに、次のメッセージ。</span><span class="sxs-lookup"><span data-stu-id="dcc73-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="dcc73-120">登録アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="dcc73-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="dcc73-121">今すぐ RSVP を夕食の詳細ページからユーザーを有効にするために必要な機能を追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="dcc73-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="dcc73-122">を実装する、\Controllers ディレクトリを右クリックし、[追加]-選択して、新しい"RSVPController"クラスを作成します、&gt;コント ローラーのメニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="dcc73-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="dcc73-123">引数としての Dinner の id を受け取り、適切な Dinner オブジェクトの場合とでは、ログインのユーザーが現在、登録したユーザーの一覧である場合にチェックを取得する新しい RSVPController クラス内での"Register"アクション メソッドを実装しますそれらの RSVP オブジェクトを追加できません。</span><span class="sxs-lookup"><span data-stu-id="dcc73-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="dcc73-124">上記のアクション メソッドの出力として、単純な文字列を返す方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dcc73-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="dcc73-125">ビュー テンプレート – 内でこのメッセージを埋め込むことでしたが、コント ローラーの基本クラスを戻り値の上などの文字列メッセージ Content() ヘルパー メソッドを使用しますだけは非常に小さいため。</span><span class="sxs-lookup"><span data-stu-id="dcc73-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="dcc73-126">AJAX を使用して RSVPForEvent アクション メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="dcc73-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="dcc73-127">AJAX、詳細ビューから登録アクション メソッドの呼び出しに使用します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="dcc73-128">この実装は非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="dcc73-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="dcc73-129">最初に、2 つのスクリプト ライブラリの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="dcc73-130">最初のライブラリは、core の ASP.NET AJAX クライアント側のスクリプト ライブラリを参照します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="dcc73-131">このファイルは、約 24 k (圧縮) サイズでは、中核となるクライアント側の AJAX 機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dcc73-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="dcc73-132">2 番目のライブラリには、ASP.NET MVC の組み込み AJAX ヘルパー メソッド (これはまもなく使用します) と統合するユーティリティ関数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dcc73-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="dcc73-133">更新プログラム ビュー テンプレート コードが表示されるように「が登録されていないこのイベントの」メッセージを出力時ではなく代わりにリンクするプッシュされるときに追加しましたが、RSVP コント ローラーで、RSVPForEvent アクション メソッドを呼び出す AJAX 呼び出しを実行してからことができます。ユーザーを RSVPs:</span><span class="sxs-lookup"><span data-stu-id="dcc73-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="dcc73-134">上記 Ajax.ActionLink() ヘルパー メソッドは ASP.NET MVC に組み込まれてと同様 Html.ActionLink() ヘルパー メソッドになる標準的な移動を実行する代わりに、アクション メソッドへの AJAX 呼び出しをリンクをクリックするとします。</span><span class="sxs-lookup"><span data-stu-id="dcc73-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="dcc73-135">上記の"RSVP"コント ローラーの"Register"のアクション メソッドを呼び出すことはしています"id"パラメーターとして、DinnerID を渡します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="dcc73-136">最後の AjaxOptions パラメーターを渡しているでは、アクション メソッドから返されるコンテンツを受け取り、HTML を更新することを示します&lt;div&gt; id が"rsvpmsg"、ページの要素。</span><span class="sxs-lookup"><span data-stu-id="dcc73-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="dcc73-137">およびようになりました、夕食をユーザーが閲覧する際に登録されていませんまだ、その RSVP へのリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dcc73-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="dcc73-138">RSVP コント ローラーで、登録アクション メソッドへの AJAX 呼び出しを行うします"RSVP このイベントの"リンクをクリックした場合と更新されたメッセージが表示されますが、完了時に次のような。</span><span class="sxs-lookup"><span data-stu-id="dcc73-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="dcc73-139">ネットワーク帯域幅およびトラフィックに関係するは、この AJAX 呼び出しを行うときに、非常に軽量です。</span><span class="sxs-lookup"><span data-stu-id="dcc73-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="dcc73-140">HTTP POST 小さなネットワーク要求が行われる、ユーザーは、「このイベントを予約」リンクをクリックすると、 */Dinners/Register/1* URL をネットワーク上で次のようになります。</span><span class="sxs-lookup"><span data-stu-id="dcc73-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="dcc73-141">この登録アクション メソッドからの応答は単に.</span><span class="sxs-lookup"><span data-stu-id="dcc73-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="dcc73-142">この軽量の呼び出しは、高速は低速ネットワーク上でも動作します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="dcc73-143">JQuery アニメーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="dcc73-144">AJAX 機能を実装しましたは、そして高速に動作します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="dcc73-145">場合によって発生してもため高速で、ユーザーに RSVP のリンクを新しいテキストに置き換えられること気付かないこと。</span><span class="sxs-lookup"><span data-stu-id="dcc73-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="dcc73-146">結果をもう少し明確なさせる更新メッセージに注目させるために単純なアニメーションを追加できます。</span><span class="sxs-lookup"><span data-stu-id="dcc73-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="dcc73-147">既定 ASP.NET MVC プロジェクト テンプレートにはには、jQuery – Microsoft ではサポートされても優れた (と非常に人気のある) のオープン ソース JavaScript ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dcc73-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="dcc73-148">jQuery では、さまざまな優れた HTML DOM の選択と効果ライブラリを含む機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="dcc73-149">JQuery を使用するには、スクリプト参照を最初に追加します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="dcc73-150">サイト内のさまざまな場所で jQuery を使用するので、すべてのページが使用できるように、Site.master マスター ページ ファイル内のスクリプト参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="dcc73-151">*ヒント: VS 2008 sp1 (jQuery など)、JavaScript ファイルの高度な intellisense サポートを有効にする JavaScript intellisense の修正プログラムがインストールされていることを確認します。ダウンロードすることができます。 http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="dcc73-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="dcc73-152">多くの場合、JQuery を使用して記述されたコードは、グローバル「$ ()」を使用して JavaScript メソッドを CSS セレクターを使用して 1 つまたは複数の HTML 要素を取得します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="dcc73-153">たとえば、 <em>$("#rsvpmsg")</em> rsvpmsg の id を持つ任意の HTML 要素を選択中に<em>$(".something")</em> 「何か」CSS ですべての要素を選択クラス名。</span><span class="sxs-lookup"><span data-stu-id="dcc73-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="dcc73-154">「すべてのチェックのラジオ ボタンを返す」などのより高度なクエリを記述することもできます。 セレクターのようなクエリを使用して: <em>$("入力 [@type= ラジオ] [@checked]")</em>します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="dcc73-155">要素を選択すると、非表示にするように、アクションを実行するためにメソッドを呼び出すことができます: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="dcc73-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="dcc73-156">ここで予約は、"rsvpmsg"を選択する"AnimateRSVPMessage"という名前の単純な JavaScript 関数を定義します&lt;div&gt;とそのテキスト コンテンツのサイズをアニメーション化します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="dcc73-157">次のコード開始小さなテキストとし、原因 400 ミリ秒単位の期間の経過とともに増大します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="dcc73-158">いますし、ワイヤ アップできる、Ajax.ActionLink() ヘルパー メソッドにその名前を渡すことによって、AJAX 呼び出しが正常に完了した後に呼び出される場合は、この JavaScript 関数 (AjaxOptions"OnSuccess"を使用してイベントのプロパティ)。</span><span class="sxs-lookup"><span data-stu-id="dcc73-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="dcc73-159">ようになりました"RSVP このイベントの"リンクをクリックし、AJAX 呼び出しが正常に完了、内容のメッセージが送信されるときにアニメーション化する前後のサイズが大きく。</span><span class="sxs-lookup"><span data-stu-id="dcc73-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="dcc73-160">"OnSuccess"イベントを提供するだけでなくは、AjaxOptions オブジェクトは、(その他のプロパティと便利なオプションのさまざまな) と共に処理できる OnBegin、OnFailure、および OnComplete イベントを公開します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="dcc73-161">クリーンアップ - RSVP の部分的なビューをリファクタリング</span><span class="sxs-lookup"><span data-stu-id="dcc73-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="dcc73-162">詳細ビュー テンプレートは、どの超過は少し大変になることを理解する少し長を取得する開始します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="dcc73-163">コードの読みやすさを向上させるには、終了 RSVP ビュー コードの詳細ページのすべてをカプセル化する部分ビュー – RSVPStatus.ascx – を作成します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="dcc73-164">この \Views\Dinners フォルダーを右クリックし、[追加]-で実行できます&gt;メニュー コマンドを表示します。</span><span class="sxs-lookup"><span data-stu-id="dcc73-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="dcc73-165">その厳密に型指定された ViewModel として Dinner オブジェクトの取得があります。</span><span class="sxs-lookup"><span data-stu-id="dcc73-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="dcc73-166">私たちことができますし、コピー/貼り付け RSVP コンテンツを Details.aspx ビューから。</span><span class="sxs-lookup"><span data-stu-id="dcc73-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="dcc73-167">処理が完了したら、別の部分ビュー – EditAndDeleteLinks.ascx -、編集、削除のリンクの表示コードをカプセル化するを作成しましょうも。</span><span class="sxs-lookup"><span data-stu-id="dcc73-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="dcc73-168">その厳密に型指定された ViewModel として Dinner オブジェクトとそこに、Details.aspx ビューから編集および削除ロジックのコピー/貼り付けを必要もあります。</span><span class="sxs-lookup"><span data-stu-id="dcc73-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="dcc73-169">当社の詳細は、テンプレートが表示し、下部にある 2 つの Html.RenderPartial() メソッドの呼び出しを含めるだけです。</span><span class="sxs-lookup"><span data-stu-id="dcc73-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="dcc73-170">これにより、コードの読み取りおよびメンテナンス クリーナー。</span><span class="sxs-lookup"><span data-stu-id="dcc73-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="dcc73-171">次の手順</span><span class="sxs-lookup"><span data-stu-id="dcc73-171">Next Step</span></span>

<span data-ttu-id="dcc73-172">これで、さらに詳しく AJAX を使用し、アプリケーションに対話型のマッピング サポートを追加方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="dcc73-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dcc73-173">[前へ](secure-applications-using-authentication-and-authorization.md)
> [次へ](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="dcc73-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
