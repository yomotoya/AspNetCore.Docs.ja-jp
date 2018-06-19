---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: ASP.NET Web Pages (Razor) サイトの訪問者情報 (分析) の追跡 |Microsoft ドキュメント
author: tfitzmac
description: 予定 web サイトを取得した、後に、web サイト トラフィックを分析することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528761"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="da929-103">ASP.NET Web Pages (Razor) サイトの訪問者情報 (分析) の追跡</span><span class="sxs-lookup"><span data-stu-id="da929-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="da929-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="da929-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="da929-105">この記事では、ヘルパーを使用して ASP.NET Web Pages (Razor) の web サイトのページに web サイトの分析機能を追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="da929-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="da929-106">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="da929-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="da929-107">分析プロバイダーを web サイト トラフィックに関する情報を送信する方法です。</span><span class="sxs-lookup"><span data-stu-id="da929-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="da929-108">これらは、ASP.NET のアーティクルで導入された機能をプログラミングします。</span><span class="sxs-lookup"><span data-stu-id="da929-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="da929-109">`Analytics`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="da929-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="da929-110">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="da929-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="da929-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="da929-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="da929-112">ASP.NET Web Helpers Library (NuGet パッケージ)</span><span class="sxs-lookup"><span data-stu-id="da929-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="da929-113">分析は、ユーザーが、サイトを使用する方法を理解できるように、web サイト上のトラフィックを測定するテクノロジの一般的な用語です。</span><span class="sxs-lookup"><span data-stu-id="da929-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="da929-114">分析サービスの多くは、使用可能な Google、Yahoo、StatCounter、およびその他のユーザーからのサービスを含むです。</span><span class="sxs-lookup"><span data-stu-id="da929-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="da929-115">サインアップするアカウントのプロバイダーでは、分析、サイトを登録することはように分析は、追跡するためにします。プロバイダーでは、ID または追跡、アカウントのコードを含む JavaScript コードのスニペットを送信します。</span><span class="sxs-lookup"><span data-stu-id="da929-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="da929-116">JavaScript スニペットを追跡する、サイト上の web ページに追加します。(通常、analytics スニペットを追加、ページ フッターまたはレイアウト ページや、サイト内の各ページに表示される HTML マークアップ。)ユーザーは、これらの JavaScript スニペットのいずれかを含むページを要求するときにスニペットは、ページのさまざまな詳細を記録するユーザーの分析プロバイダーを現在のページに関する情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="da929-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="da929-117">サイトの統計情報を参照する場合は、分析プロバイダーの web サイトにログインします。</span><span class="sxs-lookup"><span data-stu-id="da929-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="da929-118">同様に、サイトに関する、あらゆる種類のレポートを表示できます。</span><span class="sxs-lookup"><span data-stu-id="da929-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="da929-119">個々 のページのページ ビューの数。</span><span class="sxs-lookup"><span data-stu-id="da929-119">The number of page views for individual pages.</span></span> <span data-ttu-id="da929-120">これは、通知するユーザーの数 (約) は、サイト、サイトのページが、最も人気の高いです。</span><span class="sxs-lookup"><span data-stu-id="da929-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="da929-121">どのくらいの期間ユーザーが特定のページに短縮されます。</span><span class="sxs-lookup"><span data-stu-id="da929-121">How long people spend on specific pages.</span></span> <span data-ttu-id="da929-122">ホーム ページがユーザーの関心を維持するかどうかのようなこれ指示できます。</span><span class="sxs-lookup"><span data-stu-id="da929-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="da929-123">どのようなサイトのユーザーは、サイトにアクセスする前に載っていました。</span><span class="sxs-lookup"><span data-stu-id="da929-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="da929-124">これにより、検索、およびなどからのリンクから、トラフィックが送られているかどうかを理解することができます。</span><span class="sxs-lookup"><span data-stu-id="da929-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="da929-125">ユーザーが、サイトを訪問するときと期間します。</span><span class="sxs-lookup"><span data-stu-id="da929-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="da929-126">どのような国の利用者はからです。</span><span class="sxs-lookup"><span data-stu-id="da929-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="da929-127">どのようなブラウザーとオペレーティング システム、利用者が使用されています。</span><span class="sxs-lookup"><span data-stu-id="da929-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="da929-129">ヘルパーを使用して、ページに分析を追加するには</span><span class="sxs-lookup"><span data-stu-id="da929-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="da929-130">ASP.NET Web ページには、いくつかの分析ヘルパーが含まれています (`Analytics.GetGoogleHtml`、 `Analytics.GetYahooHtml`、および`Analytics.GetStatCounterHtml`) を簡単に分析のために使用する JavaScript スニペットを管理します。</span><span class="sxs-lookup"><span data-stu-id="da929-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="da929-131">方法と場所を見つけ出しの代わりに、JavaScript コードを配置する行う必要があるすべてがヘルパーをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="da929-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="da929-132">提供する必要があります。 情報は、アカウント名、ID、または追跡コードです。</span><span class="sxs-lookup"><span data-stu-id="da929-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="da929-133">(StatCounter にも指定する必要がいくつかの追加の値です。)</span><span class="sxs-lookup"><span data-stu-id="da929-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="da929-134">この手順で使用するレイアウト ページを作成、`GetGoogleHtml`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="da929-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="da929-135">その他の分析プロバイダーのいずれかのアカウントを既にがある場合は、代わりに、そのアカウントを使用してし、必要に応じて若干調整します。</span><span class="sxs-lookup"><span data-stu-id="da929-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="da929-136">Analytics アカウントを作成するときに、追跡する場合、サイトの URL を登録します。</span><span class="sxs-lookup"><span data-stu-id="da929-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="da929-137">(トラフィックのみユーザーは)、実際のトラフィックが追跡するされない場合は、ローカル コンピューター上のすべてをテストしている、ため、サイトの統計情報を記録および表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="da929-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="da929-138">この手順は、ページに分析ヘルパーを追加する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="da929-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="da929-139">サイトを発行するときに、実際のサイトは、分析プロバイダーに情報が送信されます。</span><span class="sxs-lookup"><span data-stu-id="da929-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="da929-140">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。</span><span class="sxs-lookup"><span data-stu-id="da929-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="da929-141">Google アナリティクスでアカウントを作成し、アカウント名を記録します。</span><span class="sxs-lookup"><span data-stu-id="da929-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="da929-142">名前付きレイアウト ページを作成する*Analytics.cshtml*し、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="da929-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="da929-143">呼び出しを配置する必要があります、 `Analytics` web ページの本文のヘルパー (前に、`</body>`タグ)。</span><span class="sxs-lookup"><span data-stu-id="da929-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="da929-144">それ以外の場合、ブラウザーでは、スクリプトは実行されません。</span><span class="sxs-lookup"><span data-stu-id="da929-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="da929-145">別の分析プロバイダーを使用している場合は、代わりに使用、次のヘルパーのいずれか。</span><span class="sxs-lookup"><span data-stu-id="da929-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="da929-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="da929-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="da929-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="da929-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="da929-148">置き換える`myaccount`アカウント、ID、または手順 1. で作成した追跡コードの名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="da929-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="da929-149">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="da929-149">Run the page in the browser.</span></span> <span data-ttu-id="da929-150">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。</span><span class="sxs-lookup"><span data-stu-id="da929-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="da929-151">ブラウザーでページ ソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="da929-151">In the browser, view the page source.</span></span> <span data-ttu-id="da929-152">レンダリングされた分析コードを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="da929-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="da929-153">Google アナリティクス サイトにログオンし、サイトの統計情報を確認します。</span><span class="sxs-lookup"><span data-stu-id="da929-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="da929-154">実際のサイトでページを実行した場合、ページへのアクセスをログに記録されているエントリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="da929-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="da929-155">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="da929-155">Additional Resources</span></span>

- [<span data-ttu-id="da929-156">Google アナリティクス サイト</span><span class="sxs-lookup"><span data-stu-id="da929-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="da929-157">Yahoo!Web サイトの分析</span><span class="sxs-lookup"><span data-stu-id="da929-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="da929-158">StatCounter サイト</span><span class="sxs-lookup"><span data-stu-id="da929-158">StatCounter site</span></span>](http://statcounter.com/)
