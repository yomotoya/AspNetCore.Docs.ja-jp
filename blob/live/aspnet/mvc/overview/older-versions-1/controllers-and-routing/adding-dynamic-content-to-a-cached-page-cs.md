---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: "キャッシュされたページ (c#) への動的なコンテンツの追加 |Microsoft ドキュメント"
author: microsoft
description: "同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。 キャッシュ後置換では、バナー広告 o などの動的なコンテンツを表示することができます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: bee7e17ee16d75419c215558b1deb7d6f0d79448
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="36e12-104">動的なコンテンツをキャッシュされたページ (c#) に追加します。</span><span class="sxs-lookup"><span data-stu-id="36e12-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="36e12-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="36e12-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="36e12-106">同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="36e12-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="36e12-107">キャッシュ後置換では、バナー広告またはキャッシュされた出力されているページ内のニュース項目などの動的なコンテンツを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="36e12-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="36e12-108">出力キャッシュの利用して、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上できます。</span><span class="sxs-lookup"><span data-stu-id="36e12-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="36e12-109">ページ、ページが要求されるたびに、再生成するのではなくページを 1 回生成し、複数のユーザー用のメモリにキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="36e12-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="36e12-110">問題があります。</span><span class="sxs-lookup"><span data-stu-id="36e12-110">But there is a problem.</span></span> <span data-ttu-id="36e12-111">場合、ページに動的なコンテンツを表示する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="36e12-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="36e12-112">たとえば、バナー広告をページに表示することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="36e12-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="36e12-113">すべてのユーザーが非常に同じ提供情報を表示するようにキャッシュされるバナー広告をしたくありません。</span><span class="sxs-lookup"><span data-stu-id="36e12-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="36e12-114">このように、money を行うはありません!</span><span class="sxs-lookup"><span data-stu-id="36e12-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="36e12-115">幸いにも、簡単な解決策があります。</span><span class="sxs-lookup"><span data-stu-id="36e12-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="36e12-116">呼ばれる、ASP.NET フレームワークの機能の活用*キャッシュ後置換*です。</span><span class="sxs-lookup"><span data-stu-id="36e12-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="36e12-117">キャッシュ後置換では、メモリにキャッシュされているページの動的コンテンツを代用することができます。</span><span class="sxs-lookup"><span data-stu-id="36e12-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="36e12-118">通常は、[OutputCache] 属性を使用してキャッシュ ページを出力すると、ページは、サーバーとクライアント (web ブラウザー) の両方でキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="36e12-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="36e12-119">キャッシュ後置換を使用すると、ページは、サーバー上でのみキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="36e12-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="36e12-120">キャッシュ後置換を使用します。</span><span class="sxs-lookup"><span data-stu-id="36e12-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="36e12-121">キャッシュ後置換を使用するには、2 つの手順が必要です。</span><span class="sxs-lookup"><span data-stu-id="36e12-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="36e12-122">最初に、キャッシュされたページに表示する動的なコンテンツを表す文字列を返すメソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="36e12-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="36e12-123">次に、ページに動的なコンテンツを注入 HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="36e12-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="36e12-124">たとえば、キャッシュされたページの異なるニュース項目をランダムに表示することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="36e12-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="36e12-125">リスト 1 のクラスは、ランダムに 3 つのニュース項目の一覧から 1 つのニュース項目を返す RenderNews() をという名前の 1 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="36e12-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="36e12-126">**1 – Models\News.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="36e12-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="36e12-127">キャッシュ後置換を利用するには、HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="36e12-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="36e12-128">WriteSubstitution() メソッドでコードを設定すると、動的コンテンツを含む、キャッシュされたページの領域を置換します。</span><span class="sxs-lookup"><span data-stu-id="36e12-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="36e12-129">WriteSubstitution() メソッドがビューを一覧表示する 2 で、ランダムなニュース項目を表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="36e12-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="36e12-130">**2 – Views\Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="36e12-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="36e12-131">RenderNews メソッドは、WriteSubstitution() メソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="36e12-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="36e12-132">RenderNews メソッドが呼び出されないことに注意してください (かっこはありません)。</span><span class="sxs-lookup"><span data-stu-id="36e12-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="36e12-133">代わりに、メソッドへの参照は、WriteSubstitution() に渡されます。</span><span class="sxs-lookup"><span data-stu-id="36e12-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="36e12-134">インデックス ビューがキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="36e12-134">The Index view is cached.</span></span> <span data-ttu-id="36e12-135">ビューがリスト 3 のコント ローラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="36e12-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="36e12-136">Index() アクションが 60 秒間キャッシュに保存するインデックス ビューの原因となる [OutputCache] 属性で修飾されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="36e12-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="36e12-137">**3 – controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="36e12-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="36e12-138">場合でも、インデックス ビューをキャッシュすると、インデックス ページを要求するときに別のランダムなニュース項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="36e12-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="36e12-139">インデックス ページを要求するときにページが表示される時刻は 60 秒間の (図 1 を参照してください) は変更されません。</span><span class="sxs-lookup"><span data-stu-id="36e12-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="36e12-140">時刻が変更されないという事実は、ページがキャッシュされていることを証明します。</span><span class="sxs-lookup"><span data-stu-id="36e12-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="36e12-141">各要求と一緒に WriteSubstitution() メソッド – ランダムなニュース項目 – 変更によって、コンテンツが挿入ただし、します。</span><span class="sxs-lookup"><span data-stu-id="36e12-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="36e12-142">**図 1 – キャッシュされたページ内の動的なニュース項目を挿入**</span><span class="sxs-lookup"><span data-stu-id="36e12-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="36e12-144">ヘルパー メソッドでのキャッシュ後置換の使用</span><span class="sxs-lookup"><span data-stu-id="36e12-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="36e12-145">キャッシュ後置換を活用する簡単な方法では、カスタム ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="36e12-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="36e12-146">このアプローチは、ヘルパー メソッドを一覧表示する 4 で説明されています。</span><span class="sxs-lookup"><span data-stu-id="36e12-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="36e12-147">**4 – AdHelper.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="36e12-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="36e12-148">4 を一覧表示するには、2 つのメソッドを公開する静的クラスが含まれます: RenderBanner() と RenderBannerInternal() です。</span><span class="sxs-lookup"><span data-stu-id="36e12-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="36e12-149">RenderBanner() メソッドは、実際のヘルパー メソッドを表します。</span><span class="sxs-lookup"><span data-stu-id="36e12-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="36e12-150">このメソッドは、他のヘルパー メソッドと同じようにビューで Html.RenderBanner() を呼び出すことができるように、標準の ASP.NET MVC の HtmlHelper クラスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="36e12-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="36e12-151">RenderBanner() メソッドは、RenderBannerInternal() メソッドを WriteSubsitution() メソッドに渡す HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="36e12-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="36e12-152">RenderBannerInternal() メソッドは、プライベート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="36e12-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="36e12-153">このメソッドは、ヘルパー メソッドとして公開されません。</span><span class="sxs-lookup"><span data-stu-id="36e12-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="36e12-154">RenderBannerInternal() メソッドは、バナー広告の 3 つのイメージの一覧から、バナー広告の 1 つのイメージをランダムに返します。</span><span class="sxs-lookup"><span data-stu-id="36e12-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="36e12-155">インデックス ビューを一覧表示する 5 では、RenderBanner() ヘルパー メソッドを使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="36e12-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="36e12-156">注意して、追加&lt;% @ インポート %&gt;ディレクティブは MvcApplication1.Helpers 名前空間をインポートするビューの上部に含まれます。</span><span class="sxs-lookup"><span data-stu-id="36e12-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="36e12-157">この名前空間をインポートした場合、RenderBanner() メソッドは Html プロパティのメソッドとして表示されません。</span><span class="sxs-lookup"><span data-stu-id="36e12-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="36e12-158">**5 – Views\Home\Index.aspx で (RenderBanner() メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="36e12-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="36e12-159">5 の一覧表示するビューで表示されたページを要求すると、要求ごとに異なるバナー広告が表示されます (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="36e12-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="36e12-160">ページがキャッシュされるが、RenderBanner() ヘルパー メソッドによって、バナー広告が動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="36e12-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="36e12-161">**図 2 – ランダム バナー広告を表示するインデックス ビュー**</span><span class="sxs-lookup"><span data-stu-id="36e12-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="36e12-163">概要</span><span class="sxs-lookup"><span data-stu-id="36e12-163">Summary</span></span>

<span data-ttu-id="36e12-164">このチュートリアルでは、キャッシュされたページのコンテンツを動的に更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="36e12-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="36e12-165">HttpResponse.WriteSubstitution() メソッドを使用して、キャッシュされたページに挿入する動的なコンテンツを有効にする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="36e12-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="36e12-166">HTML ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="36e12-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="36e12-167">利用可能な限り – 持つ、web アプリケーションのパフォーマンスに大幅な影響を受けることができますをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="36e12-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="36e12-168">このチュートリアルで説明、キャッシュ、ページに動的なコンテンツを表示する必要がある場合にも利用できます。</span><span class="sxs-lookup"><span data-stu-id="36e12-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

>[!div class="step-by-step"]
<span data-ttu-id="36e12-169">[前へ](improving-performance-with-output-caching-cs.md)
[次へ](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="36e12-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
