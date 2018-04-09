---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Razor 構文 (c#) を使用して ASP.NET Web プログラミングの概要 |Microsoft ドキュメント
author: tfitzmac
description: この章では、するプログラミングの概要については ASP.NET Web Pages を Razor 構文を使用します。 動的な web の pa を実行するための Microsoft のテクノロジを ASP.NET には.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 430033c06df74cc3661c40ca7f7bd9244cd257c9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="5368b-104">Razor 構文 (c#) を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="5368b-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="5368b-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5368b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5368b-106">ここで、プログラミングの概要 ASP.NET Web Pages を Razor 構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="5368b-107">ASP.NET は、web サーバーでの動的な web ページを実行するための Microsoft のテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="5368b-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="5368b-108">この記事に重点を置く c# プログラミング言語を使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="5368b-109">**学習する内容**:</span><span class="sxs-lookup"><span data-stu-id="5368b-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="5368b-110">プログラミングの Razor 構文を使用して ASP.NET Web Pages のプログラミングの概要に関するヒントの最上位 8 です。</span><span class="sxs-lookup"><span data-stu-id="5368b-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="5368b-111">基本的なプログラミング概念する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="5368b-112">どのような ASP.NET サーバー コードと Razor 構文に関するです。</span><span class="sxs-lookup"><span data-stu-id="5368b-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="5368b-113">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="5368b-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="5368b-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5368b-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5368b-115">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="5368b-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="5368b-116">最上位の 8 プログラミングのヒント</span><span class="sxs-lookup"><span data-stu-id="5368b-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="5368b-117">このセクションでは、絶対に必要な Razor 構文を使用して ASP.NET サーバー コードの記述を開始すると、いくつかのヒントを示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="5368b-118">Razor 構文は c# プログラミング言語に基づいており、ASP.NET Web Pages を最もよく使用される言語であります。</span><span class="sxs-lookup"><span data-stu-id="5368b-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="5368b-119">ただし、Razor 構文は、Visual Basic 言語と Visual Basic で実行することもできます。 表示されるものをもサポートします。</span><span class="sxs-lookup"><span data-stu-id="5368b-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="5368b-120">詳細については、「付録[Visual Basic 言語と構文](https://go.microsoft.com/fwlink/?LinkId=202908)です。</span><span class="sxs-lookup"><span data-stu-id="5368b-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="5368b-121">記事の後半でこれらのプログラミング手法のほとんどについての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="5368b-122">1.コードを使用してページを追加する、@ 文字</span><span class="sxs-lookup"><span data-stu-id="5368b-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="5368b-123">`@`文字は、インライン式、1 つのステートメント ブロック、および複数ステートメントのブロックを開始します。</span><span class="sxs-lookup"><span data-stu-id="5368b-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="5368b-124">これらのステートメントがどのようなページがブラウザーで実行されるときに次に示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="5368b-126">**HTML エンコード**</span><span class="sxs-lookup"><span data-stu-id="5368b-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="5368b-127">ページを使用して、コンテンツを表示した場合、`@`文字、上記の例では、ASP.NET は、出力を HTML でエンコードします。</span><span class="sxs-lookup"><span data-stu-id="5368b-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="5368b-128">これには、HTML の予約された文字が置き換えられます (など`<`と`>`と`&`) コードを HTML タグまたはエンティティとして解釈されるのではなく、web ページ内の文字として表示される文字を有効にするとします。</span><span class="sxs-lookup"><span data-stu-id="5368b-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="5368b-129">HTML エンコードせず、サーバー コードからの出力は正しく表示されない場合があり、セキュリティ上のリスクにページを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="5368b-130">目標は、マークアップとしてタグがレンダリングされる HTML マークアップを出力するかどうか (たとえば`<p></p>`段落のまたは`<em></em>`テキストを強調する) を参照してください[結合のテキスト、マークアップ、およびコード ブロック内のコード](#BM_CombiningTextMarkupAndCode)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="5368b-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="5368b-131">詳細での HTML エンコードに関する[フォームを使用する](https://go.microsoft.com/fwlink/?LinkId=202892)です。</span><span class="sxs-lookup"><span data-stu-id="5368b-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="5368b-132">2.コード ブロックを中かっこで囲みます</span><span class="sxs-lookup"><span data-stu-id="5368b-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="5368b-133">A*コード ブロック*1 つまたは複数のコード ステートメントが含まれており、中かっこで囲まれています。</span><span class="sxs-lookup"><span data-stu-id="5368b-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="5368b-134">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5368b-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="5368b-136">3.セミコロンで各コード ステートメントを終了するブロック内</span><span class="sxs-lookup"><span data-stu-id="5368b-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="5368b-137">コード ブロックの内部完全なコードの各ステートメントをセミコロンで終了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="5368b-138">インライン式は、セミコロンで終了しません。</span><span class="sxs-lookup"><span data-stu-id="5368b-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="5368b-139">4.値を格納するのに変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-139">4. You use variables to store values</span></span>

<span data-ttu-id="5368b-140">内の値を格納することができます、*変数*文字列、数字、および日付などを含め、します。新しい変数を使用して、作成する、`var`キーワード。</span><span class="sxs-lookup"><span data-stu-id="5368b-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="5368b-141">変数の値を挿入するにはページを使用して、直接`@`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="5368b-142">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5368b-142">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="5368b-144">5.リテラル文字列の値を二重引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="5368b-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="5368b-145">A*文字列*テキストとして扱われる文字のシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="5368b-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="5368b-146">文字列を指定するには、二重引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="5368b-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="5368b-147">表示する文字列がバック スラッシュ文字を含むかどうか ( `\` ) または二重引用符 ( `"` ) を使用して、*逐語的文字列リテラル*が付いている、`@`演算子。</span><span class="sxs-lookup"><span data-stu-id="5368b-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="5368b-148">(C# の場合、\ 文字は、逐語的文字列リテラルを使用する場合に特別な意味を持つ)。</span><span class="sxs-lookup"><span data-stu-id="5368b-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="5368b-149">二重引用符を埋め込むには、逐語的文字列リテラルを使用して、引用符を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="5368b-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="5368b-150">ページを使用してこれらの例の両方の結果を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="5368b-152">注意して、 `@` verbatim 文字列リテラル (C#) をマークして、ASP.NET ページ内のコードをマークする、文字を使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="5368b-153">6.コードは大文字小文字を区別</span><span class="sxs-lookup"><span data-stu-id="5368b-153">6. Code is case sensitive</span></span>

<span data-ttu-id="5368b-154">C# の場合、キーワード (と同様に`var`、 `true`、および`if`) し、変数名では大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="5368b-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="5368b-155">次のコード行が 2 つの異なる変数を作成する`lastName`と `LastName.`</span><span class="sxs-lookup"><span data-stu-id="5368b-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="5368b-156">として変数を宣言する場合`var lastName = "Smith";`、ページとしてその変数を参照しようとする場合と`@LastName`、ために、エラーが発生`LastName`認識されません。</span><span class="sxs-lookup"><span data-stu-id="5368b-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="5368b-157">Visual Basic のキーワードと変数は、*いない*大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="5368b-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="5368b-158">7.コーディングのほとんどには、オブジェクトが含まれます</span><span class="sxs-lookup"><span data-stu-id="5368b-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="5368b-159">*オブジェクト*プログラミングで使用できることを表す&#8212;ページ、テキスト ボックス、ファイル、画像、web 要求、電子メール メッセージ、顧客レコード (データベースの行) などです。オブジェクトの特性を記述するプロパティがあると、読み取りまたは変更ができる&#8212;テキスト ボックスのオブジェクトが、 `Text` (など) のプロパティ、要求オブジェクトが、`Url`プロパティ、電子メール メッセージには、`From`プロパティ顧客オブジェクトには、`FirstName`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="5368b-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="5368b-160">オブジェクトでは、実行されるメソッドもがある、&quot;動詞&quot;それらを実行できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="5368b-161">例としては、ファイル オブジェクトの`Save`メソッドは、イメージ オブジェクトの`Rotate`メソッド、および電子メール オブジェクトの`Send`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="5368b-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="5368b-162">使って作業を多くの場合、`Request`オブジェクト (フォーム フィールド) のテキスト ボックスの値などの情報を提供する ページで、ブラウザーの種類は、要求、ページや、ユーザー id などの URL を作成します。次の例のプロパティにアクセスする方法を示しています、`Request`オブジェクトおよび呼び出す方法、`MapPath`のメソッド、`Request`オブジェクトで、サーバー上のページの絶対パスを表示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="5368b-163">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5368b-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="5368b-165">8.意思決定を行うコードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="5368b-166">動的な web ページの主な機能は、条件に基づき、新機能を決定できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="5368b-167">これを行う最も一般的な方法は、`if`ステートメント (と省略可能な`else`ステートメント)。</span><span class="sxs-lookup"><span data-stu-id="5368b-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="5368b-168">ステートメント`if(IsPost)`書き込みのためのショートハンド方法`if(IsPost == true)`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="5368b-169">と共に`if`ステートメントはさまざまな方法は、コードのブロックを繰り返し、条件をテストするために、か、この記事の後半で説明されています。</span><span class="sxs-lookup"><span data-stu-id="5368b-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="5368b-170">結果がブラウザーで表示されます (クリックした後**送信**)。</span><span class="sxs-lookup"><span data-stu-id="5368b-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="5368b-172">HTTP GET と POST メソッドおよび IsPost プロパティ</span><span class="sxs-lookup"><span data-stu-id="5368b-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="5368b-173">Web ページ (HTTP) に使用されるプロトコルには、ごく少数のサーバーに要求に使用されるメソッド (動詞) がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5368b-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="5368b-174">2 つの最も一般的なは、ページの読み取りに使用される、GET と POST のページを送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="5368b-175">一般に、ユーザーの要求 ページでは、最初に、ページが要求された GET を使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="5368b-176">場合は、ユーザーは、フォームに入力し、送信ボタンをクリックし、ブラウザー、POST 要求、サーバーを行います。</span><span class="sxs-lookup"><span data-stu-id="5368b-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="5368b-177">Web プログラミング、かどうか、ページが要求されて、GET または POST として、ページの処理方法がわかるように、知っておくと役立ちますがしばしばあります。</span><span class="sxs-lookup"><span data-stu-id="5368b-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="5368b-178">ASP.NET Web Pages で行うこともできます、`IsPost`プロパティを要求が GET または POST があるかどうかを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5368b-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="5368b-179">要求が、投稿の場合、`IsPost`プロパティが true の場合が返され、フォーム上のテキスト ボックスの値読み取りなどを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="5368b-180">多くの例を確認する方法を示しますの値に応じて異なる方法でページを処理する`IsPost`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="5368b-181">単純なコード例</span><span class="sxs-lookup"><span data-stu-id="5368b-181">A Simple Code Example</span></span>

<span data-ttu-id="5368b-182">この手順では、基本的なプログラミング技術について説明するページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="5368b-183">例では、それらを追加し、結果を表示し、2 つの数値を入力できるように、ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="5368b-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="5368b-184">エディターで、新しいファイルを作成し、名前*AddNumbers.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="5368b-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="5368b-185">ページで、ページ内の既存のものを置き換えるには、次のコードとマークアップをコピーします。</span><span class="sxs-lookup"><span data-stu-id="5368b-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="5368b-186">ここでは、いくつかの点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="5368b-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="5368b-187">`@`文字 ページで、コードの最初のブロックを開始して、前に指定する、`totalMessage`ページの下部に埋め込まれている変数。</span><span class="sxs-lookup"><span data-stu-id="5368b-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="5368b-188">ページの上部にあるブロックは中かっこで囲まれます。</span><span class="sxs-lookup"><span data-stu-id="5368b-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="5368b-189">上部にあるブロックでは、すべての行は、セミコロンで終了します。</span><span class="sxs-lookup"><span data-stu-id="5368b-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="5368b-190">変数`total`、 `num1`、 `num2`、および`totalMessage`いくつかの数字と文字列を格納します。</span><span class="sxs-lookup"><span data-stu-id="5368b-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="5368b-191">割り当てられているリテラル文字列値、`totalMessage`変数は、二重引用符内です。</span><span class="sxs-lookup"><span data-stu-id="5368b-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="5368b-192">コードがときに、大文字小文字を区別があるため、`totalMessage`ページの下部にある変数を使用すると、その名前では、上部にある変数を完全に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="5368b-193">式`num1.AsInt() + num2.AsInt()`オブジェクトおよびメソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="5368b-194">`AsInt`各変数のメソッドに算術演算を実行できるように、数値 (整数) に、ユーザーが入力した文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="5368b-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="5368b-195">`<form>`タグが含まれています、`method="post"`属性。</span><span class="sxs-lookup"><span data-stu-id="5368b-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="5368b-196">これを指定する、ユーザーがクリックしたときに**追加**ページは、HTTP POST メソッドを使用してサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="5368b-197">ページが送信されるときに、`if(IsPost)`テスト true に評価され、条件付きコードの実行、数値を加算した結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="5368b-198">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="5368b-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="5368b-199">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。2 つの整数を入力し、クリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5368b-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="5368b-201">基本的なプログラミング概念</span><span class="sxs-lookup"><span data-stu-id="5368b-201">Basic Programming Concepts</span></span>

<span data-ttu-id="5368b-202">この記事では、ASP.NET web プログラミングの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="5368b-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="5368b-203">完全な検査、最も頻繁に使用するプログラミングの概念を通じてだけクイック ツアーではありません。</span><span class="sxs-lookup"><span data-stu-id="5368b-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="5368b-204">それでも、ほとんどの ASP.NET Web Pages を開始する必要がありますを説明します。</span><span class="sxs-lookup"><span data-stu-id="5368b-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="5368b-205">最初に、ほとんどの技術的な背景。</span><span class="sxs-lookup"><span data-stu-id="5368b-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="5368b-206">Razor 構文、サーバー コード、および ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5368b-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="5368b-207">Razor 構文は、web ページで、サーバー ベースのコードを埋め込むための単純なプログラミング構文です。</span><span class="sxs-lookup"><span data-stu-id="5368b-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="5368b-208">Razor 構文を使用する web ページは、コンテンツの 2 つの種類: クライアントのコンテンツとサーバー コード。</span><span class="sxs-lookup"><span data-stu-id="5368b-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="5368b-209">クライアントにコンテンツを web ページを使用しているものは、: (要素) の HTML マークアップのスタイルを設定、CSS などの情報もなど、JavaScript、およびプレーン テキストの一部のクライアント スクリプトです。</span><span class="sxs-lookup"><span data-stu-id="5368b-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="5368b-210">Razor 構文を使用して、このクライアント コンテンツをサーバー コードを追加できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="5368b-211">ページにサーバー コードがある場合、サーバーは、ブラウザーにページを送信する前に最初に、そのコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="5368b-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="5368b-212">サーバーで実行すると、コードは、ためにクライアント コンテンツを単独で、サーバー ベースのデータベースへのアクセスなどを使用してもっと複雑なことができるタスクを実行できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="5368b-213">最も重要なは、サーバー コードをクライアントにコンテンツを動的に作成&#8212;HTML マークアップまたはその他のコンテンツを即座に生成し、ページを含む可能性がある静的な HTML と共にブラウザーに送信します。</span><span class="sxs-lookup"><span data-stu-id="5368b-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="5368b-214">ブラウザーのパースペクティブから、サーバー コードによって生成されたクライアント コンテンツはその他のクライアント コンテンツと違いはありません。</span><span class="sxs-lookup"><span data-stu-id="5368b-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="5368b-215">既に説明したように、サーバーが必要なコードは非常にシンプルです。</span><span class="sxs-lookup"><span data-stu-id="5368b-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="5368b-216">Razor 構文を含む ASP.NET web ページが特別なファイル拡張子を持つ (*.cshtml*または*.vbhtml*)。</span><span class="sxs-lookup"><span data-stu-id="5368b-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="5368b-217">サーバーは、これらの拡張機能を認識して、Razor 構文でマークされているし、ブラウザーにページを送信するコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="5368b-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="5368b-218">ASP.NET 適合する位置ですか。</span><span class="sxs-lookup"><span data-stu-id="5368b-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="5368b-219">Razor 構文は、Microsoft .NET Framework に基づいたは、ASP.NET と呼ばれる Microsoft のテクノロジに基づいています。</span><span class="sxs-lookup"><span data-stu-id="5368b-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="5368b-220">.Net Framework は、実質的にあらゆる種類のコンピューター アプリケーションを開発するための Microsoft のビッグ、包括的なプログラミング フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="5368b-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="5368b-221">ASP.NET は、web アプリケーションの作成用に作られている .NET Framework の一部です。</span><span class="sxs-lookup"><span data-stu-id="5368b-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="5368b-222">開発者は、最大値と最高トラフィックの web サイトの多くは世界中の作成に ASP.NET を使いました。</span><span class="sxs-lookup"><span data-stu-id="5368b-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="5368b-223">(いつでも、ファイル名拡張子を表示*.aspx*サイトの URL の一部として、ASP.NET を使用して、サイトが作成されたことを知るします)。</span><span class="sxs-lookup"><span data-stu-id="5368b-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="5368b-224">Razor 構文を使用すると、専門知識をしているかどうかは、初心者とを使用すると、生産性の向上を理解しやすくなりますが簡略化された構文を使用して、ASP.NET のすべての電源ができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="5368b-225">この構文は、簡単に使用できるが、ASP.NET と .NET Framework のファミリ関係は、web サイトが複雑になると、あるに使用できる大規模なフレームワークの電源を意味します。</span><span class="sxs-lookup"><span data-stu-id="5368b-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="5368b-227">**クラスとインスタンス**</span><span class="sxs-lookup"><span data-stu-id="5368b-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="5368b-228">ASP.NET サーバー コードでは、クラスの概念にさらに作成されるオブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="5368b-229">クラスは、定義またはオブジェクトのテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="5368b-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="5368b-230">たとえば、アプリケーションが含まれます、`Customer`プロパティと任意の顧客オブジェクトが必要なメソッドを定義するクラス。</span><span class="sxs-lookup"><span data-stu-id="5368b-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="5368b-231">アプリケーションは、実際の顧客情報を処理する必要がありますのインスタンスが作成されます (または*をインスタンス化*) customer オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5368b-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="5368b-232">個々 の顧客が別のインスタンスの`Customer`クラスです。</span><span class="sxs-lookup"><span data-stu-id="5368b-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="5368b-233">すべてのインスタンスを同じプロパティとメソッドをサポートしていますが、各顧客オブジェクトが一意であるために、各インスタンスのプロパティの値は、通常は異なります。</span><span class="sxs-lookup"><span data-stu-id="5368b-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="5368b-234">1 人の顧客オブジェクトに、`LastName`プロパティを"Smith"にすることがあります以外の場合は顧客の別のオブジェクトで、`LastName`プロパティを"Jones"にすることがあります</span><span class="sxs-lookup"><span data-stu-id="5368b-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="5368b-235">同様に、サイト内の個別の web ページは、`Page`オブジェクトのインスタンスでは、`Page`クラスです。</span><span class="sxs-lookup"><span data-stu-id="5368b-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="5368b-236">ページのボタンは、`Button`オブジェクトのインスタンスでは、`Button`クラス、およびなです。</span><span class="sxs-lookup"><span data-stu-id="5368b-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="5368b-237">各インスタンスには独自の特性がすべてに基づいている新機能は、オブジェクトのクラスの定義で指定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="5368b-238">基本構文</span><span class="sxs-lookup"><span data-stu-id="5368b-238">Basic Syntax</span></span>

<span data-ttu-id="5368b-239">サーバー コードを HTML マークアップを追加する方法と、ASP.NET Web Pages ページを作成する方法の基本的な例は前述です。</span><span class="sxs-lookup"><span data-stu-id="5368b-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="5368b-240">ここで、Razor 構文を使用して ASP.NET サーバー コードの記述の基本を学習&#8212;プログラミング言語の規則では、します。</span><span class="sxs-lookup"><span data-stu-id="5368b-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="5368b-241">(特に C、C++、c#、Visual Basic、または JavaScript を使用した) 場合のプログラミングを使用した経験が場合、次を参照する新機能の多くについて理解されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="5368b-242">習熟するのみでのマークアップにサーバー コードを追加する方法が必要*.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5368b-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="5368b-243">テキスト、マークアップ、およびコード ブロック内のコードを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="5368b-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="5368b-244">サーバー コード ブロックでは、多くの場合に出力テキストかマークアップ (あるいはその両方) のページにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="5368b-245">サーバー コード ブロックのコードではないとする代わりに表示するかは、テキストが含まれている場合、ASP.NET をコードからそのテキストを識別できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="5368b-246">これにはいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-246">There are several ways to do this.</span></span>

- <span data-ttu-id="5368b-247">ような HTML 要素にテキストを囲みます`<p></p>`または`<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="5368b-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="5368b-248">HTML 要素には、テキスト、HTML 要素の追加、およびサーバー コード式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="5368b-249">ASP.NET で開始 HTML タグがどのように表示される場合 (たとえば、 `<p>`)、レンダリングされる要素を含むすべての式およびとしてその内容は、ブラウザーには、サーバー コード式を解決するときにそれをします。</span><span class="sxs-lookup"><span data-stu-id="5368b-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="5368b-250">使用して、`@:`演算子または`<text>`要素。</span><span class="sxs-lookup"><span data-stu-id="5368b-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="5368b-251">`@:`プレーン テキストまたは一致しない HTML タグを含むコンテンツの 1 つの行が出力、`<text>`要素を出力する複数の行で囲みます。</span><span class="sxs-lookup"><span data-stu-id="5368b-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="5368b-252">これらのオプションは、出力の一部としての HTML 要素を表示したくない場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5368b-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="5368b-253">複数行テキストまたは一致しない HTML タグを出力する場合は、各行の前に記述できます`@:`、または内の行を囲むことができ、`<text>`要素。</span><span class="sxs-lookup"><span data-stu-id="5368b-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="5368b-254">同様に、`@:`演算子、`<text>`タグはテキスト コンテンツを識別する、ASP.NET で使用され、ページの出力には常にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="5368b-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="5368b-255">最初の例は、前の例を繰り返しますの 1 つのペアを使用して`<text>`タグをレンダリングするテキストを囲みます。</span><span class="sxs-lookup"><span data-stu-id="5368b-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="5368b-256">2 番目の例では、`<text>`と`</text>`タグがいくつか含まれていないテキストと一致しない HTML タグがあるすべての 3 つの行を囲みます (`<br />`)、およびサーバー コードと一致した HTML タグ。</span><span class="sxs-lookup"><span data-stu-id="5368b-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="5368b-257">もう一度、ごとに 1 行の前でしたも、`@:`演算子以外のいずれかの方法の動作です。</span><span class="sxs-lookup"><span data-stu-id="5368b-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5368b-258">このセクションで示すようにテキストを出力するときに&#8212;、HTML 要素を使用して、`@:`演算子、または`<text>`要素&#8212;ASP.NET は HTML エンコード出力します。</span><span class="sxs-lookup"><span data-stu-id="5368b-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="5368b-259">(ASP.NET はサーバーのコード式と末尾に挿入されるサーバー コード ブロックの出力はエンコード前に述べたよう`@`、このセクションで説明した特殊なケースでは可します)。</span><span class="sxs-lookup"><span data-stu-id="5368b-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="5368b-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="5368b-260">Whitespace</span></span>

<span data-ttu-id="5368b-261">ステートメントで (および、文字列リテラルの外部) 余分なスペースは、ステートメントに影響しません。</span><span class="sxs-lookup"><span data-stu-id="5368b-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="5368b-262">ステートメント内での改行には、ステートメントに影響がなく、読みやすくするためのステートメントをラップすることができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="5368b-263">次のステートメントでは、同じです。</span><span class="sxs-lookup"><span data-stu-id="5368b-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="5368b-264">ただし、文字列リテラルの途中で行をラップすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5368b-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="5368b-265">次の例が機能しません。</span><span class="sxs-lookup"><span data-stu-id="5368b-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="5368b-266">上記のコードのように複数の行に折り返す長い文字列を結合するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="5368b-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="5368b-267">連結演算子を使用することができます (`+`)、この記事で後述があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="5368b-268">使用することも、`@`文字に逐語的文字列リテラル、この記事の前半で説明したとおりです。</span><span class="sxs-lookup"><span data-stu-id="5368b-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="5368b-269">行にわたって verbatim 文字列リテラルを中断することができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="5368b-270">コード (およびマークアップ) コメント</span><span class="sxs-lookup"><span data-stu-id="5368b-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="5368b-271">コメントでは、自分自身または他のユーザーのノートのままにできます。</span><span class="sxs-lookup"><span data-stu-id="5368b-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="5368b-272">これらも無効にすること (*をコメント アウト*) コードまたはを実行したくないが、当面は、ページに維持するマークアップのセクションです。</span><span class="sxs-lookup"><span data-stu-id="5368b-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="5368b-273">Razor コードの構文と HTML マークアップのコメントを付けるさまざまながあります。</span><span class="sxs-lookup"><span data-stu-id="5368b-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="5368b-274">すべての Razor コードでは、Razor コメントは処理 (と同様、削除)、ページがブラウザーに送信される前に、サーバー上。</span><span class="sxs-lookup"><span data-stu-id="5368b-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="5368b-275">したがって、Razor コメントの構文では、ファイルを編集するものの、ページのソースであっても、ユーザーが表示されない場合に表示されるコメント コードに (またはマークアップにあっても) を挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="5368b-276">ASP.NET Razor コメントのコメントを開始する`@*`終了することで、`*@`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="5368b-277">1 つの行または複数の行にコメントができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="5368b-278">コード ブロック内でコメントを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="5368b-279">ここでは、同じコードのブロック、コードの行をコメント アウトされて実行されないようにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="5368b-280">Razor コメントの構文を使用する代わりに、コード ブロックの内側を使用して、c# など、プログラミング言語のコメント構文を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="5368b-281">C# の場合は、単一行コメントが付きます、`//`文字、および複数行コメントで始まる`/*`で終わります`*/`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="5368b-282">(Razor コメントと c# コメントは、ブラウザーにレンダリングされません)。</span><span class="sxs-lookup"><span data-stu-id="5368b-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="5368b-283">マークアップ、ご存知のとおりは、HTML コメントを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="5368b-284">HTML コメントで始まる`<!--`文字、末尾が`-->`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="5368b-285">HTML コメントを使用すると、囲むだけでなく、テキスト、またすべての HTML マークアップの表示にしたくないが、使用ページ内に維持することがあります。</span><span class="sxs-lookup"><span data-stu-id="5368b-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="5368b-286">この HTML コメントには、タグとが含まれているテキストの内容全体が非表示には。</span><span class="sxs-lookup"><span data-stu-id="5368b-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="5368b-287">Razor コメント、HTML コメントとは異なり*は*のページに表示し、ユーザーがページのソースを表示して確認できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="5368b-288">Razor では、c# の入れ子になったブロックに制限があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="5368b-289">詳細については、次を参照してください[という名前の変数を使用する C# の場合、入れ子になったブロック壊れたコードの生成。](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="5368b-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="5368b-290">変数</span><span class="sxs-lookup"><span data-stu-id="5368b-290">Variables</span></span>

<span data-ttu-id="5368b-291">変数は、データの格納に使用する名前付きオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="5368b-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="5368b-292">名前を付ける変数、何もが、名前の先頭の文字は英字にする必要があり、空白文字または予約文字は使用できません。</span><span class="sxs-lookup"><span data-stu-id="5368b-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="5368b-293">変数とデータ型</span><span class="sxs-lookup"><span data-stu-id="5368b-293">Variables and Data Types</span></span>

<span data-ttu-id="5368b-294">変数は、データの種類が変数に格納されていることを示しますが、特定のデータ型を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="5368b-295">文字列値を格納する文字列変数があることができます (のような&quot;Hello world&quot;)、(3 または 79) などの整数値を格納する整数変数とさまざまな (4/12/2012 や 2009 年 3 月などの形式で日付値を格納する date 型の変数).</span><span class="sxs-lookup"><span data-stu-id="5368b-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="5368b-296">いて、その他の多くのデータ型を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="5368b-297">ただし、一般にする必要はありません、変数の型を指定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="5368b-298">ほとんどの場合、ASP.NET は、変数内のデータの使用方法に基づいて型を見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="5368b-299">(場合によっては、型を指定する必要があります。 これが true の例が表示されます。)</span><span class="sxs-lookup"><span data-stu-id="5368b-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="5368b-300">使用して変数を宣言する、`var`キーワード (型を指定しない) 場合や、型の名前を使用しています。</span><span class="sxs-lookup"><span data-stu-id="5368b-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="5368b-301">次の例では、web ページで変数の一般的な使用を示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="5368b-302">ページで、前の例を組み合わせると、ブラウザーで表示されます。 これが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="5368b-304">変換して、データ型のテスト</span><span class="sxs-lookup"><span data-stu-id="5368b-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="5368b-305">ASP.NET はこのデータ型を自動的に決定できますは通常があります、できません。</span><span class="sxs-lookup"><span data-stu-id="5368b-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="5368b-306">そのため、明示的な変換を実行して ASP.NET を手助けする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="5368b-307">型変換を持っていない場合でもいる場合もありますデータの種類が使用することを確認するためのテストに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5368b-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="5368b-308">最も一般的なケースでは、整数や日付など、他の型を文字列に変換していることです。</span><span class="sxs-lookup"><span data-stu-id="5368b-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="5368b-309">次の例は、文字列を数値に変換する必要があります、一般的な事例を示しています。</span><span class="sxs-lookup"><span data-stu-id="5368b-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="5368b-310">原則として、ユーザー入力に文字列として。</span><span class="sxs-lookup"><span data-stu-id="5368b-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="5368b-311">数値を入力するユーザーを要求した場合でも、ユーザー入力が送信されたコードで読み取るときに、数字を入力したした場合でも、データは文字列の形式です。</span><span class="sxs-lookup"><span data-stu-id="5368b-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="5368b-312">そのため、文字列を数値に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="5368b-313">例では、したり、変換せず、値に対して算術演算を実行しようとする場合、次のエラーにより、ASP.NET は、2 つの文字列を追加できないため。</span><span class="sxs-lookup"><span data-stu-id="5368b-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="5368b-314">*型 'string' から 'int' を暗黙的に変換できません。*</span><span class="sxs-lookup"><span data-stu-id="5368b-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="5368b-315">呼び出すの値を整数に変換する、`AsInt`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="5368b-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="5368b-316">変換が成功した場合は、数値を追加できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="5368b-317">次の表は、変数の一般的ないくつかの変換とテスト方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-317">The following table lists some common conversion and test methods for variables.</span></span>


|   <span data-ttu-id="5368b-318"><strong>メソッド</strong></span><span class="sxs-lookup"><span data-stu-id="5368b-318"><strong>Method</strong></span></span>    |                                                                              <span data-ttu-id="5368b-319"><strong>説明</strong></span><span class="sxs-lookup"><span data-stu-id="5368b-319"><strong>Description</strong></span></span>                                                                              |                         <span data-ttu-id="5368b-320"><strong>例</strong></span><span class="sxs-lookup"><span data-stu-id="5368b-320"><strong>Example</strong></span></span>                         |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|      `AsInt(), IsInt()`      |                                                      <span data-ttu-id="5368b-321">整数 (「593」) のような自然数を表す文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="5368b-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>                                                      |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]   |
|     `AsBool(), IsBool()`     |                                                    <span data-ttu-id="5368b-322">ように文字列に変換します&quot;true&quot;または&quot;false&quot; Boolean 型にします。</span><span class="sxs-lookup"><span data-stu-id="5368b-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>                                                     |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]   |
|    `AsFloat(), IsFloat()`    |                                    <span data-ttu-id="5368b-323">ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を浮動小数点数。</span><span class="sxs-lookup"><span data-stu-id="5368b-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]   |
|  `AsDecimal(), IsDecimal()`  | <span data-ttu-id="5368b-324">ような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を 10 進数。</span><span class="sxs-lookup"><span data-stu-id="5368b-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="5368b-325">(ASP.NET では、10 進数は、浮動小数点数よりも正確です。)</span><span class="sxs-lookup"><span data-stu-id="5368b-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span> |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]   |
| `AsDateTime(), IsDateTime()` |                                                <span data-ttu-id="5368b-326">Asp.net の日付と時刻の値を表す文字列に変換します`DateTime`型です。</span><span class="sxs-lookup"><span data-stu-id="5368b-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>                                                 |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]   |
|         `ToString()`         |                                                                       <span data-ttu-id="5368b-327">その他の任意のデータ型を文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="5368b-327">Converts any other data type to a string.</span></span>                                                                        | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a><span data-ttu-id="5368b-328">演算子</span><span class="sxs-lookup"><span data-stu-id="5368b-328">Operators</span></span>

<span data-ttu-id="5368b-329">演算子は、キーワードまたは ASP.NET を式の中で実行するコマンドの種類を示す文字です。</span><span class="sxs-lookup"><span data-stu-id="5368b-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="5368b-330">C# 言語 (および基になる Razor 構文) は、多くの演算子をサポートしますが、のみ開始するいくつかを認識する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="5368b-331">次の表は、最も一般的な演算子をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="5368b-331">The following table summarizes the most common operators.</span></span>


|   <span data-ttu-id="5368b-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="5368b-332"><strong>Operator</strong></span></span>    |                                                                     <span data-ttu-id="5368b-333"><strong>説明</strong></span><span class="sxs-lookup"><span data-stu-id="5368b-333"><strong>Description</strong></span></span>                                                                     |                        <span data-ttu-id="5368b-334"><strong>例</strong></span><span class="sxs-lookup"><span data-stu-id="5368b-334"><strong>Examples</strong></span></span>                         |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|        <span data-ttu-id="5368b-335">`+` `-` `*` `/`</span><span class="sxs-lookup"><span data-stu-id="5368b-335">`+` `-` `*` `/`</span></span>         |                                                            <span data-ttu-id="5368b-336">算術演算子: 数値式で使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-336">Math operators used in numerical expressions.</span></span>                                                             |    [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]    |
|              `=`               |                                    <span data-ttu-id="5368b-337">代入。</span><span class="sxs-lookup"><span data-stu-id="5368b-337">Assignment.</span></span> <span data-ttu-id="5368b-338">左側にあるオブジェクトにステートメントの右側にある値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="5368b-338">Assigns the value on the right side of a statement to the object on the left side.</span></span>                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]   |
|              `==`              |                      <span data-ttu-id="5368b-339">等値。</span><span class="sxs-lookup"><span data-stu-id="5368b-339">Equality.</span></span> <span data-ttu-id="5368b-340">返します`true`値が等しい場合。</span><span class="sxs-lookup"><span data-stu-id="5368b-340">Returns `true` if the values are equal.</span></span> <span data-ttu-id="5368b-341">(上での違いに注意してください、`=`演算子および`==`演算子です)。</span><span class="sxs-lookup"><span data-stu-id="5368b-341">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>                      |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]   |
|              `!=`              |                                                       <span data-ttu-id="5368b-342">非等値。</span><span class="sxs-lookup"><span data-stu-id="5368b-342">Inequality.</span></span> <span data-ttu-id="5368b-343">返します`true`値が等しくない場合。</span><span class="sxs-lookup"><span data-stu-id="5368b-343">Returns `true` if the values are not equal.</span></span>                                                        |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]   |
|          `< > <= >=`           |                                               <span data-ttu-id="5368b-344">以下のより大きい-より、以下によりも-または-等しく、大きい-よりも-または-等しいかどうか。</span><span class="sxs-lookup"><span data-stu-id="5368b-344">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>                                                |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]   |
|              `+`               | <span data-ttu-id="5368b-345">連結、文字列の結合に使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-345">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="5368b-346">ASP.NET では、この演算子と式のデータ型に基づいて、加算演算子の違いを認識します。</span><span class="sxs-lookup"><span data-stu-id="5368b-346">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span> |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]   |
|           <span data-ttu-id="5368b-347">`+=` `-=`</span><span class="sxs-lookup"><span data-stu-id="5368b-347">`+=` `-=`</span></span>            |                                   <span data-ttu-id="5368b-348">インクリメントとデクリメント演算子を追加し、変数から 1 をそれぞれ減算します。</span><span class="sxs-lookup"><span data-stu-id="5368b-348">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]   |
|              `.`               |                                                  <span data-ttu-id="5368b-349">ドットします。</span><span class="sxs-lookup"><span data-stu-id="5368b-349">Dot.</span></span> <span data-ttu-id="5368b-350">オブジェクトとそのプロパティおよびメソッドを区別するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-350">Used to distinguish objects and their properties and methods.</span></span>                                                  |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]   |
|              `()`              |                                              <span data-ttu-id="5368b-351">かっこです。</span><span class="sxs-lookup"><span data-stu-id="5368b-351">Parentheses.</span></span> <span data-ttu-id="5368b-352">グループ式とをメソッドにパラメーターを渡すために使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-352">Used to group expressions and to pass parameters to methods.</span></span>                                               | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
|              `[]`              |                                                    <span data-ttu-id="5368b-353">角かっこです。</span><span class="sxs-lookup"><span data-stu-id="5368b-353">Brackets.</span></span> <span data-ttu-id="5368b-354">配列またはコレクション内の値にアクセスするために使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-354">Used for accessing values in arrays or collections.</span></span>                                                     |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]   |
|              `!`               |               <span data-ttu-id="5368b-355">じゃない。</span><span class="sxs-lookup"><span data-stu-id="5368b-355">Not.</span></span> <span data-ttu-id="5368b-356">逆に、`true`値を`false`およびその逆です。</span><span class="sxs-lookup"><span data-stu-id="5368b-356">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="5368b-357">通常、テストするための短縮形方法として使用`false`(用には、いない`true`)。</span><span class="sxs-lookup"><span data-stu-id="5368b-357">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>               |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]   |
| <span data-ttu-id="5368b-358">`&&` <code>&#124;&#124;</code></span><span class="sxs-lookup"><span data-stu-id="5368b-358">`&&` <code>&#124;&#124;</code></span></span> |                                                   <span data-ttu-id="5368b-359">論理 AND 条件をまとめてリンクに使用されるかとします。</span><span class="sxs-lookup"><span data-stu-id="5368b-359">Logical AND and OR, which are used to link conditions together.</span></span>                                                    |   [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]   |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="5368b-360">ファイルとフォルダーのパス コードでの操作</span><span class="sxs-lookup"><span data-stu-id="5368b-360">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="5368b-361">コードでは、ファイルとフォルダーのパスを持つ多くの場合、操作します。</span><span class="sxs-lookup"><span data-stu-id="5368b-361">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="5368b-362">開発用コンピューターで表示されるように、web サイトの物理的なフォルダー構造の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-362">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="5368b-363">Url およびパスの重要な詳細を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-363">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="5368b-364">URL がいずれかで、ドメイン名を開始 (`http://www.example.com`) またはサーバー名 (`http://localhost`、 `http://mycomputer`)。</span><span class="sxs-lookup"><span data-stu-id="5368b-364">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="5368b-365">URL は、ホスト コンピューター上の物理パスに対応します。</span><span class="sxs-lookup"><span data-stu-id="5368b-365">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="5368b-366">たとえば、`http://myserver`フォルダーに対応する*C:\websites\mywebsite*サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-366">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="5368b-367">仮想パスは、完全なパスを指定せずに、コード内のパスを表すための短縮形です。</span><span class="sxs-lookup"><span data-stu-id="5368b-367">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="5368b-368">ドメインまたはサーバー名に続く URL の部分が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5368b-368">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="5368b-369">仮想パスを使用する場合は、パスを更新することがなく別のドメインまたはサーバーに、コードを移動できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-369">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="5368b-370">相違点の把握に役立つ例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-370">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="5368b-371">完全な URL</span><span class="sxs-lookup"><span data-stu-id="5368b-371">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="5368b-372">サーバー名</span><span class="sxs-lookup"><span data-stu-id="5368b-372">Server name</span></span> | <span data-ttu-id="5368b-373">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="5368b-373">*mycompanyserver*</span></span> |
| <span data-ttu-id="5368b-374">[仮想パス]</span><span class="sxs-lookup"><span data-stu-id="5368b-374">Virtual path</span></span> | <span data-ttu-id="5368b-375">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="5368b-375">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="5368b-376">物理パス</span><span class="sxs-lookup"><span data-stu-id="5368b-376">Physical path</span></span> | <span data-ttu-id="5368b-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="5368b-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="5368b-378">仮想ルートは、/、ドライブは、c: のルートと同じように \ です。</span><span class="sxs-lookup"><span data-stu-id="5368b-378">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="5368b-379">(仮想フォルダーのパス常にフォワード スラッシュを使用します。)フォルダーの仮想パスは、物理フォルダー; として同じ名前である必要はありません。エイリアスがあります。</span><span class="sxs-lookup"><span data-stu-id="5368b-379">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="5368b-380">(運用サーバーで仮想パスほとんどと一致する正確な物理パスです。)</span><span class="sxs-lookup"><span data-stu-id="5368b-380">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="5368b-381">コードのファイルとフォルダーを使用する場合は場合がありますの物理パスおよび場合によってはどのようなオブジェクトを扱う場合によっては、仮想パスを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-381">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="5368b-382">ASP.NET では、これらのツールでコード ファイルとフォルダーのパスを操作するため:`Server.MapPath`メソッド、および`~`演算子と`Href`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="5368b-382">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="5368b-383">物理仮想パスに変換する: Server.MapPath メソッド</span><span class="sxs-lookup"><span data-stu-id="5368b-383">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="5368b-384">`Server.MapPath`メソッドは、仮想パスを変換 (と同様に*/default.cshtml*) への絶対物理パス (と同様に*C:\WebSites\MyWebSiteFolder\default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="5368b-384">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="5368b-385">このメソッドを使用すると、いつでも、完全な物理パスを作成する必要がありますにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-385">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="5368b-386">典型的な例は、読み取りまたはテキスト ファイルまたは web サーバー上の画像ファイルを作成するときです。</span><span class="sxs-lookup"><span data-stu-id="5368b-386">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="5368b-387">通常ホスティング サイトのサーバー上のサイトの絶対物理パスがわからない、このメソッドは、パスを変換できるようにするかが分かって — 仮想パス: サーバー上の対応するパスにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-387">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="5368b-388">ファイルまたはフォルダーをメソッドに仮想パスを渡すし、物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="5368b-388">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="5368b-389">仮想ルートを参照している: ~ 演算子と Href メソッド</span><span class="sxs-lookup"><span data-stu-id="5368b-389">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="5368b-390">*.Cshtml*または*.vbhtml*ファイル、仮想ルート パスを使用して、参照することができます、`~`演算子。</span><span class="sxs-lookup"><span data-stu-id="5368b-390">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="5368b-391">これは、機能は、サイトでは、のページを移動することができ、他のページに含まれているすべてのリンクが壊れているされませんので非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="5368b-391">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="5368b-392">これまで、web サイトを別の場所に移動する場合にも便利です。</span><span class="sxs-lookup"><span data-stu-id="5368b-392">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="5368b-393">次にいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-393">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="5368b-394">場合は、web サイトは`http://myserver/myapp`、方法 ASP.NET が扱うこれらのパス、ページの実行時に次に示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-394">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="5368b-395">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="5368b-395">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="5368b-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="5368b-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="5368b-397">(変数の値としてこれらのパスは実際には表示されませんが、ASP.NET は、パスとして扱うは何でした)。</span><span class="sxs-lookup"><span data-stu-id="5368b-397">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="5368b-398">使用することができます、 `~` (上記と同じ) のサーバー コードと次のように、マークアップの両方の演算子。</span><span class="sxs-lookup"><span data-stu-id="5368b-398">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="5368b-399">使用するマークアップで、`~`オペレーターをイメージ ファイル、その他の web ページ、および CSS ファイルなどのリソースへのパスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5368b-399">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="5368b-400">ASP.NET がページ (コードとマークアップの両方) を検索し、すべて解決、ページの実行時に、`~`適切なパスへの参照。</span><span class="sxs-lookup"><span data-stu-id="5368b-400">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="5368b-401">条件付きロジック、ループ</span><span class="sxs-lookup"><span data-stu-id="5368b-401">Conditional Logic and Loops</span></span>

<span data-ttu-id="5368b-402">ASP.NET サーバー コードでは、条件に基づいて、タスクを実行し、特定回数のステートメントを繰り返し実行するコードを記述することができます (つまり、ループを実行しているコード)。</span><span class="sxs-lookup"><span data-stu-id="5368b-402">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="5368b-403">条件をテストします。</span><span class="sxs-lookup"><span data-stu-id="5368b-403">Testing Conditions</span></span>

<span data-ttu-id="5368b-404">使用する単純な条件をテストする、`if`ステートメントで、指定したテストに基づく true または false を返します。</span><span class="sxs-lookup"><span data-stu-id="5368b-404">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="5368b-405">`if`キーワード、ブロックを開始します。</span><span class="sxs-lookup"><span data-stu-id="5368b-405">The `if` keyword starts a block.</span></span> <span data-ttu-id="5368b-406">実際のテスト (条件) では、かっこ内には、し、true または false を返します。</span><span class="sxs-lookup"><span data-stu-id="5368b-406">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="5368b-407">テストが true の場合に実行するステートメントは、中かっこで囲まれています。</span><span class="sxs-lookup"><span data-stu-id="5368b-407">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="5368b-408">`if`ステートメントを含めることができます、`else`ブロックで、条件が false の場合に実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-408">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="5368b-409">使用して複数の条件を追加することができます、`else if`ブロック。</span><span class="sxs-lookup"><span data-stu-id="5368b-409">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="5368b-410">場合は、最初の条件の場合、この例ではブロックが真ではありません、`else if`条件をチェックします。</span><span class="sxs-lookup"><span data-stu-id="5368b-410">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="5368b-411">その条件が満たされた場合内のステートメント、`else if`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-411">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="5368b-412">条件も満たさない場合、内のステートメント、`else`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-412">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="5368b-413">場合、その他の任意の数を追加することができます、ブロックを閉じて、`else`としてブロック、&quot;他のすべて&quot;条件。</span><span class="sxs-lookup"><span data-stu-id="5368b-413">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="5368b-414">多くの条件をテストするには、使用、`switch`ブロック。</span><span class="sxs-lookup"><span data-stu-id="5368b-414">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="5368b-415">テストする値がかっこ内 (この例で、`weekday`変数)。</span><span class="sxs-lookup"><span data-stu-id="5368b-415">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="5368b-416">各テストを使用して、`case`コロン (:) で終了するステートメント。</span><span class="sxs-lookup"><span data-stu-id="5368b-416">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="5368b-417">場合の値、`case`ステートメントには、テスト値が一致する、そのケース ブロック内のコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="5368b-417">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="5368b-418">各 case ステートメントを閉じる、`break`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="5368b-418">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="5368b-419">(それぞれに改行を含めるを忘れた場合`case`次のコードをブロックします`case`ステートメントも実行されます。)。A`switch`ブロックに多くの場合は、`default`ステートメントの最後のケースとして、&quot;他のすべて&quot;の他のケースに該当しない場合に実行するオプションです。</span><span class="sxs-lookup"><span data-stu-id="5368b-419">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="5368b-420">ブラウザーに表示される最後の 2 つの条件付きブロックの結果:</span><span class="sxs-lookup"><span data-stu-id="5368b-420">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="5368b-422">コードのループ</span><span class="sxs-lookup"><span data-stu-id="5368b-422">Looping Code</span></span>

<span data-ttu-id="5368b-423">多くの場合、同じステートメントを繰り返し実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-423">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="5368b-424">これを行うをループします。</span><span class="sxs-lookup"><span data-stu-id="5368b-424">You do this by looping.</span></span> <span data-ttu-id="5368b-425">たとえば、多くの場合、ステートメントを実行する同じ項目ごとにデータのコレクション。</span><span class="sxs-lookup"><span data-stu-id="5368b-425">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="5368b-426">使用することが正確に何回かループすることがわかっている場合、`for`ループします。</span><span class="sxs-lookup"><span data-stu-id="5368b-426">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="5368b-427">このようなループ カウントまたはカウント ダウンの際に特に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5368b-427">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="5368b-428">ループはで始まり、`for`キーワード、丸かっこ、3 つのステートメントに続くそれぞれをセミコロンで終了します。</span><span class="sxs-lookup"><span data-stu-id="5368b-428">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="5368b-429">最初のステートメントでかっこの内側 (`var i=10;`) カウンターを作成し、10 を初期化します。</span><span class="sxs-lookup"><span data-stu-id="5368b-429">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="5368b-430">カウンターの名前を付ける必要はありません`i`&#8212;任意の変数を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-430">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="5368b-431">ときに、`for`ループが実行される、カウンターが自動的にインクリメントされます。</span><span class="sxs-lookup"><span data-stu-id="5368b-431">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="5368b-432">2 番目のステートメント (`i < 21;`) をカウントするどの程度の条件を設定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-432">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="5368b-433">ここでは、希望に最大 20 まで移動する (つまり、読み進めて、カウンターが 21 未満)。</span><span class="sxs-lookup"><span data-stu-id="5368b-433">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="5368b-434">3 番目のステートメント (`i++` ) は、インクリメント演算子、単に 1 が加算、ループが実行されるたびにカウンターを使用するを指定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-434">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="5368b-435">中かっこ内には、ループの各反復処理を実行するコードを示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-435">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="5368b-436">マークアップは、新しい段落を作成する (`<p>`要素) の値を表示する出力に行を追加し、各`i`(カウンター)。</span><span class="sxs-lookup"><span data-stu-id="5368b-436">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="5368b-437">このページを実行すると、例は、11 行の行のテキスト項目数を示す行ごとに、出力の表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="5368b-437">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="5368b-439">コレクションまたは配列を扱う場合場合、多くの場合、使用する、`foreach`ループします。</span><span class="sxs-lookup"><span data-stu-id="5368b-439">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="5368b-440">コレクションは、類似のオブジェクトのグループと`foreach`ループに、コレクション内の各項目にタスクを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-440">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="5368b-441">この種類のループは、コレクションの便利なためとは異なり、`for`ループすることも、カウンターをインクリメントまたは制限を設定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-441">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="5368b-442">代わりに、`foreach`が終了するまで、このループのコードは、単にコレクションを続行されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-442">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="5368b-443">たとえば、次のコードが内の項目を返します、`Request.ServerVariables`コレクションは、これは、web サーバーに関する情報を格納するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5368b-443">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="5368b-444">使用して、`foreac`を新規作成して各アイテムの名前を表示する h ループ`<li>`HTML 箇条書きリスト内の要素。</span><span class="sxs-lookup"><span data-stu-id="5368b-444">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="5368b-445">`foreach`キーワードが続くかっこをコレクション内の 1 つの項目を表す変数を宣言する (例では、 `var item`) と、その後、`in`キーワード、ループ処理コレクション。</span><span class="sxs-lookup"><span data-stu-id="5368b-445">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="5368b-446">本体で、`foreach`ループ、先ほど宣言した変数を使用する現在のアイテムにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-446">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="5368b-448">汎用的なループを作成するには、使用、`while`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="5368b-448">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="5368b-449">A`while`ループから始まります、`while`後にかっこをループを続行する期間を指定するキーワード (ここでは、限り`countNum`50 未満) を繰り返すブロックします。</span><span class="sxs-lookup"><span data-stu-id="5368b-449">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="5368b-450">ループが通常インクリメント (に追加する) または減分 (減算) 変数またはオブジェクト カウントで使用されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-450">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="5368b-451">例では、`+=`演算子を追加する場合は 1`countNum`ループが実行されるたびにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-451">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="5368b-452">(カウント ダウンをループ内の変数をデクリメントするためには、デクリメント演算子を使用すると`-=`)。</span><span class="sxs-lookup"><span data-stu-id="5368b-452">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="5368b-453">オブジェクトとコレクション</span><span class="sxs-lookup"><span data-stu-id="5368b-453">Objects and Collections</span></span>

<span data-ttu-id="5368b-454">ほぼすべての ASP.NET web サイトでは、web ページ自体を含むオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="5368b-454">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="5368b-455">このセクションでは、演習では多くの場合、コードでいくつかの重要なオブジェクトについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5368b-455">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="5368b-456">ページのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="5368b-456">Page Objects</span></span>

<span data-ttu-id="5368b-457">ASP.NET の最も基本的なオブジェクトは、ページです。</span><span class="sxs-lookup"><span data-stu-id="5368b-457">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="5368b-458">オブジェクトを修飾せずに直接 page オブジェクトのプロパティにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-458">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="5368b-459">次のコード ページのファイルのパスを取得するを使用して、`Request`ページのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5368b-459">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="5368b-460">ようにクリアのプロパティと、現在のページ オブジェクト メソッドを参照していること、必要に応じてキーワードを使用できます、`this`をコード内のページ オブジェクトを表します。</span><span class="sxs-lookup"><span data-stu-id="5368b-460">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="5368b-461">前のコード例を次に示します`this`を追加して、ページを表します。</span><span class="sxs-lookup"><span data-stu-id="5368b-461">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="5368b-462">プロパティを使用することができます、`Page`など多くの情報を取得するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5368b-462">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="5368b-463">`Request`。</span><span class="sxs-lookup"><span data-stu-id="5368b-463">`Request`.</span></span> <span data-ttu-id="5368b-464">既に説明したようにこれは、ブラウザーの種類の要求を作成、ページや、ユーザー id などの URL を含む、現在の要求に関する情報のコレクション。</span><span class="sxs-lookup"><span data-stu-id="5368b-464">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="5368b-465">`Response`。</span><span class="sxs-lookup"><span data-stu-id="5368b-465">`Response`.</span></span> <span data-ttu-id="5368b-466">これは、サーバー コードの実行が終了したら、ブラウザーに送信される応答 (ページ) に関する情報のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="5368b-466">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="5368b-467">たとえば、このプロパティを使用すると、情報を応答に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="5368b-467">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="5368b-468">コレクション オブジェクト (配列とディクショナリ)</span><span class="sxs-lookup"><span data-stu-id="5368b-468">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="5368b-469">A*コレクション*のコレクションなど、同じ種類のオブジェクトのグループは、`Customer`データベースからのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5368b-469">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="5368b-470">同様に ASP.NET に多くの組み込みコレクションが含まれています、`Request.Files`コレクション。</span><span class="sxs-lookup"><span data-stu-id="5368b-470">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="5368b-471">多くの場合、コレクション内のデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="5368b-471">You'll often work with data in collections.</span></span> <span data-ttu-id="5368b-472">2 つの共通コレクション型とは、*配列*と*ディクショナリ*です。</span><span class="sxs-lookup"><span data-stu-id="5368b-472">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="5368b-473">配列は、類似した項目のコレクションを格納するたくない個別の各項目を保持する変数を作成するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="5368b-473">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="5368b-474">配列、特定のデータ型を宣言するなど`string`、 `int`、または`DateTime`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-474">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="5368b-475">変数が配列を含めることができます、宣言に角かっこを追加することを示すために (など`string[]`または`int[]`)。</span><span class="sxs-lookup"><span data-stu-id="5368b-475">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="5368b-476">アイテムの位置 (インデックス) を使用して配列またはを使用してアクセスできます、`foreach`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="5368b-476">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="5368b-477">配列のインデックスは 0 から始まる&#8212;は、最初の項目がある位置 0、2 番目の項目位置は 1 というようにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-477">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="5368b-478">取得することによって、配列内のアイテムの数を指定できます、`Length`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="5368b-478">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="5368b-479">(配列を検索します)、配列内の特定の項目の位置を取得するを使用して、`Array.IndexOf`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="5368b-479">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="5368b-480">行うことができますも逆方向のような配列の内容 (、`Array.Reverse`メソッド) するか、内容を並べ替える (、`Array.Sort`メソッド)。</span><span class="sxs-lookup"><span data-stu-id="5368b-480">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="5368b-481">ブラウザーで表示されている文字列配列のコードの出力:</span><span class="sxs-lookup"><span data-stu-id="5368b-481">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="5368b-483">設定または対応する値を取得するキー (または名前) を指定したキー/値ペアのコレクションをディクショナリには。</span><span class="sxs-lookup"><span data-stu-id="5368b-483">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="5368b-484">使用するディクショナリを作成する、`new`新しい辞書オブジェクトを作成していることを示すキーワードです。</span><span class="sxs-lookup"><span data-stu-id="5368b-484">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="5368b-485">変数を使用して、ディクショナリを割り当てることができます、`var`キーワード。</span><span class="sxs-lookup"><span data-stu-id="5368b-485">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="5368b-486">山かっこを使用してディクショナリ内の項目のデータ型を指定する ( `< >` )。</span><span class="sxs-lookup"><span data-stu-id="5368b-486">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="5368b-487">宣言の最後に、これは、実際には、メソッド、新しいディクショナリを作成するため、かっこのペアを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-487">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="5368b-488">ディクショナリに項目を追加するに呼び出せる、`Add`辞書の変数のメソッド (`myScores`ここでは)、キーと値を指定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-488">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="5368b-489">または、キーが示され、次の例のように、単純な割り当てを行うには角かっこを使用できます。</span><span class="sxs-lookup"><span data-stu-id="5368b-489">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="5368b-490">ディクショナリから値を取得するには、角かっこでキーを指定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-490">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="5368b-491">パラメーターを持つメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="5368b-491">Calling Methods with Parameters</span></span>

<span data-ttu-id="5368b-492">この記事の前半の読み取りとメソッド オブジェクトをプログラミングすることができます。</span><span class="sxs-lookup"><span data-stu-id="5368b-492">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="5368b-493">たとえば、`Database`オブジェクトがあります、`Database.Connect`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="5368b-493">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="5368b-494">多くのメソッドには、1 つまたは複数のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="5368b-494">Many methods also have one or more parameters.</span></span> <span data-ttu-id="5368b-495">A*パラメーター*メソッドに渡す値は、そのタスクを完了する方法を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5368b-495">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="5368b-496">たとえばの宣言を見て、`Request.MapPath`を 3 つのパラメーターを受け取るメソッド。</span><span class="sxs-lookup"><span data-stu-id="5368b-496">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="5368b-497">(行は読みやすくするためにラッピングされています。</span><span class="sxs-lookup"><span data-stu-id="5368b-497">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="5368b-498">ほぼすべての場所を inside 文字列以外の改行を配置することができますに注意してくださいは引用符で囲まれます)。</span><span class="sxs-lookup"><span data-stu-id="5368b-498">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="5368b-499">このメソッドは、指定した仮想パスに対応するサーバーの物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="5368b-499">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="5368b-500">次の 3 つのパラメーター、メソッドは、 `virtualPath`、 `baseVirtualDir`、および`allowCrossAppMapping`です。</span><span class="sxs-lookup"><span data-stu-id="5368b-500">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="5368b-501">(宣言では、パラメーターが表示されることを承諾するデータのデータ型とに注意してください)。このメソッドを呼び出すときに、次の 3 つのすべてのパラメーターの値を指定してください。</span><span class="sxs-lookup"><span data-stu-id="5368b-501">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="5368b-502">Razor 構文は、メソッドにパラメーターを渡すための 2 つのオプションを使用:*位置指定パラメーター*と*名前付きパラメーター*です。</span><span class="sxs-lookup"><span data-stu-id="5368b-502">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="5368b-503">位置指定パラメーターを使用して、メソッドを呼び出すには、メソッドの宣言で指定されている厳密な順序でパラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="5368b-503">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="5368b-504">(通常わかりますこの順序メソッドのドキュメントを読み取ることによってです。)、順序に従う必要があります、パラメーターのいずれかをスキップすることはできませんと&#8212;かどうか必要に応じて、空の文字列を渡す (`""`) または`null`位置指定パラメーターの値がないためです。</span><span class="sxs-lookup"><span data-stu-id="5368b-504">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="5368b-505">次の例は、という名前のフォルダーがある前提としています。*スクリプト*、web サイトにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-505">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="5368b-506">コードの呼び出し、`Request.MapPath`メソッドとパスの値が正しい順序で 3 つのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="5368b-506">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="5368b-507">結果のマップされたパスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-507">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="5368b-508">メソッドは、多くのパラメーターを持ち、ときにするおくと、コードより読みやすい名前付きパラメーターを使用しています。</span><span class="sxs-lookup"><span data-stu-id="5368b-508">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="5368b-509">名前付きパラメーターを使用してメソッドを呼び出すには、パラメーター名とそれに続くコロン (:)、し、値を指定します。</span><span class="sxs-lookup"><span data-stu-id="5368b-509">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="5368b-510">名前付きパラメーターの利点は、任意の順序で渡すことができます、です。</span><span class="sxs-lookup"><span data-stu-id="5368b-510">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="5368b-511">(欠点は、メソッドの呼び出しがコンパクトではないことです。)</span><span class="sxs-lookup"><span data-stu-id="5368b-511">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="5368b-512">次の例は、上記と同じメソッドを呼び出しますが、名前付きの値を指定するパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="5368b-512">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="5368b-513">ご覧のように、異なる順序でパラメーターが渡されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-513">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="5368b-514">ただし、前の例、この例を実行する場合は、同じ値を返すがあります。</span><span class="sxs-lookup"><span data-stu-id="5368b-514">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="5368b-515">エラー処理</span><span class="sxs-lookup"><span data-stu-id="5368b-515">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="5368b-516">Try-catch ステートメント</span><span class="sxs-lookup"><span data-stu-id="5368b-516">Try-Catch Statements</span></span>

<span data-ttu-id="5368b-517">ステートメントは、上の理由から、コントロールの外部できない可能性があるコードの多くの場合があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-517">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="5368b-518">例えば:</span><span class="sxs-lookup"><span data-stu-id="5368b-518">For example:</span></span>

- <span data-ttu-id="5368b-519">コードを作成またはファイルにアクセスしようとすると、あらゆる種類のエラーが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-519">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="5368b-520">目的のファイルが存在しない可能性があります、ロックされる可能性があります、コード可能性がありますいないアクセス許可を持つし、なります。</span><span class="sxs-lookup"><span data-stu-id="5368b-520">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="5368b-521">同様に、コードでは、データベース内のレコードを更新しようとすると、アクセス許可の問題があります、データベースへの接続を削除する場合があります、無効な場合とに、データを保存する場合があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-521">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="5368b-522">プログラミング用語では、このような状況が呼び出されます*例外*です。</span><span class="sxs-lookup"><span data-stu-id="5368b-522">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="5368b-523">(スロー) が生成されますが、コードには、例外が発生すると、エラー メッセージの最高でユーザーに迷惑な。</span><span class="sxs-lookup"><span data-stu-id="5368b-523">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="5368b-525">コードが例外を発生可能性がある場合に、この種類のエラー メッセージを回避するために、使用する`try/catch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="5368b-525">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="5368b-526">`try`をチェックする場合のコードを実行するステートメントでは、します。</span><span class="sxs-lookup"><span data-stu-id="5368b-526">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="5368b-527">1 つまたは複数`catch`ステートメントを検索できる固有の仕様が発生したエラー (特定の種類の例外)。</span><span class="sxs-lookup"><span data-stu-id="5368b-527">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="5368b-528">多くとして含めることができます`catch`としてするステートメントが予測は、エラーを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5368b-528">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="5368b-529">使用しないことをお勧め、`Response.Redirect`メソッド`try/catch`ステートメント、ページに、例外を引き起こす可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="5368b-529">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="5368b-530">次の例では、最初の要求でテキスト ファイルを作成し、ユーザーがファイルを開くできるボタンを表示するページを示します。</span><span class="sxs-lookup"><span data-stu-id="5368b-530">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="5368b-531">意図的を使って不適切なファイル名と、例外を使用することができるようにします。</span><span class="sxs-lookup"><span data-stu-id="5368b-531">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="5368b-532">コードが含まれています`catch`可能性のある例外の 2 つのステートメント: `FileNotFoundException`、ファイル名が間違っている場合に発生して`DirectoryNotFoundException`ASP.NET はフォルダーにも見つからない場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="5368b-532">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="5368b-533">(できますコメントを解除する例では、内のステートメントすべてが正しく機能しているときの動作を確認するためにします。)</span><span class="sxs-lookup"><span data-stu-id="5368b-533">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="5368b-534">コードが例外を処理していない場合、前のスクリーン ショットのようなエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5368b-534">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="5368b-535">ただし、`try/catch`セクションでは、ユーザーがこの種のエラーを表示するを防ぐのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5368b-535">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="5368b-536">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5368b-536">Additional Resources</span></span>

<span data-ttu-id="5368b-537">**Visual Basic を使用したプログラミング**</span><span class="sxs-lookup"><span data-stu-id="5368b-537">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="5368b-538">付録: Visual Basic 言語と構文</span><span class="sxs-lookup"><span data-stu-id="5368b-538">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="5368b-539">**リファレンス ドキュメント**</span><span class="sxs-lookup"><span data-stu-id="5368b-539">**Reference Documentation**</span></span>


[<span data-ttu-id="5368b-540">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5368b-540">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="5368b-541">C# 言語</span><span class="sxs-lookup"><span data-stu-id="5368b-541">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
