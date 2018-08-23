---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法として許可または ASP.NET Core アプリでのクロス オリジン要求を拒否するための標準 CORS します。
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2018
ms.locfileid: "41828128"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="5ada0-103">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5ada0-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="5ada0-104">作成者 [Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5ada0-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5ada0-105">ブラウザーのセキュリティは、Web ページが別のドメインに AJAX 要求を行うことを防止します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="5ada0-106">この制限は*同一生成元ポリシー*と呼ばれ、悪意のあるサイトが別のサイトから機密データを読み取れないようにします。</span><span class="sxs-lookup"><span data-stu-id="5ada0-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="5ada0-107">しかし、他のサイトがあなたの Web API にクロスオリジン要求を行えるようにする必要がある場合もあります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="5ada0-108">[クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、サーバーに同一生成元ポリシーの制限を緩和させる W3C 標準の１つです。</span><span class="sxs-lookup"><span data-stu-id="5ada0-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="5ada0-109">CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="5ada0-110">CORS は [JSONP](https://wikipedia.org/wiki/JSONP) のようなかつての技術より安全でフレキシブルなものです。</span><span class="sxs-lookup"><span data-stu-id="5ada0-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="5ada0-111">このトピックでは、ASP.NET Core アプリケーションで CORS を有効にする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="5ada0-112">「同一生成元」とは</span><span class="sxs-lookup"><span data-stu-id="5ada0-112">What is "same origin"?</span></span>

<span data-ttu-id="5ada0-113">2 つの URL のスキーム、ホスト、ポートが同じである場合、その URL は同一生成元となります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="5ada0-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="5ada0-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="5ada0-115">次の 2 つの URL は生成元が同じです。</span><span class="sxs-lookup"><span data-stu-id="5ada0-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="5ada0-116">次の URL は、上の URL とは生成元が異なります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="5ada0-117">`http://example.net` - 異なるドメイン </span><span class="sxs-lookup"><span data-stu-id="5ada0-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="5ada0-118">`http://www.example.com/foo.html` - 異なるサブドメイン</span><span class="sxs-lookup"><span data-stu-id="5ada0-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="5ada0-119">`https://example.com/foo.html` - 異なるスキーム</span><span class="sxs-lookup"><span data-stu-id="5ada0-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="5ada0-120">`http://example.com:9000/foo.html` - 異なるポート</span><span class="sxs-lookup"><span data-stu-id="5ada0-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="5ada0-121">Internet Explorer は、生成元を比較するときにポートを考慮しません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="5ada0-122">CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5ada0-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="5ada0-123">アプリケーションに CORS を設定するために `Microsoft.AspNetCore.Cors` パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="5ada0-124">呼び出す[AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)で`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5ada0-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="5ada0-125">ミドルウェアによる CORS の有効化</span><span class="sxs-lookup"><span data-stu-id="5ada0-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="5ada0-126">CORS を有効にするには、要求パイプラインを使用して、CORS ミドルウェアを追加、`UseCors`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="5ada0-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="5ada0-127">CORS ミドルウェアする必要がありますの前に、エンドポイントが定義されて、アプリでのクロス オリジン要求をサポートする (たとえばへの呼び出しの前に`UseMvc`)。</span><span class="sxs-lookup"><span data-stu-id="5ada0-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="5ada0-128">使用して、CORS ミドルウェアを追加するときに、クロス オリジン ポリシーを指定することができます、 [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)クラス。</span><span class="sxs-lookup"><span data-stu-id="5ada0-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="5ada0-129">これには、2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-129">There are two ways to do this.</span></span> <span data-ttu-id="5ada0-130">最初に呼び出す`UseCors`ラムダで。</span><span class="sxs-lookup"><span data-stu-id="5ada0-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="5ada0-131">**注:** URL は末尾にスラッシュを付けずに指定される必要があります (`/`)。</span><span class="sxs-lookup"><span data-stu-id="5ada0-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="5ada0-132">URL が `/`で終了する場合、比較時に`false`が返され、ヘッダーが返されません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="5ada0-133">ラムダは、`CorsPolicyBuilder` オブジェクトをとります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="5ada0-134">[構成オプション](#cors-policy-options)のリストはこのトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="5ada0-135">この例では、ポリシーは `http://example.com` からのクロス オリジン要求を許可し、他の生成元からの要求は許可しません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="5ada0-136">CorsPolicyBuilder では、メソッドの呼び出しをチェーンするように fluent API があります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="5ada0-137">2 つ目の方法は、名前が付いた CORS ポリシーを 1 つまたは複数定義し、実行時に名前によってポリシーを選択することです。</span><span class="sxs-lookup"><span data-stu-id="5ada0-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="5ada0-138">この例では、"AllowSpecificOrigin" という名前の CORS ポリシーを追加します。 </span><span class="sxs-lookup"><span data-stu-id="5ada0-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="5ada0-139">このポリシーを選択するには、`UseCors` にこの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="5ada0-140">MVC で CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5ada0-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="5ada0-141">MVC を使用して、アクション、コント ローラーごと、またはすべてのコント ローラーをグローバルにごとの特定の CORS を適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="5ada0-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="5ada0-142">MVC を使用して CORS を有効にする場合、同じ CORS サービスを使用するが、CORS ミドルウェアはありません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="5ada0-143">アクションごと</span><span class="sxs-lookup"><span data-stu-id="5ada0-143">Per action</span></span>

<span data-ttu-id="5ada0-144">指定する特定のアクションの CORS ポリシーの追加、`[EnableCors]`属性をアクションにします。</span><span class="sxs-lookup"><span data-stu-id="5ada0-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="5ada0-145">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="5ada0-146">コント ローラーごと</span><span class="sxs-lookup"><span data-stu-id="5ada0-146">Per controller</span></span>

<span data-ttu-id="5ada0-147">指定する CORS ポリシーを特定のコント ローラーの追加、`[EnableCors]`属性をコント ローラー クラス。</span><span class="sxs-lookup"><span data-stu-id="5ada0-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="5ada0-148">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="5ada0-149">グローバルに</span><span class="sxs-lookup"><span data-stu-id="5ada0-149">Globally</span></span>

<span data-ttu-id="5ada0-150">できます CORS を有効にグローバルにすべてのコント ローラーを追加することで、`CorsAuthorizationFilterFactory`グローバル フィルターのコレクションをフィルターします。</span><span class="sxs-lookup"><span data-stu-id="5ada0-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="5ada0-151">優先順位は、: アクション、コント ローラー、グローバルです。</span><span class="sxs-lookup"><span data-stu-id="5ada0-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="5ada0-152">アクション レベルのポリシーがコント ローラー レベルのポリシーよりも優先され、コント ローラー レベルのポリシーがグローバル ポリシーより優先します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="5ada0-153">CORS を無効にします。</span><span class="sxs-lookup"><span data-stu-id="5ada0-153">Disable CORS</span></span>

<span data-ttu-id="5ada0-154">コント ローラーまたはアクションの CORS を無効にする、`[DisableCors]`属性。</span><span class="sxs-lookup"><span data-stu-id="5ada0-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="5ada0-155">CORS ポリシー オプション</span><span class="sxs-lookup"><span data-stu-id="5ada0-155">CORS policy options</span></span>

<span data-ttu-id="5ada0-156">このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="5ada0-157">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="5ada0-158">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="5ada0-159">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="5ada0-160">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="5ada0-161">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="5ada0-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="5ada0-162">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="5ada0-163">いくつかのオプションの読み取りをすると役立つ場合があります[CORS ではどのように動作](#how-cors-works)最初。</span><span class="sxs-lookup"><span data-stu-id="5ada0-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="5ada0-164">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-164">Set the allowed origins</span></span>

<span data-ttu-id="5ada0-165">1 つまたは複数の特定のオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="5ada0-166">すべてのオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="5ada0-167">任意のオリジンからの要求を許可する前に慎重に検討してください。</span><span class="sxs-lookup"><span data-stu-id="5ada0-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="5ada0-168">あらゆる web サイトが、API への AJAX 呼び出しを実行できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="5ada0-169">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="5ada0-170">すべての HTTP メソッドを許可します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="5ada0-171">これは、事前要求とアクセスの制御-許可する-メソッド ヘッダーに影響します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="5ada0-172">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-172">Set the allowed request headers</span></span>

<span data-ttu-id="5ada0-173">CORS プレフライト要求に、アプリケーションで設定される HTTP ヘッダーを一覧表示、アクセス制御の要求ヘッダー ヘッダーが含まれます (いわゆる"author 要求ヘッダー")。</span><span class="sxs-lookup"><span data-stu-id="5ada0-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="5ada0-174">特定のヘッダーをホワイト リストに登録します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="5ada0-175">すべてを許可するには、要求ヘッダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="5ada0-176">ブラウザーのアクセス制御の要求ヘッダーを設定する方法で完全に一貫していません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="5ada0-177">以外に何もヘッダーを設定する場合は、"\*"を含める必要がある、少なくとも"accept"、「コンテンツの種類」と"origin"、およびサポートするカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="5ada0-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="5ada0-178">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-178">Set the exposed response headers</span></span>

<span data-ttu-id="5ada0-179">既定では、ブラウザーはすべてのアプリケーションに応答ヘッダーで公開されません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="5ada0-180">(を参照してください[ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header))。既定で使用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5ada0-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="5ada0-181">キャッシュ制御</span><span class="sxs-lookup"><span data-stu-id="5ada0-181">Cache-Control</span></span>

* <span data-ttu-id="5ada0-182">コンテンツの言語</span><span class="sxs-lookup"><span data-stu-id="5ada0-182">Content-Language</span></span>

* <span data-ttu-id="5ada0-183">Content-Type</span><span class="sxs-lookup"><span data-stu-id="5ada0-183">Content-Type</span></span>

* <span data-ttu-id="5ada0-184">有効期限が切れます</span><span class="sxs-lookup"><span data-stu-id="5ada0-184">Expires</span></span>

* <span data-ttu-id="5ada0-185">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="5ada0-185">Last-Modified</span></span>

* <span data-ttu-id="5ada0-186">プラグマ</span><span class="sxs-lookup"><span data-stu-id="5ada0-186">Pragma</span></span>

<span data-ttu-id="5ada0-187">CORS の仕様を呼び出す*単純な応答ヘッダー*します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="5ada0-188">その他のヘッダーをアプリケーションに使用可能には。</span><span class="sxs-lookup"><span data-stu-id="5ada0-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="5ada0-189">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="5ada0-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="5ada0-190">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="5ada0-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="5ada0-191">既定では、ブラウザーは、クロス オリジン要求と共に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="5ada0-192">資格情報には、cookie として HTTP 認証方式がなどがあります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="5ada0-193">クロス オリジン要求に資格情報を送信するには、クライアントは XMLHttpRequest.withCredentials を true に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="5ada0-194">XMLHttpRequest を直接使用するには。</span><span class="sxs-lookup"><span data-stu-id="5ada0-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="5ada0-195">Jquery では。</span><span class="sxs-lookup"><span data-stu-id="5ada0-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="5ada0-196">さらに、サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="5ada0-197">クロス オリジンの資格情報を許可します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="5ada0-198">これで、HTTP 応答には、アクセス コントロール-許可する-資格情報のヘッダー。 クロス オリジン要求の資格情報で、サーバーは、ブラウザーに指示が含まれます。</span><span class="sxs-lookup"><span data-stu-id="5ada0-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="5ada0-199">ブラウザーが資格情報を送信、応答には有効なアクセス制御を許可する-資格情報のヘッダーが含まれていない場合は、ブラウザーは、アプリケーションへの応答を公開しないし、AJAX 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="5ada0-200">クロス オリジンの資格情報を許可する際に注意します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="5ada0-201">別のドメインに web サイトでは、ユーザーの知識がなくても、ユーザーの代わりに、アプリにログインしているユーザーの資格情報を送信できます。</span><span class="sxs-lookup"><span data-stu-id="5ada0-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="5ada0-202">CORS の仕様もその設定を示すオリジンを`"*"`(すべてのオリジン) 有効でない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="5ada0-203">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-203">Set the preflight expiration time</span></span>

<span data-ttu-id="5ada0-204">アクセス制御、Max-age ヘッダーでは、プレフライト要求に応答をキャッシュできる期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="5ada0-205">このヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="5ada0-206">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="5ada0-206">How CORS works</span></span>

<span data-ttu-id="5ada0-207">このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="5ada0-208">CORS ポリシーを正しく構成されているし、予期しない動作が発生したときにデバッグできるようにの CORS のしくみを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="5ada0-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="5ada0-209">CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="5ada0-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="5ada0-210">ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="5ada0-211">カスタム JavaScript コードは、CORS を有効にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="5ada0-212">クロス オリジン要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="5ada0-213">`Origin`ヘッダーは要求を行っているサイトのドメインを提供します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="5ada0-214">サーバーは、要求を許可している場合、応答のアクセス制御の許可-オリジン ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="5ada0-215">このヘッダーの値は、要求から配信元のヘッダーと一致するか、ワイルドカード値は、"\*"、任意のオリジンを許可することを意味します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="5ada0-216">応答には、アクセス制御の許可-オリジン ヘッダーが含まれていない、AJAX 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="5ada0-217">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="5ada0-218">サーバーに正常な応答が返される場合でも、ブラウザーは応答を使用できるように、クライアント アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="5ada0-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="5ada0-219">プレフライト要求</span><span class="sxs-lookup"><span data-stu-id="5ada0-219">Preflight Requests</span></span>

<span data-ttu-id="5ada0-220">一部の CORS 要求では、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="5ada0-221">次の条件に該当する場合、ブラウザーでプレフライト要求をスキップできます。</span><span class="sxs-lookup"><span data-stu-id="5ada0-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="5ada0-222">要求メソッドが GET、HEAD、または POST、および</span><span class="sxs-lookup"><span data-stu-id="5ada0-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="5ada0-223">アプリケーションが承諾、Accept-language、Content-language 以外のすべての要求ヘッダーに設定されていないコンテンツの種類、または最後のイベント ID、および</span><span class="sxs-lookup"><span data-stu-id="5ada0-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="5ada0-224">Content-type ヘッダー (場合設定) は、次の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="5ada0-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="5ada0-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="5ada0-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="5ada0-226">マルチパート/フォーム データ</span><span class="sxs-lookup"><span data-stu-id="5ada0-226">multipart/form-data</span></span>

  * <span data-ttu-id="5ada0-227">テキスト/プレーン</span><span class="sxs-lookup"><span data-stu-id="5ada0-227">text/plain</span></span>

<span data-ttu-id="5ada0-228">要求ヘッダーについて、ルールは、XMLHttpRequest オブジェクトに対して setRequestHeader を呼び出すことで、アプリケーションを設定するヘッダーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="5ada0-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="5ada0-229">(CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。ユーザー エージェント、ホスト、またはコンテンツの長さなど、ブラウザーから設定できるヘッダーには、ルールは適用されません。</span><span class="sxs-lookup"><span data-stu-id="5ada0-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="5ada0-230">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-230">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="5ada0-231">事前要求は HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="5ada0-232">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5ada0-232">It includes two special headers:</span></span>

* <span data-ttu-id="5ada0-233">アクセス制御の要求メソッド: 実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="5ada0-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="5ada0-234">アクセス制御の要求ヘッダー: アプリケーションが、実際の要求で設定できる要求ヘッダーの一覧。</span><span class="sxs-lookup"><span data-stu-id="5ada0-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="5ada0-235">(ここでも、これは含まれません、ブラウザーを設定するヘッダー。)</span><span class="sxs-lookup"><span data-stu-id="5ada0-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="5ada0-236">サーバーが要求を許可すると仮定すると、例応答を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-236">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="5ada0-237">応答には、許可されているメソッドを一覧表示するアクセスの制御-許可する-メソッド ヘッダーと、必要に応じて、アクセスの制御-許可する-ヘッダー ヘッダー、許可されたヘッダーの一覧を表示するが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5ada0-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="5ada0-238">プレフライト要求が成功すると、ブラウザーは、前述のように、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="5ada0-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
