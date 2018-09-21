---
title: ASP.NET Core での HTTPS を適用します。
author: rick-anderson
description: Web アプリに ASP.NET Core では、HTTPS や TLS を必要とする方法を示します。
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 6e16191b1a4627e683fd2281e5556b2a6e84c082
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523145"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="d7aa7-103">ASP.NET Core での HTTPS を適用します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="d7aa7-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d7aa7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d7aa7-105">このドキュメントでは次の方法について説明します:</span><span class="sxs-lookup"><span data-stu-id="d7aa7-105">This document shows how to:</span></span>

* <span data-ttu-id="d7aa7-106">すべての要求に HTTPS を必要とさせる。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="d7aa7-107">すべての HTTP 要求を HTTPS にリダイレクトさせる。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="d7aa7-108">API ようにするありませんクライアントの最初の要求で機密データを送信します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="d7aa7-109">**いない**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)機密情報を受け取る Web Api で。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="d7aa7-110">`RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="d7aa7-111">API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="d7aa7-112">このようなクライアントは、HTTP 経由で情報を送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="d7aa7-113">Web API は次のいずれかの対策を講じるべきです:</span><span class="sxs-lookup"><span data-stu-id="d7aa7-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="d7aa7-114">HTTP をリッスンしない。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="d7aa7-115">ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。
</span><span class="sxs-lookup"><span data-stu-id="d7aa7-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="d7aa7-116">HTTPS が必要です。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d7aa7-117">Web アプリの呼び出しすべての実稼働 ASP.NET Core を推奨します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="d7aa7-118">HTTPS のリダイレクトのミドルウェア ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="d7aa7-119">[UseHsts](#hsts)、HTTP Strict Transport Security プロトコル (HSTS)。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="d7aa7-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="d7aa7-120">UseHttpsRedirection</span></span>

<span data-ttu-id="d7aa7-121">次のコード呼び出し`UseHttpsRedirection`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="d7aa7-122">上記の強調表示されたコード。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="d7aa7-123">既定値を使用して[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`)。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="d7aa7-124">既定値を使用して[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport)によってオーバーライドされない限り、(null)、`ASPNETCORE_HTTPS_PORT`環境変数または[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="d7aa7-125">ポートは、HTTPS にリダイレクトするミドルウェアの使用可能なである必要があります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="d7aa7-126">ポートが使用できない場合は、HTTPS へのリダイレクトは発生しません。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="d7aa7-127">HTTPS ポートは、次の設定のいずれかで指定できます。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="d7aa7-128">`ASPNETCORE_HTTPS_PORT`環境変数。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="d7aa7-129">開発では、HTTPS url で*launchsettings.json*します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="d7aa7-130">Kestrel または HttpSys で直接構成されている HTTPS url です。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="d7aa7-131">次のコードの呼び出しを強調表示されている[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)ミドルウェアのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="d7aa7-132">呼び出す`AddHttpsRedirection`の値を変更するだけで済みます` HttpsPort`または` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="d7aa7-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="d7aa7-133">上記の強調表示されたコード。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="d7aa7-134">セット[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)に`Status307TemporaryRedirect`、これは、既定値。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="d7aa7-135">HTTPS ポートを 5001 に設定します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="d7aa7-136">既定値には 443 です。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-136">The default value is 443.</span></span>

<span data-ttu-id="d7aa7-137">次のメカニズムでは、自動的にポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="d7aa7-138">ミドルウェアを使用したポートを検出できる[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)次の条件が適用されます。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="d7aa7-139">Kestrel または HTTP.sys を使用して HTTPS エンドポイントを直接持つ (Visual Studio Code のデバッガーで、アプリを実行中にも適用されます)。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="d7aa7-140">のみ**1 つの HTTPS ポート**アプリによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="d7aa7-141">Visual Studio が使用されます。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="d7aa7-142">IIS Express には、HTTPS を有効になっているがあります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="d7aa7-143">*launchSettings.json*設定、 `sslPort` IIS Express 用です。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="d7aa7-144">アプリを実行したリバース プロキシ (たとえば、IIS、IIS Express) の背後にあるときに`IServerAddressesFeature`は使用できません。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="d7aa7-145">ポートを手動で構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-145">The port must be manually configured.</span></span> <span data-ttu-id="d7aa7-146">ポートが設定されていないときに要求をリダイレクトされません。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="d7aa7-147">設定して、ポートを構成することができます、 [https_port Web ホストの構成設定](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="d7aa7-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="d7aa7-148">**キー**: https_port</span><span class="sxs-lookup"><span data-stu-id="d7aa7-148">**Key**: https_port</span></span>  
<span data-ttu-id="d7aa7-149">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="d7aa7-149">**Type**: *string*</span></span>  
<span data-ttu-id="d7aa7-150">**既定**: 既定値が設定されていません。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="d7aa7-151">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d7aa7-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d7aa7-152">**環境変数**: `<PREFIX_>HTTPS_PORT` (プレフィックスは`ASPNETCORE_`Web ホストを使用する場合)。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="d7aa7-153">ポート構成できます直接の URL を設定して、`ASPNETCORE_URLS`環境変数。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="d7aa7-154">環境変数は、サーバーを構成し、ミドルウェア直接を検出しない経由で HTTPS ポート`IServerAddressesFeature`します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="d7aa7-155">ポートが設定されていない: 場合</span><span class="sxs-lookup"><span data-stu-id="d7aa7-155">If no port is set:</span></span>

* <span data-ttu-id="d7aa7-156">要求はリダイレクトされません。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="d7aa7-157">ミドルウェア「をリダイレクト用 https ポートを特定できませんでした」警告をログします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="d7aa7-158">HTTPS のリダイレクトのミドルウェアを使用する代わりに (`UseHttpsRedirection`) は、URL リライト ミドルウェアを使用する (`AddRedirectToHttps`)。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="d7aa7-159">`AddRedirectToHttps` 設定こともできます、ステータス コードとポートのリダイレクトを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="d7aa7-160">さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="d7aa7-161">HTTPS リダイレクト ミドルウェアの使用をお勧めときに、追加のリダイレクト ルールを必要としないを HTTPS にリダイレクトする、(`UseHttpsRedirection`) このトピックで説明します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="d7aa7-162">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) は HTTPS を必須とさせるために使用します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="d7aa7-163">`[RequireHttpsAttribute]` はメソッドまたはコントローラーを装飾するか、またはグローバルに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="d7aa7-164">属性をグローバルに適用するには、`ConfigureServices` の `Startup` に次のコードを追加します:</span><span class="sxs-lookup"><span data-stu-id="d7aa7-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="d7aa7-165">上記の強調表示されたコードでは、すべての要求を使用して、必要があります`HTTPS`。 したがって、HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="d7aa7-166">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="d7aa7-167">さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="d7aa7-168">ミドルウェアは、アプリのリダイレクトを実行すると、状態コードまたは状態コードと、ポートを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="d7aa7-169">グローバルに HTTPS を必須とさせること (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="d7aa7-170">`[RequireHttps]`属性をすべてのコントローラー/Razor ページ に適用することは、グローバルに HTTPS を必須とさせることほど安全性が高いとは考えられていません。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="d7aa7-171">新しいコントローラー または Razor ページが追加されたときに `[RequireHttps]` 属性が適用されるとは限らないためです。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="d7aa7-172">HTTP Strict Transport Security プロトコル (HSTS)</span><span class="sxs-lookup"><span data-stu-id="d7aa7-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="d7aa7-173">あたり[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)、 [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)は応答ヘッダーを使用して web アプリによって指定されるオプトイン セキュリティ拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="d7aa7-174">ときに、 [HSTS をサポートするブラウザー](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)このヘッダーを受信します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="d7aa7-175">ブラウザーでは、HTTP 経由で通信を送信しないように、ドメインの構成を格納します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="d7aa7-176">ブラウザーでは、HTTPS 経由ですべての通信を強制します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="d7aa7-177">ブラウザーでは、ユーザーが信頼されていないか無効な証明書を使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="d7aa7-178">ブラウザーには、一時的にこのような証明書を信頼するユーザーを許可するプロンプトが無効にします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="d7aa7-179">HSTS は、クライアントで強制されるため、いくつかの制限があります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="d7aa7-180">クライアントは、HSTS をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-180">The client must support HSTS.</span></span>
* <span data-ttu-id="d7aa7-181">HSTS では、少なくとも 1 つ HSTS ポリシーを確立するために HTTPS 要求の成功の場合必要があります。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="d7aa7-182">アプリケーションは、すべての HTTP 要求を確認し、リダイレクトまたは HTTP 要求を拒否します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="d7aa7-183">ASP.NET Core 2.1 以降で HSTS を実装する、`UseHsts`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="d7aa7-184">次のコード呼び出し`UseHsts`でアプリができないときに[開発モード](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="d7aa7-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="d7aa7-185">`UseHsts` お勧めしません開発 HSTS 設定は、キャッシュ可能であるためのブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="d7aa7-186">既定では、`UseHsts`ローカル ループバック アドレスを除外します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="d7aa7-187">最初に HTTPS を実装する運用環境では、初期の HSTS 値を小さい値に設定します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="d7aa7-188">値を設定時間から no により、1 つの 1 日には、HTTP、HTTPS インフラストラクチャに戻す必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="d7aa7-189">HTTPS の構成の持続性に確信したら、HSTS 最長値を増やす一般的に使用される値は、1 年間です。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="d7aa7-190">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="d7aa7-191">Strict-トランスポート セキュリティ ヘッダーのプリロード パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="d7aa7-192">プリロードの一部でない、 [RFC HSTS 仕様](https://tools.ietf.org/html/rfc6797)、HSTS サイトは新規インストールを事前に web ブラウザーでがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="d7aa7-193">詳細については、「[https://hstspreload.org/](https://hstspreload.org/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="d7aa7-194">により、 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)サブドメインのホストに HSTS ポリシーを適用します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="d7aa7-195">60 日間に Strict-トランスポート セキュリティ ヘッダーの最長有効期間パラメーターを明示的に設定します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="d7aa7-196">既定値は 30 日間を設定しない場合。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="d7aa7-197">参照してください、[最長ディレクティブ](https://tools.ietf.org/html/rfc6797#section-6.1.1)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="d7aa7-198">追加`example.com`を除外するホストの一覧にします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="d7aa7-199">`UseHsts` 次のループバック ホストを除外するには。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="d7aa7-200">`localhost` IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="d7aa7-201">`127.0.0.1` IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="d7aa7-202">`[::1]` IPv6 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="d7aa7-203">前の例では、ホストを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="d7aa7-204">オプトアウト HTTPS のプロジェクトを作成</span><span class="sxs-lookup"><span data-stu-id="d7aa7-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="d7aa7-205">(Visual Studio または dotnet コマンド ライン) から ASP.NET Core 2.1 以降の web アプリケーション テンプレートを使用する[HTTPS へのリダイレクト](#require)と[HSTS](#hsts)します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="d7aa7-206">HTTPS を必要としない展開ではオプトアウトする HTTPS の。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="d7aa7-207">たとえば、一部のバックエンド サービスで HTTPS が処理される外部でエッジに HTTPS を使用して、各ノードでは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="d7aa7-208">オプトアウトの HTTPS。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7aa7-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7aa7-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d7aa7-210">オフにして、 **HTTPS の構成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![エンティティ図](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d7aa7-212">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d7aa7-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="d7aa7-213">`--no-https` オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-213">Use the `--no-https` option.</span></span> <span data-ttu-id="d7aa7-214">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-214">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="d7aa7-215">Docker の開発者の証明書を設定する方法</span><span class="sxs-lookup"><span data-stu-id="d7aa7-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="d7aa7-216">参照してください[この GitHub の問題](https://github.com/aspnet/Docs/issues/6199)します。</span><span class="sxs-lookup"><span data-stu-id="d7aa7-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="d7aa7-217">追加情報</span><span class="sxs-lookup"><span data-stu-id="d7aa7-217">Additional information</span></span>

* [<span data-ttu-id="d7aa7-218">OWASP HSTS ブラウザー サポート</span><span class="sxs-lookup"><span data-stu-id="d7aa7-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
