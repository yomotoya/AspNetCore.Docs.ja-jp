---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: ページ (Razor) サイトを ASP.NET Web で読み取り可能な Url を作成する |Microsoft Docs
author: tfitzmac
description: この記事では、ルーティングでは、ASP.NET Web Pages (Razor) の web サイト、およびこの使用がより読みやすく、優れた SEO の Url を使用する方法について説明します。 何を学習しています.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: b8405283dc5bf44a4cd8d1122d327346774d95e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832172"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="68efe-104">ASP.NET Web Pages (Razor) サイトでわかりやすい Url を作成します。</span><span class="sxs-lookup"><span data-stu-id="68efe-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="68efe-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="68efe-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="68efe-106">この記事では、ルーティングでは、ASP.NET Web Pages (Razor) の web サイト、およびこの使用がより読みやすく、優れた SEO の Url を使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="68efe-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="68efe-107">学習内容。</span><span class="sxs-lookup"><span data-stu-id="68efe-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="68efe-108">どの ASP.NET ルーティングを使用してより読みやすく、検索可能な Url を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="68efe-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="68efe-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="68efe-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="68efe-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="68efe-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="68efe-111">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="68efe-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="68efe-112">ルーティングについて</span><span class="sxs-lookup"><span data-stu-id="68efe-112">About Routing</span></span>

<span data-ttu-id="68efe-113">サイト内のページの Url には、どの程度、サイトの動作に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="68efe-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="68efe-114">URL の&quot;フレンドリ&quot;サイトを使用するユーザーを簡単にできるようにします。</span><span class="sxs-lookup"><span data-stu-id="68efe-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="68efe-115">サイトの検索エンジン最適化 (SEO) にも役立ちます。</span><span class="sxs-lookup"><span data-stu-id="68efe-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="68efe-116">ASP.NET web サイトには、フレンドリな Url を自動的に使用する機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="68efe-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="68efe-117">ASP.NET では、サーバー上のファイルを指すだけではなくユーザーの操作について説明するわかりやすい Url を作成できます。</span><span class="sxs-lookup"><span data-stu-id="68efe-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="68efe-118">架空のブログを考慮して Url。</span><span class="sxs-lookup"><span data-stu-id="68efe-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="68efe-119">次の Url を比較します。</span><span class="sxs-lookup"><span data-stu-id="68efe-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="68efe-120">最初のペアでユーザーが を使用して、ブログを表示することを確認する必要があります、 *blog.cshtml*ページ、および適切なカテゴリまたは日付範囲を取得するクエリ文字列を生成する必要があります、します。</span><span class="sxs-lookup"><span data-stu-id="68efe-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="68efe-121">2 つ目の例は、理解して、作成にはるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="68efe-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="68efe-122">最初の例の Url が特定のファイルを直接ポイントも (*blog.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="68efe-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="68efe-123">何らかの理由、ブログはサーバーで、移動を別のフォルダーが、ブログは、別のページを使用する書き換えられた場合や、リンクが正しくないでしょう。</span><span class="sxs-lookup"><span data-stu-id="68efe-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="68efe-124">Url の 2 番目のセットは、特定のページを指していませんブログ実装または場所が変更された場合でもそのため、Url が有効になります。</span><span class="sxs-lookup"><span data-stu-id="68efe-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="68efe-125">ASP.NET Web Pages で ASP.NET が使用するため、上記の例のようなわかりやすい Url を作成できます*ルーティング*します。</span><span class="sxs-lookup"><span data-stu-id="68efe-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="68efe-126">ルーティングは、要求を満たすことができますをページ (またはページ) への URL から論理マッピングを作成します。</span><span class="sxs-lookup"><span data-stu-id="68efe-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="68efe-127">マッピングは論理ため (物理、特定のファイルを)、ルーティング、サイトの Url を定義する方法の柔軟性を提供します。</span><span class="sxs-lookup"><span data-stu-id="68efe-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="68efe-128">ルーティングの動作</span><span class="sxs-lookup"><span data-stu-id="68efe-128">How Routing Works</span></span>

<span data-ttu-id="68efe-129">ASP.NET 要求を処理するときにルーティングする方法を特定する URL を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="68efe-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="68efe-130">ASP.NET は、左から右へと向かうディスク上のファイルへの URL の個別のセグメントの一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="68efe-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="68efe-131">一致がある場合、URL に残っているものとしてページに渡されます。*パス情報*します。</span><span class="sxs-lookup"><span data-stu-id="68efe-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="68efe-132">ユーザーが、この URL を使用して、要求することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="68efe-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="68efe-133">検索では、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="68efe-133">The search goes like this:</span></span>

1. <span data-ttu-id="68efe-134">名前とパスを持つファイルがある */a/b/c.cshtml*でしょうか。</span><span class="sxs-lookup"><span data-stu-id="68efe-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="68efe-135">そうである場合は、そのページを実行し、情報を渡したりありません。</span><span class="sxs-lookup"><span data-stu-id="68efe-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="68efe-136">それ以外の場合.</span><span class="sxs-lookup"><span data-stu-id="68efe-136">Otherwise ...</span></span>
2. <span data-ttu-id="68efe-137">名前とパスを持つファイルがある */a/b.cshtml*でしょうか。</span><span class="sxs-lookup"><span data-stu-id="68efe-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="68efe-138">そのため、そのページを実行し、値を渡す場合`c`にします。</span><span class="sxs-lookup"><span data-stu-id="68efe-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="68efe-139">それ以外の場合.</span><span class="sxs-lookup"><span data-stu-id="68efe-139">Otherwise …</span></span>
3. <span data-ttu-id="68efe-140">名前とパスを持つファイルがある */a.cshtml*でしょうか。</span><span class="sxs-lookup"><span data-stu-id="68efe-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="68efe-141">そのため、そのページを実行し、値を渡す場合`b/c`にします。</span><span class="sxs-lookup"><span data-stu-id="68efe-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="68efe-142">ない正確な検出、検索に一致する場合 *.cshtml*さらにこれらのファイルを探して、指定したフォルダーのファイルは、ASP.NET 継続します。</span><span class="sxs-lookup"><span data-stu-id="68efe-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="68efe-143">*/a/b/c/default.cshtml* (パス情報がありません)。</span><span class="sxs-lookup"><span data-stu-id="68efe-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="68efe-144">*/a/b/c/index.cshtml* (パス情報がありません)。</span><span class="sxs-lookup"><span data-stu-id="68efe-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="68efe-145">明確にする特定のページ要求 (を含む要求、 *.cshtml*ファイル名拡張子) が期待するのと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="68efe-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="68efe-146">ような要求`http://www.contoso.com/a/b.cshtml`ページの実行は*b.cshtml*問題なく。</span><span class="sxs-lookup"><span data-stu-id="68efe-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="68efe-147">ページ内では、ページのパス情報を取得できます`UrlData`ディクショナリであるプロパティ。</span><span class="sxs-lookup"><span data-stu-id="68efe-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="68efe-148">という名前のファイルがあると*ViewCustomers.cshtml*し、サイトがこの要求を取得します。</span><span class="sxs-lookup"><span data-stu-id="68efe-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="68efe-149">上記のルールのように、要求は、ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="68efe-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="68efe-150">ページ内で取得し、パス情報を表示する、次のようなコードを使用することができます (この例では、値で&quot;1000&quot;)。</span><span class="sxs-lookup"><span data-stu-id="68efe-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="68efe-151">ルーティングと、完全なファイル名が関与しない、ためにありますあいまいさが同じであるページがあるとが異なるファイル名拡張子の名前 (たとえば、 *MyPage.cshtml*と*MyPage.html*).</span><span class="sxs-lookup"><span data-stu-id="68efe-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="68efe-152">ルーティングの問題を回避するために、その拡張機能でのみが異なる名前を持つサイトのページがないことを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="68efe-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="68efe-153">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="68efe-153">Additional Resources</span></span>

<span data-ttu-id="68efe-154">[WebMatrix - Url、UrlData およびルーティングの SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)します。</span><span class="sxs-lookup"><span data-stu-id="68efe-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="68efe-155">Mike Brind によるブログ エントリでは、ルーティングの動作では、ASP.NET Web ページに追加の詳細情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="68efe-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
