---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Razor 構文 (C#) を使用して ASP.NET Web プログラミングの概要 |Microsoft Docs
author: Rick-Anderson
description: この章では、するプログラミングの概要 ASP.NET Web ページ Razor 構文を使用します。 動的な web の pa を実行するためのマイクロソフトのテクノロジを ASP.NET には.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: b5eb98dfdf3fc013920f45080d4a20e1fa507725
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021782"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="24611-104">Razor 構文 (C#) を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="24611-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="24611-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24611-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="24611-106">この記事では、Razor 構文を使ったASP.NET Web ページにおけるプログラミングの概要を解説します。</span><span class="sxs-lookup"><span data-stu-id="24611-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="24611-107">ASP.NETは、Webサーバーで動的な Web ページを運用するためのマイクロソフトの技術です。</span><span class="sxs-lookup"><span data-stu-id="24611-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="24611-108">この記事では、C# プログラミング言語の使用に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="24611-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="24611-109">**学習内容**:</span><span class="sxs-lookup"><span data-stu-id="24611-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="24611-110">Razor構文を使ったASP.NET Webページのプログラミングをはじめるための上位8つのプログラミング Tips</span><span class="sxs-lookup"><span data-stu-id="24611-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="24611-111">必要とする基本的なプログラミング概念</span><span class="sxs-lookup"><span data-stu-id="24611-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="24611-112">ASP.NETサーバーコードとRazor構文に関する全体像</span><span class="sxs-lookup"><span data-stu-id="24611-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="24611-113">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="24611-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="24611-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="24611-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="24611-115">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="24611-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="24611-116">8 つのプログラミング Tips</span><span class="sxs-lookup"><span data-stu-id="24611-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="24611-117">このセクションでは、Razor 構文を使用して ASP.NET サーバー コードの記述を開始する際に知っておく必要ないくつかのヒントを示します。</span><span class="sxs-lookup"><span data-stu-id="24611-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="24611-118">Razor 構文は C# プログラミング言語に基づいており、ASP.NET Web Pages を最もよく使用されている言語です。</span><span class="sxs-lookup"><span data-stu-id="24611-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="24611-119">ただし、Razor 構文は、Visual Basic 言語と Visual Basic で実行することもできます。 表示されるものをもサポートします。</span><span class="sxs-lookup"><span data-stu-id="24611-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="24611-120">詳細については、「付録」を参照してください。 [Visual Basic 言語と構文](https://go.microsoft.com/fwlink/?LinkId=202908)します。</span><span class="sxs-lookup"><span data-stu-id="24611-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="24611-121">記事の後半でこれらのプログラミング手法のほとんどについての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="24611-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="24611-122">1.@文字を使用してページにコードを追加する</span><span class="sxs-lookup"><span data-stu-id="24611-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="24611-123">`@`文字は、インライン式、単一文のブロック、複文のブロックの始まりを表します。</span><span class="sxs-lookup"><span data-stu-id="24611-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="24611-124">これは、これらのステートメントがどのようなページがブラウザーで実行するとします。</span><span class="sxs-lookup"><span data-stu-id="24611-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="24611-126">**HTML エンコード**</span><span class="sxs-lookup"><span data-stu-id="24611-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="24611-127">ページを使用して、コンテンツを表示すると、 `@` ASP.NET HTML エンコードの出力を前の例のように文字します。</span><span class="sxs-lookup"><span data-stu-id="24611-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="24611-128">これにより、HTML の予約文字が置き換えられます (など`<`と`>`と`&`) の HTML タグまたはエンティティとして解釈されるのではなく、web ページ内の文字として表示される文字を有効にするコードを持つ。</span><span class="sxs-lookup"><span data-stu-id="24611-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="24611-129">HTML エンコードをせずサーバー コードからの出力は正しく表示されない場合があり、セキュリティのリスクについてのページを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24611-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="24611-130">目的がマークアップとしてタグをレンダリングする HTML マークアップを出力するかどうか (たとえば`<p></p>`段落のまたは`<em></em>`テキストを強調するために) を参照してください[を組み合わせることのテキスト、マークアップ、およびコード ブロック内のコード](#BM_CombiningTextMarkupAndCode)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="24611-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="24611-131">詳細について、HTML でのエンコード[フォームを使用する](https://go.microsoft.com/fwlink/?LinkId=202892)。</span><span class="sxs-lookup"><span data-stu-id="24611-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="24611-132">2.中カッコでコードブロックを囲む</span><span class="sxs-lookup"><span data-stu-id="24611-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="24611-133">A*コード ブロック*1 つまたは複数のコード ステートメントを含み、中かっこで囲まれました。</span><span class="sxs-lookup"><span data-stu-id="24611-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="24611-134">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="24611-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="24611-136">3.ブロックの中で、各コード文をセミコロンで終わらせる</span><span class="sxs-lookup"><span data-stu-id="24611-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="24611-137">コードブロックの中では、それぞれの完結するコード文はセミコロンで終わらせます。</span><span class="sxs-lookup"><span data-stu-id="24611-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="24611-138">インライン式は、セミコロンで終わりません。</span><span class="sxs-lookup"><span data-stu-id="24611-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="24611-139">4.値を格納するのに変数を使う</span><span class="sxs-lookup"><span data-stu-id="24611-139">4. You use variables to store values</span></span>

<span data-ttu-id="24611-140">*変数*を使って値を格納できます。これには文字列、数値、日付などが含まれます。新しい変数は予約語`var`を使って作成します。</span><span class="sxs-lookup"><span data-stu-id="24611-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="24611-141">変数は`@`を使って直接ページに挿入できます。</span><span class="sxs-lookup"><span data-stu-id="24611-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="24611-142">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="24611-142">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="24611-144">5.リテラル文字列値を二重引用符で括る</span><span class="sxs-lookup"><span data-stu-id="24611-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="24611-145">*文字列*は、テキストとして扱われる文字のつながりです。</span><span class="sxs-lookup"><span data-stu-id="24611-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="24611-146">文字列を指定するには、ダブル クォーテーション記号で囲みます。</span><span class="sxs-lookup"><span data-stu-id="24611-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="24611-147">文字列表示するにはかどうかは、円記号が含まれています ( `\` ) または二重引用符 ( `"` )、使用、 *verbatim 文字列リテラル*が付いている`@`演算子。</span><span class="sxs-lookup"><span data-stu-id="24611-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="24611-148">(C# で、\ 文字は verbatim 文字列リテラルを使用する場合を除き、特別な意味を持つ)。</span><span class="sxs-lookup"><span data-stu-id="24611-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="24611-149">二重引用符を埋め込むには、逐語的文字列リテラルを使用し、引用符を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="24611-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="24611-150">ページを使用してこれらの例の両方の結果を次に示します。</span><span class="sxs-lookup"><span data-stu-id="24611-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="24611-152">注意、 `@` verbatim 文字列リテラル (C#) をマークして、ASP.NET ページ内のコードをマークする、文字を使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="24611-153">6.コードは大文字小文字を区別</span><span class="sxs-lookup"><span data-stu-id="24611-153">6. Code is case sensitive</span></span>

<span data-ttu-id="24611-154">C# のキーワード (など`var`、 `true`、および`if`) し、変数名では大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="24611-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="24611-155">次のコード行が 2 つの異なる変数を作成`lastName`と `LastName.`</span><span class="sxs-lookup"><span data-stu-id="24611-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="24611-156">として変数を宣言する場合`var lastName = "Smith";`としてページには、その変数を参照しようとする場合と`@LastName`、ために、エラーが発生`LastName`認識されません。</span><span class="sxs-lookup"><span data-stu-id="24611-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="24611-157">Visual Basic ではキーワードと変数は*いない*大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="24611-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="24611-158">7.コーディングのほとんどには、オブジェクトが含まれます</span><span class="sxs-lookup"><span data-stu-id="24611-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="24611-159">*オブジェクト*をプログラミングできることを表します&#8212;ページ、テキスト ボックス、ファイル、画像、web 要求を電子メール メッセージ、(データベースの行) の顧客レコードなど。オブジェクトの特性を記述するプロパティがあると、読み取りまたは変更ができる&#8212;テキスト ボックスのオブジェクトが、 `Text` (特に) プロパティは、要求オブジェクトが、`Url`プロパティ、電子メール メッセージが、`From`プロパティcustomer オブジェクトであり、`FirstName`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="24611-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="24611-160">オブジェクトには、メソッドも含まれて、&quot;動詞&quot;を実行できます。</span><span class="sxs-lookup"><span data-stu-id="24611-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="24611-161">例としては、ファイル オブジェクトの`Save`メソッドは、イメージ オブジェクトの`Rotate`メソッド、および電子メール オブジェクトの`Send`メソッド。</span><span class="sxs-lookup"><span data-stu-id="24611-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="24611-162">多くの場合、使用する、`Request`オブジェクトを説明するテキスト ボックス (フォームのフィールド) の値などの情報 ページで、ブラウザーの種類は、要求、ページ、ユーザー id などの URL を作成します。次の例のプロパティにアクセスする方法を示しています、`Request`オブジェクトおよび呼び出す方法、`MapPath`のメソッド、`Request`オブジェクトで、サーバーで、ページの絶対パスを確認できます。</span><span class="sxs-lookup"><span data-stu-id="24611-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="24611-163">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="24611-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="24611-165">8.意思決定を行うコードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="24611-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="24611-166">動的な web ページの重要な機能は、条件に基づいて対処方法を決定できます。</span><span class="sxs-lookup"><span data-stu-id="24611-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="24611-167">これを行う最も一般的な方法は、`if`ステートメント (と省略可能な`else`ステートメント)。</span><span class="sxs-lookup"><span data-stu-id="24611-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="24611-168">ステートメント`if(IsPost)`は記事の執筆を簡略化`if(IsPost == true)`します。</span><span class="sxs-lookup"><span data-stu-id="24611-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="24611-169">と共に`if`ステートメント、さまざまなコードのブロックを繰り返し、条件をテストする方法があるしはこの記事の後半で説明されています。</span><span class="sxs-lookup"><span data-stu-id="24611-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="24611-170">結果がブラウザーで表示されます (クリックすると**送信**)。</span><span class="sxs-lookup"><span data-stu-id="24611-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="24611-172">HTTP GET と POST メソッドと IsPost プロパティ</span><span class="sxs-lookup"><span data-stu-id="24611-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="24611-173">Web ページ (HTTP) で使用されるプロトコルは、サーバーに要求を行うために使用するメソッド (動詞) の非常に限られた数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="24611-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="24611-174">2 つの最も一般的なは、ページの読み取りに使用される、GET と POST のページを送信するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="24611-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="24611-175">一般に、初めてユーザーの要求 ページでは、ページの要求が GET を使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="24611-176">ユーザーが、フォームに入力し、送信ボタンをクリックし場合、ブラウザーは POST 要求をサーバーには。</span><span class="sxs-lookup"><span data-stu-id="24611-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="24611-177">Web プログラミング、かどうか、ページを要求する、POST または GET としてページを処理する方法がわかるように、知っておくと便利がよくあります。</span><span class="sxs-lookup"><span data-stu-id="24611-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="24611-178">使用する ASP.NET Web ページで、`IsPost`プロパティを要求が GET または POST がかどうかを参照してください。</span><span class="sxs-lookup"><span data-stu-id="24611-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="24611-179">要求が、投稿の場合、`IsPost`プロパティが true を返すし、フォーム上のテキスト ボックスの値などの読み取りを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="24611-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="24611-180">値に応じて異なる方法でページを処理する方法がわかります多くの例に示します`IsPost`します。</span><span class="sxs-lookup"><span data-stu-id="24611-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="24611-181">単純なコード例</span><span class="sxs-lookup"><span data-stu-id="24611-181">A Simple Code Example</span></span>

<span data-ttu-id="24611-182">この手順では、基本的なプログラミング手法を説明するページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="24611-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="24611-183">例では、次にそれらを追加し、結果が表示されます、2 つの数値を入力できるようにするページを作成します。</span><span class="sxs-lookup"><span data-stu-id="24611-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="24611-184">エディターで、新しいファイルを作成し、名前*AddNumbers.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="24611-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="24611-185">ページでは、既にページ内のあらゆるものを置き換えるには、次のコードとマークアップをコピーします。</span><span class="sxs-lookup"><span data-stu-id="24611-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="24611-186">ここでは、いくつかの点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="24611-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="24611-187">`@`文字 ページで、コードの最初のブロックを開始して、その直後にある、`totalMessage`ページの下部付近に埋め込まれている変数。</span><span class="sxs-lookup"><span data-stu-id="24611-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="24611-188">ページの上部にあるブロックは中かっこで囲まれます。</span><span class="sxs-lookup"><span data-stu-id="24611-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="24611-189">上部にあるブロックでは、すべての行は、セミコロンで終わります。</span><span class="sxs-lookup"><span data-stu-id="24611-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="24611-190">変数`total`、 `num1`、 `num2`、および`totalMessage`複数の数値と文字列を格納します。</span><span class="sxs-lookup"><span data-stu-id="24611-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="24611-191">割り当てられているリテラル文字列値、`totalMessage`変数は、二重引用符内。</span><span class="sxs-lookup"><span data-stu-id="24611-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="24611-192">コードはときに、大文字小文字を区別するため、`totalMessage`ページの下部にある変数を使用すると、その名前では、上部にある変数を正確に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="24611-193">式`num1.AsInt() + num2.AsInt()`オブジェクトおよびメソッドを使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="24611-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="24611-194">`AsInt`各変数のメソッドで算術演算を実行できるように、数値 (整数) に、ユーザーが入力した文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="24611-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="24611-195">`<form>`タグが含まれています、`method="post"`属性。</span><span class="sxs-lookup"><span data-stu-id="24611-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="24611-196">指定のユーザーがクリックしたときに**追加**ページは、HTTP POST メソッドを使用して、サーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="24611-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="24611-197">ページが送信されたときに、 `if(IsPost)` test が true で、条件式が評価される、コードの数値を加算の結果を表示するを実行します。</span><span class="sxs-lookup"><span data-stu-id="24611-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="24611-198">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="24611-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="24611-199">(内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。2 つの整数を入力し、クリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="24611-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="24611-201">基本的なプログラミング概念</span><span class="sxs-lookup"><span data-stu-id="24611-201">Basic Programming Concepts</span></span>

<span data-ttu-id="24611-202">この記事では、ASP.NET web プログラミングの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="24611-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="24611-203">徹底的な検査も、最も頻繁に使用するプログラミングのコンセプトを通じて、クイック ツアーだけではありません。</span><span class="sxs-lookup"><span data-stu-id="24611-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="24611-204">そうであっても、ほとんどの ASP.NET Web Pages を開始する必要がありますが含まれます。</span><span class="sxs-lookup"><span data-stu-id="24611-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="24611-205">しかし、最初にほとんどの技術的な背景。</span><span class="sxs-lookup"><span data-stu-id="24611-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="24611-206">Razor 構文、サーバー コード、および ASP.NET</span><span class="sxs-lookup"><span data-stu-id="24611-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="24611-207">Razor 構文は、web ページにサーバー ベースのコードを埋め込むの単純なプログラミング構文です。</span><span class="sxs-lookup"><span data-stu-id="24611-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="24611-208">Razor 構文を使用する web ページでは、コンテンツの 2 つの種類があります。 クライアントのコンテンツとサーバーのコード。</span><span class="sxs-lookup"><span data-stu-id="24611-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="24611-209">クライアントのコンテンツは、web ページを使用しているもの。 HTML マークアップ (要素) のスタイルを設定、CSS などの情報も JavaScript、およびプレーン テキストなどのいくつかのクライアント スクリプト。</span><span class="sxs-lookup"><span data-stu-id="24611-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="24611-210">Razor 構文を使用して、このクライアントがコンテンツをサーバー コードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="24611-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="24611-211">ページにサーバー コードがある場合、サーバーは、ブラウザーにページを送信する前に最初に、そのコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="24611-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="24611-212">サーバーで実行すると、コードは、はるかに複雑クライアント コンテンツを単独で、サーバー ベースのデータベースへのアクセスなどを使用して行うことができるタスクを実行できます。</span><span class="sxs-lookup"><span data-stu-id="24611-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="24611-213">最も重要なは、サーバー コードをクライアントがコンテンツ動的に作成&#8212;HTML マークアップまたはその他のコンテンツをその場で生成し、ページを含む可能性がある静的な HTML と共にブラウザーに送信します。</span><span class="sxs-lookup"><span data-stu-id="24611-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="24611-214">ブラウザーの観点から、サーバー コードによって生成されるクライアントのコンテンツは、その他のクライアントのコンテンツと同じです。</span><span class="sxs-lookup"><span data-stu-id="24611-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="24611-215">既に説明したように必要なサーバー コードは簡単です。</span><span class="sxs-lookup"><span data-stu-id="24611-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="24611-216">Razor 構文を含む ASP.NET web ページがある特殊なファイル拡張子 (*.cshtml*または *.vbhtml*)。</span><span class="sxs-lookup"><span data-stu-id="24611-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="24611-217">サーバーでは、これらの拡張機能を認識して、、Razor 構文でマークされ、ブラウザーにページを送信するコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="24611-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="24611-218">ここが ASP.NET 適合しますか。</span><span class="sxs-lookup"><span data-stu-id="24611-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="24611-219">Razor 構文は、Microsoft .NET Framework に基づくさらに、ASP.NET と呼ばれる Microsoft からのテクノロジに基づいています。</span><span class="sxs-lookup"><span data-stu-id="24611-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="24611-220">.Net Framework は、任意の種類のコンピューターのアプリケーションのほとんどを開発するための Microsoft から、包括的なプログラミング フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="24611-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="24611-221">ASP.NET は、web アプリケーションを作成するために設計されている .NET Framework の一部です。</span><span class="sxs-lookup"><span data-stu-id="24611-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="24611-222">開発者は、ASP.NET を使用して、世界中で最大かつ最高のトラフィックの web サイトの多くを作成するのにがします。</span><span class="sxs-lookup"><span data-stu-id="24611-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="24611-223">(ファイル名拡張子を表示、いつ *.aspx*サイトの URL の一部として、ASP.NET を使用して、サイトに書き込まれたことを確認します)。</span><span class="sxs-lookup"><span data-stu-id="24611-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="24611-224">Razor 構文では、専門家がいるかどうかは、初心者と生産性の向上を利用を理解しやすくなりますが簡略化された構文を使用して、ASP.NET のすべての電源を使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="24611-225">この構文は簡単に使用できるが、ASP.NET および .NET Framework とファミリの関係は、web サイトが複雑になると、使用できる大規模なフレームワークの機能をあることを意味します。</span><span class="sxs-lookup"><span data-stu-id="24611-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="24611-227">**クラスとインスタンス**</span><span class="sxs-lookup"><span data-stu-id="24611-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="24611-228">ASP.NET サーバー コードでは、クラスの概念に組み込まれているさらに、オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="24611-229">クラスは、定義またはオブジェクトのテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="24611-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="24611-230">たとえば、アプリケーションが含まれる、`Customer`の customer オブジェクトが必要なメソッドとプロパティを定義するクラス。</span><span class="sxs-lookup"><span data-stu-id="24611-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="24611-231">アプリケーションは、実際の顧客情報を使用する必要があるのインスタンスが作成されます (または*をインスタンス化*) customer オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="24611-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="24611-232">個々 の顧客は、別のインスタンス、`Customer`クラス。</span><span class="sxs-lookup"><span data-stu-id="24611-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="24611-233">すべてのインスタンスを同じプロパティとメソッドをサポートしていますが、各 customer オブジェクトが一意であるために、各インスタンスのプロパティの値は通常は異なります。</span><span class="sxs-lookup"><span data-stu-id="24611-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="24611-234">1 人の顧客のオブジェクトで、`LastName`プロパティが"Smith"をする可能性がありますもう 1 つの customer オブジェクトに、`LastName`プロパティで"Jones。"可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24611-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="24611-235">同様に、すべて個別の web ページ サイトでは、`Page`オブジェクトのインスタンスでは、`Page`クラス。</span><span class="sxs-lookup"><span data-stu-id="24611-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="24611-236">ページ上のボタンは、`Button`オブジェクトのインスタンスでは、`Button`クラス、という具合です。</span><span class="sxs-lookup"><span data-stu-id="24611-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="24611-237">各インスタンスがその独自の特性がすべてに基づいているオブジェクトのクラスの定義で指定されています。</span><span class="sxs-lookup"><span data-stu-id="24611-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="24611-238">基本構文</span><span class="sxs-lookup"><span data-stu-id="24611-238">Basic Syntax</span></span>

<span data-ttu-id="24611-239">以前、ASP.NET Web Pages ページを作成する方法と、HTML マークアップをサーバー コードを追加する方法の基本的な例を説明しました。</span><span class="sxs-lookup"><span data-stu-id="24611-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="24611-240">Razor 構文を使用して ASP.NET サーバー コードの記述の基礎を学習ここ&#8212;プログラミング言語の規則では、します。</span><span class="sxs-lookup"><span data-stu-id="24611-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="24611-241">(特に C、C++、C#、Visual Basic、または JavaScript を使用した) 場合のプログラミング経験豊富な場合は、ここで読んだの多くが使い慣れたになります。</span><span class="sxs-lookup"><span data-stu-id="24611-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="24611-242">内のマークアップにサーバー コードを追加する方法でのみについて理解しておく必要がありますおそらく *.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="24611-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="24611-243">テキスト、マークアップ、およびコード ブロック内のコードを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="24611-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="24611-244">サーバー コード ブロックでは、多くの場合、出力テキストまたはマークアップ (またはその両方) のページにします。</span><span class="sxs-lookup"><span data-stu-id="24611-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="24611-245">サーバー コード ブロックにコードでないし、する代わりに表示するか、テキストが含まれている場合、ASP.NET をコードからそのテキストを区別するためにできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="24611-246">これにはいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="24611-246">There are several ways to do this.</span></span>

- <span data-ttu-id="24611-247">ような HTML 要素にテキストを囲む`<p></p>`または`<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="24611-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="24611-248">HTML 要素には、テキスト、HTML 要素の追加、およびサーバー コード式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="24611-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="24611-249">ASP.NET が HTML の開始タグを表示する場合 (たとえば、 `<p>`) としてその内容は、ブラウザーが移動するときに、サーバー コード式を解決するのには、要素を含め、すべてを表示します。</span><span class="sxs-lookup"><span data-stu-id="24611-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="24611-250">使用して、`@:`演算子または`<text>`要素。</span><span class="sxs-lookup"><span data-stu-id="24611-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="24611-251">`@:`プレーン テキストまたは HTML タグが一致していませんが含まれているコンテンツを 1 行の出力、`<text>`要素を出力する複数の行で囲みます。</span><span class="sxs-lookup"><span data-stu-id="24611-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="24611-252">これらのオプションは、出力の一部として HTML 要素を表示したくない場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="24611-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="24611-253">複数のテキストまたは HTML タグが一致しない行を出力する場合は、各行の前に記述できます`@:`、または内の行を囲むことができます、`<text>`要素。</span><span class="sxs-lookup"><span data-stu-id="24611-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="24611-254">ように、`@:`演算子、`<text>`タグのテキスト コンテンツを識別するために、ASP.NET で使用され、ページ出力には常にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="24611-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="24611-255">最初の例では、前の例の繰り返しですがの 1 つのペアを使用して`<text>`タグを表示するテキストを囲みます。</span><span class="sxs-lookup"><span data-stu-id="24611-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="24611-256">2 番目の例で、`<text>`と`</text>`タグは、一部の非包含エンティティのテキストと HTML タグの不一致があるすべての 3 つの行を囲む (`<br />`)、サーバー コードと一致した HTML タグ。</span><span class="sxs-lookup"><span data-stu-id="24611-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="24611-257">ここでも、使用して個別に各の行の前でしたも、`@:`演算子です。 いずれかの方法の動作。</span><span class="sxs-lookup"><span data-stu-id="24611-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="24611-258">このセクションで示すようにテキストを出力するときに&#8212;、HTML 要素を使用して、`@:`演算子、または`<text>`要素&#8212;ASP.NET は HTML エンコード出力します。</span><span class="sxs-lookup"><span data-stu-id="24611-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="24611-259">(ASP.NET がサーバーのコード式とが付いているサーバー コード ブロックの出力をエンコードする前述のように、 `@`、このセクションに記載されている特殊なケースでは可します)。</span><span class="sxs-lookup"><span data-stu-id="24611-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="24611-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="24611-260">Whitespace</span></span>

<span data-ttu-id="24611-261">余分なスペース (および、文字列リテラルの外部で) ステートメントでステートメントには影響しません。</span><span class="sxs-lookup"><span data-stu-id="24611-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="24611-262">ステートメント内での改行は、ステートメントの影響を与えませんし、読みやすくするためのステートメントをラップすることができます。</span><span class="sxs-lookup"><span data-stu-id="24611-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="24611-263">次のステートメントでは、同じです。</span><span class="sxs-lookup"><span data-stu-id="24611-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="24611-264">ただし、文字列リテラル内の行をラップすることはできません。</span><span class="sxs-lookup"><span data-stu-id="24611-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="24611-265">次の例が機能しません。</span><span class="sxs-lookup"><span data-stu-id="24611-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="24611-266">上記のコードのような複数の行に折り返す長い文字列を結合するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="24611-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="24611-267">連結演算子を使用することができます (`+`)、この記事の後半でがあります。</span><span class="sxs-lookup"><span data-stu-id="24611-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="24611-268">使用することも、`@`逐語的文字列リテラルの作成、この記事の前半で説明するようにする文字。</span><span class="sxs-lookup"><span data-stu-id="24611-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="24611-269">Verbatim 文字列リテラルを分割することで、境界をまたがってできます。</span><span class="sxs-lookup"><span data-stu-id="24611-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="24611-270">コード (およびマークアップ) コメント</span><span class="sxs-lookup"><span data-stu-id="24611-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="24611-271">コメントを使用して、自分自身または他のユーザーは、ノートをそのまま使用できます。</span><span class="sxs-lookup"><span data-stu-id="24611-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="24611-272">これらも無効にすること (*をコメント アウト*) コードまたはを実行する必要はありませんが、当面は、ページに保持するマークアップのセクション。</span><span class="sxs-lookup"><span data-stu-id="24611-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="24611-273">別の構文と、HTML マークアップの Razor コードのコメントがあります。</span><span class="sxs-lookup"><span data-stu-id="24611-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="24611-274">すべての Razor コードでは、Razor コメントは処理 (と同様、削除)、ページがブラウザーに送信される前に、サーバー上。</span><span class="sxs-lookup"><span data-stu-id="24611-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="24611-275">そのため、Razor コメントの構文では、そのファイルを編集するが、ページのソースであっても、ユーザーが表示されないときに表示できますコメントをコードに (または、さらには、マークアップ) を挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="24611-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="24611-276">ASP.NET Razor のコメントのコメントを開始する`@*`で終わら`*@`します。</span><span class="sxs-lookup"><span data-stu-id="24611-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="24611-277">コメントは、1 つの行または複数の行のできます。</span><span class="sxs-lookup"><span data-stu-id="24611-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="24611-278">コード ブロック内のコメントを次に示します。</span><span class="sxs-lookup"><span data-stu-id="24611-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="24611-279">ここでは行のコードで、コードの同じブロック コメント アウトされて実行されないようにします。</span><span class="sxs-lookup"><span data-stu-id="24611-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="24611-280">Razor コメントの構文を使用する代わりにコード ブロック内では c# などを使用するプログラミング言語のコメント構文を使用できます。</span><span class="sxs-lookup"><span data-stu-id="24611-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="24611-281">C# の場合は、単一行コメントが付いて、`//`文字、および複数行コメントの始まり`/*`と末尾`*/`します。</span><span class="sxs-lookup"><span data-stu-id="24611-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="24611-282">(Razor のコメントと同様 c# コメントは、ブラウザーにレンダリングされません)。</span><span class="sxs-lookup"><span data-stu-id="24611-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="24611-283">マークアップの場合、ご存知のとおり、HTML コメントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="24611-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="24611-284">HTML コメントの始まり`<!--`文字、末尾`-->`します。</span><span class="sxs-lookup"><span data-stu-id="24611-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="24611-285">だけでなく、テキスト、HTML マークアップもに、ページを保持することがありますが、表示するためにしたくないを囲む HTML コメントを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="24611-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="24611-286">この HTML コメントには、タグとが含まれているテキストの内容全体が非表示になります。</span><span class="sxs-lookup"><span data-stu-id="24611-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="24611-287">Razor コメント、HTML コメントとは異なり*は*ページに表示されると、ユーザーがページのソースを表示して確認できます。</span><span class="sxs-lookup"><span data-stu-id="24611-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="24611-288">Razor では、c# の入れ子になったブロックに制限があります。</span><span class="sxs-lookup"><span data-stu-id="24611-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="24611-289">詳細については、次を参照してください[という名前の c# 変数と入れ子になったブロック壊れたコードの生成。](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="24611-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="24611-290">変数</span><span class="sxs-lookup"><span data-stu-id="24611-290">Variables</span></span>

<span data-ttu-id="24611-291">変数は、データの格納に使用する名前付きオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="24611-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="24611-292">名前を付ける変数、何かが、名前がアルファベットの文字で始める必要があり、空白文字または予約文字を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="24611-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="24611-293">変数とデータ型</span><span class="sxs-lookup"><span data-stu-id="24611-293">Variables and Data Types</span></span>

<span data-ttu-id="24611-294">変数は、データの種類が変数に格納されていることを示しますが、特定のデータ型を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="24611-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="24611-295">文字列値を格納する文字列変数があることができます (よう&quot;Hello world&quot;)、(3 または 79) などの整数値を格納する整数変数とさまざまな (4/12/2012 や 2009 年 3 月などの形式で日付の値を格納する date 型の変数).</span><span class="sxs-lookup"><span data-stu-id="24611-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="24611-296">その他の多くのデータ型を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="24611-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="24611-297">ただし、通常、変数の型を指定するがありません。</span><span class="sxs-lookup"><span data-stu-id="24611-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="24611-298">ほとんどの場合、ASP.NET は、変数内のデータの使用方法に基づいて型を判断することができます。</span><span class="sxs-lookup"><span data-stu-id="24611-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="24611-299">(場合によっては、型を指定する必要があります。 これは true、例が表示されます)。</span><span class="sxs-lookup"><span data-stu-id="24611-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="24611-300">使用して変数を宣言する、`var`キーワード (型を指定しない) 場合や、型の名前を使用しています。</span><span class="sxs-lookup"><span data-stu-id="24611-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="24611-301">次の例では、web ページで変数の一般的な使用を示します。</span><span class="sxs-lookup"><span data-stu-id="24611-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="24611-302">ページで、前の例を結合すると、ブラウザーで表示されます。 これが表示されます。</span><span class="sxs-lookup"><span data-stu-id="24611-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="24611-304">変換して、データ型をテストします。</span><span class="sxs-lookup"><span data-stu-id="24611-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="24611-305">ASP.NET は、データ型を自動的に決定することができます、通常が場合があります、ことはできません。</span><span class="sxs-lookup"><span data-stu-id="24611-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="24611-306">そのため、明示的な変換を実行することで ASP.NET を手助けする必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="24611-307">型変換を持っていない場合でもデータの種類が使用することを確認するテストに便利です。</span><span class="sxs-lookup"><span data-stu-id="24611-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="24611-308">最も一般的なは、整数や日付など、別の型を文字列に変換することがあります。</span><span class="sxs-lookup"><span data-stu-id="24611-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="24611-309">次の例では、文字列を数値に変換する必要があります、一般的なケースを示します。</span><span class="sxs-lookup"><span data-stu-id="24611-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="24611-310">原則として、ユーザー入力では、文字列として。</span><span class="sxs-lookup"><span data-stu-id="24611-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="24611-311">番号を入力するユーザーを要求した場合でも、および数字をユーザー入力が送信され、コードでの読み取り時に入力する場合でも、データは文字列の形式です。</span><span class="sxs-lookup"><span data-stu-id="24611-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="24611-312">そのため、文字列を数値に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="24611-313">例では、変換、しなくても、値に対して算術演算を実行しようとする場合、次のエラーの結果、ASP.NET は、2 つの文字列を追加できないため。</span><span class="sxs-lookup"><span data-stu-id="24611-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="24611-314">*'Int' に ' string' 型に暗黙的に変換できません。*</span><span class="sxs-lookup"><span data-stu-id="24611-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="24611-315">呼び出すの値を整数に変換する、`AsInt`メソッド。</span><span class="sxs-lookup"><span data-stu-id="24611-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="24611-316">変換が成功した場合は、数値を追加できます。</span><span class="sxs-lookup"><span data-stu-id="24611-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="24611-317">次の表では、変数の一般的ないくつかの変換とテスト方法を示します。</span><span class="sxs-lookup"><span data-stu-id="24611-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="24611-318"><strong>メソッド</strong></span><span class="sxs-lookup"><span data-stu-id="24611-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-319"><strong>説明</strong></span><span class="sxs-lookup"><span data-stu-id="24611-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-320"><strong>例</strong></span><span class="sxs-lookup"><span data-stu-id="24611-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-321">整数 (「593」) などの整数を表す文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="24611-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-322">などの文字列に変換します&quot;true&quot;または&quot;false&quot;ブール型にします。</span><span class="sxs-lookup"><span data-stu-id="24611-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-323">ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を浮動小数点数。</span><span class="sxs-lookup"><span data-stu-id="24611-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-324">ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot; 10 進数。</span><span class="sxs-lookup"><span data-stu-id="24611-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="24611-325">(ASP.NET では、10 進数、浮動小数点数よりも正確)。</span><span class="sxs-lookup"><span data-stu-id="24611-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-326">Asp.net の日付と時刻の値を表す文字列に変換します`DateTime`型。</span><span class="sxs-lookup"><span data-stu-id="24611-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-327">その他の任意のデータ型を文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="24611-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="24611-328">演算子</span><span class="sxs-lookup"><span data-stu-id="24611-328">Operators</span></span>

<span data-ttu-id="24611-329">演算子は、キーワードまたは式の中で実行するコマンドの種類を ASP.NET に指示する文字です。</span><span class="sxs-lookup"><span data-stu-id="24611-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="24611-330">C# 言語 (とそれに基づいている Razor 構文) は、多くの演算子をサポートしていますが、開始するいくつかを認識するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="24611-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="24611-331">次の表では、最も一般的な演算子をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="24611-331">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
    <span data-ttu-id="24611-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="24611-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-333"><strong>説明</strong></span><span class="sxs-lookup"><span data-stu-id="24611-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-334"><strong>例</strong></span><span class="sxs-lookup"><span data-stu-id="24611-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-335">算術演算子が数値式で使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-335">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-336">代入。</span><span class="sxs-lookup"><span data-stu-id="24611-336">Assignment.</span></span> <span data-ttu-id="24611-337">左側にあるオブジェクトには、ステートメントの右側にある値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="24611-337">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-338">等値。</span><span class="sxs-lookup"><span data-stu-id="24611-338">Equality.</span></span> <span data-ttu-id="24611-339">返します`true`値が等しい場合。</span><span class="sxs-lookup"><span data-stu-id="24611-339">Returns `true` if the values are equal.</span></span> <span data-ttu-id="24611-340">(上での違いに注意してください、`=`演算子と`==`演算子です)。</span><span class="sxs-lookup"><span data-stu-id="24611-340">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-341">非等値。</span><span class="sxs-lookup"><span data-stu-id="24611-341">Inequality.</span></span> <span data-ttu-id="24611-342">返します`true`値が等しくない場合。</span><span class="sxs-lookup"><span data-stu-id="24611-342">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-343">以下のより大きい-より、小さいよりも-または-等しく、大きい-よりも-または-等しい。</span><span class="sxs-lookup"><span data-stu-id="24611-343">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-344">連結、文字列を結合するために使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-344">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="24611-345">ASP.NET では、この演算子と式のデータ型に基づく加算演算子の違いを認識します。</span><span class="sxs-lookup"><span data-stu-id="24611-345">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-346">インクリメントとデクリメント演算子を追加し、変数から 1 をそれぞれ減算します。</span><span class="sxs-lookup"><span data-stu-id="24611-346">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-347">ドットです。</span><span class="sxs-lookup"><span data-stu-id="24611-347">Dot.</span></span> <span data-ttu-id="24611-348">オブジェクトとそのプロパティおよびメソッドを区別するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="24611-348">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-349">かっこです。</span><span class="sxs-lookup"><span data-stu-id="24611-349">Parentheses.</span></span> <span data-ttu-id="24611-350">グループ式をメソッドにパラメーターを渡すために使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-350">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-351">角かっこです。</span><span class="sxs-lookup"><span data-stu-id="24611-351">Brackets.</span></span> <span data-ttu-id="24611-352">配列またはコレクション内の値にアクセスするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="24611-352">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-353">じゃない。</span><span class="sxs-lookup"><span data-stu-id="24611-353">Not.</span></span> <span data-ttu-id="24611-354">逆に、`true`値を`false`またはその逆です。</span><span class="sxs-lookup"><span data-stu-id="24611-354">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="24611-355">通常のテストを簡略化として使用される`false`(には、いない`true`)。</span><span class="sxs-lookup"><span data-stu-id="24611-355">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
    <span data-ttu-id="24611-356">論理 AND またはと、条件をまとめてリンクに使用されます。</span><span class="sxs-lookup"><span data-stu-id="24611-356">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="24611-357">ファイルとコード内のフォルダー パスを使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-357">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="24611-358">コードでは、ファイルとフォルダーのパスを持つ多くの場合、操作します。</span><span class="sxs-lookup"><span data-stu-id="24611-358">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="24611-359">開発用コンピューターに表示される次の web サイトの物理フォルダー構造の例に示します。</span><span class="sxs-lookup"><span data-stu-id="24611-359">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="24611-360">Url およびパスについていくつかの重要な詳細情報を次に示します。</span><span class="sxs-lookup"><span data-stu-id="24611-360">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="24611-361">URL のいずれかでドメイン名を開始 (`http://www.example.com`) またはサーバー名 (`http://localhost`、 `http://mycomputer`)。</span><span class="sxs-lookup"><span data-stu-id="24611-361">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="24611-362">URL は、ホスト コンピューター上の物理パスに対応します。</span><span class="sxs-lookup"><span data-stu-id="24611-362">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="24611-363">たとえば、`http://myserver`フォルダーに対応する*C:\websites\mywebsite*サーバー。</span><span class="sxs-lookup"><span data-stu-id="24611-363">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="24611-364">仮想パスは、完全なパスを指定せずに、コード内のパスを表すための短縮形です。</span><span class="sxs-lookup"><span data-stu-id="24611-364">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="24611-365">ドメインまたはサーバー名に続く URL の一部が含まれています。</span><span class="sxs-lookup"><span data-stu-id="24611-365">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="24611-366">仮想パスを使用すると、パスを更新することがなく別のドメインまたはサーバーに、コードを移動できます。</span><span class="sxs-lookup"><span data-stu-id="24611-366">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="24611-367">相違点の理解に役立つ例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="24611-367">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="24611-368">完全な URL</span><span class="sxs-lookup"><span data-stu-id="24611-368">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="24611-369">サーバー名</span><span class="sxs-lookup"><span data-stu-id="24611-369">Server name</span></span> | <span data-ttu-id="24611-370">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="24611-370">*mycompanyserver*</span></span> |
| <span data-ttu-id="24611-371">[仮想パス]</span><span class="sxs-lookup"><span data-stu-id="24611-371">Virtual path</span></span> | <span data-ttu-id="24611-372">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="24611-372">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="24611-373">物理パス</span><span class="sxs-lookup"><span data-stu-id="24611-373">Physical path</span></span> | <span data-ttu-id="24611-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="24611-374">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="24611-375">仮想ルートは、/、ドライブは、c: のルートと同じように \ です。</span><span class="sxs-lookup"><span data-stu-id="24611-375">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="24611-376">(仮想フォルダーのパス常にスラッシュを使用します。)フォルダーの仮想パスとして、物理フォルダー。 同じ名前を指定する必要はありません。エイリアスがあります。</span><span class="sxs-lookup"><span data-stu-id="24611-376">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="24611-377">(実稼働サーバー上の仮想パスことはほとんどありませんと一致する特定の物理パス。)</span><span class="sxs-lookup"><span data-stu-id="24611-377">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="24611-378">コードのファイルとフォルダーを使用する場合は場合がありますの物理パスおよび場合によってはどのようなオブジェクトを使用しているに応じての仮想パスを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-378">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="24611-379">ASP.NET ツールを提供するこれらのコードでのファイルとフォルダーのパスの作業:`Server.MapPath`メソッド、および`~`演算子と`Href`メソッド。</span><span class="sxs-lookup"><span data-stu-id="24611-379">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="24611-380">仮想物理パスに変換する: Server.MapPath メソッド</span><span class="sxs-lookup"><span data-stu-id="24611-380">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="24611-381">`Server.MapPath`メソッドが仮想パスに変換します (など */default.cshtml*) を絶対物理パス (など*C:\WebSites\MyWebSiteFolder\default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="24611-381">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="24611-382">このメソッドを使用すると、いつでも完全な物理パスを作成する必要がありますに。</span><span class="sxs-lookup"><span data-stu-id="24611-382">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="24611-383">典型的な例では、読み取りまたはテキスト ファイルまたは web サーバー上の画像ファイルを作成する場合です。</span><span class="sxs-lookup"><span data-stu-id="24611-383">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="24611-384">通常ホスティング サイトのサーバー上のサイトの絶対物理パスがわからない、わかっているため、このメソッドは、パスを変換できます: 仮想パス: サーバー上の対応するパスにします。</span><span class="sxs-lookup"><span data-stu-id="24611-384">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="24611-385">仮想パスをファイルまたはフォルダー、メソッドに渡すし、物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="24611-385">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="24611-386">仮想ルートを参照する: ~ 演算子と Href メソッド</span><span class="sxs-lookup"><span data-stu-id="24611-386">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="24611-387">*.Cshtml*または *.vbhtml*ファイル、仮想ルート パスを使用して、参照することができます、`~`演算子。</span><span class="sxs-lookup"><span data-stu-id="24611-387">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="24611-388">これは、機能は、サイトでは、内のページを移動することができ、他のページが含まれているすべてのリンクが壊れているできませんので、非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="24611-388">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="24611-389">これまで、web サイトを別の場所に移動する場合にも便利です。</span><span class="sxs-lookup"><span data-stu-id="24611-389">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="24611-390">次にいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="24611-390">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="24611-391">場合は、web サイトが`http://myserver/myapp`、ここでは、ASP.NET が処理する方法これらのパス、ページの実行時に。</span><span class="sxs-lookup"><span data-stu-id="24611-391">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="24611-392">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="24611-392">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="24611-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="24611-393">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="24611-394">(実際には、変数の値としてこれらのパスは表示されませんが ASP.NET は、パスとして扱うだったです)。</span><span class="sxs-lookup"><span data-stu-id="24611-394">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="24611-395">使用することができます、 `~` (上記) としてサーバー コードと次のように、マークアップの両方の演算子。</span><span class="sxs-lookup"><span data-stu-id="24611-395">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="24611-396">マークアップでは、使用する、`~`演算子イメージ ファイル、その他の web ページ、および CSS ファイルなどのリソースへのパスを作成します。</span><span class="sxs-lookup"><span data-stu-id="24611-396">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="24611-397">ASP.NET が (コードとマークアップの両方) のページを探し、すべて解決、ページの実行時に、`~`適切なパスへの参照。</span><span class="sxs-lookup"><span data-stu-id="24611-397">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="24611-398">条件付きロジックやループ</span><span class="sxs-lookup"><span data-stu-id="24611-398">Conditional Logic and Loops</span></span>

<span data-ttu-id="24611-399">ASP.NET サーバー コードでは、条件に基づいてタスクを実行して、特定回数のステートメントを繰り返し実行するコードを記述することができます (つまり、ループを実行するコード)。</span><span class="sxs-lookup"><span data-stu-id="24611-399">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="24611-400">条件のテスト</span><span class="sxs-lookup"><span data-stu-id="24611-400">Testing Conditions</span></span>

<span data-ttu-id="24611-401">使用する単純な条件をテストする、`if`ステートメントで、指定したテストに基づく true または false を返します。</span><span class="sxs-lookup"><span data-stu-id="24611-401">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="24611-402">`if`キーワードは、ブロックを開始します。</span><span class="sxs-lookup"><span data-stu-id="24611-402">The `if` keyword starts a block.</span></span> <span data-ttu-id="24611-403">実際のテスト (条件) はかっこであり、true または false を返します。</span><span class="sxs-lookup"><span data-stu-id="24611-403">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="24611-404">テストが true の場合に実行されるステートメントは、中かっこで囲まれます。</span><span class="sxs-lookup"><span data-stu-id="24611-404">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="24611-405">`if`ステートメントを含めることができます、`else`ブロックする条件が false の場合に実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="24611-405">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="24611-406">使用して複数の条件を追加することができます、`else if`ブロック。</span><span class="sxs-lookup"><span data-stu-id="24611-406">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="24611-407">場合、最初の条件の場合、この例ではブロックが true でない、`else if`条件がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="24611-407">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="24611-408">その条件が満たされた場合、ステートメントで、`else if`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="24611-408">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="24611-409">どの条件が満たされた場合、ステートメントで、`else`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="24611-409">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="24611-410">場合、他の任意の数を追加する、ブロックを閉じて、`else`としてブロック、&quot;他のすべて&quot;条件。</span><span class="sxs-lookup"><span data-stu-id="24611-410">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="24611-411">多くの条件をテストするには、使用、`switch`ブロック。</span><span class="sxs-lookup"><span data-stu-id="24611-411">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="24611-412">テストする値がかっこ内に示します (例では、`weekday`変数)。</span><span class="sxs-lookup"><span data-stu-id="24611-412">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="24611-413">各テストを使用して、`case`コロン (:) で終了するステートメント。</span><span class="sxs-lookup"><span data-stu-id="24611-413">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="24611-414">場合の値を`case`ステートメントには、テスト値が一致すると、その case ブロック内のコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="24611-414">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="24611-415">各 case ステートメントで閉じる、`break`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="24611-415">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="24611-416">(それぞれに区切りを含めるを忘れた場合`case`次のコードをブロックします`case`ステートメントも実行されます。)。A`switch`ブロックには多くの場合、`default`ステートメントの最後のケースとして、&quot;他のすべて&quot;該当するその他のケースがない場合に実行するオプション。</span><span class="sxs-lookup"><span data-stu-id="24611-416">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="24611-417">ブラウザーで表示される最後の 2 つの条件付きブロックの結果:</span><span class="sxs-lookup"><span data-stu-id="24611-417">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="24611-419">ループのコード</span><span class="sxs-lookup"><span data-stu-id="24611-419">Looping Code</span></span>

<span data-ttu-id="24611-420">多くの場合、同じステートメントを繰り返し実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-420">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="24611-421">そのためには、ループ処理します。</span><span class="sxs-lookup"><span data-stu-id="24611-421">You do this by looping.</span></span> <span data-ttu-id="24611-422">たとえば、多くの場合、ステートメントを実行する同じ項目ごとにデータのコレクション。</span><span class="sxs-lookup"><span data-stu-id="24611-422">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="24611-423">使用することが正確に何回ループすることがわかっている場合、`for`ループします。</span><span class="sxs-lookup"><span data-stu-id="24611-423">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="24611-424">この種のループはまでカウントするか、カウント ダウン特に便利です。</span><span class="sxs-lookup"><span data-stu-id="24611-424">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="24611-425">ループが始まり、`for`キーワード、かっこ、3 つのステートメントに続く各をセミコロンで終了します。</span><span class="sxs-lookup"><span data-stu-id="24611-425">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="24611-426">最初のステートメントをかっこ内 (`var i=10;`) カウンターを作成し、10 を初期化します。</span><span class="sxs-lookup"><span data-stu-id="24611-426">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="24611-427">カウンターの名前を付ける必要はありません`i`&#8212;任意の変数を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="24611-427">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="24611-428">ときに、`for`ループが実行され、カウンターが自動的にインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="24611-428">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="24611-429">2 番目のステートメント (`i < 21;`) 条件をカウントする距離を設定します。</span><span class="sxs-lookup"><span data-stu-id="24611-429">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="24611-430">20 個に移動させたいここでは、(これは、引き続き、カウンターは 21 未満)。</span><span class="sxs-lookup"><span data-stu-id="24611-430">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="24611-431">3 番目のステートメント (`i++` ) カウンターは、ループが実行されるたびに追加する 1 である必要がありますを指定するだけインクリメント演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-431">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="24611-432">中かっこ内のループの反復ごとに実行されるコードに示します。</span><span class="sxs-lookup"><span data-stu-id="24611-432">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="24611-433">マークアップは、新しい段落を作成する (`<p>`要素) 時間し、の値を表示する出力に行を追加します。 各`i`(カウンター)。</span><span class="sxs-lookup"><span data-stu-id="24611-433">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="24611-434">このページを実行すると、例は、11 行の行のテキスト項目の数を示す行ごとに、出力の表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="24611-434">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="24611-436">多くの場合、使用、コレクションまたは配列で作業している場合、`foreach`ループします。</span><span class="sxs-lookup"><span data-stu-id="24611-436">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="24611-437">コレクションは、類似のオブジェクトのグループと`foreach`ループでコレクション内の各項目のタスクを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="24611-437">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="24611-438">この種のループはコレクションの場合、便利なためとは異なり、`for`ループ カウンターを増分または制限を設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="24611-438">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="24611-439">代わりに、`foreach`ループのコードだけを進め、コレクションが完了するまでです。</span><span class="sxs-lookup"><span data-stu-id="24611-439">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="24611-440">たとえば、次のコードが内の項目を返します、`Request.ServerVariables`コレクションで、web サーバーに関する情報を含むオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="24611-440">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="24611-441">使用して、`foreac`新たに作成して各項目の名前を表示する h ループ`<li>`HTML の箇条書きリスト内の要素。</span><span class="sxs-lookup"><span data-stu-id="24611-441">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="24611-442">`foreach`キーワードに続けてかっこ、コレクション内の 1 つの項目を表す変数を宣言する (例では、 `var item`)、その後に、`in`キーワードのコレクションをループするか後にします。</span><span class="sxs-lookup"><span data-stu-id="24611-442">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="24611-443">本体で、`foreach`ループ、先ほど宣言した変数を使用して現在の項目にアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="24611-443">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="24611-445">汎用的なループを作成するには、使用、`while`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="24611-445">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="24611-446">A`while`でループが開始、`while`キーワード、その後にかっこ、ループの継続期間を指定する (ここでは、限り`countNum`50 未満) を繰り返すブロックします。</span><span class="sxs-lookup"><span data-stu-id="24611-446">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="24611-447">ループの通常のインクリメント (に追加する) または減分 (減算)、変数またはオブジェクト カウントで使用されます。</span><span class="sxs-lookup"><span data-stu-id="24611-447">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="24611-448">例では、`+=`演算子に 1 を追加します`countNum`たびに、ループを実行します。</span><span class="sxs-lookup"><span data-stu-id="24611-448">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="24611-449">(カウント ダウンをループ内の変数をデクリメントするは、デクリメント演算子を使用して`-=`)。</span><span class="sxs-lookup"><span data-stu-id="24611-449">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="24611-450">オブジェクトとコレクション</span><span class="sxs-lookup"><span data-stu-id="24611-450">Objects and Collections</span></span>

<span data-ttu-id="24611-451">ほぼすべての ASP.NET web サイトでは、web ページ自体を含むオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="24611-451">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="24611-452">このセクションでは、使用する多くの場合、コードでいくつかの重要なオブジェクトについて説明します。</span><span class="sxs-lookup"><span data-stu-id="24611-452">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="24611-453">ページのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="24611-453">Page Objects</span></span>

<span data-ttu-id="24611-454">ASP.NET の最も基本的なオブジェクトは、ページです。</span><span class="sxs-lookup"><span data-stu-id="24611-454">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="24611-455">オブジェクトを修飾せずに直接ページ オブジェクトのプロパティにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="24611-455">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="24611-456">次のコード ページのファイルのパスを取得するを使用して、`Request`ページのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="24611-456">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="24611-457">ようにクリアのプロパティやメソッド、現在のページを参照していることができます必要に応じてキーワードを使用する`this`を表す、コード内のページ オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="24611-457">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="24611-458">上記のコード例を次に示します`this`を追加して、ページを表します。</span><span class="sxs-lookup"><span data-stu-id="24611-458">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="24611-459">プロパティを使用して、`Page`など多くの情報を取得するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="24611-459">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="24611-460">`Request`。</span><span class="sxs-lookup"><span data-stu-id="24611-460">`Request`.</span></span> <span data-ttu-id="24611-461">既に説明したように行われる要求、ページ、ユーザー id などの URL をブラウザーの種類を含む、現在の要求に関する情報のコレクションになります。</span><span class="sxs-lookup"><span data-stu-id="24611-461">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="24611-462">`Response`。</span><span class="sxs-lookup"><span data-stu-id="24611-462">`Response`.</span></span> <span data-ttu-id="24611-463">これは、サーバー コードの実行が終了したら、ブラウザーに送信される応答 (ページ) に関する情報のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="24611-463">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="24611-464">たとえば、このプロパティを使用すると、応答に情報を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="24611-464">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="24611-465">コレクション オブジェクト (配列、ディクショナリ)</span><span class="sxs-lookup"><span data-stu-id="24611-465">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="24611-466">A*コレクション*のコレクションなど、同じ型のオブジェクトのグループは、`Customer`データベースからのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="24611-466">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="24611-467">このような ASP.NET に多くの組み込みコレクションが含まれています、`Request.Files`コレクション。</span><span class="sxs-lookup"><span data-stu-id="24611-467">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="24611-468">多くの場合、コレクション内のデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="24611-468">You'll often work with data in collections.</span></span> <span data-ttu-id="24611-469">2 つの一般的なコレクション型は、*配列*と*ディクショナリ*します。</span><span class="sxs-lookup"><span data-stu-id="24611-469">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="24611-470">配列は、類似した項目のコレクションを格納するが、それぞれ個別の各項目を保持する変数を作成したくない場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="24611-470">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="24611-471">配列の使用、特定のデータ型を宣言するなど`string`、 `int`、または`DateTime`します。</span><span class="sxs-lookup"><span data-stu-id="24611-471">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="24611-472">変数が配列を含めることができます、宣言に角かっこを追加することを示すために (など`string[]`または`int[]`)。</span><span class="sxs-lookup"><span data-stu-id="24611-472">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="24611-473">またはを使用してそれらの位置 (インデックス) を使用して、配列内の項目にアクセスすることができます、`foreach`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="24611-473">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="24611-474">配列のインデックスは 0 から始まる&#8212;つまり最初の項目がある 0 の位置は、2 番目の項目は、位置 1、という具合にします。</span><span class="sxs-lookup"><span data-stu-id="24611-474">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="24611-475">取得することによって、配列内の項目の数を判断するその`Length`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="24611-475">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="24611-476">(配列を検索します)、配列内の特定の項目の位置を取得する、`Array.IndexOf`メソッド。</span><span class="sxs-lookup"><span data-stu-id="24611-476">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="24611-477">行うことができますも逆の順序など、配列の内容 (、`Array.Reverse`メソッド) または内容を並べ替える (、`Array.Sort`メソッド)。</span><span class="sxs-lookup"><span data-stu-id="24611-477">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="24611-478">ブラウザーで表示されている文字列配列コードの出力:</span><span class="sxs-lookup"><span data-stu-id="24611-478">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="24611-480">ディクショナリは設定または対応する値を取得するキー (または名前) を指定するキー/値のペアのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="24611-480">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="24611-481">使用するディクショナリを作成する、`new`新しいディクショナリ オブジェクトを作成していることを示すキーワードです。</span><span class="sxs-lookup"><span data-stu-id="24611-481">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="24611-482">変数を使用するディクショナリを割り当てることができます、`var`キーワード。</span><span class="sxs-lookup"><span data-stu-id="24611-482">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="24611-483">山かっこを使用して、ディクショナリ内の項目のデータ型を指定する ( `< >` )。</span><span class="sxs-lookup"><span data-stu-id="24611-483">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="24611-484">宣言の末尾には、これは実際には、新しいディクショナリを作成するメソッドなので 1 組のかっこを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-484">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="24611-485">項目をディクショナリに追加するを呼び出すことができます、`Add`辞書の変数のメソッド (`myScores`ここでは)、キーと値を指定します。</span><span class="sxs-lookup"><span data-stu-id="24611-485">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="24611-486">または、次の例のように、単純な割り当てを行い、キーを示すに角かっこを使用できます。</span><span class="sxs-lookup"><span data-stu-id="24611-486">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="24611-487">ディクショナリから値を取得するには、角かっこでキーを指定します。</span><span class="sxs-lookup"><span data-stu-id="24611-487">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="24611-488">パラメーターを持つメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="24611-488">Calling Methods with Parameters</span></span>

<span data-ttu-id="24611-489">この記事で前述の読み取りとプログラミングでのオブジェクトはメソッドを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="24611-489">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="24611-490">たとえば、`Database`オブジェクトがあります、`Database.Connect`メソッド。</span><span class="sxs-lookup"><span data-stu-id="24611-490">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="24611-491">多くのメソッドは、1 つまたは複数のパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="24611-491">Many methods also have one or more parameters.</span></span> <span data-ttu-id="24611-492">A*パラメーター*メソッドに渡す値は、タスクが完了する方法を有効にします。</span><span class="sxs-lookup"><span data-stu-id="24611-492">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="24611-493">たとえばの宣言を見て、`Request.MapPath`メソッドは、次の 3 つのパラメーターを受け取る。</span><span class="sxs-lookup"><span data-stu-id="24611-493">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="24611-494">(読みやすくする、行に折り返されています。</span><span class="sxs-lookup"><span data-stu-id="24611-494">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="24611-495">ほぼすべての場所内で文字列を除いて、改行を配置できることに注意してください引用符で囲まれた。)。</span><span class="sxs-lookup"><span data-stu-id="24611-495">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="24611-496">このメソッドは、指定した仮想パスに対応するサーバー上の物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="24611-496">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="24611-497">メソッドの 3 つのパラメーターは`virtualPath`、 `baseVirtualDir`、および`allowCrossAppMapping`します。</span><span class="sxs-lookup"><span data-stu-id="24611-497">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="24611-498">(宣言で、パラメーターは許可されるデータのデータ型の一覧に注目してください)。このメソッドを呼び出すときに、次の 3 つのすべてのパラメーターの値を指定してください。</span><span class="sxs-lookup"><span data-stu-id="24611-498">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="24611-499">Razor 構文は、メソッドにパラメーターを渡すための 2 つのオプションを使用:*位置指定パラメーター*と*名前付きパラメーター*します。</span><span class="sxs-lookup"><span data-stu-id="24611-499">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="24611-500">位置指定パラメーターを使用してメソッドを呼び出すには、メソッドの宣言で指定されている厳密な順序でパラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="24611-500">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="24611-501">(が通常わかるこの順序メソッドのドキュメントを参照しています。)順序に従い、任意のパラメーターをスキップすることはできません&#8212;かどうか、必要に応じて空の文字列を渡す (`""`) または`null`位置指定パラメーターの値がないのです。</span><span class="sxs-lookup"><span data-stu-id="24611-501">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="24611-502">次の例は、という名前のフォルダーにあることを前提*スクリプト*web サイトにします。</span><span class="sxs-lookup"><span data-stu-id="24611-502">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="24611-503">コードの呼び出し、`Request.MapPath`を正しい順序で 3 つのパラメーターの値をメソッドを渡します。</span><span class="sxs-lookup"><span data-stu-id="24611-503">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="24611-504">結果のマップされたパスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="24611-504">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="24611-505">メソッドは、多くのパラメーターを持ち、ときにするおくと、コードより読みやすい名前付きパラメーターを使用しています。</span><span class="sxs-lookup"><span data-stu-id="24611-505">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="24611-506">名前付きパラメーターを使用してメソッドを呼び出すには、パラメーター名とそれに続くコロン (:)、し、値を指定します。</span><span class="sxs-lookup"><span data-stu-id="24611-506">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="24611-507">名前付きパラメーターの利点は、任意の順序で渡すできることです。</span><span class="sxs-lookup"><span data-stu-id="24611-507">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="24611-508">(欠点は、メソッドの呼び出しが、compact ではないこと) です。</span><span class="sxs-lookup"><span data-stu-id="24611-508">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="24611-509">次の例は、上記と同じメソッドを呼び出しますが、名前付きの値を指定するパラメーターの使用。</span><span class="sxs-lookup"><span data-stu-id="24611-509">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="24611-510">ご覧のように、パラメーターは異なる順序で渡されます。</span><span class="sxs-lookup"><span data-stu-id="24611-510">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="24611-511">ただし、前の例と、この例を実行する場合、同じ値を返すします。</span><span class="sxs-lookup"><span data-stu-id="24611-511">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="24611-512">エラー処理</span><span class="sxs-lookup"><span data-stu-id="24611-512">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="24611-513">Try-catch ステートメント</span><span class="sxs-lookup"><span data-stu-id="24611-513">Try-Catch Statements</span></span>

<span data-ttu-id="24611-514">上の理由から、コントロールの外部に障害が発生する、コード内のステートメントを多くの場合、があります。</span><span class="sxs-lookup"><span data-stu-id="24611-514">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="24611-515">例えば:</span><span class="sxs-lookup"><span data-stu-id="24611-515">For example:</span></span>

- <span data-ttu-id="24611-516">コードを作成またはファイルにアクセスしようとすると、あらゆる種類のエラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24611-516">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="24611-517">目的のファイルが存在しない可能性があります、ロックアウトされることがあります、コードが、権限がありませんして具合可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24611-517">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="24611-518">同様に場合は、コードでは、データベース内のレコードを更新しようとして、アクセス許可の問題があります、データベースへの接続が削除される可能性が、無効なおよびこれを保存するデータがあります。</span><span class="sxs-lookup"><span data-stu-id="24611-518">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="24611-519">プログラミング用語では、このような状況が呼び出されます*例外*します。</span><span class="sxs-lookup"><span data-stu-id="24611-519">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="24611-520">(スロー) が生成されますが、コードには、例外が発生すると、エラー メッセージのユーザーに最高でいたずら。</span><span class="sxs-lookup"><span data-stu-id="24611-520">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="24611-522">コードが例外を発生可能性がある場合に、この種類のエラー メッセージを回避するためには、使用する`try/catch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="24611-522">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="24611-523">`try`ステートメントでは、チェックするコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="24611-523">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="24611-524">1 つまたは複数`catch`ステートメントでは、特定の検索できます (特定の種類の例外の) エラーの発生した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24611-524">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="24611-525">多くとして含めることができます`catch`するとしてステートメントを予測しているエラーを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="24611-525">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="24611-526">使用を避けることをお勧めします。、`Response.Redirect`メソッド`try/catch`ステートメント、ページで、例外が生じるためです。</span><span class="sxs-lookup"><span data-stu-id="24611-526">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="24611-527">次の例では、最初の要求でテキスト ファイルを作成し、ファイルを開くユーザーを切り替えるボタンを表示するページを示します。</span><span class="sxs-lookup"><span data-stu-id="24611-527">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="24611-528">例は、例外が発生されるように意図的に不適切なファイル名を使用します。</span><span class="sxs-lookup"><span data-stu-id="24611-528">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="24611-529">コードが含まれています`catch`可能性のある例外の 2 つのステートメント: `FileNotFoundException`、ファイル名が間違っている場合に発生して`DirectoryNotFoundException`ASP.NET は、フォルダーにも見つからない場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="24611-529">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="24611-530">(コメントを解除できます、ステートメントの例では、すべてが適切に動作しているときの動作を確認するためにします。)</span><span class="sxs-lookup"><span data-stu-id="24611-530">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="24611-531">コードが例外を処理していない場合は、前のスクリーン ショットのようなエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="24611-531">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="24611-532">ただし、`try/catch`セクションでは、ユーザーがこの種のエラーが表示されることを防ぐのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="24611-532">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="24611-533">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="24611-533">Additional Resources</span></span>

<span data-ttu-id="24611-534">**Visual Basic を使用したプログラミング**</span><span class="sxs-lookup"><span data-stu-id="24611-534">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="24611-535">付録: Visual Basic 言語と構文</span><span class="sxs-lookup"><span data-stu-id="24611-535">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="24611-536">**リファレンス ドキュメント**</span><span class="sxs-lookup"><span data-stu-id="24611-536">**Reference Documentation**</span></span>


[<span data-ttu-id="24611-537">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="24611-537">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="24611-538">C# 言語</span><span class="sxs-lookup"><span data-stu-id="24611-538">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
