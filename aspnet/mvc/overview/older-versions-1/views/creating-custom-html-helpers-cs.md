---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: カスタム HTML ヘルパー (c#) の作成 |Microsoft Docs
author: microsoft
description: このチュートリアルの目的を MVC ビュー内で使用できるカスタム HTML ヘルパーを作成する方法を示すことです。 HTML ヘルパーを活用しています.
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 0606ec3b5595fbe73918b82e32b393871e8533a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839410"
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="79523-104">カスタム HTML ヘルパーの作成 (c#)</span><span class="sxs-lookup"><span data-stu-id="79523-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="79523-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="79523-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="79523-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="79523-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="79523-107">このチュートリアルの目的を MVC ビュー内で使用できるカスタム HTML ヘルパーを作成する方法を示すことです。</span><span class="sxs-lookup"><span data-stu-id="79523-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="79523-108">HTML ヘルパーを利用して HTML タグの標準の HTML ページを作成するために必要ことの面倒な型指定の量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="79523-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="79523-109">このチュートリアルの目的を MVC ビュー内で使用できるカスタム HTML ヘルパーを作成する方法を示すことです。</span><span class="sxs-lookup"><span data-stu-id="79523-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="79523-110">HTML ヘルパーを利用して HTML タグの標準の HTML ページを作成するために必要ことの面倒な型指定の量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="79523-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="79523-111">このチュートリアルの最初の部分では、ASP.NET MVC フレームワークに含まれている既存の HTML ヘルパーの一部を取り上げます。</span><span class="sxs-lookup"><span data-stu-id="79523-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="79523-112">次に、カスタム HTML ヘルパーの作成の 2 つの方法を説明しました。 静的メソッドを作成し、拡張メソッドを作成して、カスタム HTML ヘルパーを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="79523-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="79523-113">HTML ヘルパーを理解します。</span><span class="sxs-lookup"><span data-stu-id="79523-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="79523-114">HTML ヘルパーは、文字列を返す方法だけです。</span><span class="sxs-lookup"><span data-stu-id="79523-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="79523-115">文字列は、コンテンツの任意の型を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="79523-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="79523-116">たとえば、HTML と同様に、標準の HTML タグをレンダリングする HTML ヘルパーを使用できます`<input>`と`<img>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="79523-117">タブ ストリップまたはデータベースのデータの HTML テーブルなどのより複雑なコンテンツをレンダリングするのに HTML ヘルパーを使用することができますも。</span><span class="sxs-lookup"><span data-stu-id="79523-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="79523-118">ASP.NET MVC フレームワークには、次の一連標準の HTML ヘルパー (これは完全な一覧) にはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="79523-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="79523-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="79523-119">Html.ActionLink()</span></span>
- <span data-ttu-id="79523-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="79523-120">Html.BeginForm()</span></span>
- <span data-ttu-id="79523-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="79523-121">Html.CheckBox()</span></span>
- <span data-ttu-id="79523-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="79523-122">Html.DropDownList()</span></span>
- <span data-ttu-id="79523-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="79523-123">Html.EndForm()</span></span>
- <span data-ttu-id="79523-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="79523-124">Html.Hidden()</span></span>
- <span data-ttu-id="79523-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="79523-125">Html.ListBox()</span></span>
- <span data-ttu-id="79523-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="79523-126">Html.Password()</span></span>
- <span data-ttu-id="79523-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="79523-127">Html.RadioButton()</span></span>
- <span data-ttu-id="79523-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="79523-128">Html.TextArea()</span></span>
- <span data-ttu-id="79523-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="79523-129">Html.TextBox()</span></span>

<span data-ttu-id="79523-130">たとえば、リスト 1 でフォームを検討してください。</span><span class="sxs-lookup"><span data-stu-id="79523-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="79523-131">(図 1 参照)、標準の HTML ヘルパーの 2 つのヘルプでは、このフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="79523-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="79523-132">このフォームを使用して、`Html.BeginForm()`と`Html.TextBox()`単純な HTML フォームを表示するためにヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="79523-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="79523-133">[![HTML ヘルパーのページが表示されます。](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="79523-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="79523-134">**図 01**: HTML ヘルパーのページが表示される ([フルサイズの画像を表示する をクリックします](creating-custom-html-helpers-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="79523-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="79523-135">**1 – を一覧表示します。 `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="79523-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="79523-136">Html.BeginForm() ヘルパー メソッドはかっこと右の HTML を作成するために使用`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="79523-137">注意、`Html.BeginForm()`内を使用して、メソッドが呼び出されるステートメントです。</span><span class="sxs-lookup"><span data-stu-id="79523-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="79523-138">確実にステートメントを使用して、`<form>`を使用して、最後にタグが閉じられるブロックします。</span><span class="sxs-lookup"><span data-stu-id="79523-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="79523-139">使用して、作成する代わりに、希望する場合、ブロックを終了する Html.EndForm() ヘルパー メソッドを呼び出すことができます、`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="79523-140">作成の開始と終了にどの方法を使用して、`<form>`最も直感的にすると思われるタグ。</span><span class="sxs-lookup"><span data-stu-id="79523-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="79523-141">`Html.TextBox()`ヘルパー メソッドは、リスト 1 で HTML を表示するために使用される`<input>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="79523-142">リスト 2 での HTML ソースを表示し、ブラウザーでソースの表示を選択します。 場合、</span><span class="sxs-lookup"><span data-stu-id="79523-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="79523-143">標準の HTML タグが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79523-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79523-144">注意、 `Html.TextBox()`HTML ヘルパーでレンダリングされて`<%= %>`タグの代わりに`<% %>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="79523-145">等号 (=) を含めない場合、ブラウザーに表示 nothing を取得します。</span><span class="sxs-lookup"><span data-stu-id="79523-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="79523-146">ASP.NET MVC フレームワークには、ヘルパーの小さなセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="79523-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="79523-147">ほとんどの場合、カスタム HTML ヘルパーの MVC フレームワークを拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79523-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="79523-148">このチュートリアルの残りの部分では、カスタム HTML ヘルパーの作成の 2 つの方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="79523-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="79523-149">**2 – を一覧表示します。 `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="79523-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="79523-150">静的メソッドで HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="79523-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="79523-151">新しい HTML ヘルパーを作成する最も簡単な方法では、文字列を返す静的メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="79523-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="79523-152">HTML をレンダリングする新しい HTML ヘルパーの作成を決定することなど想像`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="79523-153">表示するために、リスト 2 でクラスを使用することができます、`<label>`します。</span><span class="sxs-lookup"><span data-stu-id="79523-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="79523-154">**2 – を一覧表示します。 `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="79523-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="79523-155">リスト 2 でクラスについて特別なものがあります。</span><span class="sxs-lookup"><span data-stu-id="79523-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="79523-156">`Label()`メソッドは単に文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="79523-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="79523-157">リスト 3 に変更したのインデックス ビューを使用して、 `LabelHelper` HTML をレンダリングする`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="79523-158">ビューが含まれていますが、`<%@ imports %>`インポート ディレクティブ、`Application1.Helpers`名前空間。</span><span class="sxs-lookup"><span data-stu-id="79523-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="79523-159">**2 – を一覧表示します。 `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="79523-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="79523-160">拡張メソッドで HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="79523-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="79523-161">作成する場合のみ動作する HTML ヘルパーなどの拡張メソッドを作成する必要があります、ASP.NET MVC フレームワークに含まれる標準の HTML ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="79523-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="79523-162">拡張メソッドを使用すると、既存のクラスに新しいメソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="79523-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="79523-163">HTML ヘルパー メソッドを作成するには、新しいメソッドがビューの Html プロパティによって表される HtmlHelper クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="79523-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="79523-164">リスト 3 のクラスにメソッドを拡張機能の追加、`HtmlHelper`という名前のクラス`Label()`します。</span><span class="sxs-lookup"><span data-stu-id="79523-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="79523-165">このクラスについて注意すべきいくつかがあります。</span><span class="sxs-lookup"><span data-stu-id="79523-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="79523-166">最初に、クラスが静的クラスであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79523-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="79523-167">静的クラスを使用して拡張メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79523-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="79523-168">次に、注意の最初のパラメーター、`Label()`メソッドには、キーワードが付いて`this`します。</span><span class="sxs-lookup"><span data-stu-id="79523-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="79523-169">拡張メソッドの最初のパラメーターでは、拡張メソッドを拡張するクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="79523-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="79523-170">**3 – を一覧表示します。 `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="79523-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="79523-171">拡張メソッドがすべてのクラスの他のメソッドのように Visual Studio の Intellisense に表示されます、拡張メソッドを作成し、アプリケーションを正常にビルドした後 (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="79523-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="79523-172">唯一の違いは、その拡張機能メソッドがそれら (下向きの矢印のアイコン) の横にある特別なシンボルを表示します。</span><span class="sxs-lookup"><span data-stu-id="79523-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="79523-173">[![Html.Label() の拡張メソッドを使用](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="79523-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="79523-174">**図 02**: Html.Label() の拡張メソッドを使用して ([フルサイズの画像を表示する をクリックします](creating-custom-html-helpers-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="79523-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="79523-175">リスト 4 変更後のインデックス ビューのすべてを表示するために、Html.Label() 拡張メソッドを使用してその`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="79523-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="79523-176">**4 – を一覧表示します。 `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="79523-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="79523-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="79523-177">Summary</span></span>

<span data-ttu-id="79523-178">このチュートリアルでは、カスタム HTML ヘルパーの作成の 2 つの方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="79523-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="79523-179">最初に、カスタムを作成する方法を学習しました。`Label()`静的メソッドを作成して HTML ヘルパーは、文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="79523-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="79523-180">次に、カスタムを作成する方法を学習しました。 `Label()` HTML ヘルパー メソッドで拡張メソッドを作成して、`HtmlHelper`クラス。</span><span class="sxs-lookup"><span data-stu-id="79523-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="79523-181">このチュートリアルでは、非常に単純な HTML ヘルパー メソッドの構築に注目しました。</span><span class="sxs-lookup"><span data-stu-id="79523-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="79523-182">必要な複雑な作業は HTML ヘルパーであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="79523-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="79523-183">ツリー ビュー、メニューのまたはデータベースのデータのテーブルなどのリッチ コンテンツをレンダリングする HTML ヘルパーをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="79523-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="79523-184">[前へ](asp-net-mvc-views-overview-cs.md)
> [次へ](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="79523-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
