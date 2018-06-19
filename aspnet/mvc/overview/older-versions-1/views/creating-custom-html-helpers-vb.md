---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: カスタム HTML ヘルパー (VB) の作成 |Microsoft ドキュメント
author: microsoft
description: このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。 HTML ヘルパーを活用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 6980026e2653eacb71697f9b34def9bc38638726
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871507"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="c50ba-104">カスタム HTML ヘルパー (VB) の作成</span><span class="sxs-lookup"><span data-stu-id="c50ba-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="c50ba-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c50ba-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="c50ba-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="c50ba-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="c50ba-107">このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="c50ba-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="c50ba-108">HTML ヘルパーを利用して、標準の HTML ページを作成する必要がありますを HTML タグの面倒な入力量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="c50ba-109">このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="c50ba-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="c50ba-110">HTML ヘルパーを利用して、標準の HTML ページを作成する必要がありますを HTML タグの面倒な入力量を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="c50ba-111">このチュートリアルの最初の部分では、ASP.NET MVC フレームワークに含まれている既存の HTML ヘルパーの一部を説明します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="c50ba-112">次に、カスタム HTML ヘルパーの作成の 2 つの方法を説明します。 共有メソッドを作成し、拡張メソッドを作成することで、カスタム HTML ヘルパーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="c50ba-113">HTML ヘルパーを理解します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="c50ba-114">HTML ヘルパーは、文字列を返すメソッドのみです。</span><span class="sxs-lookup"><span data-stu-id="c50ba-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="c50ba-115">文字列は、内容の任意の型を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="c50ba-116">たとえば、HTML などの標準の HTML タグを表示するために HTML ヘルパーを使用できます`<input>`と`<img>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="c50ba-117">タブ ストリップまたはデータベースのデータの HTML テーブルなどのより複雑なコンテンツを表示するために HTML ヘルパーを使用することができますも。</span><span class="sxs-lookup"><span data-stu-id="c50ba-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="c50ba-118">ASP.NET MVC フレームワークには、次の (完全な一覧では無効) 標準の HTML ヘルパーのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c50ba-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="c50ba-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="c50ba-119">Html.ActionLink()</span></span>
- <span data-ttu-id="c50ba-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="c50ba-120">Html.BeginForm()</span></span>
- <span data-ttu-id="c50ba-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="c50ba-121">Html.CheckBox()</span></span>
- <span data-ttu-id="c50ba-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="c50ba-122">Html.DropDownList()</span></span>
- <span data-ttu-id="c50ba-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="c50ba-123">Html.EndForm()</span></span>
- <span data-ttu-id="c50ba-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="c50ba-124">Html.Hidden()</span></span>
- <span data-ttu-id="c50ba-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="c50ba-125">Html.ListBox()</span></span>
- <span data-ttu-id="c50ba-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="c50ba-126">Html.Password()</span></span>
- <span data-ttu-id="c50ba-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="c50ba-127">Html.RadioButton()</span></span>
- <span data-ttu-id="c50ba-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="c50ba-128">Html.TextArea()</span></span>
- <span data-ttu-id="c50ba-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="c50ba-129">Html.TextBox()</span></span>

<span data-ttu-id="c50ba-130">たとえば、1 を一覧表示するフォームがあるとします。</span><span class="sxs-lookup"><span data-stu-id="c50ba-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="c50ba-131">(図 1 を参照してください)、標準の HTML ヘルパーの 2 つのヘルプでは、このフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="c50ba-132">このフォームを使用して、`Html.BeginForm()`と`Html.TextBox()`ヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c50ba-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="c50ba-133">[![HTML ヘルパー ページが表示されます。](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c50ba-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="c50ba-134">**図 01**: HTML ヘルパー ページが表示されます ([フルサイズのイメージを表示するをクリックして](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c50ba-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="c50ba-135">**1 – を一覧表示します。 `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c50ba-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="c50ba-136">`Html.BeginForm()`を始めと終わりの HTML を作成するヘルパー メソッドが使用される`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="c50ba-137">注意して、`Html.BeginForm()`を使用して、内でメソッドが呼び出されたステートメントです。</span><span class="sxs-lookup"><span data-stu-id="c50ba-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="c50ba-138">確実にステートメントを使用して、`<form>`を使用して、最後にタグが閉じられるブロックします。</span><span class="sxs-lookup"><span data-stu-id="c50ba-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="c50ba-139">場合を使用して、作成する代わりに、ブロックを終了する Html.EndForm() ヘルパー メソッドを呼び出すことができます、`<form>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="c50ba-140">タグを作成して、終了するには、どの方法を使用して`<form>`ように最も理解できるタグです。</span><span class="sxs-lookup"><span data-stu-id="c50ba-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="c50ba-141">`Html.TextBox()`ヘルパー メソッドは HTML を表示するリスト 1 で使用される`<input>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="c50ba-142">お使いのブラウザーでソースの表示を選択した場合をリスト 2 での HTML ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="c50ba-143">ソースが標準の HTML タグが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c50ba-144">注意して、 `Html.TextBox()`HTML ヘルパーでレンダリングされて`<%= %>`の代わりにタグ`<% %>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="c50ba-145">等号 (=) を含めない場合は、ブラウザーにレンダリング取得が何も行われません。</span><span class="sxs-lookup"><span data-stu-id="c50ba-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="c50ba-146">ASP.NET MVC フレームワークには、ヘルパーの少数のセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c50ba-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="c50ba-147">ほとんどの場合、カスタム HTML ヘルパーに MVC フレームワークを拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c50ba-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="c50ba-148">このチュートリアルの残りの部分では、カスタム HTML ヘルパーの作成の 2 つの方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="c50ba-149">**2 – を一覧表示します。 `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="c50ba-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="c50ba-150">共有メソッドと HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="c50ba-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="c50ba-151">新しい HTML ヘルパーを作成する最も簡単な方法では、文字列を返す共有メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="c50ba-152">たとえば、HTML をレンダリングする新しい HTML ヘルパーの作成を決定すること`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="c50ba-153">表示するために一覧表示する 2 でクラスを使用することができます、`<label>`です。</span><span class="sxs-lookup"><span data-stu-id="c50ba-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="c50ba-154">**2 – を一覧表示します。 `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="c50ba-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="c50ba-155">2 のリスト内のクラスに関する特別なはありません。</span><span class="sxs-lookup"><span data-stu-id="c50ba-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="c50ba-156">`Label()`メソッドは単に文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="c50ba-157">リスト 3 の変更、インデックス ビューを使用して、 `LabelHelper` HTML をレンダリングする`<label>`タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="c50ba-158">ビューに含まれることに注意してください、 `<%@ imports %>` Application1.Helpers 名前空間をインポートするディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="c50ba-159">**2 – を一覧表示します。 `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c50ba-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="c50ba-160">拡張メソッドで HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="c50ba-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="c50ba-161">作成する場合だけに使用できる HTML ヘルパーなどの拡張メソッドを作成する必要があります、ASP.NET MVC フレームワークに含まれる標準の HTML ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="c50ba-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="c50ba-162">拡張メソッドを使用すると、既存のクラスに新しいメソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="c50ba-163">新しいメソッドを追加する HTML ヘルパー メソッドを作成するときに、`HtmlHelper`クラス ビューの Html プロパティで表されます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="c50ba-164">Visual Basic モジュールを一覧表示する 3 という拡張メソッドを追加する`Label()`を`HtmlHelper`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c50ba-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="c50ba-165">このモジュールに関する注意すべき点が 2 つがあります。</span><span class="sxs-lookup"><span data-stu-id="c50ba-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="c50ba-166">最初に、モジュールで装飾されていることを確認、`<Extension()>`属性。</span><span class="sxs-lookup"><span data-stu-id="c50ba-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="c50ba-167">この属性を使用するためにインポートする必要があります、`System.Runtime.CompilerServices`名前空間</span><span class="sxs-lookup"><span data-stu-id="c50ba-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="c50ba-168">次に、ことに注意しての最初のパラメーター、`Label()`メソッドを表す、`HtmlHelper`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c50ba-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="c50ba-169">拡張メソッドの最初のパラメーターでは、拡張メソッドを拡張するクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="c50ba-170">**3 – を一覧表示します。 `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="c50ba-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="c50ba-171">拡張メソッドがすべてのクラスの他のメソッドと同様に Visual Studio Intellisense に表示されます、拡張メソッドを作成し、アプリケーションを正常にビルドした後 (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c50ba-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="c50ba-172">唯一の違いは、その拡張メソッドは、特別な記号が横に (下向きの矢印のアイコン) で表示されます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="c50ba-173">[![Html.Label() 拡張メソッドの使用](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c50ba-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="c50ba-174">**図 02**: Html.Label() 拡張メソッドの使用 ([フルサイズのイメージを表示するをクリックして](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c50ba-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="c50ba-175">インデックス ビューを一覧表示する 4 でのすべてを表示するために Html.Label() 拡張メソッドを使用してその&lt;ラベル&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="c50ba-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="c50ba-176">**4 – を一覧表示します。 `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c50ba-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="c50ba-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="c50ba-177">Summary</span></span>

<span data-ttu-id="c50ba-178">このチュートリアルでは、カスタム HTML ヘルパーの作成の 2 つの方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="c50ba-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="c50ba-179">最初に、カスタムを作成する方法を学習しました`Label()`共有メソッドを作成することで HTML ヘルパーは、文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="c50ba-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="c50ba-180">次に、カスタムを作成する方法を学習しました`Label()`HTML ヘルパー メソッドの拡張メソッドを作成することで、`HtmlHelper`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c50ba-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="c50ba-181">このチュートリアルでは、非常に単純な HTML ヘルパー メソッドを構築に注目しました。</span><span class="sxs-lookup"><span data-stu-id="c50ba-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="c50ba-182">HTML ヘルパーが必要な複雑な作業できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c50ba-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="c50ba-183">ツリー ビュー、メニューのまたはデータベースのデータのテーブルなどの豊富なコンテンツを表示する HTML ヘルパーをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="c50ba-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c50ba-184">[前へ](asp-net-mvc-views-overview-vb.md)
> [次へ](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c50ba-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
