---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法として許可または ASP.NET Core アプリでのクロス オリジン要求を拒否するための標準 CORS します。
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: security/cors
ms.openlocfilehash: 8e5056b448d47d75272e9394b03ce8a58b05a0f4
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191322"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="6ca13-103">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="6ca13-104">作成者 [Mike Wasson](https://github.com/mikewasson)、 [Shayne Boyer](https://twitter.com/spboyer)、および [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6ca13-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="6ca13-105">ブラウザーのセキュリティは、web ページが web ページを提供するものとは異なるドメインに要求を行うことを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="6ca13-106">この制限と呼ばれる、*同一オリジン ポリシー*します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="6ca13-107">同一オリジン ポリシーは、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="6ca13-108">場合によっては、他のサイトでは、クロス オリジン要求を行うアプリに許可する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="6ca13-109">[クロス オリジン リソース共有](https://www.w3.org/TR/cors/)(CORS) は、サーバーに同一生成元ポリシーの制限を緩和させる W3C 標準の１つです。</span><span class="sxs-lookup"><span data-stu-id="6ca13-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="6ca13-110">CORS を使用することによって、不明なリクエストは拒否しながら、一部のクロス オリジン要求のみを明示的に許可できるようになります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="6ca13-111">CORS は安全性と以前の手法よりも柔軟性など[JSONP](https://wikipedia.org/wiki/JSONP)します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="6ca13-112">このトピックでは、ASP.NET Core アプリで CORS を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="6ca13-113">同じ生成元</span><span class="sxs-lookup"><span data-stu-id="6ca13-113">Same origin</span></span>

<span data-ttu-id="6ca13-114">同じスキーム、ホスト、およびポートがある場合、2 つの Url が同じ配信元がある ([RFC 6454](https://tools.ietf.org/html/rfc6454))。</span><span class="sxs-lookup"><span data-stu-id="6ca13-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="6ca13-115">次の 2 つの URL は生成元が同じです。</span><span class="sxs-lookup"><span data-stu-id="6ca13-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="6ca13-116">これらの Url があるさまざまなオリジンより前の 2 つの Url:</span><span class="sxs-lookup"><span data-stu-id="6ca13-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="6ca13-117">`https://example.net` &ndash; 別のドメイン</span><span class="sxs-lookup"><span data-stu-id="6ca13-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="6ca13-118">`https://www.example.com/foo.html` &ndash; 別のサブドメイン</span><span class="sxs-lookup"><span data-stu-id="6ca13-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="6ca13-119">`http://example.com/foo.html` &ndash; 別の配色</span><span class="sxs-lookup"><span data-stu-id="6ca13-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="6ca13-120">`https://example.com:9000/foo.html` &ndash; 別のポート</span><span class="sxs-lookup"><span data-stu-id="6ca13-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="6ca13-121">Internet Explorer は、生成元を比較するときにポートを考慮しません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="6ca13-122">CORS のサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6ca13-123">参照、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)へのパッケージ参照を追加したり、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="6ca13-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6ca13-124">参照、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)へのパッケージ参照を追加したり、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="6ca13-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6ca13-125">パッケージ参照を追加、 [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="6ca13-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="6ca13-126">呼び出す<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>で`Startup.ConfigureServices`CORS サービス アプリのサービス コンテナーを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="6ca13-127">CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-127">Enable CORS</span></span>

<span data-ttu-id="6ca13-128">CORS のサービスを登録すると、ASP.NET Core アプリで CORS を有効にするのに方法を次のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="6ca13-129">[CORS ミドルウェア](#enable-cors-with-cors-middleware)&ndash;ミドルウェアを使用してアプリをグローバルに適用する CORS ポリシー。</span><span class="sxs-lookup"><span data-stu-id="6ca13-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="6ca13-130">[MVC で CORS](#enable-cors-in-mvc) &ndash;アクションごとまたはコント ローラーごとに適用する CORS ポリシー。</span><span class="sxs-lookup"><span data-stu-id="6ca13-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="6ca13-131">CORS ミドルウェアは使用されません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="6ca13-132">CORS ミドルウェアで CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="6ca13-133">CORS ミドルウェアは、アプリへのクロス オリジン要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="6ca13-134">要求処理パイプラインでは、CORS ミドルウェアを有効にするを呼び出して、<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>拡張メソッドで`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="6ca13-135">CORS ミドルウェアする必要がありますの前に、エンドポイントが定義されて、アプリでのクロス オリジン要求をサポートする (たとえば、呼び出しの前に`UseMvc`MVC と Razor ページのミドルウェアの)。</span><span class="sxs-lookup"><span data-stu-id="6ca13-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="6ca13-136">A*クロス オリジン ポリシー*を使用して、CORS ミドルウェアを追加するときに指定することができます、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>クラス。</span><span class="sxs-lookup"><span data-stu-id="6ca13-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="6ca13-137">CORS ポリシーを定義するための 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="6ca13-138">呼び出す`UseCors`ラムダで。</span><span class="sxs-lookup"><span data-stu-id="6ca13-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="6ca13-139">ラムダは、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> オブジェクトをとります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="6ca13-140">[構成オプション](#cors-policy-options)など`WithOrigins`はこのトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="6ca13-141">上記の例では、ポリシーによりからのクロス オリジン要求`https://example.com`およびその他のオリジンはありません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="6ca13-142">末尾のスラッシュせず、URL を指定する必要があります (`/`)。</span><span class="sxs-lookup"><span data-stu-id="6ca13-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="6ca13-143">URL が終了した場合は`/`、比較を返します`false`ヘッダーは返されません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="6ca13-144">`CorsPolicyBuilder` メソッドの呼び出しをチェーンできるように、fluent API があります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="6ca13-145">1 つまたは複数の名前付き CORS ポリシーを定義し、実行時に名前で、ポリシーを選択します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="6ca13-146">次の例では、という名前のユーザー定義の CORS ポリシー *AllowSpecificOrigin*します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="6ca13-147">ポリシーを選択する名前を渡す`UseCors`:</span><span class="sxs-lookup"><span data-stu-id="6ca13-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="6ca13-148">MVC で CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-148">Enable CORS in MVC</span></span>

<span data-ttu-id="6ca13-149">MVC を使用して、アクションごとまたはコント ローラーごとに特定の CORS ポリシーを適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="6ca13-150">MVC を使用して、CORS を有効にする、登録されている CORS サービスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="6ca13-151">CORS ミドルウェアは使用されません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="6ca13-152">アクションごと</span><span class="sxs-lookup"><span data-stu-id="6ca13-152">Per action</span></span>

<span data-ttu-id="6ca13-153">特定のアクションの CORS ポリシーを指定するには、追加、 [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性をアクションにします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="6ca13-154">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="6ca13-155">コント ローラーごと</span><span class="sxs-lookup"><span data-stu-id="6ca13-155">Per controller</span></span>

<span data-ttu-id="6ca13-156">特定のコント ローラーの CORS ポリシーを指定するには、追加、 [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性をコント ローラー クラス。</span><span class="sxs-lookup"><span data-stu-id="6ca13-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="6ca13-157">ポリシー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="6ca13-158">優先順位は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="6ca13-158">The precedence order is:</span></span>

1. <span data-ttu-id="6ca13-159">アクション</span><span class="sxs-lookup"><span data-stu-id="6ca13-159">action</span></span>
1. <span data-ttu-id="6ca13-160">コントローラー</span><span class="sxs-lookup"><span data-stu-id="6ca13-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="6ca13-161">CORS を無効にします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-161">Disable CORS</span></span>

<span data-ttu-id="6ca13-162">コント ローラーまたはアクションの CORS を無効にする、 [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)属性。</span><span class="sxs-lookup"><span data-stu-id="6ca13-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="6ca13-163">CORS ポリシー オプション</span><span class="sxs-lookup"><span data-stu-id="6ca13-163">CORS policy options</span></span>

<span data-ttu-id="6ca13-164">このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="6ca13-165">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="6ca13-166">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="6ca13-167">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="6ca13-168">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="6ca13-169">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="6ca13-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="6ca13-170">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="6ca13-171">いくつかのオプションの読み取りをすると役立つ場合があります、 [CORS ではどのように動作](#how-cors-works)最初のセクションします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="6ca13-172">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-172">Set the allowed origins</span></span>

<span data-ttu-id="6ca13-173">ASP.NET Core MVC で CORS ミドルウェアでは、許可されるオリジンを指定するいくつかの方法があります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-173">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="6ca13-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: 1 つまたは複数の Url を指定できます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-174"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>: Allows specifying one or more URLs.</span></span> <span data-ttu-id="6ca13-175">URL には、スキーム、ホスト名、およびパス情報がないポートを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-175">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="6ca13-176">たとえば、`https://example.com` のようにします。</span><span class="sxs-lookup"><span data-stu-id="6ca13-176">For example, `https://example.com`.</span></span> <span data-ttu-id="6ca13-177">末尾のスラッシュせず、URL を指定する必要があります (`/`)。</span><span class="sxs-lookup"><span data-stu-id="6ca13-177">The URL must be specified without a trailing slash (`/`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

* <span data-ttu-id="6ca13-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: 任意のスキームですべてのオリジンからの CORS 要求を許可する (`http`または`https`)。</span><span class="sxs-lookup"><span data-stu-id="6ca13-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="6ca13-179">任意のオリジンからの要求を許可する前に慎重に検討してください。</span><span class="sxs-lookup"><span data-stu-id="6ca13-179">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="6ca13-180">任意のオリジンからの要求を許可することを意味*任意の web サイト*アプリへのクロス オリジン要求を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-180">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="6ca13-181">指定する`AllowAnyOrigin`と`AllowCredentials`構成は安全でないと、クロスサイト リクエスト フォージェリで発生することができます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="6ca13-182">CORS サービスは、2 つのアプリが構成されている場合に、CORS の無効な応答を返します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-182">The CORS service returns an invalid CORS response when an app is configured with the two.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="6ca13-183">指定する`AllowAnyOrigin`と`AllowCredentials`構成は安全でないと、クロスサイト リクエスト フォージェリで発生することができます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="6ca13-184">クライアントがサーバー リソースにアクセスを承認する必要がある場合は、オリジンの正確なリストを指定することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="6ca13-184">Consider specifying an exact list of origins if your client needs to authorize to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="6ca13-185">この設定に影響[プレフライト要求とアクセス制御の許可-オリジン ヘッダー](#preflight-requests) (このトピックの後半で説明)。</span><span class="sxs-lookup"><span data-stu-id="6ca13-185">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

* <span data-ttu-id="6ca13-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> -特定のドメインの任意のサブドメインからの CORS 要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-186"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> - Allows CORS requests from any sub-domain of a given domain.</span></span> <span data-ttu-id="6ca13-187">スキームでは、ワイルドカードをすることはできません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-187">The scheme cannot be a wildcard.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=98-104&highlight=4)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="6ca13-188">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-188">Set the allowed HTTP methods</span></span>

<span data-ttu-id="6ca13-189">すべての HTTP メソッドを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="6ca13-189">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="6ca13-190">この設定に影響[プレフライト要求とアクセスの制御-許可する-メソッド ヘッダー](#preflight-requests) (このトピックの後半で説明)。</span><span class="sxs-lookup"><span data-stu-id="6ca13-190">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="6ca13-191">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-191">Set the allowed request headers</span></span>

<span data-ttu-id="6ca13-192">CORS 要求で送信される特定のヘッダーを許可するという*要求ヘッダーを作成する*、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>し、許可されたヘッダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="6ca13-193">許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="6ca13-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="6ca13-194">この設定に影響[プレフライト要求とアクセス制御の要求ヘッダー ヘッダー](#preflight-requests) (このトピックの後半で説明)。</span><span class="sxs-lookup"><span data-stu-id="6ca13-194">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6ca13-195">指定された特定のヘッダーに一致する CORS ミドルウェア ポリシー`WithHeaders`ヘッダーが送信されるときにのみ可能なは`Access-Control-Request-Headers`に記載されているヘッダーと正確に一致`WithHeaders`します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-195">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="6ca13-196">たとえば、次のように構成されているアプリを検討してください。</span><span class="sxs-lookup"><span data-stu-id="6ca13-196">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="6ca13-197">CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求を拒否`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) の一覧にない`WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="6ca13-197">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="6ca13-198">アプリを返します、 *200 ok をクリック*応答が戻り、CORS ヘッダーを送信しません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-198">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="6ca13-199">そのため、ブラウザーでは、クロス オリジン要求を試行しません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-199">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="6ca13-200">CORS ミドルウェアで 4 つのヘッダーを常に許可する、 `Access-Control-Request-Headers` CorsPolicy.Headers で構成されている値に関係なく送信します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-200">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="6ca13-201">このヘッダーの一覧は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="6ca13-201">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="6ca13-202">たとえば、次のように構成されているアプリを検討してください。</span><span class="sxs-lookup"><span data-stu-id="6ca13-202">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="6ca13-203">CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求に正常に応答ため`Content-Language`は常にホワイト リストに登録します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-203">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="6ca13-204">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-204">Set the exposed response headers</span></span>

<span data-ttu-id="6ca13-205">既定では、ブラウザーはすべてのアプリに応答ヘッダーで公開されません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-205">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="6ca13-206">詳細については、次を参照してください。 [W3C のクロス オリジン リソース共有 (用語集): 単純な応答ヘッダー](https://www.w3.org/TR/cors/#simple-response-header)します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-206">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="6ca13-207">既定で使用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="6ca13-207">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="6ca13-208">CORS の仕様は、これらのヘッダーを呼び出す*単純な応答ヘッダー*します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-208">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="6ca13-209">で他のヘッダーをアプリに使用できるように呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="6ca13-209">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="6ca13-210">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="6ca13-210">Credentials in cross-origin requests</span></span>

<span data-ttu-id="6ca13-211">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="6ca13-211">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="6ca13-212">既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-212">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="6ca13-213">Cookie および HTTP 認証方式は、資格情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-213">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="6ca13-214">クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります`XMLHttpRequest.withCredentials`に`true`します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-214">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="6ca13-215">使用して`XMLHttpRequest`直接。</span><span class="sxs-lookup"><span data-stu-id="6ca13-215">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="6ca13-216">Jquery では。</span><span class="sxs-lookup"><span data-stu-id="6ca13-216">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="6ca13-217">さらに、サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-217">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="6ca13-218">クロス オリジンの資格情報を許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="6ca13-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="6ca13-219">HTTP 応答が含まれる、`Access-Control-Allow-Credentials`ヘッダーで、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="6ca13-220">ブラウザーが資格情報を送信しますが、有効な応答を含まない`Access-Control-Allow-Credentials`ヘッダー、ブラウザーは、アプリへの応答を公開しないし、クロス オリジン要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="6ca13-221">クロス オリジンの資格情報を許可する際に注意します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-221">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="6ca13-222">別のドメインに web サイトでは、ユーザーの知識がなくても、ユーザーの代わりに、アプリにサインイン済みのユーザーの資格情報を送信できます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="6ca13-223">CORS の仕様もその設定を示すオリジンを`"*"`(すべてのオリジン) 有効でない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="6ca13-224">プレフライト要求</span><span class="sxs-lookup"><span data-stu-id="6ca13-224">Preflight requests</span></span>

<span data-ttu-id="6ca13-225">一部の CORS 要求では、ブラウザーは、実際の要求を行う前に、追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="6ca13-226">この要求と呼ばれる、*プレフライト要求*します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="6ca13-227">次の条件に該当する場合、ブラウザーでプレフライト要求をスキップできます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="6ca13-228">要求メソッドは、GET、HEAD、または POST です。</span><span class="sxs-lookup"><span data-stu-id="6ca13-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="6ca13-229">アプリが要求ヘッダー以外に設定されていない`Accept`、 `Accept-Language`、 `Content-Language`、 `Content-Type`、または`Last-Event-ID`します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="6ca13-230">`Content-Type`ヘッダー場合、次の値のいずれか。</span><span class="sxs-lookup"><span data-stu-id="6ca13-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="6ca13-231">要求ヘッダーでルール セットのクライアント要求を呼び出すことによって、アプリを設定するヘッダーに適用の`setRequestHeader`上、`XMLHttpRequest`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="6ca13-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="6ca13-232">CORS の仕様は、これらのヘッダーを呼び出す*要求ヘッダーを作成する*します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="6ca13-233">ヘッダーは、ブラウザー設定できるように、ルールは適用されません`User-Agent`、 `Host`、または`Content-Length`します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="6ca13-234">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="6ca13-235">事前要求は HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="6ca13-236">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ca13-236">It includes two special headers:</span></span>

* <span data-ttu-id="6ca13-237">`Access-Control-Request-Method`: 実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="6ca13-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="6ca13-238">`Access-Control-Request-Headers`: アプリが、実際の要求で設定できる要求ヘッダーの一覧。</span><span class="sxs-lookup"><span data-stu-id="6ca13-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="6ca13-239">前述のように、ブラウザー設定などのヘッダーは含まれません`User-Agent`します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="6ca13-240">CORS プレフライト要求を含めることができます、`Access-Control-Request-Headers`ヘッダーで、サーバーの実際の要求で送信されるヘッダーを示します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="6ca13-241">特定のヘッダーを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="6ca13-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="6ca13-242">許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="6ca13-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="6ca13-243">ブラウザーはどのように設定でまったく一貫性のある`Access-Control-Request-Headers`します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="6ca13-244">以外に何もヘッダーを設定する場合`"*"`(を使用して、または<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)、以上含める必要がある`Accept`、 `Content-Type`、および`Origin`、さらにサポートするカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="6ca13-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="6ca13-245">(サーバーが要求を許可することを想定) プレフライト要求に応答の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="6ca13-246">応答が含まれています、`Access-Control-Allow-Methods`ヘッダーを許可されるメソッドを一覧表示して、必要に応じて、`Access-Control-Allow-Headers`ヘッダーで、許可されたヘッダーを一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="6ca13-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="6ca13-247">プレフライト要求が成功すると、ブラウザーは、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="6ca13-248">アプリが返したプレフライト要求が拒否された場合、 *200 OK*応答戻る、CORS ヘッダーを送信しないが。</span><span class="sxs-lookup"><span data-stu-id="6ca13-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="6ca13-249">そのため、ブラウザーでは、クロス オリジン要求を試行しません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="6ca13-250">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-250">Set the preflight expiration time</span></span>

<span data-ttu-id="6ca13-251">`Access-Control-Max-Age`ヘッダーは、プレフライト要求に応答をキャッシュできる期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="6ca13-252">このヘッダーを設定するには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="6ca13-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="6ca13-253">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="6ca13-253">How CORS works</span></span>

<span data-ttu-id="6ca13-254">このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-254">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="6ca13-255">CORS ポリシーを正しく構成されているし、予期しない動作が発生したときにデバッグできるようにの CORS のしくみを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ca13-255">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="6ca13-256">CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="6ca13-256">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="6ca13-257">ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-257">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="6ca13-258">カスタム JavaScript コードは、CORS を有効にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-258">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="6ca13-259">次は、クロス オリジン要求の例です。</span><span class="sxs-lookup"><span data-stu-id="6ca13-259">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="6ca13-260">`Origin`ヘッダーは要求を行っているサイトのドメインを提供します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-260">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="6ca13-261">設定されているサーバーでは、要求を許可している場合、`Access-Control-Allow-Origin`応答のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="6ca13-261">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="6ca13-262">このヘッダーの値と一致するか、`Origin`要求からヘッダーまたはワイルドカード値`"*"`、任意のオリジンを許可することを意味します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-262">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="6ca13-263">応答に含まれていない場合、`Access-Control-Allow-Origin`ヘッダー、クロス オリジン要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="6ca13-263">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="6ca13-264">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="6ca13-264">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="6ca13-265">サーバーに正常な応答が返される場合でも、ブラウザーは、応答を使用できるようにクライアント アプリ。</span><span class="sxs-lookup"><span data-stu-id="6ca13-265">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ca13-266">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6ca13-266">Additional resources</span></span>

* [<span data-ttu-id="6ca13-267">クロス オリジン リソース共有 (CORS)</span><span class="sxs-lookup"><span data-stu-id="6ca13-267">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
