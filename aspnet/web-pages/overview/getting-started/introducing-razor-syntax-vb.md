---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要 |Microsoft ドキュメント
author: tfitzmac
description: この付録概要を説明する ASP.NET Web pages でのプログラミングの Visual basic で、Razor 構文を使用します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: aad951a0e4344dbaafbdcc3b3980307a26fa75fc
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483485"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="3a5b3-103">Razor 構文 (Visual Basic) を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="3a5b3-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="3a5b3-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3a5b3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3a5b3-105">ここで、プログラミングの概要 ASP.NET Web Pages を Razor 構文と Visual Basic を使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="3a5b3-106">ASP.NET は、web サーバーでの動的な web ページを実行するための Microsoft のテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="3a5b3-107">**学習する内容**:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="3a5b3-108">プログラミングの Razor 構文を使用して ASP.NET Web Pages のプログラミングの概要に関するヒントの最上位 8 です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="3a5b3-109">基本的なプログラミング概念する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="3a5b3-110">どのような ASP.NET サーバー コードと Razor 構文に関するです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="3a5b3-111">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="3a5b3-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="3a5b3-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3a5b3-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3a5b3-113">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="3a5b3-114">Razor 構文を使用する ASP.NET Web Pages を使用するほとんどの例は c# を使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="3a5b3-115">Razor 構文では、Visual Basic もサポートされています。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="3a5b3-116">Visual Basic での ASP.NET web ページをプログラムで web ページを作成する、 *.vbhtml*ファイル名拡張子、し、Visual Basic コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="3a5b3-117">この記事では、Visual Basic 言語と ASP.NET web ページを作成するための構文での作業の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="3a5b3-118">Microsoft WebMatrix の既定の web サイト テンプレート (**ベーカリー**、**フォト ギャラリー**、および**スターター サイト**など) は c# および Visual Basic のバージョンで使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="3a5b3-119">NuGet パッケージとしてでは、Visual Basic テンプレートをインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="3a5b3-120">Web サイト テンプレートがという名前のフォルダーで、サイトのルート フォルダーにインストールされている*Microsoft テンプレート*です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="3a5b3-121">最上位の 8 プログラミングのヒント</span><span class="sxs-lookup"><span data-stu-id="3a5b3-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="3a5b3-122">このセクションでは、絶対に必要な Razor 構文を使用して ASP.NET サーバー コードの記述を開始すると、いくつかのヒントを示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="3a5b3-123">1.コードを使用してページを追加する、@ 文字</span><span class="sxs-lookup"><span data-stu-id="3a5b3-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="3a5b3-124">`@`文字は、インライン式、単一ステートメント ブロック、および複数ステートメントのブロックを開始します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="3a5b3-125">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="3a5b3-127">**HTML エンコード**</span><span class="sxs-lookup"><span data-stu-id="3a5b3-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="3a5b3-128">ページを使用して、コンテンツを表示した場合、`@`文字、上記の例では、ASP.NET は、出力を HTML でエンコードします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="3a5b3-129">これには、HTML の予約された文字が置き換えられます (など`<`と`>`と`&`) コードを HTML タグまたはエンティティとして解釈されるのではなく、web ページ内の文字として表示される文字を有効にするとします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="3a5b3-130">HTML エンコードせず、サーバー コードからの出力は正しく表示されない場合があり、セキュリティ上のリスクにページを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="3a5b3-131">目標は、マークアップとしてタグがレンダリングされる HTML マークアップを出力するかどうか (たとえば`<p></p>`段落のまたは`<em></em>`テキストを強調する) を参照してください[結合のテキスト、マークアップ、およびコード ブロック内のコード](#BM_CombiningTextMarkupAndCode)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="3a5b3-132">詳細での HTML エンコードに関する[HTML フォームの ASP.NET Web Pages サイトで使用](https://go.microsoft.com/fwlink/?LinkId=202892)です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="3a5b3-133">2.コードを使って、コード ブロックで囲むとしています.終了コード</span><span class="sxs-lookup"><span data-stu-id="3a5b3-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="3a5b3-134">コード ブロックが 1 つまたは複数のコード ステートメントが含まれ、キーワードで囲まれた`Code`と`End Code`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="3a5b3-135">配置開始`Code`キーワード後すぐに、`@`文字&#8212;間それらを空白にすることはできませんがあります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="3a5b3-136">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="3a5b3-138">3.ブロック内で改行するのには、各コード ステートメントを終了します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="3a5b3-139">Visual Basic のコード ブロックには、各ステートメントは、改行で終わります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="3a5b3-140">(記事の後半で必要な場合は、複数の行に長いコード ステートメントをラップする方法を参照します)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="3a5b3-141">4.値を格納するのに変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-141">4. You use variables to store values</span></span>

<span data-ttu-id="3a5b3-142">内の値を格納することができます、*変数*文字列、数字、および日付などを含め、します。新しい変数を使用して、作成する、`Dim`キーワード。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="3a5b3-143">変数の値を挿入するにはページを使用して、直接`@`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="3a5b3-144">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="3a5b3-146">5.リテラル文字列の値を二重引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="3a5b3-147">A*文字列*テキストとして扱われる文字のシーケンスです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="3a5b3-148">文字列を指定するには、二重引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="3a5b3-149">文字列値の中では二重引用符を埋め込むには、次の 2 つの二重引用符文字を挿入します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="3a5b3-150">ページ出力に 1 回出現する二重引用符文字を挿入する場合は、入力として`""`内で、引用符で囲まれた文字列、および 2 回表示する場合は入力として`""""`引用符で囲まれた文字列内で。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="3a5b3-151">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="3a5b3-153">6.Visual Basic のコードは、大文字小文字を区別はありません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="3a5b3-154">Visual Basic 言語では、大文字小文字を区別されません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="3a5b3-155">プログラミング キーワード (のように`Dim`、 `If`、および`True`) と変数名 (など`myString`、または`subTotal`) いずれの場合も記述できます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="3a5b3-156">次のコード行は、変数に値を割り当てる`lastname`小文字を使用して名前、および大文字の名前を使用して変数の値を出力します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="3a5b3-157">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="3a5b3-159">7.コーディングのほとんどには、オブジェクトによる操作が含まれます</span><span class="sxs-lookup"><span data-stu-id="3a5b3-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="3a5b3-160">オブジェクトをプログラミングで使用できることを表す&#8212;ページ、テキスト ボックス、ファイル、画像、web 要求、電子メール メッセージ、顧客レコード (データベースの行) などです。オブジェクトの特性を記述するプロパティがある&#8212;テキスト ボックスのオブジェクトが、`Text`プロパティ、要求オブジェクトが、`Url`プロパティ、電子メール メッセージには、`From`プロパティ、および顧客オブジェクトが、 `FirstName`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="3a5b3-161">オブジェクトでは、実行されるメソッドもがある、&quot;動詞&quot;それらを実行できます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="3a5b3-162">例としては、ファイル オブジェクトの`Save`メソッドは、イメージ オブジェクトの`Rotate`メソッド、および電子メール オブジェクトの`Send`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="3a5b3-163">使って作業を多くの場合、`Request`ブラウザーの種類の要求を作成、ページや、ユーザー id などの URL (テキスト ボックスなど)、ページ上のフォームの値などの情報を提供するオブジェクトのフィールドです。この例のプロパティにアクセスする方法を示しています、`Request`オブジェクトおよび呼び出す方法、`MapPath`のメソッド、`Request`オブジェクトで、サーバー上のページの絶対パスを表示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="3a5b3-164">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="3a5b3-166">8.意思決定を行うコードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="3a5b3-167">動的な web ページの主な機能は、条件に基づき、新機能を決定できます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="3a5b3-168">これを行う最も一般的な方法は、`If`ステートメント (と省略可能な`Else`ステートメント)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="3a5b3-169">ステートメント`If IsPost`書き込みのためのショートハンド方法`If IsPost = True`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="3a5b3-170">と共に`If`ステートメントはさまざまな方法は、コードのブロックを繰り返し、条件をテストするために、か、この記事の後半で説明されています。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="3a5b3-171">結果がブラウザーで表示されます (クリックした後**送信**)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="3a5b3-173">**HTTP GET と POST メソッドおよび IsPost プロパティ**</span><span class="sxs-lookup"><span data-stu-id="3a5b3-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="3a5b3-174">Web ページ (HTTP) に使用されるプロトコルは、メソッドの非常に限られた数をサポートしています (&quot;動詞&quot;) に、サーバー要求に使用されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="3a5b3-175">2 つの最も一般的なは、ページの読み取りに使用される、GET と POST のページを送信するために使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="3a5b3-176">一般に、ユーザーの要求 ページでは、最初に、ページが要求された GET を使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="3a5b3-177">かどうか、ユーザーは、フォームに入力して、クリックして**送信**ブラウザー、サーバーへの POST 要求。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="3a5b3-178">Web プログラミング、かどうか、ページが要求されて、GET または POST として、ページの処理方法がわかるように、知っておくと役立ちますがしばしばあります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="3a5b3-179">ASP.NET Web Pages で行うこともできます、`IsPost`プロパティを要求が GET または POST があるかどうかを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="3a5b3-180">要求が、投稿の場合、`IsPost`プロパティが true の場合が返され、フォーム上のテキスト ボックスの値読み取りなどを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="3a5b3-181">多くの例を確認する方法を示しますの値に応じて異なる方法でページを処理する`IsPost`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="3a5b3-182">単純なコード例</span><span class="sxs-lookup"><span data-stu-id="3a5b3-182">A Simple Code Example</span></span>

<span data-ttu-id="3a5b3-183">この手順では、基本的なプログラミング技術について説明するページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="3a5b3-184">例では、それらを追加し、結果を表示し、2 つの数値を入力できるように、ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="3a5b3-185">エディターで、新しいファイルを作成し、名前*AddNumbers.vbhtml*です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="3a5b3-186">ページで、ページ内の既存のものを置き換えるには、次のコードとマークアップをコピーします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="3a5b3-187">ここでは、いくつかの点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="3a5b3-188">`@`文字 ページで、コードの最初のブロックを開始して、前に指定する、`totalMessage`下部近くにある変数が埋め込まれています。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="3a5b3-189">ページの上部にあるブロックを囲む`Code...End Code`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="3a5b3-190">変数`total`、 `num1`、 `num2`、および`totalMessage`いくつかの数字と文字列を格納します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="3a5b3-191">割り当てられているリテラル文字列値、`totalMessage`変数は、二重引用符内です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="3a5b3-192">Visual Basic コードにないため、大文字と小文字が区別すると、`totalMessage`ページの下部にある変数を使用、その名前は、ページの上部にある変数の宣言のスペルが一致するようにのみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="3a5b3-193">大文字と小文字は関係ありません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="3a5b3-194">式`num1.AsInt()`  +  `num2.AsInt()`オブジェクトおよびメソッドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="3a5b3-195">`AsInt`各変数のメソッドを追加できる整数 (整数)、ユーザーが入力した文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="3a5b3-196">`<form>`タグが含まれています、`method="post"`属性。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="3a5b3-197">これを指定する、ユーザーがクリックしたときに**追加**ページは、HTTP POST メソッドを使用してサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="3a5b3-198">ページを送信するときに、コード`If IsPost`true に評価され、条件付きコードの実行、数値を加算した結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="3a5b3-199">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="3a5b3-200">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。2 つの整数を入力し、クリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="3a5b3-202">Visual Basic 言語と構文</span><span class="sxs-lookup"><span data-stu-id="3a5b3-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="3a5b3-203">ASP.NET web ページを作成する方法とサーバー コードを HTML マークアップを追加する方法の基本的な例は前述です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="3a5b3-204">ここで Visual Basic を使用して、Razor 構文を使用して ASP.NET サーバー コードを記述の基本を学習&#8212;プログラミング言語の規則では、します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="3a5b3-205">(特に C、C++、c#、Visual Basic、または JavaScript を使用した) 場合のプログラミングを使用した経験が場合、次を参照する新機能の多くについて理解されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="3a5b3-206">習熟するのみでのマークアップに WebMatrix のコードを追加する方法が必要 *.vbhtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="3a5b3-207">テキスト、マークアップ、およびコード ブロック内のコードを組み合わせること</span><span class="sxs-lookup"><span data-stu-id="3a5b3-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="3a5b3-208">サーバー コード ブロックでは、多くの場合にしておくテキスト、およびページ マークアップを出力します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="3a5b3-209">サーバー コード ブロックのコードではないとする代わりに表示するかは、テキストが含まれている場合、ASP.NET をコードからそのテキストを識別できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="3a5b3-210">これにはいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-210">There are several ways to do this.</span></span>

- <span data-ttu-id="3a5b3-211">ような HTML ブロック要素内のテキストを囲みます`<p></p>`または`<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="3a5b3-212">HTML 要素には、テキスト、HTML 要素の追加、およびサーバー コード式を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="3a5b3-213">ASP.NET で開始 HTML タグがどのように表示される場合 (たとえば、 `<p>`)、要素をレンダリングすべてと、ブラウザー (および、サーバー コード式を解決) には、その内容として、します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="3a5b3-214">使用して、`@:`演算子または`<text>`要素。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="3a5b3-215">`@:`プレーン テキストまたは一致しない HTML タグを含むコンテンツの 1 つの行が出力、`<text>`要素を出力する複数の行で囲みます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="3a5b3-216">これらのオプションは、出力の一部としての HTML 要素を表示したくない場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="3a5b3-217">次の例は、前の例を繰り返しますの 1 つのペアを使用して`<text>`タグをレンダリングするテキストを囲みます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="3a5b3-218">次の例で、`<text>`と`</text>`タグがいくつか含まれていないテキストと一致しない HTML タグがあるすべての 3 つの行を囲みます (`<br />`)、およびサーバー コードと一致した HTML タグ。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="3a5b3-219">もう一度、ごとに 1 行の前でしたも、`@:`演算子以外のいずれかの方法の動作です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="3a5b3-220">このセクションで示すようにテキストを出力するときに&#8212;、HTML 要素を使用して、`@:`演算子、または`<text>`要素&#8212;ASP.NET は HTML エンコード出力します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="3a5b3-221">(ASP.NET はサーバーのコード式と末尾に挿入されるサーバー コード ブロックの出力はエンコード前に述べたよう`@`、このセクションで説明した特殊なケースでは可します)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="3a5b3-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="3a5b3-222">Whitespace</span></span>

<span data-ttu-id="3a5b3-223">ステートメントで (および、文字列リテラルの外部) 余分なスペースは、ステートメントに影響しません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="3a5b3-224">長いステートメントを複数の行に分割</span><span class="sxs-lookup"><span data-stu-id="3a5b3-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="3a5b3-225">アンダー スコア文字を使用して、複数の行に長いコード ステートメントを中断できます`_`(Visual Basic では、呼び出される、*連結文字*) コードの各行の後にします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="3a5b3-226">次の行にステートメントを中断するには、行の末尾にスペースと連結文字を追加します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="3a5b3-227">次の行に、ステートメントを続行します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-227">Continue the statement on the next line.</span></span> <span data-ttu-id="3a5b3-228">読みやすさを向上させるために必要な数の行にステートメントをラップすることができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="3a5b3-229">次のステートメントでは、同じです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="3a5b3-230">ただし、文字列リテラルの途中で行をラップすることはできません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="3a5b3-231">次の例が機能しません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="3a5b3-232">上記のコードのように複数の行に折り返す長い文字列を結合するには、まず必要がありますを使用する、*連結演算子*(`&`)、この記事で後述があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="3a5b3-233">コードのコメント</span><span class="sxs-lookup"><span data-stu-id="3a5b3-233">Code comments</span></span>

<span data-ttu-id="3a5b3-234">コメントでは、自分自身または他のユーザーのノートのままにできます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="3a5b3-235">Razor 構文のコメントの先頭が付いている`@*`で終わります`*@`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="3a5b3-236">コード ブロック内でコメントを使用して、Razor 構文、または単一引用符は、通常の Visual Basic のコメント文字を使用することができます (`'`) の先頭行ごとに指定します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="3a5b3-237">変数</span><span class="sxs-lookup"><span data-stu-id="3a5b3-237">Variables</span></span>

<span data-ttu-id="3a5b3-238">変数は、データの格納に使用する名前付きオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="3a5b3-239">名前を付ける変数、何もが、名前の先頭の文字は英字にする必要があり、空白文字または予約文字は使用できません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="3a5b3-240">Visual Basic では、前に説明したとおり、変数名の文字の大文字と小文字は関係ありません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="3a5b3-241">変数とデータ型</span><span class="sxs-lookup"><span data-stu-id="3a5b3-241">Variables and data types</span></span>

<span data-ttu-id="3a5b3-242">変数は、データの種類が変数に格納されていることを示しますが、特定のデータ型を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="3a5b3-243">文字列値を格納する文字列変数があることができます (のような&quot;Hello world&quot;)、(3 または 79) などの整数値を格納する整数変数とさまざまな (4/12/2012 や 2009 年 3 月などの形式で日付値を格納する date 型の変数).</span><span class="sxs-lookup"><span data-stu-id="3a5b3-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="3a5b3-244">いて、その他の多くのデータ型を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="3a5b3-245">ただし、変数の型を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="3a5b3-246">ほとんどの場合 ASP.NET は、変数内のデータの使用方法に基づいて型を見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="3a5b3-247">(場合によっては、型を指定する必要があります。 これが true の例が表示されます。)</span><span class="sxs-lookup"><span data-stu-id="3a5b3-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="3a5b3-248">使用して型を指定せず、変数を宣言する`Dim`変数名を加えた (たとえば、 `Dim myVar`)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="3a5b3-249">型の変数を宣言するには使用`Dim`続けて、変数名を加えた`As`と型名では、(たとえば、 `Dim myVar As String`)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="3a5b3-250">次の例では、web ページの変数を使用するインライン式を示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="3a5b3-251">ブラウザーに表示される結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="3a5b3-253">変換して、データ型のテスト</span><span class="sxs-lookup"><span data-stu-id="3a5b3-253">Converting and testing data types</span></span>

<span data-ttu-id="3a5b3-254">ASP.NET はこのデータ型を自動的に決定できますは通常があります、できません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="3a5b3-255">そのため、明示的な変換を実行して ASP.NET を手助けする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="3a5b3-256">型変換を持っていない場合でもいる場合もありますデータの種類が使用することを確認するためのテストに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="3a5b3-257">最も一般的なケースでは、整数や日付など、他の型を文字列に変換していることです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="3a5b3-258">次の例は、文字列を数値に変換する必要があります、一般的な事例を示しています。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="3a5b3-259">原則として、ユーザー入力に文字列として。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="3a5b3-260">ユーザーに、数値の入力を要求した場合でも、ユーザー入力が送信されたコードで読み取るときに、数字を入力したした場合でも、データは文字列の形式です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="3a5b3-261">そのため、文字列を数値に変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="3a5b3-262">例では、したり、変換せず、値に対して算術演算を実行しようとする場合、次のエラーにより、ASP.NET は、2 つの文字列を追加できないため。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="3a5b3-263">呼び出すの値を整数に変換する、`AsInt`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="3a5b3-264">変換が成功した場合は、数値を追加できます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="3a5b3-265">次の表は、変数の一般的ないくつかの変換とテスト方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-265">The following table lists some common conversion and test methods for variables.</span></span>


<span data-ttu-id="3a5b3-266">0x%2 行 0x%2 0x%2 列 0x%2<strong>メソッド</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>説明</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>例</strong>0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-266">:::row::: :::column::: <strong>Method</strong> :::column-end::: :::column::: <strong>Description</strong> :::column-end::: :::column::: <strong>Example</strong> :::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-267">0x%2 行 0x%2 0x%2 列 0x%2 `AsInt(), IsInt()` 0x%2 列終了 0x%2 0x%2 列 0x%2 数を示す整数を表す文字列に変換します (など&quot;593&quot;) 整数にします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-267">:::row::: :::column::: `AsInt(), IsInt()` :::column-end::: :::column::: Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
<span data-ttu-id="3a5b3-268">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-268">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]</span></span>
    <span data-ttu-id="3a5b3-269">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-269">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-270">0x%2 行 0x%2 0x%2 列 0x%2 `AsBool(), IsBool()` 0x%2 列終了 0x%2 0x%2 列 0x%2 のように文字列に変換します&quot;true&quot;または&quot;false&quot; Boolean 型にします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-270">:::row::: :::column::: `AsBool(), IsBool()` :::column-end::: :::column::: Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
<span data-ttu-id="3a5b3-271">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-271">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]</span></span>
    <span data-ttu-id="3a5b3-272">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-272">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-273">0x%2 行 0x%2 0x%2 列 0x%2 `AsFloat(), IsFloat()` 0x%2 列終了 0x%2 0x%2 列 0x%2 のような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を浮動小数点数。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-273">:::row::: :::column::: `AsFloat(), IsFloat()` :::column-end::: :::column::: Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
<span data-ttu-id="3a5b3-274">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-274">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]</span></span>
    <span data-ttu-id="3a5b3-275">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-275">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-276">0x%2 行 0x%2 0x%2 列 0x%2 `AsDecimal(), IsDecimal()` 0x%2 列終了 0x%2 0x%2 列 0x%2 のような 10 進数の値を持つ文字列に変換します&quot;1.3&quot;または&quot;7.439&quot;を 10 進数。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-276">:::row::: :::column::: `AsDecimal(), IsDecimal()` :::column-end::: :::column::: Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="3a5b3-277">(ASP.NET では、10 進数は、浮動小数点数よりも正確です。)0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-277">(In ASP.NET, a decimal number is more precise than a floating-point number.) :::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]</span></span>
    <span data-ttu-id="3a5b3-278">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-278">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-279">0x%2 行 0x%2 0x%2 列 0x%2 `AsDateTime(), IsDateTime()` 0x%2 列終了 0x%2 0x%2 列 0x%2 を ASP.NET に日付と時刻の値を表す文字列を変換`DateTime`型です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-279">:::row::: :::column::: `AsDateTime(), IsDateTime()` :::column-end::: :::column::: Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
<span data-ttu-id="3a5b3-280">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-280">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]</span></span>
    <span data-ttu-id="3a5b3-281">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-281">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-282">0x%2 行 0x%2 0x%2 列 0x%2 `ToString()` 0x%2 列終了 0x%2 0x%2 列 0x%2 の他の任意のデータ型を文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-282">:::row::: :::column::: `ToString()` :::column-end::: :::column::: Converts any other data type to a string.</span></span>
<span data-ttu-id="3a5b3-283">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-283">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]</span></span>
    <span data-ttu-id="3a5b3-284">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-284">:::column-end::: :::row-end:::</span></span>


## <a name="operators"></a><span data-ttu-id="3a5b3-285">演算子</span><span class="sxs-lookup"><span data-stu-id="3a5b3-285">Operators</span></span>

<span data-ttu-id="3a5b3-286">演算子は、キーワードまたは ASP.NET を式の中で実行するコマンドの種類を示す文字です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-286">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="3a5b3-287">Visual Basic は、多くの演算子をサポートしていますが、ASP.NET web pages の開発を開始するいくつかを認識するだけです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-287">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="3a5b3-288">次の表は、最も一般的な演算子をまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-288">The following table summarizes the most common operators.</span></span>


<span data-ttu-id="3a5b3-289">0x%2 行 0x%2 0x%2 列 0x%2<strong>演算子</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>説明</strong>0x%2 列終了 0x%2 0x%2 列 0x%2<strong>例</strong>0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-289">:::row::: :::column::: <strong>Operator</strong> :::column-end::: :::column::: <strong>Description</strong> :::column-end::: :::column::: <strong>Examples</strong> :::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-290">0x%2 行 0x%2 0x%2 列 0x%2 `+ - * /` 0x%2 列終了 0x%2 0x%2 列 0x%2 算術演算子が数値式で使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-290">:::row::: :::column::: `+ - * /` :::column-end::: :::column::: Math operators used in numerical expressions.</span></span>
<span data-ttu-id="3a5b3-291">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-291">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]</span></span>
    <span data-ttu-id="3a5b3-292">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-292">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-293">0x%2 行 0x%2 0x%2 列 0x%2 `=` 0x%2 列終了 0x%2 0x%2 列 0x%2 の割り当てと等しいかどうか。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-293">:::row::: :::column::: `=` :::column-end::: :::column::: Assignment and equality.</span></span> <span data-ttu-id="3a5b3-294">コンテキストに応じて、左側にあるオブジェクトにステートメントの右側にある値を割り当てますか、等しいかどうかの値をチェックします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-294">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
<span data-ttu-id="3a5b3-295">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-295">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]</span></span>
    <span data-ttu-id="3a5b3-296">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-296">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-297">0x%2 行 0x%2 0x%2 列 0x%2 `<>` 0x%2 列終了 0x%2 0x%2 列 0x%2 非等値。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-297">:::row::: :::column::: `<>` :::column-end::: :::column::: Inequality.</span></span> <span data-ttu-id="3a5b3-298">返します`True`値が等しくない場合。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-298">Returns `True` if the values are not equal.</span></span>
<span data-ttu-id="3a5b3-299">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-299">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]</span></span>
    <span data-ttu-id="3a5b3-300">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-300">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-301">0x%2 行 0x%2 0x%2 列 0x%2 `< > <= >=` 0x%2 列終了 0x%2 0x%2 列 0x%2 よりも小さい、大きい、小さいよりまたは等しい、およびより大きいまたは等しい。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-301">:::row::: :::column::: `< > <= >=` :::column-end::: :::column::: Less than, greater than, less than or equal, and greater than or equal.</span></span>
<span data-ttu-id="3a5b3-302">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-302">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]</span></span>
    <span data-ttu-id="3a5b3-303">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-303">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-304">0x%2 行 0x%2 0x%2 列 0x%2 `&` 0x%2 列終了 0x%2 0x%2 列 0x%2 連結、文字列を結合するために使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-304">:::row::: :::column::: `&` :::column-end::: :::column::: Concatenation, which is used to join strings.</span></span>
<span data-ttu-id="3a5b3-305">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-305">:::column-end::: :::column::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]</span></span>
    <span data-ttu-id="3a5b3-306">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-306">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-307">0x%2 行 0x%2 0x%2 列 0x%2 `+= -=` 0x%2 列終了 0x%2 0x%2 列 0x%2、インクリメントとデクリメント演算子を追加し、変数から 1 をそれぞれ減算します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-307">:::row::: :::column::: `+= -=` :::column-end::: :::column::: The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
<span data-ttu-id="3a5b3-308">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-308">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]</span></span>
    <span data-ttu-id="3a5b3-309">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-309">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-310">0x%2 行 0x%2 0x%2 列 0x%2 `.` 0x%2 列終了 0x%2 0x%2 列 0x%2 ドットします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-310">:::row::: :::column::: `.` :::column-end::: :::column::: Dot.</span></span> <span data-ttu-id="3a5b3-311">オブジェクトとそのプロパティおよびメソッドを区別するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-311">Used to distinguish objects and their properties and methods.</span></span>
<span data-ttu-id="3a5b3-312">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-312">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]</span></span>
    <span data-ttu-id="3a5b3-313">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-313">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-314">0x%2 行 0x%2 0x%2 列 0x%2 `()` 0x%2 列終了 0x%2 0x%2 列 0x%2 かっこです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-314">:::row::: :::column::: `()` :::column-end::: :::column::: Parentheses.</span></span> <span data-ttu-id="3a5b3-315">、グループ式にパラメーターを渡すメソッド、および配列およびコレクションのメンバーにアクセスするために使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-315">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
<span data-ttu-id="3a5b3-316">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-316">:::column-end::: :::column::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]</span></span>
    <span data-ttu-id="3a5b3-317">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-317">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-318">0x%2 行 0x%2 0x%2 列 0x%2 `Not` 0x%2 列終了 0x%2 0x%2 列 0x%2 されません。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-318">:::row::: :::column::: `Not` :::column-end::: :::column::: Not.</span></span> <span data-ttu-id="3a5b3-319">False またはその逆は真の値を反転させます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-319">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="3a5b3-320">通常、テストするための短縮形方法として使用`False`(用には、いない`True`)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-320">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
<span data-ttu-id="3a5b3-321">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-321">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]</span></span>
    <span data-ttu-id="3a5b3-322">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-322">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="3a5b3-323">0x%2 行 0x%2 0x%2 列 0x%2 `AndAlso OrElse` 0x%2 列終了 0x%2 0x%2 列 0x%2 論理的かつ条件をまとめてリンクを使用すると、します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-323">:::row::: :::column::: `AndAlso OrElse` :::column-end::: :::column::: Logical AND and OR, which are used to link conditions together.</span></span>
<span data-ttu-id="3a5b3-324">0x%2 列終了 0x%2 0x%2 列 0x%2 [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-324">:::column-end::: :::column::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]</span></span>
    <span data-ttu-id="3a5b3-325">0x%2 列終了 0x%2 0x%2 行の終わり 0x%2</span><span class="sxs-lookup"><span data-stu-id="3a5b3-325">:::column-end::: :::row-end:::</span></span>

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="3a5b3-326">ファイルとフォルダーのパス コードでの操作</span><span class="sxs-lookup"><span data-stu-id="3a5b3-326">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="3a5b3-327">コードでは、ファイルとフォルダーのパスを持つ多くの場合、操作します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-327">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="3a5b3-328">開発用コンピューターで表示されるように、web サイトの物理的なフォルダー構造の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-328">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="3a5b3-329">Url およびパスの重要な詳細を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-329">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="3a5b3-330">URL がいずれかで、ドメイン名を開始 (`http://www.example.com`) またはサーバー名 (`http://localhost`、 `http://mycomputer`)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-330">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="3a5b3-331">URL は、ホスト コンピューター上の物理パスに対応します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-331">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="3a5b3-332">たとえば、`http://myserver`フォルダーに対応する*C:\websites\mywebsite*サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-332">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="3a5b3-333">仮想パスは、完全なパスを指定せずに、コード内のパスを表すための短縮形です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-333">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="3a5b3-334">ドメインまたはサーバー名に続く URL の部分が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-334">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="3a5b3-335">仮想パスを使用する場合は、パスを更新することがなく別のドメインまたはサーバーに、コードを移動できます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-335">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="3a5b3-336">相違点の把握に役立つ例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-336">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="3a5b3-337">完全な URL</span><span class="sxs-lookup"><span data-stu-id="3a5b3-337">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="3a5b3-338">サーバー名</span><span class="sxs-lookup"><span data-stu-id="3a5b3-338">Server name</span></span> | <span data-ttu-id="3a5b3-339">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="3a5b3-339">*mycompanyserver*</span></span> |
| <span data-ttu-id="3a5b3-340">[仮想パス]</span><span class="sxs-lookup"><span data-stu-id="3a5b3-340">Virtual path</span></span> | <span data-ttu-id="3a5b3-341">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="3a5b3-341">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="3a5b3-342">物理パス</span><span class="sxs-lookup"><span data-stu-id="3a5b3-342">Physical path</span></span> | <span data-ttu-id="3a5b3-343">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="3a5b3-343">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="3a5b3-344">仮想ルートは、/、ドライブは、c: のルートと同じように \ です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-344">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="3a5b3-345">(仮想フォルダーのパス常にフォワード スラッシュを使用します。)フォルダーの仮想パスは、物理フォルダー; として同じ名前である必要はありません。エイリアスがあります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-345">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="3a5b3-346">(運用サーバーで仮想パスほとんどと一致する正確な物理パスです。)</span><span class="sxs-lookup"><span data-stu-id="3a5b3-346">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="3a5b3-347">コードのファイルとフォルダーを使用する場合は場合がありますの物理パスおよび場合によってはどのようなオブジェクトを扱う場合によっては、仮想パスを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-347">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="3a5b3-348">ASP.NET では、これらのツールでコード ファイルとフォルダーのパスを操作するため:`Server.MapPath`メソッド、および`~`演算子と`Href`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-348">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="3a5b3-349">物理仮想パスに変換する: Server.MapPath メソッド</span><span class="sxs-lookup"><span data-stu-id="3a5b3-349">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="3a5b3-350">`Server.MapPath`メソッドは、仮想パスを変換 (と同様に */default.cshtml*) への絶対物理パス (と同様に*C:\WebSites\MyWebSiteFolder\default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-350">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="3a5b3-351">このメソッドを使用すると、いつでも、完全な物理パスを作成する必要がありますにします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-351">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="3a5b3-352">典型的な例は、読み取りまたはテキスト ファイルまたは web サーバー上の画像ファイルを作成するときです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-352">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="3a5b3-353">通常ホスティング サイトのサーバー上のサイトの絶対物理パスがわからない、このメソッドは、パスを変換できるようにするかが分かって — 仮想パス: サーバー上の対応するパスにします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-353">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="3a5b3-354">ファイルまたはフォルダーをメソッドに仮想パスを渡すし、物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-354">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="3a5b3-355">仮想ルートを参照している: ~ 演算子と Href メソッド</span><span class="sxs-lookup"><span data-stu-id="3a5b3-355">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="3a5b3-356">*.Cshtml*または *.vbhtml*ファイル、仮想ルート パスを使用して、参照することができます、`~`演算子。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-356">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="3a5b3-357">これは、機能は、サイトでは、のページを移動することができ、他のページに含まれているすべてのリンクが壊れているされませんので非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-357">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="3a5b3-358">これまで、web サイトを別の場所に移動する場合にも便利です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-358">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="3a5b3-359">次にいくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-359">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="3a5b3-360">場合は、web サイトは`http://myserver/myapp`、方法 ASP.NET が扱うこれらのパス、ページの実行時に次に示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-360">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="3a5b3-361">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="3a5b3-361">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="3a5b3-362">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="3a5b3-362">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="3a5b3-363">(変数の値としてこれらのパスは実際には表示されませんが、ASP.NET は、パスとして扱うは何でした)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-363">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="3a5b3-364">使用することができます、 `~` (上記と同じ) のサーバー コードと次のように、マークアップの両方の演算子。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-364">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="3a5b3-365">使用するマークアップで、`~`オペレーターをイメージ ファイル、その他の web ページ、および CSS ファイルなどのリソースへのパスを作成します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-365">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="3a5b3-366">ASP.NET がページ (コードとマークアップの両方) を検索し、すべて解決、ページの実行時に、`~`適切なパスへの参照。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-366">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="3a5b3-367">条件付きロジック、ループ</span><span class="sxs-lookup"><span data-stu-id="3a5b3-367">Conditional Logic and Loops</span></span>

<span data-ttu-id="3a5b3-368">ASP.NET サーバー コード タスクを実行できます条件とループを実行しているステートメントは、コードの指定の回数を繰り返し実行する書き込みコードに基づく)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-368">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="3a5b3-369">条件をテストします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-369">Testing conditions</span></span>

<span data-ttu-id="3a5b3-370">使用する単純な条件をテストする、`If...Then`ステートメントでは、返す`True`または`False`を指定するテストに基づきます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-370">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="3a5b3-371">`If`キーワード、ブロックを開始します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-371">The `If` keyword starts a block.</span></span> <span data-ttu-id="3a5b3-372">実際のテスト (条件) に依存して、`If`キーワードと true または false を返します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-372">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="3a5b3-373">`If`文の末尾に`Then`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-373">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="3a5b3-374">テストが true の場合に実行するステートメントが囲ま`If`と`End If`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-374">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="3a5b3-375">`If`ステートメントを含めることができます、`Else`ブロックで、条件が false の場合に実行するステートメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-375">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="3a5b3-376">場合、`If`ステートメントには、コード ブロックが開始され、通常を使用する必要はありません`Code...End Code`ブロックを含めるようにステートメントをします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-376">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="3a5b3-377">追加するだけ`@`ブロックにも、正しく機能します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-377">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="3a5b3-378">このアプローチに`If`だけでなく他の Visual Basic の含むコード ブロックが続くキーワードをプログラミング`For`、 `For Each`、 `Do While`, などです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-378">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="3a5b3-379">1 つまたは複数を使用して複数の条件を追加する`ElseIf`ブロック。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-379">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="3a5b3-380">最初の条件の場合、この例では、`If`ブロックが真ではありません、`ElseIf`条件をチェックします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-380">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="3a5b3-381">その条件が満たされた場合内のステートメント、`ElseIf`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-381">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="3a5b3-382">条件も満たさない場合、内のステートメント、`Else`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-382">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="3a5b3-383">任意の数を追加する`ElseIf`をブロックしてを閉じて、`Else`としてブロック、&quot;他のすべて&quot;条件。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-383">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="3a5b3-384">多くの条件をテストするには、使用、`Select Case`ブロック。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-384">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="3a5b3-385">テストする値は、(例では、weekday 変数) のかっこ内です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-385">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="3a5b3-386">各テストを使用して、`Case`ステートメントが、値を一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-386">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="3a5b3-387">場合の値、`Case`ステートメントはそのコードのテスト値に一致`Case`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-387">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="3a5b3-388">ブラウザーに表示される最後の 2 つの条件付きブロックの結果:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-388">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="3a5b3-390">コードのループ</span><span class="sxs-lookup"><span data-stu-id="3a5b3-390">Looping code</span></span>

<span data-ttu-id="3a5b3-391">多くの場合、同じステートメントを繰り返し実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-391">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="3a5b3-392">これを行うをループします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-392">You do this by looping.</span></span> <span data-ttu-id="3a5b3-393">たとえば、多くの場合、ステートメントを実行する同じ項目ごとにデータのコレクション。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-393">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="3a5b3-394">使用することが正確に何回かループすることがわかっている場合、`For`ループします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-394">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="3a5b3-395">このようなループ カウントまたはカウント ダウンの際に特に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-395">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="3a5b3-396">ループはで始まり、`For`キーワード、3 つの要素に続きます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-396">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="3a5b3-397">直後に、`For`カウンター変数を宣言するステートメントでは、(を使用する必要はありません`Dim`) ように、範囲を示す`i = 10 to 20`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-397">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="3a5b3-398">つまり、変数`i`10 にカウントを開始および 20 (包括) に到達するまで続行されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-398">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="3a5b3-399">間、`For`と`Next`ステートメントは、ブロックのコンテンツ。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-399">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="3a5b3-400">これは、1 つまたは複数のコード ステートメントを実行する各ループが発生を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-400">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="3a5b3-401">`Next i`ステートメントはループを終了します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-401">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="3a5b3-402">カウンターをインクリメントし、ループの次のイテレーションを開始します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-402">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="3a5b3-403">コードの間の行、`For`と`Next`行には、ループのイテレーションごとに実行されるコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-403">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="3a5b3-404">マークアップは、新しい段落を作成する (`<p>`要素) の値を表示する出力に行を追加し、それぞれ i (カウンター)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-404">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="3a5b3-405">このページを実行すると、例は、11 行の行のテキスト項目数を示す行ごとに、出力の表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-405">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="3a5b3-407">コレクションまたは配列を扱う場合場合、多くの場合、使用する、`For Each`ループします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-407">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="3a5b3-408">コレクションは、類似のオブジェクトのグループと`For Each`ループに、コレクション内の各項目にタスクを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-408">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="3a5b3-409">この種類のループは、コレクションの便利なためとは異なり、`For`ループすることも、カウンターをインクリメントまたは制限を設定します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-409">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="3a5b3-410">代わりに、`For Each`が終了するまで、このループのコードは、単にコレクションを続行されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-410">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="3a5b3-411">この例は、内の項目を返します、 `Request.ServerVariables` (コレクションを web サーバーに関する情報が含まれています)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-411">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="3a5b3-412">使用して、`For Each`を新規作成して各アイテムの名前を表示するループ`<li>`HTML 箇条書きリスト内の要素。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-412">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="3a5b3-413">`For Each`キーワードの後に、コレクション内の 1 つの項目を表す変数 (例では、 `myItem`) と、その後、`In`キーワードのコレクションをループするか後にします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-413">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="3a5b3-414">本体で、`For Each`ループ、先ほど宣言した変数を使用する現在のアイテムにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-414">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="3a5b3-416">汎用的なループを作成するには、使用、`Do While`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-416">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="3a5b3-417">このループはで始まり、`Do While`状態は、後に、キーワードの後に、ブロックを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-417">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="3a5b3-418">ループが通常インクリメント (に追加する) または減分 (減算) 変数またはオブジェクト カウントで使用されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-418">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="3a5b3-419">この例で、`+=`演算子に 1 を加算する変数の値、ループが実行されるたびにします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-419">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="3a5b3-420">(カウント ダウンをループ内の変数をデクリメントするためには、デクリメント演算子を使用すると`-=`)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-420">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="3a5b3-421">オブジェクトとコレクション</span><span class="sxs-lookup"><span data-stu-id="3a5b3-421">Objects and Collections</span></span>

<span data-ttu-id="3a5b3-422">ほぼすべての ASP.NET web サイトでは、web ページ自体を含むオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-422">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="3a5b3-423">このセクションでは、演習では多くの場合、コードでいくつかの重要なオブジェクトについて説明します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-423">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="3a5b3-424">ページのオブジェクト</span><span class="sxs-lookup"><span data-stu-id="3a5b3-424">Page objects</span></span>

<span data-ttu-id="3a5b3-425">ASP.NET の最も基本的なオブジェクトは、ページです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-425">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="3a5b3-426">オブジェクトを修飾せずに直接 page オブジェクトのプロパティにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-426">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="3a5b3-427">次のコード ページのファイルのパスを取得するを使用して、`Request`ページのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-427">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="3a5b3-428">プロパティを使用することができます、`Page`など多くの情報を取得するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-428">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="3a5b3-429">`Request`。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-429">`Request`.</span></span> <span data-ttu-id="3a5b3-430">既に説明したようにこれは、ブラウザーの種類の要求を作成、ページや、ユーザー id などの URL を含む、現在の要求に関する情報のコレクション。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-430">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="3a5b3-431">`Response`。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-431">`Response`.</span></span> <span data-ttu-id="3a5b3-432">これは、サーバー コードの実行が終了したら、ブラウザーに送信される応答 (ページ) に関する情報のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-432">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="3a5b3-433">たとえば、このプロパティを使用すると、情報を応答に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-433">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="3a5b3-434">コレクション オブジェクト (配列とディクショナリ)</span><span class="sxs-lookup"><span data-stu-id="3a5b3-434">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="3a5b3-435">コレクションのコレクションなど、同じ種類のオブジェクトのグループは、`Customer`データベースからのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-435">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="3a5b3-436">同様に ASP.NET に多くの組み込みコレクションが含まれています、`Request.Files`コレクション。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-436">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="3a5b3-437">多くの場合、コレクション内のデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-437">You'll often work with data in collections.</span></span> <span data-ttu-id="3a5b3-438">2 つの共通コレクション型とは、*配列*と*ディクショナリ*です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-438">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="3a5b3-439">配列は、類似した項目のコレクションを格納するたくない個別の各項目を保持する変数を作成するときに便利です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-439">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="3a5b3-440">配列、特定のデータ型を宣言するなど`String`、 `Integer`、または`DateTime`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-440">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="3a5b3-441">変数が配列を含めることができます、宣言で変数名にかっこを追加することを示すために (など`Dim myVar() As String`)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-441">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="3a5b3-442">アイテムの位置 (インデックス) を使用して配列またはを使用してアクセスできます、`For Each`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-442">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="3a5b3-443">配列のインデックスは 0 から始まる&#8212;は、最初の項目がある位置 0、2 番目の項目位置は 1 というようにします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-443">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="3a5b3-444">取得することによって、配列内のアイテムの数を指定できます、`Length`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-444">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="3a5b3-445">配列内の特定の項目の位置を取得する (つまり、配列を検索します)、使用、`Array.IndexOf`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-445">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="3a5b3-446">行うことができますも逆方向のような配列の内容 (、`Array.Reverse`メソッド) するか、内容を並べ替える (、`Array.Sort`メソッド)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-446">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="3a5b3-447">ブラウザーで表示されている文字列配列のコードの出力:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-447">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="3a5b3-449">設定または対応する値を取得するキー (または名前) を指定したキー/値ペアのコレクションをディクショナリには。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-449">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="3a5b3-450">使用するディクショナリを作成する、`New`を新規作成しているかを示すキーワード`Dictionary`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-450">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="3a5b3-451">変数を使用して、ディクショナリを割り当てることができます、`Dim`キーワード。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-451">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="3a5b3-452">かっこを使用してディクショナリ内の項目のデータ型を指定する ( `( )` )。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-452">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="3a5b3-453">宣言の最後に、これは、実際には、メソッド、新しいディクショナリを作成するために、別のかっこのペアを追加してください。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-453">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="3a5b3-454">ディクショナリに項目を追加するに呼び出せる、`Add`辞書の変数のメソッド (`myScores`ここでは)、キーと値を指定します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-454">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="3a5b3-455">または、かっこを使用して、キーが示され、次の例のように、単純な代入を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-455">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="3a5b3-456">をディクショナリから値を取得するには、かっこ内にキーを指定します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-456">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="3a5b3-457">パラメーターを持つメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="3a5b3-457">Calling Methods with Parameters</span></span>

<span data-ttu-id="3a5b3-458">この記事の前半で説明したとおりにプログラム オブジェクトには方法があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-458">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="3a5b3-459">たとえば、`Database`オブジェクトがあります、`Database.Connect`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-459">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="3a5b3-460">多くのメソッドには、1 つまたは複数のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-460">Many methods also have one or more parameters.</span></span> <span data-ttu-id="3a5b3-461">A*パラメーター*メソッドに渡す値は、そのタスクを完了する方法を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-461">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="3a5b3-462">たとえばの宣言を見て、`Request.MapPath`を 3 つのパラメーターを受け取るメソッド。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-462">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="3a5b3-463">このメソッドは、指定した仮想パスに対応するサーバーの物理パスを返します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-463">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="3a5b3-464">次の 3 つのパラメーター、メソッドは、 `virtualPath`、 `baseVirtualDir`、および`allowCrossAppMapping`です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-464">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="3a5b3-465">(宣言では、パラメーターが表示されることを承諾するデータのデータ型とに注意してください)。このメソッドを呼び出すときに、次の 3 つのすべてのパラメーターの値を指定してください。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-465">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="3a5b3-466">メソッドにパラメーターを渡すための 2 つのオプションを Visual Basic を使用して、Razor 構文を使用している場合がある:*位置指定パラメーター*または*名前付きパラメーター*です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-466">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="3a5b3-467">位置指定パラメーターを使用して、メソッドを呼び出すには、メソッドの宣言で指定されている厳密な順序でパラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-467">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="3a5b3-468">(通常わかりますこの順序メソッドのドキュメントを読み取ることによってです。)、順序に従う必要があります、パラメーターのいずれかをスキップすることはできませんと&#8212;かどうか必要に応じて、空の文字列を渡す (`""`) またはの値がない位置指定パラメーターに null です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-468">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="3a5b3-469">次の例は、という名前のフォルダーがある前提としています。*スクリプト*、web サイトにします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-469">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="3a5b3-470">コードの呼び出し、`Request.MapPath`メソッドとパスの値が正しい順序で 3 つのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-470">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="3a5b3-471">結果のマップされたパスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-471">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="3a5b3-472">メソッドの多くのパラメーターが存在する場合、維持する、コード クリーナーより読みやすい名前付きパラメーターを使用しています。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-472">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="3a5b3-473">名前付きパラメーターを使用してメソッドを呼び出すには、続けてパラメーター名を指定`:=`し、値を指定します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-473">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="3a5b3-474">名前付きパラメーターの利点は、任意の順序で追加できることです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-474">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="3a5b3-475">(欠点は、メソッドの呼び出しがコンパクトではないことです。)</span><span class="sxs-lookup"><span data-stu-id="3a5b3-475">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="3a5b3-476">次の例は、上記と同じメソッドを呼び出しますが、名前付きの値を指定するパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-476">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="3a5b3-477">ご覧のように、異なる順序でパラメーターが渡されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-477">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="3a5b3-478">ただし、前の例、この例を実行する場合は、同じ値を返すがあります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-478">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="3a5b3-479">エラー処理</span><span class="sxs-lookup"><span data-stu-id="3a5b3-479">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="3a5b3-480">Try-catch ステートメント</span><span class="sxs-lookup"><span data-stu-id="3a5b3-480">Try-Catch statements</span></span>

<span data-ttu-id="3a5b3-481">ステートメントは、上の理由から、コントロールの外部できない可能性があるコードの多くの場合があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-481">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="3a5b3-482">例えば:</span><span class="sxs-lookup"><span data-stu-id="3a5b3-482">For example:</span></span>

- <span data-ttu-id="3a5b3-483">コードは、開く、作成、読み取り、またはファイルの書き込みを試みると、あらゆる種類のエラーが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-483">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="3a5b3-484">目的のファイルが存在しない可能性があります、ロックされる可能性があります、コード可能性がありますいないアクセス許可を持つし、なります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-484">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="3a5b3-485">同様に、コードでは、データベース内のレコードを更新しようとすると、アクセス許可の問題があります、データベースへの接続を削除する場合があります、無効な場合とに、データを保存する場合があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-485">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="3a5b3-486">プログラミング用語では、このような状況が呼び出されます*例外*です。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-486">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="3a5b3-487">場合に、コードには、例外が発生すると、(スロー) を生成するエラー メッセージが表示されている、最高でユーザーに迷惑なです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-487">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="3a5b3-489">コードが例外を発生可能性がある場合に、この種類のエラー メッセージを回避するために、使用する`Try/Catch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-489">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="3a5b3-490">`Try`をチェックする場合のコードを実行するステートメントでは、します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-490">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="3a5b3-491">1 つまたは複数`Catch`ステートメントを検索できる固有の仕様が発生したエラー (特定の種類の例外)。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-491">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="3a5b3-492">多くとして含めることができます`Catch`するとしてステートメントを予測しているエラーを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-492">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="3a5b3-493">使用しないことをお勧め、`Response.Redirect`メソッド`Try/Catch`ステートメント、ページに、例外を引き起こす可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-493">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="3a5b3-494">次の例では、最初の要求でテキスト ファイルを作成し、ユーザーがファイルを開くできるボタンを表示するページを示します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-494">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="3a5b3-495">意図的を使って不適切なファイル名と、例外を使用することができるようにします。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-495">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="3a5b3-496">コードが含まれています`Catch`可能性のある例外の 2 つのステートメント: `FileNotFoundException`、ファイル名が間違っている場合に発生して`DirectoryNotFoundException`ASP.NET はフォルダーにも見つからない場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-496">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="3a5b3-497">(できますコメントを解除する例では、内のステートメントすべてが正しく機能しているときの動作を確認するためにします。)</span><span class="sxs-lookup"><span data-stu-id="3a5b3-497">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="3a5b3-498">コードが例外を処理していない場合、前のスクリーン ショットのようなエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-498">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="3a5b3-499">ただし、`Try/Catch`セクションでは、ユーザーがこの種のエラーを表示するを防ぐのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3a5b3-499">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="3a5b3-500">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="3a5b3-500">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="3a5b3-501">リファレンス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="3a5b3-501">Reference Documentation</span></span>

- [<span data-ttu-id="3a5b3-502">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3a5b3-502">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="3a5b3-503">Visual Basic 言語</span><span class="sxs-lookup"><span data-stu-id="3a5b3-503">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
