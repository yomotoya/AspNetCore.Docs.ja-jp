---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: "Introduction to デバッグの ASP.NET Web Pages (Razor) サイト |Microsoft ドキュメント"
author: tfitzmac
description: "デバッグは、検索、およびコード ページにエラーの修正のプロセスです。 この章ではするが表示されるツールやデバッグに使用できる手法と analyz をしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: 0b6b5a886efe515b434948dade1ae840ddaecd42
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a><span data-ttu-id="66f32-104">Introduction to デバッグの ASP.NET Web Pages (Razor) サイト</span><span class="sxs-lookup"><span data-stu-id="66f32-104">Introduction to Debugging ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="66f32-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="66f32-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="66f32-106">この記事では、ASP.NET Web Pages (Razor) の web サイトにページをデバッグするさまざまな方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="66f32-106">This article explains various ways to debug pages in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="66f32-107">デバッグは、検索、およびコード ページにエラーの修正のプロセスです。</span><span class="sxs-lookup"><span data-stu-id="66f32-107">Debugging is the process of finding and fixing errors in your code pages.</span></span>
> 
> <span data-ttu-id="66f32-108">**学習する内容。**</span><span class="sxs-lookup"><span data-stu-id="66f32-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="66f32-109">ときに役立つ情報を表示する方法は、分析して、ページをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="66f32-109">How to display information that helps analyze and debug pages.</span></span>
> - <span data-ttu-id="66f32-110">デバッグを使用する方法は、Visual Studio のツールです。</span><span class="sxs-lookup"><span data-stu-id="66f32-110">How to use debugging tools in Visual Studio.</span></span>
>   
> 
> <span data-ttu-id="66f32-111">アーティクルで導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="66f32-111">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="66f32-112">`ServerInfo`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="66f32-112">The `ServerInfo` helper.</span></span>
> - <span data-ttu-id="66f32-113">`ObjectInfo`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="66f32-113">`ObjectInfo` helper.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="66f32-114">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="66f32-114">Software versions</span></span>
> 
> 
> - <span data-ttu-id="66f32-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="66f32-115">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="66f32-116">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="66f32-116">Visual Studio 2013</span></span>
>   
> 
> <span data-ttu-id="66f32-117">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="66f32-117">This tutorial also works with ASP.NET Web Pages 2.</span></span> <span data-ttu-id="66f32-118">WebMatrix 3 を使用できますが、統合されたデバッガーがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="66f32-118">You can use WebMatrix 3 but the integrated debugger is not supported.</span></span>


<span data-ttu-id="66f32-119">エラーと、コード内の問題のトラブルシューティングの重要な側面は、まずそれを回避することです。</span><span class="sxs-lookup"><span data-stu-id="66f32-119">An important aspect of troubleshooting errors and problems in your code is to avoid them in the first place.</span></span> <span data-ttu-id="66f32-120">エラーが発生する可能性のあるコードのセクションを設定することによって行うことができます`try/catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="66f32-120">You can do that by putting sections of your code that are likely to cause errors into `try/catch` blocks.</span></span> <span data-ttu-id="66f32-121">詳細については、上でのエラー処理セクションを参照してください。 [Introduction to ASP.NET Web プログラミング構文を使用して、Razor](https://go.microsoft.com/fwlink/?LinkId=202890)です。</span><span class="sxs-lookup"><span data-stu-id="66f32-121">For more information, see the section on handling errors in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890).</span></span>

<span data-ttu-id="66f32-122">`ServerInfo`ヘルパーは、ページをホストする web サーバー環境に関する情報の概要を説明する診断ツールです。</span><span class="sxs-lookup"><span data-stu-id="66f32-122">The `ServerInfo` helper is a diagnostic tool that gives you an overview of information about the web server environment that hosts your page.</span></span> <span data-ttu-id="66f32-123">ブラウザーがページを要求時に送信される HTTP 要求の情報も示します。</span><span class="sxs-lookup"><span data-stu-id="66f32-123">It also shows you HTTP request information that's sent when a browser requests the page.</span></span> <span data-ttu-id="66f32-124">`ServerInfo`ヘルパーは、現在のユーザー id の要求を行ったブラウザーの種類を表示にします。</span><span class="sxs-lookup"><span data-stu-id="66f32-124">The `ServerInfo` helper displays the current user identity, the type of browser that made the request, and so on.</span></span> <span data-ttu-id="66f32-125">このような情報は、一般的な問題をトラブルシューティングするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="66f32-125">This kind of information can help you troubleshoot common issues.</span></span>

1. <span data-ttu-id="66f32-126">という名前の新しい web ページを作成する*ServerInfo.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="66f32-126">Create a new web page named *ServerInfo.cshtml*.</span></span>
2. <span data-ttu-id="66f32-127">ページの最後に、直前の終了`</body>`タグ、追加`@ServerInfo.GetHtml()`:</span><span class="sxs-lookup"><span data-stu-id="66f32-127">At the end of the page, just before the closing `</body>` tag, add `@ServerInfo.GetHtml()`:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    <span data-ttu-id="66f32-128">追加することができます、`ServerInfo`ページ内の任意の場所のコード。</span><span class="sxs-lookup"><span data-stu-id="66f32-128">You can add the `ServerInfo` code anywhere in the page.</span></span> <span data-ttu-id="66f32-129">末尾に追加すると、その出力から切り離しておきます、他のページ コンテンツを読みやすくなります。</span><span class="sxs-lookup"><span data-stu-id="66f32-129">But adding it at the end will keep its output separate from your other page content, which makes it easier to read.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="66f32-130">**重要な**実稼働サーバーに web ページを移動する前に、web ページからの診断コードを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="66f32-130">**Important** You should remove any diagnostic code from your web pages before you move web pages to a production server.</span></span> <span data-ttu-id="66f32-131">これに適用されます、`ServerInfo`ヘルパーだけでなく、他の診断方法この記事の内容を含むコード ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="66f32-131">This applies to the `ServerInfo` helper as well as the other diagnostic techniques in this article that involve adding code to a page.</span></span> <span data-ttu-id="66f32-132">この種の情報が悪意のある方に役立つ可能性があるために、サーバー、そのような詳細については、サーバー名、ユーザー名、パスに関する情報を表示する、web サイトの訪問者をしたくありません。</span><span class="sxs-lookup"><span data-stu-id="66f32-132">You don't want your website visitors to see information about your server name, user names, paths on your server, and similar details, because this type of information might be useful to people with malicious intent.</span></span>
3. <span data-ttu-id="66f32-133">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="66f32-133">Save the page and run it in a browser.</span></span>

    ![デバッグ 1](introduction-to-debugging/_static/image1.jpg)

    <span data-ttu-id="66f32-135">`ServerInfo`ヘルパー ページの情報の 4 つのテーブルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="66f32-135">The `ServerInfo` helper displays four tables of information in the page:</span></span>

    - <span data-ttu-id="66f32-136">サーバーの構成。</span><span class="sxs-lookup"><span data-stu-id="66f32-136">Server Configuration.</span></span> <span data-ttu-id="66f32-137">このセクションでは、コンピューター名、バージョンの ASP.NET を実行している、ドメイン名、およびサーバーの時刻を含む、ホストする web サーバーに関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="66f32-137">This section provides information about the hosting web server, including computer name, the version of ASP.NET you're running, the domain name, and server time.</span></span>
    - <span data-ttu-id="66f32-138">ASP.NET サーバー変数。</span><span class="sxs-lookup"><span data-stu-id="66f32-138">ASP.NET Server Variables.</span></span> <span data-ttu-id="66f32-139">このセクションでは、さまざまな HTTP プロトコル詳細 (と呼ばれる HTTP 変数) の詳細を説明し、値を各 web ページ要求の一部であります。</span><span class="sxs-lookup"><span data-stu-id="66f32-139">This section provides details about the many HTTP protocol details (called HTTP variables) and values that are part of each web page request.</span></span>
    - <span data-ttu-id="66f32-140">HTTP ランタイム情報。</span><span class="sxs-lookup"><span data-stu-id="66f32-140">HTTP Runtime Information.</span></span> <span data-ttu-id="66f32-141">このセクションでは、バージョンの web ページが実行されている Microsoft .NET Framework では、パス、キャッシュについての詳細については、について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="66f32-141">This section provides details about that the version of the Microsoft .NET Framework that your web page is running under, the path, details about the cache, and so on.</span></span> <span data-ttu-id="66f32-142">(で学習したよう[Introduction to ASP.NET Web プログラミング構文を使用して、Razor](https://go.microsoft.com/fwlink/?LinkId=202890)構文が広範なソフトウェア上に構築された自体である Microsoft の ASP.NET web サーバー テクノロジに基づいて構築されて Razor を使用して ASP.NET Web ページ開発ライブラリ、.NET Framework と呼ばれます。)</span><span class="sxs-lookup"><span data-stu-id="66f32-142">(As you learned in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web Pages using the Razor syntax are built on Microsoft's ASP.NET web server technology, which is itself built on an extensive software development library called the .NET Framework.)</span></span>
    - <span data-ttu-id="66f32-143">環境変数。</span><span class="sxs-lookup"><span data-stu-id="66f32-143">Environment Variables.</span></span> <span data-ttu-id="66f32-144">このセクションでは、web サーバーですべてのローカルの環境変数とその値の一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="66f32-144">This section provides a list of all the local environment variables and their values on the web server.</span></span>

    <span data-ttu-id="66f32-145">すべてのサーバーと要求情報の詳細についてはこの記事の範囲外ですがあることがわかります、`ServerInfo`ヘルパーは、多くの診断情報を返します。</span><span class="sxs-lookup"><span data-stu-id="66f32-145">A full description of all the server and request information is beyond the scope of this article, but you can see that the `ServerInfo` helper returns a lot of diagnostic information.</span></span> <span data-ttu-id="66f32-146">値の詳細についてを`ServerInfo`戻り値を参照してください[環境変数の認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)、Microsoft TechNet web サイトと[IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN web サイトです。</span><span class="sxs-lookup"><span data-stu-id="66f32-146">For more information about the values that `ServerInfo` returns, see [Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) on the Microsoft TechNet website and [IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) on the MSDN website.</span></span>

## <a name="embedding-output-expressions-to-display-page-values"></a><span data-ttu-id="66f32-147">埋め込み式をページの値を表示するには</span><span class="sxs-lookup"><span data-stu-id="66f32-147">Embedding Output Expressions to Display Page Values</span></span>

<span data-ttu-id="66f32-148">別の方法で、コードで何が起こっているかを参照してください ページで、出力の式を埋め込むを開始します。</span><span class="sxs-lookup"><span data-stu-id="66f32-148">Another way to see what's happening in your code is to embed output expressions in the page.</span></span> <span data-ttu-id="66f32-149">ようなものを追加することで、変数の値を直接出力ご存じのように、`@myVariable`または`@(subTotal * 12)`ページにします。</span><span class="sxs-lookup"><span data-stu-id="66f32-149">As you know, you can directly output the value of a variable by adding something like `@myVariable` or `@(subTotal * 12)` to the page.</span></span> <span data-ttu-id="66f32-150">をデバッグする場合は、コード内で重要なポイントこれらの式の出力を配置できます。</span><span class="sxs-lookup"><span data-stu-id="66f32-150">For debugging, you can place these output expressions at strategic points in your code.</span></span> <span data-ttu-id="66f32-151">これにより、ページの実行時に、主要な変数またはの計算結果の値を参照してください。</span><span class="sxs-lookup"><span data-stu-id="66f32-151">This enables you to see the value of key variables or the result of calculations when your page runs.</span></span> <span data-ttu-id="66f32-152">終了すると、デバッグ、式を削除したり、コメント アウトします。この手順は、ページをデバッグする際に埋め込み式を使用する一般的な方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="66f32-152">When you're done debugging, you can remove the expressions or comment them out. This procedure illustrates a typical way to use embedded expressions to help debug a page.</span></span>

1. <span data-ttu-id="66f32-153">名前は新しい WebMatrix ページを作成する*OutputExpression.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="66f32-153">Create a new WebMatrix page that's named *OutputExpression.cshtml*.</span></span>
2. <span data-ttu-id="66f32-154">次のページに次のようにコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="66f32-154">Replace the page content with the following:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    <span data-ttu-id="66f32-155">この例では、`switch`の値を確認するステートメント、`weekday`変数とは、週の曜日によって別の出力メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="66f32-155">The example uses a `switch` statement to check the value of the `weekday` variable and then display a different output message depending on which day of the week it is.</span></span> <span data-ttu-id="66f32-156">この例で、`if`最初のコード ブロック内のブロックが 1 日を現在の曜日の値に追加して週の曜日を任意に変更します。</span><span class="sxs-lookup"><span data-stu-id="66f32-156">In the example, the `if` block within the first code block arbitrarily changes the day of the week by adding one day to the current weekday value.</span></span> <span data-ttu-id="66f32-157">これは、わかりやすくするために導入されたエラーです。</span><span class="sxs-lookup"><span data-stu-id="66f32-157">This is an error introduced for illustration purposes.</span></span>
3. <span data-ttu-id="66f32-158">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="66f32-158">Save the page and run it in a browser.</span></span>

    <span data-ttu-id="66f32-159">ページは、間違った曜日のメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="66f32-159">The page displays the message for the wrong day of the week.</span></span> <span data-ttu-id="66f32-160">すべての日、週の実際には、1 日の後で、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="66f32-160">Whatever day of the week it actually is, you'll see the message for one day later.</span></span> <span data-ttu-id="66f32-161">ここではわかっている理由、メッセージがオフ (ため、コードでは、意図的に正しくない日付の値を設定します) が、実際には多くの場合、作業がうまくいかないコード内を知ることが難しい。</span><span class="sxs-lookup"><span data-stu-id="66f32-161">Although in this case you know why the message is off (because the code deliberately sets the incorrect day value), in reality it's often hard to know where things are going wrong in the code.</span></span> <span data-ttu-id="66f32-162">デバッグするには、何が起こっているかの主要なオブジェクトおよび変数の値になどを確認する必要があります`weekday`です。</span><span class="sxs-lookup"><span data-stu-id="66f32-162">To debug, you need to find out what's happening to the value of key objects and variables such as `weekday`.</span></span>
4. <span data-ttu-id="66f32-163">出力の式を挿入することによって追加`@weekday`コード内のコメントで示される 2 つの場所で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="66f32-163">Add output expressions by inserting `@weekday` as shown in the two places indicated by comments in the code.</span></span> <span data-ttu-id="66f32-164">これらの式の出力は、コードの実行に、変数の値をその時点で表示されます。</span><span class="sxs-lookup"><span data-stu-id="66f32-164">These output expressions will display the values of the variable at that point in the code execution.</span></span>

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. <span data-ttu-id="66f32-165">保存し、ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="66f32-165">Save and run the page in a browser.</span></span>

    <span data-ttu-id="66f32-166">ページが、実際の曜日を最初に、表示し、更新された日、週の結果得られる、1 日とし、結果のメッセージからの追加、`switch`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="66f32-166">The page displays the real day of the week first, then the updated day of the week that results from adding one day, and then the resulting message from the `switch` statement.</span></span> <span data-ttu-id="66f32-167">2 つの変数の式からの出力 (`@weekday`)、スペースを含まない、日の間で、任意の HTML を追加していないため`<p>`出力にタグを式は、テストするためだけです。</span><span class="sxs-lookup"><span data-stu-id="66f32-167">The output from the two variable expressions (`@weekday`) has no spaces between the days because you didn't add any HTML `<p>` tags to the output; the expressions are just for testing.</span></span>

    ![デバッグ 2](introduction-to-debugging/_static/image2.jpg)

    <span data-ttu-id="66f32-169">これで、エラーが表示できます。</span><span class="sxs-lookup"><span data-stu-id="66f32-169">Now you can see where the error is.</span></span> <span data-ttu-id="66f32-170">表示するとき、`weekday`コードに変数が、正しい日はすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="66f32-170">When you first display the `weekday` variable in the code, it shows the correct day.</span></span> <span data-ttu-id="66f32-171">表示するときに、2 回目の後に、`if`コードのブロックは、1 日が 1 つではオフです。</span><span class="sxs-lookup"><span data-stu-id="66f32-171">When you display it the second time, after the `if` block in the code, the day is off by one.</span></span> <span data-ttu-id="66f32-172">したがって、曜日の変数の最初と 2 番目の外観の間で問題が発生したことがわかります。</span><span class="sxs-lookup"><span data-stu-id="66f32-172">So you know that something has happened between the first and second appearance of the weekday variable.</span></span> <span data-ttu-id="66f32-173">これが実際のバグの場合、このようなアプローチが役立つ問題の原因となったコードの場所を絞り込みます。</span><span class="sxs-lookup"><span data-stu-id="66f32-173">If this were a real bug, this kind of approach would help you narrow down the location of the code that's causing the problem.</span></span>
6. <span data-ttu-id="66f32-174">ページで、コードを修正して追加した 2 つの出力式の消去を週の曜日を変更するコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="66f32-174">Fix the code in the page by removing the two output expressions you added, and removing the code that changes the day of the week.</span></span> <span data-ttu-id="66f32-175">コードの残り、完全なブロックは、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="66f32-175">The remaining, complete block of code looks like the following example:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. <span data-ttu-id="66f32-176">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="66f32-176">Run the page in a browser.</span></span> <span data-ttu-id="66f32-177">今度は、メッセージが表示正しいを週の実際の日付が表示されます。</span><span class="sxs-lookup"><span data-stu-id="66f32-177">This time you see the correct message displayed for the actual day of the week.</span></span>

## <a name="using-the-objectinfo-helper-to-display-object-values"></a><span data-ttu-id="66f32-178">ObjectInfo ヘルパーを使用してオブジェクトの値を表示するには</span><span class="sxs-lookup"><span data-stu-id="66f32-178">Using the ObjectInfo Helper to Display Object Values</span></span>

<span data-ttu-id="66f32-179">`ObjectInfo`ヘルパーは、型とそれに渡す各オブジェクトの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="66f32-179">The `ObjectInfo` helper displays the type and the value of each object you pass to it.</span></span> <span data-ttu-id="66f32-180">使用できます (前の例で出力式で行った) 同じように、コード内の変数およびオブジェクトの値を表示すると、データ型のオブジェクトに関する情報を確認できます。</span><span class="sxs-lookup"><span data-stu-id="66f32-180">You can use it to view the value of variables and objects in your code (like you did with output expressions in the previous example), plus you can see data type information about the object.</span></span>

1. <span data-ttu-id="66f32-181">という名前のファイルを開く*OutputExpression.cshtml*先ほど作成しました。</span><span class="sxs-lookup"><span data-stu-id="66f32-181">Open the file named *OutputExpression.cshtml* that you created earlier.</span></span>
2. <span data-ttu-id="66f32-182">ページ内のすべてのコードを次のコード ブロックに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="66f32-182">Replace all code in the page with the following block of code:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. <span data-ttu-id="66f32-183">保存し、ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="66f32-183">Save and run the page in a browser.</span></span>

    ![デバッグ 4](introduction-to-debugging/_static/image3.jpg)

    <span data-ttu-id="66f32-185">この例では、`ObjectInfo`ヘルパーには、2 つの項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="66f32-185">In this example, the `ObjectInfo` helper displays two items:</span></span>

    - <span data-ttu-id="66f32-186">型。</span><span class="sxs-lookup"><span data-stu-id="66f32-186">The type.</span></span> <span data-ttu-id="66f32-187">最初の変数の型は`DayOfWeek`します。</span><span class="sxs-lookup"><span data-stu-id="66f32-187">For the first variable, the type is `DayOfWeek`.</span></span> <span data-ttu-id="66f32-188">2 番目の変数の型は`String`します。</span><span class="sxs-lookup"><span data-stu-id="66f32-188">For the second variable, the type is `String`.</span></span>
    - <span data-ttu-id="66f32-189">値。</span><span class="sxs-lookup"><span data-stu-id="66f32-189">The value.</span></span> <span data-ttu-id="66f32-190">この場合、ページに既にのあいさつの変数の値を表示するため、値が表示されますもう一度に変数を渡すときに`ObjectInfo`です。</span><span class="sxs-lookup"><span data-stu-id="66f32-190">In this case, because you already display the value of the greeting variable in the page, the value is displayed again when you pass the variable to `ObjectInfo`.</span></span>

    <span data-ttu-id="66f32-191">複雑なオブジェクトの場合、`ObjectInfo`詳細 &#8212;ヘルパーを表示できます。 型とすべてのオブジェクトのプロパティの値を表示、基本的に、します。</span><span class="sxs-lookup"><span data-stu-id="66f32-191">For more complex objects, the `ObjectInfo` helper can display more information &#8212; basically, it can display the types and values of all of an object's properties.</span></span>

## <a name="using-debugging-tools-in-visual-studio"></a><span data-ttu-id="66f32-192">Visual Studio でのデバッグ ツールの使用</span><span class="sxs-lookup"><span data-stu-id="66f32-192">Using Debugging Tools in Visual Studio</span></span>

<span data-ttu-id="66f32-193">Visual Studio 2013 または無料を使用して、包括的なデバッグ機能の[Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)です。</span><span class="sxs-lookup"><span data-stu-id="66f32-193">For a more comprehensive debugging experience, use Visual Studio 2013 or the free [Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express).</span></span> <span data-ttu-id="66f32-194">Visual Studio では、調査対象の行で、コードでブレークポイントを設定できます。</span><span class="sxs-lookup"><span data-stu-id="66f32-194">With Visual Studio, you can set a breakpoint in your code at the line that you want to inspect.</span></span>

![ブレークポイントの設定](introduction-to-debugging/_static/image1.png)

<span data-ttu-id="66f32-196">Web サイトをテストするときにコードの実行がブレークポイントで停止します。</span><span class="sxs-lookup"><span data-stu-id="66f32-196">When you test the web site, the executing code halts at the breakpoint.</span></span>

![ブレークポイントに到達します。](introduction-to-debugging/_static/image2.png)

<span data-ttu-id="66f32-198">変数、およびステップ実行、コードで行の現在の値を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="66f32-198">You can examine the current values of the variables, and step through the code line-by-line.</span></span>

![値を参照してください。](introduction-to-debugging/_static/image3.png)

<span data-ttu-id="66f32-200">Visual Studio での統合デバッガーを使用して、ASP.NET Razor ページをデバッグする方法については、次を参照してください。[プログラミング ASP.NET Web Pages (Razor) を使用してクトリ](https://go.microsoft.com/fwlink/?LinkId=205854)です。</span><span class="sxs-lookup"><span data-stu-id="66f32-200">For information about using the integrated debugger in Visual Studio to debug ASP.NET Razor pages, see [Programming ASP.NET Web Pages (Razor) Using Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66f32-201">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="66f32-201">Additional Resources</span></span>

- [<span data-ttu-id="66f32-202">Visual Studio を使用してプログラミングする ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="66f32-202">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>](https://go.microsoft.com/fwlink/?LinkId=205854)
- <span data-ttu-id="66f32-203">[IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="66f32-203">[IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)</span></span>
- <span data-ttu-id="66f32-204">[環境変数を認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)</span><span class="sxs-lookup"><span data-stu-id="66f32-204">[Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)</span></span>
