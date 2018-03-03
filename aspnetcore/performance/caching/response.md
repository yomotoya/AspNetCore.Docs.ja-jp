---
title: "ASP.NET Core で応答のキャッシュ"
author: rick-anderson
description: "ASP.NET Core アプリケーションのパフォーマンスを向上させるし、応答の下の帯域幅要件にキャッシュを使用する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/response
ms.openlocfilehash: c654cfd7c2d291849067bfd3297f940018ccb3d8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="2227c-103">ASP.NET Core で応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="2227c-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="2227c-104">によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)、および[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2227c-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="2227c-105">応答のキャッシュ[コア 2.0 の ASP.NET Razor ページではサポートされていません](https://github.com/aspnet/Mvc/issues/6437)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-105">Response caching [isn't supported in Razor Pages with ASP.NET Core 2.0](https://github.com/aspnet/Mvc/issues/6437).</span></span> <span data-ttu-id="2227c-106">この機能がでサポートされる、 [ASP.NET Core 2.1 リリース](https://github.com/aspnet/Home/wiki/Roadmap)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-106">This feature will be supported in the [ASP.NET Core 2.1 release](https://github.com/aspnet/Home/wiki/Roadmap).</span></span>
  
<span data-ttu-id="2227c-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2227c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2227c-108">応答のキャッシュ クライアントまたはプロキシ web サーバーに、要求の数を削減します。</span><span class="sxs-lookup"><span data-stu-id="2227c-108">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="2227c-109">応答のキャッシュ量を削減しても、応答を生成する作業の web サーバーを実行します。</span><span class="sxs-lookup"><span data-stu-id="2227c-109">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="2227c-110">応答のキャッシュは、クライアント、プロキシ、およびミドルウェアが応答をキャッシュする方法を指定できるヘッダーによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-110">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="2227c-111">追加するときに、web サーバーは応答をキャッシュできます[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-111">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="2227c-112">応答の HTTP ベースのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="2227c-112">HTTP-based response caching</span></span>

<span data-ttu-id="2227c-113">[仕様の HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234)インターネット キャッシュの動作方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2227c-113">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="2227c-114">キャッシュに使用するプライマリ HTTP ヘッダーは[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)、キャッシュの指定に使用される*ディレクティブ*です。</span><span class="sxs-lookup"><span data-stu-id="2227c-114">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="2227c-115">ディレクティブは、回答のサーバーからクライアントには、その方法を変更すると、要求のクライアントからサーバーには、その方法を変更すると、キャッシュ動作を制御します。</span><span class="sxs-lookup"><span data-stu-id="2227c-115">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="2227c-116">要求および応答がプロキシ サーバーで移動し、プロキシ サーバーが HTTP 1.1 キャッシュ仕様に準拠する必要がありますもします。</span><span class="sxs-lookup"><span data-stu-id="2227c-116">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="2227c-117">一般的な`Cache-Control`ディレクティブが次の表に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="2227c-117">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="2227c-118">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2227c-118">Directive</span></span>                                                       | <span data-ttu-id="2227c-119">アクション</span><span class="sxs-lookup"><span data-stu-id="2227c-119">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="2227c-120">public</span><span class="sxs-lookup"><span data-stu-id="2227c-120">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="2227c-121">キャッシュは、応答を格納できます。</span><span class="sxs-lookup"><span data-stu-id="2227c-121">A cache may store the response.</span></span> |
| [<span data-ttu-id="2227c-122">private</span><span class="sxs-lookup"><span data-stu-id="2227c-122">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="2227c-123">キャッシュを共有して応答を格納する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="2227c-123">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="2227c-124">プライベート キャッシュは、格納し、応答を再利用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2227c-124">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="2227c-125">max-age</span><span class="sxs-lookup"><span data-stu-id="2227c-125">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="2227c-126">クライアントが年齢が指定した秒数よりも大きい応答を受け付けませんでした。</span><span class="sxs-lookup"><span data-stu-id="2227c-126">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="2227c-127">例: `max-age=60` (60 秒) `max-age=2592000` (1 か月)</span><span class="sxs-lookup"><span data-stu-id="2227c-127">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="2227c-128">no-cache</span><span class="sxs-lookup"><span data-stu-id="2227c-128">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="2227c-129">**要求に**: 要求を満たせませんストアド応答がキャッシュでは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="2227c-129">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="2227c-130">注: は、元のサーバーでは、クライアントの場合、応答を再生成し、ミドルウェアがストアド応答をキャッシュを更新します。</span><span class="sxs-lookup"><span data-stu-id="2227c-130">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="2227c-131">**応答で**: 元のサーバーで検証を伴わない後続の要求の応答を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="2227c-131">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="2227c-132">no-store</span><span class="sxs-lookup"><span data-stu-id="2227c-132">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="2227c-133">**要求に**: キャッシュは、要求を格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="2227c-133">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="2227c-134">**応答で**: キャッシュは、応答の任意の部分を格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="2227c-134">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="2227c-135">その他のキャッシュでは、役割を果たすキャッシュ ヘッダーは、次の表に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-135">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="2227c-136">Header</span><span class="sxs-lookup"><span data-stu-id="2227c-136">Header</span></span>                                                     | <span data-ttu-id="2227c-137">関数</span><span class="sxs-lookup"><span data-stu-id="2227c-137">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="2227c-138">経過時間</span><span class="sxs-lookup"><span data-stu-id="2227c-138">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="2227c-139">応答の生成または送信元のサーバーに正常に検証されてからの秒単位で時間の推定値。</span><span class="sxs-lookup"><span data-stu-id="2227c-139">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="2227c-140">有効期限が切れる</span><span class="sxs-lookup"><span data-stu-id="2227c-140">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="2227c-141">その後、応答は古いと見なされます日付/時刻。</span><span class="sxs-lookup"><span data-stu-id="2227c-141">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="2227c-142">プラグマ</span><span class="sxs-lookup"><span data-stu-id="2227c-142">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="2227c-143">Http/1.0 との互換性は設定をキャッシュする下位存在`no-cache`動作します。</span><span class="sxs-lookup"><span data-stu-id="2227c-143">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="2227c-144">場合、`Cache-Control`ヘッダーが含まれている、`Pragma`ヘッダーは無視されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-144">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="2227c-145">Vary</span><span class="sxs-lookup"><span data-stu-id="2227c-145">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="2227c-146">指定するキャッシュされた応答する必要がありますいない送信しない限り、すべての`Vary`ヘッダー フィールドにキャッシュされた応答の元の要求と、新しい要求の両方に一致します。</span><span class="sxs-lookup"><span data-stu-id="2227c-146">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="2227c-147">HTTP ベースのキャッシュは要求のキャッシュ制御ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="2227c-147">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="2227c-148">[Cache-control ヘッダーの HTTP 1.1 キャッシュ仕様](https://tools.ietf.org/html/rfc7234#section-5.2)に従う、有効なキャッシュを必要と`Cache-Control`クライアントによって送信されたヘッダー。</span><span class="sxs-lookup"><span data-stu-id="2227c-148">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="2227c-149">クライアントが要求を行うことができます、`no-cache`ヘッダーの値と force 要求ごとに新しい応答を生成するサーバー。</span><span class="sxs-lookup"><span data-stu-id="2227c-149">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="2227c-150">常にクライアントを受け付けたり`Cache-Control`HTTP キャッシュの目標を考慮する場合に要求ヘッダーを意味します。</span><span class="sxs-lookup"><span data-stu-id="2227c-150">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="2227c-151">公式の仕様では、下にあるキャッシュはクライアント、プロキシ、およびサーバー、ネットワーク経由で要求を満たすの待機時間とネットワークのオーバーヘッドを削減を意図したものです。</span><span class="sxs-lookup"><span data-stu-id="2227c-151">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="2227c-152">必ずしも元のサーバーの負荷を制御する方法です。</span><span class="sxs-lookup"><span data-stu-id="2227c-152">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="2227c-153">使用する場合、このキャッシュ動作上の現在の開発者コントロールはありません、[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)ミドルウェアが公式の仕様をキャッシュに準拠しているためです。</span><span class="sxs-lookup"><span data-stu-id="2227c-153">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="2227c-154">[ミドルウェアは将来拡張](https://github.com/aspnet/ResponseCaching/issues/96)を無視する要求のミドルウェアの構成を許可する`Cache-Control`ヘッダーを提供するキャッシュされた応答を決定するとき。</span><span class="sxs-lookup"><span data-stu-id="2227c-154">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="2227c-155">これが提供されますをサーバーに適切に制御負荷営業案件、ミドルウェアを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="2227c-155">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="2227c-156">ASP.NET Core でのキャッシュの他のテクノロジ</span><span class="sxs-lookup"><span data-stu-id="2227c-156">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="2227c-157">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="2227c-157">In-memory caching</span></span>

<span data-ttu-id="2227c-158">キャッシュされたデータを格納するのにサーバーのメモリを使用するメモリ内キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="2227c-158">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="2227c-159">この種類のキャッシュは 1 台のサーバーまたは複数のサーバーを使用するのに適した*スティッキー セッション*です。</span><span class="sxs-lookup"><span data-stu-id="2227c-159">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="2227c-160">スティッキー セッションでは、クライアントからの要求を処理するため、同じサーバーに常にルーティングされることを意味します。</span><span class="sxs-lookup"><span data-stu-id="2227c-160">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="2227c-161">詳細については、次を参照してください。 [ASP.NET Core でのメモリ内キャッシュの概要](xref:performance/caching/memory)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-161">For more information, see [Introduction to in-memory caching in ASP.NET Core](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="2227c-162">分散キャッシュ</span><span class="sxs-lookup"><span data-stu-id="2227c-162">Distributed Cache</span></span>

<span data-ttu-id="2227c-163">分散キャッシュを使用して、アプリが、クラウド サーバー ファームでホストされている場合は、データをメモリに格納します。</span><span class="sxs-lookup"><span data-stu-id="2227c-163">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="2227c-164">キャッシュは、要求を処理するサーバー間で共有されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-164">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="2227c-165">クライアントは、クライアントのキャッシュされたデータが使用可能な場合は、グループ内のサーバーによって処理される要求を送信できます。</span><span class="sxs-lookup"><span data-stu-id="2227c-165">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="2227c-166">ASP.NET Core は、SQL Server と分散 Redis キャッシュを提供します。</span><span class="sxs-lookup"><span data-stu-id="2227c-166">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="2227c-167">詳細については、次を参照してください。[分散キャッシュの使用](xref:performance/caching/distributed)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-167">For more information, see [Working with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="2227c-168">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2227c-168">Cache Tag Helper</span></span>

<span data-ttu-id="2227c-169">キャッシュ タグ ヘルパーの MVC ビューまたは Razor ページからコンテンツをキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="2227c-169">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="2227c-170">キャッシュ タグ ヘルパーは、メモリ内キャッシュを使用してデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="2227c-170">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="2227c-171">詳細については、次を参照してください。 [ASP.NET Core MVC でのキャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-171">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="2227c-172">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2227c-172">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="2227c-173">分散キャッシュ タグ ヘルパーの MVC ビューまたは分散クラウドまたは web ファームのシナリオに Razor ページからコンテンツをキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="2227c-173">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="2227c-174">分散キャッシュ タグ ヘルパーは、データを格納するのに SQL Server または Redis を使用します。</span><span class="sxs-lookup"><span data-stu-id="2227c-174">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="2227c-175">詳細については、次を参照してください。[キャッシュ タグ ヘルパーの分散](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-175">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="2227c-176">ResponseCache 属性</span><span class="sxs-lookup"><span data-stu-id="2227c-176">ResponseCache attribute</span></span>

<span data-ttu-id="2227c-177">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)応答のキャッシュでは、適切なヘッダーを設定するために必要なパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="2227c-177">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="2227c-178">認証されたクライアントに関する情報を含むコンテンツのキャッシュを無効にします。</span><span class="sxs-lookup"><span data-stu-id="2227c-178">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="2227c-179">キャッシュのみ有効にするユーザーの id またはユーザーがサインインしているかどうかに基づいてコンテンツは変更されません。</span><span class="sxs-lookup"><span data-stu-id="2227c-179">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="2227c-180">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)ストアドの応答を指定された一連のクエリ キーの値によって異なります。</span><span class="sxs-lookup"><span data-stu-id="2227c-180">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="2227c-181">1 つの値のときに`*`すべてからの応答は要求クエリ文字列パラメーターが用意されて、ミドルウェアによって異なります。</span><span class="sxs-lookup"><span data-stu-id="2227c-181">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="2227c-182">`VaryByQueryKeys` ASP.NET Core 1.1 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="2227c-182">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="2227c-183">設定するには、応答のキャッシュ ミドルウェアを有効にする必要があります、`VaryByQueryKeys`プロパティです。 それ以外の場合、ランタイム例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="2227c-183">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="2227c-184">対応する HTTP ヘッダーがない、`VaryByQueryKeys`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="2227c-184">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="2227c-185">プロパティは、応答のキャッシュ ミドルウェアによって処理される HTTP 機能です。</span><span class="sxs-lookup"><span data-stu-id="2227c-185">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="2227c-186">キャッシュされた応答を提供するミドルウェアをクエリ文字列とクエリ文字列の値必要がありますと一致前の要求。</span><span class="sxs-lookup"><span data-stu-id="2227c-186">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="2227c-187">たとえば、要求と、次の表に示すように結果のシーケンスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="2227c-187">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="2227c-188">要求</span><span class="sxs-lookup"><span data-stu-id="2227c-188">Request</span></span>                          | <span data-ttu-id="2227c-189">結果</span><span class="sxs-lookup"><span data-stu-id="2227c-189">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="2227c-190">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="2227c-190">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="2227c-191">ミドルウェアから返される</span><span class="sxs-lookup"><span data-stu-id="2227c-191">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="2227c-192">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="2227c-192">Returned from server</span></span>     |

<span data-ttu-id="2227c-193">最初の要求がサーバーによって返され、ミドルウェア内でキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="2227c-193">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="2227c-194">2 番目の要求は、クエリ文字列が、前回の要求と一致するので、ミドルウェアによって返されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-194">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="2227c-195">クエリ文字列の値は、前の要求と一致しないため、ミドルウェア キャッシュでは、3 番目の要求がありません。</span><span class="sxs-lookup"><span data-stu-id="2227c-195">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="2227c-196">`ResponseCacheAttribute`構成を作成するために使用 (を介して`IFilterFactory`)、 [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)です。</span><span class="sxs-lookup"><span data-stu-id="2227c-196">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="2227c-197">`ResponseCacheFilter`適切な HTTP ヘッダーと応答の機能の更新の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="2227c-197">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="2227c-198">フィルター:</span><span class="sxs-lookup"><span data-stu-id="2227c-198">The filter:</span></span>

* <span data-ttu-id="2227c-199">既存のすべてのヘッダーを削除`Vary`、 `Cache-Control`、および`Pragma`です。</span><span class="sxs-lookup"><span data-stu-id="2227c-199">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="2227c-200">設定されたプロパティに基づいて、適切なヘッダーを書き込みます、`ResponseCacheAttribute`です。</span><span class="sxs-lookup"><span data-stu-id="2227c-200">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="2227c-201">更新プログラムの応答の場合は、HTTP 機能をキャッシュ`VaryByQueryKeys`が設定されています。</span><span class="sxs-lookup"><span data-stu-id="2227c-201">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="2227c-202">異なる</span><span class="sxs-lookup"><span data-stu-id="2227c-202">Vary</span></span>

<span data-ttu-id="2227c-203">このヘッダーがのみ書き込まれるときに、`VaryByHeader`プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="2227c-203">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="2227c-204">設定されている、`Vary`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="2227c-204">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="2227c-205">次のサンプルは、`VaryByHeader`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="2227c-205">The following sample uses the `VaryByHeader` property:</span></span>

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

<span data-ttu-id="2227c-206">ブラウザーのネットワーク ツールを使用して応答ヘッダーを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="2227c-206">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="2227c-207">次の図は出力エッジ F12、**ネットワーク** タブのときに、`About2`アクション メソッドが更新されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-207">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![About2 アクション メソッドが呼び出されたときに、[ネットワーク] タブで F12 出力をエッジします。](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="2227c-209">NoStore と Location.None</span><span class="sxs-lookup"><span data-stu-id="2227c-209">NoStore and Location.None</span></span>

<span data-ttu-id="2227c-210">`NoStore` ほとんどの他のプロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="2227c-210">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="2227c-211">このプロパティに設定するときに`true`、`Cache-Control`ヘッダーに設定されて`no-store`です。</span><span class="sxs-lookup"><span data-stu-id="2227c-211">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="2227c-212">場合`Location`に設定されている`None`:</span><span class="sxs-lookup"><span data-stu-id="2227c-212">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="2227c-213">`Cache-Control` が `no-store,no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-213">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="2227c-214">`Pragma` が `no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-214">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="2227c-215">場合`NoStore`は`false`と`Location`は`None`、`Cache-Control`と`Pragma`に設定されている`no-cache`です。</span><span class="sxs-lookup"><span data-stu-id="2227c-215">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="2227c-216">通常は設定`NoStore`に`true`エラー ページにします。</span><span class="sxs-lookup"><span data-stu-id="2227c-216">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="2227c-217">例:</span><span class="sxs-lookup"><span data-stu-id="2227c-217">For example:</span></span>

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

<span data-ttu-id="2227c-218">これは、結果、次のヘッダーになります。</span><span class="sxs-lookup"><span data-stu-id="2227c-218">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="2227c-219">位置と長さ</span><span class="sxs-lookup"><span data-stu-id="2227c-219">Location and Duration</span></span>

<span data-ttu-id="2227c-220">キャッシュを有効にする`Duration`正の値に設定する必要がありますと`Location`いずれかである必要があります`Any`(既定) または`Client`です。</span><span class="sxs-lookup"><span data-stu-id="2227c-220">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="2227c-221">ここで、`Cache-Control`場所の値の後にヘッダーが設定されている、`max-age`応答のです。</span><span class="sxs-lookup"><span data-stu-id="2227c-221">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="2227c-222">`Location`オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="2227c-222">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="2227c-223">前述のように、設定`Location`に`None`設定両方`Cache-Control`と`Pragma`ヘッダーを`no-cache`です。</span><span class="sxs-lookup"><span data-stu-id="2227c-223">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="2227c-224">次のヘッダーを示す例によって生成される設定`Duration`し、既定のままにして`Location`値。</span><span class="sxs-lookup"><span data-stu-id="2227c-224">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

<span data-ttu-id="2227c-225">これには、次のヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="2227c-225">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="2227c-226">キャッシュ プロファイル</span><span class="sxs-lookup"><span data-stu-id="2227c-226">Cache profiles</span></span>

<span data-ttu-id="2227c-227">複製ではなく`ResponseCache`で MVC を設定するとき、オプションとしてコント ローラー アクションの多くの属性でキャッシュ プロファイルの設定を構成できます、`ConfigureServices`メソッド`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="2227c-227">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="2227c-228">によって既定値として参照されているキャッシュ プロファイル内の値が使用されます、`ResponseCache`属性および属性に指定したプロパティによって上書きされます。</span><span class="sxs-lookup"><span data-stu-id="2227c-228">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="2227c-229">キャッシュ プロファイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="2227c-229">Setting up a cache profile:</span></span>

[!code-csharp[](response/sample/Startup.cs?name=snippet1)] 

<span data-ttu-id="2227c-230">キャッシュ プロファイルを参照するには。</span><span class="sxs-lookup"><span data-stu-id="2227c-230">Referencing a cache profile:</span></span>

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

<span data-ttu-id="2227c-231">`ResponseCache` (メソッド) のアクションとコント ローラー (クラス) の両方に属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="2227c-231">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="2227c-232">メソッド レベルの属性は、クラス レベルの属性で指定された設定をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="2227c-232">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="2227c-233">上記の例では、クラス レベルの属性は、メソッド レベルの属性は、継続時間が 60 秒に設定するキャッシュ プロファイルを参照中に 30 秒の期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="2227c-233">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="2227c-234">結果として得られるヘッダー。</span><span class="sxs-lookup"><span data-stu-id="2227c-234">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="2227c-235">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="2227c-235">Additional resources</span></span>

* [<span data-ttu-id="2227c-236">指定から HTTP でのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="2227c-236">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="2227c-237">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="2227c-237">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="2227c-238">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="2227c-238">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="2227c-239">分散キャッシュの使用</span><span class="sxs-lookup"><span data-stu-id="2227c-239">Working with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="2227c-240">変更トークンを使用する変更の検出</span><span class="sxs-lookup"><span data-stu-id="2227c-240">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="2227c-241">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="2227c-241">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="2227c-242">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2227c-242">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="2227c-243">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2227c-243">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
