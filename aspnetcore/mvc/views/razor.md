---
title: "ASP.NET Core の razor 構文のリファレンス"
author: guardrex
description: "サーバー ベースのコードを埋め込む web ページの Razor マークアップの構文について説明します。"
keywords: "ASP.NET Core、Razor、Razor ディレクティブ"
ms.author: riande
manager: wpickett
ms.date: 09/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 532e278597a0029b5bae93068af5b7b147c35688
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="f3bc0-104">ASP.NET Core の razor 構文</span><span class="sxs-lookup"><span data-stu-id="f3bc0-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="f3bc0-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Luke Latham](https://github.com/guardrex)、および[Taylor Mullen](https://twitter.com/ntaylormullen)</span><span class="sxs-lookup"><span data-stu-id="f3bc0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), and [Taylor Mullen](https://twitter.com/ntaylormullen)</span></span>

<span data-ttu-id="f3bc0-106">Razor とは、サーバー ベースのコードを埋め込む web ページのマークアップ構文です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="f3bc0-107">Razor 構文は、Razor マークアップ、c#、および HTML で構成されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="f3bc0-108">通常、Razor を含むファイルが、 *.cshtml*ファイル拡張子。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="f3bc0-109">HTML を表示</span><span class="sxs-lookup"><span data-stu-id="f3bc0-109">Rendering HTML</span></span>

<span data-ttu-id="f3bc0-110">Razor の既定の言語は、HTML です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-110">The default Razor language is HTML.</span></span> <span data-ttu-id="f3bc0-111">Razor マークアップからレンダリング HTML は、HTML ファイルから HTML をレンダリングと違いはありません。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="f3bc0-112">HTML マークアップを配置する場合、 *.cshtml*レンダリング変更されていないサーバーで Razor ファイル。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-112">If you place HTML markup into a *.cshtml* Razor file, it's rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="f3bc0-113">Razor 構文</span><span class="sxs-lookup"><span data-stu-id="f3bc0-113">Razor syntax</span></span>

<span data-ttu-id="f3bc0-114">Razor (C#) をサポートしを使用して、 `@` HTML を c# から移行する記号。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="f3bc0-115">Razor では、c# 式を評価し、それらを HTML 出力でレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="f3bc0-116">ときに、`@`シンボルが続く、 [Razor 予約キーワード](#razor-reserved-keywords)、Razor 固有のマークアップに移行します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="f3bc0-117">それ以外の場合、一般的な C# の場合に移行します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="f3bc0-118">エスケープする、`@`シンボル Razor マークアップを 1 秒間を使用して`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="f3bc0-119">コードが、1 つの HTML でレンダリング`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="f3bc0-120">HTML 属性と電子メール アドレスを含むコンテンツが処理されない、`@`遷移文字として記号。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="f3bc0-121">Razor 解析では、次の例では電子メール アドレスがそのまま残ります。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="f3bc0-122">Razor の暗黙的な式</span><span class="sxs-lookup"><span data-stu-id="f3bc0-122">Implicit Razor expressions</span></span>

<span data-ttu-id="f3bc0-123">Razor の暗黙的な式が始まる`@`c# コードを続けて。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="f3bc0-124">C# の場合を除く`await`キーワードで暗黙的な式はスペースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="f3bc0-125">C# ステートメントにクリア終了がある場合は、スペースを intermingle できます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-125">You can intermingle spaces if the C# statement has a clear ending:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="f3bc0-126">Razor の明示的な式</span><span class="sxs-lookup"><span data-stu-id="f3bc0-126">Explicit Razor expressions</span></span>

<span data-ttu-id="f3bc0-127">Razor の明示的な式から成る、`@`バランスの取れたかっこ記号。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-127">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="f3bc0-128">過去 1 週間の時間を表示するために、次の Razor マークアップを使用します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-128">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="f3bc0-129">内のコンテンツ、`@()`かっこが評価され、出力に表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-129">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="f3bc0-130">一般に、前のセクションで説明されている、暗黙的な式は、スペースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-130">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="f3bc0-131">次のコードでは、1 週間は現在の時間から減算されていません。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-131">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="f3bc0-132">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-132">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="f3bc0-133">Expression の結果の文字列を連結するのに明示的な式を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-133">You can use an explicit expression to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="f3bc0-134">明示的の式がない`<p>Age@joe.Age</p>`、電子メール アドレスとして扱われるおよび`<p>Age@joe.Age</p>`が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-134">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="f3bc0-135">明示的な式として書き込まれるときに`<p>Age33</p>`が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-135">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="f3bc0-136">式のエンコード</span><span class="sxs-lookup"><span data-stu-id="f3bc0-136">Expression encoding</span></span>

<span data-ttu-id="f3bc0-137">文字列に評価される c# 式は、HTML エンコードです。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-137">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="f3bc0-138">C# 式の評価が`IHtmlContent`経由で直接レンダリング`IHtmlContent.WriteTo`です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-138">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="f3bc0-139">C# 式の評価がない`IHtmlContent`によって文字列に変換`ToString`描画する前にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-139">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="f3bc0-140">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-140">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="f3bc0-141">としてブラウザーで HTML が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-141">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="f3bc0-142">`HtmlHelper.Raw`出力では、エンコードされたが、HTML マークアップとしてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-142">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="f3bc0-143">使用して`HtmlHelper.Raw`unsanitized ユーザー入力は、セキュリティ上のリスクです。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-143">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="f3bc0-144">ユーザー入力は、悪意のある JavaScript または他の攻撃に含まれる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-144">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="f3bc0-145">ユーザー入力をサニタイズしすることは困難です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-145">Sanitizing user input is difficult.</span></span> <span data-ttu-id="f3bc0-146">使用しないでください`HtmlHelper.Raw`ユーザー入力とします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-146">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="f3bc0-147">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-147">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="f3bc0-148">Razor コードのブロック</span><span class="sxs-lookup"><span data-stu-id="f3bc0-148">Razor code blocks</span></span>

<span data-ttu-id="f3bc0-149">Razor コード ブロックが始まる`@`で囲まれたと`{}`です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-149">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="f3bc0-150">式とは異なり、コード ブロック内の c# コードは表示されません。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-150">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="f3bc0-151">コード ブロックと、ビューの式は、同じスコープの共有の順序で定義されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-151">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="f3bc0-152">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-152">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="f3bc0-153">暗黙の切り替え</span><span class="sxs-lookup"><span data-stu-id="f3bc0-153">Implicit transitions</span></span>

<span data-ttu-id="f3bc0-154">既定の言語コード ブロックでは、C# の場合が、HTML に移行することができます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-154">The default language in a code block is C#, but you can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="f3bc0-155">明示的な区切り記号付き遷移</span><span class="sxs-lookup"><span data-stu-id="f3bc0-155">Explicit delimited transition</span></span>

<span data-ttu-id="f3bc0-156">HTML を描画するコード ブロックのサブ セクションを定義するのには、Razor で表示するための文字を囲む**\<テキスト >**タグ。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-156">To define a sub-section of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="f3bc0-157">HTML タグで囲まれていない HTML をレンダリングするときは、この方法を使用します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-157">Use this approach when you want to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="f3bc0-158">HTML または Razor のタグのない Razor 実行時エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-158">Without an HTML or Razor tag, you receive a Razor runtime error.</span></span>

<span data-ttu-id="f3bc0-159">**\<テキスト >**タグもコンテンツを表示するときに空白を制御する便利です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-159">The **\<text>** tag is also useful to control whitespace when rendering content.</span></span> <span data-ttu-id="f3bc0-160">間のコンテンツのみ、 **\<テキスト >**のタグをレンダリングすると、しの前後に空白、 **\<テキスト >**タグは HTML 出力で表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-160">Only the content between the **\<text>** tags is rendered, and no whitespace before or after the **\<text>** tags appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="f3bc0-161">明示的な行の遷移と @。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-161">Explicit Line Transition with @:</span></span>

<span data-ttu-id="f3bc0-162">コード ブロックの内部 HTML として残りの行全体を表示を使用して、`@:`構文。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-162">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="f3bc0-163">なし、 `@:` Razor ランタイム エラーが発生するコードでは、します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-163">Without the `@:` in the code, you recieve a Razor runtime error.</span></span>

## <a name="control-structures"></a><span data-ttu-id="f3bc0-164">制御構造</span><span class="sxs-lookup"><span data-stu-id="f3bc0-164">Control Structures</span></span>

<span data-ttu-id="f3bc0-165">制御構造体は、コード ブロックの拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-165">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="f3bc0-166">コード ブロック (マークアップ、インライン c# への遷移) ものすべての側面は、次の構造体に適用されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-166">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="f3bc0-167">条件付き@if、一致しない場合、#else、および@switch</span><span class="sxs-lookup"><span data-stu-id="f3bc0-167">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="f3bc0-168">`@if`コードの実行時にコントロール:</span><span class="sxs-lookup"><span data-stu-id="f3bc0-168">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="f3bc0-169">`else`および`else if`を必要としない、`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-169">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="f3bc0-170">次のようにスイッチ ステートメントを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-170">You can use a switch statement like this:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="f3bc0-171">ループ@for、 @foreach、 @while、および@do中</span><span class="sxs-lookup"><span data-stu-id="f3bc0-171">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="f3bc0-172">コントロール ステートメントのループで template 宣言された HTML をレンダリングすることができます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-172">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="f3bc0-173">人のユーザーの一覧を表示するには。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-173">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="f3bc0-174">次のループ ステートメントのいずれかの操作を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-174">You can use any of the following looping statements:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
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

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="f3bc0-175">複合@using</span><span class="sxs-lookup"><span data-stu-id="f3bc0-175">Compound @using</span></span>

<span data-ttu-id="f3bc0-176">C# で、`using`オブジェクトが破棄されることを確認するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-176">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="f3bc0-177">Razor では、同じメカニズムを使用してを追加のコンテンツを含む HTML ヘルパーを作成します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-177">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="f3bc0-178">インスタンスを含む form タグを表示するために HTML ヘルパーを利用できる、`@using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-178">For instance, you can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="f3bc0-179">スコープ レベルのアクションを実行することもできます。[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-179">You can also perform scope-level actions with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="f3bc0-180">@try、catch、finally</span><span class="sxs-lookup"><span data-stu-id="f3bc0-180">@try, catch, finally</span></span>

<span data-ttu-id="f3bc0-181">例外処理は、C# の場合に似ています。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-181">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="f3bc0-182">Razor では、クリティカル セクション lock ステートメントを保護する機能があります。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-182">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="f3bc0-183">コメント</span><span class="sxs-lookup"><span data-stu-id="f3bc0-183">Comments</span></span>

<span data-ttu-id="f3bc0-184">Razor には、c# と HTML のコメントがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-184">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="f3bc0-185">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-185">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="f3bc0-186">Razor コメントは、web ページが表示される前に、サーバーによって削除されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-186">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="f3bc0-187">Razor を使用して`@*  *@`コメントを区切るためにします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-187">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="f3bc0-188">次のコードはコメント アウトされて、サーバーは、マークアップを表示しないようにします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-188">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="f3bc0-189">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="f3bc0-189">Directives</span></span>

<span data-ttu-id="f3bc0-190">Razor ディレクティブが次の予約済みキーワードからの暗黙的な式で表される、`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-190">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="f3bc0-191">ディレクティブは、通常ビューは解析または別の機能を有効にする方法を変更します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-191">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="f3bc0-192">Razor がビューのコードを生成する方法を理解しやすくディレクティブの動作を理解します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-192">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="f3bc0-193">コードでは、次のようなクラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-193">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="f3bc0-194">この記事のセクションで後述[ビューに対して生成された Razor c# クラスを表示する](#viewing-the-razor-c-class-generated-for-a-view)この生成されたクラスを表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-194">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="f3bc0-195">`@using`ディレクティブを追加、c#`using`ディレクティブを生成されたビューに。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-195">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="f3bc0-196">`@model`ディレクティブがビューに渡されたモデルの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-196">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="f3bc0-197">個々 のユーザー アカウントを持つ ASP.NET Core MVC アプリを作成する場合、 *Views/Account/Login.cshtml*ビューには、次のモデルの宣言が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-197">If you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="f3bc0-198">生成されたクラスから継承`RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="f3bc0-198">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="f3bc0-199">Razor の公開、`Model`ビューに渡されるモデルにアクセスするためのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-199">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="f3bc0-200">`@model`ディレクティブは、このプロパティの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-200">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="f3bc0-201">ディレクティブを指定します、`T`で`RazorPage<T>`を生成するクラスから派生して、表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-201">The directive specifies the `T` in `RazorPage<T>` that the generated class that your view derives from.</span></span> <span data-ttu-id="f3bc0-202">指定しない場合は、`@model`ディレクティブ、`Model`プロパティの型は`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-202">If you don't specify the `@model` directive, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="f3bc0-203">モデルの値は、コント ローラーからビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-203">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="f3bc0-204">参照してください[モデルを厳密に型指定と@modelキーワード](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-204">See [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) for more information.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="f3bc0-205">`@inherits`ディレクティブには、ビューが継承するクラスのフル コントロールが提供します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-205">The `@inherits` directive gives you full control of the class your view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="f3bc0-206">カスタム Razor ページの種類を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-206">The following is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="f3bc0-207">`CustomText`ビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-207">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="f3bc0-208">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-208">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

<span data-ttu-id="f3bc0-209">使用することはできません`@model`と`@inherits`同じビューにします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-209">You can't use `@model` and `@inherits` in the same view.</span></span> <span data-ttu-id="f3bc0-210">した`@inherits`で、 *_ViewImports.cshtml*ビューをインポートするファイル。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-210">You can have `@inherits` in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="f3bc0-211">厳密に型指定されたビューの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-211">The following is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="f3bc0-212">場合"rick@contoso.com"渡されるモデルでは、ビューには、次の HTML マークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-212">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="f3bc0-213">`@inject`ディレクティブを使用すると、サービスからの挿入、[サービス コンテナー](xref:fundamentals/dependency-injection)ビュー内にします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-213">The `@inject` directive enables you to inject a service from your [service container](xref:fundamentals/dependency-injection) into your view.</span></span> <span data-ttu-id="f3bc0-214">参照してください[ビューに依存性の注入](xref:mvc/views/dependency-injection)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-214">See [Dependency injection into views](xref:mvc/views/dependency-injection) for more information.</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="f3bc0-215">`@functions`ディレクティブでは、関数レベルのコンテンツ ビューを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-215">The `@functions` directive enables you to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="f3bc0-216">例:</span><span class="sxs-lookup"><span data-stu-id="f3bc0-216">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="f3bc0-217">コードでは、次の HTML マークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-217">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="f3bc0-218">次のコードでは、生成された Razor c# クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-218">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="f3bc0-219">`@section`ディレクティブを組み合わせて使用、[レイアウト](xref:mvc/views/layout)HTML ページのさまざまな部分にコンテンツをレンダリングするビューを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-219">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="f3bc0-220">参照してください[セクション](xref:mvc/views/layout#layout-sections-label)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-220">See [Sections](xref:mvc/views/layout#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="f3bc0-221">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="f3bc0-221">Tag Helpers</span></span>

<span data-ttu-id="f3bc0-222">関連する次の 3 つのディレクティブがある[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-222">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="f3bc0-223">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="f3bc0-223">Directive</span></span> | <span data-ttu-id="f3bc0-224">関数</span><span class="sxs-lookup"><span data-stu-id="f3bc0-224">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="f3bc0-225">タグ ヘルパーを可能にすると表示されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-225">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="f3bc0-226">以前に追加したビューからタグ ヘルパーを削除します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-226">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="f3bc0-227">タグ ヘルパーのサポートを有効にして、タグ ヘルパーの使用法を明確にするのには、タグ プレフィックスを指定します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-227">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="f3bc0-228">Razor の予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="f3bc0-228">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="f3bc0-229">Razor キーワード</span><span class="sxs-lookup"><span data-stu-id="f3bc0-229">Razor keywords</span></span>

* <span data-ttu-id="f3bc0-230">(ASP.NET Core 2.0 以降が必要) ページ</span><span class="sxs-lookup"><span data-stu-id="f3bc0-230">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="f3bc0-231">関数</span><span class="sxs-lookup"><span data-stu-id="f3bc0-231">functions</span></span>
* <span data-ttu-id="f3bc0-232">継承</span><span class="sxs-lookup"><span data-stu-id="f3bc0-232">inherits</span></span>
* <span data-ttu-id="f3bc0-233">モデル</span><span class="sxs-lookup"><span data-stu-id="f3bc0-233">model</span></span>
* <span data-ttu-id="f3bc0-234">section</span><span class="sxs-lookup"><span data-stu-id="f3bc0-234">section</span></span>
* <span data-ttu-id="f3bc0-235">ヘルパー (ASP.NET Core ではサポートされていない)</span><span class="sxs-lookup"><span data-stu-id="f3bc0-235">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="f3bc0-236">Razor のキーワードでエスケープ`@(Razor Keyword)`(たとえば、 `@(functions)`)。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-236">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="f3bc0-237">C# Razor キーワード</span><span class="sxs-lookup"><span data-stu-id="f3bc0-237">C# Razor keywords</span></span>

* <span data-ttu-id="f3bc0-238">case</span><span class="sxs-lookup"><span data-stu-id="f3bc0-238">case</span></span>
* <span data-ttu-id="f3bc0-239">do</span><span class="sxs-lookup"><span data-stu-id="f3bc0-239">do</span></span>
* <span data-ttu-id="f3bc0-240">default</span><span class="sxs-lookup"><span data-stu-id="f3bc0-240">default</span></span>
* <span data-ttu-id="f3bc0-241">for</span><span class="sxs-lookup"><span data-stu-id="f3bc0-241">for</span></span>
* <span data-ttu-id="f3bc0-242">foreach</span><span class="sxs-lookup"><span data-stu-id="f3bc0-242">foreach</span></span>
* <span data-ttu-id="f3bc0-243">if</span><span class="sxs-lookup"><span data-stu-id="f3bc0-243">if</span></span>
* <span data-ttu-id="f3bc0-244">else</span><span class="sxs-lookup"><span data-stu-id="f3bc0-244">else</span></span>
* <span data-ttu-id="f3bc0-245">lock</span><span class="sxs-lookup"><span data-stu-id="f3bc0-245">lock</span></span>
* <span data-ttu-id="f3bc0-246">switch</span><span class="sxs-lookup"><span data-stu-id="f3bc0-246">switch</span></span>
* <span data-ttu-id="f3bc0-247">try</span><span class="sxs-lookup"><span data-stu-id="f3bc0-247">try</span></span>
* <span data-ttu-id="f3bc0-248">catch</span><span class="sxs-lookup"><span data-stu-id="f3bc0-248">catch</span></span>
* <span data-ttu-id="f3bc0-249">finally</span><span class="sxs-lookup"><span data-stu-id="f3bc0-249">finally</span></span>
* <span data-ttu-id="f3bc0-250">使用</span><span class="sxs-lookup"><span data-stu-id="f3bc0-250">using</span></span>
* <span data-ttu-id="f3bc0-251">while</span><span class="sxs-lookup"><span data-stu-id="f3bc0-251">while</span></span>

<span data-ttu-id="f3bc0-252">C# Razor キーワードでダブル エスケープする必要があります`@(@C# Razor Keyword)`(たとえば、 `@(@case)`)。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-252">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="f3bc0-253">最初の`@`Razor パーサーをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-253">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="f3bc0-254">2 番目`@`c# パーサーをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-254">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="f3bc0-255">Razor で使用されていない予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="f3bc0-255">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="f3bc0-256">namespace</span><span class="sxs-lookup"><span data-stu-id="f3bc0-256">namespace</span></span>
* <span data-ttu-id="f3bc0-257">class</span><span class="sxs-lookup"><span data-stu-id="f3bc0-257">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="f3bc0-258">ビューに対して生成された Razor c# クラスを表示します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-258">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="f3bc0-259">ASP.NET Core MVC プロジェクトに次のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-259">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="f3bc0-260">上書き、`RazorTemplateEngine`での MVC によって追加された、`CustomTemplateEngine`クラス。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-260">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="f3bc0-261">ブレークポイントの設定、`return csharpDocument`ステートメントの`CustomTemplateEngine`します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-261">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="f3bc0-262">プログラムの実行がブレークポイントで停止したときの値を表示`generatedCode`です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-262">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![GeneratedCode のテキスト ビジュアライザーのビュー](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="f3bc0-264">ビューの参照や大文字と小文字の区別</span><span class="sxs-lookup"><span data-stu-id="f3bc0-264">View lookups and case sensitivity</span></span>

<span data-ttu-id="f3bc0-265">Razor ビュー エンジンは、ビューを区別する検索を実行します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-265">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="f3bc0-266">ただし、実際の参照は、基になるファイル システムによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-266">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="f3bc0-267">ファイル ベースのソース:</span><span class="sxs-lookup"><span data-stu-id="f3bc0-267">File based source:</span></span> 
  * <span data-ttu-id="f3bc0-268">大文字と小文字のファイル システム (Windows など) でのオペレーティング システムで物理ファイルのプロバイダーの参照は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-268">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="f3bc0-269">たとえば、`return View("Test")`に一致する結果*/Views/Home/Test.cshtml*、 */Views/home/test.cshtml*、およびその他の文字種バリアント。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-269">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="f3bc0-270">大文字と小文字のファイル システムで (たとえば、Linux、os X を使用して`EmbeddedFileProvider`)、検索は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-270">On case sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case sensitive.</span></span> <span data-ttu-id="f3bc0-271">たとえば、`return View("Test")`具体的には一致*/Views/Home/Test.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-271">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="f3bc0-272">ビューをプリコンパイル: ASP.Net Core 2.0 以降ではすべてのオペレーティング システムで大文字と小文字はプリコンパイル済みのビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-272">Precompiled views: With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="f3bc0-273">動作は、Windows 上の物理ファイルのプロバイダーの動作と同じです。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-273">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="f3bc0-274">プリコンパイル済みの 2 つのビューが大文字と小文字が異なる場合、参照の結果は非決定的です。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-274">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="f3bc0-275">開発者は、領域、コント ローラー、およびアクションの名前の大文字と小文字をファイルおよびディレクトリ名の大文字と小文字が一致することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-275">Developers are encouraged to match the casing of file and directory names to the casing of area, controller, and action names.</span></span> <span data-ttu-id="f3bc0-276">これにより、デプロイは、基になるファイル システムに関係なく、ビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="f3bc0-276">This ensures your deployments will find their views regardless of the underlying file system.</span></span>
