---
title: "クロス オリジン要求 (CORS) を有効にします。"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.assetid: f9d95e88-4d7e-4d0c-a8e1-47de1128d505
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: e441ce1c50139a5b33865eec8e8d99764258730d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="64d38-103">クロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="64d38-103">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="64d38-104">によって[Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="64d38-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="64d38-105">ブラウザーのセキュリティは、web ページが別のドメインに AJAX 要求を行うことを防止します。</span><span class="sxs-lookup"><span data-stu-id="64d38-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="64d38-106">この制限が呼び出された、*同一生成元ポリシー*、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="64d38-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="64d38-107">ただし、場合もあります可能性がある、web API へのクロス オリジン要求を行う他のサイトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="64d38-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="64d38-108">[クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、W3C 標準により、同じオリジンのポリシーを緩和するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="64d38-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="64d38-109">CORS を使用して、サーバー明示的に許可できますいくつかのクロス オリジン要求中に、他のユーザーを拒否します。</span><span class="sxs-lookup"><span data-stu-id="64d38-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="64d38-110">CORS などがより安全なと以前の手法より柔軟な[JSONP](https://wikipedia.org/wiki/JSONP)です。</span><span class="sxs-lookup"><span data-stu-id="64d38-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="64d38-111">このトピックでは、ASP.NET Core アプリケーションで CORS を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="64d38-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="64d38-112">「同じ発生元」とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="64d38-112">What is "same origin"?</span></span>

<span data-ttu-id="64d38-113">2 つの Url では、同じスキーム、ホスト、およびポートがある場合の原点が同じがあります。</span><span class="sxs-lookup"><span data-stu-id="64d38-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="64d38-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="64d38-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="64d38-115">これら 2 つの Url は、原点が同じであります。</span><span class="sxs-lookup"><span data-stu-id="64d38-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="64d38-116">これらの Url は、2 つよりも前の別の原点をあります。</span><span class="sxs-lookup"><span data-stu-id="64d38-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="64d38-117">`http://example.net`-別のドメイン</span><span class="sxs-lookup"><span data-stu-id="64d38-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="64d38-118">`http://www.example.com/foo.html`-別のサブドメイン</span><span class="sxs-lookup"><span data-stu-id="64d38-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="64d38-119">`https://example.com/foo.html`-別のスキーム</span><span class="sxs-lookup"><span data-stu-id="64d38-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="64d38-120">`http://example.com:9000/foo.html`-別のポート</span><span class="sxs-lookup"><span data-stu-id="64d38-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="64d38-121">Internet Explorer では、元のドメインを比較するときに、ポートは考慮されません。</span><span class="sxs-lookup"><span data-stu-id="64d38-121">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="64d38-122">CORS の設定</span><span class="sxs-lookup"><span data-stu-id="64d38-122">Setting up CORS</span></span>

<span data-ttu-id="64d38-123">設定するアプリケーションの CORS の追加、`Microsoft.AspNetCore.Cors`をプロジェクトにパッケージします。</span><span class="sxs-lookup"><span data-stu-id="64d38-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="64d38-124">Startup.cs の CORS サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="64d38-124">Add the CORS services in Startup.cs:</span></span>

<span data-ttu-id="64d38-125">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]</span><span class="sxs-lookup"><span data-stu-id="64d38-125">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]</span></span>

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="64d38-126">ミドルウェアで CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="64d38-126">Enabling CORS with middleware</span></span>

<span data-ttu-id="64d38-127">有効にする、アプリケーション全体の CORS では、要求パイプラインを使用して、CORS ミドルウェアを追加、`UseCors`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="64d38-127">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="64d38-128">CORS ミドルウェアが (ex クロス オリジン要求をサポートするアプリで定義されたエンドポイントを付ける必要がありますに注意してください。。</span><span class="sxs-lookup"><span data-stu-id="64d38-128">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="64d38-129">呼び出しの前に`UseMvc`)。</span><span class="sxs-lookup"><span data-stu-id="64d38-129">before any call to `UseMvc`).</span></span>

<span data-ttu-id="64d38-130">クロス オリジン ポリシーを指定するには、CORS ミドルウェアを使用して、追加するときに、`CorsPolicyBuilder`クラスです。</span><span class="sxs-lookup"><span data-stu-id="64d38-130">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="64d38-131">これには、2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="64d38-131">There are two ways to do this.</span></span> <span data-ttu-id="64d38-132">1 つは、ラムダで UseCors を呼び出すには。</span><span class="sxs-lookup"><span data-stu-id="64d38-132">The first is to call UseCors with a lambda:</span></span>

<span data-ttu-id="64d38-133">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]</span><span class="sxs-lookup"><span data-stu-id="64d38-133">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]</span></span>

<span data-ttu-id="64d38-134">**注:**末尾のスラッシュせず、URL を指定する必要があります (`/`)。</span><span class="sxs-lookup"><span data-stu-id="64d38-134">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="64d38-135">URL が終了した場合は`/`、比較が返されます`false`され、ヘッダーは返されません。</span><span class="sxs-lookup"><span data-stu-id="64d38-135">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="64d38-136">ラムダは、`CorsPolicyBuilder`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="64d38-136">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="64d38-137">リストができたら、[構成オプション](#cors-policy-options)このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="64d38-137">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="64d38-138">この例では、ポリシーによりからのクロス オリジン要求`http://example.com`しない他のオリジンです。</span><span class="sxs-lookup"><span data-stu-id="64d38-138">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="64d38-139">CorsPolicyBuilder いる fluent API では、メソッド呼び出しを連結することができますので注意してください。</span><span class="sxs-lookup"><span data-stu-id="64d38-139">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

<span data-ttu-id="64d38-140">[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]</span><span class="sxs-lookup"><span data-stu-id="64d38-140">[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]</span></span>

<span data-ttu-id="64d38-141">2 番目の方法を 1 つまたは複数名前付き CORS ポリシーを定義し、名前によって実行時にポリシーを選択します。</span><span class="sxs-lookup"><span data-stu-id="64d38-141">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

<span data-ttu-id="64d38-142">[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]</span><span class="sxs-lookup"><span data-stu-id="64d38-142">[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]</span></span>

<span data-ttu-id="64d38-143">この例では、"AllowSpecificOrigin"をという名前の CORS ポリシーを追加します。</span><span class="sxs-lookup"><span data-stu-id="64d38-143">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="64d38-144">ポリシーを選択するには、名前を渡す`UseCors`です。</span><span class="sxs-lookup"><span data-stu-id="64d38-144">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="64d38-145">MVC での CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="64d38-145">Enabling CORS in MVC</span></span>

<span data-ttu-id="64d38-146">MVC は、アクション、コント ローラーごとまたはグローバルにすべてのコント ローラーごとの特定の CORS を適用するのに代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="64d38-146">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="64d38-147">MVC を使用して、CORS を有効にする場合、同じ CORS サービスを使用するが、CORS ミドルウェアがありません。</span><span class="sxs-lookup"><span data-stu-id="64d38-147">When using MVC to enable CORS the same CORS services are used, but the CORS middleware is not.</span></span>

### <a name="per-action"></a><span data-ttu-id="64d38-148">アクションごと</span><span class="sxs-lookup"><span data-stu-id="64d38-148">Per action</span></span>

<span data-ttu-id="64d38-149">特定のアクションの CORS ポリシーの追加を指定する、`[EnableCors]`属性をアクションにします。</span><span class="sxs-lookup"><span data-stu-id="64d38-149">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="64d38-150">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-150">Specify the policy name.</span></span>

<span data-ttu-id="64d38-151">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]</span><span class="sxs-lookup"><span data-stu-id="64d38-151">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]</span></span>

### <a name="per-controller"></a><span data-ttu-id="64d38-152">コント ローラーあたり</span><span class="sxs-lookup"><span data-stu-id="64d38-152">Per controller</span></span>

<span data-ttu-id="64d38-153">特定のコント ローラーの CORS ポリシーの追加を指定する、`[EnableCors]`属性をコント ローラー クラスにします。</span><span class="sxs-lookup"><span data-stu-id="64d38-153">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="64d38-154">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-154">Specify the policy name.</span></span>

<span data-ttu-id="64d38-155">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]</span><span class="sxs-lookup"><span data-stu-id="64d38-155">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]</span></span>

### <a name="globally"></a><span data-ttu-id="64d38-156">グローバル</span><span class="sxs-lookup"><span data-stu-id="64d38-156">Globally</span></span>

<span data-ttu-id="64d38-157">有効にする CORS グローバルにすべてのコント ローラーの追加、`CorsAuthorizationFilterFactory`グローバル フィルターのコレクションをフィルターします。</span><span class="sxs-lookup"><span data-stu-id="64d38-157">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

<span data-ttu-id="64d38-158">[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]</span><span class="sxs-lookup"><span data-stu-id="64d38-158">[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]</span></span>

<span data-ttu-id="64d38-159">優先順位: アクション、コント ローラーで、グローバルです。</span><span class="sxs-lookup"><span data-stu-id="64d38-159">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="64d38-160">アクション レベル ポリシー コント ローラー レベルのポリシーより優先し、コント ローラー レベルのポリシーのグローバル ポリシーよりも優先します。</span><span class="sxs-lookup"><span data-stu-id="64d38-160">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="64d38-161">CORS を無効にします。</span><span class="sxs-lookup"><span data-stu-id="64d38-161">Disable CORS</span></span>

<span data-ttu-id="64d38-162">コント ローラーまたはアクションの CORS を無効にする、`[DisableCors]`属性。</span><span class="sxs-lookup"><span data-stu-id="64d38-162">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

<span data-ttu-id="64d38-163">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]</span><span class="sxs-lookup"><span data-stu-id="64d38-163">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]</span></span>

## <a name="cors-policy-options"></a><span data-ttu-id="64d38-164">CORS ポリシーのオプション</span><span class="sxs-lookup"><span data-stu-id="64d38-164">CORS policy options</span></span>

<span data-ttu-id="64d38-165">このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="64d38-165">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="64d38-166">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-166">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="64d38-167">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="64d38-168">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="64d38-169">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="64d38-170">クロス オリジン要求に資格情報</span><span class="sxs-lookup"><span data-stu-id="64d38-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="64d38-171">プレフライト有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="64d38-172">いくつかのオプションの読み取りに役立つ場合があります[方法 CORS 機能](#how-cors-works)最初。</span><span class="sxs-lookup"><span data-stu-id="64d38-172">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="64d38-173">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-173">Set the allowed origins</span></span>

<span data-ttu-id="64d38-174">1 つまたは複数の特定のオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="64d38-174">To allow one or more specific origins:</span></span>

<span data-ttu-id="64d38-175">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]</span><span class="sxs-lookup"><span data-stu-id="64d38-175">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]</span></span>

<span data-ttu-id="64d38-176">すべてのオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="64d38-176">To allow all origins:</span></span>

<span data-ttu-id="64d38-177">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]</span><span class="sxs-lookup"><span data-stu-id="64d38-177">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]</span></span>

<span data-ttu-id="64d38-178">すべてのオリジンからの要求を許可する前に慎重に検討してください。</span><span class="sxs-lookup"><span data-stu-id="64d38-178">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="64d38-179">これは、事実上あらゆる web サイトが、API への AJAX 呼び出しを実行できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="64d38-179">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="64d38-180">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-180">Set the allowed HTTP methods</span></span>

<span data-ttu-id="64d38-181">すべての HTTP メソッドを使用するには。</span><span class="sxs-lookup"><span data-stu-id="64d38-181">To allow all HTTP methods:</span></span>

<span data-ttu-id="64d38-182">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]</span><span class="sxs-lookup"><span data-stu-id="64d38-182">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]</span></span>

<span data-ttu-id="64d38-183">これは、事前要求とアクセスの制御の許可する-メソッド ヘッダーに影響します。</span><span class="sxs-lookup"><span data-stu-id="64d38-183">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="64d38-184">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-184">Set the allowed request headers</span></span>

<span data-ttu-id="64d38-185">CORS プレフライト要求がアプリケーションによって設定される HTTP ヘッダーの一覧を表示する、アクセス コントロール-要求ヘッダー ヘッダーが含ま可能性があります (いわゆる"要求ヘッダーを author") です。</span><span class="sxs-lookup"><span data-stu-id="64d38-185">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="64d38-186">特定のヘッダー。 ホワイト リストを</span><span class="sxs-lookup"><span data-stu-id="64d38-186">To whitelist specific headers:</span></span>

<span data-ttu-id="64d38-187">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]</span><span class="sxs-lookup"><span data-stu-id="64d38-187">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]</span></span>

<span data-ttu-id="64d38-188">許可するには、すべての著者要求ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="64d38-188">To allow all author request headers:</span></span>

<span data-ttu-id="64d38-189">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]</span><span class="sxs-lookup"><span data-stu-id="64d38-189">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]</span></span>

<span data-ttu-id="64d38-190">ブラウザーがアクセス コントロール-要求ヘッダーを設定する方法の完全一致しません。</span><span class="sxs-lookup"><span data-stu-id="64d38-190">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="64d38-191">以外の何もするヘッダーを設定する場合は"*"、する必要がありますを含めるには、少なくとも「受け入れる」、「コンテンツの種類」と「発生元」、およびサポートする任意のカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="64d38-191">If you set headers to anything other than "*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="64d38-192">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-192">Set the exposed response headers</span></span>

<span data-ttu-id="64d38-193">既定では、ブラウザーは公開しませんすべてのアプリケーションに応答ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="64d38-193">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="64d38-194">(を参照してください[http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header))。既定で利用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="64d38-194">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="64d38-195">キャッシュ制御</span><span class="sxs-lookup"><span data-stu-id="64d38-195">Cache-Control</span></span>

* <span data-ttu-id="64d38-196">コンテンツの言語</span><span class="sxs-lookup"><span data-stu-id="64d38-196">Content-Language</span></span>

* <span data-ttu-id="64d38-197">コンテンツの種類</span><span class="sxs-lookup"><span data-stu-id="64d38-197">Content-Type</span></span>

* <span data-ttu-id="64d38-198">有効期限が切れる</span><span class="sxs-lookup"><span data-stu-id="64d38-198">Expires</span></span>

* <span data-ttu-id="64d38-199">最終更新</span><span class="sxs-lookup"><span data-stu-id="64d38-199">Last-Modified</span></span>

* <span data-ttu-id="64d38-200">プラグマ</span><span class="sxs-lookup"><span data-stu-id="64d38-200">Pragma</span></span>

<span data-ttu-id="64d38-201">CORS の仕様を呼び出す*単純な応答ヘッダー*です。</span><span class="sxs-lookup"><span data-stu-id="64d38-201">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="64d38-202">その他のヘッダー使用可能にするアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="64d38-202">To make other headers available to the application:</span></span>

<span data-ttu-id="64d38-203">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]</span><span class="sxs-lookup"><span data-stu-id="64d38-203">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]</span></span>

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="64d38-204">クロス オリジン要求に資格情報</span><span class="sxs-lookup"><span data-stu-id="64d38-204">Credentials in cross-origin requests</span></span>

<span data-ttu-id="64d38-205">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="64d38-205">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="64d38-206">既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="64d38-206">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="64d38-207">Cookie と、HTTP 認証スキームの資格情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="64d38-207">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="64d38-208">クロス オリジン要求に資格情報を送信するには、クライアントは XMLHttpRequest.withCredentials を true に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="64d38-208">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="64d38-209">XMLHttpRequest を直接使用するには。</span><span class="sxs-lookup"><span data-stu-id="64d38-209">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="64d38-210">JQuery: で</span><span class="sxs-lookup"><span data-stu-id="64d38-210">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="64d38-211">さらに、サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="64d38-211">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="64d38-212">クロス オリジンの資格情報を使用するには。</span><span class="sxs-lookup"><span data-stu-id="64d38-212">To allow cross-origin credentials:</span></span>

<span data-ttu-id="64d38-213">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]</span><span class="sxs-lookup"><span data-stu-id="64d38-213">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]</span></span>

<span data-ttu-id="64d38-214">これで、HTTP 応答では、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示する、アクセス コントロール-を許可する-資格情報ヘッダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="64d38-214">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="64d38-215">ブラウザーが資格情報を送信しますが、応答は有効なアクセス制御を許可する-資格情報ヘッダーを含まない、ブラウザーは、アプリケーションへの応答を公開しないと、AJAX 要求が失敗します。</span><span class="sxs-lookup"><span data-stu-id="64d38-215">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="64d38-216">十分に注意クロス オリジンの資格情報を許可するため、別のドメインで web サイトはユーザーに気付かれることがなく、ユーザーの代理でアプリにログインしているユーザーの資格情報を送信することができます。</span><span class="sxs-lookup"><span data-stu-id="64d38-216">Be very careful about allowing cross-origin credentials, because it means a website at another domain can send a logged-in user’s credentials to your app on the user’s behalf, without the user being aware.</span></span> <span data-ttu-id="64d38-217">CORS 仕様も状態には、その設定オリジン"*"(すべてのオリジン) は、アクセス コントロール-を許可する-資格情報のヘッダーが存在する場合は無効です。</span><span class="sxs-lookup"><span data-stu-id="64d38-217">The CORS spec also states that setting origins to "*" (all origins) is invalid if the Access-Control-Allow-Credentials header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="64d38-218">プレフライト有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-218">Set the preflight expiration time</span></span>

<span data-ttu-id="64d38-219">アクセス コントロール-Max-age ヘッダーでは、プレフライト要求に応答をキャッシュできる期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-219">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="64d38-220">このヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-220">To set this header:</span></span>

<span data-ttu-id="64d38-221">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]</span><span class="sxs-lookup"><span data-stu-id="64d38-221">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]</span></span>

<a name=cors-how-cors-works></a>

## <a name="how-cors-works"></a><span data-ttu-id="64d38-222">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="64d38-222">How CORS works</span></span>

<span data-ttu-id="64d38-223">このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="64d38-223">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="64d38-224">ことが重要について理解して CORS、CORS ポリシーを正しく構成され、期待どおりに機能しない場合のトラブルシューティングを行うようにします。</span><span class="sxs-lookup"><span data-stu-id="64d38-224">It’s important to understand how CORS works, so that you can configure your CORS policy correctly, and troubleshoot if things don’t work as you expect.</span></span>

<span data-ttu-id="64d38-225">CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="64d38-225">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="64d38-226">ブラウザーでは、CORS をサポートする場合、クロス オリジン要求を自動的にこれらのヘッダーを設定します。JavaScript コードで特別な何もする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="64d38-226">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don’t need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="64d38-227">クロス オリジン要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="64d38-227">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="64d38-228">"Origin"ヘッダーには、要求を行っているサイトのドメインが与えられます。</span><span class="sxs-lookup"><span data-stu-id="64d38-228">The "Origin" header gives the domain of the site that is making the request:</span></span>

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

<span data-ttu-id="64d38-229">サーバーは、要求を許可している場合は、アクセス コントロール-を許可する-オリジン ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="64d38-229">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="64d38-230">このヘッダーの値は、Origin ヘッダーと一致するか、ワイルドカード文字は、"*"、すべてのオリジンを許可されていることを意味します。。</span><span class="sxs-lookup"><span data-stu-id="64d38-230">The value of this header either matches the Origin header, or is the wildcard value "*", meaning that any origin is allowed.:</span></span>

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

<span data-ttu-id="64d38-231">応答に、アクセス コントロール-を許可する-オリジン ヘッダーが含まれていない場合、AJAX 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="64d38-231">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="64d38-232">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="64d38-232">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="64d38-233">サーバーでは、正常な応答を返す、場合でも、ブラウザーは行いません応答クライアント アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="64d38-233">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="64d38-234">プレフライト要求</span><span class="sxs-lookup"><span data-stu-id="64d38-234">Preflight Requests</span></span>

<span data-ttu-id="64d38-235">いくつかの CORS 要求については、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる、追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="64d38-235">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="64d38-236">ブラウザーは、次の条件に該当する場合、プレフライト要求を省略できます。</span><span class="sxs-lookup"><span data-stu-id="64d38-236">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="64d38-237">要求メソッドが GET、HEAD、または POST、および</span><span class="sxs-lookup"><span data-stu-id="64d38-237">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="64d38-238">アプリケーションは Accept、Accept-language、Content-language 以外の任意の要求ヘッダーを設定していないコンテンツの種類、または最後のイベント ID、および</span><span class="sxs-lookup"><span data-stu-id="64d38-238">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="64d38-239">Content-type ヘッダー (場合に設定) は、次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="64d38-239">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="64d38-240">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="64d38-240">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="64d38-241">マルチパート フォーム データ</span><span class="sxs-lookup"><span data-stu-id="64d38-241">multipart/form-data</span></span>

  * <span data-ttu-id="64d38-242">テキスト/プレーン</span><span class="sxs-lookup"><span data-stu-id="64d38-242">text/plain</span></span>

<span data-ttu-id="64d38-243">アプリケーションで、XMLHttpRequest オブジェクトで setRequestHeader を呼び出すことによって設定されたヘッダーを要求ヘッダーについて規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="64d38-243">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="64d38-244">(CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。ルールは、ユーザー エージェント、ホスト、またはコンテンツの長さなど、ブラウザーを設定できますヘッダーには適用されません。</span><span class="sxs-lookup"><span data-stu-id="64d38-244">(The CORS specification calls these "author request headers".) The rule does not apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="64d38-245">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="64d38-245">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="64d38-246">事前要求は、HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="64d38-246">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="64d38-247">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="64d38-247">It includes two special headers:</span></span>

* <span data-ttu-id="64d38-248">アクセス コントロール-要求メソッド: 実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="64d38-248">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="64d38-249">アクセス コントロール-要求ヘッダー。 アプリケーションが、実際の要求で設定できる要求ヘッダーの一覧。</span><span class="sxs-lookup"><span data-stu-id="64d38-249">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="64d38-250">(ここでも、これは含まれません、ブラウザーを設定するヘッダー。)</span><span class="sxs-lookup"><span data-stu-id="64d38-250">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="64d38-251">次に、応答の例、サーバーで要求を許可すると仮定した場合を示します。</span><span class="sxs-lookup"><span data-stu-id="64d38-251">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="64d38-252">応答には、許可されているメソッドを一覧表示するアクセスの制御の許可する-メソッド ヘッダーおよび必要に応じて、アクセス コントロール-を許可する-ヘッダー ヘッダー、許可されているヘッダーの一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="64d38-252">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="64d38-253">プレフライト要求が成功した場合、ブラウザーは、前述のとおり、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="64d38-253">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
