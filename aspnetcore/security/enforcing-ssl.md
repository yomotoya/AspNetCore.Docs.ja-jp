---
title: ASP.NET Core で HTTPS を適用します。
author: rick-anderson
description: Web アプリで ASP.NET Core HTTPS/TLS を必要とする方法を示します。
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 24ab83192ded381b46fab337a986f51fb22b2227
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729501"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="a1e2a-103">ASP.NET Core で HTTPS を適用します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="a1e2a-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a1e2a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a1e2a-105">このドキュメントでは次の方法について説明します:</span><span class="sxs-lookup"><span data-stu-id="a1e2a-105">This document shows how to:</span></span>

* <span data-ttu-id="a1e2a-106">すべての要求に HTTPS を必要とさせる。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="a1e2a-107">すべての HTTP 要求を HTTPS にリダイレクトさせる。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="a1e2a-108">操作を行います**いない**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)機密情報を受信する Web Api にします。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="a1e2a-109">`RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="a1e2a-110">API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="a1e2a-111">このようなクライアントは、HTTP 経由で情報を送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="a1e2a-112">Web API は次のいずれかの対策を講じるべきです:</span><span class="sxs-lookup"><span data-stu-id="a1e2a-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="a1e2a-113">HTTP をリッスンしない。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="a1e2a-114">ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。
</span><span class="sxs-lookup"><span data-stu-id="a1e2a-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="a1e2a-115">HTTPS が必要</span><span class="sxs-lookup"><span data-stu-id="a1e2a-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a1e2a-116">すべての ASP.NET Core web アプリケーションが HTTPS のリダイレクトのミドルウェアを呼び出すことをお勧め ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="a1e2a-117">次のコード呼び出し`UseHttpsRedirection`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="a1e2a-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="a1e2a-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="a1e2a-119">次のコード呼び出し[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)ミドルウェアのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="a1e2a-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="a1e2a-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="a1e2a-121">上記の強調表示されたコード。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="a1e2a-122">セット[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span></span>
* <span data-ttu-id="a1e2a-123">5001 を HTTPS ポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-123">Sets the HTTPS port to 5001.</span></span>

<span data-ttu-id="a1e2a-124">次のメカニズムでは、ポートが自動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="a1e2a-125">ミドルウェアを使用してポートを検出できる[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)次の条件を適用する場合。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="a1e2a-126">HTTPS エンドポイントを直接 kestrel または HTTP.sys を使用 (Visual Studio のコードのデバッガーでは、アプリの実行にも適用されます)。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="a1e2a-127">のみ**1 つの HTTPS ポート**アプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="a1e2a-128">Visual Studio を使用します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="a1e2a-129">IIS Express、HTTPS 対応があります。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="a1e2a-130">*launchSettings.json*設定、 `sslPort` IIS Express 用です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="a1e2a-131">(たとえば、IIS、IIS Express) は、リバース プロキシの背後にあるアプリの実行時に`IServerAddressesFeature`は使用できません。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="a1e2a-132">ポートを手動で構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-132">The port must be manually configured.</span></span> <span data-ttu-id="a1e2a-133">ポートが設定されていない、ときに要求をリダイレクトされません。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="a1e2a-134">設定して、ポートを構成することができます、します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="a1e2a-135">`ASPNETCORE_HTTPS_PORT` 環境変数。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="a1e2a-136">`http_port` ホストの構成のキー (などを介して*hostsettings.json*またはコマンドライン引数)。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="a1e2a-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport)です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="a1e2a-138">5001 にポートを設定する方法を示しています。 前の例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="a1e2a-139">ポートを設定することが直接使用して URL を設定して、`ASPNETCORE_URLS`環境変数。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="a1e2a-140">環境変数は、サーバーを構成し、ミドルウェア直接、検出されません経由の HTTPS ポート`IServerAddressesFeature`です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="a1e2a-141">ポートが設定されていない: 場合</span><span class="sxs-lookup"><span data-stu-id="a1e2a-141">If no port is set:</span></span>

* <span data-ttu-id="a1e2a-142">要求がリダイレクトされません。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="a1e2a-143">ミドルウェアは、警告を記録します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-143">The middleware logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="a1e2a-144">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) は HTTPS を必須とさせるために使用します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-144">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="a1e2a-145">`[RequireHttpsAttribute]` はメソッドまたはコントローラーを装飾するか、またはグローバルに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-145">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="a1e2a-146">属性をグローバルに適用するには、`ConfigureServices` の `Startup` に次のコードを追加します:</span><span class="sxs-lookup"><span data-stu-id="a1e2a-146">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="a1e2a-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="a1e2a-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="a1e2a-148">前の強調表示されたコードでは、すべての要求を使用して`HTTPS`です。 そのため、HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-148">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="a1e2a-149">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-149">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="a1e2a-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="a1e2a-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="a1e2a-151">さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-151">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="a1e2a-152">グローバルに HTTPS を必須とさせること (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-152">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="a1e2a-153">`[RequireHttps]`属性をすべてのコントローラー/Razor ページ に適用することは、グローバルに HTTPS を必須とさせることほど安全性が高いとは考えられていません。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-153">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="a1e2a-154">新しいコントローラー または Razor ページが追加されたときに `[RequireHttps]` 属性が適用されるとは限らないためです。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-154">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="a1e2a-155">HTTP 厳密なトランスポート セキュリティ プロトコル (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a1e2a-155">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="a1e2a-156">あたり[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)、 [HTTP 厳密なトランスポート セキュリティ (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)オプトイン セキュリティ拡張機能を利用、特別な応答ヘッダーを使用して web アプリケーションによって指定されています。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-156">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="a1e2a-157">サポートされているブラウザーがこのヘッダーを受け取るし、そのブラウザーから、指定したドメインに HTTP 経由で送信されるすべての通信を防ぐ HTTPS 経由ですべての通信を代わりに送信されます。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-157">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="a1e2a-158">ブラウザーでのプロンプトの HTTPS クリックスルーも回避されます。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-158">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="a1e2a-159">ASP.NET Core 2.1 以降と HSTS を実装して、`UseHsts`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-159">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="a1e2a-160">次のコード呼び出し`UseHsts`にアプリがないとき[開発モード](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="a1e2a-160">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="a1e2a-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="a1e2a-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="a1e2a-162">`UseHsts` 開発のことをお勧めためには HSTS ヘッダーがブラウザーでキャッシュ可能な高度です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-162">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="a1e2a-163">既定では、UseHsts はローカル ループバック アドレスを除外します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-163">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="a1e2a-164">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-164">The following code:</span></span>

<span data-ttu-id="a1e2a-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="a1e2a-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="a1e2a-166">Strict トランスポート セキュリティ ヘッダーのプリロード パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-166">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="a1e2a-167">プリロードされていないの一部、 [RFC HSTS 仕様](https://tools.ietf.org/html/rfc6797)が新規インストールで HSTS サイトをプリロードする web ブラウザーでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-167">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="a1e2a-168">詳細については、「[https://hstspreload.org/](https://hstspreload.org/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-168">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="a1e2a-169">により、 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)、HSTS ポリシー サブドメインをホストに適用されます。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-169">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="a1e2a-170">明示的に 60 日間にする高レベルのトランスポートのセキュリティ ヘッダーの最大継続期間パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-170">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="a1e2a-171">設定されていない場合、既定値は 30 日間です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-171">If not set, defaults to 30 days.</span></span> <span data-ttu-id="a1e2a-172">参照してください、[最大継続期間ディレクティブ](https://tools.ietf.org/html/rfc6797#section-6.1.1)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-172">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="a1e2a-173">追加`example.com`を除外するホストの一覧にします。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-173">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="a1e2a-174">`UseHsts` 次のループバックのホストを除外します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-174">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="a1e2a-175">`localhost` : IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-175">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a1e2a-176">`127.0.0.1` : IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-176">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="a1e2a-177">`[::1]` : IPv6 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-177">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="a1e2a-178">前の例では、他のホストを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-178">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="a1e2a-179">オプトアウト HTTPS でプロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="a1e2a-179">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="a1e2a-180">(Visual Studio または dotnet コマンド ライン) から ASP.NET Core 2.1 以降の web アプリケーション テンプレートを使用する[HTTPS リダイレクト](#require)と[HSTS](#hsts)です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-180">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="a1e2a-181">HTTPS は必要ありません、展開にすることができますオプトアウト HTTPS です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-181">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="a1e2a-182">たとえば、ここで HTTPS を処理している外部で、エッジに各ノードで HTTPS を使用して一部のバックエンド サービスは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-182">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="a1e2a-183">無効にするは、HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a1e2a-183">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a1e2a-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1e2a-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="a1e2a-185">オフにして、 **HTTPS の構成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-185">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![エンティティ図](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a1e2a-187">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a1e2a-187">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="a1e2a-188">`--no-https` オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-188">Use the `--no-https` option.</span></span> <span data-ttu-id="a1e2a-189">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-189">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="a1e2a-190">Docker の開発者の証明書をセットアップする方法</span><span class="sxs-lookup"><span data-stu-id="a1e2a-190">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="a1e2a-191">参照してください[この GitHub 問題](https://github.com/aspnet/Docs/issues/6199)です。</span><span class="sxs-lookup"><span data-stu-id="a1e2a-191">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
