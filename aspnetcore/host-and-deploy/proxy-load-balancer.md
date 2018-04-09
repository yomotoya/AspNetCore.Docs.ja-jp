---
title: プロキシ サーバーを操作して、ロード バランサーに ASP.NET Core の構成します。
author: guardrex
description: プロキシ サーバーと多くの場合、要求の重要な情報がわかりにくくなるは、ロード バランサーの背後にホストされているアプリの構成について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="0e720-103">プロキシ サーバーを操作して、ロード バランサーに ASP.NET Core の構成します。</span><span class="sxs-lookup"><span data-stu-id="0e720-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="0e720-104">によって[Luke Latham](https://github.com/guardrex)と[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="0e720-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="0e720-105">ASP.NET Core の推奨構成で、アプリは IIS/ASP.NET コア モジュールや Nginx、Apache を使用してホストされます。</span><span class="sxs-lookup"><span data-stu-id="0e720-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="0e720-106">プロキシ サーバー、ロード バランサー、およびその他のネットワーク アプライアンス、アプリケーションに到達する前に多くの場合、要求に関する情報を表面化しません。</span><span class="sxs-lookup"><span data-stu-id="0e720-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="0e720-107">HTTPS 要求は、over HTTP プロキシが、元の設定 (HTTPS) が失われ、ヘッダーで転送する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e720-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="0e720-108">アプリは、プロキシおよびその場合は true。 ソース インターネットまたは社内ネットワーク上ではなくから要求を受信するため元のクライアント IP アドレスにヘッダーに転送ことも必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e720-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="0e720-109">この情報は、要求の処理、たとえばリダイレクト、認証、リンクの生成、ポリシーの評価、およびクライアント geoloation で重要になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0e720-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="0e720-110">転送されたヘッダー</span><span class="sxs-lookup"><span data-stu-id="0e720-110">Forwarded headers</span></span>

<span data-ttu-id="0e720-111">慣例によりは、プロキシは、HTTP ヘッダーの情報を転送します。</span><span class="sxs-lookup"><span data-stu-id="0e720-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="0e720-112">Header</span><span class="sxs-lookup"><span data-stu-id="0e720-112">Header</span></span> | <span data-ttu-id="0e720-113">説明</span><span class="sxs-lookup"><span data-stu-id="0e720-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="0e720-114">X-Forwarded-For</span><span class="sxs-lookup"><span data-stu-id="0e720-114">X-Forwarded-For</span></span> | <span data-ttu-id="0e720-115">要求とプロキシのチェーン内の後続のプロキシを開始したクライアントに関する情報を保持します。</span><span class="sxs-lookup"><span data-stu-id="0e720-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="0e720-116">このパラメーターには、IP アドレス (および、必要に応じて、ポート番号) を含めることがあります。</span><span class="sxs-lookup"><span data-stu-id="0e720-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="0e720-117">プロキシ サーバーのチェーンには、最初のパラメーターは、要求が行われた最初のクライアントを示します。</span><span class="sxs-lookup"><span data-stu-id="0e720-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="0e720-118">後続のプロキシの識別子に従ってください。</span><span class="sxs-lookup"><span data-stu-id="0e720-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="0e720-119">パラメーターの一覧では、チェーン内の最後のプロキシがありません。</span><span class="sxs-lookup"><span data-stu-id="0e720-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="0e720-120">最後のプロキシの IP アドレス、および必要に応じて、ポート番号は、トランスポート層でのリモートの IP アドレスとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="0e720-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="0e720-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="0e720-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="0e720-122">発信元スキーム (HTTP または HTTPS) の値。</span><span class="sxs-lookup"><span data-stu-id="0e720-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="0e720-123">値場合もありますスキームの一覧要求が複数のプロキシを通過します。</span><span class="sxs-lookup"><span data-stu-id="0e720-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="0e720-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="0e720-124">X-Forwarded-Host</span></span> | <span data-ttu-id="0e720-125">ホスト ヘッダー フィールドの元の値。</span><span class="sxs-lookup"><span data-stu-id="0e720-125">The original value of the Host header field.</span></span> <span data-ttu-id="0e720-126">通常、プロキシでは、ホスト ヘッダーを変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="0e720-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="0e720-127">参照してください[マイクロソフト セキュリティ アドバイザリ CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295)プロキシを検証しませんシステムに影響を与える権限昇格の脆弱性や既知の適切な値に restict ホスト ヘッダーの詳細についてです。</span><span class="sxs-lookup"><span data-stu-id="0e720-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="0e720-128">ヘッダーの転送ミドルウェアから、 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)パッケージ化、これらのヘッダーを読み取り、および関連付けられているフィールドに入力[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="0e720-129">ミドルウェアの更新:</span><span class="sxs-lookup"><span data-stu-id="0e720-129">The middleware updates:</span></span>

* <span data-ttu-id="0e720-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span><span class="sxs-lookup"><span data-stu-id="0e720-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="0e720-131">ミドルウェアを設定する方法に影響を与える追加設定`RemoteIpAddress`です。</span><span class="sxs-lookup"><span data-stu-id="0e720-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="0e720-132">詳細については、次を参照してください。、[転送ヘッダー ミドルウェアのオプション](#forwarded-headers-middleware-options)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="0e720-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash;設定を使用して、`X-Forwarded-Proto`ヘッダーの値。</span><span class="sxs-lookup"><span data-stu-id="0e720-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="0e720-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash;設定を使用して、`X-Forwarded-Host`ヘッダーの値。</span><span class="sxs-lookup"><span data-stu-id="0e720-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="0e720-135">すべてのネットワーク アプライアンスの追加、`X-Forwarded-For`と`X-Forwarded-Proto`追加構成なしヘッダー。</span><span class="sxs-lookup"><span data-stu-id="0e720-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="0e720-136">アプリに到達したときは、プロキシの要求にこれらのヘッダーが含まれていない場合は、アプライアンスの製造元のガイダンスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0e720-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="0e720-137">ヘッダーのミドルウェアを転送[既定の設定](#forwarded-headers-middleware-options)ように構成できます。</span><span class="sxs-lookup"><span data-stu-id="0e720-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="0e720-138">既定の設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="0e720-138">The default settings are:</span></span>

* <span data-ttu-id="0e720-139">のみがある*1 つのプロキシ*アプリケーションと、要求のソースの間です。</span><span class="sxs-lookup"><span data-stu-id="0e720-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="0e720-140">ループバック アドレスのみが既知のプロキシ用に構成し、既知のネットワーク。</span><span class="sxs-lookup"><span data-stu-id="0e720-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="0e720-141">IIS または IIS Express および ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="0e720-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="0e720-142">IIS および ASP.NET Core モジュールの背後にあるアプリの実行時に、転送されたヘッダーのミドルウェアを IIS 統合ミドルウェアによって既定で有効です。</span><span class="sxs-lookup"><span data-stu-id="0e720-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="0e720-143">転送されたヘッダーのミドルウェアが最初に実行する特定の制限の構成のミドルウェア パイプラインで転送されたヘッダーを含む信頼上の問題により、ASP.NET のコア モジュールにアクティブ化 (たとえば、 [IP スプーフィング](https://www.iplocation.net/ip-spoofing))。</span><span class="sxs-lookup"><span data-stu-id="0e720-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="0e720-144">ミドルウェアの転送が構成されて、`X-Forwarded-For`と`X-Forwarded-Proto`ヘッダーとが 1 つの localhost プロキシに制限します。</span><span class="sxs-lookup"><span data-stu-id="0e720-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="0e720-145">追加の構成が必要な場合を参照してください、[転送ヘッダー ミドルウェアのオプション](#forwarded-headers-middleware-options)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0e720-146">その他のプロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="0e720-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0e720-147">IIS 統合ミドルウェアを使用して、外部ヘッダー ミドルウェアの転送は、既定で有効になっていません。</span><span class="sxs-lookup"><span data-stu-id="0e720-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="0e720-148">アプリのヘッダーの転送処理に転送されたヘッダーのミドルウェアを有効にする必要があります[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="0e720-149">ない場合は、ミドルウェアを有効にした後[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)ミドルウェア、既定値に指定された[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)は[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="0e720-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="0e720-150">ミドルウェアを構成する[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)を転送する、`X-Forwarded-For`と`X-Forwarded-Proto`でヘッダー`Startup.ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="0e720-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0e720-151">呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`他のミドルウェアを呼び出す前に。</span><span class="sxs-lookup"><span data-stu-id="0e720-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="0e720-152">ない場合は[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)で指定された`Startup.ConfigureServices`または使用して拡張メソッドを直接[UseForwardedHeaders (IApplicationBuilder、ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_)、既定値転送するヘッダーが[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="0e720-153">[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)を転送するヘッダーのプロパティを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e720-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="0e720-154">転送されたヘッダーのミドルウェアのオプション</span><span class="sxs-lookup"><span data-stu-id="0e720-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="0e720-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)ヘッダーの転送ミドルウェアの動作を制御します。</span><span class="sxs-lookup"><span data-stu-id="0e720-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="0e720-156">オプション</span><span class="sxs-lookup"><span data-stu-id="0e720-156">Option</span></span> | <span data-ttu-id="0e720-157">説明</span><span class="sxs-lookup"><span data-stu-id="0e720-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="0e720-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="0e720-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="0e720-159">このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="0e720-160">既定値は、`X-Forwarded-For` です。</span><span class="sxs-lookup"><span data-stu-id="0e720-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="0e720-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="0e720-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="0e720-162">どのフォワーダーを処理するかを識別します。</span><span class="sxs-lookup"><span data-stu-id="0e720-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="0e720-163">参照してください、 [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)適用されるフィールドの一覧についてはします。</span><span class="sxs-lookup"><span data-stu-id="0e720-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="0e720-164">このプロパティに割り当てられている標準値は<code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>します。</span><span class="sxs-lookup"><span data-stu-id="0e720-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="0e720-165">既定値は[ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="0e720-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="0e720-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="0e720-167">このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="0e720-168">既定値は、`X-Forwarded-Host` です。</span><span class="sxs-lookup"><span data-stu-id="0e720-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="0e720-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="0e720-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="0e720-170">このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="0e720-171">既定値は、`X-Forwarded-Proto` です。</span><span class="sxs-lookup"><span data-stu-id="0e720-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="0e720-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="0e720-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="0e720-173">処理されるヘッダー内のエントリの数を制限します。</span><span class="sxs-lookup"><span data-stu-id="0e720-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="0e720-174">設定`null`、制限が、これを無効にする必要がある場合にのみ実行`KnownProxies`または`KnownNetworks`が構成されています。</span><span class="sxs-lookup"><span data-stu-id="0e720-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="0e720-175">既定値は 1 です。</span><span class="sxs-lookup"><span data-stu-id="0e720-175">The default is 1.</span></span> |
| [<span data-ttu-id="0e720-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="0e720-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="0e720-177">アドレスから転送されたヘッダーをそのまま使用する既知のプロキシの範囲です。</span><span class="sxs-lookup"><span data-stu-id="0e720-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="0e720-178">クラスレス ドメイン間ルーティング (CIDR) 表記を使用して IP の範囲を提供します。</span><span class="sxs-lookup"><span data-stu-id="0e720-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="0e720-179">既定値は、 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[ip ネットワーク](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> の 1 つのエントリを含む`IPAddress.Loopback`です。</span><span class="sxs-lookup"><span data-stu-id="0e720-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="0e720-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="0e720-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="0e720-181">転送されたヘッダーをそのまま使用する既知のプロキシのアドレス。</span><span class="sxs-lookup"><span data-stu-id="0e720-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="0e720-182">使用する`KnownProxies`と一致する正確な IP アドレスを指定します。</span><span class="sxs-lookup"><span data-stu-id="0e720-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="0e720-183">既定値は、 [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> の 1 つのエントリを含む`IPAddress.IPv6Loopback`です。</span><span class="sxs-lookup"><span data-stu-id="0e720-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="0e720-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="0e720-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="0e720-185">このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="0e720-186">既定値は、`X-Original-For` です。</span><span class="sxs-lookup"><span data-stu-id="0e720-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="0e720-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="0e720-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="0e720-188">このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="0e720-189">既定値は、`X-Original-Host` です。</span><span class="sxs-lookup"><span data-stu-id="0e720-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="0e720-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="0e720-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="0e720-191">このプロパティで指定された 1 つではなくで指定されたヘッダーを使用して[ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="0e720-192">既定値は、`X-Original-Proto` です。</span><span class="sxs-lookup"><span data-stu-id="0e720-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="0e720-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="0e720-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="0e720-194">間に同期するヘッダーの値の数が必要、 [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders)処理中です。</span><span class="sxs-lookup"><span data-stu-id="0e720-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="0e720-195">既定の ASP.NET Core 1.x は`true`します。</span><span class="sxs-lookup"><span data-stu-id="0e720-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="0e720-196">ASP.NET Core 2.0 またはそれ以降の既定値は`false`します。</span><span class="sxs-lookup"><span data-stu-id="0e720-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="0e720-197">シナリオとユース ケース</span><span class="sxs-lookup"><span data-stu-id="0e720-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="0e720-198">ヘッダーとすべての要求はセキュリティで保護されたときに、転送を追加することはできません。</span><span class="sxs-lookup"><span data-stu-id="0e720-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="0e720-199">場合によっては、要求をアプリにプロキシに転送されたヘッダーを追加できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="0e720-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="0e720-200">スキームを手動で設定場合は、プロキシを適用するには、すべてのパブリック外部要求が HTTPS である、`Startup.Configure`ミドルウェアの任意の型を使用する前に。</span><span class="sxs-lookup"><span data-stu-id="0e720-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="0e720-201">環境変数と、開発環境またはステージング環境の他の構成設定は、このコードを無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="0e720-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="0e720-202">パスのベースと要求のパスを変更するプロキシを処理します。</span><span class="sxs-lookup"><span data-stu-id="0e720-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="0e720-203">一部のプロキシが、パスをそのままの渡しますが、アプリにルーティングされるように削除する必要のある基本パスが正しく機能します。</span><span class="sxs-lookup"><span data-stu-id="0e720-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="0e720-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase)ミドルウェアにパスを分割する[HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)とに、アプリの基本パス[HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="0e720-205">場合`/foo`として渡されるプロキシ パスのアプリケーション ベース パスは、 `/foo/api/1`、ミドルウェア セット`Request.PathBase`に`/foo`と`Request.Path`に`/api/1`次のコマンド。</span><span class="sxs-lookup"><span data-stu-id="0e720-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="0e720-206">ミドルウェアが逆方向にもう一度呼び出されると、元のパスと基本パスが再適用されます。</span><span class="sxs-lookup"><span data-stu-id="0e720-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="0e720-207">ミドルウェアの注文処理の詳細については、次を参照してください。[ミドルウェア](xref:fundamentals/middleware/index)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="0e720-208">プロキシは、パスを削除します。 場合 (たとえば、転送`/foo/api/1`に`/api/1`)、修正プログラムを選択し、リダイレクトするには、要求のリンク[PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="0e720-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="0e720-209">プロキシは、パスのデータを追加するを使用してリダイレクトとのリンクを解決するパスの一部を破棄して[StartsWithSegments (PathString、PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__)に割り当てると、[パス](/dotnet/api/microsoft.aspnetcore.http.httprequest.path)プロパティ。</span><span class="sxs-lookup"><span data-stu-id="0e720-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a><span data-ttu-id="0e720-210">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="0e720-210">Troubleshoot</span></span>

<span data-ttu-id="0e720-211">ヘッダーは、期待どおりに転送されない、ときに有効に[ログ](xref:fundamentals/logging/index)です。</span><span class="sxs-lookup"><span data-stu-id="0e720-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="0e720-212">ログが問題を解決するための十分な情報を提供されない場合は、サーバーが受信した要求ヘッダーを列挙します。</span><span class="sxs-lookup"><span data-stu-id="0e720-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="0e720-213">ヘッダーは、インラインのミドルウェアを使用して、アプリの応答に記述できます。</span><span class="sxs-lookup"><span data-stu-id="0e720-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

<span data-ttu-id="0e720-214">転送の X を確認してください \* 予期される値を持つサーバー ヘッダーが受信します。</span><span class="sxs-lookup"><span data-stu-id="0e720-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="0e720-215">指定されたヘッダーに複数の値がある場合は、逆の順序で右から左へヘッダー ミドルウェアの転送プロセスのヘッダーに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e720-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="0e720-216">要求の元のリモート IP のエントリに一致する必要があります、`KnownProxies`または`KnownNetworks`X-転送の場合は、処理される前に一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="0e720-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="0e720-217">これは、信頼されていないプロキシのフォワーダを受け付けないことでヘッダーのスプーフィングを制限します。</span><span class="sxs-lookup"><span data-stu-id="0e720-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e720-218">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0e720-218">Additional resources</span></span>

* [<span data-ttu-id="0e720-219">マイクロソフト セキュリティ アドバイザリ CVE-2018-0787: ASP.NET Core の昇格の脆弱性</span><span class="sxs-lookup"><span data-stu-id="0e720-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
