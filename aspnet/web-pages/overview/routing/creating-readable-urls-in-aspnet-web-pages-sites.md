---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: ページ (Razor) サイトの ASP.NET Web で読み取り可能な Url を作成する |Microsoft ドキュメント
author: tfitzmac
description: この記事では、ASP.NET Web Pages (Razor) の web サイト、およびこれを使用する方法を読みやすく、優れた SEO の Url を使用してルーティングについて説明します。 新機能を学習しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529751"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="80902-104">ASP.NET Web Pages (Razor) のサイトで読み取り可能な Url を作成します。</span><span class="sxs-lookup"><span data-stu-id="80902-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="80902-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="80902-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="80902-106">この記事では、ASP.NET Web Pages (Razor) の web サイト、およびこれを使用する方法を読みやすく、優れた SEO の Url を使用してルーティングについて説明します。</span><span class="sxs-lookup"><span data-stu-id="80902-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="80902-107">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="80902-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="80902-108">どの ASP.NET を使用してルーティング以上読み取り可能で、検索可能な Url を使用できます。</span><span class="sxs-lookup"><span data-stu-id="80902-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="80902-109">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="80902-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="80902-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="80902-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="80902-111">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="80902-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="80902-112">ルーティングについて</span><span class="sxs-lookup"><span data-stu-id="80902-112">About Routing</span></span>

<span data-ttu-id="80902-113">サイト内のページの Url には、どの程度、サイトの動作に影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="80902-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="80902-114">URL の&quot;フレンドリ&quot;となるサイトを使用するユーザーを簡単にします。</span><span class="sxs-lookup"><span data-stu-id="80902-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="80902-115">サイトの検索エンジン最適化 (SEO) にも役立ちます。</span><span class="sxs-lookup"><span data-stu-id="80902-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="80902-116">ASP.NET web サイトには、フレンドリな Url を自動的に使用する機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="80902-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="80902-117">ASP.NET では、サーバー上のファイルを指しているだけではなくユーザーの操作について説明するわかりやすい Url を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="80902-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="80902-118">架空のブログを考えてみましょう Url:</span><span class="sxs-lookup"><span data-stu-id="80902-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="80902-119">次のようにこれらの Url を比較します。</span><span class="sxs-lookup"><span data-stu-id="80902-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="80902-120">使用して、ブログが表示されたことを確認する必要があります、ユーザー、最初のペアで、 *blog.cshtml*ページ、および適切なカテゴリまたは日付範囲を取得するクエリ文字列を構築し、必要があります。</span><span class="sxs-lookup"><span data-stu-id="80902-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="80902-121">2 番目の例のセットは理解し、作成をはるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="80902-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="80902-122">最初の例の Url は、特定のファイルを直接もポイント (*blog.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="80902-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="80902-123">何らかの理由により、ブログが別のフォルダーにサーバー上の移動、別のページを使用するブログが書き換えられた場合や、リンクが正しくないがあります。</span><span class="sxs-lookup"><span data-stu-id="80902-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="80902-124">Url の 2 番目のセットは、特定のページを指していませんブログ実装または場所が変更された場合でもそのため、Url もよい無効です。</span><span class="sxs-lookup"><span data-stu-id="80902-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="80902-125">ASP.NET Web Pages で ASP.NET を使用するためのような前の例でわかりやすい Url を作成できます*ルーティング*です。</span><span class="sxs-lookup"><span data-stu-id="80902-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="80902-126">ページ (ページ) への要求を満たすことができますを URL から論理マッピングを作成するルーティングします。</span><span class="sxs-lookup"><span data-stu-id="80902-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="80902-127">マッピングは論理ので (物理、特定のファイルを)、ルーティング非常に柔軟に、サイトの Url を定義する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="80902-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="80902-128">ルーティングのしくみ</span><span class="sxs-lookup"><span data-stu-id="80902-128">How Routing Works</span></span>

<span data-ttu-id="80902-129">ASP.NET 要求を処理するときの宛先に転送する方法を決定する URL を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="80902-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="80902-130">ASP.NET では、左から右へと向かうディスク上のファイルへの URL の個別のセグメントを照合します。</span><span class="sxs-lookup"><span data-stu-id="80902-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="80902-131">一致がある場合、URL の残りのものとしてそのページへ渡されます。*パス情報*です。</span><span class="sxs-lookup"><span data-stu-id="80902-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="80902-132">他のユーザーがこの URL を使用して、要求を行うことを想像してください。</span><span class="sxs-lookup"><span data-stu-id="80902-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="80902-133">検索は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="80902-133">The search goes like this:</span></span>

1. <span data-ttu-id="80902-134">名前とパスを持つファイルがある */a/b/c.cshtml*しますか?</span><span class="sxs-lookup"><span data-stu-id="80902-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="80902-135">場合は、そのページを実行し、情報を渡したりありません。</span><span class="sxs-lookup"><span data-stu-id="80902-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="80902-136">それ以外の場合.</span><span class="sxs-lookup"><span data-stu-id="80902-136">Otherwise ...</span></span>
2. <span data-ttu-id="80902-137">名前とパスを持つファイルがある */a/b.cshtml*しますか?</span><span class="sxs-lookup"><span data-stu-id="80902-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="80902-138">そのため、そのページを実行し、値を渡す場合`c`にします。</span><span class="sxs-lookup"><span data-stu-id="80902-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="80902-139">それ以外の場合.</span><span class="sxs-lookup"><span data-stu-id="80902-139">Otherwise …</span></span>
3. <span data-ttu-id="80902-140">名前とパスを持つファイルがある */a.cshtml*しますか?</span><span class="sxs-lookup"><span data-stu-id="80902-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="80902-141">そのため、そのページを実行し、値を渡す場合`b/c`にします。</span><span class="sxs-lookup"><span data-stu-id="80902-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="80902-142">正確ないいえ、検索に一致する場合 *.cshtml*さらにこれらのファイルを探して、指定したフォルダーのファイルは、ASP.NET 継続します。</span><span class="sxs-lookup"><span data-stu-id="80902-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="80902-143">*/a/b/c/default.cshtml* (パス情報がない)。</span><span class="sxs-lookup"><span data-stu-id="80902-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="80902-144">*/a/b/c/index.cshtml* (パス情報がない)。</span><span class="sxs-lookup"><span data-stu-id="80902-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="80902-145">、明確にする特定のページの要求 (含む要求は、、 *.cshtml*ファイル名拡張子) が期待するのと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="80902-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="80902-146">要求と同様に`http://www.contoso.com/a/b.cshtml`ページの実行は*b.cshtml*うまくです。</span><span class="sxs-lookup"><span data-stu-id="80902-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="80902-147">ページ内には、ページのパス情報を取得することができます`UrlData`プロパティ ディクショナリです。</span><span class="sxs-lookup"><span data-stu-id="80902-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="80902-148">という名前のファイルがあると*ViewCustomers.cshtml*をサイトがこの要求を取得します。</span><span class="sxs-lookup"><span data-stu-id="80902-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="80902-149">上記の規則のように、要求は、ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="80902-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="80902-150">ページ内を取得し、パス情報を表示する次のようにコードを使用することができます (この場合、値&quot;1000&quot;)。</span><span class="sxs-lookup"><span data-stu-id="80902-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="80902-151">ルーティングと、完全なファイル名が含まれまるありません、ためにありますあいまいさが同じであるページがある場合が異なるファイル名拡張子の名前 (たとえば、 *MyPage.cshtml*と*MyPage.html*).</span><span class="sxs-lookup"><span data-stu-id="80902-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="80902-152">ルーティングの問題を回避するために、拡張機能でのみが異なる名前を持つサイトのページがないことを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="80902-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="80902-153">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="80902-153">Additional Resources</span></span>

<span data-ttu-id="80902-154">[WebMatrix の Url、UrlData およびルーティングの SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)です。</span><span class="sxs-lookup"><span data-stu-id="80902-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="80902-155">Mike Brind によるブログ エントリでは、ルーティング機能の仕組み ASP.NET Web Pages での追加の詳細情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="80902-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
