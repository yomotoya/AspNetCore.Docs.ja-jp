---
title: "ASP.NET Core で応答のキャッシュ"
author: rick-anderson
description: "応答の帯域幅を削減し、パフォーマンスを向上させるキャッシュの使用方法を説明します。"
keywords: "ASP.NET Core、応答の HTTP ヘッダーをキャッシュします。"
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 957bdf5fe24216fa3459ac7ecee0464a45226828
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="63b39-104">ASP.NET Core で応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="63b39-104">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="63b39-105">によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)、および[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="63b39-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="63b39-106">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="63b39-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

<span data-ttu-id="63b39-107">応答のキャッシュ クライアントまたはプロキシ web サーバーに、要求の数を削減します。</span><span class="sxs-lookup"><span data-stu-id="63b39-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="63b39-108">応答のキャッシュ量を削減しても、応答を生成する作業の web サーバーを実行します。</span><span class="sxs-lookup"><span data-stu-id="63b39-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="63b39-109">応答のキャッシュは、クライアント、プロキシ、およびミドルウェアが応答をキャッシュする方法を指定できるヘッダーによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="63b39-110">追加するときに、web サーバーは応答をキャッシュできます[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)です。</span><span class="sxs-lookup"><span data-stu-id="63b39-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="63b39-111">応答の HTTP ベースのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="63b39-111">HTTP-based response caching</span></span>

<span data-ttu-id="63b39-112">[仕様の HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234)インターネット キャッシュの動作方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="63b39-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="63b39-113">キャッシュに使用するプライマリ HTTP ヘッダーは[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)、キャッシュの指定に使用される*ディレクティブ*です。</span><span class="sxs-lookup"><span data-stu-id="63b39-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="63b39-114">ディレクティブは、回答のサーバーからクライアントには、その方法を変更すると、要求のクライアントからサーバーには、その方法を変更すると、キャッシュ動作を制御します。</span><span class="sxs-lookup"><span data-stu-id="63b39-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="63b39-115">要求および応答がプロキシ サーバーで移動し、プロキシ サーバーが HTTP 1.1 キャッシュ仕様に準拠する必要がありますもします。</span><span class="sxs-lookup"><span data-stu-id="63b39-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="63b39-116">一般的な`Cache-Control`ディレクティブが次の表に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="63b39-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="63b39-117">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="63b39-117">Directive</span></span>                                                       | <span data-ttu-id="63b39-118">操作</span><span class="sxs-lookup"><span data-stu-id="63b39-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="63b39-119">public</span><span class="sxs-lookup"><span data-stu-id="63b39-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="63b39-120">キャッシュは、応答を格納できます。</span><span class="sxs-lookup"><span data-stu-id="63b39-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="63b39-121">private</span><span class="sxs-lookup"><span data-stu-id="63b39-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="63b39-122">キャッシュを共有して応答を格納する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="63b39-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="63b39-123">プライベート キャッシュは、格納し、応答を再利用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="63b39-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="63b39-124">最大継続期間</span><span class="sxs-lookup"><span data-stu-id="63b39-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="63b39-125">クライアントが年齢が指定した秒数よりも大きい応答を受け付けませんでした。</span><span class="sxs-lookup"><span data-stu-id="63b39-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="63b39-126">例: `max-age=60` (60 秒) `max-age=2592000` (1 か月)</span><span class="sxs-lookup"><span data-stu-id="63b39-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="63b39-127">キャッシュなし</span><span class="sxs-lookup"><span data-stu-id="63b39-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="63b39-128">**要求に**: 要求を満たせませんストアド応答がキャッシュでは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="63b39-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="63b39-129">注: は、元のサーバーでは、クライアントの場合、応答を再生成し、ミドルウェアがストアド応答をキャッシュを更新します。</span><span class="sxs-lookup"><span data-stu-id="63b39-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="63b39-130">**応答で**: 元のサーバーで検証を伴わない後続の要求の応答を使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="63b39-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="63b39-131">no ストア</span><span class="sxs-lookup"><span data-stu-id="63b39-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="63b39-132">**要求に**: キャッシュは、要求を格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="63b39-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="63b39-133">**応答で**: キャッシュは、応答の任意の部分を格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="63b39-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="63b39-134">その他のキャッシュでは、役割を果たすキャッシュ ヘッダーは、次の表に表示されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="63b39-135">Header</span><span class="sxs-lookup"><span data-stu-id="63b39-135">Header</span></span>                                                     | <span data-ttu-id="63b39-136">関数</span><span class="sxs-lookup"><span data-stu-id="63b39-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="63b39-137">経過時間</span><span class="sxs-lookup"><span data-stu-id="63b39-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="63b39-138">応答の生成または送信元のサーバーに正常に検証されてからの秒単位で時間の推定値。</span><span class="sxs-lookup"><span data-stu-id="63b39-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="63b39-139">有効期限が切れる</span><span class="sxs-lookup"><span data-stu-id="63b39-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="63b39-140">その後、応答は古いと見なされます日付/時刻。</span><span class="sxs-lookup"><span data-stu-id="63b39-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="63b39-141">プラグマ</span><span class="sxs-lookup"><span data-stu-id="63b39-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="63b39-142">Http/1.0 との互換性は設定をキャッシュする下位存在`no-cache`動作します。</span><span class="sxs-lookup"><span data-stu-id="63b39-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="63b39-143">場合、`Cache-Control`ヘッダーが含まれている、`Pragma`ヘッダーは無視されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="63b39-144">異なる</span><span class="sxs-lookup"><span data-stu-id="63b39-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="63b39-145">指定するキャッシュされた応答する必要がありますいない送信しない限り、すべての`Vary`ヘッダー フィールドにキャッシュされた応答の元の要求と、新しい要求の両方に一致します。</span><span class="sxs-lookup"><span data-stu-id="63b39-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="63b39-146">HTTP ベースのキャッシュは要求のキャッシュ制御ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="63b39-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="63b39-147">[Cache-control ヘッダーの HTTP 1.1 キャッシュ仕様](https://tools.ietf.org/html/rfc7234#section-5.2)に従う、有効なキャッシュを必要と`Cache-Control`クライアントによって送信されたヘッダー。</span><span class="sxs-lookup"><span data-stu-id="63b39-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="63b39-148">クライアントが要求を行うことができます、`no-cache`ヘッダーの値と force 要求ごとに新しい応答を生成するサーバー。</span><span class="sxs-lookup"><span data-stu-id="63b39-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="63b39-149">常にクライアントを受け付けたり`Cache-Control`HTTP キャッシュの目標を考慮する場合に要求ヘッダーを意味します。</span><span class="sxs-lookup"><span data-stu-id="63b39-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="63b39-150">公式の仕様では、下にあるキャッシュはクライアント、プロキシ、およびサーバー、ネットワーク経由で要求を満たすの待機時間とネットワークのオーバーヘッドを削減を意図したものです。</span><span class="sxs-lookup"><span data-stu-id="63b39-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="63b39-151">必ずしも元のサーバーの負荷を制御する方法です。</span><span class="sxs-lookup"><span data-stu-id="63b39-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="63b39-152">使用する場合、このキャッシュ動作上の現在の開発者コントロールはありません、[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)ミドルウェアが公式の仕様をキャッシュに準拠しているためです。</span><span class="sxs-lookup"><span data-stu-id="63b39-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="63b39-153">[ミドルウェアは将来拡張](https://github.com/aspnet/ResponseCaching/issues/96)を無視する要求のミドルウェアの構成を許可する`Cache-Control`ヘッダーを提供するキャッシュされた応答を決定するとき。</span><span class="sxs-lookup"><span data-stu-id="63b39-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="63b39-154">これが提供されますをサーバーに適切に制御負荷営業案件、ミドルウェアを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="63b39-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="63b39-155">ASP.NET Core でのキャッシュの他のテクノロジ</span><span class="sxs-lookup"><span data-stu-id="63b39-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="63b39-156">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="63b39-156">In-memory caching</span></span>

<span data-ttu-id="63b39-157">キャッシュされたデータを格納するのにサーバーのメモリを使用するメモリ内キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="63b39-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="63b39-158">この種類のキャッシュは 1 台のサーバーまたは複数のサーバーを使用するのに適した*スティッキー セッション*です。</span><span class="sxs-lookup"><span data-stu-id="63b39-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="63b39-159">スティッキー セッションでは、クライアントからの要求を処理するため、同じサーバーに常にルーティングされることを意味します。</span><span class="sxs-lookup"><span data-stu-id="63b39-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="63b39-160">詳細については、次を参照してください。 [ASP.NET Core でのメモリ内キャッシュの概要](xref:performance/caching/memory)です。</span><span class="sxs-lookup"><span data-stu-id="63b39-160">For more information, see [Introduction to in-memory caching in ASP.NET Core](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="63b39-161">分散キャッシュ</span><span class="sxs-lookup"><span data-stu-id="63b39-161">Distributed Cache</span></span>

<span data-ttu-id="63b39-162">分散キャッシュを使用して、アプリが、クラウド サーバー ファームでホストされている場合は、データをメモリに格納します。</span><span class="sxs-lookup"><span data-stu-id="63b39-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="63b39-163">キャッシュは、要求を処理するサーバー間で共有されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="63b39-164">クライアントは、グループ内のサーバーによって処理される要求を送信できる、クライアントのキャッシュされたデータが利用できます。</span><span class="sxs-lookup"><span data-stu-id="63b39-164">A client can submit a request that is handled by any server in the group and cached data for the client is available.</span></span> <span data-ttu-id="63b39-165">ASP.NET Core は、SQL Server と分散 Redis キャッシュを提供します。</span><span class="sxs-lookup"><span data-stu-id="63b39-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="63b39-166">詳細については、次を参照してください。[分散キャッシュの使用](xref:performance/caching/distributed)です。</span><span class="sxs-lookup"><span data-stu-id="63b39-166">For more information, see [Working with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="63b39-167">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="63b39-167">Cache Tag Helper</span></span>

<span data-ttu-id="63b39-168">キャッシュ タグ ヘルパーの MVC ビューまたは Razor ページからコンテンツをキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="63b39-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="63b39-169">キャッシュ タグ ヘルパーは、メモリ内キャッシュを使用してデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="63b39-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="63b39-170">詳細については、次を参照してください。 [ASP.NET Core MVC でのキャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="63b39-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="63b39-171">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="63b39-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="63b39-172">分散キャッシュ タグ ヘルパーの MVC ビューまたは分散クラウドまたは web ファームのシナリオに Razor ページからコンテンツをキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="63b39-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="63b39-173">分散キャッシュ タグ ヘルパーは、データを格納するのに SQL Server または Redis を使用します。</span><span class="sxs-lookup"><span data-stu-id="63b39-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="63b39-174">詳細については、次を参照してください。[キャッシュ タグ ヘルパーの分散](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="63b39-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="63b39-175">ResponseCache 属性</span><span class="sxs-lookup"><span data-stu-id="63b39-175">ResponseCache attribute</span></span>

<span data-ttu-id="63b39-176">`ResponseCacheAttribute`応答のキャッシュでは、適切なヘッダーを設定するために必要なパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="63b39-176">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="63b39-177">参照してください[ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)パラメーターの詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="63b39-177">See [ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) for a description of the parameters.</span></span>

> [!WARNING]
> <span data-ttu-id="63b39-178">認証されたクライアントに関する情報を含むコンテンツのキャッシュを無効にします。</span><span class="sxs-lookup"><span data-stu-id="63b39-178">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="63b39-179">キャッシュのみ有効にするユーザーの id や、ユーザーがログインしているかどうかに基づいてコンテンツは変更されません。</span><span class="sxs-lookup"><span data-stu-id="63b39-179">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is logged in.</span></span>

<span data-ttu-id="63b39-180">`VaryByQueryKeys string[]`(ASP.NET Core 1.1 以降が必要です)。 設定すると、応答のキャッシュ ミドルウェアが応答の識別ストアド クエリ キーの指定された一覧の値。</span><span class="sxs-lookup"><span data-stu-id="63b39-180">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1 and higher): When set, the Response Caching Middleware varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="63b39-181">設定するには、応答のキャッシュ ミドルウェアを有効にする必要があります、`VaryByQueryKeys`プロパティです。 それ以外の場合、ランタイム例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="63b39-181">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="63b39-182">対応する HTTP ヘッダーがない、`VaryByQueryKeys`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="63b39-182">There's no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="63b39-183">このプロパティは、応答のキャッシュ ミドルウェアによって処理される HTTP 機能です。</span><span class="sxs-lookup"><span data-stu-id="63b39-183">This property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="63b39-184">キャッシュされた応答を提供するミドルウェアをクエリ文字列とクエリ文字列の値必要がありますと一致前の要求。</span><span class="sxs-lookup"><span data-stu-id="63b39-184">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="63b39-185">たとえば、要求と、次の表に示すように結果のシーケンスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="63b39-185">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="63b39-186">要求</span><span class="sxs-lookup"><span data-stu-id="63b39-186">Request</span></span>                          | <span data-ttu-id="63b39-187">結果</span><span class="sxs-lookup"><span data-stu-id="63b39-187">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="63b39-188">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="63b39-188">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="63b39-189">ミドルウェアから返される</span><span class="sxs-lookup"><span data-stu-id="63b39-189">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="63b39-190">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="63b39-190">Returned from server</span></span>     |

<span data-ttu-id="63b39-191">最初の要求がサーバーによって返され、ミドルウェア内でキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="63b39-191">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="63b39-192">2 番目の要求は、クエリ文字列が、前回の要求と一致するので、ミドルウェアによって返されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-192">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="63b39-193">3 番目の要求ではありません、ミドルウェア キャッシュにクエリ文字列の値は、前の要求と一致しません。</span><span class="sxs-lookup"><span data-stu-id="63b39-193">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="63b39-194">`ResponseCacheAttribute`構成を作成するために使用 (を介して`IFilterFactory`)、`ResponseCacheFilter`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-194">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="63b39-195">`ResponseCacheFilter`適切な HTTP ヘッダーと応答の機能の更新の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="63b39-195">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="63b39-196">フィルター:</span><span class="sxs-lookup"><span data-stu-id="63b39-196">The filter:</span></span>

* <span data-ttu-id="63b39-197">既存のすべてのヘッダーを削除`Vary`、 `Cache-Control`、および`Pragma`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-197">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="63b39-198">設定されたプロパティに基づいて、適切なヘッダーを書き込みます、`ResponseCacheAttribute`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-198">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="63b39-199">更新プログラムの応答の場合は、HTTP 機能をキャッシュ`VaryByQueryKeys`が設定されています。</span><span class="sxs-lookup"><span data-stu-id="63b39-199">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="63b39-200">異なる</span><span class="sxs-lookup"><span data-stu-id="63b39-200">Vary</span></span>

<span data-ttu-id="63b39-201">このヘッダーがのみ書き込まれるときに、`VaryByHeader`プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="63b39-201">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="63b39-202">設定されている、`Vary`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="63b39-202">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="63b39-203">次のサンプルは、`VaryByHeader`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="63b39-203">The following sample uses the `VaryByHeader` property:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

<span data-ttu-id="63b39-204">ブラウザーのネットワーク ツールを使用して応答ヘッダーを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="63b39-204">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="63b39-205">次の図は出力エッジ F12、**ネットワーク** タブのときに、`About2`アクション メソッドが更新されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-205">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![About2 アクション メソッドが呼び出されたときに、[ネットワーク] タブで F12 出力をエッジします。](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="63b39-207">NoStore と Location.None</span><span class="sxs-lookup"><span data-stu-id="63b39-207">NoStore and Location.None</span></span>

<span data-ttu-id="63b39-208">`NoStore`ほとんどの他のプロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="63b39-208">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="63b39-209">このプロパティに設定するときに`true`、`Cache-Control`ヘッダーに設定されて`no-store`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-209">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="63b39-210">場合`Location`に設定されている`None`:</span><span class="sxs-lookup"><span data-stu-id="63b39-210">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="63b39-211">`Cache-Control` が `no-store,no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-211">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="63b39-212">`Pragma` が `no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-212">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="63b39-213">場合`NoStore`は`false`と`Location`は`None`、`Cache-Control`と`Pragma`に設定されている`no-cache`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-213">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="63b39-214">通常は設定`NoStore`に`true`エラー ページにします。</span><span class="sxs-lookup"><span data-stu-id="63b39-214">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="63b39-215">例:</span><span class="sxs-lookup"><span data-stu-id="63b39-215">For example:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

<span data-ttu-id="63b39-216">これは、結果、次のヘッダーになります。</span><span class="sxs-lookup"><span data-stu-id="63b39-216">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="63b39-217">位置と長さ</span><span class="sxs-lookup"><span data-stu-id="63b39-217">Location and Duration</span></span>

<span data-ttu-id="63b39-218">キャッシュを有効にする`Duration`正の値に設定する必要がありますと`Location`いずれかである必要があります`Any`(既定) または`Client`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-218">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="63b39-219">ここで、`Cache-Control`場所の値の後にヘッダーが設定されている、`max-age`応答のです。</span><span class="sxs-lookup"><span data-stu-id="63b39-219">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="63b39-220">`Location`オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="63b39-220">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="63b39-221">前述のように、設定`Location`に`None`設定両方`Cache-Control`と`Pragma`ヘッダーを`no-cache`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-221">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="63b39-222">次のヘッダーを示す例によって生成される設定`Duration`し、既定のままにして`Location`値。</span><span class="sxs-lookup"><span data-stu-id="63b39-222">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

<span data-ttu-id="63b39-223">これには、次のヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="63b39-223">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="63b39-224">キャッシュ プロファイル</span><span class="sxs-lookup"><span data-stu-id="63b39-224">Cache profiles</span></span>

<span data-ttu-id="63b39-225">複製ではなく`ResponseCache`で MVC を設定するとき、オプションとしてコント ローラー アクションの多くの属性でキャッシュ プロファイルの設定を構成できます、`ConfigureServices`メソッド`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="63b39-225">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="63b39-226">によって既定値として参照されているキャッシュ プロファイル内の値が使用されます、`ResponseCache`属性および属性に指定したプロパティによって上書きされます。</span><span class="sxs-lookup"><span data-stu-id="63b39-226">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="63b39-227">キャッシュ プロファイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="63b39-227">Setting up a cache profile:</span></span>

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

<span data-ttu-id="63b39-228">キャッシュ プロファイルを参照するには。</span><span class="sxs-lookup"><span data-stu-id="63b39-228">Referencing a cache profile:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

<span data-ttu-id="63b39-229">`ResponseCache` (メソッド) のアクションとコント ローラー (クラス) の両方に属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="63b39-229">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="63b39-230">メソッド レベルの属性は、クラス レベルの属性で指定された設定をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="63b39-230">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="63b39-231">上記の例では、クラス レベルの属性は、メソッド レベルの属性は、継続時間が 60 秒に設定するキャッシュ プロファイルを参照中に 30 秒の期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="63b39-231">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="63b39-232">結果として得られるヘッダー。</span><span class="sxs-lookup"><span data-stu-id="63b39-232">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="63b39-233">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="63b39-233">Additional resources</span></span>

* [<span data-ttu-id="63b39-234">指定から HTTP でのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="63b39-234">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="63b39-235">キャッシュ制御</span><span class="sxs-lookup"><span data-stu-id="63b39-235">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="63b39-236">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="63b39-236">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
