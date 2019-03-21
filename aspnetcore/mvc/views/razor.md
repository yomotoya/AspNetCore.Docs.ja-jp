---
title: ASP.NET Core の Razor 構文リファレンス
author: rick-anderson
description: Web ページにサーバー ベースのコードを埋め込むための Razor マークアップの構文について説明します。
ms.author: riande
ms.date: 10/26/2018
uid: mvc/views/razor
ms.openlocfilehash: 254c85ee9e74dc72170b19d27fbc5f1ae7ccd3dc
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264755"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="306cd-103">ASP.NET Core の Razor 構文リファレンス</span><span class="sxs-lookup"><span data-stu-id="306cd-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="306cd-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Luke Latham](https://github.com/guardrex)、[Taylor Mullen](https://twitter.com/ntaylormullen)、[Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="306cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="306cd-105">Razor は、Web ページにサーバー ベースのコードを埋め込むためのマークアップ構文です。</span><span class="sxs-lookup"><span data-stu-id="306cd-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="306cd-106">Razor 構文は、Razor マークアップ、C#、HTML で構成されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="306cd-107">通常、Razor を含むファイルのファイル拡張子は *.cshtml* です。</span><span class="sxs-lookup"><span data-stu-id="306cd-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="306cd-108">HTML のレンダリング</span><span class="sxs-lookup"><span data-stu-id="306cd-108">Rendering HTML</span></span>

<span data-ttu-id="306cd-109">Razor の既定の言語は HTML です。</span><span class="sxs-lookup"><span data-stu-id="306cd-109">The default Razor language is HTML.</span></span> <span data-ttu-id="306cd-110">Razor マークアップからの HTML のレンダリングは、HTML ファイルからの HTML のレンダリングと同じです。</span><span class="sxs-lookup"><span data-stu-id="306cd-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="306cd-111">*.cshtml* Razor ファイル内の HTML マークアップは、サーバーによって変更されずにレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="306cd-112">Razor の構文</span><span class="sxs-lookup"><span data-stu-id="306cd-112">Razor syntax</span></span>

<span data-ttu-id="306cd-113">Razor は C# をサポートし、`@` 記号を使って HTML から C# に移行します。</span><span class="sxs-lookup"><span data-stu-id="306cd-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="306cd-114">Razor は C# の式を評価し、それらを HTML の出力でレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="306cd-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="306cd-115">`@` 記号の後に [Razor の予約済みキーワード](#razor-reserved-keywords)が続いている場合は、Razor 固有のマークアップに移行します。</span><span class="sxs-lookup"><span data-stu-id="306cd-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="306cd-116">それ以外の場合は、普通の C# に移行します。</span><span class="sxs-lookup"><span data-stu-id="306cd-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="306cd-117">Razor マークアップで `@` 記号をエスケープするには、`@` 記号をもう 1 つ使います。</span><span class="sxs-lookup"><span data-stu-id="306cd-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="306cd-118">HTML では、コードは 1 つの `@` 記号でレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="306cd-119">メール アドレスを含む HTML の属性とコンテンツは、`@` 記号を遷移文字として扱いません。</span><span class="sxs-lookup"><span data-stu-id="306cd-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="306cd-120">次の例のメール アドレスは、Razor の解析ではそのまま残ります。</span><span class="sxs-lookup"><span data-stu-id="306cd-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="306cd-121">暗黙的な Razor 式</span><span class="sxs-lookup"><span data-stu-id="306cd-121">Implicit Razor expressions</span></span>

<span data-ttu-id="306cd-122">Razor の暗黙的な式は、`@` で始まって C# のコードが続きます。</span><span class="sxs-lookup"><span data-stu-id="306cd-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="306cd-123">C# の `await` キーワードを除き、暗黙的な式にスペースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="306cd-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="306cd-124">C# のステートメントに明確な終わりがある場合は、スペースを混在させることができます。</span><span class="sxs-lookup"><span data-stu-id="306cd-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="306cd-125">暗黙的な式では、山かっこ (`<>`) の内側の文字は HTML タグとして解釈されるため、C# ジェネリックを含めることは**できません**。</span><span class="sxs-lookup"><span data-stu-id="306cd-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="306cd-126">次のコードは有効では**ありません**。</span><span class="sxs-lookup"><span data-stu-id="306cd-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="306cd-127">上記のコードでは、次のいずれかのようなコンパイラ エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-127">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="306cd-128">The "int" element wasn't closed.</span><span class="sxs-lookup"><span data-stu-id="306cd-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="306cd-129">All elements must be either self-closing or have a matching end tag. ("int" 要素が閉じられませんでした。すべての要素は、自己終了するか、対応する終了タグが存在する必要があります。)</span><span class="sxs-lookup"><span data-stu-id="306cd-129">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="306cd-130">メソッド グループ 'GenericMethod' を非デリゲート型 'object' に変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="306cd-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="306cd-131">このメソッドを呼び出しますか?</span><span class="sxs-lookup"><span data-stu-id="306cd-131">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="306cd-132">ジェネリック メソッドの呼び出しは、[明示的な Razor 式](#explicit-razor-expressions)または [Razor コード ブロック](#razor-code-blocks)にラップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="306cd-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="306cd-133">明示的な Razor 式</span><span class="sxs-lookup"><span data-stu-id="306cd-133">Explicit Razor expressions</span></span>

<span data-ttu-id="306cd-134">明示的な Razor 式は、`@` 記号とバランスの取れたかっこ記号で構成されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="306cd-135">1 週間前の時刻を表示するには、次の Razor マークアップを使います。</span><span class="sxs-lookup"><span data-stu-id="306cd-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="306cd-136">`@()` のかっこ内の内容が評価されて、出力にレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="306cd-137">前のセクションで説明した暗黙的な式は、一般に、スペースを含むことはできません。</span><span class="sxs-lookup"><span data-stu-id="306cd-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="306cd-138">次のコードでは、現在時刻から 1 週間は減算されません。</span><span class="sxs-lookup"><span data-stu-id="306cd-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="306cd-139">このコードでは、次のような HTML がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="306cd-140">明示的な式を使うと、テキストと式の結果を連結できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="306cd-141">明示的な式を使わないと、`<p>Age@joe.Age</p>` はメール アドレスとして扱われ、`<p>Age@joe.Age</p>` がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="306cd-142">明示的な式として記述すると、`<p>Age33</p>` がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="306cd-143">明示的な式を使うと、ジェネリック メソッドから *.cshtml* ファイルに出力できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="306cd-144">次のマークアップは、C# ジェネリックの山かっこによって発生した前述のエラーを修正する方法について示します。</span><span class="sxs-lookup"><span data-stu-id="306cd-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="306cd-145">コードは明示的な式として書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="306cd-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="306cd-146">式のエンコード</span><span class="sxs-lookup"><span data-stu-id="306cd-146">Expression encoding</span></span>

<span data-ttu-id="306cd-147">文字列として評価される C# の式は、HTML でエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="306cd-148">`IHtmlContent` として評価される C# の式は、`IHtmlContent.WriteTo` によって直接レンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="306cd-149">`IHtmlContent` として評価されない C# の式は、`ToString` によって文字列に変換され、レンダリングされる前にエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="306cd-150">このコードでは、次のような HTML がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="306cd-151">HTML はブラウザーで次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="306cd-152">`HtmlHelper.Raw` の出力はエンコードされませんが、HTML マークアップとしてレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="306cd-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="306cd-153">サニタイズされていないユーザー入力で `HtmlHelper.Raw` を使うと、セキュリティ上のリスクがあります。</span><span class="sxs-lookup"><span data-stu-id="306cd-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="306cd-154">ユーザー入力には、悪意のある JavaScript または他の攻撃が含まれる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="306cd-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="306cd-155">ユーザー入力をサニタイズすることは困難です。</span><span class="sxs-lookup"><span data-stu-id="306cd-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="306cd-156">ユーザー入力では `HtmlHelper.Raw` を使わないでください。</span><span class="sxs-lookup"><span data-stu-id="306cd-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="306cd-157">このコードでは、次のような HTML がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="306cd-158">Razor コード ブロック</span><span class="sxs-lookup"><span data-stu-id="306cd-158">Razor code blocks</span></span>

<span data-ttu-id="306cd-159">Razor コード ブロックは、`@` で始まり、`{}` で囲まれています。</span><span class="sxs-lookup"><span data-stu-id="306cd-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="306cd-160">式とは異なり、コード ブロック内の C# コードはレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="306cd-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="306cd-161">ビュー内のコード ブロックと式は同じスコープを共有し、次の順序で定義されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="306cd-162">このコードでは、次のような HTML がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="306cd-163">暗黙の移行</span><span class="sxs-lookup"><span data-stu-id="306cd-163">Implicit transitions</span></span>

<span data-ttu-id="306cd-164">コード ブロック内の既定の言語は C# ですが、Razor ページは HTML に移行して戻ることがあります。</span><span class="sxs-lookup"><span data-stu-id="306cd-164">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="306cd-165">明示的に区切られた遷移</span><span class="sxs-lookup"><span data-stu-id="306cd-165">Explicit delimited transition</span></span>

<span data-ttu-id="306cd-166">HTML をレンダリングする必要のあるコード ブロックのサブセクションを定義するには、レンダリングする文字を Razor の **\<text>** タグで囲みます。</span><span class="sxs-lookup"><span data-stu-id="306cd-166">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="306cd-167">HTML タグによって囲まれていない HTML をレンダリングするには、この方法を使います。</span><span class="sxs-lookup"><span data-stu-id="306cd-167">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="306cd-168">HTML タグまたは Razor タグがない場合、Razor ラインタイム エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="306cd-168">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="306cd-169">**\<text>** タグは、内容をレンダリングするときに空白文字を制御するのに便利です。</span><span class="sxs-lookup"><span data-stu-id="306cd-169">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="306cd-170">**\<text>** タグの間の内容だけがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-170">Only the content between the **\<text>** tag is rendered.</span></span>
* <span data-ttu-id="306cd-171">**\<text>** タグの前後にある空白文字は HTML の出力には表示されません。</span><span class="sxs-lookup"><span data-stu-id="306cd-171">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="306cd-172">@: による明示的な行の遷移</span><span class="sxs-lookup"><span data-stu-id="306cd-172">Explicit Line Transition with @:</span></span>

<span data-ttu-id="306cd-173">残りの行全体をコード ブロック内に HTML としてレンダリングするには、`@:` 構文を使います。</span><span class="sxs-lookup"><span data-stu-id="306cd-173">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="306cd-174">コードに `@:` がないと、Razor ランタイム エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-174">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="306cd-175">警告 :Razor ファイルに余分な `@` 文字があると、ブロックの後続のステートメントでコンパイラ エラーが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="306cd-175">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="306cd-176">これらのコンパイラ エラーは、実際のエラーは報告されたエラーより前で発生しているため、理解するのが難しい場合があります。</span><span class="sxs-lookup"><span data-stu-id="306cd-176">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="306cd-177">このエラーは、複数の暗黙的/明示的な式を 1 つのコード ブロックに結合した後で発生することがよくあります。</span><span class="sxs-lookup"><span data-stu-id="306cd-177">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="306cd-178">制御構造</span><span class="sxs-lookup"><span data-stu-id="306cd-178">Control structures</span></span>

<span data-ttu-id="306cd-179">制御構造は、コード ブロックの拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="306cd-179">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="306cd-180">コード ブロックのすべての側面 (マークアップへの遷移、インライン C# ) が、次の構造にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-180">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="306cd-181">条件付き @if、else if、else、@switch</span><span class="sxs-lookup"><span data-stu-id="306cd-181">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="306cd-182">`@if` は、いつコードを実行するかを制御します。</span><span class="sxs-lookup"><span data-stu-id="306cd-182">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="306cd-183">`else` および `else if` には、`@` 記号は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="306cd-183">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="306cd-184">次のマークアップでは、switch ステートメントの使い方を示します。</span><span class="sxs-lookup"><span data-stu-id="306cd-184">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="306cd-185">@for、@foreach、@while、@do while のループ</span><span class="sxs-lookup"><span data-stu-id="306cd-185">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="306cd-186">ループ制御ステートメントを使って、テンプレート化された HTML をレンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="306cd-186">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="306cd-187">人の一覧をレンダリングするには:</span><span class="sxs-lookup"><span data-stu-id="306cd-187">To render a list of people:</span></span>

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

<span data-ttu-id="306cd-188">以下のループ ステートメントがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="306cd-188">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="306cd-189">複合 @using</span><span class="sxs-lookup"><span data-stu-id="306cd-189">Compound @using</span></span>

<span data-ttu-id="306cd-190">C# では、オブジェクトを確実に破棄するために `using` オブジェクトが使われています。</span><span class="sxs-lookup"><span data-stu-id="306cd-190">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="306cd-191">Razor では、同じメカニズムが、追加コンテンツを含む HTML ヘルパーを作成するために使われています。</span><span class="sxs-lookup"><span data-stu-id="306cd-191">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="306cd-192">次のコードの HTML ヘルパーは、`@using` ステートメントを含むフォーム タグをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="306cd-192">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="306cd-193">スコープ レベルのアクションは、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使って実行できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-193">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="306cd-194">@try、catch、finally</span><span class="sxs-lookup"><span data-stu-id="306cd-194">@try, catch, finally</span></span>

<span data-ttu-id="306cd-195">例外処理は C# に似ています。</span><span class="sxs-lookup"><span data-stu-id="306cd-195">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="306cd-196">Razor には、重要なセクションを lock ステートメントで保護する機能があります。</span><span class="sxs-lookup"><span data-stu-id="306cd-196">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="306cd-197">コメント</span><span class="sxs-lookup"><span data-stu-id="306cd-197">Comments</span></span>

<span data-ttu-id="306cd-198">Razor は、C# と HTML のコメントをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="306cd-198">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="306cd-199">このコードでは、次のような HTML がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-199">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="306cd-200">Razor のコメントは、Web ページがレンダリングされる前に、サーバーによって削除されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-200">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="306cd-201">Razor では、`@*  *@` を使ってコメントを区切ります。</span><span class="sxs-lookup"><span data-stu-id="306cd-201">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="306cd-202">次のコードはコメント化されているため、サーバーはどのマークアップもレンダリングしません。</span><span class="sxs-lookup"><span data-stu-id="306cd-202">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="306cd-203">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="306cd-203">Directives</span></span>

<span data-ttu-id="306cd-204">Razor のディレクティブは、`@` 記号の後の予約キーワードによる暗黙的な式で表されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-204">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="306cd-205">通常、ディレクティブは、ビューの解析方法を変更したり、異なる機能を有効にしたりします。</span><span class="sxs-lookup"><span data-stu-id="306cd-205">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="306cd-206">Razor がビューのコードを生成する方法を理解すると、ディレクティブの動作を理解しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="306cd-206">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="306cd-207">上のコードでは、次のようなクラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-207">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="306cd-208">後の「[ビューに対して生成された Razor C# クラスを調べる](#inspect-the-razor-c-class-generated-for-a-view)」セクションでは、この生成されたクラスを表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="306cd-208">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>

### <a name="using"></a>@using

<span data-ttu-id="306cd-209">`@using` ディレクティブは、生成されるビューに C# の `using` ディレクティブを追加します。</span><span class="sxs-lookup"><span data-stu-id="306cd-209">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="306cd-210">`@model` ディレクティブは、ビューに渡されるモデルの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="306cd-210">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="306cd-211">個々のユーザー アカウントで作成される ASP.NET Core MVC アプリの *Views/Account/Login.cshtml* ビューには、次のモデル宣言が含まれます。</span><span class="sxs-lookup"><span data-stu-id="306cd-211">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="306cd-212">生成されるクラスは、`RazorPage<dynamic>` を継承します。</span><span class="sxs-lookup"><span data-stu-id="306cd-212">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="306cd-213">Razor では、ビューに渡されるモデルにアクセスするための `Model` プロパティが公開されています。</span><span class="sxs-lookup"><span data-stu-id="306cd-213">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="306cd-214">`@model` ディレクティブは、このプロパティの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="306cd-214">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="306cd-215">ディレクティブでは、ビューが派生する生成されたクラスの `T` を `RazorPage<T>` で指定します。</span><span class="sxs-lookup"><span data-stu-id="306cd-215">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="306cd-216">`@model` ディレクティブが指定されていない場合、`Model` プロパティは `dynamic` 型になります。</span><span class="sxs-lookup"><span data-stu-id="306cd-216">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="306cd-217">モデルの値は、コントローラーからビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-217">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="306cd-218">詳細については、「[厳密に型指定されたモデルと &commat;model キーワード](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="306cd-218">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="306cd-219">`@inherits` ディレクティブは、ビューが継承するクラスの完全な制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="306cd-219">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="306cd-220">次のコードは、カスタム Razor ページ型です。</span><span class="sxs-lookup"><span data-stu-id="306cd-220">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="306cd-221">`CustomText` がビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-221">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="306cd-222">このコードでは、次のような HTML がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="306cd-222">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="306cd-223">`@model` と `@inherits` は同じビューで使うことができます。</span><span class="sxs-lookup"><span data-stu-id="306cd-223">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="306cd-224">`@inherits` は、ビューがインポートする *_ViewImports.cshtml* ファイルで指定できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-224">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="306cd-225">次のコードは、厳密に型指定されたビューの例です。</span><span class="sxs-lookup"><span data-stu-id="306cd-225">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="306cd-226">モデルに rick@contoso.com が渡された場合、ビューは次の HTML マークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="306cd-226">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="306cd-227">`@inject` ディレクティブを使うと、Razor ページで[サービス コンテナー](xref:fundamentals/dependency-injection)からビューにサービスを挿入できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-227">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="306cd-228">詳しくは、「[ビューへの依存関係の挿入](xref:mvc/views/dependency-injection)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="306cd-228">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="306cd-229">`@functions` ディレクティブを使うと、Razor ページで C# コード ブロックをビューに追加できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-229">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="306cd-230">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="306cd-230">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="306cd-231">このコードは、次の HTML マークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="306cd-231">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="306cd-232">次のコードは、生成された Razor C# クラスです。</span><span class="sxs-lookup"><span data-stu-id="306cd-232">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="306cd-233">`@section` ディレクティブを[レイアウト](xref:mvc/views/layout)と組み合わせて使うと、HTML ページのさまざまな部分のコンテンツをビューでレンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="306cd-233">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="306cd-234">詳しくは、「[セクション](xref:mvc/views/layout#layout-sections-label)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="306cd-234">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="306cd-235">テンプレート化された Razor デリゲート</span><span class="sxs-lookup"><span data-stu-id="306cd-235">Templated Razor delegates</span></span>

<span data-ttu-id="306cd-236">Razor テンプレートを使用すると、次の形式で UI スニペットを定義できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-236">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="306cd-237">次の例では、テンプレート化された Razor デリゲートを <xref:System.Func`2> として指定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="306cd-237">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func`2>.</span></span> <span data-ttu-id="306cd-238">デリゲートによってカプセル化されるメソッドのパラメーターに対しては、[dynamic 型](/dotnet/csharp/programming-guide/types/using-type-dynamic)を指定します。</span><span class="sxs-lookup"><span data-stu-id="306cd-238">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="306cd-239">デリゲートの戻り値としては、[object 型](/dotnet/csharp/language-reference/keywords/object)を指定します。</span><span class="sxs-lookup"><span data-stu-id="306cd-239">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="306cd-240">テンプレートは、`Name` プロパティを持つ `Pet` の <xref:System.Collections.Generic.List`1> で使用されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-240">The template is used with a <xref:System.Collections.Generic.List`1> of `Pet` that has a `Name` property.</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

<span data-ttu-id="306cd-241">テンプレートは、`foreach` ステートメントによって提供される `pets` で表示されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-241">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="306cd-242">表示される出力:</span><span class="sxs-lookup"><span data-stu-id="306cd-242">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="306cd-243">メソッドへの引数としてインライン Razor テンプレートを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="306cd-243">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="306cd-244">次の例では、`Repeat` メソッドは Razor テンプレートを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="306cd-244">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="306cd-245">メソッドは、テンプレートを使用して、リストから提供される項目の繰り返しで HTML コンテンツを生成します。</span><span class="sxs-lookup"><span data-stu-id="306cd-245">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

<span data-ttu-id="306cd-246">前の例のペットのリストを使用して、次のように `Repeat` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="306cd-246">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="306cd-247"><xref:System.Collections.Generic.List`1> の `Pet`。</span><span class="sxs-lookup"><span data-stu-id="306cd-247"><xref:System.Collections.Generic.List`1> of `Pet`.</span></span>
* <span data-ttu-id="306cd-248">各ペットを繰り返す回数。</span><span class="sxs-lookup"><span data-stu-id="306cd-248">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="306cd-249">順不同のリストのリスト項目に対して使用するインライン テンプレート。</span><span class="sxs-lookup"><span data-stu-id="306cd-249">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="306cd-250">表示される出力:</span><span class="sxs-lookup"><span data-stu-id="306cd-250">Rendered output:</span></span>

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a><span data-ttu-id="306cd-251">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="306cd-251">Tag Helpers</span></span>

<span data-ttu-id="306cd-252">[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)に関する 3 つのディレクティブがあります。</span><span class="sxs-lookup"><span data-stu-id="306cd-252">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="306cd-253">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="306cd-253">Directive</span></span> | <span data-ttu-id="306cd-254">関数</span><span class="sxs-lookup"><span data-stu-id="306cd-254">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="306cd-255">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="306cd-255">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="306cd-256">ビューでタグ ヘルパーを使えるようにします。</span><span class="sxs-lookup"><span data-stu-id="306cd-256">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="306cd-257">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="306cd-257">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="306cd-258">前に追加したタグ ヘルパーをビューから削除します。</span><span class="sxs-lookup"><span data-stu-id="306cd-258">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="306cd-259">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="306cd-259">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="306cd-260">タグ プレフィックスを指定して、タグ ヘルパーのサポートを有効にしたり、タグ ヘルパーの使用を明示的にしたりします。</span><span class="sxs-lookup"><span data-stu-id="306cd-260">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="306cd-261">Razor の予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="306cd-261">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="306cd-262">Razor のキーワード</span><span class="sxs-lookup"><span data-stu-id="306cd-262">Razor keywords</span></span>

* <span data-ttu-id="306cd-263">page (ASP.NET Core 2.0 以降が必要)</span><span class="sxs-lookup"><span data-stu-id="306cd-263">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="306cd-264">namespace</span><span class="sxs-lookup"><span data-stu-id="306cd-264">namespace</span></span>
* <span data-ttu-id="306cd-265">関数</span><span class="sxs-lookup"><span data-stu-id="306cd-265">functions</span></span>
* <span data-ttu-id="306cd-266">継承</span><span class="sxs-lookup"><span data-stu-id="306cd-266">inherits</span></span>
* <span data-ttu-id="306cd-267">モデル</span><span class="sxs-lookup"><span data-stu-id="306cd-267">model</span></span>
* <span data-ttu-id="306cd-268">section</span><span class="sxs-lookup"><span data-stu-id="306cd-268">section</span></span>
* <span data-ttu-id="306cd-269">helper (現在は ASP.NET Core ではサポートされていません)</span><span class="sxs-lookup"><span data-stu-id="306cd-269">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="306cd-270">Razor のキーワードは、`@(Razor Keyword)` でエスケープします (例: `@(functions)`)。</span><span class="sxs-lookup"><span data-stu-id="306cd-270">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="306cd-271">C# Razor のキーワード</span><span class="sxs-lookup"><span data-stu-id="306cd-271">C# Razor keywords</span></span>

* <span data-ttu-id="306cd-272">case</span><span class="sxs-lookup"><span data-stu-id="306cd-272">case</span></span>
* <span data-ttu-id="306cd-273">do</span><span class="sxs-lookup"><span data-stu-id="306cd-273">do</span></span>
* <span data-ttu-id="306cd-274">default</span><span class="sxs-lookup"><span data-stu-id="306cd-274">default</span></span>
* <span data-ttu-id="306cd-275">for</span><span class="sxs-lookup"><span data-stu-id="306cd-275">for</span></span>
* <span data-ttu-id="306cd-276">foreach</span><span class="sxs-lookup"><span data-stu-id="306cd-276">foreach</span></span>
* <span data-ttu-id="306cd-277">if</span><span class="sxs-lookup"><span data-stu-id="306cd-277">if</span></span>
* <span data-ttu-id="306cd-278">else</span><span class="sxs-lookup"><span data-stu-id="306cd-278">else</span></span>
* <span data-ttu-id="306cd-279">lock</span><span class="sxs-lookup"><span data-stu-id="306cd-279">lock</span></span>
* <span data-ttu-id="306cd-280">switch</span><span class="sxs-lookup"><span data-stu-id="306cd-280">switch</span></span>
* <span data-ttu-id="306cd-281">try</span><span class="sxs-lookup"><span data-stu-id="306cd-281">try</span></span>
* <span data-ttu-id="306cd-282">catch</span><span class="sxs-lookup"><span data-stu-id="306cd-282">catch</span></span>
* <span data-ttu-id="306cd-283">finally</span><span class="sxs-lookup"><span data-stu-id="306cd-283">finally</span></span>
* <span data-ttu-id="306cd-284">使用</span><span class="sxs-lookup"><span data-stu-id="306cd-284">using</span></span>
* <span data-ttu-id="306cd-285">while</span><span class="sxs-lookup"><span data-stu-id="306cd-285">while</span></span>

<span data-ttu-id="306cd-286">C# Razor のキーワードは、`@(@C# Razor Keyword)` で二重にエスケープする必要があります (例: `@(@case)`)。</span><span class="sxs-lookup"><span data-stu-id="306cd-286">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="306cd-287">最初の `@` は、Razor パーサーをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="306cd-287">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="306cd-288">2 番目の `@` は、C# パーサーをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="306cd-288">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="306cd-289">Razor で使われない予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="306cd-289">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="306cd-290">class</span><span class="sxs-lookup"><span data-stu-id="306cd-290">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="306cd-291">ビューに対して生成された Razor C# クラスを調べる</span><span class="sxs-lookup"><span data-stu-id="306cd-291">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="306cd-292">.NET Core SDK 2.1 以降、[Razor SDK](xref:razor-pages/sdk) は Razor ファイルのコンパイルを処理します。</span><span class="sxs-lookup"><span data-stu-id="306cd-292">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="306cd-293">プロジェクトを作成する際に、Razor SDK はプロジェクト ルートに *obj/<build_configuration>/<target_framework_moniker>/Razor* ディレクトリを生成します。</span><span class="sxs-lookup"><span data-stu-id="306cd-293">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="306cd-294">*Razor* ディレクトリ内のディレクトリ構造は、プロジェクトのディレクトリ構造をミラー化します。</span><span class="sxs-lookup"><span data-stu-id="306cd-294">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="306cd-295">.NET Core 2.1 をターゲットとする ASP.NET Core 2.1 Razor Pages プロジェクト内の次のディレクトリ構造を考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="306cd-295">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="306cd-296">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="306cd-296">**Areas/**</span></span>
  * <span data-ttu-id="306cd-297">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="306cd-297">**Admin/**</span></span>
    * <span data-ttu-id="306cd-298">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="306cd-298">**Pages/**</span></span>
      * <span data-ttu-id="306cd-299">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="306cd-299">*Index.cshtml*</span></span>
      * <span data-ttu-id="306cd-300">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="306cd-300">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="306cd-301">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="306cd-301">**Pages/**</span></span>
  * <span data-ttu-id="306cd-302">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="306cd-302">**Shared/**</span></span>
    * <span data-ttu-id="306cd-303">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="306cd-303">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="306cd-304">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="306cd-304">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="306cd-305">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="306cd-305">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="306cd-306">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="306cd-306">*Index.cshtml*</span></span>
  * <span data-ttu-id="306cd-307">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="306cd-307">*Index.cshtml.cs*</span></span>

<span data-ttu-id="306cd-308">*Debug* 構成でプロジェクトを作成すると、次の *obj* ディレクトリが生成されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-308">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="306cd-309">**obj/**</span><span class="sxs-lookup"><span data-stu-id="306cd-309">**obj/**</span></span>
  * <span data-ttu-id="306cd-310">**Debug/**</span><span class="sxs-lookup"><span data-stu-id="306cd-310">**Debug/**</span></span>
    * <span data-ttu-id="306cd-311">**netcoreapp2.1/**</span><span class="sxs-lookup"><span data-stu-id="306cd-311">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="306cd-312">**Razor/**</span><span class="sxs-lookup"><span data-stu-id="306cd-312">**Razor/**</span></span>
        * <span data-ttu-id="306cd-313">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="306cd-313">**Areas/**</span></span>
          * <span data-ttu-id="306cd-314">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="306cd-314">**Admin/**</span></span>
            * <span data-ttu-id="306cd-315">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="306cd-315">**Pages/**</span></span>
              * <span data-ttu-id="306cd-316">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="306cd-316">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="306cd-317">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="306cd-317">**Pages/**</span></span>
          * <span data-ttu-id="306cd-318">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="306cd-318">**Shared/**</span></span>
            * <span data-ttu-id="306cd-319">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="306cd-319">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="306cd-320">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="306cd-320">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="306cd-321">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="306cd-321">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="306cd-322">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="306cd-322">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="306cd-323">*Pages/Index.cshtml* に対して生成されたクラスを表示するには、*obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs* を開きます。</span><span class="sxs-lookup"><span data-stu-id="306cd-323">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="306cd-324">次のクラスを ASP.NET Core MVC プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="306cd-324">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="306cd-325">`Startup.ConfigureServices` で、MVC によって追加された `RazorTemplateEngine` を `CustomTemplateEngine` クラスでオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="306cd-325">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="306cd-326">`CustomTemplateEngine` の `return csharpDocument;` ステートメントにブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="306cd-326">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="306cd-327">プログラムの実行がブレークポイントで停止したら、`generatedCode` の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="306cd-327">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![generatedCode のテキスト ビジュアライザーの表示](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="306cd-329">ビューの参照と大文字/小文字の区別</span><span class="sxs-lookup"><span data-stu-id="306cd-329">View lookups and case sensitivity</span></span>

<span data-ttu-id="306cd-330">Razor ビュー エンジンによるビューの参照では、大文字と小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-330">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="306cd-331">ただし、実際の参照は、基になるファイル システムによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="306cd-331">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="306cd-332">ファイル ベースのソース:</span><span class="sxs-lookup"><span data-stu-id="306cd-332">File based source:</span></span>
  * <span data-ttu-id="306cd-333">大文字と小文字が区別されないファイル システムを使っているオペレーティング システム (Windows など) では、物理的なファイル プロバイダーの参照は大文字と小文字を区別しません。</span><span class="sxs-lookup"><span data-stu-id="306cd-333">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="306cd-334">たとえば、`return View("Test")` は、*/Views/Home/Test.cshtml*、*/Views/home/test.cshtml*、その他のすべての大文字と小文字のバリエーションと一致します。</span><span class="sxs-lookup"><span data-stu-id="306cd-334">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="306cd-335">大文字と小文字が区別されるファイル システム (たとえば、Linux、OSX、および `EmbeddedFileProvider`) では、参照は大文字と小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="306cd-335">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="306cd-336">たとえば、`return View("Test")` は */Views/Home/Test.cshtml* だけと一致します。</span><span class="sxs-lookup"><span data-stu-id="306cd-336">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="306cd-337">プリコンパイル済みのビュー:ASP.NET Core 2.0 以降では、プリコンパイル済みのビューの参照は、すべてのオペレーティング システムで大文字と小文字を区別しません。</span><span class="sxs-lookup"><span data-stu-id="306cd-337">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="306cd-338">動作は、Windows での物理ファイル プロバイダーの動作と同じです。</span><span class="sxs-lookup"><span data-stu-id="306cd-338">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="306cd-339">2 つのプリコンパイル済みビューの相違点が大文字と小文字の使い分けだけの場合、参照の結果はどちらになるかわかりません。</span><span class="sxs-lookup"><span data-stu-id="306cd-339">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="306cd-340">開発者には、ファイル名とディレクトリ名の大文字/小文字の使い分けを、次のものと一致させることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="306cd-340">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="306cd-341">領域、コントローラー、アクションの名前。</span><span class="sxs-lookup"><span data-stu-id="306cd-341">Area, controller, and action names.</span></span>
* <span data-ttu-id="306cd-342">Razor ページ。</span><span class="sxs-lookup"><span data-stu-id="306cd-342">Razor Pages.</span></span>

<span data-ttu-id="306cd-343">大文字と小文字の使い分けを一致させると、展開は基になっているファイル システムに関係なくビューを検索できます。</span><span class="sxs-lookup"><span data-stu-id="306cd-343">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
