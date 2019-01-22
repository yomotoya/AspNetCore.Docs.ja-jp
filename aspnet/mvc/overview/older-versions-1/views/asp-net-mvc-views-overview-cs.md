---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: ASP.NET MVC ビュー概要 (c#) |Microsoft Docs
author: StephenWalther
description: ASP.NET MVC ビューし、HTML ページから違う方法でしょうか。 このチュートリアルでは、Stephen Walther がビューを紹介し、t を行う方法を示します.
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: a8e64a99549584f150d64d909ac97210257b1147
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444130"
---
<a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="88463-104">ASP.NET MVC ビュー概要 (c#)</span><span class="sxs-lookup"><span data-stu-id="88463-104">ASP.NET MVC Views Overview (C#)</span></span>
====================
<span data-ttu-id="88463-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="88463-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="88463-106">ASP.NET MVC ビューし、HTML ページから違う方法でしょうか。</span><span class="sxs-lookup"><span data-stu-id="88463-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="88463-107">このチュートリアルでは、Stephen Walther がビューを紹介し、データの表示と HTML ヘルパーをビュー内の利用の実行方法を示します。</span><span class="sxs-lookup"><span data-stu-id="88463-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="88463-108">このチュートリアルの目的は、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を提供するためにです。</span><span class="sxs-lookup"><span data-stu-id="88463-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="88463-109">このチュートリアルの目的は、新しいビューを作成、コント ローラーからビューにデータを渡すし、HTML ヘルパーを使用して、ビューでコンテンツを生成する方法を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88463-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="88463-110">ビューについて</span><span class="sxs-lookup"><span data-stu-id="88463-110">Understanding Views</span></span>

<span data-ttu-id="88463-111">ASP.NET または Active Server Pages、ASP.NET MVC は含まれません、ページに直接対応するものです。</span><span class="sxs-lookup"><span data-stu-id="88463-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="88463-112">ASP.NET MVC アプリケーションでページがあるないブラウザーのアドレス バーに入力した URL のパスに対応するディスク上。</span><span class="sxs-lookup"><span data-stu-id="88463-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="88463-113">ASP.NET MVC アプリケーションのページに最も近いものは何かと呼ばれる、*ビュー*します。</span><span class="sxs-lookup"><span data-stu-id="88463-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="88463-114">ASP.NET MVC アプリケーションでは、受信ブラウザー要求はコント ローラー アクションにマップされます。</span><span class="sxs-lookup"><span data-stu-id="88463-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="88463-115">コント ローラー アクションには、ビューを返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88463-115">A controller action might return a view.</span></span> <span data-ttu-id="88463-116">ただし、コント ローラーのアクションは、他の種類を別のコント ローラー アクションへのリダイレクトなどのアクションを実行する場合があります。</span><span class="sxs-lookup"><span data-stu-id="88463-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="88463-117">1 を一覧表示するには、シンプルなコント ローラー、HomeController という名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="88463-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="88463-118">HomeController は Index() Details() という 2 つのコント ローラー アクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="88463-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="88463-119">**1 - HomeController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="88463-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="88463-120">ブラウザーのアドレス バーに次の URL を入力して、最初のアクションで、Index() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="88463-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="88463-121">/ホーム/インデックス</span><span class="sxs-lookup"><span data-stu-id="88463-121">/Home/Index</span></span>

<span data-ttu-id="88463-122">お使いのブラウザーにこのアドレスを入力し、2 番目のアクション、Details() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="88463-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="88463-123">/ホーム/詳細</span><span class="sxs-lookup"><span data-stu-id="88463-123">/Home/Details</span></span>

<span data-ttu-id="88463-124">Index() アクションでは、ビューを返します。</span><span class="sxs-lookup"><span data-stu-id="88463-124">The Index() action returns a view.</span></span> <span data-ttu-id="88463-125">ほとんどのアクションを作成するには、ビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="88463-125">Most actions that you create will return views.</span></span> <span data-ttu-id="88463-126">ただし、アクションは、他の種類のアクションの結果を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="88463-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="88463-127">たとえば、Details() アクションは、Index() アクションに受信要求をリダイレクトする RedirectToActionResult を返します。</span><span class="sxs-lookup"><span data-stu-id="88463-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="88463-128">Index() アクションには、次の 1 行コードにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="88463-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="88463-129">View();</span><span class="sxs-lookup"><span data-stu-id="88463-129">View();</span></span>

<span data-ttu-id="88463-130">次のコードでは、web サーバーで次のパスに配置する必要があるビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="88463-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="88463-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="88463-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="88463-132">ビューへのパスは、コント ローラーの名前とコント ローラー アクションの名前から推測されます。</span><span class="sxs-lookup"><span data-stu-id="88463-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="88463-133">場合は、ビューの明示的に指定できます。</span><span class="sxs-lookup"><span data-stu-id="88463-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="88463-134">次のコード行には、Fred という名前のビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="88463-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="88463-135">View( Fred );</span><span class="sxs-lookup"><span data-stu-id="88463-135">View( Fred );</span></span>

<span data-ttu-id="88463-136">次のコードが実行されたときに、次のパスからビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="88463-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="88463-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="88463-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="88463-138">ASP.NET MVC アプリケーション用の単体テストを作成する場合は、ビューの名前について明示的に指定することをお勧めです。</span><span class="sxs-lookup"><span data-stu-id="88463-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="88463-139">これにより、必要なビューがコント ローラーのアクションによって返されたことを確認する単体テストを作成できます。</span><span class="sxs-lookup"><span data-stu-id="88463-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="88463-140">コンテンツは、ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="88463-140">Adding Content to a View</span></span>

<span data-ttu-id="88463-141">ビューは、(X) スクリプトを含むことのできる HTML ドキュメントの標準です。</span><span class="sxs-lookup"><span data-stu-id="88463-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="88463-142">スクリプトを使用して、動的なコンテンツをビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="88463-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="88463-143">たとえば、リスト 2 でのビューには、現在の日付と時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="88463-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="88463-144">**Listing 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="88463-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="88463-145">リスト 2 で HTML ページの本文が次のスクリプトを含むことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88463-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="88463-146">&lt;%Response.write(datetime.now); %&gt;</span><span class="sxs-lookup"><span data-stu-id="88463-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="88463-147">スクリプトの区切り記号を使用する&lt;% と %&gt;先頭とスクリプトの末尾をマークします。</span><span class="sxs-lookup"><span data-stu-id="88463-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="88463-148">このスクリプトは、c# で記述されます。</span><span class="sxs-lookup"><span data-stu-id="88463-148">This script is written in C#.</span></span> <span data-ttu-id="88463-149">ブラウザーにコンテンツをレンダリングする Response.Write() メソッドを呼び出して、現在の日付と時刻を表示します。</span><span class="sxs-lookup"><span data-stu-id="88463-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="88463-150">スクリプトの区切り記号&lt;% と %&gt; 1 つまたは複数のステートメントを実行するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="88463-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="88463-151">Response.Write() を呼び出すことが多いためので、Microsoft でショートカット Response.Write() メソッドを呼び出すため。</span><span class="sxs-lookup"><span data-stu-id="88463-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="88463-152">リスト 3 のビューで使用する区切り記号&lt;% =、%&gt; Response.Write() を呼び出すためのショートカットとして。</span><span class="sxs-lookup"><span data-stu-id="88463-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="88463-153">**Listing 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="88463-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="88463-154">任意の .NET 言語を使用して、ビューでの動的なコンテンツを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="88463-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="88463-155">通常、いずれかの Visual Basic .NET を使用またはC#コント ローラーとビューを記述します。</span><span class="sxs-lookup"><span data-stu-id="88463-155">Normally, you'll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="88463-156">HTML ヘルパーを使用して、コンテンツの表示を生成するには</span><span class="sxs-lookup"><span data-stu-id="88463-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="88463-157">コンテンツ ビューを追加する容易にできるようにと呼ばれるものの利用を行う、 *HTML ヘルパー*します。</span><span class="sxs-lookup"><span data-stu-id="88463-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="88463-158">通常、HTML ヘルパーは、文字列を生成する方法です。</span><span class="sxs-lookup"><span data-stu-id="88463-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="88463-159">HTML ヘルパーを使用して、テキスト ボックス、リンク、ドロップダウン リスト、リスト ボックスなどの標準の HTML 要素を生成することができます。</span><span class="sxs-lookup"><span data-stu-id="88463-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="88463-160">例では、リスト 4 - 3 つの HTML ヘルパーを利用で、ビューのログインを生成する--BeginForm()、TextBox() および Password() ヘルパーは、(図 1 参照) を形成します。</span><span class="sxs-lookup"><span data-stu-id="88463-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="88463-161">**Listing 4 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="88463-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


<span data-ttu-id="88463-162">[![[新しいプロジェクト] ダイアログ ボックス](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="88463-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="88463-163">**図 01**:標準ログイン フォーム ([フルサイズの画像を表示する をクリックします](asp-net-mvc-views-overview-cs/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="88463-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="88463-164">ビューの Html プロパティでは、すべての HTML ヘルパー メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="88463-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="88463-165">たとえば、テキスト ボックスを表示するには Html.TextBox() メソッドを呼び出してなります。</span><span class="sxs-lookup"><span data-stu-id="88463-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="88463-166">スクリプトの区切り記号を使用することに注意してください&lt;% =、%&gt; Html.TextBox() と Html.Password() ヘルパーを呼び出すときにします。</span><span class="sxs-lookup"><span data-stu-id="88463-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="88463-167">これらのヘルパーは、単に文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="88463-167">These helpers simply return a string.</span></span> <span data-ttu-id="88463-168">Response.Write() をブラウザーに文字列を表示するために呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="88463-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="88463-169">HTML ヘルパー メソッドを使用することは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="88463-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="88463-170">生活を楽 HTML とスクリプトを記述する必要があるの量を減らすことで。</span><span class="sxs-lookup"><span data-stu-id="88463-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="88463-171">ビュー リスト 5 では、HTML ヘルパーを使用せず、リスト 4 ビューとまったく同じ形式をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="88463-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="88463-172">**Listing 5 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="88463-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="88463-173">また、独自の HTML ヘルパーの作成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="88463-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="88463-174">たとえば、HTML テーブルの一連のデータベース レコードを自動的に表示する GridView() ヘルパー メソッドを作成できます。</span><span class="sxs-lookup"><span data-stu-id="88463-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="88463-175">このトピックで、このチュートリアルで説明**カスタム HTML ヘルパーの作成**です。</span><span class="sxs-lookup"><span data-stu-id="88463-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="88463-176">データの表示を使用して、ビューにデータを渡す</span><span class="sxs-lookup"><span data-stu-id="88463-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="88463-177">データの表示を使用して、コント ローラーからビューにデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="88463-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="88463-178">電子メールで送信するパッケージと同様にデータの表示と考えてください。</span><span class="sxs-lookup"><span data-stu-id="88463-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="88463-179">このパッケージを使用して、コント ローラーからビューに渡されるすべてのデータを送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88463-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="88463-180">たとえば、6 の一覧で、コント ローラーは、データを表示するメッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="88463-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="88463-181">**6 - ProductController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="88463-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="88463-182">コント ローラーの ViewData プロパティは、名前と値のペアのコレクションを表します。</span><span class="sxs-lookup"><span data-stu-id="88463-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="88463-183">6 の一覧表示するのには、Index() メソッドは、値 Hello World メッセージをという名前のビュー データ コレクションに項目を追加しました。</span><span class="sxs-lookup"><span data-stu-id="88463-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="88463-184">ビューが Index() メソッドによって返されると、データの表示は自動的に、ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="88463-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="88463-185">リスト 7 でのビューでは、データの表示から、メッセージを取得し、ブラウザーにメッセージをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="88463-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="88463-186">**Listing 7 -- \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="88463-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="88463-187">Html.Encode() HTML ヘルパーの方法の利点をメッセージを表示するときに、表示にかかることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="88463-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="88463-188">Html.Encode() の HTML ヘルパーなどの特殊文字をエンコード&lt;と&gt;に安全に web ページに表示される文字。</span><span class="sxs-lookup"><span data-stu-id="88463-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="88463-189">ユーザーが web サイトに送信されるコンテンツをレンダリングするときに、JavaScript インジェクション攻撃を防ぐためにコンテンツをエンコードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="88463-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="88463-190">(ため、自分たちは、「productcontroller」で、実際には不要メッセージをエンコードするメッセージを作成しました。</span><span class="sxs-lookup"><span data-stu-id="88463-190">(Because we created the message ourselves in the ProductController, we don't really need to encode the message.</span></span> <span data-ttu-id="88463-191">ビュー内のデータをビューから取得したコンテンツを表示するときに常に Html.Encode() メソッドを呼び出すことを推奨します。)</span><span class="sxs-lookup"><span data-stu-id="88463-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="88463-192">リスト 7 では、単純な文字列メッセージをコント ローラーからビューに渡すデータの表示の利点をしました。</span><span class="sxs-lookup"><span data-stu-id="88463-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="88463-193">データの表示を使用して、他の種類のビュー コント ローラーからのデータベース レコードのコレクションなどのデータを渡すことができますも。</span><span class="sxs-lookup"><span data-stu-id="88463-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="88463-194">たとえば、データベースのコレクションを渡す場合、ビューでは、製品のデータベース テーブルの内容を表示したい場合表示データ記録します。</span><span class="sxs-lookup"><span data-stu-id="88463-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="88463-195">また、コント ローラーからビューに厳密に型指定されたビュー データを渡すことがあります。</span><span class="sxs-lookup"><span data-stu-id="88463-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="88463-196">このトピックで、このチュートリアルで説明**について厳密に型指定されたデータの表示とビュー**します。</span><span class="sxs-lookup"><span data-stu-id="88463-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="88463-197">まとめ</span><span class="sxs-lookup"><span data-stu-id="88463-197">Summary</span></span>

<span data-ttu-id="88463-198">このチュートリアルでは、ASP.NET MVC ビュー、データの表示、および HTML ヘルパーの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="88463-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="88463-199">最初のセクションでは、プロジェクトに新しいビューを追加する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="88463-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="88463-200">ビューを特定のコント ローラーから呼び出すために適切なフォルダーに追加する必要がありますを学習しました。</span><span class="sxs-lookup"><span data-stu-id="88463-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="88463-201">次に、HTML ヘルパーのトピックを説明します。</span><span class="sxs-lookup"><span data-stu-id="88463-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="88463-202">HTML ヘルパーを使用すると、標準の HTML コンテンツを簡単に生成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="88463-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="88463-203">最後に、コント ローラーからビューにデータを渡すデータの表示を活用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="88463-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="88463-204">次へ</span><span class="sxs-lookup"><span data-stu-id="88463-204">Next</span></span>](creating-custom-html-helpers-cs.md)
