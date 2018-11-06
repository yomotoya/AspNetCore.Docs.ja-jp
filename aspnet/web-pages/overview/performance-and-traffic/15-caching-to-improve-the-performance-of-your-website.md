---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: パフォーマンス向上のためのページ (Razor) サイトの ASP.NET web データのキャッシュ |Microsoft Docs
author: Rick-Anderson
description: 高速化する web サイト - つまり、保存することによりキャッシュのデータを取得または処理にかなりの時間を通常の結果をしています.
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: ede341e02869a9c81cbe2fa7ef97345dc87519a1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020366"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a><span data-ttu-id="00d06-103">パフォーマンス向上のための ASP.NET Web Pages (Razor) サイトでデータのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="00d06-103">Caching Data in an ASP.NET Web Pages (Razor) Site for Better Performance</span></span>
====================
<span data-ttu-id="00d06-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="00d06-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="00d06-105">この記事では、ASP.NET Web Pages (Razor) の web サイトに高速なパフォーマンスの情報をキャッシュするためのヘルパーを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="00d06-105">This article explains how to use a helper to cache information for faster performance in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="00d06-106">格納することにより、web サイトを短縮できます&#8212;キャッシュは、&#8212;通常を取得または処理にかなりの時間がかかるし、を頻繁に変更されないデータの結果。</span><span class="sxs-lookup"><span data-stu-id="00d06-106">You can speed up your website by having it store &#8212; that is, cache &#8212; the results of data that ordinarily would take considerable time to retrieve or process and that does not change often.</span></span>
> 
> <span data-ttu-id="00d06-107">**学習内容。**</span><span class="sxs-lookup"><span data-stu-id="00d06-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="00d06-108">Web サイトの応答性を向上させるためにキャッシュを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="00d06-108">How to use caching to improve the responsiveness of your website.</span></span>
> 
> <span data-ttu-id="00d06-109">この記事で導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="00d06-109">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="00d06-110">`WebCache`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="00d06-110">The `WebCache` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="00d06-111">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="00d06-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="00d06-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="00d06-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="00d06-113">このチュートリアルは、ASP.NET Web Pages 2 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="00d06-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="00d06-114">ユーザーは、サイトからページを要求するたびに要求に対処するために作業を web サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="00d06-114">Every time someone requests a page from your site, the web server has to do some work in order to fulfill the request.</span></span> <span data-ttu-id="00d06-115">ページの一部で、サーバーは、データベースからデータの取得などの (比較的) に長い時間がかかるタスクを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00d06-115">For some of your pages, the server might have to perform tasks that take a (comparatively) long time, such as retrieving data from a database.</span></span> <span data-ttu-id="00d06-116">場合でも、これらのタスクを絶対の用語で時間の長い使用しないサイトには、大量のトラフィックが発生した場合、一連の複雑なまたは低速のタスクを実行する web サーバーとなる個々 の要求が、多くの作業追加できます。</span><span class="sxs-lookup"><span data-stu-id="00d06-116">Even if these tasks don't take long in absolute terms, if your site experiences a lot of traffic, a whole series of individual requests that cause the web server to perform the complicated or slow task can add up to a lot of work.</span></span> <span data-ttu-id="00d06-117">これにより、サイトのパフォーマンスが最終的には影響します。</span><span class="sxs-lookup"><span data-stu-id="00d06-117">This can ultimately affect the performance of the site.</span></span>

<span data-ttu-id="00d06-118">このような状況で、web サイトのパフォーマンスを向上させる方法の 1 つでは、データをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="00d06-118">One way to improve the performance of your website in circumstances like this is to cache data.</span></span> <span data-ttu-id="00d06-119">情報は、各ユーザーに変更する必要はありませんし時間は、サイトが同じ情報については、繰り返しの要求を取得します再フェッチまたは再計算するには、代わりに、機密性の高い 1 回のデータをフェッチして結果を格納します。</span><span class="sxs-lookup"><span data-stu-id="00d06-119">If your site gets repeated requests for the same information, and the information does not need to be modified for each person, and it's not time sensitive, instead of re-fetching or recalculating it, you can fetch the data once and then store the results.</span></span> <span data-ttu-id="00d06-120">次回の要求を受信については、するだけを元にキャッシュから。</span><span class="sxs-lookup"><span data-stu-id="00d06-120">The next time a request comes in for that information, you just get it out of the cache.</span></span>

<span data-ttu-id="00d06-121">一般に、頻繁に変更されない情報をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="00d06-121">In general, you cache information that doesn't change frequently.</span></span> <span data-ttu-id="00d06-122">キャッシュに情報を格納するときに、web サーバーのメモリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="00d06-122">When you put information in the cache, it's stored in memory on the web server.</span></span> <span data-ttu-id="00d06-123">どのくらいの期間キャッシュするように、秒から日に指定できます。</span><span class="sxs-lookup"><span data-stu-id="00d06-123">You can specify how long it should be cached, from seconds to days.</span></span> <span data-ttu-id="00d06-124">キャッシュの期間が経過すると、情報がキャッシュから自動的に削除します。</span><span class="sxs-lookup"><span data-stu-id="00d06-124">When the caching period expires, the information is automatically removed from the cache.</span></span>

> [!NOTE]
> <span data-ttu-id="00d06-125">キャッシュ内のエントリ削除の可能性あり上の理由から他にも、期限が切れるしました。</span><span class="sxs-lookup"><span data-stu-id="00d06-125">Entries in the cache might be removed for reasons other than that they've expired.</span></span> <span data-ttu-id="00d06-126">など、web サーバーが一時的が残り少なくなったら、メモリとメモリを再利用できる 1 つの方法は、キャッシュからエントリをスローすることによって。</span><span class="sxs-lookup"><span data-stu-id="00d06-126">For example, the web server might temporarily run low on memory, and one way it can reclaim memory is by throwing entries out of the cache.</span></span> <span data-ttu-id="00d06-127">わかる、情報をキャッシュに配置した場合でもを確認して必要なときではまだありますがあります。</span><span class="sxs-lookup"><span data-stu-id="00d06-127">As you'll see, even if you've put information into the cache, you have to check to be sure it's still there when you need it.</span></span>


<span data-ttu-id="00d06-128">Web サイトが、現在の気温と天気予報を表示するページを想像してください。</span><span class="sxs-lookup"><span data-stu-id="00d06-128">Imagine your website has a page that displays the current temperature and weather forecast.</span></span> <span data-ttu-id="00d06-129">この種の情報を取得するには、外部のサービスに要求を送信する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="00d06-129">To get this type of information, you might send a request to an external service.</span></span> <span data-ttu-id="00d06-130">この情報は多く (たとえば 2 時間期間) 内で変更されないため、および外部の呼び出しは、時間と帯域幅が必要なため、キャッシュの候補を勧めします。</span><span class="sxs-lookup"><span data-stu-id="00d06-130">Since this information doesn't change much (within a two-hour time period, for example) and since external calls require time and bandwidth, it's a good candidate for caching.</span></span>

## <a name="adding-caching-to-a-page"></a><span data-ttu-id="00d06-131">ページのキャッシュの追加</span><span class="sxs-lookup"><span data-stu-id="00d06-131">Adding Caching to a Page</span></span>

<span data-ttu-id="00d06-132">ASP.NET には、`WebCache`をキャッシュにデータを追加し、サイトにキャッシュを追加するが容易にするヘルパー。</span><span class="sxs-lookup"><span data-stu-id="00d06-132">ASP.NET includes a `WebCache` helper that makes it easy to add caching to your site and add data to the cache.</span></span> <span data-ttu-id="00d06-133">この手順では、現在の時刻をキャッシュするページを作成します。</span><span class="sxs-lookup"><span data-stu-id="00d06-133">In this procedure, you'll create a page that caches the current time.</span></span> <span data-ttu-id="00d06-134">現在の時刻が多くの場合は変更されると、さらに計算する複雑なはないため、実際の例はありません。</span><span class="sxs-lookup"><span data-stu-id="00d06-134">This isn't a real-world example, since the current time is something that does change often, and that moreover isn't complex to calculate.</span></span> <span data-ttu-id="00d06-135">ただし、これはキャッシュの動作を説明することをお勧めです。</span><span class="sxs-lookup"><span data-stu-id="00d06-135">However, it's a good way to illustrate caching in action.</span></span>

1. <span data-ttu-id="00d06-136">という名前の新しいページを追加*WebCache.cshtml* web サイトにします。</span><span class="sxs-lookup"><span data-stu-id="00d06-136">Add a new page named *WebCache.cshtml* to the website.</span></span>
2. <span data-ttu-id="00d06-137">次のコードとマークアップをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="00d06-137">Add the following code and markup to the page:</span></span>

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    <span data-ttu-id="00d06-138">データをキャッシュするときに配置する名前を使用してキャッシュにこれは、web サイトで一意です。</span><span class="sxs-lookup"><span data-stu-id="00d06-138">When you cache data, you put it into the cache using a name this is unique across the website.</span></span> <span data-ttu-id="00d06-139">この場合、という名前のキャッシュ エントリを使用します`CachedTime`します。</span><span class="sxs-lookup"><span data-stu-id="00d06-139">In this case, you'll use a cache entry named `CachedTime`.</span></span> <span data-ttu-id="00d06-140">これは、`cacheItemKey`コード例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="00d06-140">This is the `cacheItemKey` shown in the code example.</span></span>

    <span data-ttu-id="00d06-141">コードを読み取り、`CachedTime`エントリをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="00d06-141">The code first reads the `CachedTime` cache entry.</span></span> <span data-ttu-id="00d06-142">(つまり、キャッシュ エントリがない場合 null) 値が返された場合、コードは単データをキャッシュする時の変数の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="00d06-142">If a value is returned (that is, if the cache entry isn't null), the code just sets the value of the time variable to the cache data.</span></span>

    <span data-ttu-id="00d06-143">ただし、キャッシュ エントリが存在しない場合 (つまり、null の)、コード、時刻の値を設定、それをキャッシュに追加および有効期限の値を 1 分に設定します。</span><span class="sxs-lookup"><span data-stu-id="00d06-143">However, if the cache entry doesn't exist (that is, it's null), the code sets the time value, adds it to the cache, and sets an expiration value to one minute.</span></span> <span data-ttu-id="00d06-144">1 分後にキャッシュ エントリは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="00d06-144">After one minute, the cache entry is discarded.</span></span> <span data-ttu-id="00d06-145">(キャッシュ内の項目の既定の有効期限の値は 20 分です)。コマンドは、`WebCache.Set(cacheItemKey, time, 1, false)`キャッシュに現在の時刻の値を追加し、1 分に、有効期限を設定する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="00d06-145">(The default expiration value for an item in the cache is 20 minutes.) The command `WebCache.Set(cacheItemKey, time, 1, false)` shows how to add the current time value to the cache and set its expiration to 1 minute.</span></span> <span data-ttu-id="00d06-146">設定、 *slidingExpiration*パラメーターを`false`有効期限が要求されるたびに更新されないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="00d06-146">Setting the *slidingExpiration* parameter to `false` means the expiration time is not renewed each time it is requested.</span></span> <span data-ttu-id="00d06-147">最初にキャッシュに追加された後、正確に 1 分に期限が切れます。</span><span class="sxs-lookup"><span data-stu-id="00d06-147">It will expire exactly 1 minute after it was originally added to the cache.</span></span> <span data-ttu-id="00d06-148">この値を設定する場合`true`1 分の有効期限がキャッシュから値が要求されるたびにリセットされます。</span><span class="sxs-lookup"><span data-stu-id="00d06-148">If you set this value to `true` the 1 minute expiration time is reset each time the value is requested from the cache.</span></span>

    <span data-ttu-id="00d06-149">このコードは、データをキャッシュするときに常に使用する必要があります、パターンを示しています。</span><span class="sxs-lookup"><span data-stu-id="00d06-149">This code illustrates the pattern you should always use when you cache data.</span></span> <span data-ttu-id="00d06-150">キャッシュから何かを取得する前に常にまず、かどうか、`WebCache.Get`メソッドが null を返しました。</span><span class="sxs-lookup"><span data-stu-id="00d06-150">Before you get something out of the cache, always check first whether the `WebCache.Get` method has returned null.</span></span> <span data-ttu-id="00d06-151">キャッシュ エントリが期限切れになった可能性がありますか、削除されているその他の何らかの理由がキャッシュ内に、指定したエントリが保証されることはありませんので注意してください。</span><span class="sxs-lookup"><span data-stu-id="00d06-151">Remember that the cache entry might have expired or might have been removed for some other reason, so any given entry is never guaranteed to be in the cache.</span></span>
3. <span data-ttu-id="00d06-152">実行*WebCache.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="00d06-152">Run *WebCache.cshtml* in a browser.</span></span> <span data-ttu-id="00d06-153">(内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。最初に、ページを要求するには時間データがキャッシュにないし、コードは、時刻の値をキャッシュに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00d06-153">(Make sure the page is selected in the **Files** workspace before you run it.) The first time you request the page, the time data isn't in the cache, and the code has to add the time value to the cache.</span></span>

    ![キャッシュ-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. <span data-ttu-id="00d06-155">更新*WebCache.cshtml*ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="00d06-155">Refresh *WebCache.cshtml* in the browser.</span></span> <span data-ttu-id="00d06-156">今回は、時刻のデータがキャッシュにします。</span><span class="sxs-lookup"><span data-stu-id="00d06-156">This time, the time data is in the cache.</span></span> <span data-ttu-id="00d06-157">前回のページを表示した後、時間が変更されていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="00d06-157">Notice that the time hasn't changed since the last time you viewed the page.</span></span>

    ![キャッシュ 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. <span data-ttu-id="00d06-159">キャッシュを空にするのには 1 分間待機し、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="00d06-159">Wait one minute for the cache to be emptied, and then refresh the page.</span></span> <span data-ttu-id="00d06-160">ページ、もう一度ことを示します時刻のデータがキャッシュに見つかりませんでした更新時刻が、キャッシュに追加されます。</span><span class="sxs-lookup"><span data-stu-id="00d06-160">The page again indicates that the time data wasn't found in the cache, and the updated time is added to the cache.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="00d06-161">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="00d06-161">Additional Resources</span></span>


- [<span data-ttu-id="00d06-162">グラフでデータを表示する</span><span class="sxs-lookup"><span data-stu-id="00d06-162">Displaying Data in a Chart</span></span>](https://go.microsoft.com/fwlink/?LinkId=202895)
- <span data-ttu-id="00d06-163">[WebCache API リファレンス](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="00d06-163">[WebCache API reference](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)</span></span>
