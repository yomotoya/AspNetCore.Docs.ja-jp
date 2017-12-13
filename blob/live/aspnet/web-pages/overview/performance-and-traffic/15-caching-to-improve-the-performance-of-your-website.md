---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "パフォーマンス向上のためのページ (Razor) サイトの ASP.NET Web のデータ キャッシュ |Microsoft ドキュメント"
author: tfitzmac
description: "時間を短縮できます web サイトをストア - は、キャッシュのデータを取得または処理にかなりの時間を通常の結果をしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: c747fef33a6d1db19f09fd0303c47d689b956687
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a><span data-ttu-id="43f71-103">パフォーマンス向上のための ASP.NET Web Pages (Razor) サイトのデータのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="43f71-103">Caching Data in an ASP.NET Web Pages (Razor) Site for Better Performance</span></span>
====================
<span data-ttu-id="43f71-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="43f71-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="43f71-105">この記事では、パフォーマンスを向上させる ASP.NET Web Pages (Razor) の web サイトに情報をキャッシュするためのヘルパーを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="43f71-105">This article explains how to use a helper to cache information for faster performance in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="43f71-106">Web サイトをスピードアップするには、ストア &#8212; ことでキャッシュ &#8212; は、多くの場合、通常は夥しいを取得または処理にかなりの時間とデータの結果は変更されません。</span><span class="sxs-lookup"><span data-stu-id="43f71-106">You can speed up your website by having it store &#8212; that is, cache &#8212; the results of data that ordinarily would take considerable time to retrieve or process and that does not change often.</span></span>
> 
> <span data-ttu-id="43f71-107">**学習する内容。**</span><span class="sxs-lookup"><span data-stu-id="43f71-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="43f71-108">キャッシュを使用して、web サイトの応答性を向上させる方法です。</span><span class="sxs-lookup"><span data-stu-id="43f71-108">How to use caching to improve the responsiveness of your website.</span></span>
> 
> <span data-ttu-id="43f71-109">アーティクルで導入された ASP.NET 機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="43f71-109">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="43f71-110">`WebCache`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="43f71-110">The `WebCache` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="43f71-111">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="43f71-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="43f71-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="43f71-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="43f71-113">このチュートリアルは、ASP.NET Web Pages 2 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="43f71-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="43f71-114">他のユーザーは、サイトからページを要求するたびに、web サーバーは、いくつかの作業を行う要求を処理するためにします。</span><span class="sxs-lookup"><span data-stu-id="43f71-114">Every time someone requests a page from your site, the web server has to do some work in order to fulfill the request.</span></span> <span data-ttu-id="43f71-115">ページの一部で、サーバーは、時間がかかる (比較的)、データベースからデータを取得するなどのタスクを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="43f71-115">For some of your pages, the server might have to perform tasks that take a (comparatively) long time, such as retrieving data from a database.</span></span> <span data-ttu-id="43f71-116">場合でも、サイトには、大量のトラフィックが発生した場合、これらのタスクは、絶対的に長いとらない、一連の web サーバーの複雑なまたは低速のタスクを実行する個々 の要求は、多くの作業追加できます。</span><span class="sxs-lookup"><span data-stu-id="43f71-116">Even if these tasks don't take long in absolute terms, if your site experiences a lot of traffic, a whole series of individual requests that cause the web server to perform the complicated or slow task can add up to a lot of work.</span></span> <span data-ttu-id="43f71-117">これにより、サイトのパフォーマンスが最終的には影響します。</span><span class="sxs-lookup"><span data-stu-id="43f71-117">This can ultimately affect the performance of the site.</span></span>

<span data-ttu-id="43f71-118">次のような状況で、web サイトのパフォーマンスを向上させるために 1 つの方法は、データをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="43f71-118">One way to improve the performance of your website in circumstances like this is to cache data.</span></span> <span data-ttu-id="43f71-119">サイトが、同じ情報を繰り返し要求を取得する場合と情報が各ユーザーに変更する必要はありません、時間がない場合再フェッチまたは再計算するには、代わりに、機密性の高い 1 回に、データをフェッチして結果を格納します。</span><span class="sxs-lookup"><span data-stu-id="43f71-119">If your site gets repeated requests for the same information, and the information does not need to be modified for each person, and it's not time sensitive, instead of re-fetching or recalculating it, you can fetch the data once and then store the results.</span></span> <span data-ttu-id="43f71-120">次回、要求を受け取ったことについては、だけ取得してキャッシュからです。</span><span class="sxs-lookup"><span data-stu-id="43f71-120">The next time a request comes in for that information, you just get it out of the cache.</span></span>

<span data-ttu-id="43f71-121">一般に、頻繁に変化しない情報をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="43f71-121">In general, you cache information that doesn't change frequently.</span></span> <span data-ttu-id="43f71-122">キャッシュ内の情報を格納するときに、web サーバー上のメモリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="43f71-122">When you put information in the cache, it's stored in memory on the web server.</span></span> <span data-ttu-id="43f71-123">時間キャッシュするように、日に秒からを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="43f71-123">You can specify how long it should be cached, from seconds to days.</span></span> <span data-ttu-id="43f71-124">キャッシュ期間が経過すると、情報は自動的にキャッシュから削除します。</span><span class="sxs-lookup"><span data-stu-id="43f71-124">When the caching period expires, the information is automatically removed from the cache.</span></span>

> [!NOTE]
> <span data-ttu-id="43f71-125">キャッシュ内のエントリが削除される上の理由から以外の場合、期限が切れるしました。</span><span class="sxs-lookup"><span data-stu-id="43f71-125">Entries in the cache might be removed for reasons other than that they've expired.</span></span> <span data-ttu-id="43f71-126">たとえば、web サーバーが一時的に実行される低メモリ、およびメモリが再利用できる 1 つの方法はキャッシュからエントリをスローすることによって。</span><span class="sxs-lookup"><span data-stu-id="43f71-126">For example, the web server might temporarily run low on memory, and one way it can reclaim memory is by throwing entries out of the cache.</span></span> <span data-ttu-id="43f71-127">わかる、情報をキャッシュに配置した場合でも、確認しなければならない場合に必要なときではまだ存在します。</span><span class="sxs-lookup"><span data-stu-id="43f71-127">As you'll see, even if you've put information into the cache, you have to check to be sure it's still there when you need it.</span></span>


<span data-ttu-id="43f71-128">Web サイトが、現在の温度と天気予報を表示するページを想像してください。</span><span class="sxs-lookup"><span data-stu-id="43f71-128">Imagine your website has a page that displays the current temperature and weather forecast.</span></span> <span data-ttu-id="43f71-129">この種の情報を取得するには、外部サービスに要求を送信する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="43f71-129">To get this type of information, you might send a request to an external service.</span></span> <span data-ttu-id="43f71-130">この情報はあまり (2 時間の時間内など) を変更しないため、および外部呼び出しには、時間と帯域幅が必要があるため、適切な候補はキャッシュです。</span><span class="sxs-lookup"><span data-stu-id="43f71-130">Since this information doesn't change much (within a two-hour time period, for example) and since external calls require time and bandwidth, it's a good candidate for caching.</span></span>

## <a name="adding-caching-to-a-page"></a><span data-ttu-id="43f71-131">キャッシュ ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="43f71-131">Adding Caching to a Page</span></span>

<span data-ttu-id="43f71-132">ASP.NET には、`WebCache`ヘルパーをサイトにキャッシュを追加し、キャッシュにデータを追加するが容易です。</span><span class="sxs-lookup"><span data-stu-id="43f71-132">ASP.NET includes a `WebCache` helper that makes it easy to add caching to your site and add data to the cache.</span></span> <span data-ttu-id="43f71-133">この手順では、現在の時刻をキャッシュするページを作成します。</span><span class="sxs-lookup"><span data-stu-id="43f71-133">In this procedure, you'll create a page that caches the current time.</span></span> <span data-ttu-id="43f71-134">現在の時刻は、多くの場合、その変更があり、さらにではないを計算する複雑なため実際の例ではありません。</span><span class="sxs-lookup"><span data-stu-id="43f71-134">This isn't a real-world example, since the current time is something that does change often, and that moreover isn't complex to calculate.</span></span> <span data-ttu-id="43f71-135">ただしはキャッシュの動作を説明することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="43f71-135">However, it's a good way to illustrate caching in action.</span></span>

1. <span data-ttu-id="43f71-136">という名前の新しいページを追加*WebCache.cshtml* web サイトを参照します。</span><span class="sxs-lookup"><span data-stu-id="43f71-136">Add a new page named *WebCache.cshtml* to the website.</span></span>
2. <span data-ttu-id="43f71-137">ページには、次のコードとマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="43f71-137">Add the following code and markup to the page:</span></span>

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    <span data-ttu-id="43f71-138">データをキャッシュする場合に配置する名前を使用してキャッシュにこれは、web サイト全体で一意です。</span><span class="sxs-lookup"><span data-stu-id="43f71-138">When you cache data, you put it into the cache using a name this is unique across the website.</span></span> <span data-ttu-id="43f71-139">この場合は、という名前のキャッシュ エントリを使用します`CachedTime`です。</span><span class="sxs-lookup"><span data-stu-id="43f71-139">In this case, you'll use a cache entry named `CachedTime`.</span></span> <span data-ttu-id="43f71-140">これは、`cacheItemKey`のコード例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="43f71-140">This is the `cacheItemKey` shown in the code example.</span></span>

    <span data-ttu-id="43f71-141">コードを読み取り、`CachedTime`エントリをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="43f71-141">The code first reads the `CachedTime` cache entry.</span></span> <span data-ttu-id="43f71-142">(つまり、キャッシュ エントリがない場合 null) 値が返される場合は、コードは、データをキャッシュする時間変数の値をだけ設定します。</span><span class="sxs-lookup"><span data-stu-id="43f71-142">If a value is returned (that is, if the cache entry isn't null), the code just sets the value of the time variable to the cache data.</span></span>

    <span data-ttu-id="43f71-143">ただし、キャッシュ エントリが存在しない場合 (つまり、null である)、コード、時刻の値を設定、キャッシュに追加および有効期限の値を 1 分に設定します。</span><span class="sxs-lookup"><span data-stu-id="43f71-143">However, if the cache entry doesn't exist (that is, it's null), the code sets the time value, adds it to the cache, and sets an expiration value to one minute.</span></span> <span data-ttu-id="43f71-144">1 分後、キャッシュ エントリは破棄されます。</span><span class="sxs-lookup"><span data-stu-id="43f71-144">After one minute, the cache entry is discarded.</span></span> <span data-ttu-id="43f71-145">(キャッシュ内のアイテムを既定の有効期限の値は 20 分です)。コマンドは、`WebCache.Set(cacheItemKey, time, 1, false)`キャッシュに現在の時刻値を追加し、1 分に、有効期限を設定する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="43f71-145">(The default expiration value for an item in the cache is 20 minutes.) The command `WebCache.Set(cacheItemKey, time, 1, false)` shows how to add the current time value to the cache and set its expiration to 1 minute.</span></span> <span data-ttu-id="43f71-146">設定、 *slidingExpiration*パラメーターを`false`有効期限が要求されるたびに更新されていないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="43f71-146">Setting the *slidingExpiration* parameter to `false` means the expiration time is not renewed each time it is requested.</span></span> <span data-ttu-id="43f71-147">最初にキャッシュに追加された後に、正確に 1 分に期限が切れます。</span><span class="sxs-lookup"><span data-stu-id="43f71-147">It will expire exactly 1 minute after it was originally added to the cache.</span></span> <span data-ttu-id="43f71-148">この値を設定する場合`true`1 分の有効期限は、値がキャッシュから要求されるたびをリセットします。</span><span class="sxs-lookup"><span data-stu-id="43f71-148">If you set this value to `true` the 1 minute expiration time is reset each time the value is requested from the cache.</span></span>

    <span data-ttu-id="43f71-149">このコードは、データをキャッシュするときに常に使用する必要があります、パターンを示します。</span><span class="sxs-lookup"><span data-stu-id="43f71-149">This code illustrates the pattern you should always use when you cache data.</span></span> <span data-ttu-id="43f71-150">キャッシュからのものを取得する前に常に最初がチェックするかどうか、`WebCache.Get`メソッドが null を返しました。</span><span class="sxs-lookup"><span data-stu-id="43f71-150">Before you get something out of the cache, always check first whether the `WebCache.Get` method has returned null.</span></span> <span data-ttu-id="43f71-151">キャッシュ エントリの期限が切れている可能性がありますかが削除された可能性が何らかの理由により、キャッシュ内にある、指定したエントリが保証されることはありませんのでことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="43f71-151">Remember that the cache entry might have expired or might have been removed for some other reason, so any given entry is never guaranteed to be in the cache.</span></span>
3. <span data-ttu-id="43f71-152">実行*WebCache.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="43f71-152">Run *WebCache.cshtml* in a browser.</span></span> <span data-ttu-id="43f71-153">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。初めて、ページを要求する時のデータは、キャッシュではありません、コードには、キャッシュに時刻の値を追加するには</span><span class="sxs-lookup"><span data-stu-id="43f71-153">(Make sure the page is selected in the **Files** workspace before you run it.) The first time you request the page, the time data isn't in the cache, and the code has to add the time value to the cache.</span></span>

    ![キャッシュ-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. <span data-ttu-id="43f71-155">更新*WebCache.cshtml*ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="43f71-155">Refresh *WebCache.cshtml* in the browser.</span></span> <span data-ttu-id="43f71-156">現時点では、時間のデータは、キャッシュにです。</span><span class="sxs-lookup"><span data-stu-id="43f71-156">This time, the time data is in the cache.</span></span> <span data-ttu-id="43f71-157">前回のページを表示した後、時間が変更されていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="43f71-157">Notice that the time hasn't changed since the last time you viewed the page.</span></span>

    ![キャッシュ-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. <span data-ttu-id="43f71-159">キャッシュを空にするのには 1 分間待機し、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="43f71-159">Wait one minute for the cache to be emptied, and then refresh the page.</span></span> <span data-ttu-id="43f71-160">ページ再度ことを示す時のデータがキャッシュに見つかりませんでしたが更新された時間が、キャッシュに追加されます。</span><span class="sxs-lookup"><span data-stu-id="43f71-160">The page again indicates that the time data wasn't found in the cache, and the updated time is added to the cache.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="43f71-161">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="43f71-161">Additional Resources</span></span>


- [<span data-ttu-id="43f71-162">グラフのデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="43f71-162">Displaying Data in a Chart</span></span>](https://go.microsoft.com/fwlink/?LinkId=202895)
- <span data-ttu-id="43f71-163">[WebCache API リファレンス](https://msdn.microsoft.com/en-us/library/system.web.helpers.webcache(v=vs.99).aspx)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="43f71-163">[WebCache API reference](https://msdn.microsoft.com/en-us/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)</span></span>
