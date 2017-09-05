---
title: "ASP.NET Core の kestrel web サーバーの実装"
author: tdykstra
description: "Libuv に基づいて ASP.NET Core の Kestrel、クロスプラット フォームの web サーバーを紹介します。"
keywords: "ASP.NET Core、Kestrel、libuv、url のプレフィックス"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451a548403c8fa0ed2befeb6969a3ee28fe34790
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="10d2b-104">ASP.NET Core の Kestrel web サーバーの実装の概要</span><span class="sxs-lookup"><span data-stu-id="10d2b-104">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="10d2b-105">によって[Tom Dykstra](http://github.com/tdykstra)、 [Chris Ross](https://github.com/Tratcher)、および[Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="10d2b-105">By [Tom Dykstra](http://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="10d2b-106">Kestrel はクロスプラット フォーム[ASP.NET core web server](index.md)に基づいて[libuv](https://github.com/libuv/libuv)プラットフォーム間の非同期 I/O ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-106">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="10d2b-107">Kestrel は、既定では ASP.NET Core プロジェクト テンプレートに含まれている web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-107">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="10d2b-108">Kestrel には、次の機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="10d2b-108">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="10d2b-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="10d2b-109">HTTPS</span></span>
  * <span data-ttu-id="10d2b-110">有効にするために使用する不透明なアップグレード[Websocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="10d2b-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="10d2b-111">Nginx の背後にある高パフォーマンスの Unix ソケット</span><span class="sxs-lookup"><span data-stu-id="10d2b-111">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="10d2b-112">すべてのプラットフォームと .NET Core をサポートするバージョンでは、kestrel をサポートします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-112">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10d2b-113">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="10d2b-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="10d2b-114">表示または 2.x のサンプル コードをダウンロード</span><span class="sxs-lookup"><span data-stu-id="10d2b-114">View or download sample code for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10d2b-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10d2b-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="10d2b-116">表示または 1.x のサンプル コードをダウンロード</span><span class="sxs-lookup"><span data-stu-id="10d2b-116">View or download sample code for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="10d2b-117">リバース プロキシで Kestrel を使用する場合</span><span class="sxs-lookup"><span data-stu-id="10d2b-117">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10d2b-118">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="10d2b-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="10d2b-119">Kestrel を使用するには、単独またはで、*リバース プロキシ サーバー*IIS、Nginx、Apache などです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="10d2b-120">リバース プロキシ サーバーでは、インターネットから HTTP 要求を受け取り、いくつかの予備処理後に Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel がリバース プロキシ サーバーを使用せず、インターネットに直接通信します。](kestrel/_static/kestrel-to-internet2.png)

![Kestrel が IIS、Nginx、Apache などのリバース プロキシ サーバー経由でインターネットといない直接通信します。](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="10d2b-123">いずれかの構成&mdash;リバース プロキシ サーバーの有無&mdash;Kestrel が内部ネットワークにのみ公開される場合にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10d2b-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10d2b-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="10d2b-125">アプリケーションが内部ネットワークからのみ要求を受け入れる場合は、単独で Kestrel を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel が内部ネットワークに直接通信します。](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="10d2b-127">インターネット アプリケーションを公開する場合は、IIS、Nginx、またはとして Apache を使用する必要があります、*リバース プロキシ サーバー*です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="10d2b-128">リバース プロキシ サーバーでは、インターネットから HTTP 要求を受け取り、いくつかの予備処理後に Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel が IIS、Nginx、Apache などのリバース プロキシ サーバー経由でインターネットといない直接通信します。](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="10d2b-130">リバース プロキシは、セキュリティ上の理由のエッジの展開 (インターネットからのトラフィックに対して公開) 必要があります。</span><span class="sxs-lookup"><span data-stu-id="10d2b-130">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="10d2b-131">1.x バージョン Kestrel の攻撃に対する防御の完全な補数がありません。</span><span class="sxs-lookup"><span data-stu-id="10d2b-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="10d2b-132">これが含まれていますが、適切なタイムアウト、サイズの制限、および同時接続の制限に限定されません。</span><span class="sxs-lookup"><span data-stu-id="10d2b-132">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="10d2b-133">リバース プロキシを必要とするシナリオ、同じ IP と 1 台のサーバーで実行されているポートを共有する複数のアプリケーションがある場合です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-133">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="10d2b-134">それでも Kestrel で直接 Kestrel が同じ IP と複数のプロセス間でポート共有をサポートしていないためです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-134">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="10d2b-135">ポートでリッスンするように Kestrel を構成するときに、ホスト ヘッダーに関係なく、そのポートのすべてのトラフィックを処理します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-135">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="10d2b-136">リバース プロキシ ポートを共有できる、Kestrel 一意の IP とポートでに転送する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10d2b-136">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="10d2b-137">場合でも、リバース プロキシ サーバーは必要ありません、いずれかの方法と、他の理由により、適切な選択可能性があります。</span><span class="sxs-lookup"><span data-stu-id="10d2b-137">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="10d2b-138">公開されている領域を制限することができます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-138">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="10d2b-139">省略可能な追加の構成と多層の層を提供します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-139">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="10d2b-140">既存のインフラストラクチャをより適切に統合可能性があります。</span><span class="sxs-lookup"><span data-stu-id="10d2b-140">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="10d2b-141">これには、負荷分散と SSL のセットアップが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-141">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="10d2b-142">リバース プロキシ サーバーのみが、SSL 証明書を必要と、そのサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリケーション サーバーと通信できます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-142">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="10d2b-143">ASP.NET Core アプリケーションで Kestrel を使用する方法</span><span class="sxs-lookup"><span data-stu-id="10d2b-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10d2b-144">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="10d2b-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="10d2b-145">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)にパッケージが含まれる、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="10d2b-146">ASP.NET Core プロジェクト テンプレートは、既定では Kestrel を使用します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="10d2b-147">*Program.cs*、テンプレート コードの呼び出し`CreateDefaultBuilder`、どの呼び出し[UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)バック グラウンドでします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="10d2b-148">Kestrel オプションを構成する必要がある場合は、呼び出す`UseKestrel`で*Program.cs*次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10d2b-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10d2b-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="10d2b-150">インストール、 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="10d2b-151">呼び出す、 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)拡張メソッドを`WebHostBuilder`で、`Main`いずれかを指定してメソッド[Kestrel オプション](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)する必要がある、次のセクションで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="10d2b-152">Kestrel オプション</span><span class="sxs-lookup"><span data-stu-id="10d2b-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10d2b-153">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="10d2b-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="10d2b-154">Kestrel web サーバーには、インターネットに接続された環境で特に便利な制約の構成オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="10d2b-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="10d2b-155">次に設定できる値の範囲を示します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="10d2b-156">最大クライアント接続</span><span class="sxs-lookup"><span data-stu-id="10d2b-156">Maximum client connections</span></span>
- <span data-ttu-id="10d2b-157">最大要求本文のサイズ</span><span class="sxs-lookup"><span data-stu-id="10d2b-157">Maximum request body size</span></span>
- <span data-ttu-id="10d2b-158">最小の要求本文データ レート</span><span class="sxs-lookup"><span data-stu-id="10d2b-158">Minimum request body data rate</span></span>

<span data-ttu-id="10d2b-159">これらの制約と内の他に設定する、`Limits`のプロパティ、 [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)クラスです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="10d2b-160">`Limits`プロパティのインスタンスを保持する、 [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)クラスです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="10d2b-161">**最大クライアント接続**</span><span class="sxs-lookup"><span data-stu-id="10d2b-161">**Maximum client connections**</span></span>

<span data-ttu-id="10d2b-162">次のコード全体のアプリケーションの同時実行の開いている TCP 接続の最大数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="10d2b-163">(たとえば、Websocket 要求など) 上の別のプロトコルに HTTP または HTTPS からアップグレードした接続に対して個別の制限があります。</span><span class="sxs-lookup"><span data-stu-id="10d2b-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="10d2b-164">接続がアップグレードされた後に照らし合わせてカウントされるされていない、`MaxConcurrentConnections`制限します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="10d2b-165">接続の最大数は、既定で無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="10d2b-166">**最大要求本文のサイズ**</span><span class="sxs-lookup"><span data-stu-id="10d2b-166">**Maximum request body size**</span></span>

<span data-ttu-id="10d2b-167">既定の最大要求本文のサイズは、約 28.6 MB である 30,000,000 (バイト単位) です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="10d2b-168">ASP.NET Core MVC アプリの制限を上書きすることをお勧めを使用して、 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)アクション メソッドの属性。</span><span class="sxs-lookup"><span data-stu-id="10d2b-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="10d2b-169">全体のアプリケーションでは、すべての要求の制約を構成する方法を示す例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="10d2b-170">ミドルウェア内で特定の要求の設定を上書きできます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="10d2b-171">アプリケーションの要求の読み取りが開始した後、要求の制限を構成しようとする場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="10d2b-172">`IsReadOnly`プロパティを示す場合、`MaxRequestBodySize`プロパティは読み取り専用状態で制限を構成するには遅すぎますを意味します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="10d2b-173">**最小の要求本文データ レート**</span><span class="sxs-lookup"><span data-stu-id="10d2b-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="10d2b-174">データが送信される場合で指定したレートでバイト数/秒、kestrel は毎秒をチェックします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="10d2b-175">速度が、最小値を下回る場合、接続がタイムアウトしました。Kestrel は、最小; まで、送信速度を向上させるクライアントを時間は、猶予期間はレートは、この期間中はチェックされません。</span><span class="sxs-lookup"><span data-stu-id="10d2b-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="10d2b-176">猶予期間により、TCP 速度の遅い起動のための低速なレートで最初にデータを送信する接続を削除しないでください。</span><span class="sxs-lookup"><span data-stu-id="10d2b-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="10d2b-177">既定の最小間隔は、240 バイト/秒、5 秒の猶予時間です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="10d2b-178">最短間隔は、応答にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="10d2b-179">要求の制限と応答の上限を設定するコードがあることを除いて同じ`RequestBody`または`Response`プロパティおよびインターフェイスの名前でします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="10d2b-180">最小限のデータ間隔を構成する方法を示す例を次に示します*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="10d2b-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="10d2b-181">ミドルウェア内で 1 回の要求レートを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="10d2b-182">その他の Kestrel オプションについては、次のクラスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="10d2b-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="10d2b-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="10d2b-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="10d2b-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="10d2b-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="10d2b-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="10d2b-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10d2b-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10d2b-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="10d2b-187">Kestrel オプションについては、次を参照してください。 [KestrelServerOptions クラス](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="10d2b-188">エンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="10d2b-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10d2b-189">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="10d2b-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="10d2b-190">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="10d2b-191">URL プレフィックスと Kestrel を呼び出すことによってリッスン用のポートを構成する`Listen`または`ListenUnixSocket`メソッド`KestrelServerOptions`です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="10d2b-192">(`UseUrls`、`urls`コマンドラインの引数とも作業 ASPNETCORE_URLS 環境変数は、説明した制限がある[この記事で後述](#useurls-limitations))。</span><span class="sxs-lookup"><span data-stu-id="10d2b-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="10d2b-193">**TCP ソケットをバインドします。**</span><span class="sxs-lookup"><span data-stu-id="10d2b-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="10d2b-194">`Listen`メソッドは、TCP ソケットをバインドし、オプション ラムダでは、SSL 証明書を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="10d2b-195">どのようにこの例 SSL の構成、特定のエンドポイントを使用してに注意してください。 [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="10d2b-196">同じ API を使用すると、特定のエンドポイントの場合は、その他の Kestrel 設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="10d2b-197">**Unix ソケットにバインドします。**</span><span class="sxs-lookup"><span data-stu-id="10d2b-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="10d2b-198">この例で示すようには、Nginx、によるパフォーマンスの向上の Unix ソケットでリッスンできます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="10d2b-199">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="10d2b-199">**Port 0**</span></span>

<span data-ttu-id="10d2b-200">ポート番号を 0 を指定すると、Kestrel は動的に使用可能なポートにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="10d2b-201">次の例では、Kestrel が実際に実行時にバインドされるポートを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="10d2b-202">**UseUrls の制限事項**</span><span class="sxs-lookup"><span data-stu-id="10d2b-202">**UseUrls limitations**</span></span>

<span data-ttu-id="10d2b-203">エンドポイントを構成するには呼び出すことによって、`UseUrls`メソッドまたはを使用して、`urls`コマンドライン引数または ASPNETCORE_URLS 環境変数。</span><span class="sxs-lookup"><span data-stu-id="10d2b-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="10d2b-204">これらのメソッドは Kestrel 以外のサーバーを使用するコードを作成する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="10d2b-205">ただし、これらの制限に注意してください。</span><span class="sxs-lookup"><span data-stu-id="10d2b-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="10d2b-206">これらのメソッドで SSL を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="10d2b-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="10d2b-207">両方を使用する場合、`Listen`メソッドおよび`UseUrls`、`Listen`エンドポイントを上書き、`UseUrls`エンドポイント。</span><span class="sxs-lookup"><span data-stu-id="10d2b-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="10d2b-208">**IIS のエンドポイントの構成**</span><span class="sxs-lookup"><span data-stu-id="10d2b-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="10d2b-209">IIS の URL のバインディングがいずれかを呼び出して設定するすべてのバインドを上書き IIS を使用する場合`Listen`または`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="10d2b-210">詳細については、次を参照してください。 [Introduction to ASP.NET Core モジュール](aspnet-core-module.md)です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10d2b-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10d2b-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="10d2b-212">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="10d2b-213">URL プレフィックスおよび Kestrel を使用してリッスンするポートを構成することができます、`UseUrls`の拡張メソッドで、`urls`コマンドライン引数、または ASP.NET Core の構成システムです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="10d2b-214">これらのメソッドの詳細については、次を参照してください。[ホスティング](../../fundamentals/hosting.md)です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="10d2b-215">リバース プロキシとして IIS を使用するときの URL のバインドの動作方法の詳細については、次を参照してください。 [ASP.NET Core モジュール](aspnet-core-module.md)です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="10d2b-216">URL プレフィックス</span><span class="sxs-lookup"><span data-stu-id="10d2b-216">URL prefixes</span></span>

<span data-ttu-id="10d2b-217">呼び出す場合`UseUrls`か使用して、`urls`コマンドライン引数または ASPNETCORE_URLS 環境変数、URL プレフィックス可能で、次の形式のいずれか。</span><span class="sxs-lookup"><span data-stu-id="10d2b-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10d2b-218">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="10d2b-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="10d2b-219">HTTP URL プレフィックスのみが無効です。Kestrel が SSL をサポートしていないを使用してバインディングを URL を構成するときに`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-219">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="10d2b-220">ポート番号を含む IPv4 アドレス</span><span class="sxs-lookup"><span data-stu-id="10d2b-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="10d2b-221">0.0.0.0 は、すべての IPv4 アドレスにバインドする特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="10d2b-222">ポート番号を含む IPv6 アドレス</span><span class="sxs-lookup"><span data-stu-id="10d2b-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="10d2b-223">[::] は、ipv4、IPv6 該当するショートカットは 0.0.0.0 です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="10d2b-224">ポート番号を持つホスト名</span><span class="sxs-lookup"><span data-stu-id="10d2b-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="10d2b-225">ホスト名、* と +, ではありません。</span><span class="sxs-lookup"><span data-stu-id="10d2b-225">Host names, *, and +, are not special.</span></span> <span data-ttu-id="10d2b-226">認識されている IP アドレスまたは"localhost"ではないものは、すべての IPv4 および IPv6 ip アドレスにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-226">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="10d2b-227">別のホスト名を同じポート上の別の ASP.NET Core アプリケーションにバインドする必要がある場合を使用して[HTTP.sys](httpsys.md)または IIS、Nginx、Apache などのリバース プロキシ サーバー。</span><span class="sxs-lookup"><span data-stu-id="10d2b-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="10d2b-228">ポート番号またはループバック IP アドレスとポート番号を"Localhost"の名前</span><span class="sxs-lookup"><span data-stu-id="10d2b-228">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="10d2b-229">ときに`localhost`Kestrel IPv4 と IPv6 の両方のループバック インターフェイスにバインドしようとするを指定します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-229">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="10d2b-230">いずれのループバック インターフェイス上の別のサービスで使用中では、要求されたポートを開始する Kestrel は失敗します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-230">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="10d2b-231">かどうか、ループバック インターフェイスは使用できません何らかの理由 (ほとんど IPv6 はサポートされていないためによく)、Kestrel 警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-231">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10d2b-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10d2b-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="10d2b-233">ポート番号を含む IPv4 アドレス</span><span class="sxs-lookup"><span data-stu-id="10d2b-233">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="10d2b-234">0.0.0.0 は、すべての IPv4 アドレスにバインドする特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="10d2b-234">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="10d2b-235">ポート番号を含む IPv6 アドレス</span><span class="sxs-lookup"><span data-stu-id="10d2b-235">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="10d2b-236">[::] は、ipv4、IPv6 該当するショートカットは 0.0.0.0 です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-236">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="10d2b-237">ポート番号を持つホスト名</span><span class="sxs-lookup"><span data-stu-id="10d2b-237">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="10d2b-238">ホスト名、 \*、および特殊ないない + です。</span><span class="sxs-lookup"><span data-stu-id="10d2b-238">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="10d2b-239">認識される IP アドレスまたは"localhost"がないものは、すべての IPv4 および IPv6 ip アドレスにバインドします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-239">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="10d2b-240">別のホスト名を同じポート上の別の ASP.NET Core アプリケーションにバインドする必要がある場合を使用して[WebListener](weblistener.md)または IIS、Nginx、Apache などのリバース プロキシ サーバー。</span><span class="sxs-lookup"><span data-stu-id="10d2b-240">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="10d2b-241">ポート番号またはループバック IP アドレスとポート番号を"Localhost"の名前</span><span class="sxs-lookup"><span data-stu-id="10d2b-241">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="10d2b-242">ときに`localhost`Kestrel IPv4 と IPv6 の両方のループバック インターフェイスにバインドしようとするを指定します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-242">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="10d2b-243">いずれのループバック インターフェイス上の別のサービスで使用中では、要求されたポートを開始する Kestrel は失敗します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-243">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="10d2b-244">かどうか、ループバック インターフェイスは使用できません何らかの理由 (ほとんど IPv6 はサポートされていないためによく)、Kestrel 警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-244">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="10d2b-245">Unix ソケット</span><span class="sxs-lookup"><span data-stu-id="10d2b-245">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="10d2b-246">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="10d2b-246">**Port 0**</span></span>

<span data-ttu-id="10d2b-247">ポート番号を 0 を指定すると、Kestrel は動的に使用可能なポートにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="10d2b-247">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="10d2b-248">ホスト名または IP を除く任意のポート 0 へのバインドは許可されて`localhost`名。</span><span class="sxs-lookup"><span data-stu-id="10d2b-248">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="10d2b-249">次の例では、Kestrel が実際に実行時にバインドされるポートを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="10d2b-249">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="10d2b-250">**SSL 用の URL のプレフィックス**</span><span class="sxs-lookup"><span data-stu-id="10d2b-250">**URL prefixes for SSL**</span></span>

<span data-ttu-id="10d2b-251">持つ URL プレフィックスを含めてください`https:`を呼び出す場合は、`UseHttps`拡張メソッドを次のようにします。</span><span class="sxs-lookup"><span data-stu-id="10d2b-251">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="10d2b-252">同じポートでは、HTTPS と HTTP をホストすることはできません。</span><span class="sxs-lookup"><span data-stu-id="10d2b-252">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="10d2b-253">次のステップ</span><span class="sxs-lookup"><span data-stu-id="10d2b-253">Next steps</span></span>

<span data-ttu-id="10d2b-254">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="10d2b-254">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="10d2b-255">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="10d2b-255">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="10d2b-256">2.x 用のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="10d2b-256">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="10d2b-257">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="10d2b-257">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="10d2b-258">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="10d2b-258">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="10d2b-259">1.x 用のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="10d2b-259">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="10d2b-260">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="10d2b-260">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
