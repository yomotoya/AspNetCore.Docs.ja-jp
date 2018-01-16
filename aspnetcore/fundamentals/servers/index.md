---
title: "ASP.NET Core での Web サーバーの実装"
author: tdykstra
description: "ASP.NET Core の Web サーバーである Kestrel と WebListener の概要を示します。 いずれかを選択する方法と、リバース プロキシ サーバーでいずれかを使用するタイミングに関するガイダンスを提供します。"
keywords: "ASP.NET Core, IServer, Web サーバー, Kestrel, WebListener, リバース プロキシ"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: b149cb316e4266e67d846b8ef8c2c7f2a25ded5c
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="ca26f-105">ASP.NET Core での Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="ca26f-105">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="ca26f-106">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Steve Smith](https://ardalis.com/)、[Stephen Halter](https://twitter.com/halter73)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ca26f-106">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="ca26f-107">ASP.NET Core アプリケーションは、インプロセス HTTP サーバー実装を使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-107">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="ca26f-108">サーバー実装では HTTP 要求をリッスンし、`HttpContext` に構成された[要求機能](https://docs.microsoft.com/aspnet/core/fundamentals/request-features)のセットとしてアプリケーションに公開します。</span><span class="sxs-lookup"><span data-stu-id="ca26f-108">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="ca26f-109">ASP.NET Core には次の 2 つのサーバー実装が用意されています。</span><span class="sxs-lookup"><span data-stu-id="ca26f-109">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ca26f-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="ca26f-111">[Kestrel](kestrel.md) は、クロスプラットフォームの非同期 I/O ライブラリである [libuv](https://github.com/libuv/libuv) に基づくクロスプラットフォームの HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="ca26f-111">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="ca26f-112">[HTTP.sys](httpsys.md) は、[Http.Sys カーネル ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="ca26f-112">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ca26f-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="ca26f-114">[Kestrel](kestrel.md) は、クロスプラットフォームの非同期 I/O ライブラリである [libuv](https://github.com/libuv/libuv) に基づくクロスプラットフォームの HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="ca26f-114">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="ca26f-115">[WebListener](weblistener.md) は、[Http.Sys カーネル ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づく Windows 専用の HTTP サーバーです。</span><span class="sxs-lookup"><span data-stu-id="ca26f-115">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="ca26f-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ca26f-116">Kestrel</span></span>

<span data-ttu-id="ca26f-117">Kestrel は、ASP.NET Core の新しいプロジェクト テンプレートに既定で含まれる Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="ca26f-117">Kestrel is the web server that is included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ca26f-118">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ca26f-119">Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="ca26f-120">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="ca26f-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ca26f-123">いずれの構成 &mdash; リバース プロキシ サーバーがある場合とない場合 &mdash; も、Kestrel が内部ネットワークにのみ公開される場合にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="ca26f-124">Kestrel とリバース プロキシを併用するタイミングについては、[Kestrel の概要](kestrel.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-124">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ca26f-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ca26f-126">アプリケーションが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-126">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="ca26f-128">アプリケーションをインターネットに公開する場合は、*リバース プロキシ サーバー*として IIS、Nginx、または Apache を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca26f-128">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="ca26f-129">以下の図のように、リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="ca26f-129">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ca26f-131">エッジ展開 (インターネットからのトラフィックに公開される) でリバース プロキシを使用する最も重要な理由は、セキュリティです。</span><span class="sxs-lookup"><span data-stu-id="ca26f-131">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="ca26f-132">1.x バージョンの Kestrel には、攻撃に対する防御の全装備が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="ca26f-132">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="ca26f-133">これには、適切なタイムアウト、サイズ制限、および同時接続の制限などが含まれます (ただし、これらに限定されない)。</span><span class="sxs-lookup"><span data-stu-id="ca26f-133">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="ca26f-134">Kestrel とリバース プロキシを併用するタイミングについては、[Kestrel の概要](kestrel.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-134">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="ca26f-135">IIS、Nginx、または Apache を Kestrel や[カスタム サーバー実装](#custom-servers)なしで使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ca26f-135">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="ca26f-136">ASP.NET Core は、プラットフォーム間で一貫して動作できるように、独自のプロセスで実行するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="ca26f-136">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="ca26f-137">IIS、Nginx、および Apache では独自のスタートアップ プロセスと環境が指示されます。それらを直接使用するには、ASP.NET Core はそれぞれのニーズに適応する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca26f-137">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="ca26f-138">Kestrel などの Web サーバー実装を使用することで、ASP.NET Core はスタートアップ プロセスと環境を制御できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-138">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="ca26f-139">したがって、ASP.NET Core を IIS、Nginx、または Apache に適応するのではなく、Kestrel に要求をプロキシするようにこれらの Web サーバーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ca26f-139">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="ca26f-140">このようにすれば、展開場所に関係なく、`Program.Main` および `Startup` クラスを本質的に同じにすることができます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-140">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="ca26f-141">IIS と Kestrel</span><span class="sxs-lookup"><span data-stu-id="ca26f-141">IIS with Kestrel</span></span>

<span data-ttu-id="ca26f-142">ASP.NET Core のリバース プロキシとして IIS または IIS Express を使用する場合、ASP.NET Core アプリケーションは IIS ワーカー プロセスとは別のプロセスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-142">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="ca26f-143">IIS プロセスでは、リバース プロキシの関係を調整するために特別な IIS モジュールが実行されます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-143">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="ca26f-144">これは *ASP.NET Core モジュール*です。</span><span class="sxs-lookup"><span data-stu-id="ca26f-144">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="ca26f-145">ASP.NET Core モジュールの主な機能は、ASP.NET Core アプリケーションを開始し、クラッシュ時に再始動し、HTTP トラフィックを転送することです。</span><span class="sxs-lookup"><span data-stu-id="ca26f-145">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="ca26f-146">詳細については、[ASP.NET Core モジュール](aspnet-core-module.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-146">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="ca26f-147">Nginx と Kestrel</span><span class="sxs-lookup"><span data-stu-id="ca26f-147">Nginx with Kestrel</span></span>

<span data-ttu-id="ca26f-148">Kestrel のリバース プロキシ サーバーとして Linux で Nginx を使用する方法については、「[Nginx による Linux でのホスト](xref:host-and-deploy/linux-nginx)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-148">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="ca26f-149">Apache と Kestrel</span><span class="sxs-lookup"><span data-stu-id="ca26f-149">Apache with Kestrel</span></span>

<span data-ttu-id="ca26f-150">Kestrel のリバース プロキシ サーバーとして Linux で Apache を使用する方法については、「[Apache による Linux でのホスト](xref:host-and-deploy/linux-apache)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-150">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="ca26f-151">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ca26f-151">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ca26f-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ca26f-153">Windows で ASP.NET Core アプリを実行する場合は、HTTP.sys を Kestrel の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-153">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="ca26f-154">アプリをインターネットに公開し、Kestrel でサポートされない HTTP.sys 機能が必要なシナリオの場合、HTTP.sys を使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-154">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="ca26f-156">HTTP.sys は、内部ネットワークにのみ公開されるアプリケーションにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-156">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="ca26f-158">内部ネットワークのシナリオの場合、最適なパフォーマンスを得るには、一般に Kestrel が推奨されますが、一部のシナリオでは、HTTP.sys のみで提供される機能を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-158">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="ca26f-159">HTTP.sys の機能については、[HTTP.sys](httpsys.md) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-159">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ca26f-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ca26f-161">ASP.NET Core 1.x では、HTTP.sys は WebListener と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-161">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="ca26f-162">Windows で ASP.NET Core アプリを実行する場合、アプリをインターネットに公開する際に IIS を使用できないシナリオで WebListener を代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-162">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="ca26f-164">Kestrel でサポートされない WebListener の機能が必要な場合は、内部ネットワークにのみ公開されるアプリケーションでも Kestrel の代わりに WebListener を使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-164">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![内部ネットワークと直接通信する Weblistener](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="ca26f-166">内部ネットワークのシナリオの場合、最適なパフォーマンスを得るには、一般に Kestrel が推奨されますが、一部のシナリオでは、WebListener のみで提供される機能を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-166">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="ca26f-167">WebListener の機能については、[WebListener](weblistener.md) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-167">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="ca26f-168">ASP.NET Core サーバー インフラストラクチャに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="ca26f-168">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="ca26f-169">`Startup` クラスの `Configure` メソッドで使用可能な [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) は、[`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection) 型の `ServerFeatures` プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="ca26f-169">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="ca26f-170">Kestrel と WebListener はどちらも [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) という 1 つの機能しか公開しませんが、異なるサーバー実装では追加の機能が公開される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ca26f-170">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="ca26f-171">`IServerAddressesFeature` を使用すれば、実行時にサーバー実装がバインドされたポートを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-171">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="ca26f-172">カスタム サーバー</span><span class="sxs-lookup"><span data-stu-id="ca26f-172">Custom servers</span></span>

<span data-ttu-id="ca26f-173">組み込みサーバーがニーズに合わない場合は、カスタム サーバー実装を作成できます。</span><span class="sxs-lookup"><span data-stu-id="ca26f-173">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="ca26f-174">[Nowin](https://github.com/Bobris/Nowin) ベースの [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) 実装の作成方法については、[Open Web Interface for .NET (OWIN) のガイド](../owin.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-174">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="ca26f-175">アプリケーションで必要な機能インターフェイスのみを実装するのは自由です。ただし、少なくとも [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) と [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca26f-175">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca26f-176">次の手順</span><span class="sxs-lookup"><span data-stu-id="ca26f-176">Next steps</span></span>

<span data-ttu-id="ca26f-177">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ca26f-177">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ca26f-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="ca26f-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ca26f-179">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="ca26f-180">Kestrel と IIS</span><span class="sxs-lookup"><span data-stu-id="ca26f-180">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="ca26f-181">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="ca26f-181">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="ca26f-182">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="ca26f-182">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="ca26f-183">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ca26f-183">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ca26f-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ca26f-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="ca26f-185">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ca26f-185">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="ca26f-186">Kestrel と IIS</span><span class="sxs-lookup"><span data-stu-id="ca26f-186">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="ca26f-187">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="ca26f-187">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="ca26f-188">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="ca26f-188">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="ca26f-189">WebListener</span><span class="sxs-lookup"><span data-stu-id="ca26f-189">WebListener</span></span>](weblistener.md)

---
