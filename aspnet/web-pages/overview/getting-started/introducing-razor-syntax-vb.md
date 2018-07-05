---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要 |Microsoft Docs
author: tfitzmac
description: この付録概要 ASP.NET Web ページを使用したプログラミングの Visual basic で Razor 構文を使用します。
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 72f995e62141df4e8f4cd082b4873d82067af8c1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816549"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="5b796-103">Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="5b796-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="5b796-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5b796-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5b796-105">この記事では、プログラミングの概要 ASP.NET Web Pages を Razor の構文と Visual Basic を使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="5b796-106">ASP.NET は、web サーバー上で動的な web ページを実行するためのマイクロソフトのテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="5b796-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="5b796-107">**学習内容**:</span><span class="sxs-lookup"><span data-stu-id="5b796-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="5b796-108">プログラミングの Razor 構文を使用して ASP.NET Web Pages のプログラミングの概要に関するヒント上位 8。</span><span class="sxs-lookup"><span data-stu-id="5b796-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="5b796-109">基本的なプログラミング概念必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="5b796-110">どのような ASP.NET サーバー コードと Razor 構文についてです。</span><span class="sxs-lookup"><span data-stu-id="5b796-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="5b796-111">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="5b796-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="5b796-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5b796-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5b796-113">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="5b796-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="5b796-114">Razor 構文を使用する ASP.NET Web ページを使用するほとんどの例は c# を使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="5b796-115">Razor 構文では、Visual Basic もサポートされています。</span><span class="sxs-lookup"><span data-stu-id="5b796-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="5b796-116">Visual Basic での ASP.NET web ページをプログラミングで web ページを作成する、 *.vbhtml*ファイル名拡張子、Visual Basic コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5b796-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="5b796-117">この記事では、Visual Basic 言語と ASP.NET web ページを作成する構文の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="5b796-118">Microsoft WebMatrix の既定の web サイト テンプレート (**パン屋さん**、**フォト ギャラリー**、および**スターター サイト**など) は c# および Visual Basic のバージョンで使用できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="5b796-119">NuGet パッケージとしてでは、Visual Basic テンプレートをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="5b796-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="5b796-120">Web サイト テンプレートがという名前のフォルダーで、サイトのルート フォルダーにインストールされている*Microsoft テンプレート*します。</span><span class="sxs-lookup"><span data-stu-id="5b796-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="5b796-121">最上位の 8 つのプログラミングのヒント</span><span class="sxs-lookup"><span data-stu-id="5b796-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="5b796-122">このセクションでは、Razor 構文を使用して ASP.NET サーバー コードの記述を開始する際に知っておく必要ないくつかのヒントを示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="5b796-123">1.コードを追加するページを使用して、@ 文字</span><span class="sxs-lookup"><span data-stu-id="5b796-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="5b796-124">`@`文字は、インライン式、単一ステートメント ブロック、および複数ステートメントのブロックを開始します。</span><span class="sxs-lookup"><span data-stu-id="5b796-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="5b796-125">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="5b796-127">**HTML エンコード**</span><span class="sxs-lookup"><span data-stu-id="5b796-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="5b796-128">ページを使用して、コンテンツを表示すると、 `@` ASP.NET HTML エンコードの出力を前の例のように文字します。</span><span class="sxs-lookup"><span data-stu-id="5b796-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="5b796-129">これにより、HTML の予約文字が置き換えられます (など`<`と`>`と`&`) の HTML タグまたはエンティティとして解釈されるのではなく、web ページ内の文字として表示される文字を有効にするコードを持つ。</span><span class="sxs-lookup"><span data-stu-id="5b796-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="5b796-130">HTML エンコードをせずサーバー コードからの出力は正しく表示されない場合があり、セキュリティのリスクについてのページを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="5b796-131">目的がマークアップとしてタグをレンダリングする HTML マークアップを出力するかどうか (たとえば`<p></p>`段落のまたは`<em></em>`テキストを強調するために) を参照してください[を組み合わせることのテキスト、マークアップ、およびコード ブロック内のコード](#BM_CombiningTextMarkupAndCode)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="5b796-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="5b796-132">詳細での HTML エンコードについて[ASP.NET Web Pages サイトでの HTML フォームを使用する](https://go.microsoft.com/fwlink/?LinkId=202892)します。</span><span class="sxs-lookup"><span data-stu-id="5b796-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="5b796-133">2.コードのコード ブロックを囲むとしています.終了コード</span><span class="sxs-lookup"><span data-stu-id="5b796-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="5b796-134">コード ブロックが 1 つまたは複数のコード ステートメントが含まれていて、キーワードで囲まれた`Code`と`End Code`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="5b796-135">配置開始`Code`キーワードの直後に、`@`文字&#8212;それらの間を空白にすることはできませんがあります。</span><span class="sxs-lookup"><span data-stu-id="5b796-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="5b796-136">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="5b796-138">3.ブロック内で改行するのには、各コード ステートメントを終了します。</span><span class="sxs-lookup"><span data-stu-id="5b796-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="5b796-139">Visual Basic のコード ブロックには、各ステートメントは、改行で終わります。</span><span class="sxs-lookup"><span data-stu-id="5b796-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="5b796-140">(記事の後半で必要な場合は、複数の行に長いコード ステートメントをラップする方法を参照します)。</span><span class="sxs-lookup"><span data-stu-id="5b796-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="5b796-141">4.値を格納するのに変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-141">4. You use variables to store values</span></span>

<span data-ttu-id="5b796-142">内の値を格納することができます、*変数*文字列、数字、および日付などを含め、します。新しい変数を使用して、作成する、`Dim`キーワード。</span><span class="sxs-lookup"><span data-stu-id="5b796-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="5b796-143">変数の値を挿入するにを使用してページに直接`@`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="5b796-144">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="5b796-146">5.リテラル文字列値を二重引用符で括る</span><span class="sxs-lookup"><span data-stu-id="5b796-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="5b796-147">A*文字列*テキストとして扱われる文字のシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="5b796-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="5b796-148">文字列を指定するには、二重引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="5b796-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="5b796-149">文字列値内で二重引用符を埋め込むには、するには、2 つの二重引用符文字を挿入します。</span><span class="sxs-lookup"><span data-stu-id="5b796-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="5b796-150">ページの出力に 1 回出現する二重引用符文字にする場合は、入力として`""`内で、引用符で囲まれた文字列、および 2 回表示する場合は入力として`""""`引用符で囲まれた文字列内で。</span><span class="sxs-lookup"><span data-stu-id="5b796-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="5b796-151">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="5b796-153">6.Visual Basic のコードは大文字小文字を区別しません。</span><span class="sxs-lookup"><span data-stu-id="5b796-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="5b796-154">Visual Basic 言語は大文字小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="5b796-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="5b796-155">プログラミング キーワード (など`Dim`、 `If`、および`True`) と変数名 (など`myString`、または`subTotal`) いずれの場合も記述できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="5b796-156">次のコード行では、変数に値を割り当てる`lastname`小文字を使用して、名前し、ページの大文字の名前を使用する変数の値を出力します。</span><span class="sxs-lookup"><span data-stu-id="5b796-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="5b796-157">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="5b796-159">7.コーディングのほとんどには、オブジェクトの操作が含まれます</span><span class="sxs-lookup"><span data-stu-id="5b796-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="5b796-160">オブジェクトをプログラミングできることを表します&#8212;ページ、テキスト ボックス、ファイル、画像、web 要求を電子メール メッセージ、(データベースの行) の顧客レコードなど。オブジェクトの特性を記述するプロパティがある&#8212;テキスト ボックスのオブジェクトが、`Text`プロパティ、要求オブジェクトが、`Url`プロパティ、電子メール メッセージが、`From`プロパティ、および customer オブジェクトが、 `FirstName`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5b796-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="5b796-161">オブジェクトには、メソッドも含まれて、&quot;動詞&quot;を実行できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="5b796-162">例としては、ファイル オブジェクトの`Save`メソッドは、イメージ オブジェクトの`Rotate`メソッド、および電子メール オブジェクトの`Send`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5b796-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="5b796-163">多くの場合、使用する、`Request`オブジェクトは、フォームの値などの情報を提供するフィールド (テキスト ボックスなど) のページにブラウザーの種類は、要求、ページ、ユーザー id などの URL を作成します。この例のプロパティにアクセスする方法を示しています、`Request`オブジェクトおよび呼び出す方法、`MapPath`のメソッド、`Request`オブジェクトで、サーバーで、ページの絶対パスを確認できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="5b796-164">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="5b796-166">8.意思決定を行うコードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="5b796-167">動的な web ページの重要な機能は、条件に基づいて対処方法を決定できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="5b796-168">これを行う最も一般的な方法は、`If`ステートメント (と省略可能な`Else`ステートメント)。</span><span class="sxs-lookup"><span data-stu-id="5b796-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="5b796-169">ステートメント`If IsPost`は記事の執筆を簡略化`If IsPost = True`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="5b796-170">と共に`If`ステートメント、さまざまなコードのブロックを繰り返し、条件をテストする方法があるしはこの記事の後半で説明されています。</span><span class="sxs-lookup"><span data-stu-id="5b796-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="5b796-171">結果がブラウザーで表示されます (クリックすると**送信**)。</span><span class="sxs-lookup"><span data-stu-id="5b796-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="5b796-173">**HTTP GET と POST メソッドと IsPost プロパティ**</span><span class="sxs-lookup"><span data-stu-id="5b796-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="5b796-174">Web ページ (HTTP) で使用されるプロトコルは、メソッドの非常に限られた数をサポートしています (&quot;動詞&quot;) サーバーに要求を行うに使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="5b796-175">2 つの最も一般的なは、ページの読み取りに使用される、GET と POST のページを送信するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="5b796-176">一般に、初めてユーザーの要求 ページでは、ページの要求が GET を使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="5b796-177">かどうか、ユーザーは、フォームに入力して、クリックして**送信**ブラウザー、サーバーへの POST 要求。</span><span class="sxs-lookup"><span data-stu-id="5b796-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="5b796-178">Web プログラミング、かどうか、ページを要求する、POST または GET としてページを処理する方法がわかるように、知っておくと便利がよくあります。</span><span class="sxs-lookup"><span data-stu-id="5b796-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="5b796-179">使用する ASP.NET Web ページで、`IsPost`プロパティを要求が GET または POST がかどうかを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5b796-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="5b796-180">要求が、投稿の場合、`IsPost`プロパティが true を返すし、フォーム上のテキスト ボックスの値などの読み取りを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="5b796-181">値に応じて異なる方法でページを処理する方法がわかります多くの例に示します`IsPost`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="5b796-182">単純なコード例</span><span class="sxs-lookup"><span data-stu-id="5b796-182">A Simple Code Example</span></span>

<span data-ttu-id="5b796-183">この手順では、基本的なプログラミング手法を説明するページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="5b796-184">例では、次にそれらを追加し、結果が表示されます、2 つの数値を入力できるようにするページを作成します。</span><span class="sxs-lookup"><span data-stu-id="5b796-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="5b796-185">エディターで、新しいファイルを作成し、名前*AddNumbers.vbhtml*します。</span><span class="sxs-lookup"><span data-stu-id="5b796-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="5b796-186">ページでは、既にページ内のあらゆるものを置き換えるには、次のコードとマークアップをコピーします。</span><span class="sxs-lookup"><span data-stu-id="5b796-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="5b796-187">ここでは、いくつかの点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="5b796-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="5b796-188">`@`文字 ページで、コードの最初のブロックを開始して、その直後にある、`totalMessage`下部にある変数が埋め込まれています。</span><span class="sxs-lookup"><span data-stu-id="5b796-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="5b796-189">ページの上部にあるブロックがで囲まれた`Code...End Code`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="5b796-190">変数`total`、 `num1`、 `num2`、および`totalMessage`複数の数値と文字列を格納します。</span><span class="sxs-lookup"><span data-stu-id="5b796-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="5b796-191">割り当てられているリテラル文字列値、`totalMessage`変数は、二重引用符内。</span><span class="sxs-lookup"><span data-stu-id="5b796-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="5b796-192">Visual Basic コードは小文字を区別しない場合があるため、`totalMessage`ページの下部にある変数が使用される、その名前は、ページの上部にある変数の宣言のスペルが一致するようにのみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="5b796-193">大文字小文字の区別が重要でありません。</span><span class="sxs-lookup"><span data-stu-id="5b796-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="5b796-194">式`num1.AsInt()`  +  `num2.AsInt()`オブジェクトおよびメソッドを使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5b796-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="5b796-195">`AsInt`各変数のメソッドに追加できる整数 (整数) に、ユーザーが入力した文字列を変換します。</span><span class="sxs-lookup"><span data-stu-id="5b796-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="5b796-196">`<form>`タグが含まれています、`method="post"`属性。</span><span class="sxs-lookup"><span data-stu-id="5b796-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="5b796-197">指定のユーザーがクリックしたときに**追加**ページは、HTTP POST メソッドを使用して、サーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="5b796-198">ページを送信するときに、コード`If IsPost`true に評価され、条件付きコードの数値を加算の結果を表示するを実行します。</span><span class="sxs-lookup"><span data-stu-id="5b796-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="5b796-199">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="5b796-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="5b796-200">(内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。2 つの整数を入力し、クリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5b796-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="5b796-202">Visual Basic 言語と構文</span><span class="sxs-lookup"><span data-stu-id="5b796-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="5b796-203">以前、ASP.NET web ページを作成する方法と、HTML マークアップをサーバー コードを追加する方法の基本的な例を説明しました。</span><span class="sxs-lookup"><span data-stu-id="5b796-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="5b796-204">Visual Basic を使用して、Razor 構文を使用して ASP.NET サーバー コードを記述するの基礎を学習ここ&#8212;プログラミング言語の規則では、します。</span><span class="sxs-lookup"><span data-stu-id="5b796-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="5b796-205">(特に C、C++、c#、Visual Basic、または JavaScript を使用した) 場合のプログラミング経験豊富な場合は、ここで読んだの多くが使い慣れたになります。</span><span class="sxs-lookup"><span data-stu-id="5b796-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="5b796-206">内のマークアップに WebMatrix のコードを追加する方法でのみについて理解しておく必要がありますおそらく *.vbhtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5b796-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="5b796-207">テキスト、マークアップ、およびコード ブロック内のコードを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="5b796-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="5b796-208">サーバー コード ブロックでは、テキスト、およびページ マークアップを出力するを多くの場合。</span><span class="sxs-lookup"><span data-stu-id="5b796-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="5b796-209">サーバー コード ブロックにコードでないし、する代わりに表示するか、テキストが含まれている場合、ASP.NET をコードからそのテキストを区別するためにできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="5b796-210">これにはいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-210">There are several ways to do this.</span></span>

- <span data-ttu-id="5b796-211">などの HTML ブロック要素で、テキストを囲む`<p></p>`または`<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="5b796-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="5b796-212">HTML 要素には、テキスト、HTML 要素の追加、およびサーバー コード式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="5b796-213">ASP.NET が HTML の開始タグを表示する場合 (たとえば、 `<p>`)、要素をレンダリングすべておよびとしてそのコンテンツがブラウザー (およびサーバー コード式を解決) に、します。</span><span class="sxs-lookup"><span data-stu-id="5b796-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="5b796-214">使用して、`@:`演算子または`<text>`要素。</span><span class="sxs-lookup"><span data-stu-id="5b796-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="5b796-215">`@:`プレーン テキストまたは HTML タグが一致していませんが含まれているコンテンツを 1 行の出力、`<text>`要素を出力する複数の行で囲みます。</span><span class="sxs-lookup"><span data-stu-id="5b796-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="5b796-216">これらのオプションは、出力の一部として HTML 要素を表示したくない場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="5b796-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="5b796-217">次の例では、前の例の繰り返しですがの 1 つのペアを使用して`<text>`タグを表示するテキストを囲みます。</span><span class="sxs-lookup"><span data-stu-id="5b796-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="5b796-218">次の例では、`<text>`と`</text>`タグは、一部の非包含エンティティのテキストと HTML タグの不一致があるすべての 3 つの行を囲む (`<br />`)、サーバー コードと一致した HTML タグ。</span><span class="sxs-lookup"><span data-stu-id="5b796-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="5b796-219">ここでも、使用して個別に各の行の前でしたも、`@:`演算子です。 いずれかの方法の動作。</span><span class="sxs-lookup"><span data-stu-id="5b796-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="5b796-220">このセクションで示すようにテキストを出力するときに&#8212;、HTML 要素を使用して、`@:`演算子、または`<text>`要素&#8212;ASP.NET は HTML エンコード出力します。</span><span class="sxs-lookup"><span data-stu-id="5b796-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="5b796-221">(ASP.NET がサーバーのコード式とが付いているサーバー コード ブロックの出力をエンコードする前述のように、 `@`、このセクションに記載されている特殊なケースでは可します)。</span><span class="sxs-lookup"><span data-stu-id="5b796-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="5b796-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="5b796-222">Whitespace</span></span>

<span data-ttu-id="5b796-223">余分なスペース (および、文字列リテラルの外部で) ステートメントでステートメントには影響しません。</span><span class="sxs-lookup"><span data-stu-id="5b796-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="5b796-224">長いステートメントを複数の行に分割</span><span class="sxs-lookup"><span data-stu-id="5b796-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="5b796-225">アンダー スコア文字を使用して、複数の行に長いコード ステートメントを中断できます`_`(Visual basic と呼ばれる、*連結文字*) 各行のコードの後にします。</span><span class="sxs-lookup"><span data-stu-id="5b796-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="5b796-226">次の行にステートメントを中断するには、行の末尾にスペースと連結文字を追加します。</span><span class="sxs-lookup"><span data-stu-id="5b796-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="5b796-227">次の行にステートメントを続行します。</span><span class="sxs-lookup"><span data-stu-id="5b796-227">Continue the statement on the next line.</span></span> <span data-ttu-id="5b796-228">読みやすさを向上させるために必要な数の行にステートメントをラップすることができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="5b796-229">次のステートメントでは、同じです。</span><span class="sxs-lookup"><span data-stu-id="5b796-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="5b796-230">ただし、文字列リテラル内の行をラップすることはできません。</span><span class="sxs-lookup"><span data-stu-id="5b796-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="5b796-231">次の例が機能しません。</span><span class="sxs-lookup"><span data-stu-id="5b796-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="5b796-232">使用する必要を上記のコードのような複数の行にラップする長い文字列を結合する、*連結演算子*(`&`)、この記事の後半でがあります。</span><span class="sxs-lookup"><span data-stu-id="5b796-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="5b796-233">コードのコメント</span><span class="sxs-lookup"><span data-stu-id="5b796-233">Code comments</span></span>

<span data-ttu-id="5b796-234">コメントを使用して、自分自身または他のユーザーは、ノートをそのまま使用できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="5b796-235">Razor 構文のコメントが付いて`@*`と末尾`*@`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="5b796-236">コード ブロック内でコメントを使用して、Razor 構文、または単一引用符は、通常の Visual Basic のコメント文字を使用することができます (`'`) 各の行にプレフィックスとして付加されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="5b796-237">変数</span><span class="sxs-lookup"><span data-stu-id="5b796-237">Variables</span></span>

<span data-ttu-id="5b796-238">変数は、データの格納に使用する名前付きオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="5b796-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="5b796-239">名前を付ける変数、何かが、名前がアルファベットの文字で始める必要があり、空白文字または予約文字を含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="5b796-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="5b796-240">Visual basic で、前に説明したように、変数名の文字の大文字と小文字は関係ありません。</span><span class="sxs-lookup"><span data-stu-id="5b796-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="5b796-241">変数とデータ型</span><span class="sxs-lookup"><span data-stu-id="5b796-241">Variables and data types</span></span>

<span data-ttu-id="5b796-242">変数は、データの種類が変数に格納されていることを示しますが、特定のデータ型を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="5b796-243">文字列値を格納する文字列変数があることができます (よう&quot;Hello world&quot;)、(3 または 79) などの整数値を格納する整数変数とさまざまな (4/12/2012 や 2009 年 3 月などの形式で日付の値を格納する date 型の変数).</span><span class="sxs-lookup"><span data-stu-id="5b796-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="5b796-244">その他の多くのデータ型を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="5b796-245">ただし、変数の型を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5b796-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="5b796-246">ほとんどの場合 ASP.NET は、変数内のデータの使用方法に基づいて型を出すことができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="5b796-247">(場合によっては、型を指定する必要があります。 これは true、例が表示されます)。</span><span class="sxs-lookup"><span data-stu-id="5b796-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="5b796-248">型を指定せずに変数を宣言するには使用`Dim`さらに、変数名 (たとえば、 `Dim myVar`)。</span><span class="sxs-lookup"><span data-stu-id="5b796-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="5b796-249">型の変数を宣言するには使用`Dim`さらに、変数名に続けて`As`してから、型名 (たとえば、 `Dim myVar As String`)。</span><span class="sxs-lookup"><span data-stu-id="5b796-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="5b796-250">次の例では、web ページで、変数を使用する一部のインライン式を示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="5b796-251">ブラウザーで表示される結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="5b796-253">変換して、データ型をテストします。</span><span class="sxs-lookup"><span data-stu-id="5b796-253">Converting and testing data types</span></span>

<span data-ttu-id="5b796-254">ASP.NET は、データ型を自動的に決定することができます、通常が場合があります、ことはできません。</span><span class="sxs-lookup"><span data-stu-id="5b796-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="5b796-255">そのため、明示的な変換を実行することで ASP.NET を手助けする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="5b796-256">型変換を持っていない場合でもデータの種類が使用することを確認するテストに便利です。</span><span class="sxs-lookup"><span data-stu-id="5b796-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="5b796-257">最も一般的なは、整数や日付など、別の型を文字列に変換することがあります。</span><span class="sxs-lookup"><span data-stu-id="5b796-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="5b796-258">次の例では、文字列を数値に変換する必要があります、一般的なケースを示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="5b796-259">原則として、ユーザー入力では、文字列として。</span><span class="sxs-lookup"><span data-stu-id="5b796-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="5b796-260">番号を入力するユーザーを要求した場合でも、および数字をユーザー入力が送信され、コードでの読み取り時に入力する場合でも、データは文字列の形式です。</span><span class="sxs-lookup"><span data-stu-id="5b796-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="5b796-261">そのため、文字列を数値に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="5b796-262">例では、変換、しなくても、値に対して算術演算を実行しようとする場合、次のエラーの結果、ASP.NET は、2 つの文字列を追加できないため。</span><span class="sxs-lookup"><span data-stu-id="5b796-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="5b796-263">呼び出すの値を整数に変換する、`AsInt`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5b796-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="5b796-264">変換が成功した場合は、数値を追加できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="5b796-265">次の表では、変数の一般的ないくつかの変換とテスト方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-265">The following table lists some common conversion and test methods for variables.</span></span>


<span data-ttu-id="5b796-266">行:: 列:<strong>メソッド</strong>: 列エンド:: 列:<strong>説明</strong>: 列エンド:: 列:<strong>例</strong>: 列終了:: 行の終わり。</span><span class="sxs-lookup"><span data-stu-id="5b796-266">:::row::: :::column::: <strong>Method</strong> :::column-end::: :::column::: <strong>Description</strong> :::column-end::: :::column::: <strong>Example</strong> :::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-267">行:: 列: `AsInt(), IsInt()` : 列エンドツー エンド:: 列: 整数を表す文字列に変換します (など&quot;593&quot;) 整数にします。</span><span class="sxs-lookup"><span data-stu-id="5b796-267">:::row::: :::column::: `AsInt(), IsInt()` :::column-end::: :::column::: Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
<span data-ttu-id="5b796-268">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-268">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]</span></span>
    <span data-ttu-id="5b796-269">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-269">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-270">行:: 列: `AsBool(), IsBool()` : 列エンド:: 列: のような文字列に変換します&quot;true&quot;または&quot;false&quot;ブール型にします。</span><span class="sxs-lookup"><span data-stu-id="5b796-270">:::row::: :::column::: `AsBool(), IsBool()` :::column-end::: :::column::: Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
<span data-ttu-id="5b796-271">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-271">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]</span></span>
    <span data-ttu-id="5b796-272">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-272">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-273">行:: 列: `AsFloat(), IsFloat()` : 列エンド:: 列: のような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を浮動小数点数。</span><span class="sxs-lookup"><span data-stu-id="5b796-273">:::row::: :::column::: `AsFloat(), IsFloat()` :::column-end::: :::column::: Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
<span data-ttu-id="5b796-274">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-274">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]</span></span>
    <span data-ttu-id="5b796-275">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-275">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-276">行:: 列: `AsDecimal(), IsDecimal()` : 列エンド:: 列: のような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot; 10 進数。</span><span class="sxs-lookup"><span data-stu-id="5b796-276">:::row::: :::column::: `AsDecimal(), IsDecimal()` :::column-end::: :::column::: Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="5b796-277">(ASP.NET では、10 進数、浮動小数点数よりも正確)。列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-277">(In ASP.NET, a decimal number is more precise than a floating-point number.) :::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]</span></span>
    <span data-ttu-id="5b796-278">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-278">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-279">行:: 列: `AsDateTime(), IsDateTime()` : 列エンド:: 列: asp.net の日付と時刻の値を表す文字列に変換します`DateTime`型。</span><span class="sxs-lookup"><span data-stu-id="5b796-279">:::row::: :::column::: `AsDateTime(), IsDateTime()` :::column-end::: :::column::: Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
<span data-ttu-id="5b796-280">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-280">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]</span></span>
    <span data-ttu-id="5b796-281">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-281">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-282">行:: 列: `ToString()` : 列エンド:: 列: その他の任意のデータ型を文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="5b796-282">:::row::: :::column::: `ToString()` :::column-end::: :::column::: Converts any other data type to a string.</span></span>
<span data-ttu-id="5b796-283">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-283">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]</span></span>
    <span data-ttu-id="5b796-284">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-284">:::column-end::: :::row-end:::</span></span>


## <a name="operators"></a><span data-ttu-id="5b796-285">演算子</span><span class="sxs-lookup"><span data-stu-id="5b796-285">Operators</span></span>

<span data-ttu-id="5b796-286">演算子は、キーワードまたは式の中で実行するコマンドの種類を ASP.NET に指示する文字です。</span><span class="sxs-lookup"><span data-stu-id="5b796-286">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="5b796-287">Visual Basic は、多くの演算子をサポートしていますが、ASP.NET web ページの開発を開始するいくつかを認識するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="5b796-287">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="5b796-288">次の表では、最も一般的な演算子をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="5b796-288">The following table summarizes the most common operators.</span></span>


<span data-ttu-id="5b796-289">行:: 列:<strong>演算子</strong>: 列エンド:: 列:<strong>説明</strong>: 列エンド:: 列:<strong>例</strong>: 列エンド:: 行の終わり。</span><span class="sxs-lookup"><span data-stu-id="5b796-289">:::row::: :::column::: <strong>Operator</strong> :::column-end::: :::column::: <strong>Description</strong> :::column-end::: :::column::: <strong>Examples</strong> :::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-290">行:: 列: `+ - * /` : 列エンド:: 列: 算術演算子が数値式で使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-290">:::row::: :::column::: `+ - * /` :::column-end::: :::column::: Math operators used in numerical expressions.</span></span>
<span data-ttu-id="5b796-291">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-291">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]</span></span>
    <span data-ttu-id="5b796-292">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-292">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-293">行:: 列: `=` : 列エンド:: 列: の割り当てと等しいかどうか。</span><span class="sxs-lookup"><span data-stu-id="5b796-293">:::row::: :::column::: `=` :::column-end::: :::column::: Assignment and equality.</span></span> <span data-ttu-id="5b796-294">、コンテキストに応じてはいずれか、左側にあるオブジェクトにステートメントの右側にある値を割り当てるか、値の等価性を確認します。</span><span class="sxs-lookup"><span data-stu-id="5b796-294">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
<span data-ttu-id="5b796-295">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-295">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]</span></span>
    <span data-ttu-id="5b796-296">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-296">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-297">行:: 列: `<>` : 列エンド:: 列: 非等値。</span><span class="sxs-lookup"><span data-stu-id="5b796-297">:::row::: :::column::: `<>` :::column-end::: :::column::: Inequality.</span></span> <span data-ttu-id="5b796-298">返します`True`値が等しくない場合。</span><span class="sxs-lookup"><span data-stu-id="5b796-298">Returns `True` if the values are not equal.</span></span>
<span data-ttu-id="5b796-299">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-299">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]</span></span>
    <span data-ttu-id="5b796-300">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-300">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-301">行:: 列: `< > <= >=` : 列エンド:: 列: よりも小さい、多い、少ないよりまたは等しいかとより大きいまたは等しい。</span><span class="sxs-lookup"><span data-stu-id="5b796-301">:::row::: :::column::: `< > <= >=` :::column-end::: :::column::: Less than, greater than, less than or equal, and greater than or equal.</span></span>
<span data-ttu-id="5b796-302">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-302">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]</span></span>
    <span data-ttu-id="5b796-303">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-303">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-304">行:: 列: `&` : 列エンド:: 列: 連結、文字列を結合するために使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-304">:::row::: :::column::: `&` :::column-end::: :::column::: Concatenation, which is used to join strings.</span></span>
<span data-ttu-id="5b796-305">列エンド:: 列。 [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]</span><span class="sxs-lookup"><span data-stu-id="5b796-305">:::column-end::: :::column::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]</span></span>
    <span data-ttu-id="5b796-306">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-306">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-307">行:: 列: `+= -=` : 列エンド:: 列: インクリメントおよびデクリメント演算子を追加し、変数から 1 をそれぞれ減算します。</span><span class="sxs-lookup"><span data-stu-id="5b796-307">:::row::: :::column::: `+= -=` :::column-end::: :::column::: The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
<span data-ttu-id="5b796-308">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-308">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]</span></span>
    <span data-ttu-id="5b796-309">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-309">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-310">行:: 列: `.` : 列エンド:: 列: ドットです。</span><span class="sxs-lookup"><span data-stu-id="5b796-310">:::row::: :::column::: `.` :::column-end::: :::column::: Dot.</span></span> <span data-ttu-id="5b796-311">オブジェクトとそのプロパティおよびメソッドを区別するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-311">Used to distinguish objects and their properties and methods.</span></span>
<span data-ttu-id="5b796-312">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-312">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]</span></span>
    <span data-ttu-id="5b796-313">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-313">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-314">行:: 列: `()` : 列エンド:: 列: かっこです。</span><span class="sxs-lookup"><span data-stu-id="5b796-314">:::row::: :::column::: `()` :::column-end::: :::column::: Parentheses.</span></span> <span data-ttu-id="5b796-315">グループ式の場合に使用すると、メソッド、および配列とコレクションのメンバーにアクセスする、パラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="5b796-315">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
<span data-ttu-id="5b796-316">列エンド:: 列。 [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]</span><span class="sxs-lookup"><span data-stu-id="5b796-316">:::column-end::: :::column::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]</span></span>
    <span data-ttu-id="5b796-317">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-317">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-318">行:: 列: `Not` : 列エンド:: 列: ありません。</span><span class="sxs-lookup"><span data-stu-id="5b796-318">:::row::: :::column::: `Not` :::column-end::: :::column::: Not.</span></span> <span data-ttu-id="5b796-319">False、またはその逆は真の値を反転させます。</span><span class="sxs-lookup"><span data-stu-id="5b796-319">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="5b796-320">通常のテストを簡略化として使用される`False`(には、いない`True`)。</span><span class="sxs-lookup"><span data-stu-id="5b796-320">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
<span data-ttu-id="5b796-321">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-321">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]</span></span>
    <span data-ttu-id="5b796-322">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-322">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="5b796-323">行:: 列: `AndAlso OrElse` : 列エンド:: 列: 論理およびまたはと、条件をまとめてリンクに使用されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-323">:::row::: :::column::: `AndAlso OrElse` :::column-end::: :::column::: Logical AND and OR, which are used to link conditions together.</span></span>
<span data-ttu-id="5b796-324">列エンド:: 列。 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]</span><span class="sxs-lookup"><span data-stu-id="5b796-324">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]</span></span>
    <span data-ttu-id="5b796-325">列エンド:: 行終了。</span><span class="sxs-lookup"><span data-stu-id="5b796-325">:::column-end::: :::row-end:::</span></span>

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="5b796-326">ファイルとコード内のフォルダー パスを使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-326">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="5b796-327">コードでは、ファイルとフォルダーのパスを持つ多くの場合、操作します。</span><span class="sxs-lookup"><span data-stu-id="5b796-327">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="5b796-328">開発用コンピューターに表示される次の web サイトの物理フォルダー構造の例に示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-328">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="5b796-329">Url およびパスについていくつかの重要な詳細情報を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-329">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="5b796-330">URL のいずれかでドメイン名を開始 (`http://www.example.com`) またはサーバー名 (`http://localhost`、 `http://mycomputer`)。</span><span class="sxs-lookup"><span data-stu-id="5b796-330">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="5b796-331">URL は、ホスト コンピューター上の物理パスに対応します。</span><span class="sxs-lookup"><span data-stu-id="5b796-331">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="5b796-332">たとえば、`http://myserver`フォルダーに対応する*C:\websites\mywebsite*サーバー。</span><span class="sxs-lookup"><span data-stu-id="5b796-332">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="5b796-333">仮想パスは、完全なパスを指定せずに、コード内のパスを表すための短縮形です。</span><span class="sxs-lookup"><span data-stu-id="5b796-333">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="5b796-334">ドメインまたはサーバー名に続く URL の一部が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5b796-334">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="5b796-335">仮想パスを使用すると、パスを更新することがなく別のドメインまたはサーバーに、コードを移動できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-335">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="5b796-336">相違点の理解に役立つ例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-336">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="5b796-337">完全な URL</span><span class="sxs-lookup"><span data-stu-id="5b796-337">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="5b796-338">サーバー名</span><span class="sxs-lookup"><span data-stu-id="5b796-338">Server name</span></span> | <span data-ttu-id="5b796-339">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="5b796-339">*mycompanyserver*</span></span> |
| <span data-ttu-id="5b796-340">[仮想パス]</span><span class="sxs-lookup"><span data-stu-id="5b796-340">Virtual path</span></span> | <span data-ttu-id="5b796-341">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="5b796-341">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="5b796-342">物理パス</span><span class="sxs-lookup"><span data-stu-id="5b796-342">Physical path</span></span> | <span data-ttu-id="5b796-343">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="5b796-343">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="5b796-344">仮想ルートは、/、ドライブは、c: のルートと同じように \ です。</span><span class="sxs-lookup"><span data-stu-id="5b796-344">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="5b796-345">(仮想フォルダーのパス常にスラッシュを使用します。)フォルダーの仮想パスとして、物理フォルダー。 同じ名前を指定する必要はありません。エイリアスがあります。</span><span class="sxs-lookup"><span data-stu-id="5b796-345">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="5b796-346">(実稼働サーバー上の仮想パスことはほとんどありませんと一致する特定の物理パス。)</span><span class="sxs-lookup"><span data-stu-id="5b796-346">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="5b796-347">コードのファイルとフォルダーを使用する場合は場合がありますの物理パスおよび場合によってはどのようなオブジェクトを使用しているに応じての仮想パスを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-347">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="5b796-348">ASP.NET ツールを提供するこれらのコードでのファイルとフォルダーのパスの作業:`Server.MapPath`メソッド、および`~`演算子と`Href`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5b796-348">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="5b796-349">仮想物理パスに変換する: Server.MapPath メソッド</span><span class="sxs-lookup"><span data-stu-id="5b796-349">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="5b796-350">`Server.MapPath`メソッドが仮想パスに変換します (など */default.cshtml*) を絶対物理パス (など*C:\WebSites\MyWebSiteFolder\default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="5b796-350">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="5b796-351">このメソッドを使用すると、いつでも完全な物理パスを作成する必要がありますに。</span><span class="sxs-lookup"><span data-stu-id="5b796-351">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="5b796-352">典型的な例では、読み取りまたはテキスト ファイルまたは web サーバー上の画像ファイルを作成する場合です。</span><span class="sxs-lookup"><span data-stu-id="5b796-352">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="5b796-353">通常ホスティング サイトのサーバー上のサイトの絶対物理パスがわからない、わかっているため、このメソッドは、パスを変換できます: 仮想パス: サーバー上の対応するパスにします。</span><span class="sxs-lookup"><span data-stu-id="5b796-353">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="5b796-354">仮想パスをファイルまたはフォルダー、メソッドに渡すし、物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="5b796-354">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="5b796-355">仮想ルートを参照する: ~ 演算子と Href メソッド</span><span class="sxs-lookup"><span data-stu-id="5b796-355">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="5b796-356">*.Cshtml*または *.vbhtml*ファイル、仮想ルート パスを使用して、参照することができます、`~`演算子。</span><span class="sxs-lookup"><span data-stu-id="5b796-356">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="5b796-357">これは、機能は、サイトでは、内のページを移動することができ、他のページが含まれているすべてのリンクが壊れているできませんので、非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="5b796-357">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="5b796-358">これまで、web サイトを別の場所に移動する場合にも便利です。</span><span class="sxs-lookup"><span data-stu-id="5b796-358">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="5b796-359">次にいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-359">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="5b796-360">場合は、web サイトが`http://myserver/myapp`、ここでは、ASP.NET が処理する方法これらのパス、ページの実行時に。</span><span class="sxs-lookup"><span data-stu-id="5b796-360">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="5b796-361">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="5b796-361">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="5b796-362">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="5b796-362">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="5b796-363">(実際には、変数の値としてこれらのパスは表示されませんが ASP.NET は、パスとして扱うだったです)。</span><span class="sxs-lookup"><span data-stu-id="5b796-363">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="5b796-364">使用することができます、 `~` (上記) としてサーバー コードと次のように、マークアップの両方の演算子。</span><span class="sxs-lookup"><span data-stu-id="5b796-364">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="5b796-365">マークアップでは、使用する、`~`演算子イメージ ファイル、その他の web ページ、および CSS ファイルなどのリソースへのパスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5b796-365">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="5b796-366">ASP.NET が (コードとマークアップの両方) のページを探し、すべて解決、ページの実行時に、`~`適切なパスへの参照。</span><span class="sxs-lookup"><span data-stu-id="5b796-366">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="5b796-367">条件付きロジックやループ</span><span class="sxs-lookup"><span data-stu-id="5b796-367">Conditional Logic and Loops</span></span>

<span data-ttu-id="5b796-368">ASP.NET サーバー コードにより、条件とループを実行する回数を特定のコードは、ステートメントを繰り返し実行する書き込みコードに基づいてタスクを実行する)。</span><span class="sxs-lookup"><span data-stu-id="5b796-368">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="5b796-369">条件のテスト</span><span class="sxs-lookup"><span data-stu-id="5b796-369">Testing conditions</span></span>

<span data-ttu-id="5b796-370">使用する単純な条件をテストする、`If...Then`ステートメントでは、返す`True`または`False`指定したテストに基づきます。</span><span class="sxs-lookup"><span data-stu-id="5b796-370">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="5b796-371">`If`キーワードは、ブロックを開始します。</span><span class="sxs-lookup"><span data-stu-id="5b796-371">The `If` keyword starts a block.</span></span> <span data-ttu-id="5b796-372">実際のテスト (条件) に依存して、`If`キーワードと true または false を返します。</span><span class="sxs-lookup"><span data-stu-id="5b796-372">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="5b796-373">`If`ステートメントが終わる`Then`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-373">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="5b796-374">テストが true の場合に実行するステートメントがで囲まれた`If`と`End If`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-374">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="5b796-375">`If`ステートメントを含めることができます、`Else`ブロックする条件が false の場合に実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="5b796-375">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="5b796-376">場合、`If`ステートメントがコード ブロックを開始、法線を使用する必要はありません`Code...End Code`要素を含めるようにステートメントをします。</span><span class="sxs-lookup"><span data-stu-id="5b796-376">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="5b796-377">追加するだけ`@`するブロックが機能します。</span><span class="sxs-lookup"><span data-stu-id="5b796-377">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="5b796-378">この方法を使用できます`If`他の Visual Basic のキーワードを含む、コード ブロックに続くのプログラミングと`For`、 `For Each`、`Do While`など。</span><span class="sxs-lookup"><span data-stu-id="5b796-378">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="5b796-379">1 つまたは複数を使用して複数の条件を追加する`ElseIf`ブロック。</span><span class="sxs-lookup"><span data-stu-id="5b796-379">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="5b796-380">最初の条件の場合、この例では、`If`ブロックが true でない、`ElseIf`条件がチェックされます。</span><span class="sxs-lookup"><span data-stu-id="5b796-380">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="5b796-381">その条件が満たされた場合、ステートメントで、`ElseIf`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-381">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="5b796-382">どの条件が満たされた場合、ステートメントで、`Else`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-382">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="5b796-383">任意の数を追加する`ElseIf`ブロック、およびを閉じて、`Else`としてブロック、&quot;他のすべて&quot;条件。</span><span class="sxs-lookup"><span data-stu-id="5b796-383">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="5b796-384">多くの条件をテストするには、使用、`Select Case`ブロック。</span><span class="sxs-lookup"><span data-stu-id="5b796-384">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="5b796-385">テストする値は、(例、weekday 変数) ではかっこ内に示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-385">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="5b796-386">各テストを使用して、`Case`値を一覧表示するステートメント。</span><span class="sxs-lookup"><span data-stu-id="5b796-386">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="5b796-387">場合の値を`Case`ステートメントには、そのコードのテスト値が一致すると`Case`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-387">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="5b796-388">ブラウザーで表示される最後の 2 つの条件付きブロックの結果:</span><span class="sxs-lookup"><span data-stu-id="5b796-388">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="5b796-390">ループのコード</span><span class="sxs-lookup"><span data-stu-id="5b796-390">Looping code</span></span>

<span data-ttu-id="5b796-391">多くの場合、同じステートメントを繰り返し実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-391">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="5b796-392">そのためには、ループ処理します。</span><span class="sxs-lookup"><span data-stu-id="5b796-392">You do this by looping.</span></span> <span data-ttu-id="5b796-393">たとえば、多くの場合、ステートメントを実行する同じ項目ごとにデータのコレクション。</span><span class="sxs-lookup"><span data-stu-id="5b796-393">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="5b796-394">使用することが正確に何回ループすることがわかっている場合、`For`ループします。</span><span class="sxs-lookup"><span data-stu-id="5b796-394">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="5b796-395">この種のループはまでカウントするか、カウント ダウン特に便利です。</span><span class="sxs-lookup"><span data-stu-id="5b796-395">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="5b796-396">ループが始まり、`For`キーワード、3 つの要素に続きます。</span><span class="sxs-lookup"><span data-stu-id="5b796-396">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="5b796-397">直後に、`For`ステートメントでは、カウンター変数を宣言する (を使用する必要はありません`Dim`) ように、範囲を示すと`i = 10 to 20`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-397">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="5b796-398">つまり、変数`i`10 からカウントが開始され、20 (両端を含む) に到達するまで続行します。</span><span class="sxs-lookup"><span data-stu-id="5b796-398">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="5b796-399">間、`For`と`Next`ステートメントは、ブロックのコンテンツ。</span><span class="sxs-lookup"><span data-stu-id="5b796-399">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="5b796-400">これは、1 つまたは複数のコード ステートメントを実行する各ループを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-400">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="5b796-401">`Next i`ステートメント、ループを終了します。</span><span class="sxs-lookup"><span data-stu-id="5b796-401">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="5b796-402">カウンターをインクリメントし、ループの次のイテレーションを開始します。</span><span class="sxs-lookup"><span data-stu-id="5b796-402">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="5b796-403">間のコード行、`For`と`Next`行には、ループの反復ごとに実行されるコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5b796-403">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="5b796-404">マークアップは、新しい段落を作成する (`<p>`要素) の各時間し、の値を表示する出力に行を追加します。 i (カウンター)。</span><span class="sxs-lookup"><span data-stu-id="5b796-404">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="5b796-405">このページを実行すると、例は、11 行の行のテキスト項目の数を示す行ごとに、出力の表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="5b796-405">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="5b796-407">多くの場合、使用、コレクションまたは配列で作業している場合、`For Each`ループします。</span><span class="sxs-lookup"><span data-stu-id="5b796-407">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="5b796-408">コレクションは、類似のオブジェクトのグループと`For Each`ループでコレクション内の各項目のタスクを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-408">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="5b796-409">この種のループはコレクションの場合、便利なためとは異なり、`For`ループ カウンターを増分または制限を設定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5b796-409">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="5b796-410">代わりに、`For Each`ループのコードだけを進め、コレクションが完了するまでです。</span><span class="sxs-lookup"><span data-stu-id="5b796-410">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="5b796-411">この例は、内の項目を返します、 `Request.ServerVariables` (コレクションを web サーバーに関する情報が含まれています)。</span><span class="sxs-lookup"><span data-stu-id="5b796-411">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="5b796-412">使用して、`For Each`新たに作成して各項目の名前を表示するループ`<li>`HTML の箇条書きリスト内の要素。</span><span class="sxs-lookup"><span data-stu-id="5b796-412">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="5b796-413">`For Each`キーワードに続けて、コレクション内の 1 つの項目を表す変数 (例では、 `myItem`)、その後に、`In`キーワードのコレクションをループするか後にします。</span><span class="sxs-lookup"><span data-stu-id="5b796-413">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="5b796-414">本体で、`For Each`ループ、先ほど宣言した変数を使用して現在の項目にアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-414">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="5b796-416">汎用的なループを作成するには、使用、`Do While`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="5b796-416">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="5b796-417">このループが始まり、`Do While`キーワード、後ろに、条件を繰り返すブロックが続きます。</span><span class="sxs-lookup"><span data-stu-id="5b796-417">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="5b796-418">ループの通常のインクリメント (に追加する) または減分 (減算)、変数またはオブジェクト カウントで使用されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-418">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="5b796-419">この例で、`+=`演算子に 1 を加算する変数の値、ループが実行されるたびにします。</span><span class="sxs-lookup"><span data-stu-id="5b796-419">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="5b796-420">(カウント ダウンをループ内の変数をデクリメントするは、デクリメント演算子を使用して`-=`)。</span><span class="sxs-lookup"><span data-stu-id="5b796-420">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="5b796-421">オブジェクトとコレクション</span><span class="sxs-lookup"><span data-stu-id="5b796-421">Objects and Collections</span></span>

<span data-ttu-id="5b796-422">ほぼすべての ASP.NET web サイトでは、web ページ自体を含むオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="5b796-422">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="5b796-423">このセクションでは、使用する多くの場合、コードでいくつかの重要なオブジェクトについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5b796-423">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="5b796-424">ページのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="5b796-424">Page objects</span></span>

<span data-ttu-id="5b796-425">ASP.NET の最も基本的なオブジェクトは、ページです。</span><span class="sxs-lookup"><span data-stu-id="5b796-425">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="5b796-426">オブジェクトを修飾せずに直接ページ オブジェクトのプロパティにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="5b796-426">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="5b796-427">次のコード ページのファイルのパスを取得するを使用して、`Request`ページのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5b796-427">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="5b796-428">プロパティを使用して、`Page`など多くの情報を取得するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5b796-428">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="5b796-429">`Request`。</span><span class="sxs-lookup"><span data-stu-id="5b796-429">`Request`.</span></span> <span data-ttu-id="5b796-430">既に説明したように行われる要求、ページ、ユーザー id などの URL をブラウザーの種類を含む、現在の要求に関する情報のコレクションになります。</span><span class="sxs-lookup"><span data-stu-id="5b796-430">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="5b796-431">`Response`。</span><span class="sxs-lookup"><span data-stu-id="5b796-431">`Response`.</span></span> <span data-ttu-id="5b796-432">これは、サーバー コードの実行が終了したら、ブラウザーに送信される応答 (ページ) に関する情報のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="5b796-432">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="5b796-433">たとえば、このプロパティを使用すると、応答に情報を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="5b796-433">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="5b796-434">コレクション オブジェクト (配列、ディクショナリ)</span><span class="sxs-lookup"><span data-stu-id="5b796-434">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="5b796-435">コレクションのコレクションなど、同じ型のオブジェクトのグループは、`Customer`データベースからのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5b796-435">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="5b796-436">このような ASP.NET に多くの組み込みコレクションが含まれています、`Request.Files`コレクション。</span><span class="sxs-lookup"><span data-stu-id="5b796-436">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="5b796-437">多くの場合、コレクション内のデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="5b796-437">You'll often work with data in collections.</span></span> <span data-ttu-id="5b796-438">2 つの一般的なコレクション型は、*配列*と*ディクショナリ*します。</span><span class="sxs-lookup"><span data-stu-id="5b796-438">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="5b796-439">配列は、類似した項目のコレクションを格納するが、それぞれ個別の各項目を保持する変数を作成したくない場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5b796-439">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="5b796-440">配列の使用、特定のデータ型を宣言するなど`String`、 `Integer`、または`DateTime`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-440">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="5b796-441">変数が配列を含めることができます、宣言で変数名にかっこを追加することを示すために (など`Dim myVar() As String`)。</span><span class="sxs-lookup"><span data-stu-id="5b796-441">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="5b796-442">またはを使用してそれらの位置 (インデックス) を使用して、配列内の項目にアクセスすることができます、`For Each`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="5b796-442">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="5b796-443">配列のインデックスは 0 から始まる&#8212;つまり最初の項目がある 0 の位置は、2 番目の項目は、位置 1、という具合にします。</span><span class="sxs-lookup"><span data-stu-id="5b796-443">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="5b796-444">取得することによって、配列内の項目の数を判断するその`Length`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5b796-444">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="5b796-445">配列内の特定の項目の位置を取得する (つまり、配列を検索します)、使用、`Array.IndexOf`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5b796-445">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="5b796-446">行うことができますも逆の順序など、配列の内容 (、`Array.Reverse`メソッド) または内容を並べ替える (、`Array.Sort`メソッド)。</span><span class="sxs-lookup"><span data-stu-id="5b796-446">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="5b796-447">ブラウザーで表示されている文字列配列コードの出力:</span><span class="sxs-lookup"><span data-stu-id="5b796-447">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="5b796-449">ディクショナリは設定または対応する値を取得するキー (または名前) を指定するキー/値のペアのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="5b796-449">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="5b796-450">使用するディクショナリを作成する、`New`を新規作成しているかを示すキーワード`Dictionary`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5b796-450">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="5b796-451">変数を使用するディクショナリを割り当てることができます、`Dim`キーワード。</span><span class="sxs-lookup"><span data-stu-id="5b796-451">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="5b796-452">かっこを使用して、ディクショナリ内の項目のデータ型を指定する ( `( )` )。</span><span class="sxs-lookup"><span data-stu-id="5b796-452">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="5b796-453">これは、実際には、新しいディクショナリを作成するメソッド宣言の末尾には、もう 1 つのかっこのペアを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-453">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="5b796-454">項目をディクショナリに追加するを呼び出すことができます、`Add`辞書の変数のメソッド (`myScores`ここでは)、キーと値を指定します。</span><span class="sxs-lookup"><span data-stu-id="5b796-454">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="5b796-455">または、キーを示すし、次の例のように、単純な割り当てを行うにはかっこを使用できます。</span><span class="sxs-lookup"><span data-stu-id="5b796-455">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="5b796-456">をディクショナリから値を取得するには、かっこで囲まれたキーを指定します。</span><span class="sxs-lookup"><span data-stu-id="5b796-456">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="5b796-457">パラメーターを持つメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="5b796-457">Calling Methods with Parameters</span></span>

<span data-ttu-id="5b796-458">この記事の前半で説明するようにプログラミングでのオブジェクトはメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="5b796-458">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="5b796-459">たとえば、`Database`オブジェクトがあります、`Database.Connect`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5b796-459">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="5b796-460">多くのメソッドは、1 つまたは複数のパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="5b796-460">Many methods also have one or more parameters.</span></span> <span data-ttu-id="5b796-461">A*パラメーター*メソッドに渡す値は、タスクが完了する方法を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5b796-461">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="5b796-462">たとえばの宣言を見て、`Request.MapPath`メソッドは、次の 3 つのパラメーターを受け取る。</span><span class="sxs-lookup"><span data-stu-id="5b796-462">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="5b796-463">このメソッドは、指定した仮想パスに対応するサーバー上の物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="5b796-463">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="5b796-464">メソッドの 3 つのパラメーターは`virtualPath`、 `baseVirtualDir`、および`allowCrossAppMapping`します。</span><span class="sxs-lookup"><span data-stu-id="5b796-464">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="5b796-465">(宣言で、パラメーターは許可されるデータのデータ型の一覧に注目してください)。このメソッドを呼び出すときに、次の 3 つのすべてのパラメーターの値を指定してください。</span><span class="sxs-lookup"><span data-stu-id="5b796-465">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="5b796-466">Visual Basic と Razor 構文を使用しているときにパラメーターをメソッドに渡す 2 つのオプションがある:*位置指定パラメーター*または*名前付きパラメーター*します。</span><span class="sxs-lookup"><span data-stu-id="5b796-466">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="5b796-467">位置指定パラメーターを使用してメソッドを呼び出すには、メソッドの宣言で指定されている厳密な順序でパラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="5b796-467">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="5b796-468">(が通常わかるこの順序メソッドのドキュメントを参照しています。)順序に従い、任意のパラメーターをスキップすることはできません&#8212;かどうか、必要に応じて空の文字列を渡す (`""`) の値がない位置指定パラメーターの場合は null です。</span><span class="sxs-lookup"><span data-stu-id="5b796-468">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="5b796-469">次の例は、という名前のフォルダーにあることを前提*スクリプト*web サイトにします。</span><span class="sxs-lookup"><span data-stu-id="5b796-469">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="5b796-470">コードの呼び出し、`Request.MapPath`を正しい順序で 3 つのパラメーターの値をメソッドを渡します。</span><span class="sxs-lookup"><span data-stu-id="5b796-470">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="5b796-471">結果のマップされたパスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-471">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="5b796-472">メソッドの多くのパラメーターが存在する場合、するおくと、コードより読みやすい名前付きパラメーターを使用しています。</span><span class="sxs-lookup"><span data-stu-id="5b796-472">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="5b796-473">名前付きパラメーターを使用してメソッドを呼び出すには、指定のパラメーター名の後に`:=`し、値を指定します。</span><span class="sxs-lookup"><span data-stu-id="5b796-473">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="5b796-474">名前付きパラメーターの利点は、任意の順序で追加できることです。</span><span class="sxs-lookup"><span data-stu-id="5b796-474">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="5b796-475">(欠点は、メソッドの呼び出しが、compact ではないこと) です。</span><span class="sxs-lookup"><span data-stu-id="5b796-475">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="5b796-476">次の例は、上記と同じメソッドを呼び出しますが、名前付きの値を指定するパラメーターの使用。</span><span class="sxs-lookup"><span data-stu-id="5b796-476">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="5b796-477">ご覧のように、パラメーターは異なる順序で渡されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-477">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="5b796-478">ただし、前の例と、この例を実行する場合、同じ値を返すします。</span><span class="sxs-lookup"><span data-stu-id="5b796-478">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="5b796-479">エラー処理</span><span class="sxs-lookup"><span data-stu-id="5b796-479">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="5b796-480">Try Catch ステートメント</span><span class="sxs-lookup"><span data-stu-id="5b796-480">Try-Catch statements</span></span>

<span data-ttu-id="5b796-481">上の理由から、コントロールの外部に障害が発生する、コード内のステートメントを多くの場合、があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-481">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="5b796-482">例えば:</span><span class="sxs-lookup"><span data-stu-id="5b796-482">For example:</span></span>

- <span data-ttu-id="5b796-483">コードは、開く、作成、読み取り、またはファイルの書き込みを試みると、あらゆる種類のエラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-483">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="5b796-484">目的のファイルが存在しない可能性があります、ロックアウトされることがあります、コードが、権限がありませんして具合可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-484">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="5b796-485">同様に場合は、コードでは、データベース内のレコードを更新しようとして、アクセス許可の問題があります、データベースへの接続が削除される可能性が、無効なおよびこれを保存するデータがあります。</span><span class="sxs-lookup"><span data-stu-id="5b796-485">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="5b796-486">プログラミング用語では、このような状況が呼び出されます*例外*します。</span><span class="sxs-lookup"><span data-stu-id="5b796-486">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="5b796-487">(スロー) が生成されますが、コードには、例外が発生すると、エラー メッセージは、最高でユーザーに面倒です。</span><span class="sxs-lookup"><span data-stu-id="5b796-487">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="5b796-489">コードが例外を発生可能性がある場合に、この種類のエラー メッセージを回避するためには、使用する`Try/Catch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="5b796-489">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="5b796-490">`Try`ステートメントでは、チェックするコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="5b796-490">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="5b796-491">1 つまたは複数`Catch`ステートメントでは、特定の検索できます (特定の種類の例外の) エラーの発生した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-491">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="5b796-492">多くとして含めることができます`Catch`とステートメントは、予測しているエラーを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b796-492">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="5b796-493">使用を避けることをお勧めします。、`Response.Redirect`メソッド`Try/Catch`ステートメント、ページで、例外が生じるためです。</span><span class="sxs-lookup"><span data-stu-id="5b796-493">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="5b796-494">次の例では、最初の要求でテキスト ファイルを作成し、ファイルを開くユーザーを切り替えるボタンを表示するページを示します。</span><span class="sxs-lookup"><span data-stu-id="5b796-494">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="5b796-495">例は、例外が発生されるように意図的に不適切なファイル名を使用します。</span><span class="sxs-lookup"><span data-stu-id="5b796-495">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="5b796-496">コードが含まれています`Catch`可能性のある例外の 2 つのステートメント: `FileNotFoundException`、ファイル名が間違っている場合に発生して`DirectoryNotFoundException`ASP.NET は、フォルダーにも見つからない場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="5b796-496">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="5b796-497">(コメントを解除できます、ステートメントの例では、すべてが適切に動作しているときの動作を確認するためにします。)</span><span class="sxs-lookup"><span data-stu-id="5b796-497">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="5b796-498">コードが例外を処理していない場合は、前のスクリーン ショットのようなエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5b796-498">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="5b796-499">ただし、`Try/Catch`セクションでは、ユーザーがこの種のエラーが表示されることを防ぐのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5b796-499">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="5b796-500">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5b796-500">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="5b796-501">リファレンス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="5b796-501">Reference Documentation</span></span>

- [<span data-ttu-id="5b796-502">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5b796-502">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="5b796-503">Visual Basic 言語</span><span class="sxs-lookup"><span data-stu-id="5b796-503">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
