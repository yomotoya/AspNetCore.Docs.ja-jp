---
title: ASP.NET Core での Web サーバーの実装
author: guardrex
description: ASP.NET Core の Web サーバー Kestrel と HTTP.sys を検出します。 サーバーを選択する方法と、リバース プロキシ サーバーを使用するタイミングについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861357"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="adf60-104">ASP.NET Core での Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="adf60-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="adf60-105">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="adf60-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="adf60-106">ASP.NET Core アプリは、インプロセス HTTP サーバー実装を使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="adf60-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="adf60-107">サーバー実装は HTTP 要求をリッスンし、<xref:Microsoft.AspNetCore.Http.HttpContext> に構成された[要求機能](xref:fundamentals/request-features)のセットとしてアプリに公開します。</span><span class="sxs-lookup"><span data-stu-id="adf60-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="adf60-108">Windows</span><span class="sxs-lookup"><span data-stu-id="adf60-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="adf60-109">ASP.NET Core には次のものが付属しています。</span><span class="sxs-lookup"><span data-stu-id="adf60-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="adf60-110">[Kestrel サーバー](xref:fundamentals/servers/kestrel)は、既定のクロスプラットフォーム HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="adf60-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="adf60-111">IIS HTTP サーバー (`IISHttpServer`) は、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)で使用される[インプロセス IIS サーバー](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model)です。</span><span class="sxs-lookup"><span data-stu-id="adf60-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="adf60-112">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys)は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="adf60-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="adf60-113">ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="adf60-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="adf60-114">macOS</span><span class="sxs-lookup"><span data-stu-id="adf60-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="adf60-115">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="adf60-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="adf60-116">Linux</span><span class="sxs-lookup"><span data-stu-id="adf60-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="adf60-117">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="adf60-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="adf60-118">Windows</span><span class="sxs-lookup"><span data-stu-id="adf60-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="adf60-119">ASP.NET Core には次のものが付属しています。</span><span class="sxs-lookup"><span data-stu-id="adf60-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="adf60-120">[Kestrel サーバー](xref:fundamentals/servers/kestrel)は、既定のクロスプラットフォーム HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="adf60-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="adf60-121">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys)は、[HTTP.sys カーネル ドライバーおよび HTTP サーバー API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="adf60-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="adf60-122">ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="adf60-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="adf60-123">macOS</span><span class="sxs-lookup"><span data-stu-id="adf60-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="adf60-124">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="adf60-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="adf60-125">Linux</span><span class="sxs-lookup"><span data-stu-id="adf60-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="adf60-126">ASP.NET Core には、既定のクロスプラットフォーム HTTP サーバーである [Kestrel サーバー](xref:fundamentals/servers/kestrel)が付属しています。</span><span class="sxs-lookup"><span data-stu-id="adf60-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="adf60-127">Kestrel</span><span class="sxs-lookup"><span data-stu-id="adf60-127">Kestrel</span></span>

<span data-ttu-id="adf60-128">Kestrel は、ASP.NET Core のプロジェクト テンプレートに含まれる既定の Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="adf60-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="adf60-129">Kestrel は次のように使用できます。</span><span class="sxs-lookup"><span data-stu-id="adf60-129">Kestrel can be used:</span></span>

* <span data-ttu-id="adf60-130">これ自体で、インターネットを含むネットワークから直接要求を処理するエッジ サーバーとして。</span><span class="sxs-lookup"><span data-stu-id="adf60-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="adf60-131">[インターネット インフォメーション サービス (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org)、[Apache](https://httpd.apache.org/) などの*リバース プロキシ サーバー*と共に。</span><span class="sxs-lookup"><span data-stu-id="adf60-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="adf60-132">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、これを Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="adf60-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="adf60-135">リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="adf60-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="adf60-136">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="adf60-137">アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="adf60-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="adf60-139">アプリをインターネットに公開する場合は、Kestrel が[インターネット インフォメーション サービス (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org)、[Apache](https://httpd.apache.org/) などの*リバース プロキシ サーバー*を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="adf60-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="adf60-140">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、これを Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="adf60-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="adf60-142">インターネットに直接公開される一般向けのエッジ サーバー展開でリバース プロキシを使用する最も重要な理由は、セキュリティです。</span><span class="sxs-lookup"><span data-stu-id="adf60-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="adf60-143">1.x バージョンの Kestrel はインターネットからの攻撃を防御する重要なセキュリティ機能を備えていません。</span><span class="sxs-lookup"><span data-stu-id="adf60-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="adf60-144">これには、適切なタイムアウト、要求サイズの制限、およびコンカレント接続の制限などが含まれます (ただし、これらに限定されない)。</span><span class="sxs-lookup"><span data-stu-id="adf60-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="adf60-145">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="adf60-146">IIS と Kestrel</span><span class="sxs-lookup"><span data-stu-id="adf60-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="adf60-147">[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用している場合、ASP.NET Core アプリは、IIS ワーカー プロセスと同じプロセス (*インプロセス* ホスティング モデル) または IIS ワーカー プロセスとは別のプロセス (*アウトプロセス* ホスティング モデル) のいずれかで実行されます。</span><span class="sxs-lookup"><span data-stu-id="adf60-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="adf60-148">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)は、インプロセスの IIS HTTP サーバーまたはアウトプロセスの Kestrel サーバーのいずれかの間でネイティブの IIS 要求を処理するネイティブの IIS モジュールです。</span><span class="sxs-lookup"><span data-stu-id="adf60-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="adf60-149">詳細については、「<xref:fundamentals/servers/aspnet-core-module>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="adf60-150">ASP.NET Core のリバース プロキシとして [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) を使用する場合、ASP.NET Core アプリは IIS ワーカー プロセスとは別のプロセスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="adf60-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="adf60-151">IIS プロセスで、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がリバース プロキシの関係を調整します。</span><span class="sxs-lookup"><span data-stu-id="adf60-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="adf60-152">ASP.NET Core モジュールの主な機能は、アプリを開始し、クラッシュ時にアプリを再始動し、HTTP トラフィックをアプリに転送することです。</span><span class="sxs-lookup"><span data-stu-id="adf60-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="adf60-153">詳細については、「<xref:fundamentals/servers/aspnet-core-module>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="adf60-154">Nginx と Kestrel</span><span class="sxs-lookup"><span data-stu-id="adf60-154">Nginx with Kestrel</span></span>

<span data-ttu-id="adf60-155">Kestrel のリバース プロキシ サーバーとして Linux で Nginx を使用する方法については、<xref:host-and-deploy/linux-nginx> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="adf60-156">Apache と Kestrel</span><span class="sxs-lookup"><span data-stu-id="adf60-156">Apache with Kestrel</span></span>

<span data-ttu-id="adf60-157">Kestrel のリバース プロキシ サーバーとして Linux で Apache を使用する方法については、<xref:host-and-deploy/linux-apache> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="adf60-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="adf60-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="adf60-159">Windows で ASP.NET Core アプリを実行する場合は、HTTP.sys を Kestrel の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="adf60-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="adf60-160">最適なパフォーマンスを得るには、通常は Kestrel をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="adf60-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="adf60-161">HTTP.sys は、アプリがインターネットに公開されていて、必要な機能が HTTP.sys でサポートされているものの、Kestrel ではサポートされていないシナリオで使用できます。</span><span class="sxs-lookup"><span data-stu-id="adf60-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="adf60-162">詳細については、「<xref:fundamentals/servers/httpsys>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="adf60-164">HTTP.sys は、内部ネットワークにのみ公開されるアプリにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="adf60-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="adf60-166">ASP.NET Core 1.x では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="adf60-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="adf60-167">IIS がホストアプリで使用できないシナリオにおいて、Windows で ASP.NET Core アプリを実行する場合は、WebListener を代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="adf60-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="adf60-169">内部ネットワークにのみ公開されるアプリで、必要な機能が WebListener でサポートされていて、Kestrel ではサポートされていない場合、Kestrel の代わりに WebListener も使用できます。</span><span class="sxs-lookup"><span data-stu-id="adf60-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="adf60-170">WebListener の情報については、[WebListener](xref:fundamentals/servers/weblistener) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![内部ネットワークと直接通信する WebListener](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="adf60-172">ASP.NET Core サーバー インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="adf60-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="adf60-173">`Startup.Configure` メソッドで使用できる [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) は、[IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection) 型の [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="adf60-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="adf60-174">Kestrel および HTTP.sys (ASP.NET Core 1.x の WebListener) は、それぞれ単独の機能 [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) のみを公開しますが、サーバー実装が異なると追加機能が公開される場合があります。</span><span class="sxs-lookup"><span data-stu-id="adf60-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="adf60-175">`IServerAddressesFeature` を使用すれば、実行時にサーバー実装がバインドされたポートを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="adf60-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="adf60-176">カスタム サーバー</span><span class="sxs-lookup"><span data-stu-id="adf60-176">Custom servers</span></span>

<span data-ttu-id="adf60-177">組み込みサーバーがアプリの要件に合わない場合は、カスタム サーバー実装を作成できます。</span><span class="sxs-lookup"><span data-stu-id="adf60-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="adf60-178">[Nowin](https://github.com/Bobris/Nowin) ベースの [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) 実装の作成方法については、[Open Web Interface for .NET (OWIN) のガイド](xref:fundamentals/owin)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="adf60-179">実装を必要とするのは、アプリで使用される機能のインターフェイスのみです。ただし、少なくとも [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) と [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) はサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="adf60-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="adf60-180">サーバーの起動</span><span class="sxs-lookup"><span data-stu-id="adf60-180">Server startup</span></span>

<span data-ttu-id="adf60-181">[Visual Studio](https://www.visualstudio.com/vs/)、[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/)、または [Visual Studio Code](https://code.visualstudio.com/) を使用する場合、サーバーはアプリが統合開発環境 (IDE) によって開始されたときに起動します。</span><span class="sxs-lookup"><span data-stu-id="adf60-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="adf60-182">Windows の Visual Studio では、[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)またはコンソールで、起動プロファイルを使用してアプリとサーバーを開始することができます。</span><span class="sxs-lookup"><span data-stu-id="adf60-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="adf60-183">Visual Studio Code では、CoreCLR デバッガーをアクティブ化する [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) によって、アプリとサーバーが開始します。</span><span class="sxs-lookup"><span data-stu-id="adf60-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="adf60-184">Visual Studio for Mac を使用する場合は、アプリとサーバーが [Mono の Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) によって開始します。</span><span class="sxs-lookup"><span data-stu-id="adf60-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="adf60-185">コマンド プロンプトからプロジェクトのフォルダーでアプリを起動する場合、[dotnet run](/dotnet/core/tools/dotnet-run) でアプリとサーバーが起動します (Kestrel および HTTP.sys のみ)。</span><span class="sxs-lookup"><span data-stu-id="adf60-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="adf60-186">この構成は、`Debug` (既定) または `Release` のどちらかに設定された `-c|--configuration` オプションによって指定されます。</span><span class="sxs-lookup"><span data-stu-id="adf60-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="adf60-187">起動プロファイルが *launchSettings.json* ファイルに存在する場合は、`--launch-profile <NAME>` オプションを使用して起動プロファイルを設定します (`Development`、`Production` など)。</span><span class="sxs-lookup"><span data-stu-id="adf60-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="adf60-188">詳細については、[dotnet run](/dotnet/core/tools/dotnet-run) と [.NET Core の配布パッケージ](/dotnet/core/build/distribution-packaging)のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="adf60-189">HTTP/2 のサポート</span><span class="sxs-lookup"><span data-stu-id="adf60-189">HTTP/2 support</span></span>

<span data-ttu-id="adf60-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) は、次の展開シナリオでの ASP.NET Core でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="adf60-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="adf60-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="adf60-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="adf60-192">オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="adf60-192">Operating system</span></span>
    * <span data-ttu-id="adf60-193">Windows Server 2016/Windows 10 以降&dagger;</span><span class="sxs-lookup"><span data-stu-id="adf60-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="adf60-194">OpenSSL 1.0.2 以降を使用した Linux (Ubuntu 16.04 以降など)</span><span class="sxs-lookup"><span data-stu-id="adf60-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="adf60-195">将来のリリースでは HTTP/2 が macOS 上でサポートされるようになります。</span><span class="sxs-lookup"><span data-stu-id="adf60-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="adf60-196">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="adf60-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="adf60-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="adf60-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="adf60-198">Windows Server 2016/Windows 10 以降</span><span class="sxs-lookup"><span data-stu-id="adf60-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="adf60-199">ターゲット フレームワーク: HTTP.sys の展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="adf60-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="adf60-200">IIS (インプロセス)</span><span class="sxs-lookup"><span data-stu-id="adf60-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="adf60-201">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="adf60-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="adf60-202">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="adf60-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="adf60-203">IIS (アウトプロセス)</span><span class="sxs-lookup"><span data-stu-id="adf60-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="adf60-204">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="adf60-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="adf60-205">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="adf60-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="adf60-206">ターゲット フレームワーク: IIS アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="adf60-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="adf60-207">&dagger;Kestrel では、Windows Server 2012 R2 および Windows 8.1 上での HTTP/2 のサポートは制限されています。</span><span class="sxs-lookup"><span data-stu-id="adf60-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="adf60-208">サポートが制限されている理由は、これらのオペレーティング システムで使用できる TLS 暗号のスイートのリストが制限されているためです。</span><span class="sxs-lookup"><span data-stu-id="adf60-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="adf60-209">TLS 接続をセキュリティで保護するためには、楕円曲線デジタル署名アルゴリズム (ECDSA) を使用して生成した証明書が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="adf60-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="adf60-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="adf60-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="adf60-211">Windows Server 2016/Windows 10 以降</span><span class="sxs-lookup"><span data-stu-id="adf60-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="adf60-212">ターゲット フレームワーク: HTTP.sys の展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="adf60-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="adf60-213">IIS (アウトプロセス)</span><span class="sxs-lookup"><span data-stu-id="adf60-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="adf60-214">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="adf60-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="adf60-215">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、Kestrel へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="adf60-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="adf60-216">ターゲット フレームワーク: IIS アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="adf60-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="adf60-217">HTTP/2 接続では、[アプリケーション レイヤー プロトコルのネゴシエーション (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) および TLS 1.2 以降を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="adf60-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="adf60-218">詳細については、ご利用のサーバーの展開シナリオに関連するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="adf60-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="adf60-219">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="adf60-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="adf60-220"><xref:fundamentals/servers/httpsys> (ASP.NET Core 1.x については、<xref:fundamentals/servers/weblistener> を参照してください)</span><span class="sxs-lookup"><span data-stu-id="adf60-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
