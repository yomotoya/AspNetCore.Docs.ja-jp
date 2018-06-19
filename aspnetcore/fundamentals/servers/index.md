---
title: ASP.NET Core での Web サーバーの実装
author: rick-anderson
description: ASP.NET Core の Web サーバー Kestrel と HTTP.sys を検出します。 サーバーを選択する方法と、リバース プロキシ サーバーを使用するタイミングについて説明します。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: c9ed385208df083f631174c7071ca31ed2114350
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2018
ms.locfileid: "34473195"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="8551f-104">ASP.NET Core での Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="8551f-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="8551f-105">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="8551f-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="8551f-106">ASP.NET Core アプリは、インプロセス HTTP サーバー実装を使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="8551f-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="8551f-107">サーバー実装は HTTP 要求をリッスンし、[HttpContext](/dotnet/api/system.web.httpcontext) に構成された[要求機能](xref:fundamentals/request-features)のセットとしてアプリに公開します。</span><span class="sxs-lookup"><span data-stu-id="8551f-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an [HttpContext](/dotnet/api/system.web.httpcontext).</span></span>

<span data-ttu-id="8551f-108">ASP.NET Core には次の 2 つのサーバー実装が用意されています。</span><span class="sxs-lookup"><span data-stu-id="8551f-108">ASP.NET Core ships two server implementations:</span></span>

* <span data-ttu-id="8551f-109">[Kestrel](xref:fundamentals/servers/kestrel) は、ASP.NET Core 向けの既定のクロスプラットフォーム HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="8551f-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="8551f-110">[HTTP.sys](xref:fundamentals/servers/httpsys) は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="8551f-110">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="8551f-111">(ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。)</span><span class="sxs-lookup"><span data-stu-id="8551f-111">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

## <a name="kestrel"></a><span data-ttu-id="8551f-112">Kestrel</span><span class="sxs-lookup"><span data-stu-id="8551f-112">Kestrel</span></span>

<span data-ttu-id="8551f-113">Kestrel は、ASP.NET Core のプロジェクト テンプレートに含まれる既定の Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="8551f-113">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8551f-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8551f-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8551f-115">Kestrel は単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。</span><span class="sxs-lookup"><span data-stu-id="8551f-115">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="8551f-116">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="8551f-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="8551f-119">リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="8551f-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="8551f-120">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-120">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8551f-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8551f-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8551f-122">アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="8551f-122">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="8551f-124">アプリをインターネットに公開する場合は、Kestrel が*リバース プロキシ サーバー*として IIS、Nginx、または Apache を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8551f-124">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="8551f-125">以下の図のように、リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="8551f-125">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="8551f-127">エッジ展開 (インターネットからのトラフィックに公開される) でリバース プロキシを使用する最も重要な理由は、セキュリティです。</span><span class="sxs-lookup"><span data-stu-id="8551f-127">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="8551f-128">1.x バージョンの Kestrel はインターネットからの攻撃を防御する重要なセキュリティ機能を備えていません。</span><span class="sxs-lookup"><span data-stu-id="8551f-128">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="8551f-129">これには、適切なタイムアウト、要求サイズの制限、および同時接続の制限などが含まれます (ただし、これらに限定されない)。</span><span class="sxs-lookup"><span data-stu-id="8551f-129">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="8551f-130">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-130">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

---

<span data-ttu-id="8551f-131">IIS、Nginx、および Apache を Kestrel や[カスタム サーバー実装](#custom-servers)なしで使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="8551f-131">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="8551f-132">ASP.NET Core は、プラットフォーム間で一貫して動作できるように、独自のプロセスで実行するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="8551f-132">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="8551f-133">IIS、Nginx、および Apache では独自のスタートアップ プロシージャと環境が指定されます。</span><span class="sxs-lookup"><span data-stu-id="8551f-133">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="8551f-134">これらのサーバー テクノロジを直接使用するには、ASP.NET Core を各サーバーの要件に適合させる必要があります。</span><span class="sxs-lookup"><span data-stu-id="8551f-134">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="8551f-135">Kestrel などの Web サーバー実装を使用することで、ASP.NET Core は異なるサーバー テクノロジでホストされている場合でもスタートアップ プロセスと環境を制御できます。</span><span class="sxs-lookup"><span data-stu-id="8551f-135">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="8551f-136">IIS と Kestrel</span><span class="sxs-lookup"><span data-stu-id="8551f-136">IIS with Kestrel</span></span>

<span data-ttu-id="8551f-137">ASP.NET Core のリバース プロキシとして [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用する場合、ASP.NET Core アプリは IIS ワーカー プロセスとは別のプロセスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8551f-137">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="8551f-138">IIS プロセスで、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がリバース プロキシの関係を調整します。</span><span class="sxs-lookup"><span data-stu-id="8551f-138">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="8551f-139">ASP.NET Core モジュールの主な機能は、ASP.NET Core アプリを開始し、クラッシュ時にアプリを再始動し、HTTP トラフィックをアプリに転送することです。</span><span class="sxs-lookup"><span data-stu-id="8551f-139">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="8551f-140">詳細については、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-140">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="8551f-141">Nginx と Kestrel</span><span class="sxs-lookup"><span data-stu-id="8551f-141">Nginx with Kestrel</span></span>

<span data-ttu-id="8551f-142">Kestrel のリバース プロキシ サーバーとして Linux で Nginx を使用する方法については、「[Host on Linux with Nginx](xref:host-and-deploy/linux-nginx)」(Nginx による Linux でのホスト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-142">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="8551f-143">Apache と Kestrel</span><span class="sxs-lookup"><span data-stu-id="8551f-143">Apache with Kestrel</span></span>

<span data-ttu-id="8551f-144">Kestrel のリバース プロキシ サーバーとして Linux で Apache を使用する方法については、「[Apache による Linux でのホスト](xref:host-and-deploy/linux-apache)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-144">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="8551f-145">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="8551f-145">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8551f-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8551f-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8551f-147">Windows で ASP.NET Core アプリを実行する場合は、HTTP.sys を Kestrel の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="8551f-147">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="8551f-148">最適なパフォーマンスを得るには、通常は Kestrel をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8551f-148">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="8551f-149">HTTP.sys は、アプリがインターネットに公開されていて、必要な機能が HTTP.sys でサポートされているものの、Kestrel ではサポートされていないシナリオで使用できます。</span><span class="sxs-lookup"><span data-stu-id="8551f-149">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required features are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="8551f-150">HTTP.sys の機能については、[HTTP.sys](xref:fundamentals/servers/httpsys) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-150">For information on HTTP.sys features, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="8551f-152">HTTP.sys は、内部ネットワークにのみ公開されるアプリにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="8551f-152">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span> 

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8551f-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8551f-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8551f-155">ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8551f-155">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="8551f-156">IIS がホストアプリで使用できないシナリオにおいて、Windows で ASP.NET Core アプリを実行する場合は、WebListener を代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="8551f-156">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="8551f-158">内部ネットワークにのみ公開されるアプリで、必要な機能が WebListener でサポートされていて、Kestrel でサポートされていない場合は、Kestrel の代わりに WebListener も使用できます。</span><span class="sxs-lookup"><span data-stu-id="8551f-158">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required features are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="8551f-159">WebListener の機能については、[WebListener](xref:fundamentals/servers/weblistener) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-159">For information on WebListener features, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![内部ネットワークと直接通信する WebListener](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="8551f-161">ASP.NET Core サーバー インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="8551f-161">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="8551f-162">`Startup.Configure` メソッドで使用できる [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) は、[IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 型の [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="8551f-162">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="8551f-163">Kestrel および HTTP.sys (ASP.NET Core 1.x の WebListener) は、それぞれ単独の機能 [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) のみを公開しますが、サーバー実装が異なると追加機能が公開される場合があります。</span><span class="sxs-lookup"><span data-stu-id="8551f-163">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="8551f-164">`IServerAddressesFeature` を使用すれば、実行時にサーバー実装がバインドされたポートを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="8551f-164">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="8551f-165">カスタム サーバー</span><span class="sxs-lookup"><span data-stu-id="8551f-165">Custom servers</span></span>

<span data-ttu-id="8551f-166">組み込みサーバーがアプリの要件に合わない場合は、カスタム サーバー実装を作成できます。</span><span class="sxs-lookup"><span data-stu-id="8551f-166">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="8551f-167">[Nowin](https://github.com/Bobris/Nowin) ベースの [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 実装の作成方法については、[Open Web Interface for .NET (OWIN) のガイド](xref:fundamentals/owin)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-167">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="8551f-168">実装を必要とするのは、アプリで使用される機能のインターフェイスのみです。ただし、少なくとも [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) と [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) はサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="8551f-168">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="8551f-169">サーバーの起動</span><span class="sxs-lookup"><span data-stu-id="8551f-169">Server startup</span></span>

<span data-ttu-id="8551f-170">[Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/)、または [Visual Studio Code](https://code.visualstudio.com/) を使用する場合、サーバーはアプリが統合開発環境 (IDE) によって開始されたときに起動します。</span><span class="sxs-lookup"><span data-stu-id="8551f-170">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="8551f-171">Windows の Visual Studio では、[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)またはコンソールで、起動プロファイルを使用してアプリとサーバーを開始することができます。</span><span class="sxs-lookup"><span data-stu-id="8551f-171">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="8551f-172">Visual Studio Code では、CoreCLR デバッガーをアクティブ化する [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) によって、アプリとサーバーが開始します。</span><span class="sxs-lookup"><span data-stu-id="8551f-172">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="8551f-173">Visual Studio for Mac を使用する場合は、アプリとサーバーが [Mono の Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) によって開始します。</span><span class="sxs-lookup"><span data-stu-id="8551f-173">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="8551f-174">コマンド プロンプトからプロジェクトのフォルダーでアプリを起動する場合、[dotnet run](/dotnet/core/tools/dotnet-run) でアプリとサーバーが起動します (Kestrel および HTTP.sys のみ)。</span><span class="sxs-lookup"><span data-stu-id="8551f-174">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="8551f-175">この構成は、`Debug` (既定) または `Release` のどちらかに設定された `-c|--configuration` オプションによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="8551f-175">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="8551f-176">起動プロファイルが *launchSettings.json* ファイルに存在する場合は、`--launch-profile <NAME>` オプションを使用して起動プロファイルを設定します (`Development`、`Production` など)。</span><span class="sxs-lookup"><span data-stu-id="8551f-176">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="8551f-177">詳細については、[dotnet run](/dotnet/core/tools/dotnet-run) と [.NET Core の配布パッケージ](/dotnet/core/build/distribution-packaging)のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8551f-177">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8551f-178">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8551f-178">Additional resources</span></span>

* [<span data-ttu-id="8551f-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="8551f-179">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="8551f-180">Kestrel と IIS</span><span class="sxs-lookup"><span data-stu-id="8551f-180">Kestrel with IIS</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="8551f-181">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="8551f-181">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="8551f-182">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="8551f-182">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* <span data-ttu-id="8551f-183">[HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET Core 1.x の場合は [WebListener](xref:fundamentals/servers/weblistener) を参照)</span><span class="sxs-lookup"><span data-stu-id="8551f-183">[HTTP.sys](xref:fundamentals/servers/httpsys) (for ASP.NET Core 1.x, see [WebListener](xref:fundamentals/servers/weblistener))</span></span>
