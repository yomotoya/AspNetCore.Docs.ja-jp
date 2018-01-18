---
title: "ASP.NET Core の razor 構文のリファレンス"
author: rick-anderson
description: "サーバー ベースのコードを埋め込む web ページの Razor マークアップの構文について説明します。"
keywords: "ASP.NET Core、Razor、Razor ディレクティブ"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 6df769069fce52755a57d8404f88203a652a1ab9
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="0e2b9-104">ASP.NET Core の razor 構文</span><span class="sxs-lookup"><span data-stu-id="0e2b9-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="0e2b9-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Luke Latham](https://github.com/guardrex)、 [Taylor Mullen](https://twitter.com/ntaylormullen)、および[Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="0e2b9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="0e2b9-106">Razor とは、サーバー ベースのコードを埋め込む web ページのマークアップ構文です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="0e2b9-107">Razor 構文は、Razor マークアップ、c#、および HTML で構成されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="0e2b9-108">通常、Razor を含むファイルが、 *.cshtml*ファイル拡張子。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="0e2b9-109">HTML を表示</span><span class="sxs-lookup"><span data-stu-id="0e2b9-109">Rendering HTML</span></span>

<span data-ttu-id="0e2b9-110">Razor の既定の言語は、HTML です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-110">The default Razor language is HTML.</span></span> <span data-ttu-id="0e2b9-111">Razor マークアップからレンダリング HTML は、HTML ファイルから HTML をレンダリングと違いはありません。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="0e2b9-112">内の HTML マークアップ*.cshtml* Razor ファイルが変更されていないサーバーを表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="0e2b9-113">Razor 構文</span><span class="sxs-lookup"><span data-stu-id="0e2b9-113">Razor syntax</span></span>

<span data-ttu-id="0e2b9-114">Razor (C#) をサポートしを使用して、 `@` HTML を c# から移行する記号。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="0e2b9-115">Razor では、c# 式を評価し、それらを HTML 出力でレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="0e2b9-116">ときに、`@`シンボルが続く、 [Razor 予約キーワード](#razor-reserved-keywords)、Razor 固有のマークアップに移行します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="0e2b9-117">それ以外の場合、一般的な C# の場合に移行します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="0e2b9-118">エスケープする、`@`シンボル Razor マークアップを 1 秒間を使用して`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="0e2b9-119">コードが、1 つの HTML でレンダリング`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="0e2b9-120">HTML 属性と電子メール アドレスを含むコンテンツが処理されない、`@`遷移文字として記号。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="0e2b9-121">Razor 解析では、次の例では電子メール アドレスがそのまま残ります。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="0e2b9-122">Razor の暗黙的な式</span><span class="sxs-lookup"><span data-stu-id="0e2b9-122">Implicit Razor expressions</span></span>

<span data-ttu-id="0e2b9-123">Razor の暗黙的な式が始まる`@`c# コードを続けて。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="0e2b9-124">C# の場合を除く`await`キーワードで暗黙的な式はスペースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="0e2b9-125">C# ステートメントにクリア終了がある場合は、スペースが混在させることができます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="0e2b9-126">暗黙的な式**できません**角かっこ内の文字として C# の場合、ジェネリックを含む (`<>`) で HTML タグとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="0e2b9-127">次のコードは**いない**無効です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="0e2b9-128">上記のコードでは、次のいずれかのようなコンパイラ エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="0e2b9-129">"Int"要素が閉じられませんでした。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-129">The "int" element was not closed.</span></span>  <span data-ttu-id="0e2b9-130">すべての要素はいずれかである必要がありますに対応する終了タグが自己終了、または。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="0e2b9-131">メソッド グループ 'GenericMethod' を非デリゲート型 'object' を変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="0e2b9-132">メソッドを呼び出すつもりでしたか?'</span><span class="sxs-lookup"><span data-stu-id="0e2b9-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="0e2b9-133">ジェネリック メソッドの呼び出しにラップする必要があります、 [Razor 式が明示的](#explicit-razor-expressions)または[Razor コードのブロック](#razor-code-blocks)です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="0e2b9-134">Razor の明示的な式</span><span class="sxs-lookup"><span data-stu-id="0e2b9-134">Explicit Razor expressions</span></span>

<span data-ttu-id="0e2b9-135">Razor の明示的な式から成る、`@`バランスの取れたかっこ記号。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="0e2b9-136">過去 1 週間の時間を表示するために、次の Razor マークアップを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="0e2b9-137">内のコンテンツ、`@()`かっこが評価され、出力に表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="0e2b9-138">一般に、前のセクションで説明されている、暗黙的な式は、スペースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="0e2b9-139">次のコードでは、1 週間は現在の時間から減算されていません。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="0e2b9-140">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="0e2b9-141">明示的な式は、式の結果を含むテキストの連結を使用できます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="0e2b9-142">明示的の式がない`<p>Age@joe.Age</p>`、電子メール アドレスとして扱われるおよび`<p>Age@joe.Age</p>`が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="0e2b9-143">明示的な式として書き込まれるときに`<p>Age33</p>`が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="0e2b9-144">明示的な式は、ジェネリックのメソッドからの出力を表示するために使用できます*.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="0e2b9-145">暗黙的な式を角かっこ内の文字で (`<>`) は、HTML タグとして解釈されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-145">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="0e2b9-146">次のマークアップが**いない**Razor が無効です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-146">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="0e2b9-147">上記のコードでは、次のいずれかのようなコンパイラ エラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-147">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="0e2b9-148">"Int"要素が閉じられませんでした。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-148">The "int" element was not closed.</span></span>  <span data-ttu-id="0e2b9-149">すべての要素はいずれかである必要がありますに対応する終了タグが自己終了、または。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-149">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="0e2b9-150">メソッド グループ 'GenericMethod' を非デリゲート型 'object' を変換することはできません。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-150">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="0e2b9-151">メソッドを呼び出すつもりでしたか?'</span><span class="sxs-lookup"><span data-stu-id="0e2b9-151">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="0e2b9-152">次のマークアップは、適切な方法の書き込みにこのコードを示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-152">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="0e2b9-153">コードは、明示的な式として書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-153">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="0e2b9-154">式のエンコード</span><span class="sxs-lookup"><span data-stu-id="0e2b9-154">Expression encoding</span></span>

<span data-ttu-id="0e2b9-155">文字列に評価される c# 式は、HTML エンコードです。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-155">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="0e2b9-156">C# 式の評価が`IHtmlContent`経由で直接レンダリング`IHtmlContent.WriteTo`です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-156">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="0e2b9-157">C# 式の評価がない`IHtmlContent`によって文字列に変換`ToString`描画する前にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-157">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="0e2b9-158">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-158">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="0e2b9-159">としてブラウザーで HTML が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-159">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="0e2b9-160">`HtmlHelper.Raw`出力では、エンコードされたが、HTML マークアップとしてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-160">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="0e2b9-161">使用して`HtmlHelper.Raw`unsanitized ユーザー入力は、セキュリティ上のリスクです。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-161">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="0e2b9-162">ユーザー入力は、悪意のある JavaScript または他の攻撃に含まれる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-162">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="0e2b9-163">ユーザー入力をサニタイズしすることは困難です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-163">Sanitizing user input is difficult.</span></span> <span data-ttu-id="0e2b9-164">使用しないでください`HtmlHelper.Raw`ユーザー入力とします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-164">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="0e2b9-165">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-165">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="0e2b9-166">Razor コードのブロック</span><span class="sxs-lookup"><span data-stu-id="0e2b9-166">Razor code blocks</span></span>

<span data-ttu-id="0e2b9-167">Razor コード ブロックが始まる`@`で囲まれたと`{}`です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-167">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="0e2b9-168">式とは異なり、コード ブロック内の c# コードは表示されません。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-168">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="0e2b9-169">コード ブロックと、ビューの式は、同じスコープの共有の順序で定義されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-169">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="0e2b9-170">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-170">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="0e2b9-171">暗黙の切り替え</span><span class="sxs-lookup"><span data-stu-id="0e2b9-171">Implicit transitions</span></span>

<span data-ttu-id="0e2b9-172">既定の言語コード ブロックでは、C# の場合が、Razor ページを HTML に移行できます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-172">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="0e2b9-173">明示的な区切り記号付き遷移</span><span class="sxs-lookup"><span data-stu-id="0e2b9-173">Explicit delimited transition</span></span>

<span data-ttu-id="0e2b9-174">HTML を描画するコード ブロックのサブセクションを定義するのには、Razor で表示するための文字を囲む**\<テキスト >**タグ。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-174">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="0e2b9-175">HTML タグで囲まれていない HTML を表示するのにには、この方法を使用します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-175">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="0e2b9-176">HTML または Razor タグなしは、Razor 実行時エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-176">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="0e2b9-177">**\<テキスト >**タグはコンテンツを表示するときに空白を制御すると便利です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-177">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="0e2b9-178">間のコンテンツのみ、 **\<テキスト >**タグを表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-178">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="0e2b9-179">前に、または後に空白、 **\<テキスト >**タグは HTML 出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-179">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="0e2b9-180">明示的な行の遷移と @。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-180">Explicit Line Transition with @:</span></span>

<span data-ttu-id="0e2b9-181">コード ブロックの内部 HTML として残りの行全体を表示を使用して、`@:`構文。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-181">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="0e2b9-182">なし、 `@:` Razor 実行時エラーが生成されたコードでは、します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-182">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="0e2b9-183">警告: 余分な`@`Razor ファイル内の文字、ブロックの後でステートメントの原因とコンパイラ エラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-183">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="0e2b9-184">これらのコンパイラ エラーは報告されたエラーの前に、実際のエラーが発生するために理解するが困難にすることはできます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-184">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="0e2b9-185">このエラーは、後、1 つのコード ブロックに複数の暗黙的または明示的な式を組み合わせることが一般的です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-185">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="0e2b9-186">制御構造</span><span class="sxs-lookup"><span data-stu-id="0e2b9-186">Control Structures</span></span>

<span data-ttu-id="0e2b9-187">制御構造体は、コード ブロックの拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-187">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="0e2b9-188">コード ブロック (マークアップ、インライン c# への遷移) ものすべての側面は、次の構造体に適用されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-188">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="0e2b9-189">条件付き@if、一致しない場合、#else、および@switch</span><span class="sxs-lookup"><span data-stu-id="0e2b9-189">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="0e2b9-190">`@if`コードの実行時にコントロール:</span><span class="sxs-lookup"><span data-stu-id="0e2b9-190">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="0e2b9-191">`else`および`else if`を必要としない、`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-191">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="0e2b9-192">次のマークアップは、switch ステートメントを使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-192">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="0e2b9-193">ループ@for、 @foreach、 @while、および@do中</span><span class="sxs-lookup"><span data-stu-id="0e2b9-193">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="0e2b9-194">コントロール ステートメントのループを設定して、テンプレート化された HTML を表示できます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-194">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="0e2b9-195">人のユーザーの一覧を表示するには。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-195">To render a list of people:</span></span>

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

<span data-ttu-id="0e2b9-196">次のループ ステートメントがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-196">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="0e2b9-197">複合@using</span><span class="sxs-lookup"><span data-stu-id="0e2b9-197">Compound @using</span></span>

<span data-ttu-id="0e2b9-198">C# で、`using`オブジェクトが破棄されることを確認するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-198">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="0e2b9-199">Razor では、同じメカニズムを使用してを追加のコンテンツを含む HTML ヘルパーを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-199">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="0e2b9-200">次のコードでは、HTML ヘルパーが含む form タグをレンダリング、`@using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-200">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="0e2b9-201">スコープ レベルのアクションを実行できます[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-201">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="0e2b9-202">@try、catch、finally</span><span class="sxs-lookup"><span data-stu-id="0e2b9-202">@try, catch, finally</span></span>

<span data-ttu-id="0e2b9-203">例外処理は、C# の場合に似ています。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-203">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="0e2b9-204">Razor では、クリティカル セクション lock ステートメントを保護する機能があります。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-204">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="0e2b9-205">コメント</span><span class="sxs-lookup"><span data-stu-id="0e2b9-205">Comments</span></span>

<span data-ttu-id="0e2b9-206">Razor には、c# と HTML のコメントがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-206">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="0e2b9-207">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-207">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="0e2b9-208">Razor コメントは、web ページが表示される前に、サーバーによって削除されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-208">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="0e2b9-209">Razor を使用して`@*  *@`コメントを区切るためにします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-209">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="0e2b9-210">次のコードはコメント アウトされて、サーバーは、マークアップを表示しないようにします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-210">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="0e2b9-211">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="0e2b9-211">Directives</span></span>

<span data-ttu-id="0e2b9-212">Razor ディレクティブが次の予約済みキーワードからの暗黙的な式で表される、`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-212">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="0e2b9-213">ディレクティブは、通常ビューは解析または別の機能を有効にする方法を変更します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-213">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="0e2b9-214">Razor がビューのコードを生成する方法を理解しやすくディレクティブの動作を理解します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-214">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="0e2b9-215">コードでは、次のようなクラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-215">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="0e2b9-216">この記事のセクションで後述[ビューに対して生成された Razor c# クラスを表示する](#viewing-the-razor-c-class-generated-for-a-view)この生成されたクラスを表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-216">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="0e2b9-217">`@using`ディレクティブを追加、c#`using`ディレクティブを生成されたビューに。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-217">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="0e2b9-218">`@model`ディレクティブがビューに渡されたモデルの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-218">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="0e2b9-219">個々 のユーザー アカウントに作成された ASP.NET Core MVC アプリで、 *Views/Account/Login.cshtml*ビューには、次のモデルの宣言が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-219">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="0e2b9-220">生成されたクラスから継承`RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="0e2b9-220">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="0e2b9-221">Razor の公開、`Model`ビューに渡されるモデルにアクセスするためのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-221">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="0e2b9-222">`@model`ディレクティブは、このプロパティの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-222">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="0e2b9-223">ディレクティブを指定します、`T`で`RazorPage<T>`を生成するクラスから派生して、表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-223">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="0e2b9-224">場合、`@model`指定すると、ディレクティブの後、`Model`プロパティの型は`dynamic`します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-224">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="0e2b9-225">モデルの値は、コント ローラーからビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-225">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="0e2b9-226">詳細については、次を参照してください。 [モデルを厳密に型指定と@modelキーワード。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-226">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="0e2b9-227">`@inherits`ディレクティブは、ビューが継承するクラスを完全に制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-227">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="0e2b9-228">次のコードでは、カスタム Razor ページの種類を示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-228">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="0e2b9-229">`CustomText`ビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-229">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="0e2b9-230">コードは、次の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-230">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="0e2b9-231">`@model`および`@inherits`同じビューで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-231">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="0e2b9-232">`@inherits`指定できます、 *_ViewImports.cshtml*ビューをインポートするファイル。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-232">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="0e2b9-233">次のコードは、厳密に型指定されたビューの例です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-233">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="0e2b9-234">場合"rick@contoso.com"渡されるモデルでは、ビューには、次の HTML マークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-234">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="0e2b9-235">`@inject`ディレクティブからサービスを挿入する Razor ページを使用する、[サービス コンテナー](xref:fundamentals/dependency-injection)な表示にします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-235">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="0e2b9-236">詳細については、次を参照してください。[ビューに依存性の注入](xref:mvc/views/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-236">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="0e2b9-237">`@functions`ディレクティブは、関数レベルのコンテンツ ビューを追加する Razor ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-237">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="0e2b9-238">例:</span><span class="sxs-lookup"><span data-stu-id="0e2b9-238">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="0e2b9-239">コードでは、次の HTML マークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-239">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="0e2b9-240">次のコードでは、生成された Razor c# クラスを示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-240">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="0e2b9-241">`@section`ディレクティブを組み合わせて使用、[レイアウト](xref:mvc/views/layout)HTML ページのさまざまな部分にコンテンツをレンダリングするビューを有効にします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-241">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="0e2b9-242">詳細については、次を参照してください。[セクション](xref:mvc/views/layout#layout-sections-label)です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-242">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="0e2b9-243">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="0e2b9-243">Tag Helpers</span></span>

<span data-ttu-id="0e2b9-244">関連する次の 3 つのディレクティブがある[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-244">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="0e2b9-245">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="0e2b9-245">Directive</span></span> | <span data-ttu-id="0e2b9-246">関数</span><span class="sxs-lookup"><span data-stu-id="0e2b9-246">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="0e2b9-247">タグ ヘルパーを可能にすると表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-247">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="0e2b9-248">以前に追加したビューからタグ ヘルパーを削除します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-248">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="0e2b9-249">タグ ヘルパーのサポートを有効にして、タグ ヘルパーの使用法を明確にするのには、タグ プレフィックスを指定します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-249">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="0e2b9-250">Razor の予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="0e2b9-250">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="0e2b9-251">Razor キーワード</span><span class="sxs-lookup"><span data-stu-id="0e2b9-251">Razor keywords</span></span>

* <span data-ttu-id="0e2b9-252">(ASP.NET Core 2.0 以降が必要) ページ</span><span class="sxs-lookup"><span data-stu-id="0e2b9-252">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="0e2b9-253">関数</span><span class="sxs-lookup"><span data-stu-id="0e2b9-253">functions</span></span>
* <span data-ttu-id="0e2b9-254">継承</span><span class="sxs-lookup"><span data-stu-id="0e2b9-254">inherits</span></span>
* <span data-ttu-id="0e2b9-255">モデル</span><span class="sxs-lookup"><span data-stu-id="0e2b9-255">model</span></span>
* <span data-ttu-id="0e2b9-256">section</span><span class="sxs-lookup"><span data-stu-id="0e2b9-256">section</span></span>
* <span data-ttu-id="0e2b9-257">ヘルパー (ASP.NET Core ではサポートされていない)</span><span class="sxs-lookup"><span data-stu-id="0e2b9-257">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="0e2b9-258">Razor のキーワードでエスケープ`@(Razor Keyword)`(たとえば、 `@(functions)`)。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-258">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="0e2b9-259">C# Razor キーワード</span><span class="sxs-lookup"><span data-stu-id="0e2b9-259">C# Razor keywords</span></span>

* <span data-ttu-id="0e2b9-260">case</span><span class="sxs-lookup"><span data-stu-id="0e2b9-260">case</span></span>
* <span data-ttu-id="0e2b9-261">do</span><span class="sxs-lookup"><span data-stu-id="0e2b9-261">do</span></span>
* <span data-ttu-id="0e2b9-262">default</span><span class="sxs-lookup"><span data-stu-id="0e2b9-262">default</span></span>
* <span data-ttu-id="0e2b9-263">for</span><span class="sxs-lookup"><span data-stu-id="0e2b9-263">for</span></span>
* <span data-ttu-id="0e2b9-264">foreach</span><span class="sxs-lookup"><span data-stu-id="0e2b9-264">foreach</span></span>
* <span data-ttu-id="0e2b9-265">if</span><span class="sxs-lookup"><span data-stu-id="0e2b9-265">if</span></span>
* <span data-ttu-id="0e2b9-266">else</span><span class="sxs-lookup"><span data-stu-id="0e2b9-266">else</span></span>
* <span data-ttu-id="0e2b9-267">lock</span><span class="sxs-lookup"><span data-stu-id="0e2b9-267">lock</span></span>
* <span data-ttu-id="0e2b9-268">switch</span><span class="sxs-lookup"><span data-stu-id="0e2b9-268">switch</span></span>
* <span data-ttu-id="0e2b9-269">try</span><span class="sxs-lookup"><span data-stu-id="0e2b9-269">try</span></span>
* <span data-ttu-id="0e2b9-270">catch</span><span class="sxs-lookup"><span data-stu-id="0e2b9-270">catch</span></span>
* <span data-ttu-id="0e2b9-271">finally</span><span class="sxs-lookup"><span data-stu-id="0e2b9-271">finally</span></span>
* <span data-ttu-id="0e2b9-272">使用</span><span class="sxs-lookup"><span data-stu-id="0e2b9-272">using</span></span>
* <span data-ttu-id="0e2b9-273">while</span><span class="sxs-lookup"><span data-stu-id="0e2b9-273">while</span></span>

<span data-ttu-id="0e2b9-274">C# Razor キーワードでダブル エスケープする必要があります`@(@C# Razor Keyword)`(たとえば、 `@(@case)`)。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-274">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="0e2b9-275">最初の`@`Razor パーサーをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-275">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="0e2b9-276">2 番目`@`c# パーサーをエスケープします。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-276">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="0e2b9-277">Razor で使用されていない予約済みキーワード</span><span class="sxs-lookup"><span data-stu-id="0e2b9-277">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="0e2b9-278">namespace</span><span class="sxs-lookup"><span data-stu-id="0e2b9-278">namespace</span></span>
* <span data-ttu-id="0e2b9-279">class</span><span class="sxs-lookup"><span data-stu-id="0e2b9-279">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="0e2b9-280">ビューに対して生成された Razor c# クラスを表示します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-280">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="0e2b9-281">ASP.NET Core の MVC プロジェクトに次のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-281">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="0e2b9-282">上書き、`RazorTemplateEngine`での MVC によって追加された、`CustomTemplateEngine`クラス。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-282">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="0e2b9-283">ブレークポイントの設定、`return csharpDocument`ステートメントの`CustomTemplateEngine`します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-283">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="0e2b9-284">プログラムの実行がブレークポイントで停止したときの値を表示`generatedCode`です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-284">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![GeneratedCode のテキスト ビジュアライザーのビュー](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="0e2b9-286">ビューの参照や大文字と小文字の区別</span><span class="sxs-lookup"><span data-stu-id="0e2b9-286">View lookups and case sensitivity</span></span>

<span data-ttu-id="0e2b9-287">Razor ビュー エンジンは、ビューを区別する検索を実行します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-287">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="0e2b9-288">ただし、実際の参照は、基になるファイル システムによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-288">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="0e2b9-289">ファイル ベースのソース:</span><span class="sxs-lookup"><span data-stu-id="0e2b9-289">File based source:</span></span> 
  * <span data-ttu-id="0e2b9-290">大文字と小文字のファイル システム (Windows など) でのオペレーティング システムで物理ファイルのプロバイダーの参照は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-290">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="0e2b9-291">たとえば、`return View("Test")`に一致する結果*/Views/Home/Test.cshtml*、 */Views/home/test.cshtml*、およびその他の文字種バリアント。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-291">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="0e2b9-292">大文字小文字を区別ファイル システムで (たとえば、Linux、os X を使用して`EmbeddedFileProvider`)、検索は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-292">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="0e2b9-293">たとえば、`return View("Test")`具体的には一致*/Views/Home/Test.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-293">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="0e2b9-294">ビューをプリコンパイル: ASP.NET Core 2.0 以降ではすべてのオペレーティング システムで大文字と小文字はプリコンパイル済みのビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-294">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="0e2b9-295">動作は、Windows 上の物理ファイルのプロバイダーの動作と同じです。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-295">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="0e2b9-296">プリコンパイル済みの 2 つのビューが大文字と小文字が異なる場合、参照の結果は非決定的です。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-296">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="0e2b9-297">開発者は、大文字と小文字のファイルとディレクトリ名の大文字と小文字が一致します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-297">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="0e2b9-298">領域、コント ローラー、およびアクションの名前。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-298">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="0e2b9-299">Razor ページ。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-299">Razor Pages.</span></span>
    
<span data-ttu-id="0e2b9-300">小文字を区別とは、展開が基になるファイル システムに関係なく、ビューを検索することを確認します。</span><span class="sxs-lookup"><span data-stu-id="0e2b9-300">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
