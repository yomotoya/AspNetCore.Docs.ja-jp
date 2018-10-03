---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法として許可または ASP.NET Core アプリでのクロス オリジン要求を拒否するための標準 CORS します。
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045589"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="15cf3-103">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="15cf3-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="15cf3-104">作成者 [Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="15cf3-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="15cf3-105">ブラウザーのセキュリティは、web ページが web ページを提供するものとは異なるドメインに要求を行うことを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="15cf3-106">この制限と呼ばれる、*同一オリジン ポリシー*します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="15cf3-107">同一オリジン ポリシーは、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="15cf3-108">場合によっては、他のサイトでは、クロス オリジン要求を行うアプリに許可する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="15cf3-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="15cf3-109">[クロス オリジン リソース共有](https://www.w3.org/TR/cors/)(CORS) は、サーバーに同一生成元ポリシーの制限を緩和させる W3C 標準の１つです。</span><span class="sxs-lookup"><span data-stu-id="15cf3-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="15cf3-110">CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。</span><span class="sxs-lookup"><span data-stu-id="15cf3-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="15cf3-111">CORS は安全性と以前の手法よりも柔軟性など[JSONP](https://wikipedia.org/wiki/JSONP)します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="15cf3-112">このトピックでは、ASP.NET Core アプリで CORS を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="15cf3-113">同じ生成元</span><span class="sxs-lookup"><span data-stu-id="15cf3-113">Same origin</span></span>

<span data-ttu-id="15cf3-114">同じスキーム、ホスト、およびポートがある場合、2 つの Url が同じ配信元がある ([RFC 6454](https://tools.ietf.org/html/rfc6454))。</span><span class="sxs-lookup"><span data-stu-id="15cf3-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="15cf3-115">次の 2 つの URL は生成元が同じです。</span><span class="sxs-lookup"><span data-stu-id="15cf3-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="15cf3-116">これらの Url があるさまざまなオリジンより前の 2 つの Url:</span><span class="sxs-lookup"><span data-stu-id="15cf3-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="15cf3-117">`https://example.net` &ndash; 別のドメイン</span><span class="sxs-lookup"><span data-stu-id="15cf3-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="15cf3-118">`https://www.example.com/foo.html` &ndash; 別のサブドメイン</span><span class="sxs-lookup"><span data-stu-id="15cf3-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="15cf3-119">`http://example.com/foo.html` &ndash; 別の配色</span><span class="sxs-lookup"><span data-stu-id="15cf3-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="15cf3-120">`https://example.com:9000/foo.html` &ndash; 別のポート</span><span class="sxs-lookup"><span data-stu-id="15cf3-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="15cf3-121">Internet Explorer は、生成元を比較するときにポートを考慮しません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="15cf3-122">CORS のサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="15cf3-123">参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)へのパッケージ参照を追加したり、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="15cf3-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="15cf3-124">参照、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)へのパッケージ参照を追加したり、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="15cf3-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="15cf3-125">パッケージ参照を追加、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="15cf3-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="15cf3-126">呼び出す<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>で`Startup.ConfigureServices`CORS サービス アプリのサービス コンテナーを追加します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="15cf3-127">CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="15cf3-127">Enable CORS</span></span>

<span data-ttu-id="15cf3-128">CORS のサービスを登録すると、ASP.NET Core アプリで CORS を有効にするのに方法を次のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="15cf3-129">[CORS ミドルウェア](#enable-cors-with-cors-middleware)&ndash;ミドルウェアを使用してアプリをグローバルに適用する CORS ポリシー。</span><span class="sxs-lookup"><span data-stu-id="15cf3-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="15cf3-130">[MVC で CORS](#enable-cors-in-mvc) &ndash;アクションごとまたはコント ローラーごとに適用する CORS ポリシー。</span><span class="sxs-lookup"><span data-stu-id="15cf3-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="15cf3-131">CORS ミドルウェアは使用されません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="15cf3-132">CORS ミドルウェアで CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="15cf3-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="15cf3-133">CORS ミドルウェアは、アプリへのクロス オリジン要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="15cf3-134">要求処理パイプラインでは、CORS ミドルウェアを有効にするを呼び出して、<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>拡張メソッドで`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="15cf3-135">CORS ミドルウェアする必要がありますの前に、エンドポイントが定義されて、アプリでのクロス オリジン要求をサポートする (たとえば、呼び出しの前に`UseMvc`MVC と Razor ページのミドルウェアの)。</span><span class="sxs-lookup"><span data-stu-id="15cf3-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="15cf3-136">A*クロス オリジン ポリシー*を使用して、CORS ミドルウェアを追加するときに指定することができます、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>クラス。</span><span class="sxs-lookup"><span data-stu-id="15cf3-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="15cf3-137">CORS ポリシーを定義するための 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="15cf3-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="15cf3-138">呼び出す`UseCors`ラムダで。</span><span class="sxs-lookup"><span data-stu-id="15cf3-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="15cf3-139">ラムダは、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> オブジェクトをとります。</span><span class="sxs-lookup"><span data-stu-id="15cf3-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="15cf3-140">[構成オプション](#cors-policy-options)など`WithOrigins`はこのトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="15cf3-141">上記の例では、ポリシーによりからのクロス オリジン要求`https://example.com`およびその他のオリジンはありません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="15cf3-142">末尾のスラッシュせず、URL を指定する必要があります (`/`)。</span><span class="sxs-lookup"><span data-stu-id="15cf3-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="15cf3-143">URL が終了した場合は`/`、比較を返します`false`ヘッダーは返されません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="15cf3-144">`CorsPolicyBuilder` メソッドの呼び出しをチェーンできるように、fluent API があります。</span><span class="sxs-lookup"><span data-stu-id="15cf3-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="15cf3-145">1 つまたは複数の名前付き CORS ポリシーを定義し、実行時に名前で、ポリシーを選択します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="15cf3-146">次の例では、という名前のユーザー定義の CORS ポリシー *AllowSpecificOrigin*します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="15cf3-147">ポリシーを選択する名前を渡す`UseCors`:</span><span class="sxs-lookup"><span data-stu-id="15cf3-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="15cf3-148">MVC で CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="15cf3-148">Enable CORS in MVC</span></span>

<span data-ttu-id="15cf3-149">MVC を使用して、アクションごとまたはコント ローラーごとに特定の CORS ポリシーを適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="15cf3-150">MVC を使用して、CORS を有効にする、登録されている CORS サービスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="15cf3-151">CORS ミドルウェアは使用されません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="15cf3-152">アクションごと</span><span class="sxs-lookup"><span data-stu-id="15cf3-152">Per action</span></span>

<span data-ttu-id="15cf3-153">特定のアクションの CORS ポリシーを指定するには、追加、 [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性をアクションにします。</span><span class="sxs-lookup"><span data-stu-id="15cf3-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="15cf3-154">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="15cf3-155">コント ローラーごと</span><span class="sxs-lookup"><span data-stu-id="15cf3-155">Per controller</span></span>

<span data-ttu-id="15cf3-156">特定のコント ローラーの CORS ポリシーを指定するには、追加、 [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性をコント ローラー クラス。</span><span class="sxs-lookup"><span data-stu-id="15cf3-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="15cf3-157">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="15cf3-158">優先順位は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="15cf3-158">The precedence order is:</span></span>

1. <span data-ttu-id="15cf3-159">アクション</span><span class="sxs-lookup"><span data-stu-id="15cf3-159">action</span></span>
1. <span data-ttu-id="15cf3-160">コントローラー</span><span class="sxs-lookup"><span data-stu-id="15cf3-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="15cf3-161">CORS を無効にします。</span><span class="sxs-lookup"><span data-stu-id="15cf3-161">Disable CORS</span></span>

<span data-ttu-id="15cf3-162">コント ローラーまたはアクションの CORS を無効にする、 [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)属性。</span><span class="sxs-lookup"><span data-stu-id="15cf3-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="15cf3-163">CORS ポリシー オプション</span><span class="sxs-lookup"><span data-stu-id="15cf3-163">CORS policy options</span></span>

<span data-ttu-id="15cf3-164">このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="15cf3-165">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="15cf3-166">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="15cf3-167">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="15cf3-168">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="15cf3-169">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="15cf3-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="15cf3-170">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="15cf3-171">いくつかのオプションの読み取りをすると役立つ場合があります、 [CORS ではどのように動作](#how-cors-works)最初のセクションします。</span><span class="sxs-lookup"><span data-stu-id="15cf3-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="15cf3-172">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-172">Set the allowed origins</span></span>

<span data-ttu-id="15cf3-173">1 つまたは複数の特定のオリジンを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-173">To allow one or more specific origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

<span data-ttu-id="15cf3-174">すべてのオリジンを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-174">To allow all origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="15cf3-175">任意のオリジンからの要求を許可する前に慎重に検討してください。</span><span class="sxs-lookup"><span data-stu-id="15cf3-175">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="15cf3-176">任意のオリジンからの要求を許可することを意味*任意の web サイト*アプリへのクロス オリジン要求を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-176">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

<span data-ttu-id="15cf3-177">この設定に影響[プレフライト要求とアクセス制御の許可-オリジン ヘッダー](#preflight-requests) (このトピックの後半で説明)。</span><span class="sxs-lookup"><span data-stu-id="15cf3-177">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="15cf3-178">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-178">Set the allowed HTTP methods</span></span>

<span data-ttu-id="15cf3-179">すべての HTTP メソッドを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-179">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="15cf3-180">この設定に影響[プレフライト要求とアクセスの制御-許可する-メソッド ヘッダー](#preflight-requests) (このトピックの後半で説明)。</span><span class="sxs-lookup"><span data-stu-id="15cf3-180">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="15cf3-181">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-181">Set the allowed request headers</span></span>

<span data-ttu-id="15cf3-182">CORS 要求で送信される特定のヘッダーを許可するという*要求ヘッダーを作成する*、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>し、許可されたヘッダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-182">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="15cf3-183">許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-183">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="15cf3-184">この設定に影響[プレフライト要求とアクセス制御の要求ヘッダー ヘッダー](#preflight-requests) (このトピックの後半で説明)。</span><span class="sxs-lookup"><span data-stu-id="15cf3-184">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="15cf3-185">指定された特定のヘッダーに一致する CORS ミドルウェア ポリシー`WithHeaders`ヘッダーが送信されるときにのみ可能なは`Access-Control-Request-Headers`に記載されているヘッダーと正確に一致`WithHeaders`します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-185">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="15cf3-186">たとえば、次のように構成されているアプリを検討してください。</span><span class="sxs-lookup"><span data-stu-id="15cf3-186">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="15cf3-187">CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求を拒否`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) の一覧にない`WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="15cf3-187">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="15cf3-188">アプリを返します、 *200 ok をクリック*応答が戻り、CORS ヘッダーを送信しません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-188">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="15cf3-189">そのため、ブラウザーでは、クロス オリジン要求を試行しません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-189">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="15cf3-190">CORS ミドルウェアで 4 つのヘッダーを常に許可する、 `Access-Control-Request-Headers` CorsPolicy.Headers で構成されている値に関係なく送信します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-190">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="15cf3-191">このヘッダーの一覧は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="15cf3-191">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="15cf3-192">たとえば、次のように構成されているアプリを検討してください。</span><span class="sxs-lookup"><span data-stu-id="15cf3-192">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="15cf3-193">CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求に正常に応答ため`Content-Language`は常にホワイト リストに登録します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-193">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="15cf3-194">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-194">Set the exposed response headers</span></span>

<span data-ttu-id="15cf3-195">既定では、ブラウザーはすべてのアプリに応答ヘッダーで公開されません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-195">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="15cf3-196">詳細については、次を参照してください。 [W3C のクロス オリジン リソース共有 (用語集): 単純な応答ヘッダー](https://www.w3.org/TR/cors/#simple-response-header)します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-196">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="15cf3-197">既定で使用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="15cf3-197">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="15cf3-198">CORS の仕様は、これらのヘッダーを呼び出す*単純な応答ヘッダー*します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-198">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="15cf3-199">で他のヘッダーをアプリに使用できるように呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-199">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="15cf3-200">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="15cf3-200">Credentials in cross-origin requests</span></span>

<span data-ttu-id="15cf3-201">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="15cf3-201">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="15cf3-202">既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-202">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="15cf3-203">Cookie および HTTP 認証方式は、資格情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-203">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="15cf3-204">クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります`XMLHttpRequest.withCredentials`に`true`します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-204">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="15cf3-205">使用して`XMLHttpRequest`直接。</span><span class="sxs-lookup"><span data-stu-id="15cf3-205">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="15cf3-206">Jquery では。</span><span class="sxs-lookup"><span data-stu-id="15cf3-206">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="15cf3-207">さらに、サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="15cf3-207">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="15cf3-208">クロス オリジンの資格情報を許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-208">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="15cf3-209">HTTP 応答が含まれる、`Access-Control-Allow-Credentials`ヘッダーで、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-209">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="15cf3-210">ブラウザーが資格情報を送信しますが、有効な応答を含まない`Access-Control-Allow-Credentials`ヘッダー、ブラウザーは、アプリへの応答を公開しないし、クロス オリジン要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-210">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="15cf3-211">クロス オリジンの資格情報を許可する際に注意します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-211">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="15cf3-212">別のドメインに web サイトでは、ユーザーの知識がなくても、ユーザーの代わりに、アプリにサインイン済みのユーザーの資格情報を送信できます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-212">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="15cf3-213">CORS の仕様もその設定を示すオリジンを`"*"`(すべてのオリジン) 有効でない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-213">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="15cf3-214">プレフライト要求</span><span class="sxs-lookup"><span data-stu-id="15cf3-214">Preflight requests</span></span>

<span data-ttu-id="15cf3-215">一部の CORS 要求では、ブラウザーは、実際の要求を行う前に、追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-215">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="15cf3-216">この要求と呼ばれる、*プレフライト要求*します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-216">This request is called a *preflight request*.</span></span> <span data-ttu-id="15cf3-217">次の条件に該当する場合、ブラウザーでプレフライト要求をスキップできます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-217">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="15cf3-218">要求メソッドは、GET、HEAD、または POST です。</span><span class="sxs-lookup"><span data-stu-id="15cf3-218">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="15cf3-219">アプリが要求ヘッダー以外に設定されていない`Accept`、 `Accept-Language`、 `Content-Language`、 `Content-Type`、または`Last-Event-ID`します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-219">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="15cf3-220">`Content-Type`ヘッダー場合、次の値のいずれか。</span><span class="sxs-lookup"><span data-stu-id="15cf3-220">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="15cf3-221">要求ヘッダーでルール セットのクライアント要求を呼び出すことによって、アプリを設定するヘッダーに適用の`setRequestHeader`上、`XMLHttpRequest`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="15cf3-221">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="15cf3-222">CORS の仕様は、これらのヘッダーを呼び出す*要求ヘッダーを作成する*します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-222">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="15cf3-223">ヘッダーは、ブラウザー設定できるように、ルールは適用されません`User-Agent`、 `Host`、または`Content-Length`します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-223">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="15cf3-224">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-224">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="15cf3-225">事前要求は HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-225">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="15cf3-226">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="15cf3-226">It includes two special headers:</span></span>

* <span data-ttu-id="15cf3-227">`Access-Control-Request-Method`: 実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="15cf3-227">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="15cf3-228">`Access-Control-Request-Headers`: アプリが、実際の要求で設定できる要求ヘッダーの一覧。</span><span class="sxs-lookup"><span data-stu-id="15cf3-228">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="15cf3-229">前述のように、ブラウザー設定などのヘッダーは含まれません`User-Agent`します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-229">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="15cf3-230">CORS プレフライト要求を含めることができます、`Access-Control-Request-Headers`ヘッダーで、サーバーの実際の要求で送信されるヘッダーを示します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-230">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="15cf3-231">特定のヘッダーを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-231">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="15cf3-232">許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-232">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="15cf3-233">ブラウザーはどのように設定でまったく一貫性のある`Access-Control-Request-Headers`します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-233">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="15cf3-234">以外に何もヘッダーを設定する場合`"*"`(を使用して、または<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)、以上含める必要がある`Accept`、 `Content-Type`、および`Origin`、さらにサポートするカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="15cf3-234">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="15cf3-235">(サーバーが要求を許可することを想定) プレフライト要求に応答の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-235">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="15cf3-236">応答が含まれています、`Access-Control-Allow-Methods`ヘッダーを許可されるメソッドを一覧表示して、必要に応じて、`Access-Control-Allow-Headers`ヘッダーで、許可されたヘッダーを一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="15cf3-236">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="15cf3-237">プレフライト要求が成功すると、ブラウザーは、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-237">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="15cf3-238">アプリが返したプレフライト要求が拒否された場合、 *200 OK*応答戻る、CORS ヘッダーを送信しないが。</span><span class="sxs-lookup"><span data-stu-id="15cf3-238">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="15cf3-239">そのため、ブラウザーでは、クロス オリジン要求を試行しません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-239">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="15cf3-240">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-240">Set the preflight expiration time</span></span>

<span data-ttu-id="15cf3-241">`Access-Control-Max-Age`ヘッダーは、プレフライト要求に応答をキャッシュできる期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-241">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="15cf3-242">このヘッダーを設定するには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="15cf3-242">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="15cf3-243">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="15cf3-243">How CORS works</span></span>

<span data-ttu-id="15cf3-244">このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-244">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="15cf3-245">CORS ポリシーを正しく構成されているし、予期しない動作が発生したときにデバッグできるようにの CORS のしくみを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="15cf3-245">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="15cf3-246">CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="15cf3-246">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="15cf3-247">ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-247">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="15cf3-248">カスタム JavaScript コードは、CORS を有効にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-248">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="15cf3-249">次は、クロス オリジン要求の例です。</span><span class="sxs-lookup"><span data-stu-id="15cf3-249">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="15cf3-250">`Origin`ヘッダーは要求を行っているサイトのドメインを提供します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-250">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="15cf3-251">設定されているサーバーでは、要求を許可している場合、`Access-Control-Allow-Origin`応答のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="15cf3-251">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="15cf3-252">このヘッダーの値と一致するか、`Origin`要求からヘッダーまたはワイルドカード値`"*"`、任意のオリジンを許可することを意味します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-252">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="15cf3-253">応答に含まれていない場合、`Access-Control-Allow-Origin`ヘッダー、クロス オリジン要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="15cf3-253">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="15cf3-254">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="15cf3-254">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="15cf3-255">サーバーに正常な応答が返される場合でも、ブラウザーは、応答を使用できるようにクライアント アプリ。</span><span class="sxs-lookup"><span data-stu-id="15cf3-255">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15cf3-256">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="15cf3-256">Additional resources</span></span>

* [<span data-ttu-id="15cf3-257">クロス オリジン リソース共有 (CORS)</span><span class="sxs-lookup"><span data-stu-id="15cf3-257">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
