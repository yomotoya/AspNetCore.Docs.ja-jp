---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: ページ (Razor) サイトを ASP.NET Web のデバッグの概要 |Microsoft Docs
author: tfitzmac
description: デバッグは、検出とコード ページ内エラーを修正のプロセスです。 この章ではいくつかのツールとデバッグに使用できる手法と率の分析をしています.
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: b8a492b065902fa10d3e4c5cccd50e63ea356709
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823804"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a><span data-ttu-id="8b793-104">ページ (Razor) サイトを ASP.NET Web のデバッグの概要</span><span class="sxs-lookup"><span data-stu-id="8b793-104">Introduction to Debugging ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="8b793-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8b793-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8b793-106">この記事では、ASP.NET Web Pages (Razor) の web サイトのページをデバッグするさまざまな方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8b793-106">This article explains various ways to debug pages in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="8b793-107">デバッグは、検出とコード ページ内エラーを修正のプロセスです。</span><span class="sxs-lookup"><span data-stu-id="8b793-107">Debugging is the process of finding and fixing errors in your code pages.</span></span>
> 
> <span data-ttu-id="8b793-108">**学習内容。**</span><span class="sxs-lookup"><span data-stu-id="8b793-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="8b793-109">役立つ情報を表示する方法は、分析し、ページをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="8b793-109">How to display information that helps analyze and debug pages.</span></span>
> - <span data-ttu-id="8b793-110">デバッグを使用する方法は、Visual Studio のツールです。</span><span class="sxs-lookup"><span data-stu-id="8b793-110">How to use debugging tools in Visual Studio.</span></span>
>   
> 
> <span data-ttu-id="8b793-111">この記事で導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-111">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="8b793-112">`ServerInfo`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="8b793-112">The `ServerInfo` helper.</span></span>
> - <span data-ttu-id="8b793-113">`ObjectInfo` ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="8b793-113">`ObjectInfo` helper.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="8b793-114">ソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="8b793-114">Software versions</span></span>
> 
> 
> - <span data-ttu-id="8b793-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="8b793-115">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="8b793-116">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8b793-116">Visual Studio 2013</span></span>
>   
> 
> <span data-ttu-id="8b793-117">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="8b793-117">This tutorial also works with ASP.NET Web Pages 2.</span></span> <span data-ttu-id="8b793-118">WebMatrix 3 を使用できますが、統合デバッガーがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="8b793-118">You can use WebMatrix 3 but the integrated debugger is not supported.</span></span>


<span data-ttu-id="8b793-119">エラーと、コード内の問題のトラブルシューティングの重要な側面は、最初にそれらを回避するためには。</span><span class="sxs-lookup"><span data-stu-id="8b793-119">An important aspect of troubleshooting errors and problems in your code is to avoid them in the first place.</span></span> <span data-ttu-id="8b793-120">エラーが発生する可能性のあるコードのセクションを配置することで行うことができます`try/catch`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="8b793-120">You can do that by putting sections of your code that are likely to cause errors into `try/catch` blocks.</span></span> <span data-ttu-id="8b793-121">詳細については、上でのエラー処理セクションをご覧ください。 [Web プログラミングを使用して ASP.NET Razor 構文の概要](https://go.microsoft.com/fwlink/?LinkId=202890)します。</span><span class="sxs-lookup"><span data-stu-id="8b793-121">For more information, see the section on handling errors in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890).</span></span>

<span data-ttu-id="8b793-122">`ServerInfo`ヘルパーは、ページをホストする web サーバー環境に関する情報の概要を示す診断ツール。</span><span class="sxs-lookup"><span data-stu-id="8b793-122">The `ServerInfo` helper is a diagnostic tool that gives you an overview of information about the web server environment that hosts your page.</span></span> <span data-ttu-id="8b793-123">ブラウザーのページを要求時に送信される HTTP 要求の情報も示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-123">It also shows you HTTP request information that's sent when a browser requests the page.</span></span> <span data-ttu-id="8b793-124">`ServerInfo`ヘルパーは、現在のユーザー id の要求を行ったブラウザーの種類を表示します。 これにします。</span><span class="sxs-lookup"><span data-stu-id="8b793-124">The `ServerInfo` helper displays the current user identity, the type of browser that made the request, and so on.</span></span> <span data-ttu-id="8b793-125">この種の情報は、一般的な問題をトラブルシューティングするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8b793-125">This kind of information can help you troubleshoot common issues.</span></span>

1. <span data-ttu-id="8b793-126">という名前の新しい web ページを作成する*ServerInfo.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="8b793-126">Create a new web page named *ServerInfo.cshtml*.</span></span>
2. <span data-ttu-id="8b793-127">ページの末尾には、直前の終了`</body>`タグを追加`@ServerInfo.GetHtml()`:</span><span class="sxs-lookup"><span data-stu-id="8b793-127">At the end of the page, just before the closing `</body>` tag, add `@ServerInfo.GetHtml()`:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    <span data-ttu-id="8b793-128">追加することができます、`ServerInfo`ページ内の任意の場所のコード。</span><span class="sxs-lookup"><span data-stu-id="8b793-128">You can add the `ServerInfo` code anywhere in the page.</span></span> <span data-ttu-id="8b793-129">末尾に追加するには、出力から分離して他のページのコンテンツを読みやすくなります。</span><span class="sxs-lookup"><span data-stu-id="8b793-129">But adding it at the end will keep its output separate from your other page content, which makes it easier to read.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="8b793-130">**重要な**実稼働サーバーへの web ページを移動する前に、web ページから診断コードを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8b793-130">**Important** You should remove any diagnostic code from your web pages before you move web pages to a production server.</span></span> <span data-ttu-id="8b793-131">これに適用されます、`ServerInfo`ヘルパーとこの記事でコード ページへの追加に関連するその他の診断技法。</span><span class="sxs-lookup"><span data-stu-id="8b793-131">This applies to the `ServerInfo` helper as well as the other diagnostic techniques in this article that involve adding code to a page.</span></span> <span data-ttu-id="8b793-132">この種の情報が悪意のある方に役立つ可能性があるため、web サイトの訪問者、server、および同様の詳細については、サーバー名、ユーザー名、パスを参照してください。 には必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8b793-132">You don't want your website visitors to see information about your server name, user names, paths on your server, and similar details, because this type of information might be useful to people with malicious intent.</span></span>
3. <span data-ttu-id="8b793-133">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="8b793-133">Save the page and run it in a browser.</span></span>

    ![デバッグ-1](introduction-to-debugging/_static/image1.jpg)

    <span data-ttu-id="8b793-135">`ServerInfo`ヘルパー ページの情報の 4 つのテーブルを表示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-135">The `ServerInfo` helper displays four tables of information in the page:</span></span>

   - <span data-ttu-id="8b793-136">サーバーの構成。</span><span class="sxs-lookup"><span data-stu-id="8b793-136">Server Configuration.</span></span> <span data-ttu-id="8b793-137">このセクションでは、コンピューター名、ASP.NET を実行している、ドメイン名、およびサーバーの時刻のバージョンを含む、ホスト web サーバーについてを説明します。</span><span class="sxs-lookup"><span data-stu-id="8b793-137">This section provides information about the hosting web server, including computer name, the version of ASP.NET you're running, the domain name, and server time.</span></span>
   - <span data-ttu-id="8b793-138">ASP.NET サーバー変数。</span><span class="sxs-lookup"><span data-stu-id="8b793-138">ASP.NET Server Variables.</span></span> <span data-ttu-id="8b793-139">このセクションでは、多くの HTTP プロトコルの詳細 (呼び出された HTTP 変数) について詳しく説明し、値を各 web ページ要求の一部であります。</span><span class="sxs-lookup"><span data-stu-id="8b793-139">This section provides details about the many HTTP protocol details (called HTTP variables) and values that are part of each web page request.</span></span>
   - <span data-ttu-id="8b793-140">HTTP ランタイム情報。</span><span class="sxs-lookup"><span data-stu-id="8b793-140">HTTP Runtime Information.</span></span> <span data-ttu-id="8b793-141">このセクションでは、バージョンの Microsoft .NET Framework で web ページが実行されている、パス、キャッシュについての詳細については、詳細を提供します。</span><span class="sxs-lookup"><span data-stu-id="8b793-141">This section provides details about that the version of the Microsoft .NET Framework that your web page is running under, the path, details about the cache, and so on.</span></span> <span data-ttu-id="8b793-142">(で説明したよう[Web プログラミングを使用して ASP.NET Razor 構文の概要](https://go.microsoft.com/fwlink/?LinkId=202890)構文は、広範なソフトウェア上に構築自体がマイクロソフトの ASP.NET web server テクノロジに基づいて構築され、Razor を使用して ASP.NET Web ページ開発ライブラリ、.NET Framework と呼ばれます。)</span><span class="sxs-lookup"><span data-stu-id="8b793-142">(As you learned in [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web Pages using the Razor syntax are built on Microsoft's ASP.NET web server technology, which is itself built on an extensive software development library called the .NET Framework.)</span></span>
   - <span data-ttu-id="8b793-143">環境変数。</span><span class="sxs-lookup"><span data-stu-id="8b793-143">Environment Variables.</span></span> <span data-ttu-id="8b793-144">このセクションは、web サーバー上のすべてのローカルの環境変数とその値の一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-144">This section provides a list of all the local environment variables and their values on the web server.</span></span>

     <span data-ttu-id="8b793-145">すべてのサーバーと要求の情報の完全な説明については、この記事の範囲外、ことを確認できますが、`ServerInfo`ヘルパーが、多くの診断情報を返します。</span><span class="sxs-lookup"><span data-stu-id="8b793-145">A full description of all the server and request information is beyond the scope of this article, but you can see that the `ServerInfo` helper returns a lot of diagnostic information.</span></span> <span data-ttu-id="8b793-146">値の詳細についてを`ServerInfo`戻り値を参照してください[環境変数の認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)、Microsoft TechNet web サイトと[IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN web サイト。</span><span class="sxs-lookup"><span data-stu-id="8b793-146">For more information about the values that `ServerInfo` returns, see [Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) on the Microsoft TechNet website and [IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) on the MSDN website.</span></span>

## <a name="embedding-output-expressions-to-display-page-values"></a><span data-ttu-id="8b793-147">ページの値を表示する埋め込み式</span><span class="sxs-lookup"><span data-stu-id="8b793-147">Embedding Output Expressions to Display Page Values</span></span>

<span data-ttu-id="8b793-148">コードで何が起こっているかを確認するもう 1 つの方法では、ページに出力式を埋め込むです。</span><span class="sxs-lookup"><span data-stu-id="8b793-148">Another way to see what's happening in your code is to embed output expressions in the page.</span></span> <span data-ttu-id="8b793-149">ようなものを追加することで、変数の値を直接出力ご存知のように、`@myVariable`または`@(subTotal * 12)`ページにします。</span><span class="sxs-lookup"><span data-stu-id="8b793-149">As you know, you can directly output the value of a variable by adding something like `@myVariable` or `@(subTotal * 12)` to the page.</span></span> <span data-ttu-id="8b793-150">デバッグについては、コードで重要なポイントをこれらの式の出力を配置できます。</span><span class="sxs-lookup"><span data-stu-id="8b793-150">For debugging, you can place these output expressions at strategic points in your code.</span></span> <span data-ttu-id="8b793-151">これにより、ページの実行時に、主要な変数または計算の結果の値を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8b793-151">This enables you to see the value of key variables or the result of calculations when your page runs.</span></span> <span data-ttu-id="8b793-152">完了したら、デバッグ、式を削除したり、コメント アウトします。この手順は、ページをデバッグする際に埋め込み式を使用する一般的な方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8b793-152">When you're done debugging, you can remove the expressions or comment them out. This procedure illustrates a typical way to use embedded expressions to help debug a page.</span></span>

1. <span data-ttu-id="8b793-153">という新しい WebMatrix ページ作成*OutputExpression.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="8b793-153">Create a new WebMatrix page that's named *OutputExpression.cshtml*.</span></span>
2. <span data-ttu-id="8b793-154">次のようにコンテンツのページに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8b793-154">Replace the page content with the following:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    <span data-ttu-id="8b793-155">この例では、`switch`の値を確認するステートメント、`weekday`変数とはどの曜日に応じて異なる出力メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-155">The example uses a `switch` statement to check the value of the `weekday` variable and then display a different output message depending on which day of the week it is.</span></span> <span data-ttu-id="8b793-156">この例で、`if`最初のコード ブロック内のブロックが 1 日を現在の曜日の値に追加することで週の曜日を任意に変更します。</span><span class="sxs-lookup"><span data-stu-id="8b793-156">In the example, the `if` block within the first code block arbitrarily changes the day of the week by adding one day to the current weekday value.</span></span> <span data-ttu-id="8b793-157">これは、わかりやすくするために導入されたエラーです。</span><span class="sxs-lookup"><span data-stu-id="8b793-157">This is an error introduced for illustration purposes.</span></span>
3. <span data-ttu-id="8b793-158">ページを保存し、ブラウザーで実行します。</span><span class="sxs-lookup"><span data-stu-id="8b793-158">Save the page and run it in a browser.</span></span>

    <span data-ttu-id="8b793-159">ページでは、間違った曜日のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8b793-159">The page displays the message for the wrong day of the week.</span></span> <span data-ttu-id="8b793-160">実際には、その週のすべての日、1 日以降、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8b793-160">Whatever day of the week it actually is, you'll see the message for one day later.</span></span> <span data-ttu-id="8b793-161">(コードは、意図的に正しくない日付の値を設定する) ためにのオフ、メッセージが理由はわかってここでは、実際には多くの場合、処理は、コードで問題がどこかを知ることが難しい。</span><span class="sxs-lookup"><span data-stu-id="8b793-161">Although in this case you know why the message is off (because the code deliberately sets the incorrect day value), in reality it's often hard to know where things are going wrong in the code.</span></span> <span data-ttu-id="8b793-162">デバッグするには、何が起こっているかの主要なオブジェクトと変数の値になどを確認する必要があります`weekday`します。</span><span class="sxs-lookup"><span data-stu-id="8b793-162">To debug, you need to find out what's happening to the value of key objects and variables such as `weekday`.</span></span>
4. <span data-ttu-id="8b793-163">挿入して出力式を追加`@weekday`コード内のコメントで示される 2 つの場所で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="8b793-163">Add output expressions by inserting `@weekday` as shown in the two places indicated by comments in the code.</span></span> <span data-ttu-id="8b793-164">これらの出力式では、コードの実行に、変数の値をその時点で表示されます。</span><span class="sxs-lookup"><span data-stu-id="8b793-164">These output expressions will display the values of the variable at that point in the code execution.</span></span>

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. <span data-ttu-id="8b793-165">保存し、ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="8b793-165">Save and run the page in a browser.</span></span>

    <span data-ttu-id="8b793-166">ページが最初に、実際の曜日を表示した後 1 日とし、結果のメッセージからの追加を結果の更新された曜日、`switch`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="8b793-166">The page displays the real day of the week first, then the updated day of the week that results from adding one day, and then the resulting message from the `switch` statement.</span></span> <span data-ttu-id="8b793-167">2 つの変数の式からの出力 (`@weekday`) 任意の HTML を追加していないため、日の間のスペースがない`<p>`出力にタグをテストするためだけは、式は。</span><span class="sxs-lookup"><span data-stu-id="8b793-167">The output from the two variable expressions (`@weekday`) has no spaces between the days because you didn't add any HTML `<p>` tags to the output; the expressions are just for testing.</span></span>

    ![デバッグ-2](introduction-to-debugging/_static/image2.jpg)

    <span data-ttu-id="8b793-169">エラーを含むことがわかります。</span><span class="sxs-lookup"><span data-stu-id="8b793-169">Now you can see where the error is.</span></span> <span data-ttu-id="8b793-170">表示すると、`weekday`コードに変数が、正しい日はすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-170">When you first display the `weekday` variable in the code, it shows the correct day.</span></span> <span data-ttu-id="8b793-171">表示すると、2 番目の時間後に、`if`コードのブロック、1 日が 1 つがオフです。</span><span class="sxs-lookup"><span data-stu-id="8b793-171">When you display it the second time, after the `if` block in the code, the day is off by one.</span></span> <span data-ttu-id="8b793-172">したがって、曜日の変数の最初と 2 つ目の外観の間で何かが発生したことがわかります。</span><span class="sxs-lookup"><span data-stu-id="8b793-172">So you know that something has happened between the first and second appearance of the weekday variable.</span></span> <span data-ttu-id="8b793-173">これが、実際のバグの場合、この種のアプローチに役立つ、問題の原因となっているコードの場所を絞り込みます。</span><span class="sxs-lookup"><span data-stu-id="8b793-173">If this were a real bug, this kind of approach would help you narrow down the location of the code that's causing the problem.</span></span>
6. <span data-ttu-id="8b793-174">ページで、追加した 2 つの出力の式を削除して、週の曜日を変更するコードを削除して、コードを修正します。</span><span class="sxs-lookup"><span data-stu-id="8b793-174">Fix the code in the page by removing the two output expressions you added, and removing the code that changes the day of the week.</span></span> <span data-ttu-id="8b793-175">コードの残り、完了ブロックは、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="8b793-175">The remaining, complete block of code looks like the following example:</span></span>

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. <span data-ttu-id="8b793-176">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="8b793-176">Run the page in a browser.</span></span> <span data-ttu-id="8b793-177">この時間、実際の曜日が表示されます正しいメッセージが表示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-177">This time you see the correct message displayed for the actual day of the week.</span></span>

## <a name="using-the-objectinfo-helper-to-display-object-values"></a><span data-ttu-id="8b793-178">ObjectInfo ヘルパーを使用して、オブジェクトの値を表示するには</span><span class="sxs-lookup"><span data-stu-id="8b793-178">Using the ObjectInfo Helper to Display Object Values</span></span>

<span data-ttu-id="8b793-179">`ObjectInfo`ヘルパー型とそれに渡す各オブジェクトの値を表示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-179">The `ObjectInfo` helper displays the type and the value of each object you pass to it.</span></span> <span data-ttu-id="8b793-180">(出力式は、前の例で行った) など、コード内の変数およびオブジェクトの値を表示する使用すると、データ型のオブジェクトに関する情報を確認できます。</span><span class="sxs-lookup"><span data-stu-id="8b793-180">You can use it to view the value of variables and objects in your code (like you did with output expressions in the previous example), plus you can see data type information about the object.</span></span>

1. <span data-ttu-id="8b793-181">という名前のファイルを開く*OutputExpression.cshtml*先ほど作成しました。</span><span class="sxs-lookup"><span data-stu-id="8b793-181">Open the file named *OutputExpression.cshtml* that you created earlier.</span></span>
2. <span data-ttu-id="8b793-182">ページ内のすべてのコードを次のコード ブロックに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8b793-182">Replace all code in the page with the following block of code:</span></span>

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. <span data-ttu-id="8b793-183">保存し、ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="8b793-183">Save and run the page in a browser.</span></span>

    ![デバッグ-4](introduction-to-debugging/_static/image3.jpg)

    <span data-ttu-id="8b793-185">この例で、`ObjectInfo`ヘルパーは、2 つの項目を表示します。</span><span class="sxs-lookup"><span data-stu-id="8b793-185">In this example, the `ObjectInfo` helper displays two items:</span></span>

   - <span data-ttu-id="8b793-186">型。</span><span class="sxs-lookup"><span data-stu-id="8b793-186">The type.</span></span> <span data-ttu-id="8b793-187">最初の変数の型は`DayOfWeek`します。</span><span class="sxs-lookup"><span data-stu-id="8b793-187">For the first variable, the type is `DayOfWeek`.</span></span> <span data-ttu-id="8b793-188">2 番目の変数の型は`String`します。</span><span class="sxs-lookup"><span data-stu-id="8b793-188">For the second variable, the type is `String`.</span></span>
   - <span data-ttu-id="8b793-189">値。</span><span class="sxs-lookup"><span data-stu-id="8b793-189">The value.</span></span> <span data-ttu-id="8b793-190">この場合は、既に ページで、あいさつの変数の値を表示するため、値が表示されますもう一度に変数を渡すときに`ObjectInfo`します。</span><span class="sxs-lookup"><span data-stu-id="8b793-190">In this case, because you already display the value of the greeting variable in the page, the value is displayed again when you pass the variable to `ObjectInfo`.</span></span>

     <span data-ttu-id="8b793-191">複雑なオブジェクトの場合、`ObjectInfo`ヘルパーは、詳細情報を表示できる&#8212;基本的には、型とすべてのオブジェクトのプロパティの値を表示できます。</span><span class="sxs-lookup"><span data-stu-id="8b793-191">For more complex objects, the `ObjectInfo` helper can display more information &#8212; basically, it can display the types and values of all of an object's properties.</span></span>

## <a name="using-debugging-tools-in-visual-studio"></a><span data-ttu-id="8b793-192">Visual Studio でのデバッグ ツールの使用</span><span class="sxs-lookup"><span data-stu-id="8b793-192">Using Debugging Tools in Visual Studio</span></span>

<span data-ttu-id="8b793-193">包括的なデバッグ エクスペリエンスでは、Visual Studio 2013 または、無料使用[Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)します。</span><span class="sxs-lookup"><span data-stu-id="8b793-193">For a more comprehensive debugging experience, use Visual Studio 2013 or the free [Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express).</span></span> <span data-ttu-id="8b793-194">Visual Studio を使用して検査する行で、コードでブレークポイントを設定できます。</span><span class="sxs-lookup"><span data-stu-id="8b793-194">With Visual Studio, you can set a breakpoint in your code at the line that you want to inspect.</span></span>

![ブレークポイントの設定](introduction-to-debugging/_static/image1.png)

<span data-ttu-id="8b793-196">Web サイトをテストするときに実行中のコードは、ブレークポイントで停止します。</span><span class="sxs-lookup"><span data-stu-id="8b793-196">When you test the web site, the executing code halts at the breakpoint.</span></span>

![ブレークポイントに到達します。](introduction-to-debugging/_static/image2.png)

<span data-ttu-id="8b793-198">変数、およびコードで行をステップの現在の値を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="8b793-198">You can examine the current values of the variables, and step through the code line-by-line.</span></span>

![値を参照してください。](introduction-to-debugging/_static/image3.png)

<span data-ttu-id="8b793-200">Visual Studio で、統合デバッガーを使用して、ASP.NET Razor ページをデバッグする方法については、次を参照してください。[プログラミング ASP.NET Web Pages (Razor) を使用して Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)します。</span><span class="sxs-lookup"><span data-stu-id="8b793-200">For information about using the integrated debugger in Visual Studio to debug ASP.NET Razor pages, see [Programming ASP.NET Web Pages (Razor) Using Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b793-201">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="8b793-201">Additional Resources</span></span>

- [<span data-ttu-id="8b793-202">Visual Studio を使用して ASP.NET Web ページ (Razor) のプログラミング</span><span class="sxs-lookup"><span data-stu-id="8b793-202">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>](https://go.microsoft.com/fwlink/?LinkId=205854)
- <span data-ttu-id="8b793-203">[IIS サーバー変数](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="8b793-203">[IIS Server Variables](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)</span></span>
- <span data-ttu-id="8b793-204">[環境変数を認識](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)</span><span class="sxs-lookup"><span data-stu-id="8b793-204">[Recognized Environment Variables](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)</span></span>
