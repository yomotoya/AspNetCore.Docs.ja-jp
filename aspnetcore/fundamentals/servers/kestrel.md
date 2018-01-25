---
title: "ASP.NET Core の kestrel web サーバーの実装"
author: tdykstra
description: "Libuv に基づいて ASP.NET Core の Kestrel、クロスプラット フォームの web サーバーを紹介します。"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e2b28f15e47789ac89213e57396060ee356ee33
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d16e4-103">ASP.NET Core の Kestrel web サーバーの実装の概要</span><span class="sxs-lookup"><span data-stu-id="d16e4-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d16e4-104">によって[Tom Dykstra](https://github.com/tdykstra)、 [Chris Ross](https://github.com/Tratcher)、および[Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="d16e4-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="d16e4-105">Kestrel はクロスプラット フォーム[ASP.NET core web server](index.md)に基づいて[libuv](https://github.com/libuv/libuv)プラットフォーム間の非同期 I/O ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="d16e4-106">Kestrel は、既定では ASP.NET Core プロジェクト テンプレートに含まれている web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="d16e4-107">Kestrel には、次の機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d16e4-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="d16e4-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d16e4-108">HTTPS</span></span>
  * <span data-ttu-id="d16e4-109">有効にするために使用する不透明なアップグレード[Websocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="d16e4-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="d16e4-110">Nginx の背後にある高パフォーマンスの Unix ソケット</span><span class="sxs-lookup"><span data-stu-id="d16e4-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="d16e4-111">すべてのプラットフォームと .NET Core をサポートするバージョンでは、kestrel をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d16e4-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d16e4-113">[表示または 2.x のサンプル コードをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d16e4-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d16e4-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d16e4-115">[表示または 1.x のサンプル コードをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d16e4-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="d16e4-116">リバース プロキシで Kestrel を使用する場合</span><span class="sxs-lookup"><span data-stu-id="d16e4-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d16e4-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d16e4-118">Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="d16e4-119">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d16e4-122">いずれの構成 &mdash; リバース プロキシ サーバーがある場合とない場合 &mdash; も、Kestrel が内部ネットワークにのみ公開される場合にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d16e4-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d16e4-124">アプリケーションが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="d16e4-126">アプリケーションをインターネットに公開する場合は、*リバース プロキシ サーバー*として IIS、Nginx、または Apache を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d16e4-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="d16e4-127">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d16e4-129">リバース プロキシは、セキュリティ上の理由のエッジの展開 (インターネットからのトラフィックに対して公開) 必要があります。</span><span class="sxs-lookup"><span data-stu-id="d16e4-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="d16e4-130">1.x バージョンの Kestrel には、攻撃に対する防御の全装備が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="d16e4-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="d16e4-131">これが含まれていますが、適切なタイムアウト、サイズの制限、および同時接続の制限に限定されません。</span><span class="sxs-lookup"><span data-stu-id="d16e4-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="d16e4-132">リバース プロキシを必要とするシナリオ、同じ IP と 1 台のサーバーで実行されているポートを共有する複数のアプリケーションがある場合です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="d16e4-133">それでも Kestrel で直接 Kestrel が同じ IP と複数のプロセス間でポート共有をサポートしていないためです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="d16e4-134">ポートでリッスンするように Kestrel を構成するときに、ホスト ヘッダーに関係なく、そのポートのすべてのトラフィックを処理します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="d16e4-135">リバース プロキシ ポートを共有できる、Kestrel 一意の IP とポートでに転送する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d16e4-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="d16e4-136">場合でも、リバース プロキシ サーバーは必要ありません、いずれかの方法と、他の理由により、適切な選択可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d16e4-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="d16e4-137">公開されている領域を制限することができます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="d16e4-138">省略可能な追加の構成と多層の層を提供します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="d16e4-139">既存のインフラストラクチャをより適切に統合可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d16e4-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="d16e4-140">これには、負荷分散と SSL のセットアップが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="d16e4-141">リバース プロキシ サーバーのみが、SSL 証明書を必要と、そのサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリケーション サーバーと通信できます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="d16e4-142">ASP.NET Core アプリケーションで Kestrel を使用する方法</span><span class="sxs-lookup"><span data-stu-id="d16e4-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d16e4-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d16e4-144">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)にパッケージが含まれる、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="d16e4-145">ASP.NET Core プロジェクト テンプレートは、既定では Kestrel を使用します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="d16e4-146">*Program.cs*、テンプレート コードの呼び出し`CreateDefaultBuilder`、どの呼び出し[UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)バック グラウンドでします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="d16e4-147">Kestrel オプションを構成する必要がある場合は、呼び出す`UseKestrel`で*Program.cs*次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d16e4-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d16e4-149">インストール、 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="d16e4-150">呼び出す、 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)拡張メソッドを`WebHostBuilder`で、`Main`いずれかを指定してメソッド[Kestrel オプション](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)する必要がある、次のセクションで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="d16e4-151">Kestrel オプション</span><span class="sxs-lookup"><span data-stu-id="d16e4-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d16e4-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d16e4-153">Kestrel web サーバーには、インターネットに接続された環境で特に便利な制約の構成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="d16e4-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="d16e4-154">次に設定できる値の範囲を示します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="d16e4-155">クライアントの最大接続数</span><span class="sxs-lookup"><span data-stu-id="d16e4-155">Maximum client connections</span></span>
- <span data-ttu-id="d16e4-156">要求本文の最大サイズ</span><span class="sxs-lookup"><span data-stu-id="d16e4-156">Maximum request body size</span></span>
- <span data-ttu-id="d16e4-157">要求本文の最小レート</span><span class="sxs-lookup"><span data-stu-id="d16e4-157">Minimum request body data rate</span></span>

<span data-ttu-id="d16e4-158">これらの制約と内の他に設定する、`Limits`のプロパティ、 [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)クラスです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="d16e4-159">`Limits`プロパティのインスタンスを保持する、 [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)クラスです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="d16e4-160">**最大クライアント接続**</span><span class="sxs-lookup"><span data-stu-id="d16e4-160">**Maximum client connections**</span></span>

<span data-ttu-id="d16e4-161">次のコード全体のアプリケーションの同時実行の開いている TCP 接続の最大数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="d16e4-162">(たとえば、Websocket 要求など) 上の別のプロトコルに HTTP または HTTPS からアップグレードした接続に対して個別の制限があります。</span><span class="sxs-lookup"><span data-stu-id="d16e4-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="d16e4-163">接続がアップグレードされた後に照らし合わせてカウントされるされていない、`MaxConcurrentConnections`制限します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="d16e4-164">接続の最大数は、既定で無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="d16e4-165">**最大要求本文のサイズ**</span><span class="sxs-lookup"><span data-stu-id="d16e4-165">**Maximum request body size**</span></span>

<span data-ttu-id="d16e4-166">既定の最大要求本文のサイズは、約 28.6 MB である 30,000,000 (バイト単位) です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="d16e4-167">ASP.NET Core MVC アプリの制限を上書きすることをお勧めを使用して、 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)アクション メソッドの属性。</span><span class="sxs-lookup"><span data-stu-id="d16e4-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d16e4-168">全体のアプリケーションでは、すべての要求の制約を構成する方法を示す例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="d16e4-169">ミドルウェア内で特定の要求の設定を上書きできます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="d16e4-170">アプリケーションの要求の読み取りが開始した後、要求の制限を構成しようとする場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="d16e4-171">`IsReadOnly`プロパティを示す場合、`MaxRequestBodySize`プロパティは読み取り専用状態で制限を構成するには遅すぎますを意味します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d16e4-172">**最小の要求本文データ レート**</span><span class="sxs-lookup"><span data-stu-id="d16e4-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="d16e4-173">データが送信される場合で指定したレートでバイト数/秒、kestrel は毎秒をチェックします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="d16e4-174">速度が、最小値を下回る場合、接続がタイムアウトしました。Kestrel は、最小; まで、送信速度を向上させるクライアントを時間は、猶予期間はこの期間中、レートがオフになってです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="d16e4-175">猶予期間により、TCP 速度の遅い起動のための低速なレートで最初にデータを送信する接続を削除しないでください。</span><span class="sxs-lookup"><span data-stu-id="d16e4-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="d16e4-176">既定の最小間隔は、240 バイト/秒、5 秒の猶予時間です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="d16e4-177">最短間隔は、応答にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="d16e4-178">要求の制限と応答の上限を設定するコードがあることを除いて同じ`RequestBody`または`Response`プロパティおよびインターフェイスの名前でします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="d16e4-179">最小限のデータ間隔を構成する方法を示す例を次に示します*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d16e4-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="d16e4-180">ミドルウェア内で 1 回の要求レートを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="d16e4-181">その他の Kestrel オプションについては、次のクラスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d16e4-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="d16e4-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="d16e4-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="d16e4-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="d16e4-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="d16e4-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="d16e4-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d16e4-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d16e4-186">Kestrel オプションについては、次を参照してください。 [KestrelServerOptions クラス](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="d16e4-187">エンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="d16e4-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d16e4-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d16e4-189">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d16e4-190">URL プレフィックスと Kestrel を呼び出すことによってリッスン用のポートを構成する`Listen`または`ListenUnixSocket`メソッド`KestrelServerOptions`です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="d16e4-191">(`UseUrls`、`urls`コマンドラインの引数とも作業 ASPNETCORE_URLS 環境変数は、説明した制限がある[この記事で後述](#useurls-limitations))。</span><span class="sxs-lookup"><span data-stu-id="d16e4-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="d16e4-192">**TCP ソケットをバインドします。**</span><span class="sxs-lookup"><span data-stu-id="d16e4-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="d16e4-193">`Listen`メソッドは、TCP ソケットをバインドし、オプション ラムダでは、SSL 証明書を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="d16e4-194">どのようにこの例 SSL の構成、特定のエンドポイントを使用してに注意してください。 [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="d16e4-195">同じ API を使用すると、特定のエンドポイントの場合は、その他の Kestrel 設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="d16e4-196">**Unix ソケットにバインドします。**</span><span class="sxs-lookup"><span data-stu-id="d16e4-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="d16e4-197">この例で示すようには、Nginx、によるパフォーマンスの向上の Unix ソケットでリッスンできます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="d16e4-198">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="d16e4-198">**Port 0**</span></span>

<span data-ttu-id="d16e4-199">ポート番号を 0 を指定すると、Kestrel は動的に使用可能なポートにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d16e4-200">次の例では、Kestrel が実際に実行時にバインドされるポートを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="d16e4-201">**UseUrls の制限事項**</span><span class="sxs-lookup"><span data-stu-id="d16e4-201">**UseUrls limitations**</span></span>

<span data-ttu-id="d16e4-202">エンドポイントを構成するには呼び出すことによって、`UseUrls`メソッドまたはを使用して、`urls`コマンドライン引数または ASPNETCORE_URLS 環境変数。</span><span class="sxs-lookup"><span data-stu-id="d16e4-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="d16e4-203">これらのメソッドは Kestrel 以外のサーバーを使用するコードを作成する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="d16e4-204">ただし、これらの制限に注意してください。</span><span class="sxs-lookup"><span data-stu-id="d16e4-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="d16e4-205">これらのメソッドで SSL を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="d16e4-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="d16e4-206">両方を使用する場合、`Listen`メソッドおよび`UseUrls`、`Listen`エンドポイントを上書き、`UseUrls`エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="d16e4-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="d16e4-207">**IIS のエンドポイントの構成**</span><span class="sxs-lookup"><span data-stu-id="d16e4-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="d16e4-208">IIS の URL のバインディングがいずれかを呼び出して設定するすべてのバインドを上書き IIS を使用する場合`Listen`または`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="d16e4-209">詳細については、次を参照してください。 [Introduction to ASP.NET Core モジュール](aspnet-core-module.md)です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d16e4-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d16e4-211">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d16e4-212">URL プレフィックスおよび Kestrel を使用してリッスンするポートを構成することができます、`UseUrls`の拡張メソッドで、`urls`コマンドライン引数、または ASP.NET Core の構成システムです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="d16e4-213">これらのメソッドの詳細については、次を参照してください。[ホスティング](../../fundamentals/hosting.md)です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="d16e4-214">リバース プロキシとして IIS を使用するときの URL のバインドの動作方法の詳細については、次を参照してください。 [ASP.NET Core モジュール](aspnet-core-module.md)です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="d16e4-215">URL プレフィックス</span><span class="sxs-lookup"><span data-stu-id="d16e4-215">URL prefixes</span></span>

<span data-ttu-id="d16e4-216">呼び出す場合`UseUrls`か使用して、`urls`コマンドライン引数または ASPNETCORE_URLS 環境変数、URL プレフィックス可能で、次の形式のいずれか。</span><span class="sxs-lookup"><span data-stu-id="d16e4-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d16e4-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d16e4-218">HTTP URL プレフィックスのみが無効です。使用してバインディングを URL を構成するときに、kestrel は SSL をサポートしない`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="d16e4-219">ポート番号を含む IPv4 アドレス</span><span class="sxs-lookup"><span data-stu-id="d16e4-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="d16e4-220">0.0.0.0 は、すべての IPv4 アドレスにバインドする特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="d16e4-221">ポート番号を含む IPv6 アドレス</span><span class="sxs-lookup"><span data-stu-id="d16e4-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="d16e4-222">[::] は、ipv4、IPv6 該当するショートカットは 0.0.0.0 です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="d16e4-223">ポート番号を持つホスト名</span><span class="sxs-lookup"><span data-stu-id="d16e4-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="d16e4-224">ホスト名、\* と +, ではありません。</span><span class="sxs-lookup"><span data-stu-id="d16e4-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="d16e4-225">認識されている IP アドレスまたは"localhost"ではないものは、すべての IPv4 および IPv6 ip アドレスにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d16e4-226">別のホスト名を同じポート上の別の ASP.NET Core アプリケーションにバインドする必要がある場合を使用して[HTTP.sys](httpsys.md)または IIS、Nginx、Apache などのリバース プロキシ サーバー。</span><span class="sxs-lookup"><span data-stu-id="d16e4-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="d16e4-227">ポート番号またはループバック IP アドレスとポート番号を"Localhost"の名前</span><span class="sxs-lookup"><span data-stu-id="d16e4-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d16e4-228">ときに`localhost`Kestrel IPv4 と IPv6 の両方のループバック インターフェイスにバインドしようとするを指定します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d16e4-229">いずれのループバック インターフェイス上の別のサービスで使用中では、要求されたポートを開始する Kestrel は失敗します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d16e4-230">かどうか、ループバック インターフェイスは使用できません何らかの理由 (ほとんど IPv6 がサポートされていないためによく)、Kestrel 警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d16e4-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="d16e4-232">ポート番号を含む IPv4 アドレス</span><span class="sxs-lookup"><span data-stu-id="d16e4-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="d16e4-233">0.0.0.0 は、すべての IPv4 アドレスにバインドする特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="d16e4-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="d16e4-234">ポート番号を含む IPv6 アドレス</span><span class="sxs-lookup"><span data-stu-id="d16e4-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="d16e4-235">[::] は、ipv4、IPv6 該当するショートカットは 0.0.0.0 です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="d16e4-236">ポート番号を持つホスト名</span><span class="sxs-lookup"><span data-stu-id="d16e4-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="d16e4-237">ホスト名、 \*、および特殊ないない + です。</span><span class="sxs-lookup"><span data-stu-id="d16e4-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="d16e4-238">認識される IP アドレスまたは"localhost"がないものは、すべての IPv4 および IPv6 ip アドレスにバインドします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d16e4-239">別のホスト名を同じポート上の別の ASP.NET Core アプリケーションにバインドする必要がある場合を使用して[WebListener](weblistener.md)または IIS、Nginx、Apache などのリバース プロキシ サーバー。</span><span class="sxs-lookup"><span data-stu-id="d16e4-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="d16e4-240">ポート番号またはループバック IP アドレスとポート番号を"Localhost"の名前</span><span class="sxs-lookup"><span data-stu-id="d16e4-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d16e4-241">ときに`localhost`Kestrel IPv4 と IPv6 の両方のループバック インターフェイスにバインドしようとするを指定します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d16e4-242">いずれのループバック インターフェイス上の別のサービスで使用中では、要求されたポートを開始する Kestrel は失敗します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d16e4-243">かどうか、ループバック インターフェイスは使用できません何らかの理由 (ほとんど IPv6 がサポートされていないためによく)、Kestrel 警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="d16e4-244">Unix ソケット</span><span class="sxs-lookup"><span data-stu-id="d16e4-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="d16e4-245">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="d16e4-245">**Port 0**</span></span>

<span data-ttu-id="d16e4-246">ポート番号を 0 を指定すると、Kestrel は動的に使用可能なポートにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="d16e4-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d16e4-247">ホスト名または IP を除く任意のポート 0 へのバインドは許可されて`localhost`名。</span><span class="sxs-lookup"><span data-stu-id="d16e4-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="d16e4-248">次の例では、Kestrel が実際に実行時にバインドされるポートを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d16e4-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="d16e4-249">**SSL 用の URL のプレフィックス**</span><span class="sxs-lookup"><span data-stu-id="d16e4-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="d16e4-250">持つ URL プレフィックスを含めてください`https:`を呼び出す場合は、`UseHttps`拡張メソッドを次のようにします。</span><span class="sxs-lookup"><span data-stu-id="d16e4-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="d16e4-251">同じポートでは、HTTPS と HTTP をホストすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d16e4-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="d16e4-252">次の手順</span><span class="sxs-lookup"><span data-stu-id="d16e4-252">Next steps</span></span>

<span data-ttu-id="d16e4-253">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d16e4-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d16e4-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="d16e4-255">2.x 用のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="d16e4-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="d16e4-256">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="d16e4-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d16e4-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d16e4-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="d16e4-258">1.x 用のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="d16e4-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="d16e4-259">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="d16e4-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
