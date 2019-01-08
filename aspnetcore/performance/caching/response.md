---
title: ASP.NET Core で応答のキャッシュ
author: rick-anderson
description: 応答キャッシュを使用し、帯域幅要件を下げ、ASP.NET Core アプリのパフォーマンスを上げる方法について説明します。
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098949"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="c8751-103">ASP.NET Core で応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="c8751-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="c8751-104">によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)、および[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c8751-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="c8751-105">Razor ページの応答のキャッシュは、ASP.NET Core 2.1 で使用できる以降です。</span><span class="sxs-lookup"><span data-stu-id="c8751-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="c8751-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c8751-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c8751-107">応答のキャッシュ クライアントまたはプロキシは、web サーバーに要求の数が減少します。</span><span class="sxs-lookup"><span data-stu-id="c8751-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="c8751-108">応答のキャッシュ量が減少も、応答を生成する作業の web サーバーを実行します。</span><span class="sxs-lookup"><span data-stu-id="c8751-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="c8751-109">応答のキャッシュは、クライアント、プロキシ、およびミドルウェアが応答をキャッシュする方法を指定するヘッダーによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="c8751-110">[ResponseCache 属性](#responsecache-attribute)応答のキャッシュ クライアントが応答をキャッシュする場合に従うことがあります、ヘッダーの設定に参加します。</span><span class="sxs-lookup"><span data-stu-id="c8751-110">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="c8751-111">[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)サーバーの応答をキャッシュに使用できます。</span><span class="sxs-lookup"><span data-stu-id="c8751-111">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="c8751-112">ミドルウェアが使用できる`ResponseCache`属性のサーバー側のキャッシュ動作を決定するプロパティ。</span><span class="sxs-lookup"><span data-stu-id="c8751-112">The middleware can use `ResponseCache` attribute properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="c8751-113">応答の HTTP ベースのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="c8751-113">HTTP-based response caching</span></span>

<span data-ttu-id="c8751-114">[仕様の HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234)インターネット キャッシュの動作方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c8751-114">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="c8751-115">キャッシュに使用される主な HTTP ヘッダーが[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)、キャッシュの指定に使用される*ディレクティブ*します。</span><span class="sxs-lookup"><span data-stu-id="c8751-115">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="c8751-116">ディレクティブは、要求で利用できるようクライアントからサーバーへと応答に届けられますサーバーからクライアントに返送時のキャッシュ動作を制御します。</span><span class="sxs-lookup"><span data-stu-id="c8751-116">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="c8751-117">要求と応答は、プロキシ サーバーを介して移動し、プロキシ サーバーが HTTP 1.1 キャッシュの仕様に準拠する必要がありますもします。</span><span class="sxs-lookup"><span data-stu-id="c8751-117">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="c8751-118">一般的な`Cache-Control`ディレクティブは、次の表に表示されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-118">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="c8751-119">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="c8751-119">Directive</span></span>                                                       | <span data-ttu-id="c8751-120">アクション</span><span class="sxs-lookup"><span data-stu-id="c8751-120">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="c8751-121">public</span><span class="sxs-lookup"><span data-stu-id="c8751-121">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="c8751-122">キャッシュは、応答を格納することができます。</span><span class="sxs-lookup"><span data-stu-id="c8751-122">A cache may store the response.</span></span> |
| [<span data-ttu-id="c8751-123">private</span><span class="sxs-lookup"><span data-stu-id="c8751-123">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="c8751-124">共有キャッシュで応答を格納する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="c8751-124">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="c8751-125">プライベート キャッシュを格納および応答を再利用できます。</span><span class="sxs-lookup"><span data-stu-id="c8751-125">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="c8751-126">max-age</span><span class="sxs-lookup"><span data-stu-id="c8751-126">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="c8751-127">クライアントは応答として、年齢が指定した秒数より大きいを受け付けません。</span><span class="sxs-lookup"><span data-stu-id="c8751-127">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="c8751-128">次に例を示します。`max-age=60` (60 秒) `max-age=2592000` (1 か月)</span><span class="sxs-lookup"><span data-stu-id="c8751-128">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="c8751-129">no-cache</span><span class="sxs-lookup"><span data-stu-id="c8751-129">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="c8751-130">**要求で**:キャッシュ要求を満たすためにストアド応答を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c8751-130">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="c8751-131">メモ:配信元サーバーがクライアントの場合、応答を再生成し、ミドルウェアがそのキャッシュ内のストアドの応答を更新します。</span><span class="sxs-lookup"><span data-stu-id="c8751-131">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="c8751-132">**応答に**:後続の要求、配信元サーバーの検証を行わずに、応答を使用しない必要があります。</span><span class="sxs-lookup"><span data-stu-id="c8751-132">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="c8751-133">no-store</span><span class="sxs-lookup"><span data-stu-id="c8751-133">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="c8751-134">**要求で**:キャッシュは、要求を格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c8751-134">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="c8751-135">**応答に**:キャッシュは、応答の一部を格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c8751-135">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="c8751-136">次の表は、キャッシュ内の役割を果たすその他のキャッシュ ヘッダーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-136">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="c8751-137">Header</span><span class="sxs-lookup"><span data-stu-id="c8751-137">Header</span></span>                                                     | <span data-ttu-id="c8751-138">関数</span><span class="sxs-lookup"><span data-stu-id="c8751-138">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="c8751-139">経過時間</span><span class="sxs-lookup"><span data-stu-id="c8751-139">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="c8751-140">生成された応答からの経過時間の推定値、または配信元サーバーに正常に検証します。</span><span class="sxs-lookup"><span data-stu-id="c8751-140">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="c8751-141">有効期限が切れます</span><span class="sxs-lookup"><span data-stu-id="c8751-141">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="c8751-142">その後、応答が古いと見なされます日付/時刻。</span><span class="sxs-lookup"><span data-stu-id="c8751-142">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="c8751-143">プラグマ</span><span class="sxs-lookup"><span data-stu-id="c8751-143">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="c8751-144">Http/1.0 との互換性設定は、キャッシュ内を後方に向かって存在`no-cache`動作します。</span><span class="sxs-lookup"><span data-stu-id="c8751-144">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="c8751-145">場合、`Cache-Control`ヘッダーが存在する、`Pragma`ヘッダーは無視されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-145">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="c8751-146">異なる</span><span class="sxs-lookup"><span data-stu-id="c8751-146">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="c8751-147">指定するキャッシュされた応答する必要がありますいない送信しない限り、すべての`Vary`ヘッダー フィールドに、キャッシュされた応答の元の要求と、新しい要求の両方に一致します。</span><span class="sxs-lookup"><span data-stu-id="c8751-147">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="c8751-148">HTTP ベースのキャッシュは要求のキャッシュ制御ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="c8751-148">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="c8751-149">[Cache-control ヘッダーの HTTP 1.1 キャッシュ仕様](https://tools.ietf.org/html/rfc7234#section-5.2)を優先する有効なキャッシュを必要と`Cache-Control`クライアントによって送信されたヘッダー。</span><span class="sxs-lookup"><span data-stu-id="c8751-149">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="c8751-150">クライアントがで要求を行うことができます、`no-cache`ヘッダーの値と force、サーバー要求のたびに新しい応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="c8751-150">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="c8751-151">クライアントを常に受け入れること`Cache-Control`HTTP キャッシュの目的を検討する場合に要求ヘッダーを意味します。</span><span class="sxs-lookup"><span data-stu-id="c8751-151">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="c8751-152">公式の仕様では、キャッシュは、クライアント、プロキシ、およびサーバーのネットワーク要求を満たすの待機時間とネットワークのオーバーヘッドを削減するもの。</span><span class="sxs-lookup"><span data-stu-id="c8751-152">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="c8751-153">必ずしも、配信元サーバーの負荷を制御する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="c8751-153">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="c8751-154">使用する場合、このキャッシュの動作に対する開発者のコントロールがない、[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)ミドルウェアは、公式のキャッシュの仕様に準拠しているためです。</span><span class="sxs-lookup"><span data-stu-id="c8751-154">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="c8751-155">[ミドルウェアの機能強化を計画](https://github.com/aspnet/AspNetCore/issues/2612)要求を無視するミドルウェアを構成する機会は`Cache-Control`ヘッダー キャッシュされた応答を処理するために決定する際にします。</span><span class="sxs-lookup"><span data-stu-id="c8751-155">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="c8751-156">計画的な機能強化では、管理サーバーの負荷を向上する機会を提供します。</span><span class="sxs-lookup"><span data-stu-id="c8751-156">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="c8751-157">ASP.NET Core でのキャッシュの他のテクノロジ</span><span class="sxs-lookup"><span data-stu-id="c8751-157">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="c8751-158">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="c8751-158">In-memory caching</span></span>

<span data-ttu-id="c8751-159">メモリ内キャッシュでは、サーバー メモリを使用して、キャッシュされたデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="c8751-159">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="c8751-160">この種のキャッシュは 1 台のサーバーまたは複数のサーバーに適した*スティッキー セッション*します。</span><span class="sxs-lookup"><span data-stu-id="c8751-160">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="c8751-161">スティッキー セッションでは、クライアントからの要求を処理するため、同じサーバーに常にルーティングされることを意味します。</span><span class="sxs-lookup"><span data-stu-id="c8751-161">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="c8751-162">詳細については、次を参照してください。[メモリ内キャッシュ](xref:performance/caching/memory)します。</span><span class="sxs-lookup"><span data-stu-id="c8751-162">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="c8751-163">分散キャッシュ</span><span class="sxs-lookup"><span data-stu-id="c8751-163">Distributed Cache</span></span>

<span data-ttu-id="c8751-164">分散キャッシュを使用して、アプリがクラウドまたはサーバー ファームでホストされている場合は、データをメモリに格納します。</span><span class="sxs-lookup"><span data-stu-id="c8751-164">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="c8751-165">キャッシュは、要求を処理するサーバー間で共有されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-165">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="c8751-166">クライアントでは、クライアントのキャッシュされたデータが使用可能な場合は、グループ内のサーバーによって処理される要求を送信できます。</span><span class="sxs-lookup"><span data-stu-id="c8751-166">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="c8751-167">ASP.NET Core には、SQL Server 分散 Redis キャッシュが提供しています。</span><span class="sxs-lookup"><span data-stu-id="c8751-167">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="c8751-168">詳細については、「 <xref:performance/caching/distributed> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c8751-168">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="c8751-169">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="c8751-169">Cache Tag Helper</span></span>

<span data-ttu-id="c8751-170">キャッシュ タグ ヘルパーでは、MVC ビューまたは Razor ページからコンテンツをキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="c8751-170">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="c8751-171">キャッシュ タグ ヘルパーは、メモリ内キャッシュを使用してデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="c8751-171">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="c8751-172">詳細については、次を参照してください。 [ASP.NET Core MVC でのキャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)します。</span><span class="sxs-lookup"><span data-stu-id="c8751-172">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="c8751-173">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="c8751-173">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="c8751-174">分散キャッシュ タグ ヘルパーでは、MVC ビューまたは分散クラウドまたは web ファームのシナリオでの Razor ページからコンテンツをキャッシュできます。</span><span class="sxs-lookup"><span data-stu-id="c8751-174">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="c8751-175">分散キャッシュ タグ ヘルパーは、データを格納するのに SQL Server または Redis を使用します。</span><span class="sxs-lookup"><span data-stu-id="c8751-175">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="c8751-176">詳細については、次を参照してください。[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)します。</span><span class="sxs-lookup"><span data-stu-id="c8751-176">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="c8751-177">ResponseCache 属性</span><span class="sxs-lookup"><span data-stu-id="c8751-177">ResponseCache attribute</span></span>

<span data-ttu-id="c8751-178">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)応答のキャッシュで適切なヘッダーを設定するために必要なパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="c8751-178">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="c8751-179">認証されたクライアントの情報を含むコンテンツのキャッシュを無効にします。</span><span class="sxs-lookup"><span data-stu-id="c8751-179">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="c8751-180">キャッシュは、ユーザーの id またはユーザーがサインインしているかどうかに基づいて変更されないコンテンツのみ有効にします。</span><span class="sxs-lookup"><span data-stu-id="c8751-180">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="c8751-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)ストアドの応答を指定した一連のクエリ キーの値によって異なります。</span><span class="sxs-lookup"><span data-stu-id="c8751-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="c8751-182">1 つの値のときに`*`応答により、すべての要求クエリ文字列パラメーターが指定されて、ミドルウェアによって異なります。</span><span class="sxs-lookup"><span data-stu-id="c8751-182">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="c8751-183">`VaryByQueryKeys` ASP.NET Core 1.1 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="c8751-183">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="c8751-184">[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)設定を有効にする必要があります、`VaryByQueryKeys`プロパティ。 それ以外の場合、ランタイム例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="c8751-184">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="c8751-185">対応する HTTP ヘッダーがない、`VaryByQueryKeys`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c8751-185">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="c8751-186">プロパティは、応答キャッシュ ミドルウェアによって処理される HTTP 機能です。</span><span class="sxs-lookup"><span data-stu-id="c8751-186">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="c8751-187">キャッシュされた応答を処理するためにミドルウェアのクエリ文字列とクエリ文字列の値は、前の要求に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c8751-187">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="c8751-188">たとえば、要求と、次の表に示すように結果のシーケンスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="c8751-188">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="c8751-189">要求</span><span class="sxs-lookup"><span data-stu-id="c8751-189">Request</span></span>                          | <span data-ttu-id="c8751-190">結果</span><span class="sxs-lookup"><span data-stu-id="c8751-190">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="c8751-191">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="c8751-191">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="c8751-192">ミドルウェアから返される</span><span class="sxs-lookup"><span data-stu-id="c8751-192">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="c8751-193">サーバーから返される</span><span class="sxs-lookup"><span data-stu-id="c8751-193">Returned from server</span></span>     |

<span data-ttu-id="c8751-194">最初の要求は、サーバーによって返され、ミドルウェアにキャッシュされています。</span><span class="sxs-lookup"><span data-stu-id="c8751-194">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="c8751-195">2 番目の要求は、クエリ文字列には、前の要求が一致するためにミドルウェアによって返されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-195">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="c8751-196">3 番目の要求は、クエリ文字列の値は、前の要求と一致しないために、ミドルウェアのキャッシュではありません。</span><span class="sxs-lookup"><span data-stu-id="c8751-196">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="c8751-197">`ResponseCacheAttribute`構成し、作成に使用されます (を使用して`IFilterFactory`)、 [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)します。</span><span class="sxs-lookup"><span data-stu-id="c8751-197">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="c8751-198">`ResponseCacheFilter`適切な HTTP ヘッダーと応答の機能の更新の作業を実行します。</span><span class="sxs-lookup"><span data-stu-id="c8751-198">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="c8751-199">フィルター:</span><span class="sxs-lookup"><span data-stu-id="c8751-199">The filter:</span></span>

* <span data-ttu-id="c8751-200">任意の既存のヘッダーを削除します。 `Vary`、 `Cache-Control`、および`Pragma`します。</span><span class="sxs-lookup"><span data-stu-id="c8751-200">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="c8751-201">設定されたプロパティに基づいて、適切なヘッダーを書き込みます、`ResponseCacheAttribute`します。</span><span class="sxs-lookup"><span data-stu-id="c8751-201">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="c8751-202">更新プログラムの応答の場合は、HTTP の機能をキャッシュ`VaryByQueryKeys`設定されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-202">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="c8751-203">異なる</span><span class="sxs-lookup"><span data-stu-id="c8751-203">Vary</span></span>

<span data-ttu-id="c8751-204">このヘッダーがのみ書き込まれるときに、`VaryByHeader`プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="c8751-204">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="c8751-205">設定されている、`Vary`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="c8751-205">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="c8751-206">次のサンプルは、`VaryByHeader`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c8751-206">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="c8751-207">ブラウザーのネットワーク ツールを使用して、応答ヘッダーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="c8751-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="c8751-208">次の図は、出力で Microsoft Edge の f12 キー、**ネットワーク**ときにタブ、`About2`アクション メソッドが更新されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![About2 アクション メソッドが呼び出されたときに、[ネットワーク] タブで F12 出力をエッジします。](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="c8751-210">NoStore と Location.None</span><span class="sxs-lookup"><span data-stu-id="c8751-210">NoStore and Location.None</span></span>

<span data-ttu-id="c8751-211">`NoStore` ほとんどの他のプロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="c8751-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="c8751-212">このプロパティに設定しているときに`true`、`Cache-Control`にヘッダーが設定されている`no-store`します。</span><span class="sxs-lookup"><span data-stu-id="c8751-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="c8751-213">場合`Location`に設定されている`None`:</span><span class="sxs-lookup"><span data-stu-id="c8751-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="c8751-214">`Cache-Control` が `no-store,no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="c8751-215">`Pragma` が `no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="c8751-216">場合`NoStore`は`false`と`Location`は`None`、`Cache-Control`と`Pragma`に設定されている`no-cache`します。</span><span class="sxs-lookup"><span data-stu-id="c8751-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="c8751-217">通常は設定`NoStore`に`true`エラー ページ。</span><span class="sxs-lookup"><span data-stu-id="c8751-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="c8751-218">例:</span><span class="sxs-lookup"><span data-stu-id="c8751-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="c8751-219">これは、次のヘッダーが得られます。</span><span class="sxs-lookup"><span data-stu-id="c8751-219">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="c8751-220">場所と期間</span><span class="sxs-lookup"><span data-stu-id="c8751-220">Location and Duration</span></span>

<span data-ttu-id="c8751-221">キャッシュを有効にする`Duration`正の値に設定する必要がありますと`Location`いずれかである必要があります`Any`(既定値) または`Client`します。</span><span class="sxs-lookup"><span data-stu-id="c8751-221">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="c8751-222">ここで、`Cache-Control`ヘッダー後に場所の値に設定されて、`max-age`応答の。</span><span class="sxs-lookup"><span data-stu-id="c8751-222">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="c8751-223">`Location`オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="c8751-223">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="c8751-224">前述のように、設定`Location`に`None`両方設定`Cache-Control`と`Pragma`ヘッダーを`no-cache`します。</span><span class="sxs-lookup"><span data-stu-id="c8751-224">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="c8751-225">以下は、ヘッダーを示す例によって生成された設定`Duration`し、既定のままにして`Location`値。</span><span class="sxs-lookup"><span data-stu-id="c8751-225">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="c8751-226">これには、次のヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="c8751-226">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="c8751-227">キャッシュ プロファイル</span><span class="sxs-lookup"><span data-stu-id="c8751-227">Cache profiles</span></span>

<span data-ttu-id="c8751-228">複製ではなく`ResponseCache`で MVC をセットアップするときに、オプションとしてコント ローラー アクションの多くの属性、キャッシュ プロファイルの設定を構成できます、`ConfigureServices`メソッド`Startup`します。</span><span class="sxs-lookup"><span data-stu-id="c8751-228">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="c8751-229">値が参照されているキャッシュ プロファイルによって既定値として使用する、`ResponseCache`属性し、属性に指定したプロパティによってオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="c8751-229">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="c8751-230">キャッシュ プロファイルの設定。</span><span class="sxs-lookup"><span data-stu-id="c8751-230">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c8751-231">キャッシュ プロファイルを参照するには。</span><span class="sxs-lookup"><span data-stu-id="c8751-231">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="c8751-232">`ResponseCache`属性は、アクション (メソッド) とコント ローラー (クラス) の両方に適用できます。</span><span class="sxs-lookup"><span data-stu-id="c8751-232">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="c8751-233">メソッド レベルの属性は、クラス レベルの属性で指定された設定をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="c8751-233">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="c8751-234">上記の例では、メソッド レベルの属性は、継続時間が 60 秒に設定キャッシュ プロファイルを参照中に、クラス レベルの属性は 30 秒の期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="c8751-234">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="c8751-235">結果として得られるヘッダー:</span><span class="sxs-lookup"><span data-stu-id="c8751-235">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="c8751-236">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c8751-236">Additional resources</span></span>

* [<span data-ttu-id="c8751-237">応答をキャッシュに格納します。</span><span class="sxs-lookup"><span data-stu-id="c8751-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="c8751-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="c8751-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
