---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: "ASP.NET MVC ビューの概要 (c#) |Microsoft ドキュメント"
author: StephenWalther
description: "ASP.NET MVC ビューとどのように違う HTML ページについて このチュートリアルでは、Stephen Walther はビューについて説明し、t する方法を示しています。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de095b0621af3b6166a2e1cbcb1c63c26a88aa2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="bc14e-104">ASP.NET MVC ビューの概要 (c#)</span><span class="sxs-lookup"><span data-stu-id="bc14e-104">ASP.NET MVC Views Overview (C#)</span></span>
====================
<span data-ttu-id="bc14e-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="bc14e-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="bc14e-106">ASP.NET MVC ビューとどのように違う HTML ページについて</span><span class="sxs-lookup"><span data-stu-id="bc14e-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="bc14e-107">このチュートリアルでは、Stephen Walther はビューについて説明しを利用できるかのデータの表示と、ビュー内の HTML ヘルパーを示します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="bc14e-108">このチュートリアルの目的は、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を説明します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="bc14e-109">このチュートリアルの目的は、新しいビューを作成、コント ローラーからビューにデータを渡すし、HTML ヘルパーを使用して、ビューのコンテンツを生成する方法を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="bc14e-110">ビューについて</span><span class="sxs-lookup"><span data-stu-id="bc14e-110">Understanding Views</span></span>

<span data-ttu-id="bc14e-111">ASP.NET または Active Server Pages の場合は、ASP.NET MVC は含まれませんページに直接対応しているもの。</span><span class="sxs-lookup"><span data-stu-id="bc14e-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="bc14e-112">ASP.NET MVC アプリケーションであるページではありません、ブラウザーのアドレス バーに入力した URL のパスに対応するディスク上。</span><span class="sxs-lookup"><span data-stu-id="bc14e-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="bc14e-113">ASP.NET MVC アプリケーションのページに最も近いものは何かと呼ばれる、*ビュー*です。</span><span class="sxs-lookup"><span data-stu-id="bc14e-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="bc14e-114">ASP.NET MVC がブラウザー アプリケーション、入力方向の要求は、コント ローラーのアクションにマップされます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-114">ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="bc14e-115">コント ローラーのアクションは、ビューを返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-115">A controller action might return a view.</span></span> <span data-ttu-id="bc14e-116">ただし、コント ローラーのアクションは、他のなんらかのアクションを別のコント ローラー アクションへのリダイレクトなどを実行する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="bc14e-117">1 を一覧表示するには、という名前のテンプレートを使用する単純なコント ローラーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="bc14e-118">HomeController は、Index() および Details() という 2 つのコント ローラーのアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="bc14e-119">**1 - HomeController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="bc14e-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="bc14e-120">ブラウザーのアドレス バーに次の URL を入力して、最初のアクションで Index() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="bc14e-121">/Home/index</span><span class="sxs-lookup"><span data-stu-id="bc14e-121">/Home/Index</span></span>

<span data-ttu-id="bc14e-122">お使いのブラウザーにこのアドレスを入力して、2 つ目の操作を Details() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="bc14e-123">/ホーム/詳細</span><span class="sxs-lookup"><span data-stu-id="bc14e-123">/Home/Details</span></span>

<span data-ttu-id="bc14e-124">Index() アクションは、ビューを返します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-124">The Index() action returns a view.</span></span> <span data-ttu-id="bc14e-125">ほとんどのアクションを作成するには、ビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-125">Most actions that you create will return views.</span></span> <span data-ttu-id="bc14e-126">ただし、アクションは、他の種類のアクションの結果を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="bc14e-127">たとえば、Details() アクションには、Index() アクションに着信要求をリダイレクトする RedirectToActionResult が返されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="bc14e-128">Index() アクションには、次の 1 行コードにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="bc14e-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="bc14e-129">View() です。</span><span class="sxs-lookup"><span data-stu-id="bc14e-129">View();</span></span>

<span data-ttu-id="bc14e-130">次のコード行には、web サーバーで次のパスに配置する必要があるビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="bc14e-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="bc14e-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="bc14e-132">ビューへのパスは、コント ローラーの名前とコント ローラー アクションの名前から推測されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="bc14e-133">場合は、ビューの明示的に指定できます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="bc14e-134">次のコード行には、Fred をという名前のビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="bc14e-135">表示 (Fred) です。</span><span class="sxs-lookup"><span data-stu-id="bc14e-135">View( Fred );</span></span>

<span data-ttu-id="bc14e-136">次のコード行が実行されると、次のパスからビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="bc14e-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="bc14e-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="bc14e-138">ASP.NET MVC アプリケーションの単体テストを作成する場合は、ビュー名は明示的に指定することをお勧めです。</span><span class="sxs-lookup"><span data-stu-id="bc14e-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="bc14e-139">このように、必要なビューがコント ローラーのアクションによって返されることを確認する単体テストを作成できます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="bc14e-140">コンテンツは、ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-140">Adding Content to a View</span></span>

<span data-ttu-id="bc14e-141">ビューは、標準の (X) スクリプトを含めることができる HTML ドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="bc14e-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="bc14e-142">ビューに動的なコンテンツを追加するのにには、スクリプトを使用します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="bc14e-143">たとえば、2 の一覧表示するビューには、現在の日付と時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="bc14e-144">**2 - を一覧表示する \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="bc14e-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="bc14e-145">2 を一覧表示する HTML ページの本文に次のスクリプトが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="bc14e-146">&lt;%Response.write(datetime.now);&gt;</span><span class="sxs-lookup"><span data-stu-id="bc14e-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="bc14e-147">スクリプトの区切り記号を使用する&lt;% と %&gt;先頭とスクリプトの末尾をマークします。</span><span class="sxs-lookup"><span data-stu-id="bc14e-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="bc14e-148">このスクリプトは c# で記述されています。</span><span class="sxs-lookup"><span data-stu-id="bc14e-148">This script is written in C#.</span></span> <span data-ttu-id="bc14e-149">ブラウザーにコンテンツを表示するために Response.Write() メソッドを呼び出して、現在の日付と時刻を表示します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="bc14e-150">スクリプトの区切り記号&lt;% と %&gt; 1 つまたは複数のステートメントを実行するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="bc14e-151">Response.Write() を頻繁に呼び出すため Microsoft が提供ショートカット Response.Write() メソッドを呼び出すためです。</span><span class="sxs-lookup"><span data-stu-id="bc14e-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="bc14e-152">ビューを一覧表示する 3 で、区切り記号を使用して&lt;% = %&gt; Response.Write() を呼び出すためのショートカットとして。</span><span class="sxs-lookup"><span data-stu-id="bc14e-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="bc14e-153">**3 - Views\Home\Index2.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="bc14e-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="bc14e-154">ビューでの動的なコンテンツを生成するのに任意の .NET 言語を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="bc14e-155">通常、する ll を使用して Visual Basic .NET や C# の場合、コント ローラーとビューを記述します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="bc14e-156">HTML ヘルパーを使用して、コンテンツの表示を生成するには</span><span class="sxs-lookup"><span data-stu-id="bc14e-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="bc14e-157">コンテンツ ビューを追加する容易にできるようにを利用すると呼ばれるものの*HTML ヘルパー*です。</span><span class="sxs-lookup"><span data-stu-id="bc14e-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="bc14e-158">通常は、HTML ヘルパーの場合は、文字列を生成する方法です。</span><span class="sxs-lookup"><span data-stu-id="bc14e-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="bc14e-159">HTML ヘルパーを使用して、テキスト ボックス、リンク、ドロップダウン リスト、およびリスト ボックスなどの標準の HTML 要素を生成することができます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="bc14e-160">例については、ビューを一覧表示する 4 - 3 つの HTML ヘルパーの利用には、ログインを生成する--BeginForm() TextBox()、Password() ヘルパーは、(図 1 を参照してください) を形成します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="bc14e-161">**リスト 4--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="bc14e-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


<span data-ttu-id="bc14e-162">[![[新しいプロジェクト] ダイアログ ボックス](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bc14e-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="bc14e-163">**図 01**: 標準的なログイン フォーム ([フルサイズのイメージを表示するをクリックして](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="bc14e-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="bc14e-164">ビューの Html プロパティでは、すべての HTML ヘルパー メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="bc14e-165">たとえば、テキスト ボックスをレンダリングする Html.TextBox() メソッドを呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="bc14e-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="bc14e-166">スクリプトの区切り記号を使用することに注意してください&lt;% = %&gt; Html.TextBox() と Html.Password() の両方のヘルパーを呼び出すときにします。</span><span class="sxs-lookup"><span data-stu-id="bc14e-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="bc14e-167">これらのヘルパーは、単に文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-167">These helpers simply return a string.</span></span> <span data-ttu-id="bc14e-168">ブラウザーに文字列を表示するために Response.Write() を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="bc14e-169">HTML ヘルパー メソッドを使用することはオプションです。</span><span class="sxs-lookup"><span data-stu-id="bc14e-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="bc14e-170">楽に、HTML および記述する必要があるスクリプトの量を減らすことによってです。</span><span class="sxs-lookup"><span data-stu-id="bc14e-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="bc14e-171">ビューを一覧表示する 5 には、HTML ヘルパーを使用せず、正確なと同じ形式を一覧表示する 4 でビューをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="bc14e-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="bc14e-172">**5 - を一覧表示する \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="bc14e-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="bc14e-173">また、独自の HTML ヘルパーの作成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="bc14e-174">たとえば、HTML テーブルの一連のデータベース レコードを自動的に表示する GridView() ヘルパー メソッドを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="bc14e-175">チュートリアルのこのトピックの内容をナビゲート**カスタム HTML ヘルパーの作成**です。</span><span class="sxs-lookup"><span data-stu-id="bc14e-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="bc14e-176">データの表示を使用して、ビューにデータを渡す</span><span class="sxs-lookup"><span data-stu-id="bc14e-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="bc14e-177">データの表示を使用して、コント ローラーからビューにデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="bc14e-178">電子メールで送信するパッケージと同様にデータの表示と考えてください。</span><span class="sxs-lookup"><span data-stu-id="bc14e-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="bc14e-179">このパッケージを使用して、コント ローラーからビューに渡されるすべてのデータを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="bc14e-180">たとえば、リスト 6 でのコント ローラーは、データを表示するメッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="bc14e-181">**6 - ProductController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="bc14e-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="bc14e-182">コント ローラー ViewData プロパティは、名前と値のペアのコレクションを表します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="bc14e-183">6 の一覧表示するのには、Index() メソッドは、Hello World の値を持つメッセージをという名前のビュー データ コレクションに項目を追加しました。</span><span class="sxs-lookup"><span data-stu-id="bc14e-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="bc14e-184">ビューが Index() メソッドによって返されると、データの表示は自動的にビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="bc14e-185">ビューを一覧表示する 7 には、ビュー データからのメッセージを取得し、ブラウザーにメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="bc14e-186">**7--を一覧表示する \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="bc14e-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="bc14e-187">ビューを活用 Html.Encode() HTML ヘルパー メソッドのメッセージを表示するときに注意してください。</span><span class="sxs-lookup"><span data-stu-id="bc14e-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="bc14e-188">Html.Encode() HTML ヘルパーがなどの特殊文字をエンコード&lt;と&gt;に安全に web ページに表示される文字。</span><span class="sxs-lookup"><span data-stu-id="bc14e-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="bc14e-189">ユーザーが web サイトに送信するコンテンツをレンダリングするときに、JavaScript インジェクション攻撃を防ぐためにコンテンツをエンコードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="bc14e-190">(T ありませんおで作成されるため、メッセージ自身、ProductController、実際にメッセージをエンコードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="bc14e-191">ただしは常にメソッドを呼び出す Html.Encode() ビュー内のビュー データから取得されたコンテンツを表示するときに推奨します。)</span><span class="sxs-lookup"><span data-stu-id="bc14e-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="bc14e-192">リスト 7 は、単純な文字列メッセージをコント ローラーからビューに渡すデータの表示の利点をかかりました。</span><span class="sxs-lookup"><span data-stu-id="bc14e-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="bc14e-193">データの表示を使用して、他のビューにコント ローラーからのデータベース レコードのコレクションなどのデータ型を渡すことができますも。</span><span class="sxs-lookup"><span data-stu-id="bc14e-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="bc14e-194">たとえば、データベースのコレクションを渡す場合、ビューでは、製品のデータベース テーブルの内容を表示したい場合ビューでデータ記録されます。</span><span class="sxs-lookup"><span data-stu-id="bc14e-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="bc14e-195">また、コント ローラーからビューを厳密に型指定されたビュー データを渡すオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="bc14e-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="bc14e-196">チュートリアルのこのトピックの内容をナビゲート**Understanding 厳密に型指定されたデータの表示やビュー**です。</span><span class="sxs-lookup"><span data-stu-id="bc14e-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="bc14e-197">概要</span><span class="sxs-lookup"><span data-stu-id="bc14e-197">Summary</span></span>

<span data-ttu-id="bc14e-198">このチュートリアルでは、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="bc14e-199">最初のセクションでは、プロジェクトに新しいビューを追加する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="bc14e-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="bc14e-200">ビューを特定のコント ローラーから呼び出すために適切なフォルダーに追加する必要がありますを学習しました。</span><span class="sxs-lookup"><span data-stu-id="bc14e-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="bc14e-201">次に、HTML ヘルパーのトピックを説明します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="bc14e-202">HTML ヘルパーを使用すると、標準の HTML コンテンツを簡単に生成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="bc14e-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="bc14e-203">最後に、コント ローラーからビューにデータを渡すデータの表示を活用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="bc14e-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="bc14e-204">次へ</span><span class="sxs-lookup"><span data-stu-id="bc14e-204">Next</span></span>](creating-custom-html-helpers-cs.md)
