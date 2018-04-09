---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: ASP.NET Web ページの HTML フォームの基本の概要 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルでは、ASP.NET Web Pages (Razor) を使用すると、ユーザーの入力を処理する方法、および入力フォームを作成する方法の基礎を説明します。 今すぐそのしてしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 6f44f74774c2fa6338524987779e15f3940d1830
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a><span data-ttu-id="01776-104">ASP.NET Web ページの HTML フォームの基本の概要</span><span class="sxs-lookup"><span data-stu-id="01776-104">Introducing ASP.NET Web Pages - HTML Form Basics</span></span>
====================
<span data-ttu-id="01776-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="01776-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="01776-106">このチュートリアルでは、ASP.NET Web Pages (Razor) を使用すると、ユーザーの入力を処理する方法、および入力フォームを作成する方法の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="01776-106">This tutorial shows you the basics of how to create an input form and how to handle the user's input when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="01776-107">データベースを取得したら、これを使用して、フォームのスキル、データベース内の特定のムービーを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="01776-107">And now that you've got a database, you'll use your form skills to let users find specific movies in the database.</span></span> <span data-ttu-id="01776-108">を通じてシリーズを完了すると想定[概要を表示するデータを使用して ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data)です。</span><span class="sxs-lookup"><span data-stu-id="01776-108">It assumes you have completed the series through [Introduction to Displaying Data Using ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).</span></span>
> 
> <span data-ttu-id="01776-109">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="01776-109">What you'll learn:</span></span>
> 
> - <span data-ttu-id="01776-110">標準の HTML 要素を使用してフォームを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="01776-110">How to create a form by using standard HTML elements.</span></span>
> - <span data-ttu-id="01776-111">ユーザーを読み取る方法は形式で入力します。</span><span class="sxs-lookup"><span data-stu-id="01776-111">How to read the user's input in a form.</span></span>
> - <span data-ttu-id="01776-112">SQL クエリを選択的に、検索を使用してデータを取得用語をユーザーを作成する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="01776-112">How to create a SQL query that selectively gets data by using a search term that the user supplies.</span></span>
> - <span data-ttu-id="01776-113">ページで「に注意してください」ユーザーが入力したフィールドを持つ方法です。</span><span class="sxs-lookup"><span data-stu-id="01776-113">How to have fields in the page "remember" what the user entered.</span></span>
>   
> 
> <span data-ttu-id="01776-114">説明されている機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="01776-114">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="01776-115">`Request` オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="01776-115">The `Request` object.</span></span>
> - <span data-ttu-id="01776-116">SQL`Where`句。</span><span class="sxs-lookup"><span data-stu-id="01776-116">The SQL `Where` clause.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="01776-117">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="01776-117">What You'll Build</span></span>

<span data-ttu-id="01776-118">前のチュートリアルでデータベースを作成、データを追加して使用して、`WebGrid`データを表示するためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="01776-118">In the previous tutorial, you created a database, added data to it, and then used the `WebGrid` helper to display the data.</span></span> <span data-ttu-id="01776-119">このチュートリアルでは、特定ジャンルの映画を検索するか、タイトルには、入力した任意の単語が含まれています、検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="01776-119">In this tutorial, you'll add a search box that lets you find movies of a specific genre or whose title contains whatever word you enter.</span></span> <span data-ttu-id="01776-120">(たとえば、ことができますをジャンルが"Action"またはタイトルには、"Harry"または"Adventure"が含まれています。 すべての映画を検索する)</span><span class="sxs-lookup"><span data-stu-id="01776-120">(For example, you'll be able to find all movies whose genre is "Action" or whose title contains "Harry" or "Adventure.")</span></span>

<span data-ttu-id="01776-121">このチュートリアルを完了したら、次のようなページが必要があります。</span><span class="sxs-lookup"><span data-stu-id="01776-121">When you're done with this tutorial, you'll have a page like this one:</span></span>

![映画ジャンルとタイトルの検索 ページ](form-basics/_static/image1.png)

<span data-ttu-id="01776-123">ページの一覧の一部は、前回のチュートリアルと同じ&mdash;グリッドです。</span><span class="sxs-lookup"><span data-stu-id="01776-123">The listing part of the page is the same as in the last tutorial &mdash; a grid.</span></span> <span data-ttu-id="01776-124">違いは、グリッドが表示される映画のみを検索することになります。</span><span class="sxs-lookup"><span data-stu-id="01776-124">The difference will be that the grid will show only the movies that you searched for.</span></span>

## <a name="about-html-forms"></a><span data-ttu-id="01776-125">HTML フォームについて</span><span class="sxs-lookup"><span data-stu-id="01776-125">About HTML Forms</span></span>

<span data-ttu-id="01776-126">(HTML フォームの作成との違いのエクスペリエンスを思うかどうか`GET`と`POST`、このセクションをスキップすることができます)。</span><span class="sxs-lookup"><span data-stu-id="01776-126">(If you've got experience with creating HTML forms and with the difference between `GET` and `POST`, you can skip this section.)</span></span>

<span data-ttu-id="01776-127">フォームがユーザー入力要素を持つ&mdash;テキスト ボックス、ボタン、オプション ボタン、チェック ボックス、ドロップダウン リスト、およびなどです。</span><span class="sxs-lookup"><span data-stu-id="01776-127">A form has user input elements &mdash; text boxes, buttons, radio buttons, check boxes, drop-down lists, and so on.</span></span> <span data-ttu-id="01776-128">ユーザーは、これらのコントロールに入力または選択を行います、ボタンをクリックして、フォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="01776-128">Users fill in these controls or make selections and then submit the form by clicking a button.</span></span>

<span data-ttu-id="01776-129">この例で、フォームの基本的な HTML 構文を示します。</span><span class="sxs-lookup"><span data-stu-id="01776-129">The basic HTML syntax of a form is illustrated by this example:</span></span>

[!code-html[Main](form-basics/samples/sample1.html)]

<span data-ttu-id="01776-130">このマークアップ ページで実行されると、次の図のような単純なフォームが作成されます。</span><span class="sxs-lookup"><span data-stu-id="01776-130">When this markup runs in a page, it creates a simple form that looks like this illustration:</span></span>

![ブラウザーでレンダリングされたとして HTML フォームの基本](form-basics/_static/image2.png)

<span data-ttu-id="01776-132">`<form>`要素を送信する HTML 要素で囲みます。</span><span class="sxs-lookup"><span data-stu-id="01776-132">The `<form>` element encloses HTML elements to be submitted.</span></span> <span data-ttu-id="01776-133">(ようにする、簡単なミスがページに要素を追加が、内部に配置を忘れること、`<form>`要素。</span><span class="sxs-lookup"><span data-stu-id="01776-133">(An easy mistake to make is to add elements to the page but then forget to put them inside a `<form>` element.</span></span> <span data-ttu-id="01776-134">その場合、何も送信されます。)`method`属性は、ユーザー入力を送信する方法をブラウザーに指示します。</span><span class="sxs-lookup"><span data-stu-id="01776-134">In that case, nothing is submitted.) The `method` attribute tells the browser how to submit the user input.</span></span> <span data-ttu-id="01776-135">これを設定する`post`サーバー上または更新プログラムを実行する場合`get`場合は、サーバーからデータをフェッチしているだけです。</span><span class="sxs-lookup"><span data-stu-id="01776-135">You set this to `post` if you're performing an update on the server or to `get` if you're just fetching data from the server.</span></span>

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> <span data-ttu-id="01776-136">**GET、POST、および HTTP 動詞の安全性**</span><span class="sxs-lookup"><span data-stu-id="01776-136">**GET, POST, and HTTP Verb Safety**</span></span>
> 
> <span data-ttu-id="01776-137">HTTP、ブラウザーとサーバーが情報の交換に使用されるプロトコルは、その基本的な操作で非常に単純です。</span><span class="sxs-lookup"><span data-stu-id="01776-137">HTTP, the protocol that browsers and servers use to exchange information, is remarkably simple in its basic operations.</span></span> <span data-ttu-id="01776-138">ブラウザーでは、いくつかの動詞だけを使用して、サーバーに対して要求を行います。</span><span class="sxs-lookup"><span data-stu-id="01776-138">Browsers use only a few verbs to make requests to servers.</span></span> <span data-ttu-id="01776-139">Web 用のコードを記述する場合は、これらの動詞と、ブラウザーとサーバーの使用方法を理解することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="01776-139">When you write code for the web, it's helpful to understand these verbs and how the browser and server use them.</span></span> <span data-ttu-id="01776-140">遠く離れたと最もよく使用される動詞とは。</span><span class="sxs-lookup"><span data-stu-id="01776-140">Far and away the most commonly used verbs are these:</span></span>
> 
> - <span data-ttu-id="01776-141">`GET`。</span><span class="sxs-lookup"><span data-stu-id="01776-141">`GET`.</span></span> <span data-ttu-id="01776-142">ブラウザーでは、この動詞を使用してサーバーからのものを取り出します。</span><span class="sxs-lookup"><span data-stu-id="01776-142">The browser uses this verb to fetch something from the server.</span></span> <span data-ttu-id="01776-143">たとえば、お使いのブラウザーに URL を入力すると、ブラウザーを実行、`GET`を目的のページを要求する操作。</span><span class="sxs-lookup"><span data-stu-id="01776-143">For example, when you type a URL into your browser, the browser performs a `GET` operation to request the page you want.</span></span> <span data-ttu-id="01776-144">ページには、グラフィックスが含まれている場合、ブラウザーが追加を実行`GET`イメージを取得する操作。</span><span class="sxs-lookup"><span data-stu-id="01776-144">If the page includes graphics, the browser performs additional `GET` operations to get the images.</span></span> <span data-ttu-id="01776-145">場合、`GET`操作は、サーバーに情報を渡す必要がある、情報が URL クエリ文字列内の一部として渡されます。</span><span class="sxs-lookup"><span data-stu-id="01776-145">If the `GET` operation has to pass information to the server, the information is passed as part of the URL in the query string.</span></span>
> - <span data-ttu-id="01776-146">`POST`。</span><span class="sxs-lookup"><span data-stu-id="01776-146">`POST`.</span></span> <span data-ttu-id="01776-147">ブラウザーが送信、`POST`にデータを追加またはサーバー上で変更を送信するために要求します。</span><span class="sxs-lookup"><span data-stu-id="01776-147">The browser sends a `POST` request in order to submit data to be added or changed on the server.</span></span> <span data-ttu-id="01776-148">たとえば、`POST`をデータベースにレコードを作成または既存のテーブルを変更する動詞を使用します。</span><span class="sxs-lookup"><span data-stu-id="01776-148">For example, the `POST` verb is used to create records in a database or change existing ones.</span></span> <span data-ttu-id="01776-149">ほとんどの場合、フォームに入力して送信 ボタンをクリックして、ブラウザーを実行、`POST`操作します。</span><span class="sxs-lookup"><span data-stu-id="01776-149">Most of the time, when you fill in a form and click the submit button, the browser performs a `POST` operation.</span></span> <span data-ttu-id="01776-150">`POST`操作、サーバーに渡されるデータが、ページの本文。</span><span class="sxs-lookup"><span data-stu-id="01776-150">In a `POST` operation, the data being passed to the server is in the body of the page.</span></span>
> 
> <span data-ttu-id="01776-151">これらの動詞を重要な違いは、`GET`操作がサーバー上のデータを変更することはできません: または抽象若干の他の方法で配置する、`GET`操作がサーバー上で状態の変更になりません。</span><span class="sxs-lookup"><span data-stu-id="01776-151">An important distinction between these verbs is that a `GET` operation is not supposed to change anything on the server — or to put it in a slightly more abstract way, a `GET` operation does not result in a change in state on the server.</span></span> <span data-ttu-id="01776-152">実行することができます、`GET`何度でも、必要な場合、およびそれらのリソースを変更しないと同じリソースで操作します。</span><span class="sxs-lookup"><span data-stu-id="01776-152">You can perform a `GET` operation on the same resources as many times as you like, and those resources don't change.</span></span> <span data-ttu-id="01776-153">(A `GET` 「安全」、または技術の用語を使用する操作とよく言われますは*べき等*)。もちろん、これに対して、`POST`変更を要求、サーバーで何か、操作を実行するたびにします。</span><span class="sxs-lookup"><span data-stu-id="01776-153">(A `GET` operation is often said to be "safe," or to use a technical term, is *idempotent*.) In contrast, of course, a `POST` request changes something on the server each time you perform the operation.</span></span>
> 
> <span data-ttu-id="01776-154">2 つの例は、この区別を示すことができます。</span><span class="sxs-lookup"><span data-stu-id="01776-154">Two examples will help illustrate this distinction.</span></span> <span data-ttu-id="01776-155">検索を実行するときに Bing や Google、エンジンを使用して 1 つのテキスト ボックスで構成される形式で入力して、[検索] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="01776-155">When you perform a search using an engine like Bing or Google, you fill in a form that consists of one text box, and then you click the search button.</span></span> <span data-ttu-id="01776-156">ブラウザーを実行、 `GET` URL の一部として渡されたボックスに入力した値を指定の操作です。</span><span class="sxs-lookup"><span data-stu-id="01776-156">The browser performs a `GET` operation, with the value you entered into the box passed as part of the URL.</span></span> <span data-ttu-id="01776-157">使用して、`GET`操作情報だけこの種類のフォームが、正常では、検索操作は、サーバー上のすべてのリソースは変わらないため、フェッチします。</span><span class="sxs-lookup"><span data-stu-id="01776-157">Using a `GET` operation for this type of form is fine, because a search operation doesn't change any resources on the server, it just fetches information.</span></span>
> 
> <span data-ttu-id="01776-158">ここでのオンライン注文を検討してください。</span><span class="sxs-lookup"><span data-stu-id="01776-158">Now consider the process of ordering something online.</span></span> <span data-ttu-id="01776-159">注文の詳細を入力し、[送信] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="01776-159">You fill in the order details and then click the submit button.</span></span> <span data-ttu-id="01776-160">この操作になります、`POST`のため、操作の結果は、新しい注文レコードをアカウント情報の変更とおそらく他多くの変更など、サーバー上の変更を要求します。</span><span class="sxs-lookup"><span data-stu-id="01776-160">This operation will be a `POST` request, because the operation will result in changes on the server, such as a new order record, a change in your account information, and perhaps many other changes.</span></span> <span data-ttu-id="01776-161">異なり、`GET`繰り返すことはできません、操作、`POST`要求: サーバーで新しい注文を生成し、削除できた場合、要求を再送信するたびにします。</span><span class="sxs-lookup"><span data-stu-id="01776-161">Unlike the `GET` operation, you cannot repeat your `POST` request — if you did, each time you resubmitted the request, you'd generate a new order on the server.</span></span> <span data-ttu-id="01776-162">(このような場合、web サイトが複数回送信ボタンをクリックする多くの場合は警告または誤ってフォームを再送信されないように、[送信] ボタンを無効になります)。</span><span class="sxs-lookup"><span data-stu-id="01776-162">(In cases like this, websites will often warn you not to click a submit button more than once, or will disable the submit button so that you don't resubmit the form accidentally.)</span></span>
> 
> <span data-ttu-id="01776-163">このチュートリアルの過程でされます両方を使用する、`GET`操作と`POST`HTML フォームを操作します。</span><span class="sxs-lookup"><span data-stu-id="01776-163">In the course of this tutorial, you'll use both a `GET` operation and a `POST` operation to work with HTML forms.</span></span> <span data-ttu-id="01776-164">各ケースのために使用する動詞は適切なを用いて説明します。</span><span class="sxs-lookup"><span data-stu-id="01776-164">We'll explain in each case why the verb you use is the appropriate one.</span></span>
> 
> <span data-ttu-id="01776-165">(HTTP 動詞の詳細については、次を参照してください、[メソッド定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C サイトの記事です。)。</span><span class="sxs-lookup"><span data-stu-id="01776-165">(To learn more about HTTP verbs, see the [Method Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article on the W3C site.)</span></span>


<span data-ttu-id="01776-166">ほとんどのユーザー入力要素は HTML`<input>`要素。</span><span class="sxs-lookup"><span data-stu-id="01776-166">Most user input elements are HTML `<input>` elements.</span></span> <span data-ttu-id="01776-167">ように見える`<input type="type" name="name">,`場所*型*対象ユーザーの入力コントロールの種類を示します。</span><span class="sxs-lookup"><span data-stu-id="01776-167">They look like `<input type="type" name="name">,` where *type* indicates the kind of user input control you want.</span></span> <span data-ttu-id="01776-168">これらの要素が共通します。</span><span class="sxs-lookup"><span data-stu-id="01776-168">These elements are the common ones:</span></span>

- <span data-ttu-id="01776-169">テキスト ボックス: `<input type="text">`</span><span class="sxs-lookup"><span data-stu-id="01776-169">Text box: `<input type="text">`</span></span>
- <span data-ttu-id="01776-170">チェック ボックス: `<input type="check">`</span><span class="sxs-lookup"><span data-stu-id="01776-170">Check box: `<input type="check">`</span></span>
- <span data-ttu-id="01776-171">ラジオ ボタンをクリックします。 `<input type="radio">`</span><span class="sxs-lookup"><span data-stu-id="01776-171">Radio button: `<input type="radio">`</span></span>
- <span data-ttu-id="01776-172">ボタンをクリックします。 `<input type="button">`</span><span class="sxs-lookup"><span data-stu-id="01776-172">Button: `<input type="button">`</span></span>
- <span data-ttu-id="01776-173">ボタンを送信するには。 `<input type="submit">`</span><span class="sxs-lookup"><span data-stu-id="01776-173">Submit button: `<input type="submit">`</span></span>

<span data-ttu-id="01776-174">使用することも、`<textarea>`複数行テキスト ボックスを作成する要素と`<select>`ドロップダウン リストまたはスクロール可能なリストを作成する要素。</span><span class="sxs-lookup"><span data-stu-id="01776-174">You can also use the `<textarea>` element to create a multiline text box and the `<select>` element to create a drop-down list or scrollable list.</span></span> <span data-ttu-id="01776-175">(詳細については、HTML フォーム要素を参照してください[HTML フォームおよび入力](http://www.w3schools.com/html/html_forms.asp)W3Schools サイトです)。</span><span class="sxs-lookup"><span data-stu-id="01776-175">(For more about HTML form elements, see [HTML Forms and Input](http://www.w3schools.com/html/html_forms.asp) on the W3Schools site.)</span></span>

<span data-ttu-id="01776-176">`name`属性された非常に重要で、名前が後で、要素の値の取得方法をすぐにわかります。</span><span class="sxs-lookup"><span data-stu-id="01776-176">The `name` attribute is very important, because the name is how you'll get the value of the element later, as you'll see shortly.</span></span>

<span data-ttu-id="01776-177">興味深い部分は、ユーザーの入力とするページの開発者にしないでください。</span><span class="sxs-lookup"><span data-stu-id="01776-177">The interesting part is what you, the page developer, do with the user's input.</span></span> <span data-ttu-id="01776-178">これらの要素に関連付けられた組み込みの動作はありません。</span><span class="sxs-lookup"><span data-stu-id="01776-178">There's no built-in behavior associated with these elements.</span></span> <span data-ttu-id="01776-179">代わりに、ユーザーが入力または選択された値を取得し、何らかの処理を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="01776-179">Instead, you have to get the values that the user has entered or selected and do something with them.</span></span> <span data-ttu-id="01776-180">このチュートリアルで学習内容です。</span><span class="sxs-lookup"><span data-stu-id="01776-180">That's what you'll learn in this tutorial.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="01776-181">**HTML5、および入力フォーム**</span><span class="sxs-lookup"><span data-stu-id="01776-181">**HTML5 and Input Forms**</span></span>
> 
> <span data-ttu-id="01776-182">可能性がありますがわかってし、遷移では、HTML、最新バージョン (HTML5) には、ユーザー情報を入力するためのより直観的な方法のサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="01776-182">As you might know, HTML is in transition and the latest version (HTML5) includes support for more intuitive ways for users to enter information.</span></span> <span data-ttu-id="01776-183">たとえば、html5、(ページ開発者) ことがわかりますページ日付を入力するユーザーをすることです。</span><span class="sxs-lookup"><span data-stu-id="01776-183">For example, in HTML5, you (the page developer) can tell the page that you want the user to enter a date.</span></span> <span data-ttu-id="01776-184">ブラウザーに自動的に表示できますカレンダーより日付を手動で入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="01776-184">The browser can then automatically display a calendar rather than requiring the user to enter a date manually.</span></span> <span data-ttu-id="01776-185">ただし、HTML5 とはまだサポートされていませんすべてのブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="01776-185">However, HTML5 is new and is not supported in all browsers yet.</span></span>
> 
> <span data-ttu-id="01776-186">ASP.NET Web ページには、HTML5、ユーザーのブラウザーがその範囲内の入力がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="01776-186">ASP.NET Web Pages supports HTML5 input to the extent that the user's browser does.</span></span> <span data-ttu-id="01776-187">新しい属性の概念、 `<input>` HTML5、内の要素を参照してください[HTML&lt;入力&gt;type 属性](http://www.w3schools.com/html/html_form_input_types.asp)W3Schools サイトです。</span><span class="sxs-lookup"><span data-stu-id="01776-187">For an idea of the new attributes for the `<input>` element in HTML5, see [HTML &lt;input&gt; type Attribute](http://www.w3schools.com/html/html_form_input_types.asp) on the W3Schools site.</span></span>


## <a name="creating-the-form"></a><span data-ttu-id="01776-188">フォームの作成</span><span class="sxs-lookup"><span data-stu-id="01776-188">Creating the Form</span></span>

<span data-ttu-id="01776-189">WebMatrix での**ファイル** ワークスペースで、開く、 *Movies.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="01776-189">In WebMatrix, in the **Files** workspace, open the *Movies.cshtml* page.</span></span>

<span data-ttu-id="01776-190">終了後に`</h1>`タグおよび開始する前に`<div>`のタグ、`grid.GetHtml`呼び出し、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="01776-190">After the closing `</h1>` tag and before the opening `<div>` tag of the `grid.GetHtml` call, add the following markup:</span></span>

[!code-html[Main](form-basics/samples/sample2.html)]

<span data-ttu-id="01776-191">このマークアップをという名前のテキスト ボックスを持つフォームを作成する`searchGenre`送信ボタン。</span><span class="sxs-lookup"><span data-stu-id="01776-191">This markup creates a form that has a text box named `searchGenre` and a submit button.</span></span> <span data-ttu-id="01776-192">テキスト ボックスし、ボタンがで囲まれた送信、`<form>`要素が`method`属性に設定されている`get`です。</span><span class="sxs-lookup"><span data-stu-id="01776-192">The text box and submit button are enclosed in a `<form>` element whose `method` attribute is set to `get`.</span></span> <span data-ttu-id="01776-193">(送信中のボタンとテキスト ボックスを配置するされない場合は、必ず、`<form>`要素、ボタンをクリックしたときに何も送信されます)。使用する、`GET`動詞ここでフォームを作成しているためを変更しない、サーバー上: 検索結果だけです。</span><span class="sxs-lookup"><span data-stu-id="01776-193">(Remember that if you don't put the text box and submit button inside a `<form>` element, nothing will be submitted when you click the button.) You use the `GET` verb here because you're creating a form that does not make any changes on the server — it just results in a search.</span></span> <span data-ttu-id="01776-194">(前のチュートリアルでは使用して、`post`メソッドは、これは、サーバーへの変更を送信する方法です。</span><span class="sxs-lookup"><span data-stu-id="01776-194">(In the previous tutorial, you used a `post` method, which is how you submit changes to the server.</span></span> <span data-ttu-id="01776-195">わかりますを次のチュートリアルでもう一度。)</span><span class="sxs-lookup"><span data-stu-id="01776-195">You'll see that in the next tutorial again.)</span></span>

<span data-ttu-id="01776-196">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="01776-196">Run the page.</span></span> <span data-ttu-id="01776-197">フォームのすべての動作を定義していない場合がどのようにが確認できます。</span><span class="sxs-lookup"><span data-stu-id="01776-197">Although you haven't defined any behavior for the form, you can see what it looks like:</span></span>

![ジャンルの検索 ボックスと映画 ページ](form-basics/_static/image3.png)

<span data-ttu-id="01776-199">「コメディ」のように、テキスト ボックスに値を入力します。</span><span class="sxs-lookup"><span data-stu-id="01776-199">Enter a value into the text box, like "Comedy."</span></span> <span data-ttu-id="01776-200">をクリックして**検索ジャンル**です。</span><span class="sxs-lookup"><span data-stu-id="01776-200">Then click **Search Genre**.</span></span>

<span data-ttu-id="01776-201">ページの URL を書き留めます。</span><span class="sxs-lookup"><span data-stu-id="01776-201">Take note of the URL of the page.</span></span> <span data-ttu-id="01776-202">設定するため、`<form>`要素の`method`属性を`get`、入力した値は、次のように、URL 内のクエリ文字列の一部ではようになりました。</span><span class="sxs-lookup"><span data-stu-id="01776-202">Because you set the `<form>` element's `method` attribute to `get`, the value you entered is now part of the query string in the URL, like this:</span></span>

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a><span data-ttu-id="01776-203">フォームの値の読み取り</span><span class="sxs-lookup"><span data-stu-id="01776-203">Reading Form Values</span></span>

<span data-ttu-id="01776-204">ページには、データベースのデータを取得し、グリッドに結果を表示するコードが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="01776-204">The page already contains some code that gets database data and displays the results in a grid.</span></span> <span data-ttu-id="01776-205">検索用語を含む SQL クエリを実行できるように、テキスト ボックスの値を読み取りますいくつかのコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="01776-205">Now you have to add some code that reads the value of the text box so you can run a SQL query that includes the search term.</span></span>

<span data-ttu-id="01776-206">フォームの方法を設定するため`get`次のようにコードを使用して、テキスト ボックスに入力された値を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="01776-206">Because you set the form's method to `get`, you can read the value that was entered into the text box by using code like the following:</span></span>

`var searchTerm = Request.QueryString["searchGenre"];`

<span data-ttu-id="01776-207">`Request.QueryString`オブジェクト (、`QueryString`のプロパティ、`Request`オブジェクト) の一部として送信された要素の値が含まれています、`GET`操作します。</span><span class="sxs-lookup"><span data-stu-id="01776-207">The `Request.QueryString` object (the `QueryString` property of the `Request` object) includes the values of elements that were submitted as part of the `GET` operation.</span></span> <span data-ttu-id="01776-208">`Request.QueryString`プロパティが含まれています、*コレクション*(一覧) の形式で送信される値。</span><span class="sxs-lookup"><span data-stu-id="01776-208">The `Request.QueryString` property contains a *collection* (a list) of the values that are submitted in the form.</span></span> <span data-ttu-id="01776-209">個別の値を取得する要素の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="01776-209">To get any individual value, you specify the name of the element that you want.</span></span> <span data-ttu-id="01776-210">理由ですが必要、`name`属性を`<input>`要素 (`searchTerm`)、テキスト ボックスを作成します。</span><span class="sxs-lookup"><span data-stu-id="01776-210">That's why you have to have a `name` attribute on the `<input>` element (`searchTerm`) that creates the text box.</span></span> <span data-ttu-id="01776-211">(詳細については、`Request`オブジェクトを参照してください、[サイドバー](#BKMK_TheRequestObject)後です)。</span><span class="sxs-lookup"><span data-stu-id="01776-211">(For more about the `Request` object, see the [sidebar](#BKMK_TheRequestObject) later.)</span></span>

<span data-ttu-id="01776-212">単純なので、テキスト ボックスの値を読み取ることはできます。</span><span class="sxs-lookup"><span data-stu-id="01776-212">It's simple enough to read the value of the text box.</span></span> <span data-ttu-id="01776-213">ユーザーが入力しなかったすべてのテキスト ボックス内をクリックした場合、**検索**を検索する項目がないために、ボタンをクリックするを無視するか、します。</span><span class="sxs-lookup"><span data-stu-id="01776-213">But if the user didn't enter anything at all in the text box but clicked **Search** anyway, you can ignore that click, since there's nothing to search.</span></span>

<span data-ttu-id="01776-214">次のコードでは、これらの条件を実装する方法を示す例を示します。</span><span class="sxs-lookup"><span data-stu-id="01776-214">The following code is an example that shows how to implement these conditions.</span></span> <span data-ttu-id="01776-215">(まだこのコードを追加する必要はありませんは、すぐに実行してみましょう。)</span><span class="sxs-lookup"><span data-stu-id="01776-215">(You don't have to add this code yet; you'll do that in a moment.)</span></span>

[!code-csharp[Main](form-basics/samples/sample3.cs)]

<span data-ttu-id="01776-216">この方法で分割するテスト。</span><span class="sxs-lookup"><span data-stu-id="01776-216">The test breaks down in this way:</span></span>

- <span data-ttu-id="01776-217">値を取得`Request.QueryString["searchGenre"]`、つまりに入力された値、`<input>`という名前の要素`searchGenre`です。</span><span class="sxs-lookup"><span data-stu-id="01776-217">Get the value of `Request.QueryString["searchGenre"]`, namely the value that was entered into the `<input>` element named `searchGenre`.</span></span>
- <span data-ttu-id="01776-218">使用して空であるかを調べる、`IsEmpty`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="01776-218">Find out if it's empty by using the `IsEmpty` method.</span></span> <span data-ttu-id="01776-219">このメソッドは、値が含まれています (たとえば、フォーム要素) のものかどうかを決定する標準的な方法です。</span><span class="sxs-lookup"><span data-stu-id="01776-219">This method is the standard way to determine whether something (for example, a form element) contains a value.</span></span> <span data-ttu-id="01776-220">実際には、するがある場合にのみ、目的が*いない*空の場合、そのためしています.</span><span class="sxs-lookup"><span data-stu-id="01776-220">But really, you care only if it's *not* empty, therefore ...</span></span>
- <span data-ttu-id="01776-221">追加、`!`演算子の前に、`IsEmpty`をテストします。</span><span class="sxs-lookup"><span data-stu-id="01776-221">Add the `!` operator in front of the `IsEmpty` test.</span></span> <span data-ttu-id="01776-222">(、`!`演算子は論理 NOT)。</span><span class="sxs-lookup"><span data-stu-id="01776-222">(The `!` operator means logical NOT).</span></span>

<span data-ttu-id="01776-223">英語、全体の`if`条件は、次に変換します*場合は、フォームの searchGenre 要素は空でなく、.。*</span><span class="sxs-lookup"><span data-stu-id="01776-223">In plain English, the entire `if` condition translates into the following: *If the form's searchGenre element is not empty, then ...*</span></span>

<span data-ttu-id="01776-224">このブロックは、検索用語を使用するクエリを作成するためのステージを設定します。</span><span class="sxs-lookup"><span data-stu-id="01776-224">This block sets the stage for creating a query that uses the search term.</span></span> <span data-ttu-id="01776-225">次のセクションを実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="01776-225">You'll do that in the next section.</span></span>

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> <span data-ttu-id="01776-226">**要求オブジェクト**</span><span class="sxs-lookup"><span data-stu-id="01776-226">**The Request Object**</span></span>
> 
> <span data-ttu-id="01776-227">`Request`オブジェクトには、ブラウザーは、ページが要求または送信されるときに、アプリケーションに送信されるすべての情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="01776-227">The `Request` object contains all the information that the browser sends to your application when a page is requested or submitted.</span></span> <span data-ttu-id="01776-228">このオブジェクトには、テキスト ボックスの値またはアップロードするファイルと同様に、ユーザーが提供する情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="01776-228">This object includes any information that the user provides, like text box values or a file to upload.</span></span> <span data-ttu-id="01776-229">あらゆる種類の cookie、URL クエリ文字列 (存在する場合) 内の値と同様に、追加の情報も含まれていますページを実行している、ユーザーが使用すると、ブラウザーで設定されている言語の一覧ブラウザーの種類のファイル パス。、その他。</span><span class="sxs-lookup"><span data-stu-id="01776-229">It also includes all sorts of additional information, like cookies, values in the URL query string (if any), the file path of the page that is running, the type of browser that the user is using, the list of languages that are set in the browser, and much more.</span></span>
> 
> <span data-ttu-id="01776-230">`Request`オブジェクトが、*コレクション*(リスト) の値。</span><span class="sxs-lookup"><span data-stu-id="01776-230">The `Request` object is a *collection* (list) of values.</span></span> <span data-ttu-id="01776-231">コレクションから個々 の値は、その名前を指定することによって取得します。</span><span class="sxs-lookup"><span data-stu-id="01776-231">You get an individual value out of the collection by specifying its name:</span></span>
> 
> `var someValue = Request["name"];`
> 
> <span data-ttu-id="01776-232">`Request`オブジェクトが実際にはいくつかのサブセットを公開します。</span><span class="sxs-lookup"><span data-stu-id="01776-232">The `Request` object actually exposes several subsets.</span></span> <span data-ttu-id="01776-233">例えば:</span><span class="sxs-lookup"><span data-stu-id="01776-233">For example:</span></span>
> 
> - <span data-ttu-id="01776-234">`Request.Form` 送信された内の要素から値`<form>`要素要求がある場合、`POST`要求します。</span><span class="sxs-lookup"><span data-stu-id="01776-234">`Request.Form` gives you values from elements inside the submitted `<form>` element if the request is a `POST` request.</span></span>
> - <span data-ttu-id="01776-235">`Request.QueryString` 使用する値だけ URL のクエリ文字列します。</span><span class="sxs-lookup"><span data-stu-id="01776-235">`Request.QueryString` gives you just the values in the URL's query string.</span></span> <span data-ttu-id="01776-236">(ような URL で`http://mysite/myapp/page?searchGenre=action&page=2`、 `?searchGenre=action&page=2` URL のセクションは、クエリ文字列です)。</span><span class="sxs-lookup"><span data-stu-id="01776-236">(In a URL like `http://mysite/myapp/page?searchGenre=action&page=2`, the `?searchGenre=action&page=2` section of the URL is the query string.)</span></span>
> - <span data-ttu-id="01776-237">`Request.Cookies` コレクションは、ブラウザーが送信されたクッキーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="01776-237">`Request.Cookies` collection gives you access to cookies that the browser has sent.</span></span>
> 
> <span data-ttu-id="01776-238">送信されたフォームがわかっている値を取得するが、使用することができます`Request["name"]`です。</span><span class="sxs-lookup"><span data-stu-id="01776-238">To get a value that you know is in the submitted form, you can use `Request["name"]`.</span></span> <span data-ttu-id="01776-239">具体的なバージョンを使用する代わりに、 `Request.Form["name"]` (の`POST`要求) または`Request.QueryString["name"]`(の`GET`要求)。</span><span class="sxs-lookup"><span data-stu-id="01776-239">Alternatively, you can use the more specific versions `Request.Form["name"]` (for `POST` requests) or `Request.QueryString["name"]` (for `GET` requests).</span></span> <span data-ttu-id="01776-240">もちろん、*名前*を取得する項目の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="01776-240">Of course, *name* is the name of the item to get.</span></span>
> 
> <span data-ttu-id="01776-241">取得する項目の名前は、使用しているコレクション内で一意であることができます。</span><span class="sxs-lookup"><span data-stu-id="01776-241">The name of the item you want to get has to be unique within the collection you're using.</span></span> <span data-ttu-id="01776-242">だからこそ、`Request`オブジェクトは、サブセットと同様に、提供`Request.Form`と`Request.QueryString`です。</span><span class="sxs-lookup"><span data-stu-id="01776-242">That's why the `Request` object provides the subsets like `Request.Form` and `Request.QueryString`.</span></span> <span data-ttu-id="01776-243">ページにはという名前のフォーム要素が含まれていると`userName`と*も*という名前の cookie を含む`userName`です。</span><span class="sxs-lookup"><span data-stu-id="01776-243">Suppose that your page contains a form element named `userName` and *also* contains a cookie named `userName`.</span></span> <span data-ttu-id="01776-244">表示された場合`Request["userName"]`フォームの値または cookie するかどうかがあいまいです。</span><span class="sxs-lookup"><span data-stu-id="01776-244">If you get `Request["userName"]`, it's ambiguous whether you want the form value or the cookie.</span></span> <span data-ttu-id="01776-245">ただし、表示された場合`Request.Form["userName"]`または`Request.Cookie["userName"]`、取得するには、どの値を明示するしています。</span><span class="sxs-lookup"><span data-stu-id="01776-245">However, if you get `Request.Form["userName"]` or `Request.Cookie["userName"]`, you're being explicit about which value to get.</span></span>
> 
> <span data-ttu-id="01776-246">特定してがのサブセットを使用することをお勧め`Request`興味のあると同様に`Request.Form`または`Request.QueryString`です。</span><span class="sxs-lookup"><span data-stu-id="01776-246">It's a good practice to be specific and use the subset of `Request` that you're interested in, like `Request.Form` or `Request.QueryString`.</span></span> <span data-ttu-id="01776-247">このチュートリアルで作成する単純なページに対してその可能性があります本当にも行いません。</span><span class="sxs-lookup"><span data-stu-id="01776-247">For the simple pages that you're creating in this tutorial, it probably doesn't really make any difference.</span></span> <span data-ttu-id="01776-248">ただしより複雑なページを作成すると、明示的なバージョンを使用`Request.Form`または`Request.QueryString`ページには、フォーム (または複数の形式) が含まれている場合に発生する可能性がある問題を回避するための cookie やクエリ文字列値です。</span><span class="sxs-lookup"><span data-stu-id="01776-248">However, as you create more complex pages, using the explicit version `Request.Form` or `Request.QueryString` can help you avoid problems that can arise when the page contains a form (or multiple forms), cookies, query string values, and so on.</span></span>


## <a name="creating-a-query-by-using-a-search-term"></a><span data-ttu-id="01776-249">検索用語を使用してクエリを作成します。</span><span class="sxs-lookup"><span data-stu-id="01776-249">Creating a Query by Using a Search Term</span></span>

<span data-ttu-id="01776-250">ユーザーが入力した検索用語を取得する方法を理解したところでは、それを使用するクエリを作成できます。</span><span class="sxs-lookup"><span data-stu-id="01776-250">Now that you know how to get the search term that the user entered, you can create a query that uses it.</span></span> <span data-ttu-id="01776-251">データベースからのすべてのムービー項目を取得するには、このステートメントのような SQL クエリを使用して注意してください。</span><span class="sxs-lookup"><span data-stu-id="01776-251">Remember that to get all the movie items out of the database, you're using a SQL query that looks like this statement:</span></span>

`SELECT * FROM Movies`

<span data-ttu-id="01776-252">特定の映画のみを取得するを含むクエリを使用する必要が、`Where`句。</span><span class="sxs-lookup"><span data-stu-id="01776-252">To get only certain movies, you have to use a query that includes a `Where` clause.</span></span> <span data-ttu-id="01776-253">この句を使用して、クエリによって返される行を条件を設定できます。</span><span class="sxs-lookup"><span data-stu-id="01776-253">This clause lets you set a condition on which rows are returned by the query.</span></span> <span data-ttu-id="01776-254">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="01776-254">Here's an example:</span></span>

`SELECT * FROM Movies WHERE Genre = 'Action'`

<span data-ttu-id="01776-255">基本形式は`WHERE column = value`します。</span><span class="sxs-lookup"><span data-stu-id="01776-255">The basic format is `WHERE column = value`.</span></span> <span data-ttu-id="01776-256">さまざまな演算子を使用するには、さらに、ジャスト`=`と同様、 `>` (より大きい)、 `<` (より小さい)、 `<>` (等しくない)、 `<=` (以下を)、探している新機能によってなどです。</span><span class="sxs-lookup"><span data-stu-id="01776-256">You can use different operators besides just `=`, like `>` (greater than), `<` (less than), `<>` (not equal to), `<=` (less than or equal to), etc., depending on what you're looking for.</span></span>

<span data-ttu-id="01776-257">念のため、SQL ステートメントを大文字小文字が区別されない&mdash;`SELECT`と同じ`Select`(あるいは`select`)。</span><span class="sxs-lookup"><span data-stu-id="01776-257">In case you're wondering, SQL statements are not case sensitive &mdash; `SELECT` is the same as `Select` (or even `select`).</span></span> <span data-ttu-id="01776-258">ただし、ユーザー多くの場合、大文字にする SQL ステートメントでキーワードと同様に`SELECT`と`WHERE`を読みやすきます。</span><span class="sxs-lookup"><span data-stu-id="01776-258">However, people often capitalize keywords in a SQL statement, like `SELECT` and `WHERE`, to make it easier to read.</span></span>

### <a name="passing-the-search-term-as-a-parameter"></a><span data-ttu-id="01776-259">検索用語をパラメーターとして渡す</span><span class="sxs-lookup"><span data-stu-id="01776-259">Passing the search term as a parameter</span></span>

<span data-ttu-id="01776-260">特定のジャンルが非常に簡単を検索 (`WHERE Genre = 'Action'`) をユーザーが入力した任意のジャンルを検索できるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="01776-260">Searching for a specific genre is easy enough (`WHERE Genre = 'Action'`), but you want to be able to search for any genre that the user enters.</span></span> <span data-ttu-id="01776-261">実行するには、検索する値のプレース ホルダーを含む SQL クエリとして作成します。</span><span class="sxs-lookup"><span data-stu-id="01776-261">To do that, you create as SQL query that includes a placeholder for the value to search.</span></span> <span data-ttu-id="01776-262">このコマンドのようになります。</span><span class="sxs-lookup"><span data-stu-id="01776-262">It will look like this command:</span></span>

`SELECT * FROM Movies WHERE Genre = @0`

<span data-ttu-id="01776-263">プレース ホルダーは、`@`文字の後に 0 個です。</span><span class="sxs-lookup"><span data-stu-id="01776-263">The placeholder is the `@` character followed by zero.</span></span> <span data-ttu-id="01776-264">ご想像のとおり、クエリが複数のプレース ホルダーを含めることができ、という名前になります`@0`、 `@1`、 `@2`, などです。</span><span class="sxs-lookup"><span data-stu-id="01776-264">As you might guess, a query can contain multiple placeholders, and they'd be named `@0`, `@1`, `@2`, etc.</span></span>

<span data-ttu-id="01776-265">クエリを設定し、実際に値を渡すにするには、次のようにコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="01776-265">To set up the query and actually pass it the value, you use the code like the following:</span></span>

[!code-sql[Main](form-basics/samples/sample4.sql)]

<span data-ttu-id="01776-266">このコードは、新機能を既にインストールしているデータを表示するグリッド内に似ています。</span><span class="sxs-lookup"><span data-stu-id="01776-266">This code is similar to what you've already done to display data in the grid.</span></span> <span data-ttu-id="01776-267">唯一の違いは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="01776-267">The only differences are:</span></span>

- <span data-ttu-id="01776-268">クエリには、プレース ホルダーが含まれています (`WHERE Genre = @0"`)。</span><span class="sxs-lookup"><span data-stu-id="01776-268">The query contains a placeholder (`WHERE Genre = @0"`).</span></span>
- <span data-ttu-id="01776-269">クエリは、変数に格納されます (`selectCommand`) 以外の場合は、クエリに直接渡される前に、`db.Query`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="01776-269">The query is put into a variable (`selectCommand`); before, you passed the query directly to the `db.Query` method.</span></span>
- <span data-ttu-id="01776-270">呼び出すと、`db.Query`メソッドを渡すクエリと、プレース ホルダーを使用する値の両方です。</span><span class="sxs-lookup"><span data-stu-id="01776-270">When you call the `db.Query` method, you pass both the query and the value to use for the placeholder.</span></span> <span data-ttu-id="01776-271">(複数のプレース ホルダーがある場合、それらを渡す場合、メソッドに別の値としてすべてです)。</span><span class="sxs-lookup"><span data-stu-id="01776-271">(If the query had multiple placeholders, you'd pass them all as separate values to the method.)</span></span>

<span data-ttu-id="01776-272">これらすべての要素を一緒に配置する場合は、次のコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="01776-272">If you put all these elements together, you get the following code:</span></span>

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="01776-273">**重要**。</span><span class="sxs-lookup"><span data-stu-id="01776-273">**Important!**</span></span> <span data-ttu-id="01776-274">プレース ホルダーを使用する (と同様に`@0`) は、SQL コマンドに値を渡す*非常に重要*セキュリティのためです。</span><span class="sxs-lookup"><span data-stu-id="01776-274">Using placeholders (like `@0`) to pass values to a SQL command is *extremely important* for security.</span></span> <span data-ttu-id="01776-275">ここに表示されること、変数のデータのプレース ホルダーでは唯一の方法が SQL コマンドを構築する必要があります。</span><span class="sxs-lookup"><span data-stu-id="01776-275">The way you see it here, with placeholders for variable data, is the only way you should construct SQL commands.</span></span>
> 
> <span data-ttu-id="01776-276">ありません (連結) のリテラル テキストと、ユーザーから取得する値、一緒に配置することにより、SQL ステートメントを構築します。</span><span class="sxs-lookup"><span data-stu-id="01776-276">Never construct a SQL statement by putting together (concatenating) literal text and values you get from the user.</span></span> <span data-ttu-id="01776-277">ようにサイトを開き、SQL ステートメントにユーザー入力の連結、 *SQL インジェクション攻撃*悪意のあるユーザーがハッキング、データベース ページへの値を送信します。</span><span class="sxs-lookup"><span data-stu-id="01776-277">Concatenating user input into a SQL statement opens your site to a *SQL injection attack* where a malicious user submits values to your page that hack your database.</span></span> <span data-ttu-id="01776-278">(詳細を読み取ることができます、記事[SQL インジェクション](https://msdn.microsoft.com/library/ms161953.aspx)MSDN web サイトです)。</span><span class="sxs-lookup"><span data-stu-id="01776-278">(You can read more in the article [SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) the MSDN website.)</span></span>


## <a name="updating-the-movies-page-with-search-code"></a><span data-ttu-id="01776-279">検索コードと映画 ページを更新</span><span class="sxs-lookup"><span data-stu-id="01776-279">Updating the Movies Page with Search Code</span></span>

<span data-ttu-id="01776-280">内のコードを更新するようになりました、 *Movies.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="01776-280">Now you can update the code in the *Movies.cshtml* file.</span></span> <span data-ttu-id="01776-281">を開始するには、このコードで、ページの上部にあるコード ブロック内のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="01776-281">To begin, replace the code in the code block at the top of the page with this code:</span></span>

[!code-csharp[Main](form-basics/samples/sample6.cs)]

<span data-ttu-id="01776-282">ここでの違いは、クエリに配置したこと、`selectCommand`変数を渡します`db.Query`以降。</span><span class="sxs-lookup"><span data-stu-id="01776-282">The difference here is that you've put the query into the `selectCommand` variable, which you'll pass to `db.Query` later.</span></span> <span data-ttu-id="01776-283">変数に SQL ステートメントを配置することでは、検索を実行する操作は、ステートメントを変更できます。</span><span class="sxs-lookup"><span data-stu-id="01776-283">Putting the SQL statement into a variable lets you change the statement, which is what you'll do to perform the search.</span></span>

<span data-ttu-id="01776-284">配置バックアップの後でこれら 2 つの行も削除されました。</span><span class="sxs-lookup"><span data-stu-id="01776-284">You've also removed these two lines, which you'll put back in later:</span></span>

[!code-csharp[Main](form-basics/samples/sample7.cs)]

<span data-ttu-id="01776-285">まだクエリを実行する (つまり、呼び出す`db.Query`) を初期化しないと、`WebGrid`ヘルパーまだです。</span><span class="sxs-lookup"><span data-stu-id="01776-285">You don't want to run the query yet (that is, call `db.Query`) and you don't want to initialize the `WebGrid` helper yet either.</span></span> <span data-ttu-id="01776-286">実行する SQL ステートメントを理解した後、それらの作業を行います。</span><span class="sxs-lookup"><span data-stu-id="01776-286">You'll do those things after you've figured out which SQL statement has to run.</span></span>

<span data-ttu-id="01776-287">この書き換えられたブロックの後に、新しい検索を処理するためのロジックを追加できます。</span><span class="sxs-lookup"><span data-stu-id="01776-287">After this rewritten block, you can add the new logic for handling the search.</span></span> <span data-ttu-id="01776-288">完成したコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="01776-288">The completed code will look like the following.</span></span> <span data-ttu-id="01776-289">この例に一致するように、ページのコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="01776-289">Update the code in your page so it matches this example:</span></span>

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

<span data-ttu-id="01776-290">ページに次のようになりました。</span><span class="sxs-lookup"><span data-stu-id="01776-290">The page now works like this.</span></span> <span data-ttu-id="01776-291">コードが、データベースを開き、ページを実行するたびに、`selectCommand`変数からのすべてのレコードを取得する SQL ステートメントを設定する、`Movies`テーブル。</span><span class="sxs-lookup"><span data-stu-id="01776-291">Every time the page runs, the code opens the database and the `selectCommand` variable is set to the SQL statement that gets all the records from the `Movies` table.</span></span> <span data-ttu-id="01776-292">また、コードを初期化、`searchTerm`変数。</span><span class="sxs-lookup"><span data-stu-id="01776-292">The code also initializes the `searchTerm` variable.</span></span>

<span data-ttu-id="01776-293">ただし、現在の要求の値が含まれる場合、`searchGenre`要素では、コード セット`selectCommand`別のクエリを: つまりに含まれています、`Where`ジャンルを検索する句。</span><span class="sxs-lookup"><span data-stu-id="01776-293">However, if the current request includes a value for the `searchGenre` element, the code sets `selectCommand` to a different query — namely, to one that includes the `Where` clause to search for a genre.</span></span> <span data-ttu-id="01776-294">また、設定`searchTerm`(可能性のある何も行われません)、検索ボックスに渡されたオブジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="01776-294">It also sets `searchTerm` to whatever was passed for the search box (which might be nothing).</span></span>

<span data-ttu-id="01776-295">ステートメントが、SQL に関係なく`selectCommand`、し、呼び出すコード`db.Query`のクエリを実行するには渡す任意`searchTerm`です。</span><span class="sxs-lookup"><span data-stu-id="01776-295">Regardless of which SQL statement is in `selectCommand`, the code then calls `db.Query` to run the query, passing it whatever is in `searchTerm`.</span></span> <span data-ttu-id="01776-296">ある場合は nothing`searchTerm`にかかわらず、その場合に値を渡すためのパラメーターがないため`selectCommand`します。</span><span class="sxs-lookup"><span data-stu-id="01776-296">If there's nothing in `searchTerm`, it doesn't matter, because in that case there's no parameter to pass the value to `selectCommand` anyway.</span></span>

<span data-ttu-id="01776-297">コードの最後に、初期化、`WebGrid`と同じようにする前に、クエリの結果を使用して、ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="01776-297">Finally, the code initializes the `WebGrid` helper by using the query results, just like before.</span></span>

<span data-ttu-id="01776-298">SQL ステートメントを記述することによって表示することができ、変数に、検索用語と、柔軟性、コードに追加しました。</span><span class="sxs-lookup"><span data-stu-id="01776-298">You can see that by putting the SQL statement and the search term into variables, you've added flexibility to the code.</span></span> <span data-ttu-id="01776-299">このチュートリアルで後述するようこの基本的なフレームワークを使用してさまざまな種類の検索ロジックを追加し続けます。</span><span class="sxs-lookup"><span data-stu-id="01776-299">As you'll see later in this tutorial, you can use this basic framework and keep adding logic for different types of searches.</span></span>

## <a name="testing-the-search-by-genre-feature"></a><span data-ttu-id="01776-300">検索-ジャンルで機能のテスト</span><span class="sxs-lookup"><span data-stu-id="01776-300">Testing the Search-by-Genre Feature</span></span>

<span data-ttu-id="01776-301">WebMatrix では、実行、 *Movies.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="01776-301">In WebMatrix, run the *Movies.cshtml* page.</span></span> <span data-ttu-id="01776-302">ジャンルのテキスト ボックスを使用してページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="01776-302">You see the page with the text box for genre.</span></span>

<span data-ttu-id="01776-303">入力 をクリックして、テスト レコードの 1 つの入力をジャンル**検索**です。</span><span class="sxs-lookup"><span data-stu-id="01776-303">Enter a genre that you've entered for one of your test records, then click **Search**.</span></span> <span data-ttu-id="01776-304">ジャンルに一致する映画のみの一覧を表示するこの時間。</span><span class="sxs-lookup"><span data-stu-id="01776-304">This time you see a listing of just the movies that match that genre:</span></span>

![ジャンル 'Comedies' の検索後を一覧表示するビデオ ページ](form-basics/_static/image4.png)

<span data-ttu-id="01776-306">異なるジャンルを入力し、もう一度検索します。</span><span class="sxs-lookup"><span data-stu-id="01776-306">Enter a different genre and search again.</span></span> <span data-ttu-id="01776-307">検索では大文字小文字を区別しないことを認識できるように、すべての文字は小文字または大文字すべてを使用して、ジャンルを入力してください。</span><span class="sxs-lookup"><span data-stu-id="01776-307">Try entering the genre by using all lowercase or all uppercase letters so that you can see that the search is not case sensitive.</span></span>

## <a name="remembering-what-the-user-entered"></a><span data-ttu-id="01776-308">ユーザーが入力した「記憶」</span><span class="sxs-lookup"><span data-stu-id="01776-308">"Remembering" What the User Entered</span></span>

<span data-ttu-id="01776-309">お気付きをジャンルを入力してクリック、した後に**検索ジャンル**ジャンルの一覧を確認しました。</span><span class="sxs-lookup"><span data-stu-id="01776-309">You might have noticed that after you entered a genre and clicked **Search Genre**, you saw a listing for that genre.</span></span> <span data-ttu-id="01776-310">ただし、検索テキスト ボックスが空でした&mdash;言い換えれば、その覚えのない入力しています。</span><span class="sxs-lookup"><span data-stu-id="01776-310">However, the search text box was empty &mdash; in other words, it didn't remember what you'd entered.</span></span>

<span data-ttu-id="01776-311">この現象が発生した理由を理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="01776-311">It's important to understand why this behavior occurs.</span></span> <span data-ttu-id="01776-312">ページを送信するときに、ブラウザーは、web サーバーに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="01776-312">When you submit a page, the browser sends a request to the web server.</span></span> <span data-ttu-id="01776-313">ASP.NET では、要求を取得が、ページの新しいインスタンスを作成、コードが実行され、ブラウザーにページをもう一度表示し、します。</span><span class="sxs-lookup"><span data-stu-id="01776-313">When ASP.NET gets the request, it creates a brand-new instance of the page, runs the code in it, and then renders the page to the browser again.</span></span> <span data-ttu-id="01776-314">実際には、ただし、ページが知りません自体の以前のバージョンでの操作だけがします。</span><span class="sxs-lookup"><span data-stu-id="01776-314">In effect, though, the page doesn't know that you were just working with a previous version of itself.</span></span> <span data-ttu-id="01776-315">いくつかのある要求を受け取りましたが認識しているすべては、これでデータを形成します。</span><span class="sxs-lookup"><span data-stu-id="01776-315">All it knows is that it got a request that had some form data in it.</span></span>

<span data-ttu-id="01776-316">ページを要求するたびに&mdash;か、最初に送信することによって&mdash;新しいページを取得します。</span><span class="sxs-lookup"><span data-stu-id="01776-316">Every time you request a page &mdash; whether for the first time or by submitting it &mdash; you're getting a new page.</span></span> <span data-ttu-id="01776-317">Web サーバーには、最後の要求のメモリがありません。</span><span class="sxs-lookup"><span data-stu-id="01776-317">The web server has no memory of your last request.</span></span> <span data-ttu-id="01776-318">ASP.NET、どちらもし、ブラウザーはどちらもします。</span><span class="sxs-lookup"><span data-stu-id="01776-318">Neither does ASP.NET, and neither does the browser.</span></span> <span data-ttu-id="01776-319">これらのページの個別のインスタンス間で唯一の接続は、それらの間に送信するすべてのデータです。</span><span class="sxs-lookup"><span data-stu-id="01776-319">The only connection between these separate instances of the page is any data that you transmit between them.</span></span> <span data-ttu-id="01776-320">ページを送信した場合など、新しいページ インスタンスは、以前のインスタンスから送信されたフォーム データを取得できます。</span><span class="sxs-lookup"><span data-stu-id="01776-320">If you submit a page, for example, the new page instance can get the form data that was sent by the earlier instance.</span></span> <span data-ttu-id="01776-321">(ページ間でデータをやりとりする別の方法は、cookie を使用するのには) です。</span><span class="sxs-lookup"><span data-stu-id="01776-321">(Another way to pass data between pages is to use cookies.)</span></span>

<span data-ttu-id="01776-322">Web ページがあるという、このような状況を記述する正式な方法では*ステートレス*です。</span><span class="sxs-lookup"><span data-stu-id="01776-322">A formal way to describe this situation is to say that web pages are *stateless*.</span></span> <span data-ttu-id="01776-323">Web サーバーおよびページ自体と、ページ内の要素では、ページの直前の状態に関する情報は保持されません。</span><span class="sxs-lookup"><span data-stu-id="01776-323">Web servers and the pages themselves and the elements in the page do not maintain any information about the previous state of a page.</span></span> <span data-ttu-id="01776-324">Web は、個々 の要求の状態を維持するが迅速にリソースを使い果たす、web サーバーは、多くの場合、おそらく何千を処理の 1 秒あたりの要求の数十万もあるために、この方法を設計されました。</span><span class="sxs-lookup"><span data-stu-id="01776-324">The web was designed this way because maintaining state for individual requests would quickly exhaust the resources of web servers, which often handle thousands, maybe even hundreds of thousands, of requests per second.</span></span>

<span data-ttu-id="01776-325">その理由になるよう、テキスト ボックスが空でした。</span><span class="sxs-lookup"><span data-stu-id="01776-325">So that's why the text box was empty.</span></span> <span data-ttu-id="01776-326">ページを送信した後、ASP.NET は、ページの新しいインスタンスを作成し、コードとマークアップを実行します。</span><span class="sxs-lookup"><span data-stu-id="01776-326">After you submitted the page, ASP.NET created a new instance of the page and ran through the code and markup.</span></span> <span data-ttu-id="01776-327">Nothing にありましたを ASP.NET、テキスト ボックスに値を格納するように通知するコード。</span><span class="sxs-lookup"><span data-stu-id="01776-327">There was nothing in that code that told ASP.NET to put a value into the text box.</span></span> <span data-ttu-id="01776-328">ASP.NET 何も出力しないとに値を指定せずにテキスト ボックスが表示されました。</span><span class="sxs-lookup"><span data-stu-id="01776-328">So ASP.NET didn't do anything, and the text box was rendered without a value in it.</span></span>

<span data-ttu-id="01776-329">この問題を回避するための簡単な方法実際には</span><span class="sxs-lookup"><span data-stu-id="01776-329">There's actually an easy way to get around this issue.</span></span> <span data-ttu-id="01776-330">テキスト ボックスに入力したジャンル*は*コードで使用可能な&mdash;内にある`Request.QueryString["searchGenre"]`です。</span><span class="sxs-lookup"><span data-stu-id="01776-330">The genre that you entered into the text box *is* available to you in code &mdash; it's in `Request.QueryString["searchGenre"]`.</span></span>

<span data-ttu-id="01776-331">テキスト ボックスのマークアップを更新できるように、`value`属性から値を取得する`searchTerm`などの次の例。</span><span class="sxs-lookup"><span data-stu-id="01776-331">Update the markup for the text box so that the `value` attribute gets its value from `searchTerm`, like this example:</span></span>

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

<span data-ttu-id="01776-332">このページで設定することがも、`value`属性を`searchTerm`その変数にも、ジャンルが含まれているので、変数を入力します。</span><span class="sxs-lookup"><span data-stu-id="01776-332">In this page, you could have also set the `value` attribute to the `searchTerm` variable, since that variable also contains the genre you entered.</span></span> <span data-ttu-id="01776-333">使用して、`Request`を設定するオブジェクト、`value`属性ようこのタスクを実行する標準的な方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="01776-333">But using the `Request` object to set the `value` attribute as shown here is the standard way to accomplish this task.</span></span> <span data-ttu-id="01776-334">(とした場合でもこれを行う&mdash;状況によっては、ページを表示するために必要な可能性があります*せず*フィールドの値。</span><span class="sxs-lookup"><span data-stu-id="01776-334">(Assuming you even want to do this &mdash; in some situations, you might want to render the page *without* values in the fields.</span></span> <span data-ttu-id="01776-335">すべてによって異なります、アプリで何が起こっています。)</span><span class="sxs-lookup"><span data-stu-id="01776-335">It all depends on what's going on with your app.)</span></span>

> [!NOTE]
> <span data-ttu-id="01776-336">「思い出せない」パスワードに使用されるテキスト ボックスの値。</span><span class="sxs-lookup"><span data-stu-id="01776-336">You can't "remember" the value of a text box that's used for passwords.</span></span> <span data-ttu-id="01776-337">コードを使用して、[パスワード] フィールドに入力できるようにするセキュリティ ホールがあります。</span><span class="sxs-lookup"><span data-stu-id="01776-337">It would be a security hole to allow people to fill in a password field by using code.</span></span>


<span data-ttu-id="01776-338">ページを再度実行し、ジャンルを入力し、クリックして**検索ジャンル**です。</span><span class="sxs-lookup"><span data-stu-id="01776-338">Run the page again, enter a genre, and click **Search Genre**.</span></span> <span data-ttu-id="01776-339">この時間だけでなく操作を確認する、検索の結果が最後に入力したテキスト ボックスの記憶。</span><span class="sxs-lookup"><span data-stu-id="01776-339">This time not only do you see the results of the search, but the text box remembers what you entered last time:</span></span>

![テキスト ボックスが '記憶こと' 前のエントリを表示するページ](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a><span data-ttu-id="01776-341">任意の単語でタイトルの検索</span><span class="sxs-lookup"><span data-stu-id="01776-341">Searching for Any Word in the Title</span></span>

<span data-ttu-id="01776-342">ジャンル、今すぐ検索することができますが、タイトルを検索する場合がありますもします。</span><span class="sxs-lookup"><span data-stu-id="01776-342">You can now search for any genre, but you might also want to search for a title.</span></span> <span data-ttu-id="01776-343">検索すると、そのため、タイトル内任意の場所に表示される語句を検索することができます、タイトルを正確に取得するには、困難です。</span><span class="sxs-lookup"><span data-stu-id="01776-343">It's hard to get a title exactly right when you search, so instead you can search for a word that appears anywhere inside a title.</span></span> <span data-ttu-id="01776-344">そのためには SQL で使用する、`LIKE`と同様に、次の演算子と構文。</span><span class="sxs-lookup"><span data-stu-id="01776-344">To do that in SQL, you use the `LIKE` operator and syntax like the following:</span></span>

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

<span data-ttu-id="01776-345">このコマンドは、タイトルが"adventure"を含むすべてのムービーを取得します。</span><span class="sxs-lookup"><span data-stu-id="01776-345">This command gets all the movies whose titles contain "adventure".</span></span> <span data-ttu-id="01776-346">使用すると、`LIKE`演算子、ワイルドカード文字を含める`%`検索用語の一部として。</span><span class="sxs-lookup"><span data-stu-id="01776-346">When you use the `LIKE` operator, you include the wildcard character `%` as part of the search term.</span></span> <span data-ttu-id="01776-347">検索`LIKE 'adventure%'`「'adventure' で始まる」ことを意味します。</span><span class="sxs-lookup"><span data-stu-id="01776-347">The search `LIKE 'adventure%'` means "starting with 'adventure'".</span></span> <span data-ttu-id="01776-348">(技術的には、そのという意味の「、文字列 'adventure' が続くものです」)同様に、特定の語句`LIKE '%adventure'`「'adventure' で終了するまで」に別の方法が「何も後に文字列 'adventure'」を意味します。</span><span class="sxs-lookup"><span data-stu-id="01776-348">(Technically, it means "The string 'adventure' followed by anything.") Similarly, the search term `LIKE '%adventure'` means "anything followed by the string 'adventure'", which is another way to say "ending with 'adventure'".</span></span>

<span data-ttu-id="01776-349">検索用語`LIKE '%adventure%'`のためことを意味します""adventure'、タイトルに任意の場所です"。</span><span class="sxs-lookup"><span data-stu-id="01776-349">The search term `LIKE '%adventure%'` therefore means "with 'adventure' anywhere in the title."</span></span> <span data-ttu-id="01776-350">(技術的には、「では何も 'adventure'、何も後に続く、タイトルです。」)</span><span class="sxs-lookup"><span data-stu-id="01776-350">(Technically, "anything in the title, followed by 'adventure', followed by anything.")</span></span>

<span data-ttu-id="01776-351">内部、`<form>`要素、終わりの右下にある次のマークアップを追加`</div>`ジャンル検索用のタグ (閉じる`</form>`要素)。</span><span class="sxs-lookup"><span data-stu-id="01776-351">Inside the `<form>` element, add the following markup right under the closing `</div>` tag for the genre search (just before the closing `</form>` element):</span></span>

[!code-html[Main](form-basics/samples/sample10.html)]

<span data-ttu-id="01776-352">この検索を処理するコード似ていますが、ジャンル検索用のコードにアセンブルする必要がある、`LIKE`検索します。</span><span class="sxs-lookup"><span data-stu-id="01776-352">The code to handle this search is similar to the code for the genre search, except that you have to assemble the `LIKE` search.</span></span> <span data-ttu-id="01776-353">ページの上部にあるコード ブロックの内部には、これを追加`if`直後ブロック、`if`ジャンル検索のブロック。</span><span class="sxs-lookup"><span data-stu-id="01776-353">Inside the code block at the top of the page, add this `if` block just after the `if` block for the genre search:</span></span>

[!code-csharp[Main](form-basics/samples/sample11.cs)]

<span data-ttu-id="01776-354">検索を使用する点を除いて、このコードが、先ほど見た同じロジックを使用して、`LIKE`演算子とコードの配置"`%`"検索用語の前後にします。</span><span class="sxs-lookup"><span data-stu-id="01776-354">This code uses the same logic you saw earlier, except that the search uses a `LIKE` operator and the code puts "`%`" before and after the search term.</span></span>

<span data-ttu-id="01776-355">どのように簡単に別の検索をページに追加することを確認します。</span><span class="sxs-lookup"><span data-stu-id="01776-355">Notice how it was easy to add another search to the page.</span></span> <span data-ttu-id="01776-356">行う必要があるすべてが発生しました。</span><span class="sxs-lookup"><span data-stu-id="01776-356">All you had to do was:</span></span>

- <span data-ttu-id="01776-357">作成、`if`関連する検索ボックスに、値があったかどうかを確認するためにテストをブロックします。</span><span class="sxs-lookup"><span data-stu-id="01776-357">Create an `if` block that tested to see whether the relevant search box had a value.</span></span>
- <span data-ttu-id="01776-358">設定、`selectCommand`変数を新しい SQL ステートメントにします。</span><span class="sxs-lookup"><span data-stu-id="01776-358">Set the `selectCommand` variable to a new SQL statement.</span></span>
- <span data-ttu-id="01776-359">設定、`searchTerm`変数をクエリに渡す値にします。</span><span class="sxs-lookup"><span data-stu-id="01776-359">Set the `searchTerm` variable to the value to pass to the query.</span></span>

<span data-ttu-id="01776-360">新しいタイトルの検索ロジックを含む完全なコード ブロックを次に示します。</span><span class="sxs-lookup"><span data-stu-id="01776-360">Here's the complete code block, which contains the new logic for a title search:</span></span>

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

<span data-ttu-id="01776-361">このコードの実行内容の概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="01776-361">Here's a summary of what this code does:</span></span>

- <span data-ttu-id="01776-362">変数`searchTerm`と`selectCommand`上部に初期化されます。</span><span class="sxs-lookup"><span data-stu-id="01776-362">The variables `searchTerm` and `selectCommand` are initialized at the top.</span></span> <span data-ttu-id="01776-363">適切な SQL コマンドが、ユーザーがページではに基づいて、(存在する場合) の適切な検索用語にこれらの変数を設定しようとしているとします。</span><span class="sxs-lookup"><span data-stu-id="01776-363">You're going to set these variables to the appropriate search term (if any) and appropriate SQL command based on what the user does in the page.</span></span> <span data-ttu-id="01776-364">既定の検索は、単純なケースをデータベースからすべてのムービーを取得することです。</span><span class="sxs-lookup"><span data-stu-id="01776-364">The default search is the simple case of getting all the movies from the database.</span></span>
- <span data-ttu-id="01776-365">テストで`searchGenre`と`searchTitle`、コード セット`searchTerm`を検索する値。</span><span class="sxs-lookup"><span data-stu-id="01776-365">In the tests for `searchGenre` and `searchTitle`, the code sets `searchTerm` to the value you want to search for.</span></span> <span data-ttu-id="01776-366">それらのコード ブロックの設定も`selectCommand`検索条件に該当する SQL コマンド。</span><span class="sxs-lookup"><span data-stu-id="01776-366">Those code blocks also set `selectCommand` to an appropriate SQL command for that search.</span></span>
- <span data-ttu-id="01776-367">`db.Query`メソッドが呼び出される 1 回だけでは、どのような SQL コマンドを使用して`selectedCommand`であり、どのような値で`searchTerm`です。</span><span class="sxs-lookup"><span data-stu-id="01776-367">The `db.Query` method is invoked only once, using whatever SQL command is in `selectedCommand` and whatever value is in `searchTerm`.</span></span> <span data-ttu-id="01776-368">検索語句 (ありませんジャンルおよびタイトル単語を指定しないで) が存在しない場合の値`searchTerm`空の文字列します。</span><span class="sxs-lookup"><span data-stu-id="01776-368">If there is no search term (no genre and no title word), the value of `searchTerm` is an empty string.</span></span> <span data-ttu-id="01776-369">ただし、重要ではない、クエリにパラメーターを必要としない場合はのでです。</span><span class="sxs-lookup"><span data-stu-id="01776-369">However, that doesn't matter, because in that case the query doesn't require a parameter.</span></span>

## <a name="testing-the-title-search-feature"></a><span data-ttu-id="01776-370">タイトルの検索機能をテストします。</span><span class="sxs-lookup"><span data-stu-id="01776-370">Testing the Title Search Feature</span></span>

<span data-ttu-id="01776-371">これで、検索が完了しました ページをテストできます。</span><span class="sxs-lookup"><span data-stu-id="01776-371">Now you can test your completed search page.</span></span> <span data-ttu-id="01776-372">実行*Movies.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="01776-372">Run *Movies.cshtml*.</span></span>

<span data-ttu-id="01776-373">ジャンルを入力し、クリックして**検索ジャンル**です。</span><span class="sxs-lookup"><span data-stu-id="01776-373">Enter a genre and click **Search Genre**.</span></span> <span data-ttu-id="01776-374">グリッドには、前のように、ジャンルのムービーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="01776-374">The grid displays movies of that genre, like before.</span></span>

<span data-ttu-id="01776-375">単語のタイトルを入力し、をクリックして**検索タイトル**です。</span><span class="sxs-lookup"><span data-stu-id="01776-375">Enter a title word and click **Search Title**.</span></span> <span data-ttu-id="01776-376">グリッドには、映画のタイトルにその単語が表示されます。</span><span class="sxs-lookup"><span data-stu-id="01776-376">The grid displays movies that have that word in the title.</span></span>

![映画ページのタイトルに '、' の検索後を一覧表示します。](form-basics/_static/image6.png)

<span data-ttu-id="01776-378">両方のテキスト ボックスを空白のままにし、いずれかのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="01776-378">Leave both text boxes blank and click either button.</span></span> <span data-ttu-id="01776-379">グリッドには、すべてのムービーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="01776-379">The grid displays all the movies.</span></span>

## <a name="combining-the-queries"></a><span data-ttu-id="01776-380">クエリの結合</span><span class="sxs-lookup"><span data-stu-id="01776-380">Combining the Queries</span></span>

<span data-ttu-id="01776-381">検索を行うことができますが排他的なことに注意してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="01776-381">You might notice that the searches you can perform are exclusive.</span></span> <span data-ttu-id="01776-382">タイトル、ジャンルは両方の検索ボックスに値がある場合でも、同時に検索することはできません。</span><span class="sxs-lookup"><span data-stu-id="01776-382">You can't search the title and the genre at the same time, even if both search boxes have values in them.</span></span> <span data-ttu-id="01776-383">たとえば、タイトルには、"Adventure"が含まれています。 すべてのアクション映画を検索できません。</span><span class="sxs-lookup"><span data-stu-id="01776-383">For example, you can't search for all action movies whose title contains "Adventure".</span></span> <span data-ttu-id="01776-384">(ように、ページは、ジャンルとタイトルの両方の値を入力する場合、コード化された、タイトルの検索を優先順位)条件を結合する検索を作成するには、構文は次のように SQL クエリを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="01776-384">(As the page is coded now, if you enter values for both genre and title, the title search gets precedence.) To create a search that combines the conditions, you would have to create a SQL query that has syntax like the following:</span></span>

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

<span data-ttu-id="01776-385">次のようにステートメントを使用してクエリを実行することが必要 (大まかに言うと)。</span><span class="sxs-lookup"><span data-stu-id="01776-385">And you'd have to run the query by using a statement like the following (roughly speaking):</span></span>

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

<span data-ttu-id="01776-386">検索条件の順列の多くを許可するロジックを作成するを取得できますをビットに関係することがわかります。</span><span class="sxs-lookup"><span data-stu-id="01776-386">Creating logic to allow many permutations of search criteria can get a bit involved, as you can see.</span></span> <span data-ttu-id="01776-387">そのため、私たちが進まないでください。</span><span class="sxs-lookup"><span data-stu-id="01776-387">Therefore, we'll stop here.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="01776-388">次へ直近の見通し</span><span class="sxs-lookup"><span data-stu-id="01776-388">Coming Up Next</span></span>

<span data-ttu-id="01776-389">次のチュートリアルでは、ユーザーがデータベースにムービーを追加できるようにするフォームを使用するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="01776-389">In the next tutorial, you'll create a page that uses a form to let users add movies to the database.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-search"></a><span data-ttu-id="01776-390">(検索を使用して更新された) ビデオ ページの完全な一覧</span><span class="sxs-lookup"><span data-stu-id="01776-390">Complete Listing for Movie Page (Updated with Search)</span></span>

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="01776-391">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="01776-391">Additional Resources</span></span>

- [<span data-ttu-id="01776-392">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="01776-392">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)
- <span data-ttu-id="01776-393">[SQL の WHERE 句](http://www.w3schools.com/sql/sql_where.asp)W3Schools サイト</span><span class="sxs-lookup"><span data-stu-id="01776-393">[SQL WHERE Clause](http://www.w3schools.com/sql/sql_where.asp) on the W3Schools site</span></span>
- <span data-ttu-id="01776-394">[メソッドの定義](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)W3C サイトの記事</span><span class="sxs-lookup"><span data-stu-id="01776-394">[Method Definitions](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article on the W3C site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01776-395">[前へ](displaying-data.md)
> [次へ](entering-data.md)</span><span class="sxs-lookup"><span data-stu-id="01776-395">[Previous](displaying-data.md)
[Next](entering-data.md)</span></span>
