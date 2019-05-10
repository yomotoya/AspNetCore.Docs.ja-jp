---
title: ASP.NET Core で応答のキャッシュ
author: rick-anderson
description: 応答キャッシュを使用し、帯域幅要件を下げ、ASP.NET Core アプリのパフォーマンスを上げる方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/28/2019
uid: performance/caching/response
ms.openlocfilehash: 2e247dcff2cbaa3711a9206d7237a061ae351e1d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892469"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="bb9af-103">ASP.NET Core で応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="bb9af-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="bb9af-104">によって[John Luo](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)、および[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bb9af-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bb9af-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bb9af-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bb9af-106">応答のキャッシュ クライアントまたはプロキシは、web サーバーに要求の数が減少します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-106">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="bb9af-107">応答のキャッシュ量が減少も、応答を生成する作業の web サーバーを実行します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-107">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="bb9af-108">応答のキャッシュは、クライアント、プロキシ、およびミドルウェアが応答をキャッシュする方法を指定するヘッダーによって制御されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-108">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="bb9af-109">[ResponseCache 属性](#responsecache-attribute)応答のキャッシュ クライアントが応答をキャッシュする場合に従うことがあります、ヘッダーの設定に参加します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-109">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="bb9af-110">[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)サーバーの応答をキャッシュに使用できます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-110">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="bb9af-111">ミドルウェアが使用できる<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>サーバー側のキャッシュ動作を決定するプロパティ。</span><span class="sxs-lookup"><span data-stu-id="bb9af-111">The middleware can use <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="bb9af-112">応答の HTTP ベースのキャッシュ</span><span class="sxs-lookup"><span data-stu-id="bb9af-112">HTTP-based response caching</span></span>

<span data-ttu-id="bb9af-113">[仕様の HTTP 1.1 キャッシュ](https://tools.ietf.org/html/rfc7234)インターネット キャッシュの動作方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-113">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="bb9af-114">キャッシュに使用される主な HTTP ヘッダーが[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)、キャッシュの指定に使用される*ディレクティブ*します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-114">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="bb9af-115">ディレクティブは、要求で利用できるようクライアントからサーバーへと応答に届けられますサーバーからクライアントに返送時のキャッシュ動作を制御します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-115">The directives control caching behavior as requests make their way from clients to servers and as responses make their way from servers back to clients.</span></span> <span data-ttu-id="bb9af-116">要求と応答は、プロキシ サーバーを介して移動し、プロキシ サーバーが HTTP 1.1 キャッシュの仕様に準拠する必要がありますもします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-116">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="bb9af-117">一般的な`Cache-Control`ディレクティブは、次の表に表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-117">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="bb9af-118">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="bb9af-118">Directive</span></span>                                                       | <span data-ttu-id="bb9af-119">アクション</span><span class="sxs-lookup"><span data-stu-id="bb9af-119">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="bb9af-120">public</span><span class="sxs-lookup"><span data-stu-id="bb9af-120">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="bb9af-121">キャッシュは、応答を格納することができます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-121">A cache may store the response.</span></span> |
| [<span data-ttu-id="bb9af-122">private</span><span class="sxs-lookup"><span data-stu-id="bb9af-122">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="bb9af-123">共有キャッシュで応答を格納する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="bb9af-123">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="bb9af-124">プライベート キャッシュを格納および応答を再利用できます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-124">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="bb9af-125">max-age</span><span class="sxs-lookup"><span data-stu-id="bb9af-125">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="bb9af-126">クライアントでは、として、年齢が指定した秒数より大きい応答を受け入れません。</span><span class="sxs-lookup"><span data-stu-id="bb9af-126">The client doesn't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="bb9af-127">次に例を示します。 `max-age=60` (60 秒) `max-age=2592000` (1 か月)</span><span class="sxs-lookup"><span data-stu-id="bb9af-127">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="bb9af-128">no-cache</span><span class="sxs-lookup"><span data-stu-id="bb9af-128">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="bb9af-129">**要求で**:キャッシュ要求を満たすためにストアド応答を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb9af-129">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="bb9af-130">配信元サーバーがクライアントの場合、応答を再生成し、ミドルウェアがストアドのキャッシュに応答を更新します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-130">The origin server regenerates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="bb9af-131">**応答に**:後続の要求、配信元サーバーの検証を行わずに、応答を使用しない必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb9af-131">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="bb9af-132">no-store</span><span class="sxs-lookup"><span data-stu-id="bb9af-132">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="bb9af-133">**要求で**:キャッシュは、要求を格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb9af-133">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="bb9af-134">**応答に**:キャッシュは、応答の一部を格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb9af-134">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="bb9af-135">次の表は、キャッシュ内の役割を果たすその他のキャッシュ ヘッダーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-135">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="bb9af-136">Header</span><span class="sxs-lookup"><span data-stu-id="bb9af-136">Header</span></span>                                                     | <span data-ttu-id="bb9af-137">関数</span><span class="sxs-lookup"><span data-stu-id="bb9af-137">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="bb9af-138">経過時間</span><span class="sxs-lookup"><span data-stu-id="bb9af-138">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="bb9af-139">生成された応答からの経過時間の推定値、または配信元サーバーに正常に検証します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-139">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="bb9af-140">有効期限が切れます</span><span class="sxs-lookup"><span data-stu-id="bb9af-140">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="bb9af-141">応答と見なされるまで時間が古い。</span><span class="sxs-lookup"><span data-stu-id="bb9af-141">The time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="bb9af-142">プラグマ</span><span class="sxs-lookup"><span data-stu-id="bb9af-142">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="bb9af-143">Http/1.0 との互換性設定は、キャッシュ内を後方に向かって存在`no-cache`動作します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-143">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="bb9af-144">場合、`Cache-Control`ヘッダーが存在する、`Pragma`ヘッダーは無視されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-144">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="bb9af-145">異なる</span><span class="sxs-lookup"><span data-stu-id="bb9af-145">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="bb9af-146">指定するキャッシュされた応答する必要がありますいない送信しない限り、すべての`Vary`ヘッダー フィールドに、キャッシュされた応答の元の要求と、新しい要求の両方に一致します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-146">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="bb9af-147">HTTP ベースのキャッシュは要求のキャッシュ制御ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="bb9af-147">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="bb9af-148">[Cache-control ヘッダーの HTTP 1.1 キャッシュ仕様](https://tools.ietf.org/html/rfc7234#section-5.2)を優先する有効なキャッシュを必要と`Cache-Control`クライアントによって送信されたヘッダー。</span><span class="sxs-lookup"><span data-stu-id="bb9af-148">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="bb9af-149">クライアントがで要求を行うことができます、`no-cache`ヘッダーの値と force、サーバー要求のたびに新しい応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-149">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="bb9af-150">クライアントを常に受け入れること`Cache-Control`HTTP キャッシュの目的を検討する場合に要求ヘッダーを意味します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-150">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="bb9af-151">公式の仕様では、キャッシュは、クライアント、プロキシ、およびサーバーのネットワーク要求を満たすの待機時間とネットワークのオーバーヘッドを削減するもの。</span><span class="sxs-lookup"><span data-stu-id="bb9af-151">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="bb9af-152">必ずしも、配信元サーバーの負荷を制御する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="bb9af-152">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="bb9af-153">使用する場合、このキャッシュの動作に対する開発者のコントロールがない、[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)ミドルウェアは、公式のキャッシュの仕様に準拠しているためです。</span><span class="sxs-lookup"><span data-stu-id="bb9af-153">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="bb9af-154">[ミドルウェアの機能強化を計画](https://github.com/aspnet/AspNetCore/issues/2612)要求を無視するミドルウェアを構成する機会は`Cache-Control`ヘッダー キャッシュされた応答を処理するために決定する際にします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-154">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="bb9af-155">計画的な機能強化では、管理サーバーの負荷を向上する機会を提供します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-155">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="bb9af-156">ASP.NET Core でのキャッシュの他のテクノロジ</span><span class="sxs-lookup"><span data-stu-id="bb9af-156">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="bb9af-157">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="bb9af-157">In-memory caching</span></span>

<span data-ttu-id="bb9af-158">メモリ内キャッシュでは、サーバー メモリを使用して、キャッシュされたデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-158">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="bb9af-159">この種のキャッシュは 1 台のサーバーまたは複数のサーバーに適した*スティッキー セッション*します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-159">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="bb9af-160">スティッキー セッションでは、クライアントからの要求を処理するため、同じサーバーに常にルーティングされることを意味します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-160">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="bb9af-161">詳細については、「 <xref:performance/caching/memory> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb9af-161">For more information, see <xref:performance/caching/memory>.</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="bb9af-162">分散キャッシュ</span><span class="sxs-lookup"><span data-stu-id="bb9af-162">Distributed Cache</span></span>

<span data-ttu-id="bb9af-163">分散キャッシュを使用して、アプリがクラウドまたはサーバー ファームでホストされている場合は、データをメモリに格納します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-163">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="bb9af-164">キャッシュは、要求を処理するサーバー間で共有されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-164">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="bb9af-165">クライアントでは、クライアントのキャッシュされたデータが使用可能な場合は、グループ内のサーバーによって処理される要求を送信できます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-165">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="bb9af-166">ASP.NET Core には、SQL Server 分散 Redis キャッシュが提供しています。</span><span class="sxs-lookup"><span data-stu-id="bb9af-166">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="bb9af-167">詳細については、「 <xref:performance/caching/distributed> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb9af-167">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="bb9af-168">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="bb9af-168">Cache Tag Helper</span></span>

<span data-ttu-id="bb9af-169">キャッシュ タグ ヘルパーと MVC ビューまたは Razor ページからコンテンツをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-169">Cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="bb9af-170">キャッシュ タグ ヘルパーは、メモリ内キャッシュを使用してデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-170">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="bb9af-171">詳細については、「 <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb9af-171">For more information, see <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="bb9af-172">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="bb9af-172">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="bb9af-173">分散キャッシュ タグ ヘルパーでは、分散クラウドまたは web ファームのシナリオでは、MVC ビューまたは Razor ページからのコンテンツをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-173">Cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="bb9af-174">分散キャッシュ タグ ヘルパーは、データを格納するのに SQL Server または Redis を使用します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-174">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="bb9af-175">詳細については、「 <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb9af-175">For more information, see <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="bb9af-176">ResponseCache 属性</span><span class="sxs-lookup"><span data-stu-id="bb9af-176">ResponseCache attribute</span></span>

<span data-ttu-id="bb9af-177"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>応答のキャッシュで適切なヘッダーを設定するために必要なパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-177">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="bb9af-178">認証されたクライアントの情報を含むコンテンツのキャッシュを無効にします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-178">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="bb9af-179">キャッシュは、ユーザーの id またはユーザーがサインインしているかどうかに基づいて変更されないコンテンツのみ有効にします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-179">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="bb9af-180"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> ストアドの応答は、クエリ キーの指定された一覧の値によって異なります。</span><span class="sxs-lookup"><span data-stu-id="bb9af-180"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="bb9af-181">1 つの値のときに`*`応答により、すべての要求クエリ文字列パラメーターが指定されて、ミドルウェアによって異なります。</span><span class="sxs-lookup"><span data-stu-id="bb9af-181">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span>

<span data-ttu-id="bb9af-182">[応答キャッシュ ミドルウェア](xref:performance/caching/middleware)設定を有効にする必要があります、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>プロパティ。</span><span class="sxs-lookup"><span data-stu-id="bb9af-182">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="bb9af-183">それ以外の場合、ランタイム例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-183">Otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="bb9af-184">対応する HTTP ヘッダーがない、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>プロパティ。</span><span class="sxs-lookup"><span data-stu-id="bb9af-184">There isn't a corresponding HTTP header for the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="bb9af-185">プロパティは、応答キャッシュ ミドルウェアによって処理される HTTP 機能です。</span><span class="sxs-lookup"><span data-stu-id="bb9af-185">The property is an HTTP feature handled by Response Caching Middleware.</span></span> <span data-ttu-id="bb9af-186">キャッシュされた応答を処理するためにミドルウェアのクエリ文字列とクエリ文字列の値は、前の要求に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb9af-186">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="bb9af-187">たとえば、要求と、次の表に示すように結果のシーケンスがあるとします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-187">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="bb9af-188">要求</span><span class="sxs-lookup"><span data-stu-id="bb9af-188">Request</span></span>                          | <span data-ttu-id="bb9af-189">結果</span><span class="sxs-lookup"><span data-stu-id="bb9af-189">Result</span></span>                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | <span data-ttu-id="bb9af-190">サーバーから返されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-190">Returned from the server.</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="bb9af-191">ミドルウェアから返されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-191">Returned from middleware.</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="bb9af-192">サーバーから返されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-192">Returned from the server.</span></span> |

<span data-ttu-id="bb9af-193">最初の要求は、サーバーによって返され、ミドルウェアにキャッシュされています。</span><span class="sxs-lookup"><span data-stu-id="bb9af-193">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="bb9af-194">2 番目の要求は、クエリ文字列には、前の要求が一致するためにミドルウェアによって返されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-194">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="bb9af-195">3 番目の要求は、クエリ文字列の値は、前の要求と一致しないために、ミドルウェアのキャッシュではありません。</span><span class="sxs-lookup"><span data-stu-id="bb9af-195">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span>

<span data-ttu-id="bb9af-196"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>構成し、作成に使用されます (を使用して<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>)、<xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-196">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> is used to configure and create (via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) a <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>.</span></span> <span data-ttu-id="bb9af-197"><xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>適切な HTTP ヘッダーと応答の機能の更新の作業を実行します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-197">The <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter> performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="bb9af-198">フィルター:</span><span class="sxs-lookup"><span data-stu-id="bb9af-198">The filter:</span></span>

* <span data-ttu-id="bb9af-199">任意の既存のヘッダーを削除します。 `Vary`、 `Cache-Control`、および`Pragma`します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-199">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span>
* <span data-ttu-id="bb9af-200">設定されたプロパティに基づいて、適切なヘッダーを書き込みます、<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-200">Writes out the appropriate headers based on the properties set in the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>.</span></span>
* <span data-ttu-id="bb9af-201">更新プログラムの応答の場合は、HTTP の機能をキャッシュ<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>設定されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-201">Updates the response caching HTTP feature if <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> is set.</span></span>

### <a name="vary"></a><span data-ttu-id="bb9af-202">異なる</span><span class="sxs-lookup"><span data-stu-id="bb9af-202">Vary</span></span>

<span data-ttu-id="bb9af-203">このヘッダーがのみ書き込まれるときに、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader>プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-203">This header is only written when the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property is set.</span></span> <span data-ttu-id="bb9af-204">プロパティに設定、`Vary`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="bb9af-204">The property set to the `Vary` property's value.</span></span> <span data-ttu-id="bb9af-205">次のサンプルは、<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader>プロパティ。</span><span class="sxs-lookup"><span data-stu-id="bb9af-205">The following sample uses the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

<span data-ttu-id="bb9af-206">サンプル アプリを使用して、ブラウザーのネットワーク ツールを使用して、応答ヘッダーを表示します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-206">Using the sample app, view the response headers with the browser's network tools.</span></span> <span data-ttu-id="bb9af-207">次の応答ヘッダーは、Cache1 ページ応答で送信されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-207">The following response headers are sent with the Cache1 page response:</span></span>

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a><span data-ttu-id="bb9af-208">NoStore と Location.None</span><span class="sxs-lookup"><span data-stu-id="bb9af-208">NoStore and Location.None</span></span>

<span data-ttu-id="bb9af-209"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> ほとんどの他のプロパティをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-209"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> overrides most of the other properties.</span></span> <span data-ttu-id="bb9af-210">このプロパティに設定しているときに`true`、`Cache-Control`にヘッダーが設定されている`no-store`します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="bb9af-211">場合<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>に設定されている`None`:</span><span class="sxs-lookup"><span data-stu-id="bb9af-211">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is set to `None`:</span></span>

* <span data-ttu-id="bb9af-212">`Cache-Control` が `no-store,no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="bb9af-213">`Pragma` が `no-cache` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="bb9af-214">場合<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore>は`false`と<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>は`None`、 `Cache-Control`、および`Pragma`に設定されている`no-cache`します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-214">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is `false` and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is `None`, `Cache-Control`, and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="bb9af-215"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 設定されている通常`true`のエラー ページ。</span><span class="sxs-lookup"><span data-stu-id="bb9af-215"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is typically set to `true` for error pages.</span></span> <span data-ttu-id="bb9af-216">サンプル アプリで Cache2 ページには、応答を格納しないようにクライアントに指示する応答ヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-216">The Cache2 page in the sample app produces response headers that instruct the client not to store the response.</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

<span data-ttu-id="bb9af-217">サンプル アプリでは、次のヘッダーと共に Cache2 ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-217">The sample app returns the Cache2 page with the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="bb9af-218">場所と期間</span><span class="sxs-lookup"><span data-stu-id="bb9af-218">Location and Duration</span></span>

<span data-ttu-id="bb9af-219">キャッシュを有効にする<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration>正の値に設定する必要がありますと<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>いずれかである必要があります`Any`(既定値) または`Client`します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-219">To enable caching, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> must be set to a positive value and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="bb9af-220">ここで、`Cache-Control`ヘッダー後に場所の値に設定されて、`max-age`応答の。</span><span class="sxs-lookup"><span data-stu-id="bb9af-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="bb9af-221"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>オプションの`Any`と`Client`変換`Cache-Control`のヘッダー値`public`と`private`、それぞれします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-221"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="bb9af-222">前述のように、設定<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>に`None`両方設定`Cache-Control`と`Pragma`ヘッダーを`no-cache`します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-222">As noted previously, setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="bb9af-223">次の例では、サンプル アプリと設定されたヘッダーから Cache3 ページ モデル<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration>し、既定のままにして<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>値。</span><span class="sxs-lookup"><span data-stu-id="bb9af-223">The following example shows the Cache3 page model from the sample app and the headers produced by setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> and leaving the default <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> value:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

<span data-ttu-id="bb9af-224">サンプル アプリでは、次のヘッダーを含む Cache3 ページが返されます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-224">The sample app returns the Cache3 page with the following header:</span></span>

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a><span data-ttu-id="bb9af-225">キャッシュ プロファイル</span><span class="sxs-lookup"><span data-stu-id="bb9af-225">Cache profiles</span></span>

<span data-ttu-id="bb9af-226">多くのコント ローラー アクション属性の応答のキャッシュ設定ではなく、キャッシュ プロファイルとして構成できるオプションの MVC と Razor ページの設定時に`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-226">Instead of duplicating response cache settings on many controller action attributes, cache profiles can be configured as options when setting up MVC/Razor Pages in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bb9af-227">値が参照されているキャッシュ プロファイルによって既定値として使用する、<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>は属性で指定された任意のプロパティでオーバーライドされるとします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-227">Values found in a referenced cache profile are used as the defaults by the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="bb9af-228">キャッシュ プロファイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-228">Set up a cache profile.</span></span> <span data-ttu-id="bb9af-229">次の例では、30 2 つ目のキャッシュ プロファイルを示しますが、サンプル アプリの`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bb9af-229">The following example shows a 30 second cache profile in the sample app's `Startup.ConfigureServices`:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="bb9af-230">サンプル アプリの Cache4 ページ モデルの参照、`Default30`プロファイルをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-230">The sample app's Cache4 page model references the `Default30` cache profile:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

<span data-ttu-id="bb9af-231"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>に適用できます。</span><span class="sxs-lookup"><span data-stu-id="bb9af-231">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> can be applied to:</span></span>

* <span data-ttu-id="bb9af-232">Razor ページ ハンドラー (クラス)&ndash;ハンドラー メソッドに属性を適用できません。</span><span class="sxs-lookup"><span data-stu-id="bb9af-232">Razor Page handlers (classes) &ndash; Attributes can't be applied to handler methods.</span></span>
* <span data-ttu-id="bb9af-233">MVC コント ローラー (クラス)。</span><span class="sxs-lookup"><span data-stu-id="bb9af-233">MVC controllers (classes).</span></span>
* <span data-ttu-id="bb9af-234">MVC アクション (メソッド)&ndash;メソッド レベルの属性がクラス レベルの属性で指定された設定をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="bb9af-234">MVC actions (methods) &ndash; Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="bb9af-235">結果として得られるヘッダーによって Cache4 ページの応答に適用される、`Default30`キャッシュ プロファイル。</span><span class="sxs-lookup"><span data-stu-id="bb9af-235">The resulting header applied to the Cache4 page response by the `Default30` cache profile:</span></span>

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a><span data-ttu-id="bb9af-236">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="bb9af-236">Additional resources</span></span>

* [<span data-ttu-id="bb9af-237">応答をキャッシュに格納します。</span><span class="sxs-lookup"><span data-stu-id="bb9af-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="bb9af-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="bb9af-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
