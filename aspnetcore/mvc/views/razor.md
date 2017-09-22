---
title: "ASP.NET Core の razor 構文のリファレンス"
author: rick-anderson
description: "Razor 構文を詳細します。"
keywords: "ASP.NET Core、Razor"
ms.author: riande
manager: wpickett
ms.date: 07/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 066fe3b2486c63bd4de2ccb865ad432a67846d77
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="450b0-104">ASP.NET Core の razor 構文</span><span class="sxs-lookup"><span data-stu-id="450b0-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="450b0-105">によって[Taylor Mullen](https://twitter.com/ntaylormullen)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="450b0-105">By [Taylor Mullen](https://twitter.com/ntaylormullen) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="450b0-106">Razor とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="450b0-106">What is Razor?</span></span>

<span data-ttu-id="450b0-107">Razor とは、サーバー ベースのコードを埋め込む web ページのマークアップ構文です。</span><span class="sxs-lookup"><span data-stu-id="450b0-107">Razor is a markup syntax for embedding server based code into web pages.</span></span> <span data-ttu-id="450b0-108">Razor 構文は、c#、HTML、Razor マークアップで構成されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-108">The Razor syntax consists of Razor markup, C# and HTML.</span></span> <span data-ttu-id="450b0-109">通常、Razor を含むファイルが、 *.cshtml*ファイル拡張子。</span><span class="sxs-lookup"><span data-stu-id="450b0-109">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="450b0-110">HTML を表示</span><span class="sxs-lookup"><span data-stu-id="450b0-110">Rendering HTML</span></span>

<span data-ttu-id="450b0-111">Razor の既定の言語は、HTML です。</span><span class="sxs-lookup"><span data-stu-id="450b0-111">The default Razor language is HTML.</span></span> <span data-ttu-id="450b0-112">Razor から HTML をレンダリングは、HTML ファイルと同じです。</span><span class="sxs-lookup"><span data-stu-id="450b0-112">Rendering HTML from Razor is no different than in an HTML file.</span></span> <span data-ttu-id="450b0-113">次のマークアップ Razor ファイル:</span><span class="sxs-lookup"><span data-stu-id="450b0-113">A Razor file with the following markup:</span></span>

```html
<p>Hello World</p>
   ```

<span data-ttu-id="450b0-114">変更されることが表示される`<p>Hello World</p>`サーバーでします。</span><span class="sxs-lookup"><span data-stu-id="450b0-114">Is rendered unchanged as `<p>Hello World</p>` by the server.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="450b0-115">Razor 構文</span><span class="sxs-lookup"><span data-stu-id="450b0-115">Razor syntax</span></span>

<span data-ttu-id="450b0-116">Razor (C#) をサポートしを使用して、 `@` HTML を c# から移行する記号。</span><span class="sxs-lookup"><span data-stu-id="450b0-116">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="450b0-117">Razor では、c# 式を評価し、それらを HTML 出力でレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="450b0-117">Razor evaluates C# expressions and renders them in the HTML output.</span></span> <span data-ttu-id="450b0-118">Razor では、HTML から C# または Razor 固有のマークアップに移行できます。</span><span class="sxs-lookup"><span data-stu-id="450b0-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="450b0-119">ときに、`@`シンボルが続く、 [Razor 予約キーワード](#razor-reserved-keywords)Razor 固有のマークアップに遷移、遷移プレーンな C# の場合にそれ以外の場合。</span><span class="sxs-lookup"><span data-stu-id="450b0-119">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords) it transitions into Razor-specific markup, otherwise it transitions into plain C#.</span></span>

<a name=escape-at-label></a>

<span data-ttu-id="450b0-120">HTML を含む`@`シンボルは、1 秒あたりにエスケープする必要があります`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="450b0-120">HTML containing `@` symbols may need to be escaped with a second `@` symbol.</span></span> <span data-ttu-id="450b0-121">例:</span><span class="sxs-lookup"><span data-stu-id="450b0-121">For example:</span></span>

```html
<p>@@Username</p>
   ```

<span data-ttu-id="450b0-122">次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="450b0-122">would render the following HTML:</span></span>

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

<span data-ttu-id="450b0-123">HTML 属性と電子メール アドレスを含むコンテンツが処理されない、`@`遷移文字として記号。</span><span class="sxs-lookup"><span data-stu-id="450b0-123">HTML attributes and content containing email addresses don’t treat the `@` symbol as a transition character.</span></span>

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a><span data-ttu-id="450b0-124">Razor の暗黙的な式</span><span class="sxs-lookup"><span data-stu-id="450b0-124">Implicit Razor expressions</span></span>

<span data-ttu-id="450b0-125">Razor の暗黙的な式が始まる`@`c# コードを続けています。</span><span class="sxs-lookup"><span data-stu-id="450b0-125">Implicit Razor expressions start with `@` followed by C# code.</span></span> <span data-ttu-id="450b0-126">例:</span><span class="sxs-lookup"><span data-stu-id="450b0-126">For example:</span></span>

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="450b0-127">ただし、c#`await`キーワードの暗黙的な式はスペースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="450b0-127">With the exception of the C# `await` keyword implicit expressions must not contain spaces.</span></span> <span data-ttu-id="450b0-128">たとえば、(C#) ステートメントがあるオフの末尾にある限りは、スペースを intermingle できます。</span><span class="sxs-lookup"><span data-stu-id="450b0-128">For example, you can intermingle spaces as long as the C# statement has a clear ending:</span></span>

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="450b0-129">Razor の明示的な式</span><span class="sxs-lookup"><span data-stu-id="450b0-129">Explicit Razor expressions</span></span>

<span data-ttu-id="450b0-130">Razor の明示的な式から成る、@ バランスの取れたかっこ記号。</span><span class="sxs-lookup"><span data-stu-id="450b0-130">Explicit Razor expressions consists of an @ symbol with balanced parenthesis.</span></span> <span data-ttu-id="450b0-131">たとえば、先週の時間を表示するためにします。</span><span class="sxs-lookup"><span data-stu-id="450b0-131">For example, to render last week's time:</span></span>

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="450b0-132">内のコンテンツ、@ () かっこが評価され、出力に表示されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-132">Any content within the @() parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="450b0-133">暗黙的な式は一般に、スペースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="450b0-133">Implicit expressions generally cannot contain spaces.</span></span> <span data-ttu-id="450b0-134">たとえば、次のコードに 1 週間はから差し引かれません、現在の時刻。</span><span class="sxs-lookup"><span data-stu-id="450b0-134">For example, in the code below, one week is not subtracted from the current time:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

<span data-ttu-id="450b0-135">これは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="450b0-135">Which renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

<span data-ttu-id="450b0-136">Expression の結果の文字列を連結するのに明示的な式を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="450b0-136">You can use an explicit expression to concatenate text with an expression result:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="450b0-137">明示的の式がない`<p>Age@joe.Age</p>`電子メール アドレスとして扱われてと`<p>Age@joe.Age</p>`レンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="450b0-137">Without the explicit expression, `<p>Age@joe.Age</p>` would be treated as an email address and `<p>Age@joe.Age</p>` would be rendered.</span></span> <span data-ttu-id="450b0-138">明示的な式として書き込まれるときに`<p>Age33</p>`が表示されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-138">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a><span data-ttu-id="450b0-139">式のエンコード</span><span class="sxs-lookup"><span data-stu-id="450b0-139">Expression encoding</span></span>

<span data-ttu-id="450b0-140">文字列に評価される c# 式は、HTML エンコードです。</span><span class="sxs-lookup"><span data-stu-id="450b0-140">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="450b0-141">C# 式の評価が`IHtmlContent`経由で直接レンダリング*IHtmlContent.WriteTo*です。</span><span class="sxs-lookup"><span data-stu-id="450b0-141">C# expressions that evaluate to `IHtmlContent` are rendered directly through *IHtmlContent.WriteTo*.</span></span> <span data-ttu-id="450b0-142">C# 式の評価がない*IHtmlContent*を文字列に変換 (によって*ToString*) し、レンダリングされる前にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="450b0-142">C# expressions that don't evaluate to *IHtmlContent* are converted to a string (by *ToString*) and encoded before they are rendered.</span></span> <span data-ttu-id="450b0-143">たとえば、次の Razor マークアップ。</span><span class="sxs-lookup"><span data-stu-id="450b0-143">For example, the following Razor markup:</span></span>

```html
@("<span>Hello World</span>")
   ```

<span data-ttu-id="450b0-144">これに HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="450b0-144">Renders this HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

<span data-ttu-id="450b0-145">ブラウザーは、としてレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="450b0-145">Which the browser renders as:</span></span>

`<span>Hello World</span>`

<span data-ttu-id="450b0-146">`HtmlHelper``Raw`出力ではなくでエンコードされた、HTML マークアップとしてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="450b0-146">`HtmlHelper` `Raw` output is not encoded but rendered as HTML markup.</span></span>

>[!WARNING]
> <span data-ttu-id="450b0-147">使用して`HtmlHelper.Raw`unsanitized ユーザー入力は、セキュリティ上のリスクです。</span><span class="sxs-lookup"><span data-stu-id="450b0-147">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="450b0-148">ユーザー入力は、悪意のある JavaScript または他の攻撃に含まれる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="450b0-148">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="450b0-149">ユーザー入力をサニタイズしが困難なを使用しないでください`HtmlHelper.Raw`ユーザーの入力します。</span><span class="sxs-lookup"><span data-stu-id="450b0-149">Sanitizing user input is difficult, avoid using `HtmlHelper.Raw` on user input.</span></span>

<span data-ttu-id="450b0-150">次の Razor マークアップ。</span><span class="sxs-lookup"><span data-stu-id="450b0-150">The following Razor markup:</span></span>

```html
@Html.Raw("<span>Hello World</span>")
   ```

<span data-ttu-id="450b0-151">これに HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="450b0-151">Renders this HTML:</span></span>

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a><span data-ttu-id="450b0-152">Razor コードのブロック</span><span class="sxs-lookup"><span data-stu-id="450b0-152">Razor code blocks</span></span>

<span data-ttu-id="450b0-153">Razor コード ブロックが始まる`@`で囲まれたと`{}`です。</span><span class="sxs-lookup"><span data-stu-id="450b0-153">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="450b0-154">式とは異なりコード ブロック内の c# コードはレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="450b0-154">Unlike expressions, C# code inside code blocks is not rendered.</span></span> <span data-ttu-id="450b0-155">同じスコープを共有し、順序でも定義コード ブロックおよび Razor ページ内の式 (、コード ブロックで宣言されると後のコード ブロックと式のスコープ内)。</span><span class="sxs-lookup"><span data-stu-id="450b0-155">Code blocks and expressions in a Razor page share the same scope and are defined in order (that is, declarations in a code block will be in scope for later code blocks and expressions).</span></span>

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

<span data-ttu-id="450b0-156">描画されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-156">Would render:</span></span>

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a><span data-ttu-id="450b0-157">暗黙の切り替え</span><span class="sxs-lookup"><span data-stu-id="450b0-157">Implicit transitions</span></span>

<span data-ttu-id="450b0-158">既定の言語コード ブロックでは、C# の場合が、HTML に移行することができます。</span><span class="sxs-lookup"><span data-stu-id="450b0-158">The default language in a code block is C#, but you can transition back to HTML.</span></span> <span data-ttu-id="450b0-159">コード ブロック内の HTML は HTML 表示に戻す移行します。</span><span class="sxs-lookup"><span data-stu-id="450b0-159">HTML within a code block will transition back into rendering HTML:</span></span>

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a><span data-ttu-id="450b0-160">明示的な区切り記号付き遷移</span><span class="sxs-lookup"><span data-stu-id="450b0-160">Explicit delimited transition</span></span>

<span data-ttu-id="450b0-161">HTML を描画するコード ブロックのサブ セクションを定義するのには、Razor で表示する文字を囲みます`<text>`タグ。</span><span class="sxs-lookup"><span data-stu-id="450b0-161">To define a sub-section of a code block that should render HTML, surround the characters to be rendered with the Razor `<text>` tag:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="450b0-162">一般に、この方法は、HTML タグで囲まれていない HTML をレンダリングするときに使用します。</span><span class="sxs-lookup"><span data-stu-id="450b0-162">You generally use this approach when you want to render HTML that is not surrounded by an HTML tag.</span></span> <span data-ttu-id="450b0-163">HTML または Razor のタグのない Razor 実行時エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-163">Without an HTML or Razor tag, you get a Razor runtime error.</span></span>

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="450b0-164">明示的な行の移行`@:`</span><span class="sxs-lookup"><span data-stu-id="450b0-164">Explicit Line Transition with `@:`</span></span>

<span data-ttu-id="450b0-165">コード ブロックの内部 HTML として残りの行全体を表示を使用して、`@:`構文。</span><span class="sxs-lookup"><span data-stu-id="450b0-165">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="450b0-166">なし、`@:`上記のコードでは、実行時エラー Razor を取得します。</span><span class="sxs-lookup"><span data-stu-id="450b0-166">Without the `@:` in the code above, you'd get a Razor run time error.</span></span>

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a><span data-ttu-id="450b0-167">制御構造</span><span class="sxs-lookup"><span data-stu-id="450b0-167">Control Structures</span></span>

<span data-ttu-id="450b0-168">制御構造体は、コード ブロックの拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="450b0-168">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="450b0-169">コード ブロック (マークアップ、インライン c# への遷移) ものすべての側面は、次の構造体に適用されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-169">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="450b0-170">条件付き`@if`、 `else if`、`else`と`@switch`</span><span class="sxs-lookup"><span data-stu-id="450b0-170">Conditionals `@if`, `else if`, `else` and `@switch`</span></span>

<span data-ttu-id="450b0-171">`@if`ファミリ制御コードが実行されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-171">The `@if` family controls when code runs:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

<span data-ttu-id="450b0-172">`else`および`else if`を必要としない、`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="450b0-172">`else` and `else if` don't require the `@` symbol:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

<span data-ttu-id="450b0-173">次のようにスイッチ ステートメントを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="450b0-173">You can use a switch statement like this:</span></span>

```none
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="450b0-174">ループ`@for`、 `@foreach`、 `@while`、および`@do while`</span><span class="sxs-lookup"><span data-stu-id="450b0-174">Looping `@for`, `@foreach`, `@while`, and `@do while`</span></span>

<span data-ttu-id="450b0-175">コントロール ステートメントのループで template 宣言された HTML をレンダリングすることができます。</span><span class="sxs-lookup"><span data-stu-id="450b0-175">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="450b0-176">たとえば、ユーザーの一覧を表示するためにします。</span><span class="sxs-lookup"><span data-stu-id="450b0-176">For example, to render a list of people:</span></span>

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

<span data-ttu-id="450b0-177">次のループ ステートメントのいずれかの操作を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="450b0-177">You can use any of the following looping statements:</span></span>

`@for`

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```none
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```none
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```none
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="450b0-178">複合`@using`</span><span class="sxs-lookup"><span data-stu-id="450b0-178">Compound `@using`</span></span>

<span data-ttu-id="450b0-179">C# を使用して、オブジェクトが破棄されることを確認するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="450b0-179">In C# a using statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="450b0-180">Razor で追加のコンテンツを含む HTML ヘルパーの作成をこれと同じメカニズムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="450b0-180">In Razor this same mechanism can be used to create HTML helpers that contain additional content.</span></span> <span data-ttu-id="450b0-181">インスタンスとフォーム タグをレンダリングする HTML ヘルパーを利用できます、`@using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="450b0-181">For instance, we can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

```none
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

<span data-ttu-id="450b0-182">上記のようなスコープ レベルのアクションを実行することもできます。[タグ ヘルパー](tag-helpers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="450b0-182">You can also perform scope level actions like the above with [Tag Helpers](tag-helpers/index.md).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="450b0-183">`@try`、`catch`、`finally`</span><span class="sxs-lookup"><span data-stu-id="450b0-183">`@try`, `catch`, `finally`</span></span>

<span data-ttu-id="450b0-184">例外処理は、C# の場合に似ています。</span><span class="sxs-lookup"><span data-stu-id="450b0-184">Exception handling is similar to  C#:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

<span data-ttu-id="450b0-185">Razor では、クリティカル セクション lock ステートメントを保護する機能があります。</span><span class="sxs-lookup"><span data-stu-id="450b0-185">Razor has the capability to protect critical sections with lock statements:</span></span>

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="450b0-186">コメント</span><span class="sxs-lookup"><span data-stu-id="450b0-186">Comments</span></span>

<span data-ttu-id="450b0-187">Razor では、c# と HTML のコメントをサポートします。</span><span class="sxs-lookup"><span data-stu-id="450b0-187">Razor supports C# and HTML comments.</span></span> <span data-ttu-id="450b0-188">次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="450b0-188">The following markup:</span></span>

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

<span data-ttu-id="450b0-189">サーバーによってレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="450b0-189">Is rendered by the server as:</span></span>

```none
<!-- HTML comment -->
```

<span data-ttu-id="450b0-190">Razor コメントは、ページが表示される前に、サーバーによって削除されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-190">Razor comments are removed by the server before the page is rendered.</span></span> <span data-ttu-id="450b0-191">Razor を使用して`@*  *@`コメントを区切るためにします。</span><span class="sxs-lookup"><span data-stu-id="450b0-191">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="450b0-192">次のコードがコメント アウト、ため、サーバーにすべてのマークアップは表示されません。</span><span class="sxs-lookup"><span data-stu-id="450b0-192">The following code is commented out, so the server will not render any markup:</span></span>

```none
 @*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a><span data-ttu-id="450b0-193">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="450b0-193">Directives</span></span>

<span data-ttu-id="450b0-194">Razor ディレクティブが次の予約済みキーワードからの暗黙的な式で表される、`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="450b0-194">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="450b0-195">ディレクティブは通常、ページの解析方法を変更するか、Razor ページ内の別の機能を有効にします。</span><span class="sxs-lookup"><span data-stu-id="450b0-195">A directive will typically change the way a page is parsed or enable different functionality within your Razor page.</span></span>

<span data-ttu-id="450b0-196">Razor がビューのコードを生成する方法を理解することが容易にできるようにディレクティブの動作を理解します。</span><span class="sxs-lookup"><span data-stu-id="450b0-196">Understanding how Razor generates code for a view will make it easier to understand how directives work.</span></span> <span data-ttu-id="450b0-197">Razor ページを使用して、c# ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="450b0-197">A Razor page is used to generate a C# file.</span></span> <span data-ttu-id="450b0-198">たとえば、Razor ページ:</span><span class="sxs-lookup"><span data-stu-id="450b0-198">For example, this Razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="450b0-199">次のようなクラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-199">Generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="450b0-200">[ビューに対して生成された Razor c# クラスを表示する](#razor-customcompilationservice-label)この生成されたクラスを表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="450b0-200">[Viewing the Razor C# class generated for a view](#razor-customcompilationservice-label) explains how to view this generated class.</span></span>

### `@using`

<span data-ttu-id="450b0-201">`@using`ディレクティブを追加すると、c#`using`ディレクティブを生成された razor ページ。</span><span class="sxs-lookup"><span data-stu-id="450b0-201">The `@using` directive will add the c# `using` directive to the generated razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

<span data-ttu-id="450b0-202">`@model`ディレクティブは、Razor ページに渡されたモデルの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="450b0-202">The `@model` directive specifies the type of the model passed to the Razor page.</span></span> <span data-ttu-id="450b0-203">このツールでは、次の構文が使用されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-203">It uses the following syntax:</span></span>

```none
@model TypeNameOfModel
   ```

<span data-ttu-id="450b0-204">たとえば、個々 のユーザー アカウントを持つ ASP.NET Core MVC アプリを作成する場合、 *Views/Account/Login.cshtml* Razor ビューには、次のモデルの宣言が含まれています。</span><span class="sxs-lookup"><span data-stu-id="450b0-204">For example, if you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* Razor view contains the following model declaration:</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="450b0-205">クラスの例で生成されるクラスを継承から`RazorPage<dynamic>`です。</span><span class="sxs-lookup"><span data-stu-id="450b0-205">In the preceding class example, the class generated inherits from `RazorPage<dynamic>`.</span></span> <span data-ttu-id="450b0-206">追加することによって、`@model`新機能は、継承を制御します。</span><span class="sxs-lookup"><span data-stu-id="450b0-206">By adding an `@model` you control what’s inherited.</span></span> <span data-ttu-id="450b0-207">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="450b0-207">For example</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="450b0-208">次のクラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-208">Generates the following class</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

<span data-ttu-id="450b0-209">Razor ページを公開、`Model`モデルにアクセスするためのプロパティ ページに渡されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-209">Razor pages expose a `Model` property for accessing the model passed to the page.</span></span>

```html
<div>The Login Email: @Model.Email</div>
   ```

<span data-ttu-id="450b0-210">`@model`ディレクティブは、このプロパティの型を指定 (指定することによって、`T`で`RazorPage<T>`からページに対して生成されたクラスが派生する)。</span><span class="sxs-lookup"><span data-stu-id="450b0-210">The `@model` directive specified the type of this property (by specifying the `T` in `RazorPage<T>` that the generated class for your page derives from).</span></span> <span data-ttu-id="450b0-211">指定しない場合は、`@model`ディレクティブ、`Model`型プロパティである`dynamic`です。</span><span class="sxs-lookup"><span data-stu-id="450b0-211">If you don't specify the `@model` directive the `Model` property will be of type `dynamic`.</span></span> <span data-ttu-id="450b0-212">モデルの値は、コント ローラーからビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-212">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="450b0-213">参照してください[モデルを厳密に型指定と@modelキーワード](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="450b0-213">See [Strongly typed models and the @model keyword](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) for more information.</span></span>

### `@inherits`

<span data-ttu-id="450b0-214">`@inherits`ディレクティブは、Razor ページを継承するクラスを完全に制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="450b0-214">The `@inherits` directive gives you full control of the class your Razor page inherits:</span></span>

```none
@inherits TypeNameOfClassToInheritFrom
   ```

<span data-ttu-id="450b0-215">インスタンスが発生しました。 次のカスタム Razor ページの種類とします。</span><span class="sxs-lookup"><span data-stu-id="450b0-215">For instance, let’s say we had the following custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="450b0-216">次の Razor 生成`<div>Custom text: Hello World</div>`です。</span><span class="sxs-lookup"><span data-stu-id="450b0-216">The following Razor would generate `<div>Custom text: Hello World</div>`.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="450b0-217">使用することはできません`@model`と`@inherits`同じページにします。</span><span class="sxs-lookup"><span data-stu-id="450b0-217">You can't use `@model` and `@inherits` on the same page.</span></span> <span data-ttu-id="450b0-218">した`@inherits`で、 *_ViewImports.cshtml* Razor ページをインポートするファイル。</span><span class="sxs-lookup"><span data-stu-id="450b0-218">You can have `@inherits` in a *_ViewImports.cshtml* file that the Razor page imports.</span></span> <span data-ttu-id="450b0-219">たとえば、Razor ビューは、次をインポート*_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="450b0-219">For example, if your Razor view imported the following *_ViewImports.cshtml* file:</span></span>

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="450b0-220">次の厳密に型指定された Razor ページ</span><span class="sxs-lookup"><span data-stu-id="450b0-220">The following strongly typed Razor page</span></span>

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="450b0-221">この HTML マークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="450b0-221">Generates this HTML markup:</span></span>

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

<span data-ttu-id="450b0-222">渡されたときに"[Rick@contoso.com](mailto:Rick@contoso.com)"モデル。</span><span class="sxs-lookup"><span data-stu-id="450b0-222">When passed "[Rick@contoso.com](mailto:Rick@contoso.com)" in the model:</span></span>

   <span data-ttu-id="450b0-223">詳細については、「[Layout](layout.md)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="450b0-223">See [Layout](layout.md) for more information.</span></span>

### `@inject`

<span data-ttu-id="450b0-224">`@inject`ディレクティブを使用すると、サービスからの挿入、[サービス コンテナー](../../fundamentals/dependency-injection.md) Razor ページの使用にします。</span><span class="sxs-lookup"><span data-stu-id="450b0-224">The `@inject` directive enables you to inject a service from your [service container](../../fundamentals/dependency-injection.md)  into your Razor page for use.</span></span> <span data-ttu-id="450b0-225">参照してください[ビューに依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="450b0-225">See [Dependency injection into views](dependency-injection.md).</span></span>

<a name="functions"></a>

### `@functions`

<span data-ttu-id="450b0-226">`@functions`ディレクティブでは、Razor ページに関数レベルのコンテンツを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="450b0-226">The `@functions` directive enables you to add function level content to your Razor page.</span></span> <span data-ttu-id="450b0-227">構文は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="450b0-227">The syntax is:</span></span>

```none
@functions { // C# Code }
   ```

<span data-ttu-id="450b0-228">例:</span><span class="sxs-lookup"><span data-stu-id="450b0-228">For example:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="450b0-229">次の HTML マークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="450b0-229">Generates the following HTML markup:</span></span>

```none
<div>From method: Hello</div>
   ```

<span data-ttu-id="450b0-230">生成された Razor c# のようになります。</span><span class="sxs-lookup"><span data-stu-id="450b0-230">The generated Razor C# looks like:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

<span data-ttu-id="450b0-231">`@section`ディレクティブを組み合わせて使用、[レイアウト ページ](layout.md)を表示する HTML ページのさまざまな部分にコンテンツをレンダリングするビューを有効にします。</span><span class="sxs-lookup"><span data-stu-id="450b0-231">The `@section` directive is used in conjunction with the [layout page](layout.md) to enable views to render content in different parts of the rendered HTML page.</span></span> <span data-ttu-id="450b0-232">参照してください[セクション](layout.md#layout-sections-label)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="450b0-232">See [Sections](layout.md#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="450b0-233">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="450b0-233">Tag Helpers</span></span>

<span data-ttu-id="450b0-234">次[タグ ヘルパー](tag-helpers/index.md)ディレクティブが記載されているリンクで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="450b0-234">The following [Tag Helpers](tag-helpers/index.md) directives are detailed in the links provided.</span></span>

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a><span data-ttu-id="450b0-235">Razor の予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="450b0-235">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="450b0-236">Razor キーワード</span><span class="sxs-lookup"><span data-stu-id="450b0-236">Razor keywords</span></span>

* <span data-ttu-id="450b0-237">(ASP.NET Core 2.0 以降が必要) ページ</span><span class="sxs-lookup"><span data-stu-id="450b0-237">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="450b0-238">関数</span><span class="sxs-lookup"><span data-stu-id="450b0-238">functions</span></span>
* <span data-ttu-id="450b0-239">継承</span><span class="sxs-lookup"><span data-stu-id="450b0-239">inherits</span></span>
* <span data-ttu-id="450b0-240">モデル</span><span class="sxs-lookup"><span data-stu-id="450b0-240">model</span></span>
* <span data-ttu-id="450b0-241">section</span><span class="sxs-lookup"><span data-stu-id="450b0-241">section</span></span>
* <span data-ttu-id="450b0-242">ヘルパー (サポートしていません ASP.NET Core。)</span><span class="sxs-lookup"><span data-stu-id="450b0-242">helper   (Not supported by ASP.NET Core.)</span></span>

<span data-ttu-id="450b0-243">Razor キーワードでエスケープできます`@(Razor Keyword)`、たとえば`@(functions)`します。</span><span class="sxs-lookup"><span data-stu-id="450b0-243">Razor keywords can be escaped with `@(Razor Keyword)`, for example `@(functions)`.</span></span> <span data-ttu-id="450b0-244">以下の完全なサンプルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="450b0-244">See the complete sample below.</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="450b0-245">C# Razor キーワード</span><span class="sxs-lookup"><span data-stu-id="450b0-245">C# Razor keywords</span></span>

* <span data-ttu-id="450b0-246">case</span><span class="sxs-lookup"><span data-stu-id="450b0-246">case</span></span>
* <span data-ttu-id="450b0-247">do</span><span class="sxs-lookup"><span data-stu-id="450b0-247">do</span></span>
* <span data-ttu-id="450b0-248">default</span><span class="sxs-lookup"><span data-stu-id="450b0-248">default</span></span>
* <span data-ttu-id="450b0-249">for</span><span class="sxs-lookup"><span data-stu-id="450b0-249">for</span></span>
* <span data-ttu-id="450b0-250">foreach</span><span class="sxs-lookup"><span data-stu-id="450b0-250">foreach</span></span>
* <span data-ttu-id="450b0-251">if</span><span class="sxs-lookup"><span data-stu-id="450b0-251">if</span></span>
* <span data-ttu-id="450b0-252">else</span><span class="sxs-lookup"><span data-stu-id="450b0-252">else</span></span>
* <span data-ttu-id="450b0-253">lock</span><span class="sxs-lookup"><span data-stu-id="450b0-253">lock</span></span>
* <span data-ttu-id="450b0-254">switch</span><span class="sxs-lookup"><span data-stu-id="450b0-254">switch</span></span>
* <span data-ttu-id="450b0-255">try</span><span class="sxs-lookup"><span data-stu-id="450b0-255">try</span></span>
* <span data-ttu-id="450b0-256">catch</span><span class="sxs-lookup"><span data-stu-id="450b0-256">catch</span></span>
* <span data-ttu-id="450b0-257">finally</span><span class="sxs-lookup"><span data-stu-id="450b0-257">finally</span></span>
* <span data-ttu-id="450b0-258">使用</span><span class="sxs-lookup"><span data-stu-id="450b0-258">using</span></span>
* <span data-ttu-id="450b0-259">while</span><span class="sxs-lookup"><span data-stu-id="450b0-259">while</span></span>

<span data-ttu-id="450b0-260">C# Razor キーワードする必要がある二重でエスケープ`@(@C# Razor Keyword)`、たとえば`@(@case)`します。</span><span class="sxs-lookup"><span data-stu-id="450b0-260">C# Razor keywords need to be double escaped with `@(@C# Razor Keyword)`, for example `@(@case)`.</span></span> <span data-ttu-id="450b0-261">最初の`@`Razor パーサーをエスケープする 2 番目`@`c# パーサーをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="450b0-261">The first `@` escapes the Razor parser, the second `@` escapes the C# parser.</span></span> <span data-ttu-id="450b0-262">以下の完全なサンプルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="450b0-262">See the complete sample below.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="450b0-263">Razor で使用されていない予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="450b0-263">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="450b0-264">namespace</span><span class="sxs-lookup"><span data-stu-id="450b0-264">namespace</span></span>
* <span data-ttu-id="450b0-265">class</span><span class="sxs-lookup"><span data-stu-id="450b0-265">class</span></span>

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="450b0-266">ビューに対して生成された Razor c# クラスを表示します。</span><span class="sxs-lookup"><span data-stu-id="450b0-266">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="450b0-267">ASP.NET Core MVC プロジェクトに次のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="450b0-267">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

<span data-ttu-id="450b0-268">上書き、 `ICompilationService` MVC によって上記のクラスに追加</span><span class="sxs-lookup"><span data-stu-id="450b0-268">Override the `ICompilationService` added by MVC with the above class;</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

<span data-ttu-id="450b0-269">ブレークポイントを設定、`Compile`メソッドの`CustomCompilationService`とビュー`compilationContent`です。</span><span class="sxs-lookup"><span data-stu-id="450b0-269">Set a break point on the `Compile` method of `CustomCompilationService` and view `compilationContent`.</span></span>

![CompilationContent のテキスト ビジュアライザーのビュー](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="450b0-271">ビューの参照や大文字と小文字の区別</span><span class="sxs-lookup"><span data-stu-id="450b0-271">View lookups and case sensitivity</span></span>

<span data-ttu-id="450b0-272">Razor ビュー エンジンは、ビューを区別する検索を実行します。</span><span class="sxs-lookup"><span data-stu-id="450b0-272">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="450b0-273">ただし、実際の参照は、基になるソースによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="450b0-273">However, the actual lookup is determined by the underlying source:</span></span>

* <span data-ttu-id="450b0-274">ファイル ベースのソース:</span><span class="sxs-lookup"><span data-stu-id="450b0-274">File based source:</span></span> 

    * <span data-ttu-id="450b0-275">(Windows) などの大文字と小文字のファイル システムでのオペレーティング システムで物理ファイルのプロバイダーの参照は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="450b0-275">On operating systems with case insensitive file systems (like Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="450b0-276">たとえば`return View("Test")`になる`/Views/Home/Test.cshtml`、`/Views/home/test.cshtml`し、その他のすべての大文字と小文字のバリアントは発見します。</span><span class="sxs-lookup"><span data-stu-id="450b0-276">For example `return View("Test")` would result in `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` and all other casing variants would be discovered.</span></span>
    * <span data-ttu-id="450b0-277">Os X、Linux を含むシステムでは大文字小文字を区別ファイル、および`EmbeddedFileProvider`検索は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="450b0-277">On case sensitive file systems, which includes Linux, OSX and `EmbeddedFileProvider`, lookups are case sensitive.</span></span> <span data-ttu-id="450b0-278">たとえば、`return View("Test")`を具体的には検索`/Views/Home/Test.cshtml`です。</span><span class="sxs-lookup"><span data-stu-id="450b0-278">For example, `return View("Test")` would specifically look for `/Views/Home/Test.cshtml`.</span></span>
        
* <span data-ttu-id="450b0-279">プリコンパイル済みのビュー:</span><span class="sxs-lookup"><span data-stu-id="450b0-279">Precompiled views:</span></span>

   * <span data-ttu-id="450b0-280">ASP.Net Core 2.0 以降はすべてのオペレーティング システムで大文字と小文字がプリコンパイル済みのビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="450b0-280">With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="450b0-281">動作は、Windows 上の物理ファイルのプロバイダーの動作と同じです。</span><span class="sxs-lookup"><span data-stu-id="450b0-281">The behavior is identical to physical file provider's behavior on Windows.</span></span> 
   <span data-ttu-id="450b0-282">注: プリコンパイル済みの 2 つのビューが大文字と小文字が異なる場合、参照の結果は非決定的です。</span><span class="sxs-lookup"><span data-stu-id="450b0-282">Note: If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="450b0-283">開発者は、領域、コント ローラーとアクション名の大文字小文字の区別するファイルとディレクトリ名の大文字と小文字が一致することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="450b0-283">Developers are encouraged to match the casing of file and directory names to the casing of area, controller and action names.</span></span> <span data-ttu-id="450b0-284">これにより、デプロイの基になるファイル システムの柔軟なままを確認します。</span><span class="sxs-lookup"><span data-stu-id="450b0-284">This would ensure your deployments remain agnostic of the underlying file system.</span></span>
