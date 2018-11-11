---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: ASP.NET Web Pages (Razor) サイトでの HTML フォームの操作 |Microsoft Docs
author: Rick-Anderson
description: フォームは、テキスト ボックス、チェック ボックス、ラジオ ボタン、およびプルダウン リストなどのユーザー入力コントロールを配置する HTML ドキュメントのセクションです。 フォームを使用する値を下回った場合.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: de700055168f9d17167c82afe836b546160c6e91
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021616"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="031f3-104">ASP.NET Web Pages (Razor) サイトでの HTML フォームの操作</span><span class="sxs-lookup"><span data-stu-id="031f3-104">Working with HTML Forms in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="031f3-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="031f3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="031f3-106">この記事では、(テキスト ボックスとボタン) を持つ、HTML フォームを処理する方法を説明します。 ASP.NET Web Pages (Razor) の web サイトで作業している場合。</span><span class="sxs-lookup"><span data-stu-id="031f3-106">This article describes how to process an HTML form (with text boxes and buttons) when you are working in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="031f3-107">**学習内容。**</span><span class="sxs-lookup"><span data-stu-id="031f3-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="031f3-108">HTML フォームを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="031f3-108">How to create an HTML form.</span></span>
> - <span data-ttu-id="031f3-109">フォームからユーザーの入力を読み取る方法。</span><span class="sxs-lookup"><span data-stu-id="031f3-109">How to read user input from the form.</span></span>
> - <span data-ttu-id="031f3-110">ユーザー入力を検証する方法。</span><span class="sxs-lookup"><span data-stu-id="031f3-110">How to validate user input.</span></span>
> - <span data-ttu-id="031f3-111">ページが送信された後に、フォームの値を復元する方法。</span><span class="sxs-lookup"><span data-stu-id="031f3-111">How to restore form values after the page is submitted.</span></span>
> 
> <span data-ttu-id="031f3-112">これらは、プログラミングの概念は、情報の記事で導入された ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="031f3-112">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="031f3-113">`Request` オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="031f3-113">The `Request` object.</span></span>
> - <span data-ttu-id="031f3-114">入力の検証。</span><span class="sxs-lookup"><span data-stu-id="031f3-114">Input validation.</span></span>
> - <span data-ttu-id="031f3-115">HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="031f3-115">HTML encoding.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="031f3-116">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="031f3-116">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="031f3-117">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="031f3-117">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="031f3-118">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="031f3-118">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="creating-a-simple-html-form"></a><span data-ttu-id="031f3-119">単純な HTML フォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="031f3-119">Creating a Simple HTML Form</span></span>

1. <span data-ttu-id="031f3-120">新しい web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="031f3-120">Create a new website.</span></span>
2. <span data-ttu-id="031f3-121">という名前の web ページを作成、ルート フォルダーで*Form.cshtml*し、次のマークアップを入力します。</span><span class="sxs-lookup"><span data-stu-id="031f3-121">In the root folder, create a web page named *Form.cshtml* and enter the following markup:</span></span>

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. <span data-ttu-id="031f3-122">お使いのブラウザーでページを起動します。</span><span class="sxs-lookup"><span data-stu-id="031f3-122">Launch the page in your browser.</span></span> <span data-ttu-id="031f3-123">(WebMatrix で、**ファイル**] ワークスペースで、ファイルを右クリックし、[**ブラウザーで起動**)。次の 3 つの入力フィールドを持つ単純なフォームと**送信**ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="031f3-123">(In WebMatrix, in the **Files** workspace, right-click the file and then select **Launch in browser**.) A simple form with three input fields and a **Submit** button is displayed.</span></span>

    ![次の 3 つのテキスト ボックスをフォームのスクリーン ショット。](4-working-with-forms/_static/image1.jpg)

    <span data-ttu-id="031f3-125">クリックした場合、この時点で、**送信**ボタンを何も起こりません。</span><span class="sxs-lookup"><span data-stu-id="031f3-125">At this point, if you click the **Submit** button, nothing happens.</span></span> <span data-ttu-id="031f3-126">形式を有効に活用するには、サーバーで実行されるいくつかのコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="031f3-126">To make the form useful, you have to add some code that will run on the server.</span></span>

## <a name="reading-user-input-from-the-form"></a><span data-ttu-id="031f3-127">フォームからユーザー入力を読み取る</span><span class="sxs-lookup"><span data-stu-id="031f3-127">Reading User Input from the Form</span></span>

<span data-ttu-id="031f3-128">フォームを処理するには、送信されたフィールドの値を読み取り、何らかの処理はコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="031f3-128">To process the form, you add code that reads the submitted field values and does something with them.</span></span> <span data-ttu-id="031f3-129">この手順では、フィールドを読み取り、ページで、ユーザー入力を表示する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="031f3-129">This procedure shows you how to read the fields and display the user input on the page.</span></span> <span data-ttu-id="031f3-130">(実稼働アプリケーションで一般的に処理を行うより興味深いユーザー入力します。</span><span class="sxs-lookup"><span data-stu-id="031f3-130">(In a production application, you generally do more interesting things with user input.</span></span> <span data-ttu-id="031f3-131">行いますでデータベース操作に関する記事。)</span><span class="sxs-lookup"><span data-stu-id="031f3-131">You'll do that in the article about working with databases.)</span></span>

1. <span data-ttu-id="031f3-132">上部にある、 *Form.cshtml*ファイルに、次のコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="031f3-132">At the top of the *Form.cshtml* file, enter the following code:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    <span data-ttu-id="031f3-133">ユーザーは、まず、ページを要求するときに、空のフォームのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="031f3-133">When the user first requests the page, only the empty form is displayed.</span></span> <span data-ttu-id="031f3-134">(することになります) ユーザーが、フォームに入力し、クリックし、**送信**します。</span><span class="sxs-lookup"><span data-stu-id="031f3-134">The user (which will be you) fills in the form and then clicks **Submit**.</span></span> <span data-ttu-id="031f3-135">(投稿) を送信するこのサーバーへのユーザー入力します。</span><span class="sxs-lookup"><span data-stu-id="031f3-135">This submits (posts) the user input to the server.</span></span> <span data-ttu-id="031f3-136">既定では、要求が同じページに進みます (つまり、 *Form.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="031f3-136">By default, the request goes to the same page (namely, *Form.cshtml*).</span></span>

    <span data-ttu-id="031f3-137">この時点で、ページを送信するときに入力した値が、フォームのすぐ上に表示されます。</span><span class="sxs-lookup"><span data-stu-id="031f3-137">When you submit the page this time, the values you entered are displayed just above the form:</span></span>

    ![ページに表示される入力した値を示すスクリーン ショット。](4-working-with-forms/_static/image2.jpg)

    <span data-ttu-id="031f3-139">ページのコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="031f3-139">Look at the code for the page.</span></span> <span data-ttu-id="031f3-140">最初に使用する、`IsPost`ページがポストされているかどうかを判断するメソッド&#8212;、かどうかをユーザーがクリックした、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="031f3-140">You first use the `IsPost` method to determine whether the page is being posted &#8212; that is, whether a user clicked the **Submit** button.</span></span> <span data-ttu-id="031f3-141">場合は、投稿`IsPost`true を返します。</span><span class="sxs-lookup"><span data-stu-id="031f3-141">If this is a post, `IsPost` returns true.</span></span> <span data-ttu-id="031f3-142">これは、最初の要求 (GET 要求)、またはポストバック (POST 要求) を扱うかどうかを判断する標準的な方法では、ASP.NET Web ページです。</span><span class="sxs-lookup"><span data-stu-id="031f3-142">This is the standard way in ASP.NET Web Pages to determine whether you're working with an initial request (a GET request) or a postback (a POST request).</span></span> <span data-ttu-id="031f3-143">(詳細については、GET と POST を参照してください「HTTP GET と POST と IsPost プロパティの」で[ASP.NET Web ページは、Razor 構文を使用プログラミングの概要について](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost))。</span><span class="sxs-lookup"><span data-stu-id="031f3-143">(For more information about GET and POST, see the sidebar "HTTP GET and POST and the IsPost Property" in [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)</span></span>

    <span data-ttu-id="031f3-144">次に、ユーザーから入力される値を取得、`Request.Form`オブジェクト、およびを置く変数に後でします。</span><span class="sxs-lookup"><span data-stu-id="031f3-144">Next, you get the values that the user filled in from the `Request.Form` object, and you put them in variables for later.</span></span> <span data-ttu-id="031f3-145">`Request.Form`オブジェクトには、キーによって識別される各ページに送信されたすべての値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="031f3-145">The `Request.Form` object contains all the values that were submitted with the page, each identified by a key.</span></span> <span data-ttu-id="031f3-146">キーと等価が、`name`読みたいフォーム フィールドの属性です。</span><span class="sxs-lookup"><span data-stu-id="031f3-146">The key is the equivalent to the `name` attribute of the form field that you want to read.</span></span> <span data-ttu-id="031f3-147">たとえば、読み取り、`companyname`フィールド (テキスト ボックス) を使用する`Request.Form["companyname"]`します。</span><span class="sxs-lookup"><span data-stu-id="031f3-147">For example, to read the `companyname` field (text box), you use `Request.Form["companyname"]`.</span></span>

    <span data-ttu-id="031f3-148">フォームの値が格納されている、`Request.Form`文字列としてのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="031f3-148">Form values are stored in the `Request.Form` object as strings.</span></span> <span data-ttu-id="031f3-149">そのため、数値または日付、またはその他の種類として値を操作するときに、文字列からその型に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="031f3-149">Therefore, when you have to work with a value as a number or a date or some other type, you have to convert it from a string to that type.</span></span> <span data-ttu-id="031f3-150">例では、`AsInt`のメソッド、 `Request.Form` (を従業員数を含む) の従業員のフィールドの値を整数に変換するために使用します。</span><span class="sxs-lookup"><span data-stu-id="031f3-150">In the example, the `AsInt` method of the `Request.Form` is used to convert the value of the employees field (which contains an employee count) to an integer.</span></span>
2. <span data-ttu-id="031f3-151">お使いのブラウザーでページを起動し、フォームのフィールドを入力し、をクリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="031f3-151">Launch the page in your browser, fill in the form fields, and click **Submit**.</span></span> <span data-ttu-id="031f3-152">ページには、入力した値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="031f3-152">The page displays the values you entered.</span></span>

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a><span data-ttu-id="031f3-153">外観とセキュリティに HTML エンコード</span><span class="sxs-lookup"><span data-stu-id="031f3-153">HTML Encoding for Appearance and Security</span></span>
> 
> <span data-ttu-id="031f3-154">HTML のような文字の特殊な用途を持つ`<`、 `>`、および`&`します。</span><span class="sxs-lookup"><span data-stu-id="031f3-154">HTML has special uses for characters like `<`, `>`, and `&`.</span></span> <span data-ttu-id="031f3-155">これらの特殊文字が表示されませんに期待している場合、web ページの機能と外観を欠きますができます。</span><span class="sxs-lookup"><span data-stu-id="031f3-155">If these special characters appear where they're not expected, they can ruin the appearance and functionality of your web page.</span></span> <span data-ttu-id="031f3-156">たとえば、ブラウザーが解釈、`<`など HTML 要素の先頭として (ない場合に空白が続いて) を文字`<b>`または`<input ...>`します。</span><span class="sxs-lookup"><span data-stu-id="031f3-156">For example, the browser interprets the `<` character (unless it's followed by a space) as the beginning of an HTML element, like `<b>` or `<input ...>`.</span></span> <span data-ttu-id="031f3-157">始まる文字列を単に破棄、ブラウザーでは、要素を認識しない場合`<`再度認識に達するまで何かです。</span><span class="sxs-lookup"><span data-stu-id="031f3-157">If the browser doesn't recognize the element, it simply discards the string that begins with `<` until it reaches something that it again recognizes.</span></span> <span data-ttu-id="031f3-158">当然ながら、いくつかの奇妙なレンダリング ページでこのがあります。</span><span class="sxs-lookup"><span data-stu-id="031f3-158">Obviously, this can result in some weird rendering in the page.</span></span>
> 
> <span data-ttu-id="031f3-159">HTML エンコードすると、ブラウザーが正しいシンボルとして解釈するコードでこれらの予約済み文字が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="031f3-159">HTML encoding replaces these reserved characters with a code that browsers interpret as the correct symbol.</span></span> <span data-ttu-id="031f3-160">たとえば、`<`文字で置換されます`&lt;`と`>`文字が置き換え`&gt;`。</span><span class="sxs-lookup"><span data-stu-id="031f3-160">For example, the `<` character is replaced with `&lt;` and the `>` character is replaced with `&gt;`.</span></span> <span data-ttu-id="031f3-161">ブラウザーでは、表示文字としてこれらの置換文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="031f3-161">The browser renders these replacement strings as the characters that you want to see.</span></span>
> 
> <span data-ttu-id="031f3-162">HTML エンコード文字列を表示する任意の時間を使用する (入力)、ユーザーから取得したことをお勧めすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="031f3-162">It's a good idea to use HTML encoding any time you display strings (input) that you got from a user.</span></span> <span data-ttu-id="031f3-163">ない場合は、ユーザーできる悪意のあるスクリプトを実行するか、別のものを web ページ、サイトのセキュリティを侵害するか、望ましくないだけであるを取得しようとします。</span><span class="sxs-lookup"><span data-stu-id="031f3-163">If you don't, a user can try to get your web page to run a malicious script or do something else that compromises your site security or that's just not what you intend.</span></span> <span data-ttu-id="031f3-164">(これは、特に重要な場合はユーザー入力を取得、別の場所を格納し、それを後で表示&#8212;ブログ コメント、ユーザーのレビュー、またはそのようなものとしてなど)。</span><span class="sxs-lookup"><span data-stu-id="031f3-164">(This is particularly important if you take user input, store it someplace, and then display it later &#8212; for example, as a blog comment, user review, or something like that.)</span></span>
> 
> <span data-ttu-id="031f3-165">自動的に ASP.NET Web Pages のこれらの問題を防ぐため HTML エンコード任意のテキスト コンテンツをコードから出力します。</span><span class="sxs-lookup"><span data-stu-id="031f3-165">To help prevent these problems, ASP.NET Web Pages automatically HTML-encodes any text content that you output from your code.</span></span> <span data-ttu-id="031f3-166">たとえば、変数またはなどのコードを使用して、式のコンテンツを表示する`@MyVar`、ASP.NET Web ページが自動的に出力をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="031f3-166">For example, when you display the content of a variable or an expression using code such as `@MyVar`, ASP.NET Web Pages automatically encodes the output.</span></span>


## <a name="validating-user-input"></a><span data-ttu-id="031f3-167">ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="031f3-167">Validating User Input</span></span>

<span data-ttu-id="031f3-168">ユーザーを犯します。</span><span class="sxs-lookup"><span data-stu-id="031f3-168">Users make mistakes.</span></span> <span data-ttu-id="031f3-169">フィールドに入力するよう依頼して、忘れてしまうまたは従業員の数を入力するように依頼し、代わりに名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="031f3-169">You ask them to fill in a field, and they forget to, or you ask them to enter the number of employees and they type a name instead.</span></span> <span data-ttu-id="031f3-170">フォーム データが格納された正しく処理する前に確認するには、ユーザーの入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="031f3-170">To make sure that a form has been filled in correctly before you process it, you validate the user's input.</span></span>

<span data-ttu-id="031f3-171">この手順では、ユーザーしなかった空白のままになっていることを確認するすべての 3 つのフォーム フィールドを検証する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="031f3-171">This procedure shows how to validate all three form fields to make sure the user didn't leave them blank.</span></span> <span data-ttu-id="031f3-172">従業員数の値が数値であることも確認します。</span><span class="sxs-lookup"><span data-stu-id="031f3-172">You also check that the employee count value is a number.</span></span> <span data-ttu-id="031f3-173">エラーがある場合は、エラーを表示するがどのような値をユーザーに通知するメッセージが検証に合格しませんでした。</span><span class="sxs-lookup"><span data-stu-id="031f3-173">If there are errors, you'll display an error message that tells the user what values didn't pass validation.</span></span>

1. <span data-ttu-id="031f3-174">*Form.cshtml*ファイルでコードの最初のブロックを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="031f3-174">In the *Form.cshtml* file, replace the first block of code with the following code:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    <span data-ttu-id="031f3-175">使用するユーザー入力を検証する、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="031f3-175">To validate user input, you use the `Validation` helper.</span></span> <span data-ttu-id="031f3-176">呼び出すことによって必要なフィールドを登録する`Validation.RequireField`します。</span><span class="sxs-lookup"><span data-stu-id="031f3-176">You register required fields by calling `Validation.RequireField`.</span></span> <span data-ttu-id="031f3-177">呼び出すことによって他の種類の検証を登録する`Validation.Add`を検証するフィールドと実行する検証の種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="031f3-177">You register other types of validation by calling `Validation.Add` and specifying the field to validate and the type of validation to perform.</span></span>

    <span data-ttu-id="031f3-178">ページが実行される ASP.NET すべての検証にすることができます。</span><span class="sxs-lookup"><span data-stu-id="031f3-178">When the page runs, ASP.NET does all the validation for you.</span></span> <span data-ttu-id="031f3-179">呼び出して結果を確認できます`Validation.IsValid`、すべてのものが渡された場合は true を返すと任意のフィールド検証に失敗した場合は false。</span><span class="sxs-lookup"><span data-stu-id="031f3-179">You can check the results by calling `Validation.IsValid`, which returns true if everything passed and false if any field failed validation.</span></span> <span data-ttu-id="031f3-180">通常、呼び出す`Validation.IsValid`ユーザー入力にどのような処理を実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="031f3-180">Typically, you call `Validation.IsValid` before you perform any processing on the user input.</span></span>
2. <span data-ttu-id="031f3-181">更新プログラム、`<body>`要素に 3 つの呼び出しを追加することで、`Html.ValidationMessage`メソッドは、次のように。</span><span class="sxs-lookup"><span data-stu-id="031f3-181">Update the `<body>` element by adding three calls to the `Html.ValidationMessage` method, like this:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    <span data-ttu-id="031f3-182">検証エラー メッセージを表示するには、Html を呼び出すことができます。`ValidationMessage`</span><span class="sxs-lookup"><span data-stu-id="031f3-182">To display validation error messages, you can call Html.`ValidationMessage`</span></span> <span data-ttu-id="031f3-183">メッセージを使用するフィールドの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="031f3-183">and pass it the name of the field that you want the message for.</span></span>
3. <span data-ttu-id="031f3-184">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="031f3-184">Run the page.</span></span> <span data-ttu-id="031f3-185">フィールドを空白のままにし、クリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="031f3-185">Leave the fields blank and click **Submit**.</span></span> <span data-ttu-id="031f3-186">エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="031f3-186">You see error messages.</span></span>

    ![ユーザー入力が検証に合格しなかったかどうかに表示されるエラー メッセージを示すスクリーン ショット。](4-working-with-forms/_static/image3.jpg)
4. <span data-ttu-id="031f3-188">文字列 (たとえば、"ABC") を追加、**従業員数**フィールドし、クリックして**送信**もう一度です。</span><span class="sxs-lookup"><span data-stu-id="031f3-188">Add a string (for example, "ABC") to the **Employee Count** field and click **Submit** again.</span></span> <span data-ttu-id="031f3-189">この時間のことを示すエラーが表示文字列は、適切な形式、つまり、整数ではありません。</span><span class="sxs-lookup"><span data-stu-id="031f3-189">This time you see an error that indicates that the string isn't in the right format, namely, an integer.</span></span>

    ![ユーザーが従業員のフィールドの文字列を入力するかどうかに表示されるエラー メッセージを示すスクリーン ショット。](4-working-with-forms/_static/image4.jpg)

<span data-ttu-id="031f3-191">ASP.NET Web ページは、自動的にユーザーがブラウザーで迅速なフィードバックを取得できるように、クライアント スクリプトを使用して検証を実行する機能など、ユーザー入力を検証するための他のオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="031f3-191">ASP.NET Web Pages provides more options for validating user input, including the ability to automatically perform validation using client script, so that users get immediate feedback in the browser.</span></span> <span data-ttu-id="031f3-192">参照してください、[資料](#Additional_Resources)詳細については後でセクション。</span><span class="sxs-lookup"><span data-stu-id="031f3-192">See the [Additional Resources](#Additional_Resources) section later for more information.</span></span>

## <a name="restoring-form-values-after-postbacks"></a><span data-ttu-id="031f3-193">ポストバック後にフォームの値を復元します。</span><span class="sxs-lookup"><span data-stu-id="031f3-193">Restoring Form Values After Postbacks</span></span>

<span data-ttu-id="031f3-194">前のセクションで、ページをテストするときにお気付きかもしれませんが、検証エラーが発生し、(無効なデータだけでなく) を入力したすべてのものがないため、され、すべてのフィールドの値を再入力する必要がありました。</span><span class="sxs-lookup"><span data-stu-id="031f3-194">When you tested the page in the previous section, you might have noticed that if you had a validation error, everything you entered (not just the invalid data) was gone, and you had to re-enter values for all the fields.</span></span> <span data-ttu-id="031f3-195">これは重要な点を示しています。 ページが最初から再作成ページを送信すると処理し、もう一度ページを表示し、します。</span><span class="sxs-lookup"><span data-stu-id="031f3-195">This illustrates an important point: when you submit a page, process it, and then render the page again, the page is re-created from scratch.</span></span> <span data-ttu-id="031f3-196">学習したように、送信されたときに、ページには任意の値が失われることを意味します。</span><span class="sxs-lookup"><span data-stu-id="031f3-196">As you saw, this means that any values that were in the page when it was submitted are lost.</span></span>

<span data-ttu-id="031f3-197">修正できます簡単に、ただしします。</span><span class="sxs-lookup"><span data-stu-id="031f3-197">You can fix this easily, however.</span></span> <span data-ttu-id="031f3-198">送信された値へのアクセスがある (で、`Request.Form`オブジェクト、ページが表示されるときに、フォーム フィールドにこれらの値を入力することができます。</span><span class="sxs-lookup"><span data-stu-id="031f3-198">You have access to the values that were submitted (in the `Request.Form` object, so you can fill those values back into the form fields when the page is rendered.</span></span>

1. <span data-ttu-id="031f3-199">*Form.cshtml*ファイルで、置換、`value`の属性、`<input>`要素を使用して、`value`属性。</span><span class="sxs-lookup"><span data-stu-id="031f3-199">In the *Form.cshtml* file, replace the `value` attributes of the `<input>` elements using the `value` attribute.:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    <span data-ttu-id="031f3-200">`value`の属性、`<input>`のフィールドの値を動的に読み取る要素が設定されて、`Request.Form`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="031f3-200">The `value` attribute of the `<input>` elements has been set to dynamically read the field value out of the `Request.Form` object.</span></span> <span data-ttu-id="031f3-201">内の値、最初に、ページが要求される、`Request.Form`オブジェクトがすべて空にします。</span><span class="sxs-lookup"><span data-stu-id="031f3-201">The first time that the page is requested, the values in the `Request.Form` object are all empty.</span></span> <span data-ttu-id="031f3-202">これは、により、フォームが空白ため問題ありません。</span><span class="sxs-lookup"><span data-stu-id="031f3-202">This is fine, because that way the form is blank.</span></span>
2. <span data-ttu-id="031f3-203">お使いのブラウザーでページを起動、フォーム フィールドを入力し、空白のまままたは をクリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="031f3-203">Launch the page in your browser, fill in the form fields or leave them blank, and click **Submit**.</span></span> <span data-ttu-id="031f3-204">送信された値を表示するページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="031f3-204">A page that shows the submitted values is displayed.</span></span>

    ![フォーム 5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="031f3-206">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="031f3-206">Additional Resources</span></span>

- [<span data-ttu-id="031f3-207">1,001 方法で Web ユーザーからの入力を取得するには</span><span class="sxs-lookup"><span data-stu-id="031f3-207">1,001 Ways to Get Input from Web Users</span></span>](https://msdn.microsoft.com/library/ms971057.aspx)
- <span data-ttu-id="031f3-208">[フォームを使用して、ユーザー入力の処理](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span><span class="sxs-lookup"><span data-stu-id="031f3-208">[Using Forms and Processing User Input](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span></span>
- [<span data-ttu-id="031f3-209">ASP.NET Web ページにおけるユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="031f3-209">Validating User Input in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=253002)
- <span data-ttu-id="031f3-210">[HTML フォームのオートコンプリート機能の使用](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="031f3-210">[Using AutoComplete in HTML Forms](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span></span>
