---
title: "ASP.NET Core への Kestrel Web サーバーの実装"
author: tdykstra
description: "libuv に基づく ASP.NET Core 用のクロスプラット フォーム Web サーバーである Kestrel を紹介します。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="ef088-103">ASP.NET Core への Kestrel Web サーバーの実装の概要</span><span class="sxs-lookup"><span data-stu-id="ef088-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="ef088-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)、[Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="ef088-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="ef088-105">Kestrel は、[libuv](https://github.com/libuv/libuv) (クロスプラットフォームの非同期 I/O ライブラリ) に基づくクロスプラット フォームの [ASP.NET Core 用 Web サーバー](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="ef088-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="ef088-106">Kestrel は、ASP.NET Core のプロジェクト テンプレートに既定で含まれる Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="ef088-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="ef088-107">Kestrel は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="ef088-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="ef088-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ef088-108">HTTPS</span></span>
  * <span data-ttu-id="ef088-109">[Websocket](https://github.com/aspnet/websockets) を有効にするために使用される非透過的なアップグレード</span><span class="sxs-lookup"><span data-stu-id="ef088-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="ef088-110">Nginx の背後にある高パフォーマンスの UNIX ソケット</span><span class="sxs-lookup"><span data-stu-id="ef088-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="ef088-111">kestrel は、.NET Core がサポートするすべてのプラットフォームおよびバージョンでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="ef088-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef088-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef088-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef088-113">[2.x のサンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ef088-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef088-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef088-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ef088-115">[1.x のサンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ef088-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="ef088-116">Kestrel とリバース プロキシを使用するタイミング</span><span class="sxs-lookup"><span data-stu-id="ef088-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef088-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef088-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef088-118">Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ef088-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="ef088-119">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="ef088-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ef088-122">いずれの構成 &mdash; リバース プロキシ サーバーがある場合とない場合 &mdash; も、Kestrel が内部ネットワークにのみ公開される場合にも使用できます。</span><span class="sxs-lookup"><span data-stu-id="ef088-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef088-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef088-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ef088-124">アプリケーションが内部ネットワークからの要求のみを受け入れる場合は、Kestrel を単独で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="ef088-126">アプリケーションをインターネットに公開する場合は、*リバース プロキシ サーバー*として IIS、Nginx、または Apache を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef088-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="ef088-127">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="ef088-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ef088-129">リバース プロキシは、セキュリティ上の理由からエッジ展開 (インターネットからのトラフィックに公開される) で必要となります。</span><span class="sxs-lookup"><span data-stu-id="ef088-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="ef088-130">1.x バージョンの Kestrel には、攻撃に対する防御の全装備が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="ef088-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="ef088-131">これには、適切なタイムアウト、サイズ制限、および同時接続の制限などが含まれます (ただし、これらに限定されない)。</span><span class="sxs-lookup"><span data-stu-id="ef088-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="ef088-132">リバース プロキシを必要とするシナリオは、単一のサーバー上で実行される同じ IP とポートを共有する複数のアプリケーションがある場合となります。</span><span class="sxs-lookup"><span data-stu-id="ef088-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="ef088-133">このシナリオは Kestrel では直接機能しません。Kestrel では、複数のプロセス間で同じ IP とポートを共有する方法がサポートされていないからです。</span><span class="sxs-lookup"><span data-stu-id="ef088-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="ef088-134">ポートをリッスンするように Kestrel を構成すると、Kestrel はホスト ヘッダーに関係なく、そのポートでのすべてのトラフィックを処理します。</span><span class="sxs-lookup"><span data-stu-id="ef088-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="ef088-135">ポートを共有できるリバース プロキシは、一意の IP とポートで Kestrel に転送される必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef088-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="ef088-136">リバース プロキシ サーバーが必要なくても、次に示すような他の理由により、これを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ef088-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="ef088-137">公開されるセキュリティ、外部からの領域を制限することができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="ef088-138">構成および防御の層を必要に応じて追加します。</span><span class="sxs-lookup"><span data-stu-id="ef088-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="ef088-139">既存のインフラストラクチャとより適切に統合できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ef088-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="ef088-140">負荷分散と SSL のセットアップを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="ef088-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="ef088-141">リバース プロキシ サーバーのみで SSL 証明書が必要です。このサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリケーション サーバーと通信することができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="ef088-142">ASP.NET Core アプリで Kestrel を使用する方法</span><span class="sxs-lookup"><span data-stu-id="ef088-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef088-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef088-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef088-144">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) パッケージは、[Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef088-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="ef088-145">ASP.NET Core プロジェクト テンプレートは既定では Kestrel を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef088-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="ef088-146">*Program.cs* 内のテンプレート コードは `CreateDefaultBuilder` を呼び出し、これによってバックグラウンドで [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef088-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="ef088-147">Kestrel オプションを構成する必要がある場合は、次の例で示すように *Program.cs* で `UseKestrel` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ef088-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef088-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef088-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ef088-149">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="ef088-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="ef088-150">`Main` メソッド内の `WebHostBuilder` で [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 拡張メソッドを呼び出して、必要な [Kestrel オプション](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)を指定します (次のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="ef088-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="ef088-151">Kestrel オプション</span><span class="sxs-lookup"><span data-stu-id="ef088-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef088-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef088-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef088-153">Kestrel Web サーバーには、インターネットに接続する展開で特に有効な制約構成オプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="ef088-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="ef088-154">設定できる制限のいくつかを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ef088-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="ef088-155">クライアントの最大接続数</span><span class="sxs-lookup"><span data-stu-id="ef088-155">Maximum client connections</span></span>
- <span data-ttu-id="ef088-156">要求本文の最大サイズ</span><span class="sxs-lookup"><span data-stu-id="ef088-156">Maximum request body size</span></span>
- <span data-ttu-id="ef088-157">要求本文の最小レート</span><span class="sxs-lookup"><span data-stu-id="ef088-157">Minimum request body data rate</span></span>

<span data-ttu-id="ef088-158">[KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) クラスの `Limits` プロパティで上記の制約およびその他の制約を設定します。</span><span class="sxs-lookup"><span data-stu-id="ef088-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="ef088-159">`Limits` プロパティは、[KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) クラスのインスタンスを保持します。</span><span class="sxs-lookup"><span data-stu-id="ef088-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="ef088-160">**クライアントの最大接続数**</span><span class="sxs-lookup"><span data-stu-id="ef088-160">**Maximum client connections**</span></span>

<span data-ttu-id="ef088-161">次のコードを使用することで、アプリケーション全体に対して同時に開かれる TCP 接続の最大数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="ef088-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="ef088-162">HTTP または HTTPS から別のプロトコルにアップグレードされた接続については別個の制限があります (WebSockets 要求に関する制限など)。</span><span class="sxs-lookup"><span data-stu-id="ef088-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="ef088-163">接続がアップグレードされた後、それは `MaxConcurrentConnections` 制限に対してカウントされません。</span><span class="sxs-lookup"><span data-stu-id="ef088-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="ef088-164">接続の最大数は既定で無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="ef088-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="ef088-165">**要求本文の最大サイズ**</span><span class="sxs-lookup"><span data-stu-id="ef088-165">**Maximum request body size**</span></span>

<span data-ttu-id="ef088-166">既定での要求本文の最大サイズは、30,000,000 バイトです。これはおよそ約 28.6 MB になります。</span><span class="sxs-lookup"><span data-stu-id="ef088-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="ef088-167">ASP.NET Core MVC アプリでの制限を上書きする方法としては、アクション メソッドに対して [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 属性を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ef088-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="ef088-168">次の例では、アプリケーション全体を対象にしてすべての要求に対する制約を構成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ef088-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="ef088-169">ミドルウェア内の特定の要求に関する設定を上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="ef088-170">アプリケーションで要求の読み取りが開始された後、要求に対する制限を構成しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="ef088-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="ef088-171">`MaxRequestBodySize` プロパティが読み取り専用状態にある (制限を構成するには遅すぎる) かどうかを知らせてくれる `IsReadOnly` プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="ef088-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="ef088-172">**要求本文の最小レート**</span><span class="sxs-lookup"><span data-stu-id="ef088-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="ef088-173">kestrel はデータが指定のレート (バイト数/秒) で着信しているかどうかを毎秒チェックします。</span><span class="sxs-lookup"><span data-stu-id="ef088-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="ef088-174">レートが最小値を下回った場合は接続がタイムアウトになります。猶予期間とは、クライアントによって送信速度を最低ラインまで引き上げられるのを、Kestrel が待機する時間のことです。この期間中、レートはチェックされません。</span><span class="sxs-lookup"><span data-stu-id="ef088-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="ef088-175">猶予期間により、TCP のスロースタートのため最初にデータを低速で送信する接続がドロップされるのを回避できます。</span><span class="sxs-lookup"><span data-stu-id="ef088-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="ef088-176">既定の最小レートは 240 バイト/秒であり、5 秒の猶予時間が設定されています。</span><span class="sxs-lookup"><span data-stu-id="ef088-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="ef088-177">最小レートは応答にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="ef088-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="ef088-178">要求制限と応答制限を設定するコードは、プロパティ名およびインターフェイス名に `RequestBody` または `Response` が使用されることを除けば同じです。</span><span class="sxs-lookup"><span data-stu-id="ef088-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="ef088-179">次の例では、*Program.cs* で最小データ レートを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ef088-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="ef088-180">ミドルウェアで要求レートを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="ef088-181">その他の Kestrel オプションについては、次のクラスを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef088-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="ef088-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="ef088-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="ef088-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="ef088-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="ef088-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="ef088-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef088-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef088-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ef088-186">Kestrel オプションについては、「[KestrelServerOptions クラス](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef088-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="ef088-187">エンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="ef088-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef088-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef088-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef088-189">既定では ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ef088-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ef088-190">`KestrelServerOptions` で `Listen` メソッドまたは `ListenUnixSocket` メソッドを呼び出すことにより、Kestrel がリッスンする URL プレフィックスおよびポートを構成します </span><span class="sxs-lookup"><span data-stu-id="ef088-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="ef088-191">(`UseUrls`、`urls` コマンドライン引数、および ASPNETCORE_URLS 環境変数も機能しますが、[この記事で後述](#useurls-limitations)する制限があります)。</span><span class="sxs-lookup"><span data-stu-id="ef088-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="ef088-192">**TCP ソケットにバインドする**</span><span class="sxs-lookup"><span data-stu-id="ef088-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="ef088-193">`Listen` メソッドは TCP ソケットにバインドします。オプションのラムダにより、SSL 証明書を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="ef088-194">この例では、[ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs) を使用して特定のエンドポイントに対して SSL を構成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ef088-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="ef088-195">同じ API を使用して、特定のエンドポイントについて、その他の Kestrel 設定を構成できます。</span><span class="sxs-lookup"><span data-stu-id="ef088-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="ef088-196">**UNIX ソケットにバインドする**</span><span class="sxs-lookup"><span data-stu-id="ef088-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="ef088-197">次の例に示すように、Nginx ではパフォーマンスの向上のために UNIX ソケットをリッスンすることができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="ef088-198">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="ef088-198">**Port 0**</span></span>

<span data-ttu-id="ef088-199">ポート番号 0 を指定すると、Kestrel は使用可能なポートに動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ef088-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="ef088-200">次の例では、Kestrel が実行時に実際にバインドするポートを特定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ef088-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="ef088-201">**UseUrls の制限事項**</span><span class="sxs-lookup"><span data-stu-id="ef088-201">**UseUrls limitations**</span></span>

<span data-ttu-id="ef088-202">エンドポイントを構成するには、`UseUrls` メソッドを呼び出すか、`urls` コマンドライン引数または ASPNETCORE_URLS 環境変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef088-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="ef088-203">コードを Kestrel 以外のサーバーで機能させる場合は、これらのメソッドが有用です。</span><span class="sxs-lookup"><span data-stu-id="ef088-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="ef088-204">ただし、次の制限事項に注意してください。</span><span class="sxs-lookup"><span data-stu-id="ef088-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="ef088-205">これらのメソッドで SSL を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ef088-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="ef088-206">`Listen` メソッドと `UseUrls` を両方とも使用する場合、`Listen` エンドポイントが `UseUrls` エンドポイントを上書きします。</span><span class="sxs-lookup"><span data-stu-id="ef088-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="ef088-207">**IIS 用のエンドポイントの構成**</span><span class="sxs-lookup"><span data-stu-id="ef088-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="ef088-208">IIS を使用する場合、`Listen` または `UseUrls` のいずれかを呼び出して設定したバインディングが、IIS 用の URL バインディングによって上書きされます。</span><span class="sxs-lookup"><span data-stu-id="ef088-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="ef088-209">詳細については、「[Introduction to ASP.NET Core Module](aspnet-core-module.md)」(ASP.NET Core モジュールの概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef088-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef088-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef088-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ef088-211">既定では ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ef088-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ef088-212">`UseUrls` 拡張メソッド、`urls` コマンドライン引数、または ASP.NET Core 構成システムを使用して、Kestrel がリッスンする URL プレフィックスおよびポートを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="ef088-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="ef088-213">これらのメソッドの詳細については、[ホスティング](../../fundamentals/hosting.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef088-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="ef088-214">IIS をリバース プロキシとして使用する場合の URL バインディングの動作方法については、[ASP.NET Core モジュール](aspnet-core-module.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef088-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="ef088-215">URL プレフィックス</span><span class="sxs-lookup"><span data-stu-id="ef088-215">URL prefixes</span></span>

<span data-ttu-id="ef088-216">`UseUrls` を呼び出すか、`urls` コマンドライン引数または ASPNETCORE_URLS 環境変数を使用する場合、URL プレフィックスは次のいずれかの形式となります。</span><span class="sxs-lookup"><span data-stu-id="ef088-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef088-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef088-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef088-218">HTTP URL プレフィックスのみが無効です。`UseUrls` を使用して URL バインディングを構成する場合、Kestrel によって SSL はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="ef088-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="ef088-219">IPv4 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="ef088-220">0.0.0.0 は、すべての IPv4 アドレスにバインドする特別なケースです。</span><span class="sxs-lookup"><span data-stu-id="ef088-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="ef088-221">IPv6 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="ef088-222">IPv6 の [::] は IPv4 の 0.0.0.0 に相当します。</span><span class="sxs-lookup"><span data-stu-id="ef088-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="ef088-223">ホスト名とポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="ef088-224">ホスト名、\*、および + は特別ではありません。</span><span class="sxs-lookup"><span data-stu-id="ef088-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="ef088-225">認識された IP アドレスまたは "localhost" ではないものはいずれも、IPv4 および IPv6 の IP にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ef088-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="ef088-226">異なるホスト名を同じポート上の異なる ASP.NET Core アプリケーションにバインドする必要がある場合は、[HTTP.sys](httpsys.md) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ef088-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="ef088-227">"Localhost" 名とポート番号、またはループバック IP とポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="ef088-228">`localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。</span><span class="sxs-lookup"><span data-stu-id="ef088-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="ef088-229">要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。</span><span class="sxs-lookup"><span data-stu-id="ef088-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="ef088-230">他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="ef088-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef088-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef088-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="ef088-232">IPv4 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="ef088-233">0.0.0.0 は、すべての IPv4 アドレスにバインドする特別なケースです。</span><span class="sxs-lookup"><span data-stu-id="ef088-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="ef088-234">IPv6 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="ef088-235">IPv6 の [::] は IPv4 の 0.0.0.0 に相当します。</span><span class="sxs-lookup"><span data-stu-id="ef088-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="ef088-236">ホスト名とポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="ef088-237">ホスト名、\*、および + は特別ではありません。</span><span class="sxs-lookup"><span data-stu-id="ef088-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="ef088-238">認識された IP アドレスまたは "localhost" ではないものはいずれも、IPv4 および IPv6 の IP にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ef088-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="ef088-239">異なるホスト名を同じポート上の異なる ASP.NET Core アプリケーションにバインドする必要がある場合は、[WebListener](weblistener.md) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ef088-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="ef088-240">"Localhost" 名とポート番号、またはループバック IP とポート番号</span><span class="sxs-lookup"><span data-stu-id="ef088-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="ef088-241">`localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。</span><span class="sxs-lookup"><span data-stu-id="ef088-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="ef088-242">要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。</span><span class="sxs-lookup"><span data-stu-id="ef088-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="ef088-243">他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="ef088-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="ef088-244">UNIX ソケット</span><span class="sxs-lookup"><span data-stu-id="ef088-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="ef088-245">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="ef088-245">**Port 0**</span></span>

<span data-ttu-id="ef088-246">ポート番号 0 を指定すると、Kestrel は使用可能なポートに動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ef088-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="ef088-247">ポート 0 へのバインドは、`localhost` 名を除き、任意のホスト名または ID で許可されます。</span><span class="sxs-lookup"><span data-stu-id="ef088-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="ef088-248">次の例では、Kestrel が実行時に実際にバインドするポートを特定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ef088-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="ef088-249">**SSL 用の URL プレフィックス**</span><span class="sxs-lookup"><span data-stu-id="ef088-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="ef088-250">`UseHttps` 拡張メソッドを呼び出す場合は、次に示すように URL プレフィックスと `https:` を必ず含めます。</span><span class="sxs-lookup"><span data-stu-id="ef088-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="ef088-251">同じポート上で、HTTPS と HTTP をホストすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ef088-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="ef088-252">次の手順</span><span class="sxs-lookup"><span data-stu-id="ef088-252">Next steps</span></span>

<span data-ttu-id="ef088-253">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef088-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef088-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef088-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="ef088-255">2.x のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="ef088-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="ef088-256">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="ef088-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef088-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef088-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="ef088-258">1.x のサンプリ アプリ</span><span class="sxs-lookup"><span data-stu-id="ef088-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="ef088-259">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="ef088-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
