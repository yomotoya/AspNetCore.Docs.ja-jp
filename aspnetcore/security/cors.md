---
title: ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。
author: rick-anderson
description: 学習方法として許可または ASP.NET Core アプリでのクロス オリジン要求を拒否するための標準 CORS します。
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: fe5b750c44e5fad9ba80efb2cc8116d0a64b1a17
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068298"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="19543-103">ASP.NET Core でのクロス オリジン要求 (CORS) を有効にします。</span><span class="sxs-lookup"><span data-stu-id="19543-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="19543-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="19543-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="19543-105">この記事では、ASP.NET Core アプリで CORS を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="19543-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="19543-106">ブラウザーのセキュリティは、web ページが web ページを提供するものとは異なるドメインに要求を行うことを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="19543-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="19543-107">この制限と呼ばれる、*同一オリジン ポリシー*します。</span><span class="sxs-lookup"><span data-stu-id="19543-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="19543-108">同一オリジン ポリシーは、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="19543-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="19543-109">場合によっては、他のサイトでは、クロス オリジン要求を行うアプリに許可する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="19543-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="19543-110">詳細については、次を参照してください。、 [Mozilla CORS 記事](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)します。</span><span class="sxs-lookup"><span data-stu-id="19543-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="19543-111">[クロス オリジン リソース共有](https://www.w3.org/TR/cors/)(CORS)。</span><span class="sxs-lookup"><span data-stu-id="19543-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="19543-112">W3C による同一オリジン ポリシーの緩和するようにサーバーを利用できる標準。</span><span class="sxs-lookup"><span data-stu-id="19543-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="19543-113">**いない**CORS、セキュリティ機能は、セキュリティを緩和します。</span><span class="sxs-lookup"><span data-stu-id="19543-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="19543-114">CORS を許可することで、API をより安全なすることはできません。</span><span class="sxs-lookup"><span data-stu-id="19543-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="19543-115">詳細については、次を参照してください。 [CORS ではどのように動作](#how-cors)します。</span><span class="sxs-lookup"><span data-stu-id="19543-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="19543-116">他のユーザーを拒否しながら、いくつかのクロス オリジン要求を明示的に許可するサーバーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="19543-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="19543-117">安全性と以前の手法よりも柔軟性はなど、 [JSONP](/dotnet/framework/wcf/samples/jsonp)します。</span><span class="sxs-lookup"><span data-stu-id="19543-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="19543-118">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="19543-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="19543-119">同じ生成元</span><span class="sxs-lookup"><span data-stu-id="19543-119">Same origin</span></span>

<span data-ttu-id="19543-120">同じスキーム、ホスト、およびポートがある場合、2 つの Url が同じ配信元がある ([RFC 6454](https://tools.ietf.org/html/rfc6454))。</span><span class="sxs-lookup"><span data-stu-id="19543-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="19543-121">次の 2 つの URL は生成元が同じです。</span><span class="sxs-lookup"><span data-stu-id="19543-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="19543-122">これらの Url があるさまざまなオリジンより前の 2 つの Url:</span><span class="sxs-lookup"><span data-stu-id="19543-122">These URLs have different origins than the previous two URLs:</span></span>

* `https://example.net` <span data-ttu-id="19543-123">&ndash; 別のドメイン</span><span class="sxs-lookup"><span data-stu-id="19543-123">&ndash; Different domain</span></span>
* `https://www.example.com/foo.html` <span data-ttu-id="19543-124">&ndash; 別のサブドメイン</span><span class="sxs-lookup"><span data-stu-id="19543-124">&ndash; Different subdomain</span></span>
* `http://example.com/foo.html` <span data-ttu-id="19543-125">&ndash; 別の配色</span><span class="sxs-lookup"><span data-stu-id="19543-125">&ndash; Different scheme</span></span>
* `https://example.com:9000/foo.html` <span data-ttu-id="19543-126">&ndash; 別のポート</span><span class="sxs-lookup"><span data-stu-id="19543-126">&ndash; Different port</span></span>

<span data-ttu-id="19543-127">Internet Explorer は、生成元を比較するときにポートを考慮しません。</span><span class="sxs-lookup"><span data-stu-id="19543-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="19543-128">名前付きポリシーとミドルウェアで CORS</span><span class="sxs-lookup"><span data-stu-id="19543-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="19543-129">CORS ミドルウェアは、クロス オリジン要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="19543-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="19543-130">次のコードでは、指定された配信元とアプリ全体に対して CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="19543-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="19543-131">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="19543-131">The preceding code:</span></span>

* <span data-ttu-id="19543-132">ポリシー名を設定"\_myAllowSpecificOrigins"。</span><span class="sxs-lookup"><span data-stu-id="19543-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="19543-133">ポリシーの名前は任意です。</span><span class="sxs-lookup"><span data-stu-id="19543-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="19543-134">呼び出し、<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>拡張メソッドは、CORS を使用します。</span><span class="sxs-lookup"><span data-stu-id="19543-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="19543-135">呼び出し<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>で、[ラムダ式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)します。</span><span class="sxs-lookup"><span data-stu-id="19543-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="19543-136">ラムダは、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> オブジェクトをとります。</span><span class="sxs-lookup"><span data-stu-id="19543-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="19543-137">[構成オプション](#cors-policy-options)など`WithOrigins`はこの記事の後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="19543-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="19543-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>メソッドの呼び出しは、アプリのサービス コンテナーにサービスの CORS を追加します。</span><span class="sxs-lookup"><span data-stu-id="19543-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="19543-139">詳細については、次を参照してください。 [CORS ポリシー オプション](#cpo)このドキュメントで。</span><span class="sxs-lookup"><span data-stu-id="19543-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="19543-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>メソッドは次のコードに示すように、メソッドを連結できます。</span><span class="sxs-lookup"><span data-stu-id="19543-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="19543-141">次の強調表示されたコードでは、CORS ミドルウェアを使用してすべてのアプリのエンドポイントに CORS ポリシーを適用します。</span><span class="sxs-lookup"><span data-stu-id="19543-141">The following highlighted code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

<span data-ttu-id="19543-142">参照してください[Razor ページ、コントローラ、およびアクション メソッドで CORS を有効にする](#ecors)アクション/ページ/コント ローラー レベルでの CORS ポリシーを適用します。</span><span class="sxs-lookup"><span data-stu-id="19543-142">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="19543-143">メモ:</span><span class="sxs-lookup"><span data-stu-id="19543-143">Note:</span></span>

* `UseCors` <span data-ttu-id="19543-144">前に呼び出す必要があります`UseMvc`します。</span><span class="sxs-lookup"><span data-stu-id="19543-144">must be called before `UseMvc`.</span></span>
* <span data-ttu-id="19543-145">URL にする必要があります**いない**末尾のスラッシュを含める (`/`)。</span><span class="sxs-lookup"><span data-stu-id="19543-145">The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="19543-146">URL が終了した場合は`/`、比較を返します`false`ヘッダーは返されません。</span><span class="sxs-lookup"><span data-stu-id="19543-146">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="19543-147">参照してください[テスト CORS](#test)については、上記のコードをテストします。</span><span class="sxs-lookup"><span data-stu-id="19543-147">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="19543-148">属性を持つ CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="19543-148">Enable CORS with attributes</span></span>

<span data-ttu-id="19543-149">[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性は、CORS をグローバルに適用する代替を提供します。</span><span class="sxs-lookup"><span data-stu-id="19543-149">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="19543-150">`[EnableCors]`属性は、すべてのエンドポイントではなく、選択した最後の点で CORS を使用します。</span><span class="sxs-lookup"><span data-stu-id="19543-150">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="19543-151">使用`[EnableCors]`既定のポリシーを指定して`[EnableCors("{Policy String}")]`ポリシーを指定します。</span><span class="sxs-lookup"><span data-stu-id="19543-151">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="19543-152">`[EnableCors]`に属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="19543-152">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="19543-153">Razor ページ</span><span class="sxs-lookup"><span data-stu-id="19543-153">Razor Page</span></span> `PageModel`
* <span data-ttu-id="19543-154">コントローラー</span><span class="sxs-lookup"><span data-stu-id="19543-154">Controller</span></span>
* <span data-ttu-id="19543-155">コント ローラー アクション メソッド</span><span class="sxs-lookup"><span data-stu-id="19543-155">Controller action method</span></span>

<span data-ttu-id="19543-156">コント ローラー/ページ-モデル/アクションとするさまざまなポリシーを適用することができます、`[EnableCors]`属性。</span><span class="sxs-lookup"><span data-stu-id="19543-156">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="19543-157">ときに、`[EnableCors]`属性をコント ローラー/ページ-モデル/アクション メソッドに適用されます、ミドルウェアで CORS が有効になっていると、両方のポリシーが適用されます。</span><span class="sxs-lookup"><span data-stu-id="19543-157">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="19543-158">ポリシーを組み合わせることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="19543-158">We recommend against combining policies.</span></span> <span data-ttu-id="19543-159">使用して、`[EnableCors]`属性または両方で同じアプリでは、ミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="19543-159">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="19543-160">次のコードでは、各メソッドに別のポリシーが適用されます。</span><span class="sxs-lookup"><span data-stu-id="19543-160">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="19543-161">次のコードは、CORS の既定のポリシーとという名前のポリシーを作成します`"AnotherPolicy"`:。</span><span class="sxs-lookup"><span data-stu-id="19543-161">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="19543-162">CORS を無効にします。</span><span class="sxs-lookup"><span data-stu-id="19543-162">Disable CORS</span></span>

<span data-ttu-id="19543-163">[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)属性は、コント ローラー/ページ-モデル/アクションに対して CORS を無効になります。</span><span class="sxs-lookup"><span data-stu-id="19543-163">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="19543-164">CORS ポリシー オプション</span><span class="sxs-lookup"><span data-stu-id="19543-164">CORS policy options</span></span>

<span data-ttu-id="19543-165">このセクションでは、CORS ポリシーで設定できるさまざまなオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="19543-165">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="19543-166">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="19543-167">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="19543-168">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="19543-169">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="19543-170">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="19543-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="19543-171">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> <span data-ttu-id="19543-172">呼び出される`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="19543-172">is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="19543-173">いくつかのオプションの読み取りをすると役立つ場合があります、 [CORS ではどのように動作](#how-cors)最初のセクションします。</span><span class="sxs-lookup"><span data-stu-id="19543-173">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="19543-174">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-174">Set the allowed origins</span></span>

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> <span data-ttu-id="19543-175">&ndash; 任意のスキームですべてのオリジンからの CORS 要求を許可 (`http`または`https`)。</span><span class="sxs-lookup"><span data-stu-id="19543-175">&ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> `AllowAnyOrigin` <span data-ttu-id="19543-176">安全でないため、*任意の web サイト*をアプリにクロスオリジン要求を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="19543-176">is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="19543-177">指定する`AllowAnyOrigin`と`AllowCredentials`構成は安全でないと、クロスサイト リクエスト フォージェリで発生することができます。</span><span class="sxs-lookup"><span data-stu-id="19543-177">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="19543-178">CORS サービスは、アプリが両方の方法で構成されている場合、CORS の無効な応答を返します。</span><span class="sxs-lookup"><span data-stu-id="19543-178">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="19543-179">指定する`AllowAnyOrigin`と`AllowCredentials`構成は安全でないと、クロスサイト リクエスト フォージェリで発生することができます。</span><span class="sxs-lookup"><span data-stu-id="19543-179">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="19543-180">セキュリティで保護されたアプリでは、クライアントを承認する必要があります自体と、サーバー リソースにアクセスする場合のオリジンの正確なリストを指定します。</span><span class="sxs-lookup"><span data-stu-id="19543-180">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

`AllowAnyOrigin` <span data-ttu-id="19543-181">プリフライト要求の影響と`Access-Control-Allow-Origin`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="19543-181">affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="19543-182">詳細については、次を参照してください。、[プレフライト要求](#preflight-requests)セクション。</span><span class="sxs-lookup"><span data-stu-id="19543-182">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="19543-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; セット、<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>配信元が許可されている場合に評価するときに構成されているワイルドカード ドメインに一致するオリジンを許可する関数として、ポリシーのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="19543-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="19543-184">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-184">Set the allowed HTTP methods</span></span>

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*><span data-ttu-id="19543-185">:</span><span class="sxs-lookup"><span data-stu-id="19543-185">:</span></span>

* <span data-ttu-id="19543-186">任意の HTTP メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="19543-186">Allows any HTTP method:</span></span>
* <span data-ttu-id="19543-187">プリフライト要求の影響と`Access-Control-Allow-Methods`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="19543-187">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="19543-188">詳細については、次を参照してください。、[プレフライト要求](#preflight-requests)セクション。</span><span class="sxs-lookup"><span data-stu-id="19543-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="19543-189">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-189">Set the allowed request headers</span></span>

<span data-ttu-id="19543-190">CORS 要求で送信される特定のヘッダーを許可するという*要求ヘッダーを作成する*、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>し、許可されたヘッダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="19543-190">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="19543-191">許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="19543-191">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="19543-192">この設定に影響を与えますプレフライト要求と`Access-Control-Request-Headers`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="19543-192">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="19543-193">詳細については、次を参照してください。、[プレフライト要求](#preflight-requests)セクション。</span><span class="sxs-lookup"><span data-stu-id="19543-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="19543-194">指定された特定のヘッダーに一致する CORS ミドルウェア ポリシー`WithHeaders`ヘッダーが送信されるときにのみ可能なは`Access-Control-Request-Headers`に記載されているヘッダーと正確に一致`WithHeaders`します。</span><span class="sxs-lookup"><span data-stu-id="19543-194">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="19543-195">たとえば、次のように構成されているアプリを検討してください。</span><span class="sxs-lookup"><span data-stu-id="19543-195">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="19543-196">CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求を拒否`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) の一覧にない`WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="19543-196">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="19543-197">アプリを返します、 *200 ok をクリック*応答が戻り、CORS ヘッダーを送信しません。</span><span class="sxs-lookup"><span data-stu-id="19543-197">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="19543-198">そのため、ブラウザーでは、クロス オリジン要求を試行しません。</span><span class="sxs-lookup"><span data-stu-id="19543-198">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="19543-199">CORS ミドルウェアで 4 つのヘッダーを常に許可する、 `Access-Control-Request-Headers` CorsPolicy.Headers で構成されている値に関係なく送信します。</span><span class="sxs-lookup"><span data-stu-id="19543-199">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="19543-200">このヘッダーの一覧は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="19543-200">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="19543-201">たとえば、次のように構成されているアプリを検討してください。</span><span class="sxs-lookup"><span data-stu-id="19543-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="19543-202">CORS ミドルウェアは、次の要求ヘッダーでプレフライト要求に正常に応答ため`Content-Language`は常にホワイト リストに登録します。</span><span class="sxs-lookup"><span data-stu-id="19543-202">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="19543-203">公開されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-203">Set the exposed response headers</span></span>

<span data-ttu-id="19543-204">既定では、ブラウザーはすべてのアプリに応答ヘッダーで公開されません。</span><span class="sxs-lookup"><span data-stu-id="19543-204">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="19543-205">詳細については、次を参照してください。 [W3C のクロス オリジン リソース共有 (用語集)。単純な応答ヘッダー](https://www.w3.org/TR/cors/#simple-response-header)します。</span><span class="sxs-lookup"><span data-stu-id="19543-205">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="19543-206">既定で使用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="19543-206">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="19543-207">CORS の仕様は、これらのヘッダーを呼び出す*単純な応答ヘッダー*します。</span><span class="sxs-lookup"><span data-stu-id="19543-207">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="19543-208">で他のヘッダーをアプリに使用できるように呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="19543-208">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="19543-209">クロス オリジン要求で資格情報</span><span class="sxs-lookup"><span data-stu-id="19543-209">Credentials in cross-origin requests</span></span>

<span data-ttu-id="19543-210">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="19543-210">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="19543-211">既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="19543-211">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="19543-212">Cookie および HTTP 認証方式は、資格情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="19543-212">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="19543-213">クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります`XMLHttpRequest.withCredentials`に`true`します。</span><span class="sxs-lookup"><span data-stu-id="19543-213">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="19543-214">使用して`XMLHttpRequest`直接。</span><span class="sxs-lookup"><span data-stu-id="19543-214">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="19543-215">JQuery を使用します。</span><span class="sxs-lookup"><span data-stu-id="19543-215">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="19543-216">使用して、 [API フェッチ](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="19543-216">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="19543-217">サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="19543-217">The server must allow the credentials.</span></span> <span data-ttu-id="19543-218">クロス オリジンの資格情報を許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="19543-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="19543-219">HTTP 応答が含まれる、`Access-Control-Allow-Credentials`ヘッダーで、サーバーでクロス オリジン要求の資格情報は、ブラウザーに指示します。</span><span class="sxs-lookup"><span data-stu-id="19543-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="19543-220">ブラウザーが資格情報を送信しますが、有効な応答を含まない`Access-Control-Allow-Credentials`ヘッダー、ブラウザーは、アプリへの応答を公開しないし、クロス オリジン要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="19543-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="19543-221">クロス オリジンの資格情報がセキュリティ上のリスクです。</span><span class="sxs-lookup"><span data-stu-id="19543-221">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="19543-222">別のドメインに web サイトでは、ユーザーの知識がなくても、ユーザーの代わりに、アプリにサインイン済みのユーザーの資格情報を送信できます。</span><span class="sxs-lookup"><span data-stu-id="19543-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="19543-223">CORS の仕様もその設定を示すオリジンを`"*"`(すべてのオリジン) 有効でない場合、`Access-Control-Allow-Credentials`ヘッダーが存在します。</span><span class="sxs-lookup"><span data-stu-id="19543-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="19543-224">プレフライト要求</span><span class="sxs-lookup"><span data-stu-id="19543-224">Preflight requests</span></span>

<span data-ttu-id="19543-225">一部の CORS 要求では、ブラウザーは、実際の要求を行う前に、追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="19543-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="19543-226">この要求と呼ばれる、*プレフライト要求*します。</span><span class="sxs-lookup"><span data-stu-id="19543-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="19543-227">次の条件に該当する場合、ブラウザーでプレフライト要求をスキップできます。</span><span class="sxs-lookup"><span data-stu-id="19543-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="19543-228">要求メソッドは、GET、HEAD、または POST です。</span><span class="sxs-lookup"><span data-stu-id="19543-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="19543-229">アプリが要求ヘッダー以外に設定されていない`Accept`、 `Accept-Language`、 `Content-Language`、 `Content-Type`、または`Last-Event-ID`します。</span><span class="sxs-lookup"><span data-stu-id="19543-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="19543-230">`Content-Type`ヘッダー場合、次の値のいずれか。</span><span class="sxs-lookup"><span data-stu-id="19543-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="19543-231">要求ヘッダーでルール セットのクライアント要求を呼び出すことによって、アプリを設定するヘッダーに適用の`setRequestHeader`上、`XMLHttpRequest`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="19543-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="19543-232">CORS の仕様は、これらのヘッダーを呼び出す*要求ヘッダーを作成する*します。</span><span class="sxs-lookup"><span data-stu-id="19543-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="19543-233">ヘッダーは、ブラウザー設定できるように、ルールは適用されません`User-Agent`、 `Host`、または`Content-Length`します。</span><span class="sxs-lookup"><span data-stu-id="19543-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="19543-234">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="19543-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="19543-235">事前要求は HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="19543-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="19543-236">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="19543-236">It includes two special headers:</span></span>

* `Access-Control-Request-Method`<span data-ttu-id="19543-237">:実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="19543-237">: The HTTP method that will be used for the actual request.</span></span>
* `Access-Control-Request-Headers`<span data-ttu-id="19543-238">:実際の要求にアプリを設定する要求ヘッダーの一覧。</span><span class="sxs-lookup"><span data-stu-id="19543-238">: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="19543-239">前述のように、ブラウザー設定などのヘッダーは含まれません`User-Agent`します。</span><span class="sxs-lookup"><span data-stu-id="19543-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="19543-240">CORS プレフライト要求を含めることができます、`Access-Control-Request-Headers`ヘッダーで、サーバーの実際の要求で送信されるヘッダーを示します。</span><span class="sxs-lookup"><span data-stu-id="19543-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="19543-241">特定のヘッダーを許可するのには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="19543-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="19543-242">許可するのには、すべての著者要求ヘッダー、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="19543-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="19543-243">ブラウザーはどのように設定でまったく一貫性のある`Access-Control-Request-Headers`します。</span><span class="sxs-lookup"><span data-stu-id="19543-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="19543-244">以外に何もヘッダーを設定する場合`"*"`(を使用して、または<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)、以上含める必要がある`Accept`、 `Content-Type`、および`Origin`、さらにサポートするカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="19543-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="19543-245">(サーバーが要求を許可することを想定) プレフライト要求に応答の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="19543-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="19543-246">応答が含まれています、`Access-Control-Allow-Methods`ヘッダーを許可されるメソッドを一覧表示して、必要に応じて、`Access-Control-Allow-Headers`ヘッダーで、許可されたヘッダーを一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="19543-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="19543-247">プレフライト要求が成功すると、ブラウザーは、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="19543-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="19543-248">アプリが返したプレフライト要求が拒否された場合、 *200 OK*応答戻る、CORS ヘッダーを送信しないが。</span><span class="sxs-lookup"><span data-stu-id="19543-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="19543-249">そのため、ブラウザーでは、クロス オリジン要求を試行しません。</span><span class="sxs-lookup"><span data-stu-id="19543-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="19543-250">プレフライトの有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-250">Set the preflight expiration time</span></span>

<span data-ttu-id="19543-251">`Access-Control-Max-Age`ヘッダーは、プレフライト要求に応答をキャッシュできる期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="19543-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="19543-252">このヘッダーを設定するには、呼び出す<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="19543-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="19543-253">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="19543-253">How CORS works</span></span>

<span data-ttu-id="19543-254">このセクションでの動作について説明します、 [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) HTTP メッセージのレベルで要求します。</span><span class="sxs-lookup"><span data-stu-id="19543-254">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="19543-255">CORS は**いない**セキュリティ機能。</span><span class="sxs-lookup"><span data-stu-id="19543-255">CORS is **not** a security feature.</span></span> <span data-ttu-id="19543-256">CORS は、サーバーによる同一オリジン ポリシーの緩和にできる W3C 標準です。</span><span class="sxs-lookup"><span data-stu-id="19543-256">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="19543-257">たとえば、悪意のあるアクターを使用[ようにクロス サイト スクリプティング (XSS)](xref:security/cross-site-scripting)サイトに対して、CORS が有効になっているサイトに情報の盗用のサイト間で要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="19543-257">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="19543-258">CORS を許可することで、API をより安全なすることはできません。</span><span class="sxs-lookup"><span data-stu-id="19543-258">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="19543-259">クライアント (ブラウザー) に CORS を適用します。</span><span class="sxs-lookup"><span data-stu-id="19543-259">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="19543-260">サーバーが要求を実行し、応答を返します、それがクライアントでエラーとブロックの応答を返します。</span><span class="sxs-lookup"><span data-stu-id="19543-260">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="19543-261">たとえば、次のツールのいずれかのサーバーの応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="19543-261">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="19543-262">Fiddler</span><span class="sxs-lookup"><span data-stu-id="19543-262">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="19543-263">Postman</span><span class="sxs-lookup"><span data-stu-id="19543-263">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="19543-264">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="19543-264">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="19543-265">アドレス バーに URL を入力して、web ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="19543-265">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="19543-266">クロス オリジンを実行するには、サーバーがブラウザーを許可する方法は[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)または[フェッチ API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)それ以外の場合、禁止されるよう要求します。</span><span class="sxs-lookup"><span data-stu-id="19543-266">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="19543-267">(CORS) なしのブラウザーでは、クロス オリジン要求を実行できません。</span><span class="sxs-lookup"><span data-stu-id="19543-267">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="19543-268">CORS をする前に[JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)この制限を回避するために使用されました。</span><span class="sxs-lookup"><span data-stu-id="19543-268">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="19543-269">JSONP は、XHR を使用していないを使用して、`<script>`応答を受信するタグ。</span><span class="sxs-lookup"><span data-stu-id="19543-269">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="19543-270">スクリプトが読み込まれたクロス オリジンが許可されます。</span><span class="sxs-lookup"><span data-stu-id="19543-270">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="19543-271">[CORS の仕様](https://www.w3.org/TR/cors/)のクロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されました。</span><span class="sxs-lookup"><span data-stu-id="19543-271">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="19543-272">ブラウザーでは、CORS をサポートする場合は、クロス オリジン要求を自動的にこれらのヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="19543-272">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="19543-273">カスタム JavaScript コードは、CORS を有効にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="19543-273">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="19543-274">次は、クロス オリジン要求の例です。</span><span class="sxs-lookup"><span data-stu-id="19543-274">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="19543-275">`Origin`ヘッダーは要求を行っているサイトのドメインを提供します。</span><span class="sxs-lookup"><span data-stu-id="19543-275">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="19543-276">設定されているサーバーでは、要求を許可している場合、`Access-Control-Allow-Origin`応答のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="19543-276">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="19543-277">このヘッダーの値と一致するか、`Origin`要求からヘッダーまたはワイルドカード値`"*"`、任意のオリジンを許可することを意味します。</span><span class="sxs-lookup"><span data-stu-id="19543-277">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="19543-278">応答に含まれていない場合、`Access-Control-Allow-Origin`ヘッダー、クロス オリジン要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="19543-278">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="19543-279">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="19543-279">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="19543-280">サーバーに正常な応答が返される場合でも、ブラウザーは、応答を使用できるようにクライアント アプリ。</span><span class="sxs-lookup"><span data-stu-id="19543-280">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="19543-281">CORS をテストします。</span><span class="sxs-lookup"><span data-stu-id="19543-281">Test CORS</span></span>

<span data-ttu-id="19543-282">CORS をテストします。</span><span class="sxs-lookup"><span data-stu-id="19543-282">To test CORS:</span></span>

1. <span data-ttu-id="19543-283">[API プロジェクトを作成](xref:tutorials/first-web-api)です。</span><span class="sxs-lookup"><span data-stu-id="19543-283">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="19543-284">または、[サンプルをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors)します。</span><span class="sxs-lookup"><span data-stu-id="19543-284">Alternatively, you can [download the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="19543-285">このドキュメントで、アプローチの 1 つを使用して、CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="19543-285">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="19543-286">例えば:</span><span class="sxs-lookup"><span data-stu-id="19543-286">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` <span data-ttu-id="19543-287">ようなサンプル アプリをテストするためにのみ使用する必要があります、[サンプル コードをダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors)します。</span><span class="sxs-lookup"><span data-stu-id="19543-287">should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="19543-288">(Razor ページまたは MVC) web アプリ プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="19543-288">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="19543-289">このサンプルでは、Razor ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="19543-289">The sample uses Razor Pages.</span></span> <span data-ttu-id="19543-290">API プロジェクトと同じソリューションでは、web アプリを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="19543-290">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="19543-291">次の強調表示されたコードを追加、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="19543-291">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="19543-292">上記のコードで置き換えます`url: 'https://<web app>.azurewebsites.net/api/values/1',`でデプロイされたアプリの URL。</span><span class="sxs-lookup"><span data-stu-id="19543-292">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="19543-293">API プロジェクトをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="19543-293">Deploy the API project.</span></span> <span data-ttu-id="19543-294">たとえば、[を Azure にデプロイ](xref:host-and-deploy/azure-apps/index)します。</span><span class="sxs-lookup"><span data-stu-id="19543-294">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="19543-295">デスクトップから、Razor ページまたは MVC アプリを実行し、をクリックして、**テスト**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="19543-295">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="19543-296">F12 ツールを使用して、エラー メッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="19543-296">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="19543-297">Localhost 原点からの削除`WithOrigins`と、アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="19543-297">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="19543-298">または、別のポートでクライアント アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="19543-298">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="19543-299">たとえば、Visual Studio から実行します。</span><span class="sxs-lookup"><span data-stu-id="19543-299">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="19543-300">クライアント アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="19543-300">Test with the client app.</span></span> <span data-ttu-id="19543-301">CORS の障害には、エラーが返されますが、エラー メッセージは JavaScript を使用できません。</span><span class="sxs-lookup"><span data-stu-id="19543-301">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="19543-302">F12 ツールで、[コンソール] タブを使用して、エラーを参照してください。</span><span class="sxs-lookup"><span data-stu-id="19543-302">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="19543-303">ブラウザーによっては、次のような (F12 ツールのコンソール) で、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="19543-303">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="19543-304">Microsoft Edge の使用。</span><span class="sxs-lookup"><span data-stu-id="19543-304">Using Microsoft Edge:</span></span>

     **<span data-ttu-id="19543-305">SEC7120: [CORS]、配信元`https://localhost:44375`が見つかりませんでした`https://localhost:44375`でクロス オリジン リソースへのアクセス制御の許可-オリジン応答ヘッダー</span><span class="sxs-lookup"><span data-stu-id="19543-305">SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at</span></span> `https://webapi.azurewebsites.net/api/values/1`**

   * <span data-ttu-id="19543-306">Chrome を使用します。</span><span class="sxs-lookup"><span data-stu-id="19543-306">Using Chrome:</span></span>

     **<span data-ttu-id="19543-307">XMLHttpRequest へのアクセス`https://webapi.azurewebsites.net/api/values/1`配信元から`https://localhost:44375`CORS ポリシーによってブロックされています。'へのアクセス制御の許可-オリジン' ヘッダーが要求されたリソースに存在しません。</span><span class="sxs-lookup"><span data-stu-id="19543-307">Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span>**

## <a name="additional-resources"></a><span data-ttu-id="19543-308">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="19543-308">Additional resources</span></span>

* [<span data-ttu-id="19543-309">クロス オリジン リソース共有 (CORS)</span><span class="sxs-lookup"><span data-stu-id="19543-309">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
