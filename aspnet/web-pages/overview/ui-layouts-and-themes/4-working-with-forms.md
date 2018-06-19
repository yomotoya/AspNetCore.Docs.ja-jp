---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: ASP.NET Web Pages (Razor) のサイトでの HTML フォームの使用 |Microsoft ドキュメント
author: tfitzmac
description: フォームは、テキスト ボックス、チェック ボックス、ラジオ ボタン、およびプルダウンで表示されるリストと同様に、ユーザー入力コントロールを配置する HTML ドキュメントのセクションです。 フォームを使用して修正しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "28046431"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="fcd2c-104">ASP.NET Web Pages (Razor) のサイトでの HTML フォームの使用</span><span class="sxs-lookup"><span data-stu-id="fcd2c-104">Working with HTML Forms in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="fcd2c-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fcd2c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fcd2c-106">(テキスト ボックスとボタン) を持つ、HTML フォームを処理する方法を説明する ASP.NET Web Pages (Razor) の web サイトで作業しているときにします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-106">This article describes how to process an HTML form (with text boxes and buttons) when you are working in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="fcd2c-107">**学習する内容。**</span><span class="sxs-lookup"><span data-stu-id="fcd2c-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="fcd2c-108">HTML フォームを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-108">How to create an HTML form.</span></span>
> - <span data-ttu-id="fcd2c-109">フォームからユーザーの入力を読み取る方法です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-109">How to read user input from the form.</span></span>
> - <span data-ttu-id="fcd2c-110">ユーザー入力を検証する方法。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-110">How to validate user input.</span></span>
> - <span data-ttu-id="fcd2c-111">ページの送信後に、フォームの値を復元する方法です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-111">How to restore form values after the page is submitted.</span></span>
> 
> <span data-ttu-id="fcd2c-112">これらは、ASP.NET のアーティクルで導入された概念をプログラミングします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-112">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="fcd2c-113">`Request` オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-113">The `Request` object.</span></span>
> - <span data-ttu-id="fcd2c-114">入力の検証。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-114">Input validation.</span></span>
> - <span data-ttu-id="fcd2c-115">HTML エンコード。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-115">HTML encoding.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fcd2c-116">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="fcd2c-116">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fcd2c-117">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="fcd2c-117">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="fcd2c-118">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-118">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="creating-a-simple-html-form"></a><span data-ttu-id="fcd2c-119">単純な HTML フォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-119">Creating a Simple HTML Form</span></span>

1. <span data-ttu-id="fcd2c-120">新しい web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-120">Create a new website.</span></span>
2. <span data-ttu-id="fcd2c-121">ルート フォルダーの作成という名前の web ページ*Form.cshtml*し、次のマークアップを入力します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-121">In the root folder, create a web page named *Form.cshtml* and enter the following markup:</span></span>

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. <span data-ttu-id="fcd2c-122">お使いのブラウザーでページを起動します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-122">Launch the page in your browser.</span></span> <span data-ttu-id="fcd2c-123">(WebMatrix での**ファイル**] ワークスペースで、ファイルを右クリックし、[**ブラウザーで起動**)。次の 3 つの入力フィールドを持つ単純な形式と**送信**ボタンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-123">(In WebMatrix, in the **Files** workspace, right-click the file and then select **Launch in browser**.) A simple form with three input fields and a **Submit** button is displayed.</span></span>

    ![次の 3 つのテキスト ボックス、フォームのスクリーン ショット。](4-working-with-forms/_static/image1.jpg)

    <span data-ttu-id="fcd2c-125">この時点をクリックした場合、**送信**ボタンを何も起こりません。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-125">At this point, if you click the **Submit** button, nothing happens.</span></span> <span data-ttu-id="fcd2c-126">フォームを便利にするために、サーバーで実行されるコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-126">To make the form useful, you have to add some code that will run on the server.</span></span>

## <a name="reading-user-input-from-the-form"></a><span data-ttu-id="fcd2c-127">フォームからユーザーの入力を読み取る</span><span class="sxs-lookup"><span data-stu-id="fcd2c-127">Reading User Input from the Form</span></span>

<span data-ttu-id="fcd2c-128">フォームを処理するには、送信されたフィールドの値を読み取り、それらに何かコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-128">To process the form, you add code that reads the submitted field values and does something with them.</span></span> <span data-ttu-id="fcd2c-129">この手順では、フィールドを読み取りおよび ページで、ユーザー入力を表示する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-129">This procedure shows you how to read the fields and display the user input on the page.</span></span> <span data-ttu-id="fcd2c-130">(実稼働アプリケーションで一般的に処理を行うより興味深いユーザー入力をします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-130">(In a production application, you generally do more interesting things with user input.</span></span> <span data-ttu-id="fcd2c-131">行いますをデータベースの使用についての情報の記事です。)</span><span class="sxs-lookup"><span data-stu-id="fcd2c-131">You'll do that in the article about working with databases.)</span></span>

1. <span data-ttu-id="fcd2c-132">上部にある、 *Form.cshtml*ファイルを次のコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-132">At the top of the *Form.cshtml* file, enter the following code:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    <span data-ttu-id="fcd2c-133">ユーザーは、最初のページを要求、空のフォームのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-133">When the user first requests the page, only the empty form is displayed.</span></span> <span data-ttu-id="fcd2c-134">(される)、ユーザーが、フォームに入力しをクリックして**送信**です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-134">The user (which will be you) fills in the form and then clicks **Submit**.</span></span> <span data-ttu-id="fcd2c-135">これが提出 (投稿) をサーバーにユーザー入力します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-135">This submits (posts) the user input to the server.</span></span> <span data-ttu-id="fcd2c-136">既定では、要求は同じページに移動 (つまり、 *Form.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-136">By default, the request goes to the same page (namely, *Form.cshtml*).</span></span>

    <span data-ttu-id="fcd2c-137">この時点で、ページを送信するときに、フォーム上で入力した値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-137">When you submit the page this time, the values you entered are displayed just above the form:</span></span>

    ![入力した、ページに表示される値を示すスクリーン ショット。](4-working-with-forms/_static/image2.jpg)

    <span data-ttu-id="fcd2c-139">ページのコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-139">Look at the code for the page.</span></span> <span data-ttu-id="fcd2c-140">最初に使用する、`IsPost`ページがポストされるかどうかを調べます&#8212;、かどうか、ユーザーがクリックされた、**送信**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-140">You first use the `IsPost` method to determine whether the page is being posted &#8212; that is, whether a user clicked the **Submit** button.</span></span> <span data-ttu-id="fcd2c-141">これは、投稿場合`IsPost`は true を返します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-141">If this is a post, `IsPost` returns true.</span></span> <span data-ttu-id="fcd2c-142">これは、最初の要求 (GET 要求など) またはポストバック (POST 要求) で作業しているかどうかを決定する標準的な方法では、ASP.NET Web ページです。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-142">This is the standard way in ASP.NET Web Pages to determine whether you're working with an initial request (a GET request) or a postback (a POST request).</span></span> <span data-ttu-id="fcd2c-143">(GET と POST の詳細についてを参照してください"HTTP GET と POST と IsPost Property"で[ASP.NET Web ページは、Razor 構文を使用プログラミングの概要](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost))。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-143">(For more information about GET and POST, see the sidebar "HTTP GET and POST and the IsPost Property" in [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)</span></span>

    <span data-ttu-id="fcd2c-144">次に、ユーザーから入力される値を取得する、`Request.Form`して、オブジェクトに置く変数後にします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-144">Next, you get the values that the user filled in from the `Request.Form` object, and you put them in variables for later.</span></span> <span data-ttu-id="fcd2c-145">`Request.Form`オブジェクトには、キーによって識別される各ページに送信されたすべての値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-145">The `Request.Form` object contains all the values that were submitted with the page, each identified by a key.</span></span> <span data-ttu-id="fcd2c-146">キーは、該当するショートカットを`name`読みたいフォーム フィールドの属性です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-146">The key is the equivalent to the `name` attribute of the form field that you want to read.</span></span> <span data-ttu-id="fcd2c-147">例については、読み取り、`companyname`フィールド (テキスト ボックス) を使用する`Request.Form["companyname"]`です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-147">For example, to read the `companyname` field (text box), you use `Request.Form["companyname"]`.</span></span>

    <span data-ttu-id="fcd2c-148">フォームの値が格納されている、`Request.Form`オブジェクトを文字列として。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-148">Form values are stored in the `Request.Form` object as strings.</span></span> <span data-ttu-id="fcd2c-149">そのため、数値、日付、またはその他の種類と値を使用する場合、その型への文字列から変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-149">Therefore, when you have to work with a value as a number or a date or some other type, you have to convert it from a string to that type.</span></span> <span data-ttu-id="fcd2c-150">例では、`AsInt`のメソッド、 `Request.Form` (を従業員数を含む) の従業員のフィールドの値を整数に変換するために使用します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-150">In the example, the `AsInt` method of the `Request.Form` is used to convert the value of the employees field (which contains an employee count) to an integer.</span></span>
2. <span data-ttu-id="fcd2c-151">お使いのブラウザーでページを起動して、フォーム フィールドの入力 をクリックして**送信**です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-151">Launch the page in your browser, fill in the form fields, and click **Submit**.</span></span> <span data-ttu-id="fcd2c-152">ページには、入力した値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-152">The page displays the values you entered.</span></span>

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a><span data-ttu-id="fcd2c-153">HTML エンコーディングの外観およびセキュリティ</span><span class="sxs-lookup"><span data-stu-id="fcd2c-153">HTML Encoding for Appearance and Security</span></span>
> 
> <span data-ttu-id="fcd2c-154">HTML などの文字の特殊な用途を持つ`<`、 `>`、および`&`です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-154">HTML has special uses for characters like `<`, `>`, and `&`.</span></span> <span data-ttu-id="fcd2c-155">これらの特殊文字が表示されませんに想定している場合、ことができますに支障をきたす、web ページの機能と外観です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-155">If these special characters appear where they're not expected, they can ruin the appearance and functionality of your web page.</span></span> <span data-ttu-id="fcd2c-156">たとえば、ブラウザーでは、解釈、`<`と同様に、HTML 要素の先頭として (ない場合に空白が続いて) を文字`<b>`または`<input ...>`です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-156">For example, the browser interprets the `<` character (unless it's followed by a space) as the beginning of an HTML element, like `<b>` or `<input ...>`.</span></span> <span data-ttu-id="fcd2c-157">始まる文字列を単に破棄ブラウザー認識しない場合、要素、`<`に達するまで何かをもう一度と認識します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-157">If the browser doesn't recognize the element, it simply discards the string that begins with `<` until it reaches something that it again recognizes.</span></span> <span data-ttu-id="fcd2c-158">当然ながら、ページの奇妙ないくつか表示になります。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-158">Obviously, this can result in some weird rendering in the page.</span></span>
> 
> <span data-ttu-id="fcd2c-159">HTML エンコードすると、これら予約文字をブラウザーが、適正なシンボルとして解釈するコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-159">HTML encoding replaces these reserved characters with a code that browsers interpret as the correct symbol.</span></span> <span data-ttu-id="fcd2c-160">たとえば、`<`文字で置換されます`&lt;`と`>`文字で置換されます`&gt;`です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-160">For example, the `<` character is replaced with `&lt;` and the `>` character is replaced with `&gt;`.</span></span> <span data-ttu-id="fcd2c-161">ブラウザーは、置換文字列を表示する文字として表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-161">The browser renders these replacement strings as the characters that you want to see.</span></span>
> 
> <span data-ttu-id="fcd2c-162">HTML エンコード文字列を表示する任意の時間を使用する (入力)、ユーザーから取得したことをお勧めすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-162">It's a good idea to use HTML encoding any time you display strings (input) that you got from a user.</span></span> <span data-ttu-id="fcd2c-163">ない場合は、ユーザーことができます、web ページに悪意のあるスクリプトを実行したり、別のものは、サイトのセキュリティを侵害するか、望ましくないだけであるを取得しようとします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-163">If you don't, a user can try to get your web page to run a malicious script or do something else that compromises your site security or that's just not what you intend.</span></span> <span data-ttu-id="fcd2c-164">(これは、ユーザー入力を受け取り、場所、保管およびし、後で表示する場合に特に重要&#8212;など、コメント、ユーザーのレビュー、またはそのようなものです)。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-164">(This is particularly important if you take user input, store it someplace, and then display it later &#8212; for example, as a blog comment, user review, or something like that.)</span></span>
> 
> <span data-ttu-id="fcd2c-165">自動的に ASP.NET Web Pages、これらの問題を防ぐために役立つを HTML エンコード任意のテキスト コンテンツをコードから出力します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-165">To help prevent these problems, ASP.NET Web Pages automatically HTML-encodes any text content that you output from your code.</span></span> <span data-ttu-id="fcd2c-166">たとえば、変数や式などを使用してコードの内容を表示する`@MyVar`、ASP.NET Web Pages は、出力を自動的にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-166">For example, when you display the content of a variable or an expression using code such as `@MyVar`, ASP.NET Web Pages automatically encodes the output.</span></span>


## <a name="validating-user-input"></a><span data-ttu-id="fcd2c-167">ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="fcd2c-167">Validating User Input</span></span>

<span data-ttu-id="fcd2c-168">ユーザーは間違いです。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-168">Users make mistakes.</span></span> <span data-ttu-id="fcd2c-169">フィールドに入力するように依頼することを忘れると、または従業員の数を入力するように依頼し、代わりに名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-169">You ask them to fill in a field, and they forget to, or you ask them to enter the number of employees and they type a name instead.</span></span> <span data-ttu-id="fcd2c-170">で、フォームが満杯になった正しく処理する前に確認するため、ユーザーの入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-170">To make sure that a form has been filled in correctly before you process it, you validate the user's input.</span></span>

<span data-ttu-id="fcd2c-171">この手順では、ユーザーがありませんでした空白のままになっていることを確認するすべての 3 つのフォーム フィールドを検証する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-171">This procedure shows how to validate all three form fields to make sure the user didn't leave them blank.</span></span> <span data-ttu-id="fcd2c-172">確認することも、従業員の数の値が数値であります。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-172">You also check that the employee count value is a number.</span></span> <span data-ttu-id="fcd2c-173">エラーがある場合は、エラーを表示するが、ユーザーにどのような値を示すメッセージが検証に合格しませんでした。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-173">If there are errors, you'll display an error message that tells the user what values didn't pass validation.</span></span>

1. <span data-ttu-id="fcd2c-174">*Form.cshtml*ファイルでコードの最初のブロックを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-174">In the *Form.cshtml* file, replace the first block of code with the following code:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    <span data-ttu-id="fcd2c-175">使用するユーザー入力を検証する、`Validation`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-175">To validate user input, you use the `Validation` helper.</span></span> <span data-ttu-id="fcd2c-176">呼び出して必要なフィールドを登録する`Validation.RequireField`です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-176">You register required fields by calling `Validation.RequireField`.</span></span> <span data-ttu-id="fcd2c-177">呼び出して他の種類の検証を登録する`Validation.Add`を検証するフィールドと、実行する検証の種類を指定することです。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-177">You register other types of validation by calling `Validation.Add` and specifying the field to validate and the type of validation to perform.</span></span>

    <span data-ttu-id="fcd2c-178">ページの実行時に ASP.NET では、すべての検証します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-178">When the page runs, ASP.NET does all the validation for you.</span></span> <span data-ttu-id="fcd2c-179">呼び出して結果を確認できます`Validation.IsValid`、すべてのものが渡された場合は true が返されます、任意のフィールド検証に失敗した場合は false。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-179">You can check the results by calling `Validation.IsValid`, which returns true if everything passed and false if any field failed validation.</span></span> <span data-ttu-id="fcd2c-180">通常、呼び出す`Validation.IsValid`ユーザー入力にどのような処理を実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-180">Typically, you call `Validation.IsValid` before you perform any processing on the user input.</span></span>
2. <span data-ttu-id="fcd2c-181">更新プログラム、`<body>`要素を 3 回の呼び出しを追加することによって、`Html.ValidationMessage`次のように、メソッド。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-181">Update the `<body>` element by adding three calls to the `Html.ValidationMessage` method, like this:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    <span data-ttu-id="fcd2c-182">検証エラー メッセージを表示するには、Html を呼び出すことができます。`ValidationMessage`</span><span class="sxs-lookup"><span data-stu-id="fcd2c-182">To display validation error messages, you can call Html.`ValidationMessage`</span></span> <span data-ttu-id="fcd2c-183">そのメッセージを使用するフィールドの名前。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-183">and pass it the name of the field that you want the message for.</span></span>
3. <span data-ttu-id="fcd2c-184">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-184">Run the page.</span></span> <span data-ttu-id="fcd2c-185">フィールドを空白のままにし、をクリックして**送信**です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-185">Leave the fields blank and click **Submit**.</span></span> <span data-ttu-id="fcd2c-186">エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-186">You see error messages.</span></span>

    ![ユーザー入力が検証に合格しなかったかどうかに表示されたエラー メッセージを示すスクリーン ショット。](4-working-with-forms/_static/image3.jpg)
4. <span data-ttu-id="fcd2c-188">文字列 ("ABC"など) を追加、**従業員数**フィールドし、をクリックして**送信**もう一度です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-188">Add a string (for example, "ABC") to the **Employee Count** field and click **Submit** again.</span></span> <span data-ttu-id="fcd2c-189">この時間のことを示すエラーが表示で、適切な形式、つまり、整数、文字列はありません。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-189">This time you see an error that indicates that the string isn't in the right format, namely, an integer.</span></span>

    ![ユーザーが従業員フィールドの文字列を入力するかどうかに表示されたエラー メッセージを示すスクリーン ショット。](4-working-with-forms/_static/image4.jpg)

<span data-ttu-id="fcd2c-191">ASP.NET Web ページは、自動的にユーザーがブラウザーで即時のフィードバックを取得できるようにクライアント スクリプトを使用して検証を実行する機能など、ユーザー入力を検証するためのより多くのオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-191">ASP.NET Web Pages provides more options for validating user input, including the ability to automatically perform validation using client script, so that users get immediate feedback in the browser.</span></span> <span data-ttu-id="fcd2c-192">参照してください、[その他のリソース](#Additional_Resources)詳細については後述します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-192">See the [Additional Resources](#Additional_Resources) section later for more information.</span></span>

## <a name="restoring-form-values-after-postbacks"></a><span data-ttu-id="fcd2c-193">ポストバックの後のフォーム値を復元します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-193">Restoring Form Values After Postbacks</span></span>

<span data-ttu-id="fcd2c-194">前のセクションで、ページをテストするときにお気付きする場合は、検証エラーが発生、(無効なデータだけでなく) を入力したすべてのものがなくなり、およびすべてのフィールドの値を再入力する必要がありました。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-194">When you tested the page in the previous section, you might have noticed that if you had a validation error, everything you entered (not just the invalid data) was gone, and you had to re-enter values for all the fields.</span></span> <span data-ttu-id="fcd2c-195">これは、重要な点を示しています。 ページが最初から再作成するとページを送信、処理、もう一度ページを表示、します。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-195">This illustrates an important point: when you submit a page, process it, and then render the page again, the page is re-created from scratch.</span></span> <span data-ttu-id="fcd2c-196">説明したように含まれていたページが送信されたときに任意の値が失われることになります。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-196">As you saw, this means that any values that were in the page when it was submitted are lost.</span></span>

<span data-ttu-id="fcd2c-197">これを修正する簡単に、ただしです。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-197">You can fix this easily, however.</span></span> <span data-ttu-id="fcd2c-198">送信された値へのアクセスがある (で、`Request.Form`オブジェクト、ページが表示される場合はフォームのフィールドにそれらの値を入力することができます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-198">You have access to the values that were submitted (in the `Request.Form` object, so you can fill those values back into the form fields when the page is rendered.</span></span>

1. <span data-ttu-id="fcd2c-199">*Form.cshtml*ファイルで置き換えます、`value`の属性、`<input>`要素を使用して、`value`属性。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-199">In the *Form.cshtml* file, replace the `value` attributes of the `<input>` elements using the `value` attribute.:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    <span data-ttu-id="fcd2c-200">`value`の属性、`<input>`要素が動的な読み取りのフィールドの値に設定されて、`Request.Form`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-200">The `value` attribute of the `<input>` elements has been set to dynamically read the field value out of the `Request.Form` object.</span></span> <span data-ttu-id="fcd2c-201">内の値、最初に、ページが要求される、`Request.Form`オブジェクトがすべて空にします。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-201">The first time that the page is requested, the values in the `Request.Form` object are all empty.</span></span> <span data-ttu-id="fcd2c-202">これは、しますこのようにして、フォームは空白なので。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-202">This is fine, because that way the form is blank.</span></span>
2. <span data-ttu-id="fcd2c-203">お使いのブラウザー内のページを起動、フォーム フィールドの入力または、空白のままにし、 をクリックして**送信**です。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-203">Launch the page in your browser, fill in the form fields or leave them blank, and click **Submit**.</span></span> <span data-ttu-id="fcd2c-204">送信された値を表示するページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcd2c-204">A page that shows the submitted values is displayed.</span></span>

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="fcd2c-206">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="fcd2c-206">Additional Resources</span></span>

- [<span data-ttu-id="fcd2c-207">Web ユーザーからの入力を取得する方法は 1,001</span><span class="sxs-lookup"><span data-stu-id="fcd2c-207">1,001 Ways to Get Input from Web Users</span></span>](https://msdn.microsoft.com/library/ms971057.aspx)
- <span data-ttu-id="fcd2c-208">[フォームを使用して、ユーザー入力の処理](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span><span class="sxs-lookup"><span data-stu-id="fcd2c-208">[Using Forms and Processing User Input](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span></span>
- [<span data-ttu-id="fcd2c-209">ASP.NET Web ページにおけるユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="fcd2c-209">Validating User Input in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=253002)
- <span data-ttu-id="fcd2c-210">[HTML フォームでのオートコンプリートの使用](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="fcd2c-210">[Using AutoComplete in HTML Forms](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span></span>
