---
title: ASP.NET Core への Kestrel Web サーバーの実装
author: rick-anderson
description: ASP.NET Core 用のクロスプラットフォーム Web サーバーである Kestrel について説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: d6157ac2bdf046c66f4b740ad2263f6b7485c05d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912307"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="ce51c-103">ASP.NET Core への Kestrel Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="ce51c-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="ce51c-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)、[Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="ce51c-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="ce51c-105">Kestrel は、[ASP.NET Core 向けのクロスプラットフォーム Web サーバー](xref:fundamentals/servers/index)です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="ce51c-106">Kestrel は、ASP.NET Core のプロジェクト テンプレートに既定で含まれる Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="ce51c-107">Kestrel は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-107">Kestrel supports the following features:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ce51c-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ce51c-108">HTTPS</span></span>
* <span data-ttu-id="ce51c-109">[Websocket](https://github.com/aspnet/websockets) を有効にするために使用される非透過的なアップグレード</span><span class="sxs-lookup"><span data-stu-id="ce51c-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="ce51c-110">Nginx の背後にある高パフォーマンスの UNIX ソケット</span><span class="sxs-lookup"><span data-stu-id="ce51c-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="ce51c-111">HTTP/2 (macOS の場合を除く&dagger;)</span><span class="sxs-lookup"><span data-stu-id="ce51c-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="ce51c-112">&dagger;将来のリリースでは HTTP/2 が macOS 上でサポートされるようになります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ce51c-113">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ce51c-113">HTTPS</span></span>
* <span data-ttu-id="ce51c-114">[Websocket](https://github.com/aspnet/websockets) を有効にするために使用される非透過的なアップグレード</span><span class="sxs-lookup"><span data-stu-id="ce51c-114">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="ce51c-115">Nginx の背後にある高パフォーマンスの UNIX ソケット</span><span class="sxs-lookup"><span data-stu-id="ce51c-115">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="ce51c-116">kestrel は、.NET Core がサポートするすべてのプラットフォームおよびバージョンでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-116">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="ce51c-117">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ce51c-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="ce51c-118">HTTP/2 のサポート</span><span class="sxs-lookup"><span data-stu-id="ce51c-118">HTTP/2 support</span></span>

<span data-ttu-id="ce51c-119">[Http/2](https://httpwg.org/specs/rfc7540.html) は、次の基本要件が満たされている場合に、ASP.NET Core アプリで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-119">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="ce51c-120">オペレーティング システム&dagger;</span><span class="sxs-lookup"><span data-stu-id="ce51c-120">Operating system&dagger;</span></span>
  * <span data-ttu-id="ce51c-121">Windows Server 2012 R2/Windows 8.1 以降</span><span class="sxs-lookup"><span data-stu-id="ce51c-121">Windows Server 2012 R2/Windows 8.1 or later</span></span>
  * <span data-ttu-id="ce51c-122">OpenSSL 1.0.2 以降を使用した Linux (Ubuntu 16.04 以降など)</span><span class="sxs-lookup"><span data-stu-id="ce51c-122">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="ce51c-123">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="ce51c-123">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="ce51c-124">[アプリケーション レイヤー プロトコル ネゴシエーション (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 接続</span><span class="sxs-lookup"><span data-stu-id="ce51c-124">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="ce51c-125">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="ce51c-125">TLS 1.2 or later connection</span></span>

<span data-ttu-id="ce51c-126">&dagger;将来のリリースでは HTTP/2 が macOS 上でサポートされるようになります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-126">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="ce51c-127">Http/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) が `HTTP/2` を報告します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="ce51c-128">Http/2 は既定では無効になっています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="ce51c-129">構成の詳細については、「[Kestrel オプション](#kestrel-options)」セクションと「[エンドポイントの構成](#endpoint-configuration)」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [Endpoint configuration](#endpoint-configuration) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="ce51c-130">Kestrel とリバース プロキシを使用するタイミング</span><span class="sxs-lookup"><span data-stu-id="ce51c-130">When to use Kestrel with a reverse proxy</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-131">Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-131">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="ce51c-132">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ce51c-135">リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ce51c-136">アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel をアプリのサーバーとして直接使うことができます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-136">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="ce51c-138">アプリをインターネットに公開する場合は、"*リバース プロキシ サーバー*" として IIS、Nginx、または Apache を使います。</span><span class="sxs-lookup"><span data-stu-id="ce51c-138">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="ce51c-139">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-139">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ce51c-141">リバース プロキシは、セキュリティ上の理由からエッジ展開 (インターネットからのトラフィックに公開される) で必要となります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-141">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="ce51c-142">1.x バージョンの Kestrel は、適切なタイムアウト、サイズの制限、コンカレント接続の制限など、攻撃に対する防御の機能が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-142">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="ce51c-143">リバース プロキシのシナリオは、単一のサーバー上で実行される同じ IP とポートを共有する複数のアプリがある場合に存在します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-143">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="ce51c-144">Kestrel は、複数のプロセスによる同じ IP とポートの共有をサポートしていないため、このシナリオには対応しません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-144">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="ce51c-145">ポートでリッスンするように Kestrel を構成すると、Kestrel は、要求のホスト ヘッダーに関係なく、そのポートに対するすべてのトラフィックを処理します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-145">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="ce51c-146">ポートを共有できるリバース プロキシは、一意の IP とポート上の Kestrel に要求を転送することができます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-146">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="ce51c-147">リバース プロキシ サーバーが必要ない場合であっても、リバース プロキシ サーバーを使用すると次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-147">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="ce51c-148">それがホストするアプリの公開されるパブリック サーフェス領域を制限することができます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-148">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="ce51c-149">構成および防御の層を追加します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-149">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="ce51c-150">既存のインフラストラクチャとより適切に統合できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-150">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="ce51c-151">負荷分散と SSL の構成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-151">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="ce51c-152">リバース プロキシ サーバーのみで SSL 証明書が必要です。このサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリ サーバーと通信することができます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-152">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="ce51c-153">ホスト フィルタリングがリバース プロキシを使っていない場合は、[ホスト フィルタリング](#host-filtering)を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-153">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="ce51c-154">ASP.NET Core アプリで Kestrel を使用する方法</span><span class="sxs-lookup"><span data-stu-id="ce51c-154">How to use Kestrel in ASP.NET Core apps</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-155">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) パッケージは、[[Microsoft.AspNetCore.App metapackage]](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) に含まれています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-155">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="ce51c-156">ASP.NET Core プロジェクト テンプレートは既定では Kestrel を使用します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-156">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="ce51c-157">*Program.cs* 内のテンプレート コードは [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出し、これによってバックグラウンドで [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-157">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ce51c-158">`CreateDefaultBuilder` を呼び出した後に追加の構成を指定するには、`ConfigureKestrel` を使用します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-158">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

<span data-ttu-id="ce51c-159">`CreateDefaultBuilder` を呼び出した後に追加の構成を指定するには、[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-159">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        })
        .Build();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ce51c-160">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-160">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="ce51c-161">`Main` メソッド内の [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) で [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) 拡張メソッドを呼び出して、必要な [Kestrel オプション](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)を指定します (次のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="ce51c-161">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="ce51c-162">Kestrel オプション</span><span class="sxs-lookup"><span data-stu-id="ce51c-162">Kestrel options</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-163">Kestrel Web サーバーには、インターネットに接続する展開で特に有効な制約構成オプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-163">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="ce51c-164">カスタマイズできる重要な制限は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-164">A few important limits that can be customized:</span></span>

* <span data-ttu-id="ce51c-165">クライアントの最大接続数</span><span class="sxs-lookup"><span data-stu-id="ce51c-165">Maximum client connections</span></span>
* <span data-ttu-id="ce51c-166">要求本文の最大サイズ</span><span class="sxs-lookup"><span data-stu-id="ce51c-166">Maximum request body size</span></span>
* <span data-ttu-id="ce51c-167">要求本文の最小レート</span><span class="sxs-lookup"><span data-stu-id="ce51c-167">Minimum request body data rate</span></span>

<span data-ttu-id="ce51c-168">[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) クラスの [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) プロパティで、上記の制約およびその他の制約を設定します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-168">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="ce51c-169">`Limits` プロパティは、[KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) クラスのインスタンスを保持します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-169">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="ce51c-170">クライアントの最大接続数</span><span class="sxs-lookup"><span data-stu-id="ce51c-170">Maximum client connections</span></span>

[<span data-ttu-id="ce51c-171">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="ce51c-171">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="ce51c-172">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="ce51c-172">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

<span data-ttu-id="ce51c-173">次のコードを使用することで、アプリ全体に対して同時に開かれる TCP 接続の最大数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-173">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="ce51c-174">HTTP または HTTPS から別のプロトコルにアップグレードされた接続については別個の制限があります (WebSockets 要求に関する制限など)。</span><span class="sxs-lookup"><span data-stu-id="ce51c-174">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="ce51c-175">接続がアップグレードされた後、それは `MaxConcurrentConnections` 制限に対してカウントされません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-175">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-176">接続の最大数は既定では無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-176">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="ce51c-177">要求本文の最大サイズ</span><span class="sxs-lookup"><span data-stu-id="ce51c-177">Maximum request body size</span></span>

[<span data-ttu-id="ce51c-178">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="ce51c-178">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="ce51c-179">既定の要求本文の最大サイズは、30,000,000 バイトです。これは約 28.6 MB になります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="ce51c-180">ASP.NET Core MVC アプリでの制限をオーバーライドする方法としては、次のように、アクション メソッドに対して [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 属性を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

<span data-ttu-id="ce51c-181">次の例では、すべての要求についての制約をアプリに対して構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="ce51c-182">ミドルウェア内の特定の要求に関する設定をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-182">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-183">アプリが要求の読み取りを開始した後で、要求に対する制限を構成しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-183">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="ce51c-184">`MaxRequestBodySize` プロパティが読み取り専用状態にある (制限を構成するには遅すぎる) かどうかを示す `IsReadOnly` プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="ce51c-185">要求本文の最小レート</span><span class="sxs-lookup"><span data-stu-id="ce51c-185">Minimum request body data rate</span></span>

[<span data-ttu-id="ce51c-186">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="ce51c-186">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="ce51c-187">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="ce51c-187">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="ce51c-188">kestrel はデータが指定のレート (バイト数/秒) で到着しているかどうかを毎秒チェックします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-188">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="ce51c-189">レートが最小値を下回った場合は接続がタイムアウトになります。猶予期間とは、クライアントによって送信速度を最低ラインまで引き上げられるのを、Kestrel が待機する時間のことです。この期間中、レートはチェックされません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-189">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="ce51c-190">猶予期間により、TCP のスロースタートのため最初にデータを低速で送信する接続がドロップされるのを回避できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-190">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="ce51c-191">既定の最小レートは 240 バイト/秒であり、5 秒の猶予時間が設定されています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-191">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="ce51c-192">最小レートは応答にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-192">A minimum rate also applies to the response.</span></span> <span data-ttu-id="ce51c-193">要求制限と応答制限を設定するコードは、プロパティ名およびインターフェイス名に `RequestBody` または `Response` が使用されることを除けば同じです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-193">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="ce51c-194">次の例では、*Program.cs* で最小データ レートを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-194">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

<span data-ttu-id="ce51c-195">ミドルウェアで要求レートを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-195">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="ce51c-196">接続ごとの最大ストリーム</span><span class="sxs-lookup"><span data-stu-id="ce51c-196">Maximum streams per connection</span></span>

<span data-ttu-id="ce51c-197">HTTP/2 接続ごとの同時要求ストリームの数は、`Http2.MaxStreamsPerConnection` によって制限されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-197">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="ce51c-198">余分なストリームは拒否されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-198">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="ce51c-199">既定値は 100 です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-199">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="ce51c-200">ヘッダー テーブルのサイズ</span><span class="sxs-lookup"><span data-stu-id="ce51c-200">Header table size</span></span>

<span data-ttu-id="ce51c-201">HTTP/2 接続では、HTTP ヘッダーは HPACK デコーダーによって圧縮解除されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-201">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="ce51c-202">HPACK デコーダーが使用するヘッダー圧縮テーブルのサイズは `Http2.HeaderTableSize` によって制限されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-202">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="ce51c-203">値はオクテット単位で指定し、ゼロ (0) より大きくなければなりません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-203">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="ce51c-204">既定値は 4096 です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-204">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="ce51c-205">最大フレーム サイズ</span><span class="sxs-lookup"><span data-stu-id="ce51c-205">Maximum frame size</span></span>

<span data-ttu-id="ce51c-206">受信する HTTP/2 接続フレーム ペイロードの最大サイズは、`Http2.MaxFrameSize` によって示されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-206">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="ce51c-207">値はオクテット単位で指定し、2^14 (16,384) から 2^24-1 (16,777,215) までの範囲とする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-207">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="ce51c-208">既定値は 2^14 (16,384) です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-208">The default value is 2^14 (16,384).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-209">Kestrel のその他のオプションと制限については、以下をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-209">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="ce51c-210">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="ce51c-210">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="ce51c-211">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="ce51c-211">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="ce51c-212">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="ce51c-212">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ce51c-213">Kestrel のオプションと制限については、以下をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-213">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="ce51c-214">KestrelServerOptions クラス</span><span class="sxs-lookup"><span data-stu-id="ce51c-214">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="ce51c-215">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="ce51c-215">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

## <a name="endpoint-configuration"></a><span data-ttu-id="ce51c-216">エンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="ce51c-216">Endpoint configuration</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ce51c-217">既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-217">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ce51c-218">Kestrel の URL プレフィックスとポートを構成するには、[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) で [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) または [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-218">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="ce51c-219">`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、`ASPNETCORE_URLS` 環境変数も機能しますが、このセクションで後述する制限があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-219">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="ce51c-220">`urls` ホスト構成キーは、アプリ構成ではなくホスト構成から取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-220">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="ce51c-221">ホストは構成が構成ファイルから読み取られるまでに完全に初期化されるため、`urls` キーと値を *appsettings.json* に追加しても、ホストの構成には影響しません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-221">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="ce51c-222">ただし、*appsettings.json* の `urls` キーをホスト ビルダー [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) と共に使用して、ホストを構成できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-222">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ce51c-223">既定では、ASP.NET Core は以下にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-223">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="ce51c-224">`https://localhost:5001` (ローカル開発証明書が存在する場合)</span><span class="sxs-lookup"><span data-stu-id="ce51c-224">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="ce51c-225">開発証明書が作成されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-225">A development certificate is created:</span></span>

* <span data-ttu-id="ce51c-226">[.NET Core SDK](/dotnet/core/sdk) がインストールされるとき。</span><span class="sxs-lookup"><span data-stu-id="ce51c-226">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="ce51c-227">証明書を作成するには [dev-certs ツール](xref:aspnetcore-2.1#https)を使います。</span><span class="sxs-lookup"><span data-stu-id="ce51c-227">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="ce51c-228">一部のブラウザーでは、ローカル開発証明書を信頼するには、ブラウザーに明示的にアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-228">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="ce51c-229">ASP.NET Core 2.1 およびそれより後のプロジェクト テンプレートでは、アプリは HTTPS で実行するように既定で構成され、[HTTPS のリダイレクトと HSTS のサポート](xref:security/enforcing-ssl)が組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-229">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="ce51c-230">Kestrel の URL プレフィックスとポートを構成するには、[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) で [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) または [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-230">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="ce51c-231">`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、`ASPNETCORE_URLS` 環境変数も機能しますが、このセクションで後述する制限があります (既定の証明書が、HTTPS エンドポイントの構成に使用できる必要があります)。</span><span class="sxs-lookup"><span data-stu-id="ce51c-231">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="ce51c-232">ASP.NET Core 2.1 の `KestrelServerOptions` の構成:</span><span class="sxs-lookup"><span data-stu-id="ce51c-232">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="ce51c-233">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="ce51c-233">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="ce51c-234">指定した各エンドポイントに対して実行するように、構成の `Action` を指定します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-234">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="ce51c-235">`ConfigureEndpointDefaults` を複数回呼び出すと、前の `Action` が最後に指定した `Action` で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-235">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="ce51c-236">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="ce51c-236">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="ce51c-237">各 HTTPS エンドポイントに対して実行するように、構成の `Action` を指定します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-237">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="ce51c-238">`ConfigureHttpsDefaults` を複数回呼び出すと、前の `Action` が最後に指定した `Action` で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-238">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="ce51c-239">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="ce51c-239">Configure(IConfiguration)</span></span>

<span data-ttu-id="ce51c-240">入力として [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を受け取る Kestrel を設定するための構成ローダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-240">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="ce51c-241">構成のスコープは、Kestrel の構成セクションにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-241">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="ce51c-242">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="ce51c-242">ListenOptions.UseHttps</span></span>

<span data-ttu-id="ce51c-243">HTTPS を使用するように Kestrel を構成します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-243">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="ce51c-244">`ListenOptions.UseHttps` 拡張機能:</span><span class="sxs-lookup"><span data-stu-id="ce51c-244">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="ce51c-245">`UseHttps` &ndash; 既定の証明書で HTTPS を使うように Kestrel を構成します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-245">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="ce51c-246">既定の証明書が構成されていない場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-246">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="ce51c-247">`ListenOptions.UseHttps` パラメーター:</span><span class="sxs-lookup"><span data-stu-id="ce51c-247">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="ce51c-248">`filename` は、アプリのコンテンツ ファイルが格納されているディレクトリを基準とする、証明書ファイルのパスとファイル名です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-248">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="ce51c-249">`password` は、X.509 証明書データにアクセスするために必要なパスワードです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-249">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="ce51c-250">`configureOptions` は、`HttpsConnectionAdapterOptions` を構成するための `Action` です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-250">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="ce51c-251">`ListenOptions` を返します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-251">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="ce51c-252">`storeName` は、証明書の読み込み元の証明書ストアです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-252">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="ce51c-253">`subject` は、証明書のサブジェクト名です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-253">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="ce51c-254">`allowInvalid` は、無効な証明書 (自己署名証明書など) を考慮する必要があるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-254">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="ce51c-255">`location` は、証明書を読み込むストアの場所です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-255">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="ce51c-256">`serverCertificate` は、X.509 証明書です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-256">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="ce51c-257">運用環境では、HTTPS を明示的に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-257">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="ce51c-258">少なくとも、既定の証明書を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-258">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="ce51c-259">サポートされている構成を次に説明します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-259">Supported configurations described next:</span></span>

* <span data-ttu-id="ce51c-260">構成なし</span><span class="sxs-lookup"><span data-stu-id="ce51c-260">No configuration</span></span>
* <span data-ttu-id="ce51c-261">構成から既定の証明書を置き換える</span><span class="sxs-lookup"><span data-stu-id="ce51c-261">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="ce51c-262">コードで既定値を変更する</span><span class="sxs-lookup"><span data-stu-id="ce51c-262">Change the defaults in code</span></span>

<span data-ttu-id="ce51c-263">*構成なし*</span><span class="sxs-lookup"><span data-stu-id="ce51c-263">*No configuration*</span></span>

<span data-ttu-id="ce51c-264">Kestrel は、`http://localhost:5000` と `https://localhost:5001` (既定の証明書が使用可能な場合) でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-264">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="ce51c-265">以下を使用して URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-265">Specify URLs using the:</span></span>

* <span data-ttu-id="ce51c-266">`ASPNETCORE_URLS` 環境変数。</span><span class="sxs-lookup"><span data-stu-id="ce51c-266">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="ce51c-267">`--urls` コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="ce51c-267">`--urls` command-line argument.</span></span>
* <span data-ttu-id="ce51c-268">`urls` ホスト構成キー。</span><span class="sxs-lookup"><span data-stu-id="ce51c-268">`urls` host configuration key.</span></span>
* <span data-ttu-id="ce51c-269">`UseUrls` 拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="ce51c-269">`UseUrls` extension method.</span></span>

<span data-ttu-id="ce51c-270">詳しくは、「[サーバーの URL](xref:fundamentals/host/web-host#server-urls)」および「[構成のオーバーライド](xref:fundamentals/host/web-host#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-270">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="ce51c-271">これらの方法を使うと、1 つまたは複数の HTTP エンドポイントおよび HTTPS エンドポイント (既定の証明書が使用可能な場合は HTTPS) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-271">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="ce51c-272">セミコロン区切りのリストとして値を構成します (例: `"Urls": "http://localhost:8000; http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="ce51c-272">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="ce51c-273">*構成から既定の証明書を置き換える*</span><span class="sxs-lookup"><span data-stu-id="ce51c-273">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="ce51c-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は既定で `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` を呼び出して Kestrel の構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="ce51c-275">Kestrel は、既定の HTTPS アプリ設定構成スキーマを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-275">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="ce51c-276">ディスク上のファイルまたは証明書ストアから、URL や使用する証明書など、複数のエンドポイントを構成します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-276">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="ce51c-277">以下の *appsettings.json* の例では、次のことが行われています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-277">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="ce51c-278">**AllowInvalid** を `true` に設定し、の無効な証明書 (自己署名証明書など) の使用を許可します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-278">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="ce51c-279">証明書 (後の例では **HttpsDefaultCert**) が指定されていないすべての HTTPS エンドポイントは、**[証明書]** > **[既定]** または開発証明書で定義されている証明書にフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-279">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="ce51c-280">証明書ノードの **Path** と **Password** を使用する代わりの方法は、証明書ストアのフィールドを使って証明書を指定することです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-280">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="ce51c-281">たとえば、**[証明書]** > **[既定]** の証明書は次のように指定できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-281">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="ce51c-282">スキーマに関する注意事項:</span><span class="sxs-lookup"><span data-stu-id="ce51c-282">Schema notes:</span></span>

* <span data-ttu-id="ce51c-283">エンドポイント名は大文字と小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-283">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="ce51c-284">たとえば、`HTTPS` と `Https` は有効です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-284">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="ce51c-285">`Url` パラメーターは、エンドポイントごとに必要です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-285">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="ce51c-286">このパラメーターの形式は、1 つの値に制限されることを除き、最上位レベルの `Urls` 構成パラメーターと同じです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-286">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="ce51c-287">これらのエンドポイントは、最上位レベルの `Urls` 構成での定義に追加されるのではなく、それを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-287">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="ce51c-288">コードで `Listen` を使用して定義されているエンドポイントは、構成セクションで定義されているエンドポイントに累積されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-288">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="ce51c-289">`Certificate` セクションは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-289">The `Certificate` section is optional.</span></span> <span data-ttu-id="ce51c-290">`Certificate` セクションを指定しないと、前述のシナリオで定義した既定値が使用されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-290">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="ce51c-291">既定値を使用できない場合、サーバーにより例外がスローされ、開始できません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-291">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="ce51c-292">`Certificate` セクションは、**Path**&ndash;**Password** 証明書と **Subject**&ndash;**Store** 証明書の両方をサポートします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-292">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="ce51c-293">ポートが競合しない限り、この方法で任意の数のエンドポイントを定義できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-293">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="ce51c-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` が `.Endpoint(string name, options => { })` メソッドで返す `KestrelConfigurationLoader` を使用して、構成されているエンドポイントの設定を補足できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="ce51c-295">`KestrelServerOptions.ConfigurationLoader` に直接アクセスして、既存のローダー ([WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) によって提供されるものなど) の反復を維持することもできます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-295">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="ce51c-296">カスタム設定を読み取ることができるように、各エンドポイントの構成セクションを `Endpoint` メソッドのオプションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-296">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="ce51c-297">別のセクションで `options.Configure(context.Configuration.GetSection("Kestrel"))` を再び呼び出すことにより、複数の構成を読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-297">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="ce51c-298">前のインスタンスで `Load` を明示的に呼び出していない限り、最後の構成のみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-298">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="ce51c-299">既定の構成セクションを置き換えることができるように、メタパッケージは `Load` を呼び出しません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-299">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="ce51c-300">`KestrelConfigurationLoader` はミラー、`KestrelServerOptions` から API の `Listen` ファミリを `Endpoint` オーバーロードとしてミラーリングするので、コードと構成のエンドポイントを同じ場所で構成できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-300">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="ce51c-301">これらのオーバーロードでは名前は使用されず、構成からの既定の設定のみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-301">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="ce51c-302">*コードで既定値を変更する*</span><span class="sxs-lookup"><span data-stu-id="ce51c-302">*Change the defaults in code*</span></span>

<span data-ttu-id="ce51c-303">`ConfigureEndpointDefaults` および`ConfigureHttpsDefaults` を使用して、`ListenOptions` および `HttpsConnectionAdapterOptions` の既定の設定を変更できます (前のシナリオで指定した既定の証明書のオーバーライドなど)。</span><span class="sxs-lookup"><span data-stu-id="ce51c-303">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="ce51c-304">エンドポイントを構成する前に、`ConfigureEndpointDefaults` および `ConfigureHttpsDefaults` を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-304">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="ce51c-305">*Kestrel による SNI のサポート*</span><span class="sxs-lookup"><span data-stu-id="ce51c-305">*Kestrel support for SNI*</span></span>

<span data-ttu-id="ce51c-306">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) を使用すると、同じ IP アドレスとポートで複数のドメインをホストできます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-306">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="ce51c-307">SNI が機能するためには、サーバーが正しい証明書を提供できるように、クライアントは TLS ハンドシェイクの間にセキュリティで保護されたセッションのホスト名をサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-307">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="ce51c-308">クライアントは、TLS ハンドシェイクに続くセキュリティで保護されたセッション中に、提供された証明書をサーバーとの暗号化された通信に使用します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-308">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="ce51c-309">Kestrel は、`ServerCertificateSelector` コールバックによって SNI をサポートします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-309">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="ce51c-310">コールバックは接続ごとに 1 回呼び出されるので、アプリはそれを使って、ホスト名を検査し、適切な証明書を選択できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-310">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="ce51c-311">SNI のサポートには、次が必要です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-311">SNI support requires:</span></span>

* <span data-ttu-id="ce51c-312">ターゲット フレームワーク `netcoreapp2.1` で実行される必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-312">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="ce51c-313">`netcoreapp2.0` および `net461` の場合は、コールバックは呼び出されますが、`name` は常に `null` です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-313">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="ce51c-314">クライアントが TLS ハンドシェイクでホスト名パラメーターを提供しない場合も、`name` は `null` です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-314">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="ce51c-315">すべての Web サイトは、同じ Kestrel インスタンスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-315">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="ce51c-316">Kestrel では、リバース プロキシは使用しない、複数のインスタンスでの IP アドレスとポートの共有はサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-316">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="ce51c-317">TCP ソケットにバインドする</span><span class="sxs-lookup"><span data-stu-id="ce51c-317">Bind to a TCP socket</span></span>

<span data-ttu-id="ce51c-318">[Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) メソッドは TCP ソケットにバインドし、オプションのラムダにより SSL 証明書を構成できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-318">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-319">例では、[ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) を使用して、エンドポイントに対する SSL が構成されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-319">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="ce51c-320">同じ API を使用して、特定のエンドポイントに対する他の Kestrel 設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-320">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="ce51c-321">UNIX ソケットにバインドする</span><span class="sxs-lookup"><span data-stu-id="ce51c-321">Bind to a Unix socket</span></span>

<span data-ttu-id="ce51c-322">次の例に示すように、Nginx でのパフォーマンス向上のため、[ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) で UNIX ソケットをリッスンします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-322">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="port-0"></a><span data-ttu-id="ce51c-323">ポート 0</span><span class="sxs-lookup"><span data-stu-id="ce51c-323">Port 0</span></span>

<span data-ttu-id="ce51c-324">ポート番号 `0` を指定すると、Kestrel は使用可能なポートに動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-324">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="ce51c-325">次の例では、Kestrel が実行時に実際にバインドするポートを特定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-325">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="ce51c-326">アプリを実行すると、コンソール ウィンドウの出力で、アプリがアクセスできる動的なポートが示されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-326">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="ce51c-327">制限事項</span><span class="sxs-lookup"><span data-stu-id="ce51c-327">Limitations</span></span>

<span data-ttu-id="ce51c-328">次の方法でエンドポイントを構成します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-328">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="ce51c-329">UseUrls</span><span class="sxs-lookup"><span data-stu-id="ce51c-329">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="ce51c-330">`--urls` コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="ce51c-330">`--urls` command-line argument</span></span>
* <span data-ttu-id="ce51c-331">`urls` ホスト構成キー</span><span class="sxs-lookup"><span data-stu-id="ce51c-331">`urls` host configuration key</span></span>
* <span data-ttu-id="ce51c-332">`ASPNETCORE_URLS` 環境変数</span><span class="sxs-lookup"><span data-stu-id="ce51c-332">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="ce51c-333">コードを Kestrel 以外のサーバーで機能させるには、これらのメソッドが有用です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-333">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="ce51c-334">ただし、次の使用制限があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-334">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="ce51c-335">HTTPS エンドポイントの構成で (たとえば、前に説明したように `KestrelServerOptions` 構成または構成ファイルを使用して) 既定の証明書が提供されていない場合、これらの方法で SSL を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-335">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="ce51c-336">`Listen` と `UseUrls` 両方の方法を同時に使用すると、`Listen` エンドポイントが `UseUrls` エンドポイントをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-336">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="ce51c-337">IIS エンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="ce51c-337">IIS endpoint configuration</span></span>

<span data-ttu-id="ce51c-338">IIS を使用するときは、IIS オーバーライド バインドに対する URL バインドを、`Listen` または `UseUrls` によって設定します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-338">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="ce51c-339">詳しくは、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-339">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ce51c-340">既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-340">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="ce51c-341">以下を使用して Kestrel の URL プレフィックスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-341">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="ce51c-342">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) 拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="ce51c-342">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="ce51c-343">`--urls` コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="ce51c-343">`--urls` command-line argument</span></span>
* <span data-ttu-id="ce51c-344">`urls` ホスト構成キー</span><span class="sxs-lookup"><span data-stu-id="ce51c-344">`urls` host configuration key</span></span>
* <span data-ttu-id="ce51c-345">ASP.NET Core 構成システム (`ASPNETCORE_URLS` 環境変数など)</span><span class="sxs-lookup"><span data-stu-id="ce51c-345">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="ce51c-346">これらのメソッドについて詳しくは、[ホスティング](xref:fundamentals/host/index)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-346">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="ce51c-347">IIS エンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="ce51c-347">IIS endpoint configuration</span></span>

<span data-ttu-id="ce51c-348">IIS を使用するときは、IIS オーバーライド バインドに対する URL バインドを、`UseUrls` によって設定します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-348">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="ce51c-349">詳しくは、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-349">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="ce51c-350">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="ce51c-350">ListenOptions.Protocols</span></span>

<span data-ttu-id="ce51c-351">`Protocols`プロパティでは、接続エンドポイントまたはサーバーに対して有効にされる HTTP プロトコル (`HttpProtocols`) が確定されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-351">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="ce51c-352">`HttpProtocols` 列挙型から `Protocols` プロパティに値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-352">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="ce51c-353">`HttpProtocols` 列挙値</span><span class="sxs-lookup"><span data-stu-id="ce51c-353">`HttpProtocols` enum value</span></span> | <span data-ttu-id="ce51c-354">許可される接続プロトコル</span><span class="sxs-lookup"><span data-stu-id="ce51c-354">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="ce51c-355">HTTP/1.1 のみ。</span><span class="sxs-lookup"><span data-stu-id="ce51c-355">HTTP/1.1 only.</span></span> <span data-ttu-id="ce51c-356">TLS の有無にかかわらず使用できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-356">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="ce51c-357">HTTP/2 のみ。</span><span class="sxs-lookup"><span data-stu-id="ce51c-357">HTTP/2 only.</span></span> <span data-ttu-id="ce51c-358">主に TLS と一緒に使用されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-358">Primarily used with TLS.</span></span> <span data-ttu-id="ce51c-359">[予備知識モード](https://tools.ietf.org/html/rfc7540#section-3.4)がクライアントでサポートされている場合に限り、TLS なしで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-359">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="ce51c-360">HTTP/1.1 および HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="ce51c-360">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="ce51c-361">HTTP/2 をネゴシエートするには TLS と[アプリケーション レイヤー プロトコル ネゴシエーション (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) が必要です。それ以外の場合、接続は既定では HTTP/1.1 となります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-361">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="ce51c-362">既定のプロトコルは HTTP/1.1 です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-362">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="ce51c-363">HTTP/2 に対する TLS 制限事項:</span><span class="sxs-lookup"><span data-stu-id="ce51c-363">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="ce51c-364">TLS バージョン 1.2 以降</span><span class="sxs-lookup"><span data-stu-id="ce51c-364">TLS version 1.2 or later</span></span>
* <span data-ttu-id="ce51c-365">再ネゴシエーションは無効</span><span class="sxs-lookup"><span data-stu-id="ce51c-365">Renegotiation disabled</span></span>
* <span data-ttu-id="ce51c-366">圧縮は無効</span><span class="sxs-lookup"><span data-stu-id="ce51c-366">Compression disabled</span></span>
* <span data-ttu-id="ce51c-367">短期キー交換サイズの上限:</span><span class="sxs-lookup"><span data-stu-id="ce51c-367">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="ce51c-368">Elliptic curve Diffie-hellman (ECDHE) &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小で 224 ビット</span><span class="sxs-lookup"><span data-stu-id="ce51c-368">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="ce51c-369">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小で 2048 ビット</span><span class="sxs-lookup"><span data-stu-id="ce51c-369">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="ce51c-370">暗号スイートはブラック リストに登録されない</span><span class="sxs-lookup"><span data-stu-id="ce51c-370">Cipher suite not blacklisted</span></span>

<span data-ttu-id="ce51c-371">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; と P-256 elliptic curve &lbrack;`FIPS186`&rbrack; の組み合わせは既定ではサポートされています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-371">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="ce51c-372">次の例では、HTTP/1.1 接続と HTTP/2 接続がポート 8000 上で許可されています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-372">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="ce51c-373">接続は、TLS と指定した証明書によってセキュリティで保護されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-373">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        }
```

<span data-ttu-id="ce51c-374">必要に応じて、`IConnectionAdapter` 実装を作成することで、接続ごとに特定の暗号について TLS ハンドシェイクをフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-374">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        }
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

<span data-ttu-id="ce51c-375">*構成からのプロトコルを設定する*</span><span class="sxs-lookup"><span data-stu-id="ce51c-375">*Set the protocol from configuration*</span></span>

<span data-ttu-id="ce51c-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は既定で `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` を呼び出して Kestrel の構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="ce51c-377">次に示す *appsettings.json* の例では、Kestrel のエンドポイントのすべてにおいて、既定の接続プロトコル (HTTP/1.1 および HTTP/2) が確定されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-377">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="ce51c-378">次に示す構成ファイルの例では、特定のエンドポイントに対して接続プロトコルが確定されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-378">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="ce51c-379">構成で設定された値は、コード内で指定されたプロトコルによってオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-379">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="ce51c-380">トランスポート構成</span><span class="sxs-lookup"><span data-stu-id="ce51c-380">Transport configuration</span></span>

<span data-ttu-id="ce51c-381">ASP.NET Core 2.1 のリリースにより、Kestrel の既定のトランスポートは、Libuv に基づかなくなり、代わりにマネージド ソケットに基づくようになりました。</span><span class="sxs-lookup"><span data-stu-id="ce51c-381">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="ce51c-382">これは、[WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) を呼び出す 2.1 にアップグレードする ASP.NET Core 2.0 アプリにとっては破壊的変更であり、以下のいずれかのパッケージに依存しています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-382">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="ce51c-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (パッケージの直接の参照)</span><span class="sxs-lookup"><span data-stu-id="ce51c-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="ce51c-384">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="ce51c-384">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="ce51c-385">[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) メタパッケージを使用し Libuv を使用する必要のある ASP.NET Core 2.1 以降のプロジェクトでは、次が必要です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-385">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="ce51c-386">アプリのプロジェクト ファイルに [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) パッケージの依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-386">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="ce51c-387">[WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-387">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="ce51c-388">URL プレフィックス</span><span class="sxs-lookup"><span data-stu-id="ce51c-388">URL prefixes</span></span>

<span data-ttu-id="ce51c-389">`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、または `ASPNETCORE_URLS` 環境変数を使用する場合、URL プレフィックスは次のいずれかの形式となります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-389">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-390">HTTP URL プレフィックスのみが有効です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-390">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="ce51c-391">`UseUrls` を使用して URL バインドを構成する場合、kestrel は SSL をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-391">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="ce51c-392">IPv4 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-392">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="ce51c-393">`0.0.0.0` は、すべての IPv4 アドレスにバインドする特別なケースです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-393">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="ce51c-394">IPv6 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-394">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="ce51c-395">IPv6 の `[::]` は IPv4 の `0.0.0.0` に相当します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-395">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="ce51c-396">ホスト名とポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-396">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="ce51c-397">ホスト名、`*`、および `+` は特別ではありません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-397">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="ce51c-398">有効な IP アドレスまたは `localhost` と認識されないものはすべて、IPv4 および IPv6 のすべての IP にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-398">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="ce51c-399">異なるホスト名を同じポート上の異なる ASP.NET Core アプリにバインドするには、[HTTP.sys](xref:fundamentals/servers/httpsys) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-399">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="ce51c-400">ホスト フィルタリングがリバース プロキシを使っていない場合は、[ホスト フィルタリング](#host-filtering)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-400">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="ce51c-401">`localhost` 名とポート番号、またはループバック IP とポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-401">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="ce51c-402">`localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-402">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="ce51c-403">要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-403">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="ce51c-404">他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-404">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="ce51c-405">IPv4 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-405">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="ce51c-406">`0.0.0.0` は、すべての IPv4 アドレスにバインドする特別なケースです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-406">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="ce51c-407">IPv6 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-407">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="ce51c-408">IPv6 の `[::]` は IPv4 の `0.0.0.0` に相当します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-408">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="ce51c-409">ホスト名とポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-409">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="ce51c-410">ホスト名、`*`、および `+` は特別ではありません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-410">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="ce51c-411">認識された IP アドレスまたは `localhost` ではないものはいずれも、IPv4 および IPv6 の IP にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-411">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="ce51c-412">異なるホスト名を同じポート上の異なる ASP.NET Core アプリにバインドするには、[WebListener](xref:fundamentals/servers/weblistener) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-412">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="ce51c-413">`localhost` 名とポート番号、またはループバック IP とポート番号</span><span class="sxs-lookup"><span data-stu-id="ce51c-413">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="ce51c-414">`localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-414">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="ce51c-415">要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-415">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="ce51c-416">他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-416">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="ce51c-417">UNIX ソケット</span><span class="sxs-lookup"><span data-stu-id="ce51c-417">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="ce51c-418">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="ce51c-418">**Port 0**</span></span>

<span data-ttu-id="ce51c-419">ポート番号 `0` を指定すると、Kestrel は使用可能なポートに動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-419">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="ce51c-420">ポート `0` へのバインドは、`localhost` を除き、任意のホスト名または IP で許可されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-420">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="ce51c-421">アプリを実行すると、コンソール ウィンドウの出力で、アプリがアクセスできる動的なポートが示されます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-421">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="ce51c-422">**SSL 用の URL プレフィックス**</span><span class="sxs-lookup"><span data-stu-id="ce51c-422">**URL prefixes for SSL**</span></span>

<span data-ttu-id="ce51c-423">`UseHttps` 拡張メソッドを呼び出す場合は、`https:` に URL プレフィックスを必ず含めます。</span><span class="sxs-lookup"><span data-stu-id="ce51c-423">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="ce51c-424">同じポート上で、HTTPS と HTTP をホストすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-424">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a><span data-ttu-id="ce51c-425">ホスト フィルタリング</span><span class="sxs-lookup"><span data-stu-id="ce51c-425">Host filtering</span></span>

<span data-ttu-id="ce51c-426">Kestrel は `http://example.com:5000` などのプレフィックスに基づく構成をサポートしますが、Kestrel はほとんどのホスト名を無視します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-426">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="ce51c-427">ホスト `localhost` は、ループバック アドレスへのバインドに使用される特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-427">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="ce51c-428">明示的な IP アドレス以外のすべてのホストは、すべてのパブリック IP アドレスにバインドします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-428">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="ce51c-429">この情報のいずれも、要求の `Host` ヘッダーの検証には使用されません。</span><span class="sxs-lookup"><span data-stu-id="ce51c-429">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ce51c-430">これを解決するには、ホスト ヘッダー フィルタリングを使用するリバース プロキシの背後でホストします。</span><span class="sxs-lookup"><span data-stu-id="ce51c-430">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="ce51c-431">これは、ASP.NET Core 1.x の Kestrel に対してのみサポートされるシナリオです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-431">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ce51c-432">これを解決するには、ミドルウェアを使用して、`Host` ヘッダーによって要求をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-432">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="ce51c-433">前述の `HostFilteringMiddleware` を `Startup.Configure` で登録します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-433">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="ce51c-434">[ミドルウェアの登録の順序](xref:fundamentals/middleware/index#order)が重要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-434">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#order) is important.</span></span> <span data-ttu-id="ce51c-435">この登録は、診断ミドルウェア (`app.UseExceptionHandler` など) の登録の直後に行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-435">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="ce51c-436">このミドルウェアには、*appsettings.json*/*appsettings.\<>.json* に、`AllowedHosts` キーを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-436">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="ce51c-437">この値は、ポート番号を含まないホスト名のセミコロン区切りリストです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-437">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ce51c-438">これを解決するには、Host Filtering Middleware を使用します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-438">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="ce51c-439">Host Filtering Middleware は、[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) に含まれる、[Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) パッケージによって提供されています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-439">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="ce51c-440">このミドルウェアは、[AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering) を呼び出す、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) によって追加されています。</span><span class="sxs-lookup"><span data-stu-id="ce51c-440">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="ce51c-441">Host Filtering Middleware は既定では無効です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-441">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="ce51c-442">このミドルウェアを有効にするには、*appsettings.json*/*appsettings.\<>.json* に、`AllowedHosts` キーを定義します。</span><span class="sxs-lookup"><span data-stu-id="ce51c-442">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="ce51c-443">この値は、ポート番号を含まないホスト名のセミコロン区切りリストです。</span><span class="sxs-lookup"><span data-stu-id="ce51c-443">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ce51c-444">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ce51c-444">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="ce51c-445">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) には、[ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) オプションもあります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-445">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="ce51c-446">Forwarded Headers Middleware および Host Filtering Middleware には、異なるシナリオ用に類似した機能があります。</span><span class="sxs-lookup"><span data-stu-id="ce51c-446">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="ce51c-447">リバース プロキシ サーバーまたはロード バランサーを使用して要求を転送するとき、ホスト ヘッダーが保存されていない場合、Forwarded Headers Middleware に `AllowedHosts` を設定するのが適切です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-447">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="ce51c-448">Kestrel がエッジ サーバーとして使用されていたり、ホスト ヘッダーが直接転送されたりしている場合、Host Filtering Middleware に `AllowedHosts` を設定するのが適切です。</span><span class="sxs-lookup"><span data-stu-id="ce51c-448">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="ce51c-449">Forwarded Headers Middleware の詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ce51c-449">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ce51c-450">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ce51c-450">Additional resources</span></span>

* [<span data-ttu-id="ce51c-451">HTTPS の適用</span><span class="sxs-lookup"><span data-stu-id="ce51c-451">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="ce51c-452">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="ce51c-452">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* <span data-ttu-id="ce51c-453">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)(RFC 7230: メッセージの構文と経路制御 (セクション 5.4: ホスト))</span><span class="sxs-lookup"><span data-stu-id="ce51c-453">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)</span></span>
* [<span data-ttu-id="ce51c-454">プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する</span><span class="sxs-lookup"><span data-stu-id="ce51c-454">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
