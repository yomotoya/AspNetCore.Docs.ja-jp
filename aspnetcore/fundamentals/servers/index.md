---
title: ASP.NET Core での Web サーバーの実装
author: rick-anderson
description: ASP.NET Core の Web サーバー Kestrel と HTTP.sys を検出します。 サーバーを選択する方法と、リバース プロキシ サーバーを使用するタイミングについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 161ab3fdf48e58d8c9af991dc5531e46d9c5adff
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325862"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="96a68-104">ASP.NET Core での Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="96a68-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="96a68-105">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="96a68-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="96a68-106">ASP.NET Core アプリは、インプロセス HTTP サーバー実装を使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="96a68-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="96a68-107">サーバー実装は HTTP 要求をリッスンし、<xref:Microsoft.AspNetCore.Http.HttpContext> に構成された[要求機能](xref:fundamentals/request-features)のセットとしてアプリに公開します。</span><span class="sxs-lookup"><span data-stu-id="96a68-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="96a68-108">ASP.NET Core には、次の 3 つのサーバー実装が用意されています。</span><span class="sxs-lookup"><span data-stu-id="96a68-108">ASP.NET Core ships three server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="96a68-109">[Kestrel](xref:fundamentals/servers/kestrel) は、ASP.NET Core 向けの既定のクロスプラットフォーム HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="96a68-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="96a68-110">`IISHttpServer` は、[インプロセス ホスティング モデル](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model)および Windows 上の [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)で使用されます。</span><span class="sxs-lookup"><span data-stu-id="96a68-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="96a68-111">[HTTP.sys](xref:fundamentals/servers/httpsys) は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="96a68-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="96a68-112">(ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。)</span><span class="sxs-lookup"><span data-stu-id="96a68-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="96a68-113">[Kestrel](xref:fundamentals/servers/kestrel) は、ASP.NET Core 向けの既定のクロスプラットフォーム HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="96a68-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="96a68-114">[HTTP.sys](xref:fundamentals/servers/httpsys) は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="96a68-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="96a68-115">(ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。)</span><span class="sxs-lookup"><span data-stu-id="96a68-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="96a68-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="96a68-116">Kestrel</span></span>

<span data-ttu-id="96a68-117">Kestrel は、ASP.NET Core のプロジェクト テンプレートに含まれる既定の Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="96a68-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="96a68-118">Kestrel は単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。</span><span class="sxs-lookup"><span data-stu-id="96a68-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="96a68-119">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="96a68-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="96a68-122">リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="96a68-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="96a68-123">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="96a68-124">アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="96a68-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="96a68-126">アプリをインターネットに公開する場合は、Kestrel が*リバース プロキシ サーバー*として IIS、Nginx、または Apache を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="96a68-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="96a68-127">以下の図のように、リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="96a68-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="96a68-129">インターネットに直接公開される一般向けのエッジ サーバー展開でリバース プロキシを使用する最も重要な理由は、セキュリティです。</span><span class="sxs-lookup"><span data-stu-id="96a68-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="96a68-130">1.x バージョンの Kestrel はインターネットからの攻撃を防御する重要なセキュリティ機能を備えていません。</span><span class="sxs-lookup"><span data-stu-id="96a68-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="96a68-131">これには、適切なタイムアウト、要求サイズの制限、およびコンカレント接続の制限などが含まれます (ただし、これらに限定されない)。</span><span class="sxs-lookup"><span data-stu-id="96a68-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="96a68-132">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="96a68-133">IIS、Nginx、および Apache を Kestrel や[カスタム サーバー実装](#custom-servers)なしで使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="96a68-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="96a68-134">ASP.NET Core は、プラットフォーム間で一貫して動作できるように、独自のプロセスで実行するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="96a68-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="96a68-135">IIS、Nginx、および Apache では独自のスタートアップ プロシージャと環境が指定されます。</span><span class="sxs-lookup"><span data-stu-id="96a68-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="96a68-136">これらのサーバー テクノロジを直接使用するには、ASP.NET Core を各サーバーの要件に適合させる必要があります。</span><span class="sxs-lookup"><span data-stu-id="96a68-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="96a68-137">Kestrel などの Web サーバー実装を使用することで、ASP.NET Core は異なるサーバー テクノロジでホストされている場合でもスタートアップ プロセスと環境を制御できます。</span><span class="sxs-lookup"><span data-stu-id="96a68-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="96a68-138">IIS と Kestrel</span><span class="sxs-lookup"><span data-stu-id="96a68-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96a68-139">[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用している場合、ASP.NET Core アプリは、IIS ワーカー プロセスと同じプロセス (*インプロセス* ホスティング モデル) または IIS ワーカー プロセスとは別のプロセス (*アウトプロセス* ホスティング モデル) のいずれかで実行されます。</span><span class="sxs-lookup"><span data-stu-id="96a68-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="96a68-140">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)は、インプロセスの IIS HTTP サーバーまたはアウトプロセスの Kestrel サーバーのいずれかの間でネイティブの IIS 要求を処理するネイティブの IIS モジュールです。</span><span class="sxs-lookup"><span data-stu-id="96a68-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="96a68-141">詳細については、「<xref:fundamentals/servers/aspnet-core-module>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="96a68-142">ASP.NET Core のリバース プロキシとして [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用する場合、ASP.NET Core アプリは IIS ワーカー プロセスとは別のプロセスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="96a68-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="96a68-143">IIS プロセスで、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がリバース プロキシの関係を調整します。</span><span class="sxs-lookup"><span data-stu-id="96a68-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="96a68-144">ASP.NET Core モジュールの主な機能は、ASP.NET Core アプリを開始し、クラッシュ時にアプリを再始動し、HTTP トラフィックをアプリに転送することです。</span><span class="sxs-lookup"><span data-stu-id="96a68-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="96a68-145">詳細については、「<xref:fundamentals/servers/aspnet-core-module>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="96a68-146">Nginx と Kestrel</span><span class="sxs-lookup"><span data-stu-id="96a68-146">Nginx with Kestrel</span></span>

<span data-ttu-id="96a68-147">Kestrel のリバース プロキシ サーバーとして Linux で Nginx を使用する方法については、「[Host on Linux with Nginx](xref:host-and-deploy/linux-nginx)」(Nginx による Linux でのホスト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="96a68-148">Apache と Kestrel</span><span class="sxs-lookup"><span data-stu-id="96a68-148">Apache with Kestrel</span></span>

<span data-ttu-id="96a68-149">Kestrel のリバース プロキシ サーバーとして Linux で Apache を使用する方法については、「[Apache による Linux でのホスト](xref:host-and-deploy/linux-apache)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="96a68-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="96a68-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="96a68-151">Windows で ASP.NET Core アプリを実行する場合は、HTTP.sys を Kestrel の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="96a68-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="96a68-152">最適なパフォーマンスを得るには、通常は Kestrel をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="96a68-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="96a68-153">HTTP.sys は、アプリがインターネットに公開されていて、必要な機能が HTTP.sys でサポートされているものの、Kestrel ではサポートされていないシナリオで使用できます。</span><span class="sxs-lookup"><span data-stu-id="96a68-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="96a68-154">HTTP.sys の情報については、[HTTP.sys](xref:fundamentals/servers/httpsys) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="96a68-156">HTTP.sys は、内部ネットワークにのみ公開されるアプリにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="96a68-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="96a68-158">ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="96a68-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="96a68-159">IIS がホストアプリで使用できないシナリオにおいて、Windows で ASP.NET Core アプリを実行する場合は、WebListener を代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="96a68-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="96a68-161">内部ネットワークにのみ公開されるアプリで、必要な機能が WebListener でサポートされていて、Kestrel ではサポートされていない場合、Kestrel の代わりに WebListener も使用できます。</span><span class="sxs-lookup"><span data-stu-id="96a68-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="96a68-162">WebListener の情報については、[WebListener](xref:fundamentals/servers/weblistener) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![内部ネットワークと直接通信する WebListener](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="96a68-164">ASP.NET Core サーバー インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="96a68-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="96a68-165">`Startup.Configure` メソッドで使用できる [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) は、[IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 型の [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="96a68-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="96a68-166">Kestrel および HTTP.sys (ASP.NET Core 1.x の WebListener) は、それぞれ単独の機能 [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) のみを公開しますが、サーバー実装が異なると追加機能が公開される場合があります。</span><span class="sxs-lookup"><span data-stu-id="96a68-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="96a68-167">`IServerAddressesFeature` を使用すれば、実行時にサーバー実装がバインドされたポートを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="96a68-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="96a68-168">カスタム サーバー</span><span class="sxs-lookup"><span data-stu-id="96a68-168">Custom servers</span></span>

<span data-ttu-id="96a68-169">組み込みサーバーがアプリの要件に合わない場合は、カスタム サーバー実装を作成できます。</span><span class="sxs-lookup"><span data-stu-id="96a68-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="96a68-170">[Nowin](https://github.com/Bobris/Nowin) ベースの [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 実装の作成方法については、[Open Web Interface for .NET (OWIN) のガイド](xref:fundamentals/owin)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="96a68-171">実装を必要とするのは、アプリで使用される機能のインターフェイスのみです。ただし、少なくとも [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) と [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) はサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="96a68-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="96a68-172">サーバーの起動</span><span class="sxs-lookup"><span data-stu-id="96a68-172">Server startup</span></span>

<span data-ttu-id="96a68-173">[Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/)、または [Visual Studio Code](https://code.visualstudio.com/) を使用する場合、サーバーはアプリが統合開発環境 (IDE) によって開始されたときに起動します。</span><span class="sxs-lookup"><span data-stu-id="96a68-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="96a68-174">Windows の Visual Studio では、[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)またはコンソールで、起動プロファイルを使用してアプリとサーバーを開始することができます。</span><span class="sxs-lookup"><span data-stu-id="96a68-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="96a68-175">Visual Studio Code では、CoreCLR デバッガーをアクティブ化する [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) によって、アプリとサーバーが開始します。</span><span class="sxs-lookup"><span data-stu-id="96a68-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="96a68-176">Visual Studio for Mac を使用する場合は、アプリとサーバーが [Mono の Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) によって開始します。</span><span class="sxs-lookup"><span data-stu-id="96a68-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="96a68-177">コマンド プロンプトからプロジェクトのフォルダーでアプリを起動する場合、[dotnet run](/dotnet/core/tools/dotnet-run) でアプリとサーバーが起動します (Kestrel および HTTP.sys のみ)。</span><span class="sxs-lookup"><span data-stu-id="96a68-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="96a68-178">この構成は、`Debug` (既定) または `Release` のどちらかに設定された `-c|--configuration` オプションによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="96a68-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="96a68-179">起動プロファイルが *launchSettings.json* ファイルに存在する場合は、`--launch-profile <NAME>` オプションを使用して起動プロファイルを設定します (`Development`、`Production` など)。</span><span class="sxs-lookup"><span data-stu-id="96a68-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="96a68-180">詳細については、[dotnet run](/dotnet/core/tools/dotnet-run) と [.NET Core の配布パッケージ](/dotnet/core/build/distribution-packaging)のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="96a68-181">HTTP/2 のサポート</span><span class="sxs-lookup"><span data-stu-id="96a68-181">HTTP/2 support</span></span>

<span data-ttu-id="96a68-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) は、次の展開シナリオでの ASP.NET Core でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="96a68-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="96a68-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="96a68-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="96a68-184">オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="96a68-184">Operating system</span></span>
    * <span data-ttu-id="96a68-185">Windows Server 2012 R2/Windows 8.1 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-185">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="96a68-186">OpenSSL 1.0.2 以降を使用した Linux (Ubuntu 16.04 以降など)</span><span class="sxs-lookup"><span data-stu-id="96a68-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="96a68-187">将来のリリースでは HTTP/2 が macOS 上でサポートされるようになります。</span><span class="sxs-lookup"><span data-stu-id="96a68-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="96a68-188">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="96a68-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="96a68-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="96a68-190">Windows Server 2016/Windows 10 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="96a68-191">ターゲット フレームワーク: HTTP.sys の展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="96a68-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="96a68-192">IIS (インプロセス)</span><span class="sxs-lookup"><span data-stu-id="96a68-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="96a68-193">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="96a68-194">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="96a68-195">IIS (アウトプロセス)</span><span class="sxs-lookup"><span data-stu-id="96a68-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="96a68-196">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="96a68-197">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="96a68-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="96a68-198">ターゲット フレームワーク: IIS アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="96a68-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="96a68-199">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="96a68-199">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="96a68-200">Windows Server 2016/Windows 10 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-200">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="96a68-201">ターゲット フレームワーク: HTTP.sys の展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="96a68-201">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="96a68-202">IIS (アウトプロセス)</span><span class="sxs-lookup"><span data-stu-id="96a68-202">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="96a68-203">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="96a68-203">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="96a68-204">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="96a68-204">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="96a68-205">ターゲット フレームワーク: IIS アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="96a68-205">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="96a68-206">HTTP/2 接続では、[アプリケーション レイヤー プロトコルのネゴシエーション (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) および TLS 1.2 以降を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="96a68-206">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="96a68-207">詳細については、ご利用のサーバーの展開シナリオに関連するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="96a68-207">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96a68-208">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="96a68-208">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="96a68-209"><xref:fundamentals/servers/httpsys> (ASP.NET Core 1.x については、<xref:fundamentals/servers/weblistener> を参照してください)</span><span class="sxs-lookup"><span data-stu-id="96a68-209"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
