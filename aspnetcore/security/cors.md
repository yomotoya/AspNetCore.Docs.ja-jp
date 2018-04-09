---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法を許可したり、ASP.NET のコア ・ アプリケーションの間の原点の要求を拒否しての標準として CORS。
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: 3c5d0840426c7ed52353a7832a1a1959027121de
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="ef0cc-103">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="ef0cc-104">作成者 [Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ef0cc-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ef0cc-105">ブラウザーのセキュリティは、Web ページが別のドメインに AJAX 要求を行うことを防止します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="ef0cc-106">この制限は*同一生成元ポリシー*と呼ばれ、悪意のあるサイトが別のサイトから機密データを読み取れないようにします。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="ef0cc-107">しかし、他のサイトがあなたの Web API にクロスオリジン要求を行えるようにする必要がある場合もあります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="ef0cc-108">[クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、サーバーに同一生成元ポリシーの制限を緩和させる W3C 標準の１つです。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="ef0cc-109">CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="ef0cc-110">CORS は [JSONP](https://wikipedia.org/wiki/JSONP) のようなかつての技術より安全でフレキシブルなものです。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="ef0cc-111">このトピックでは、ASP.NET Core アプリケーションで CORS を有効にする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="ef0cc-112">「同一生成元」とは</span><span class="sxs-lookup"><span data-stu-id="ef0cc-112">What is "same origin"?</span></span>

<span data-ttu-id="ef0cc-113">2 つの URL のスキーム、ホスト、ポートが同じである場合、その URL は同一生成元となります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="ef0cc-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="ef0cc-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="ef0cc-115">次の 2 つの URL は生成元が同じです。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="ef0cc-116">次の URL は、上の URL とは生成元が異なります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="ef0cc-117">`http://example.net` - 異なるドメイン </span><span class="sxs-lookup"><span data-stu-id="ef0cc-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="ef0cc-118">`http://www.example.com/foo.html` - 異なるサブドメイン</span><span class="sxs-lookup"><span data-stu-id="ef0cc-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="ef0cc-119">`https://example.com/foo.html` - 異なるスキーム</span><span class="sxs-lookup"><span data-stu-id="ef0cc-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="ef0cc-120">`http://example.com:9000/foo.html` - 異なるポート</span><span class="sxs-lookup"><span data-stu-id="ef0cc-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="ef0cc-121">Internet Explorer は、生成元を比較するときにポートを考慮しません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="ef0cc-122">CORS の設定</span><span class="sxs-lookup"><span data-stu-id="ef0cc-122">Setting up CORS</span></span>

<span data-ttu-id="ef0cc-123">アプリケーションに CORS を設定するために `Microsoft.AspNetCore.Cors` パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="ef0cc-124">Startup.cs に CORS サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="ef0cc-125">ミドルウェアによる CORS の有効化</span><span class="sxs-lookup"><span data-stu-id="ef0cc-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="ef0cc-126">アプリケーション全体で CORS を有効にするために`UseCors`拡張メソッドを使用して CORS ミドルウェアを要求パイプラインに追加します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="ef0cc-127">CORS ミドルウェアは アプリで定義されたエンドポイントより先に呼び出される必要があることに注意してください (例: </span><span class="sxs-lookup"><span data-stu-id="ef0cc-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="ef0cc-128">`UseMvc`を呼び出す前)。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="ef0cc-129">CORS ミドルウェアを追加するときに`CorsPolicyBuilder`クラスを使用してクロス オリジン ポリシーを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="ef0cc-130">これには、2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-130">There are two ways to do this.</span></span> <span data-ttu-id="ef0cc-131">1 つは、ラムダで UseCors を呼び出すことです:</span><span class="sxs-lookup"><span data-stu-id="ef0cc-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="ef0cc-132">**注:**URL は末尾にスラッシュを付けずに指定される必要があります (`/`)。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="ef0cc-133">URL が `/`で終了する場合、比較時に`false`が返され、ヘッダーが返されません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="ef0cc-134">ラムダは、`CorsPolicyBuilder` オブジェクトをとります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="ef0cc-135">[構成オプション](#cors-policy-options)のリストはこのトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="ef0cc-136">この例では、ポリシーは `http://example.com` からのクロス オリジン要求を許可し、他の生成元からの要求は許可しません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="ef0cc-137">fluent API をもつ CorsPolicyBuilder では、メソッドの呼び出しを連結することができることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="ef0cc-138">2 つ目の方法は、名前が付いた CORS ポリシーを 1 つまたは複数定義し、実行時に名前によってポリシーを選択することです。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="ef0cc-139">この例では、"AllowSpecificOrigin" という名前の CORS ポリシーを追加します。 </span><span class="sxs-lookup"><span data-stu-id="ef0cc-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="ef0cc-140">このポリシーを選択するには、`UseCors` にこの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="ef0cc-141">MVC での CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="ef0cc-142">MVC は、アクション、コント ローラーごとまたはグローバルにすべてのコント ローラーごとの特定の CORS を適用するのに代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="ef0cc-143">MVC を使用して、CORS を有効にする場合、同じ CORS サービスを使用するが、CORS ミドルウェアではありません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="ef0cc-144">アクションごと</span><span class="sxs-lookup"><span data-stu-id="ef0cc-144">Per action</span></span>

<span data-ttu-id="ef0cc-145">特定のアクションの CORS ポリシーの追加を指定する、`[EnableCors]`属性をアクションにします。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="ef0cc-146">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-146">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="ef0cc-147">コント ローラーあたり</span><span class="sxs-lookup"><span data-stu-id="ef0cc-147">Per controller</span></span>

<span data-ttu-id="ef0cc-148">特定のコント ローラーの CORS ポリシーの追加を指定する、`[EnableCors]`属性をコント ローラー クラスにします。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="ef0cc-149">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-149">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="ef0cc-150">グローバル</span><span class="sxs-lookup"><span data-stu-id="ef0cc-150">Globally</span></span>

<span data-ttu-id="ef0cc-151">有効にする CORS グローバルにすべてのコント ローラーの追加、`CorsAuthorizationFilterFactory`グローバル フィルターのコレクションをフィルターします。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="ef0cc-152">優先順位: アクション、コント ローラーで、グローバルです。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="ef0cc-153">アクション レベル ポリシー コント ローラー レベルのポリシーより優先し、コント ローラー レベルのポリシーのグローバル ポリシーよりも優先します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="ef0cc-154">CORS を無効にします。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-154">Disable CORS</span></span>

<span data-ttu-id="ef0cc-155">コント ローラーまたはアクションの CORS を無効にする、`[DisableCors]`属性。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="ef0cc-156">CORS ポリシーのオプション</span><span class="sxs-lookup"><span data-stu-id="ef0cc-156">CORS policy options</span></span>

<span data-ttu-id="ef0cc-157">このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="ef0cc-158">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="ef0cc-159">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="ef0cc-160">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="ef0cc-161">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="ef0cc-162">クロス オリジン要求に資格情報</span><span class="sxs-lookup"><span data-stu-id="ef0cc-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="ef0cc-163">プレフライト有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="ef0cc-164">いくつかのオプションの読み取りに役立つ場合があります[方法 CORS 機能](#how-cors-works)最初。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="ef0cc-165">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-165">Set the allowed origins</span></span>

<span data-ttu-id="ef0cc-166">1 つまたは複数の特定のオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-166">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="ef0cc-167">すべてのオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-167">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="ef0cc-168">すべてのオリジンからの要求を許可する前に慎重に検討してください。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="ef0cc-169">これは、事実上あらゆる web サイトが、API への AJAX 呼び出しを実行できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="ef0cc-170">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="ef0cc-171">すべての HTTP メソッドを使用するには。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-171">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="ef0cc-172">これは、事前要求とアクセスの制御の許可する-メソッド ヘッダーに影響します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="ef0cc-173">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-173">Set the allowed request headers</span></span>

<span data-ttu-id="ef0cc-174">CORS プレフライト要求がアプリケーションによって設定される HTTP ヘッダーの一覧を表示する、アクセス コントロール-要求ヘッダー ヘッダーが含ま可能性があります (いわゆる"要求ヘッダーを author") です。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="ef0cc-175">特定のヘッダー。 ホワイト リストを</span><span class="sxs-lookup"><span data-stu-id="ef0cc-175">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="ef0cc-176">許可するには、すべての著者要求ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-176">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="ef0cc-177">ブラウザーがアクセス コントロール-要求ヘッダーを設定する方法の完全一致しません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="ef0cc-178">以外の何もするヘッダーを設定する場合は"\*"、する必要がありますを含めるには、少なくとも「受け入れる」、「コンテンツの種類」と「発生元」、およびサポートする任意のカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="ef0cc-179">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-179">Set the exposed response headers</span></span>

<span data-ttu-id="ef0cc-180">既定では、ブラウザーはすべてのアプリケーションに応答ヘッダーを公開しません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-180">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="ef0cc-181">(を参照してください[ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header))。既定で利用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="ef0cc-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="ef0cc-182">Cache-Control</span></span>

* <span data-ttu-id="ef0cc-183">コンテンツの言語</span><span class="sxs-lookup"><span data-stu-id="ef0cc-183">Content-Language</span></span>

* <span data-ttu-id="ef0cc-184">Content-Type</span><span class="sxs-lookup"><span data-stu-id="ef0cc-184">Content-Type</span></span>

* <span data-ttu-id="ef0cc-185">有効期限が切れる</span><span class="sxs-lookup"><span data-stu-id="ef0cc-185">Expires</span></span>

* <span data-ttu-id="ef0cc-186">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="ef0cc-186">Last-Modified</span></span>

* <span data-ttu-id="ef0cc-187">プラグマ</span><span class="sxs-lookup"><span data-stu-id="ef0cc-187">Pragma</span></span>

<span data-ttu-id="ef0cc-188">CORS の仕様を呼び出す*単純な応答ヘッダー*です。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="ef0cc-189">その他のヘッダー使用可能にするアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-189">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="ef0cc-190">クロス オリジン要求に資格情報</span><span class="sxs-lookup"><span data-stu-id="ef0cc-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="ef0cc-191">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="ef0cc-192">既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-192">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="ef0cc-193">Cookie と、HTTP 認証スキームの資格情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="ef0cc-194">クロス オリジン要求に資格情報を送信するには、クライアントは XMLHttpRequest.withCredentials を true に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="ef0cc-195">XMLHttpRequest を直接使用するには。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="ef0cc-196">JQuery: で</span><span class="sxs-lookup"><span data-stu-id="ef0cc-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="ef0cc-197">さらに、サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="ef0cc-198">クロス オリジンの資格情報を使用するには。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-198">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="ef0cc-199">これで、HTTP 応答では、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示する、アクセス コントロール-を許可する-資格情報ヘッダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="ef0cc-200">ブラウザーが資格情報を送信、応答には有効なアクセス制御を許可する-資格情報ヘッダーが含まれていない場合は、ブラウザーは、アプリケーションへの応答を公開しないし、AJAX 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-200">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="ef0cc-201">クロス オリジンの資格情報を許可する場合に注意します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-201">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="ef0cc-202">別のドメインで web サイトは、ユーザーの知らない間にユーザーの代理でアプリにログインしているユーザーの資格情報を送信できます。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-202">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="ef0cc-203">CORS の仕様もその設定を規定する配信元"\*"(すべてのオリジン) が有効ではない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-203">The CORS specification also states that setting origins to "\*" (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="ef0cc-204">プレフライト有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-204">Set the preflight expiration time</span></span>

<span data-ttu-id="ef0cc-205">アクセス コントロール-Max-age ヘッダーでは、プレフライト要求に応答をキャッシュできる期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="ef0cc-206">このヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-206">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="ef0cc-207">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="ef0cc-207">How CORS works</span></span>

<span data-ttu-id="ef0cc-208">このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-208">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="ef0cc-209">予期しない動作が発生したときに CORS ポリシーを正しく構成できるようにする CORS のしくみと troubleshooted を理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-209">It's important to understand how CORS works so that the CORS policy can be configured correctly and troubleshooted when unexpected behaviors occur.</span></span>

<span data-ttu-id="ef0cc-210">CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="ef0cc-211">ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="ef0cc-212">カスタムの JavaScript コードは、CORS を有効にするため必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-212">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="ef0cc-213">クロス オリジン要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-213">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="ef0cc-214">`Origin`ヘッダーは要求を行っているサイトのドメインを提供します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-214">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="ef0cc-215">サーバーは、要求を許可している場合は、応答でアクセス制御の許可する-オリジン ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-215">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="ef0cc-216">このヘッダーの値は、要求の Origin ヘッダーと一致するか、ワイルドカード文字は、"\*"、すべてのオリジンを許可されていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-216">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="ef0cc-217">応答には、アクセス コントロール-を許可する-オリジン ヘッダーが含まれていない、AJAX 要求が失敗します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-217">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="ef0cc-218">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-218">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="ef0cc-219">サーバーでは、正常な応答を返す、場合でも、ブラウザーしない応答を使用できるように、クライアント アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-219">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="ef0cc-220">プレフライト要求</span><span class="sxs-lookup"><span data-stu-id="ef0cc-220">Preflight Requests</span></span>

<span data-ttu-id="ef0cc-221">いくつかの CORS 要求については、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる、追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-221">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="ef0cc-222">ブラウザーは、次の条件に該当する場合、プレフライト要求を省略できます。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-222">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="ef0cc-223">要求メソッドが GET、HEAD、または POST、および</span><span class="sxs-lookup"><span data-stu-id="ef0cc-223">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="ef0cc-224">アプリケーションが、要求のヘッダーを受け入れる、Accept-language、Content-language 以外に設定されていないコンテンツの種類、または最後のイベント ID、および</span><span class="sxs-lookup"><span data-stu-id="ef0cc-224">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="ef0cc-225">Content-type ヘッダー (場合に設定) は、次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-225">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="ef0cc-226">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="ef0cc-226">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="ef0cc-227">マルチパート フォーム データ</span><span class="sxs-lookup"><span data-stu-id="ef0cc-227">multipart/form-data</span></span>

  * <span data-ttu-id="ef0cc-228">text/plain</span><span class="sxs-lookup"><span data-stu-id="ef0cc-228">text/plain</span></span>

<span data-ttu-id="ef0cc-229">アプリケーションで、XMLHttpRequest オブジェクトで setRequestHeader を呼び出すことによって設定されたヘッダーを要求ヘッダーについて規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-229">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="ef0cc-230">(CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。ルールは、ユーザー エージェント、ホスト、またはコンテンツの長さなど、ブラウザーを設定できますヘッダーに適用されません。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-230">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="ef0cc-231">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-231">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="ef0cc-232">事前要求は、HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-232">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="ef0cc-233">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-233">It includes two special headers:</span></span>

* <span data-ttu-id="ef0cc-234">アクセス コントロール-要求メソッド: 実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-234">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="ef0cc-235">アクセス コントロール-要求ヘッダー。 アプリケーションが、実際の要求で設定できる要求ヘッダーの一覧。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-235">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="ef0cc-236">(ここでも、これが含まれていないブラウザーを設定するヘッダーには。)</span><span class="sxs-lookup"><span data-stu-id="ef0cc-236">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="ef0cc-237">次に、応答の例、サーバーで要求を許可すると仮定した場合を示します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-237">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="ef0cc-238">応答には、許可されているメソッドを一覧表示するアクセスの制御の許可する-メソッド ヘッダーおよび必要に応じて、アクセス コントロール-を許可する-ヘッダー ヘッダー、許可されているヘッダーの一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-238">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="ef0cc-239">プレフライト要求が成功した場合、ブラウザーは、前述のとおり、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="ef0cc-239">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
