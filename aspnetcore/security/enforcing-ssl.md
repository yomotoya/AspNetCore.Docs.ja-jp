---
title: ASP.NET Core での HTTPS を適用します。
author: rick-anderson
description: ASP.NET Core web アプリで HTTPS や TLS を必要とする方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: security/enforcing-ssl
ms.openlocfilehash: afa40db4c84820db91878bb98dae082b3dd9a2e2
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244883"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="aa64e-103">ASP.NET Core での HTTPS を適用します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="aa64e-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa64e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa64e-105">このドキュメントでは次の方法について説明します:</span><span class="sxs-lookup"><span data-stu-id="aa64e-105">This document shows how to:</span></span>

* <span data-ttu-id="aa64e-106">すべての要求に HTTPS を必要とさせる。</span><span class="sxs-lookup"><span data-stu-id="aa64e-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="aa64e-107">すべての HTTP 要求を HTTPS にリダイレクトさせる。</span><span class="sxs-lookup"><span data-stu-id="aa64e-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="aa64e-108">API ようにするありませんクライアントの最初の要求で機密データを送信します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="aa64e-109">**いない**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)機密情報を受け取る Web Api で。</span><span class="sxs-lookup"><span data-stu-id="aa64e-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="aa64e-110">`RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="aa64e-111">API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="aa64e-112">このようなクライアントは、HTTP 経由で情報を送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="aa64e-113">Web API は次のいずれかの対策を講じるべきです:</span><span class="sxs-lookup"><span data-stu-id="aa64e-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="aa64e-114">HTTP をリッスンしない。</span><span class="sxs-lookup"><span data-stu-id="aa64e-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="aa64e-115">ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。
</span><span class="sxs-lookup"><span data-stu-id="aa64e-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="aa64e-116">HTTPS が必要です。</span><span class="sxs-lookup"><span data-stu-id="aa64e-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aa64e-117">Web アプリの呼び出しを実稼働 ASP.NET Core を推奨します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="aa64e-118">HTTPS のリダイレクトのミドルウェア (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) HTTP 要求を HTTPS にリダイレクトするためです。</span><span class="sxs-lookup"><span data-stu-id="aa64e-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="aa64e-119">HSTS ミドルウェア ([UseHsts](#http-strict-transport-security-protocol-hsts)) HTTP Strict Transport Security プロトコル (HSTS) ヘッダーをクライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="aa64e-120">リバース プロキシ構成でデプロイされたアプリは、接続のセキュリティ (HTTPS) を処理するためにプロキシを使用できます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="aa64e-121">プロキシでは、HTTPS へのリダイレクトも処理する場合、HTTPS のリダイレクトのミドルウェアを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="aa64e-122">HSTS ヘッダーを書き込み、プロキシ サーバーにも処理する場合 (たとえば、[ネイティブ HSTS IIS 10.0 (1709) またはそれ以降のサポート](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support))、HSTS ミドルウェアは、アプリによって要求はありません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="aa64e-123">詳細については、次を参照してください。[プロジェクトの作成での HTTPS/HSTS オプトイン アウト](#opt-out-of-httpshsts-on-project-creation)します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="aa64e-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="aa64e-124">UseHttpsRedirection</span></span>

<span data-ttu-id="aa64e-125">次のコード呼び出し`UseHttpsRedirection`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="aa64e-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="aa64e-126">上記の強調表示されたコード。</span><span class="sxs-lookup"><span data-stu-id="aa64e-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="aa64e-127">既定値を使用して[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect))。</span><span class="sxs-lookup"><span data-stu-id="aa64e-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="aa64e-128">既定値を使用して[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport)によってオーバーライドされない限り、(null)、`ASPNETCORE_HTTPS_PORT`環境変数または[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="aa64e-129">永続的なリダイレクトではなく、一時的なリダイレクトを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="aa64e-130">リンク キャッシュと、開発環境で不安定な動作が発生することができます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="aa64e-131">アプリがない開発環境の場合、永続的なリダイレクト状態コードを送信する場合を参照してください、[実稼働環境で永続的なリダイレクトを構成する](#configure-permanent-redirects-in-production)セクション。</span><span class="sxs-lookup"><span data-stu-id="aa64e-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="aa64e-132">使用することをお勧めします。 [HSTS](#http-strict-transport-security-protocol-hsts)のみリソースをセキュリティで保護するクライアントに通知する (運用) でのみ、アプリに要求を送信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="aa64e-133">ポートの構成</span><span class="sxs-lookup"><span data-stu-id="aa64e-133">Port configuration</span></span>

<span data-ttu-id="aa64e-134">ポートは、安全でない要求を HTTPS にリダイレクトするミドルウェアの使用可能なである必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="aa64e-135">使用できるポートがない場合。</span><span class="sxs-lookup"><span data-stu-id="aa64e-135">If no port is available:</span></span>

* <span data-ttu-id="aa64e-136">HTTPS へのリダイレクトは発生しません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="aa64e-137">ミドルウェア「をリダイレクト用 https ポートを特定できませんでした」警告をログします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="aa64e-138">次の方法のいずれかを使用して、HTTPS ポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="aa64e-139">設定[HttpsRedirectionOptions.HttpsPort](#options)します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="aa64e-140">設定、`ASPNETCORE_HTTPS_PORT`環境変数または[https_port Web ホストの構成設定](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="aa64e-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="aa64e-141">**キー**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="aa64e-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="aa64e-142">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="aa64e-142">**Type**: *string*</span></span>  
  <span data-ttu-id="aa64e-143">**既定**: 既定値が設定されていません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="aa64e-144">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="aa64e-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="aa64e-145">**環境変数**: `<PREFIX_>HTTPS_PORT` (プレフィックスは`ASPNETCORE_`を使用する場合、 [Web ホスト](xref:fundamentals/host/web-host))。</span><span class="sxs-lookup"><span data-stu-id="aa64e-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="aa64e-146">構成するときに、<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>で`Program`:</span><span class="sxs-lookup"><span data-stu-id="aa64e-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="aa64e-147">使用して、セキュリティで保護されたスキームのポートを示す、`ASPNETCORE_URLS`環境変数。</span><span class="sxs-lookup"><span data-stu-id="aa64e-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="aa64e-148">環境変数は、サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-148">The environment variable configures the server.</span></span> <span data-ttu-id="aa64e-149">ミドルウェアが使用して HTTPS ポートを直接検出<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="aa64e-150">(は**いない**リバース プロキシの展開で動作します)。</span><span class="sxs-lookup"><span data-stu-id="aa64e-150">(Does **not** work in reverse proxy deployments.)</span></span>
* <span data-ttu-id="aa64e-151">開発では、HTTPS URL を設定*launchsettings.json*します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="aa64e-152">IIS Express を使用する場合は、HTTPS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="aa64e-153">HTTPS の URL のエンドポイントの公開 edge のデプロイを構成[Kestrel](xref:fundamentals/servers/kestrel)または[HTTP.sys](xref:fundamentals/servers/httpsys)します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="aa64e-154">のみ**1 つの HTTPS ポート**アプリによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="aa64e-155">ミドルウェアを使用してポートを検出する<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="aa64e-156">アプリを実行したリバース プロキシ (たとえば、IIS、IIS Express) の背後にあるときに`IServerAddressesFeature`は使用できません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-156">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="aa64e-157">ポートを手動で構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-157">The port must be manually configured.</span></span> <span data-ttu-id="aa64e-158">ポートが設定されていないときに要求をリダイレクトされません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-158">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="aa64e-159">パブリックに公開されたエッジ サーバーとして Kestrel または HTTP.sys を使用、Kestrel または HTTP.sys は、両方でリッスンするように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-159">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="aa64e-160">クライアントがリダイレクトされる場所、セキュリティで保護されたポート (通常、運用環境と開発における 5001 で 443 など)。</span><span class="sxs-lookup"><span data-stu-id="aa64e-160">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="aa64e-161">安全でないポート (通常、運用環境では 80) および開発で 5000 です。</span><span class="sxs-lookup"><span data-stu-id="aa64e-161">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="aa64e-162">安全でないポートは、安全でない要求を受信し、セキュリティで保護されたポートにクライアントをリダイレクトする、アプリのために、クライアントによってアクセス可能である必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-162">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="aa64e-163">詳細については、次を参照してください。 [Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)または<xref:fundamentals/servers/httpsys>します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-163">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="aa64e-164">展開シナリオ</span><span class="sxs-lookup"><span data-stu-id="aa64e-164">Deployment scenarios</span></span>

<span data-ttu-id="aa64e-165">クライアントとサーバー間のファイアウォールには、通信ポートのトラフィックを開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-165">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="aa64e-166">使用して、要求はリバース プロキシ構成で転送され場合、 [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) HTTPS リダイレクト ミドルウェアを呼び出す前にします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-166">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="aa64e-167">ヘッダーのミドルウェアの更新プログラムを転送、`Request.Scheme`を使用して、`X-Forwarded-Proto`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="aa64e-167">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="aa64e-168">ミドルウェアの許可は、正常に動作するには、Uri と他のセキュリティ ポリシーにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-168">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="aa64e-169">Forwarded Headers Middleware が使用されていないときに、バックエンド アプリ可能性がありますいない正しいスキームを受信およびのリダイレクト ループ。</span><span class="sxs-lookup"><span data-stu-id="aa64e-169">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="aa64e-170">一般的なエンド ユーザー エラー メッセージは、リダイレクトが多すぎますが発生したことです。</span><span class="sxs-lookup"><span data-stu-id="aa64e-170">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="aa64e-171">Azure App Service にデプロイするときのガイダンスに従って[チュートリアル: Azure Web Apps に既存のカスタム SSL 証明書をバインド](/azure/app-service/app-service-web-tutorial-custom-ssl)します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-171">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="aa64e-172">オプション</span><span class="sxs-lookup"><span data-stu-id="aa64e-172">Options</span></span>

<span data-ttu-id="aa64e-173">次のコードの呼び出しを強調表示されている[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)ミドルウェアのオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-173">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="aa64e-174">呼び出す`AddHttpsRedirection`の値を変更するだけで済みます`HttpsPort`または`RedirectStatusCode`します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-174">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="aa64e-175">上記の強調表示されたコード。</span><span class="sxs-lookup"><span data-stu-id="aa64e-175">The preceding highlighted code:</span></span>

* <span data-ttu-id="aa64e-176">セット[HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*)に<xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>、これは、既定値。</span><span class="sxs-lookup"><span data-stu-id="aa64e-176">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="aa64e-177">フィールドを使用して、<xref:Microsoft.AspNetCore.Http.StatusCodes>クラスに対する割り当ての`RedirectStatusCode`します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-177">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="aa64e-178">HTTPS ポートを 5001 に設定します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-178">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="aa64e-179">既定値には 443 です。</span><span class="sxs-lookup"><span data-stu-id="aa64e-179">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="aa64e-180">運用環境で永続的なリダイレクトを構成します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-180">Configure permanent redirects in production</span></span>

<span data-ttu-id="aa64e-181">送信、ミドルウェアの既定値は、 [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)すべてリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-181">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="aa64e-182">アプリがない開発環境の場合、永続的なリダイレクト状態コードを送信する場合は、非開発環境の条件の確認、ミドルウェアのオプション構成をラップします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-182">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="aa64e-183">構成するときに、`IWebHostBuilder`で*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa64e-183">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="aa64e-184">HTTPS のリダイレクトのミドルウェアの代替アプローチ</span><span class="sxs-lookup"><span data-stu-id="aa64e-184">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="aa64e-185">HTTPS のリダイレクトのミドルウェアを使用する代わりに (`UseHttpsRedirection`) は、URL リライト ミドルウェアを使用する (`AddRedirectToHttps`)。</span><span class="sxs-lookup"><span data-stu-id="aa64e-185">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="aa64e-186">`AddRedirectToHttps` 設定こともできます、ステータス コードとポートのリダイレクトを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-186">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="aa64e-187">さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa64e-187">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="aa64e-188">HTTPS リダイレクト ミドルウェアの使用をお勧めときに、追加のリダイレクト ルールを必要としないを HTTPS にリダイレクトする、(`UseHttpsRedirection`) このトピックで説明します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-188">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="aa64e-189">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) は HTTPS を必須とさせるために使用します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-189">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="aa64e-190">`[RequireHttpsAttribute]` はメソッドまたはコントローラーを装飾するか、またはグローバルに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-190">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="aa64e-191">属性をグローバルに適用するには、`ConfigureServices` の `Startup` に次のコードを追加します:</span><span class="sxs-lookup"><span data-stu-id="aa64e-191">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="aa64e-192">上記の強調表示されたコードでは、すべての要求を使用して、必要があります`HTTPS`。 したがって、HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-192">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="aa64e-193">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-193">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="aa64e-194">さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa64e-194">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="aa64e-195">ミドルウェアは、アプリのリダイレクトを実行すると、状態コードまたは状態コードと、ポートを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-195">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="aa64e-196">グローバルに HTTPS を必須とさせること (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="aa64e-196">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="aa64e-197">`[RequireHttps]`属性をすべてのコントローラー/Razor ページ に適用することは、グローバルに HTTPS を必須とさせることほど安全性が高いとは考えられていません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-197">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="aa64e-198">新しいコントローラー または Razor ページが追加されたときに `[RequireHttps]` 属性が適用されるとは限らないためです。</span><span class="sxs-lookup"><span data-stu-id="aa64e-198">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="aa64e-199">HTTP Strict Transport Security プロトコル (HSTS)</span><span class="sxs-lookup"><span data-stu-id="aa64e-199">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="aa64e-200">あたり[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)、 [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)は応答ヘッダーを使用して web アプリによって指定されるオプトイン セキュリティ拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="aa64e-200">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="aa64e-201">ときに、 [HSTS をサポートするブラウザー](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)このヘッダーを受信します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-201">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="aa64e-202">ブラウザーでは、HTTP 経由で通信を送信しないように、ドメインの構成を格納します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-202">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="aa64e-203">ブラウザーでは、HTTPS 経由ですべての通信を強制します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-203">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="aa64e-204">ブラウザーでは、ユーザーが信頼されていないか無効な証明書を使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-204">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="aa64e-205">ブラウザーには、一時的にこのような証明書を信頼するユーザーを許可するプロンプトが無効にします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-205">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="aa64e-206">HSTS は、クライアントで強制されるため、いくつかの制限があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-206">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="aa64e-207">クライアントは、HSTS をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-207">The client must support HSTS.</span></span>
* <span data-ttu-id="aa64e-208">HSTS では、少なくとも 1 つ HSTS ポリシーを確立するために HTTPS 要求の成功の場合必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa64e-208">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="aa64e-209">アプリケーションは、すべての HTTP 要求を確認し、リダイレクトまたは HTTP 要求を拒否します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-209">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="aa64e-210">ASP.NET Core 2.1 以降で HSTS を実装する、`UseHsts`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="aa64e-210">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="aa64e-211">次のコード呼び出し`UseHsts`でアプリができないときに[開発モード](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="aa64e-211">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="aa64e-212">`UseHsts` お勧めしません開発 HSTS 設定は、キャッシュ可能であるためのブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-212">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="aa64e-213">既定では、`UseHsts`ローカル ループバック アドレスを除外します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-213">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="aa64e-214">運用環境での HTTPS を実装する最初の設定で初期[HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*)に小さい値のいずれかを使用して、<xref:System.TimeSpan>メソッド。</span><span class="sxs-lookup"><span data-stu-id="aa64e-214">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="aa64e-215">値を設定時間から no により、1 つの 1 日には、HTTP、HTTPS インフラストラクチャに戻す必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="aa64e-215">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="aa64e-216">HTTPS の構成の持続性に確信したら、HSTS 最長値を増やす一般的に使用される値は、1 年間です。</span><span class="sxs-lookup"><span data-stu-id="aa64e-216">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="aa64e-217">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-217">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="aa64e-218">Strict-トランスポート セキュリティ ヘッダーのプリロード パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-218">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="aa64e-219">プリロードの一部でない、 [RFC HSTS 仕様](https://tools.ietf.org/html/rfc6797)、HSTS サイトは新規インストールを事前に web ブラウザーでがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="aa64e-219">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="aa64e-220">詳細については、「[https://hstspreload.org/](https://hstspreload.org/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa64e-220">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="aa64e-221">により、 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)サブドメインのホストに HSTS ポリシーを適用します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-221">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="aa64e-222">60 日間に Strict-トランスポート セキュリティ ヘッダーの最長有効期間パラメーターを明示的に設定します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-222">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="aa64e-223">既定値は 30 日間を設定しない場合。</span><span class="sxs-lookup"><span data-stu-id="aa64e-223">If not set, defaults to 30 days.</span></span> <span data-ttu-id="aa64e-224">参照してください、[最長ディレクティブ](https://tools.ietf.org/html/rfc6797#section-6.1.1)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-224">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="aa64e-225">追加`example.com`を除外するホストの一覧にします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-225">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="aa64e-226">`UseHsts` 次のループバック ホストを除外するには。</span><span class="sxs-lookup"><span data-stu-id="aa64e-226">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="aa64e-227">`localhost` IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="aa64e-227">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="aa64e-228">`127.0.0.1` IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="aa64e-228">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="aa64e-229">`[::1]` IPv6 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="aa64e-229">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="aa64e-230">オプトアウトの HTTPS/HSTS でプロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="aa64e-230">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="aa64e-231">パブリックに公開されたエッジ ネットワークの接続のセキュリティを処理する、バックエンド サービスのシナリオでの各ノードに接続のセキュリティを構成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-231">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="aa64e-232">Web アプリまたは Visual Studio でテンプレートから生成された、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンドの有効化[HTTPS へのリダイレクト](#require-https)と[HSTS](#http-strict-transport-security-protocol-hsts)します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-232">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="aa64e-233">これらのシナリオを必要としない展開の場合、できるオプトアウトする HTTPS/HSTS のテンプレートからアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-233">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="aa64e-234">オプトアウトの HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="aa64e-234">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aa64e-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aa64e-235">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="aa64e-236">オフにして、 **HTTPS の構成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="aa64e-236">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![新しい ASP.NET Core Web アプリケーションのダイアログが HTTPS チェック ボックスをオフの構成を表示します。](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="aa64e-238">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="aa64e-238">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="aa64e-239">`--no-https` オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-239">Use the `--no-https` option.</span></span> <span data-ttu-id="aa64e-240">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-240">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="aa64e-241">Windows と macOS で ASP.NET Core HTTPS 開発証明書を信頼します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-241">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="aa64e-242">.NET core SDK には、HTTPS 開発証明書が含まれています。</span><span class="sxs-lookup"><span data-stu-id="aa64e-242">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="aa64e-243">証明書は、最初の実行エクスペリエンスの一部としてインストールされます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-243">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="aa64e-244">たとえば、`dotnet --info`次のような出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-244">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="aa64e-245">.NET Core SDK をインストールすると、ローカル ユーザーの証明書ストアに ASP.NET Core HTTPS 開発証明書がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="aa64e-245">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="aa64e-246">証明書がインストールされているが、それが信頼されていません。</span><span class="sxs-lookup"><span data-stu-id="aa64e-246">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="aa64e-247">Dotnet の実行に 1 回限りの手順を実行している証明書を信頼する`dev-certs`ツール。</span><span class="sxs-lookup"><span data-stu-id="aa64e-247">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="aa64e-248">次のコマンドに関するヘルプを提供する、`dev-certs`ツール。</span><span class="sxs-lookup"><span data-stu-id="aa64e-248">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="aa64e-249">Docker の開発者の証明書を設定する方法</span><span class="sxs-lookup"><span data-stu-id="aa64e-249">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="aa64e-250">参照してください[この GitHub の問題](https://github.com/aspnet/Docs/issues/6199)します。</span><span class="sxs-lookup"><span data-stu-id="aa64e-250">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="aa64e-251">追加情報</span><span class="sxs-lookup"><span data-stu-id="aa64e-251">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="aa64e-252">Apache による Linux で ASP.NET Core のホスト: SSL の構成</span><span class="sxs-lookup"><span data-stu-id="aa64e-252">Host ASP.NET Core on Linux with Apache: SSL configuration</span></span>](xref:host-and-deploy/linux-apache#ssl-configuration)
* [<span data-ttu-id="aa64e-253">Nginx による Linux で ASP.NET Core のホスト: SSL の構成</span><span class="sxs-lookup"><span data-stu-id="aa64e-253">Host ASP.NET Core on Linux with Nginx: SSL configuration</span></span>](xref:host-and-deploy/linux-nginx#configure-ssl)
* [<span data-ttu-id="aa64e-254">IIS で SSL を設定する方法</span><span class="sxs-lookup"><span data-stu-id="aa64e-254">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="aa64e-255">OWASP HSTS ブラウザー サポート</span><span class="sxs-lookup"><span data-stu-id="aa64e-255">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
