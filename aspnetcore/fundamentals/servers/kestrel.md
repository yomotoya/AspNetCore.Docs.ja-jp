---
title: ASP.NET Core への Kestrel Web サーバーの実装
author: rick-anderson
description: ASP.NET Core 用のクロスプラットフォーム Web サーバーである Kestrel について説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 7c7ff337256792dde057c04a5b96a1eb1c093387
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902633"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="460e5-103">ASP.NET Core への Kestrel Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="460e5-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="460e5-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)、[Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="460e5-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="460e5-105">Kestrel は、[ASP.NET Core 向けのクロスプラットフォーム Web サーバー](xref:fundamentals/servers/index)です。</span><span class="sxs-lookup"><span data-stu-id="460e5-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="460e5-106">Kestrel は、ASP.NET Core のプロジェクト テンプレートに既定で含まれる Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="460e5-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="460e5-107">Kestrel は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="460e5-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="460e5-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="460e5-108">HTTPS</span></span>
* <span data-ttu-id="460e5-109">[Websocket](https://github.com/aspnet/websockets) を有効にするために使用される非透過的なアップグレード</span><span class="sxs-lookup"><span data-stu-id="460e5-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="460e5-110">Nginx の背後にある高パフォーマンスの UNIX ソケット</span><span class="sxs-lookup"><span data-stu-id="460e5-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="460e5-111">kestrel は、.NET Core がサポートするすべてのプラットフォームおよびバージョンでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="460e5-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="460e5-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="460e5-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="460e5-113">Kestrel とリバース プロキシを使用するタイミング</span><span class="sxs-lookup"><span data-stu-id="460e5-113">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="460e5-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="460e5-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="460e5-115">Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。</span><span class="sxs-lookup"><span data-stu-id="460e5-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="460e5-116">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="460e5-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="460e5-119">リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-119">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="460e5-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="460e5-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="460e5-121">アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel をアプリのサーバーとして直接使うことができます。</span><span class="sxs-lookup"><span data-stu-id="460e5-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="460e5-123">アプリをインターネットに公開する場合は、"*リバース プロキシ サーバー*" として IIS、Nginx、または Apache を使います。</span><span class="sxs-lookup"><span data-stu-id="460e5-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="460e5-124">リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。</span><span class="sxs-lookup"><span data-stu-id="460e5-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="460e5-126">リバース プロキシは、セキュリティ上の理由からエッジ展開 (インターネットからのトラフィックに公開される) で必要となります。</span><span class="sxs-lookup"><span data-stu-id="460e5-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="460e5-127">1.x バージョンの Kestrel は、適切なタイムアウト、サイズの制限、同時接続の制限など、攻撃に対する防御の機能が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="460e5-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="460e5-128">リバース プロキシのシナリオは、単一のサーバー上で実行される同じ IP とポートを共有する複数のアプリがある場合に存在します。</span><span class="sxs-lookup"><span data-stu-id="460e5-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="460e5-129">Kestrel は、複数のプロセスによる同じ IP とポートの共有をサポートしていないため、このシナリオには対応しません。</span><span class="sxs-lookup"><span data-stu-id="460e5-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="460e5-130">ポートでリッスンするように Kestrel を構成すると、Kestrel は、要求のホスト ヘッダーに関係なく、そのポートに対するすべてのトラフィックを処理します。</span><span class="sxs-lookup"><span data-stu-id="460e5-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="460e5-131">ポートを共有できるリバース プロキシは、一意の IP とポート上の Kestrel に要求を転送することができます。</span><span class="sxs-lookup"><span data-stu-id="460e5-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="460e5-132">リバース プロキシ サーバーが必要ない場合であっても、リバース プロキシ サーバーを使用すると次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="460e5-133">それがホストするアプリの公開されるパブリック サーフェス領域を制限することができます。</span><span class="sxs-lookup"><span data-stu-id="460e5-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="460e5-134">構成および防御の層を追加します。</span><span class="sxs-lookup"><span data-stu-id="460e5-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="460e5-135">既存のインフラストラクチャとより適切に統合できる場合があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="460e5-136">負荷分散と SSL の構成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="460e5-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="460e5-137">リバース プロキシ サーバーのみで SSL 証明書が必要です。このサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリ サーバーと通信することができます。</span><span class="sxs-lookup"><span data-stu-id="460e5-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="460e5-138">ホスト フィルタリングがリバース プロキシを使っていない場合は、[ホスト フィルタリング](#host-filtering)を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="460e5-139">ASP.NET Core アプリで Kestrel を使用する方法</span><span class="sxs-lookup"><span data-stu-id="460e5-139">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="460e5-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="460e5-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="460e5-141">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) パッケージは、[Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) に含まれています。</span><span class="sxs-lookup"><span data-stu-id="460e5-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="460e5-142">ASP.NET Core プロジェクト テンプレートは既定では Kestrel を使用します。</span><span class="sxs-lookup"><span data-stu-id="460e5-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="460e5-143">*Program.cs* 内のテンプレート コードは [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出し、これによってバックグラウンドで [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="460e5-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="460e5-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="460e5-145">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="460e5-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="460e5-146">`Main` メソッド内の [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) で [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) 拡張メソッドを呼び出して、必要な [Kestrel オプション](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)を指定します (次のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="460e5-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="460e5-147">Kestrel オプション</span><span class="sxs-lookup"><span data-stu-id="460e5-147">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="460e5-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="460e5-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="460e5-149">Kestrel Web サーバーには、インターネットに接続する展開で特に有効な制約構成オプションがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="460e5-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="460e5-150">カスタマイズできる重要な制限は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="460e5-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="460e5-151">クライアントの最大接続数</span><span class="sxs-lookup"><span data-stu-id="460e5-151">Maximum client connections</span></span>
* <span data-ttu-id="460e5-152">要求本文の最大サイズ</span><span class="sxs-lookup"><span data-stu-id="460e5-152">Maximum request body size</span></span>
* <span data-ttu-id="460e5-153">要求本文の最小レート</span><span class="sxs-lookup"><span data-stu-id="460e5-153">Minimum request body data rate</span></span>

<span data-ttu-id="460e5-154">[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) クラスの [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) プロパティで、上記の制約およびその他の制約を設定します。</span><span class="sxs-lookup"><span data-stu-id="460e5-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="460e5-155">`Limits` プロパティは、[KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) クラスのインスタンスを保持します。</span><span class="sxs-lookup"><span data-stu-id="460e5-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="460e5-156">**クライアントの最大接続数**</span><span class="sxs-lookup"><span data-stu-id="460e5-156">**Maximum client connections**</span></span>

[<span data-ttu-id="460e5-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="460e5-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="460e5-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="460e5-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="460e5-159">次のコードを使用することで、アプリ全体に対して同時に開かれる TCP 接続の最大数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="460e5-160">HTTP または HTTPS から別のプロトコルにアップグレードされた接続については別個の制限があります (WebSockets 要求に関する制限など)。</span><span class="sxs-lookup"><span data-stu-id="460e5-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="460e5-161">接続がアップグレードされた後、それは `MaxConcurrentConnections` 制限に対してカウントされません。</span><span class="sxs-lookup"><span data-stu-id="460e5-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="460e5-162">接続の最大数は既定で無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="460e5-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="460e5-163">**要求本文の最大サイズ**</span><span class="sxs-lookup"><span data-stu-id="460e5-163">**Maximum request body size**</span></span>

[<span data-ttu-id="460e5-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="460e5-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="460e5-165">既定の要求本文の最大サイズは、30,000,000 バイトです。これは約 28.6 MB になります。</span><span class="sxs-lookup"><span data-stu-id="460e5-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="460e5-166">ASP.NET Core MVC アプリでの制限をオーバーライドする方法としては、次のように、アクション メソッドに対して [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 属性を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="460e5-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="460e5-167">次の例では、すべての要求についての制約をアプリに対して構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="460e5-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="460e5-168">ミドルウェア内の特定の要求に関する設定をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="460e5-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="460e5-169">アプリが要求の読み取りを開始した後で、要求に対する制限を構成しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="460e5-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="460e5-170">`MaxRequestBodySize` プロパティが読み取り専用状態にある (制限を構成するには遅すぎる) かどうかを示す `IsReadOnly` プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="460e5-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="460e5-171">**要求本文の最小レート**</span><span class="sxs-lookup"><span data-stu-id="460e5-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="460e5-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="460e5-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="460e5-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="460e5-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="460e5-174">kestrel はデータが指定のレート (バイト数/秒) で到着しているかどうかを毎秒チェックします。</span><span class="sxs-lookup"><span data-stu-id="460e5-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="460e5-175">レートが最小値を下回った場合は接続がタイムアウトになります。猶予期間とは、クライアントによって送信速度を最低ラインまで引き上げられるのを、Kestrel が待機する時間のことです。この期間中、レートはチェックされません。</span><span class="sxs-lookup"><span data-stu-id="460e5-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="460e5-176">猶予期間により、TCP のスロースタートのため最初にデータを低速で送信する接続がドロップされるのを回避できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="460e5-177">既定の最小レートは 240 バイト/秒であり、5 秒の猶予時間が設定されています。</span><span class="sxs-lookup"><span data-stu-id="460e5-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="460e5-178">最小レートは応答にも適用されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="460e5-179">要求制限と応答制限を設定するコードは、プロパティ名およびインターフェイス名に `RequestBody` または `Response` が使用されることを除けば同じです。</span><span class="sxs-lookup"><span data-stu-id="460e5-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="460e5-180">次の例では、*Program.cs* で最小データ レートを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="460e5-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

<span data-ttu-id="460e5-181">ミドルウェアで要求レートを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="460e5-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="460e5-182">Kestrel のその他のオプションと制限については、以下をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="460e5-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="460e5-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="460e5-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="460e5-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="460e5-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="460e5-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="460e5-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="460e5-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="460e5-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="460e5-187">Kestrel のオプションと制限については、以下をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="460e5-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="460e5-188">KestrelServerOptions クラス</span><span class="sxs-lookup"><span data-stu-id="460e5-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="460e5-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="460e5-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a><span data-ttu-id="460e5-190">エンドポイントの構成</span><span class="sxs-lookup"><span data-stu-id="460e5-190">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="460e5-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="460e5-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="460e5-192">既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="460e5-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="460e5-193">Kestrel の URL プレフィックスとポートを構成するには、[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) で [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) または [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="460e5-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="460e5-194">`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、`ASPNETCORE_URLS` 環境変数も機能しますが、このセクションで後述する制限があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="460e5-195">`urls` ホスト構成キーは、アプリ構成ではなくホスト構成から取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="460e5-196">ホストは構成が構成ファイルから読み取られるまでに完全に初期化されるため、`urls` キーと値を *appsettings.json* に追加しても、ホストの構成には影響しません。</span><span class="sxs-lookup"><span data-stu-id="460e5-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="460e5-197">ただし、*appsettings.json* の `urls` キーをホスト ビルダー [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) と共に使用して、ホストを構成できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

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

<span data-ttu-id="460e5-198">既定では、ASP.NET Core は以下にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="460e5-198">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="460e5-199">`https://localhost:5001` (ローカル開発証明書が存在する場合)</span><span class="sxs-lookup"><span data-stu-id="460e5-199">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="460e5-200">開発証明書が作成されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-200">A development certificate is created:</span></span>

* <span data-ttu-id="460e5-201">[.NET Core SDK](/dotnet/core/sdk) がインストールされるとき。</span><span class="sxs-lookup"><span data-stu-id="460e5-201">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="460e5-202">証明書を作成するには [dev-certs ツール](xref:aspnetcore-2.1#https)を使います。</span><span class="sxs-lookup"><span data-stu-id="460e5-202">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="460e5-203">一部のブラウザーでは、ローカル開発証明書を信頼するには、ブラウザーに明示的にアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-203">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="460e5-204">ASP.NET Core 2.1 およびそれより後のプロジェクト テンプレートでは、アプリは HTTPS で実行するように既定で構成され、[HTTPS のリダイレクトと HSTS のサポート](xref:security/enforcing-ssl)が組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="460e5-204">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="460e5-205">Kestrel の URL プレフィックスとポートを構成するには、[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) で [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) または [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="460e5-205">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="460e5-206">`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、`ASPNETCORE_URLS` 環境変数も機能しますが、このセクションで後述する制限があります (既定の証明書が、HTTPS エンドポイントの構成に使用できる必要があります)。</span><span class="sxs-lookup"><span data-stu-id="460e5-206">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="460e5-207">ASP.NET Core 2.1 の `KestrelServerOptions` の構成:</span><span class="sxs-lookup"><span data-stu-id="460e5-207">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="460e5-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="460e5-208">**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**</span></span>  
<span data-ttu-id="460e5-209">指定した各エンドポイントに対して実行するように、構成の `Action` を指定します。</span><span class="sxs-lookup"><span data-stu-id="460e5-209">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="460e5-210">`ConfigureEndpointDefaults` を複数回呼び出すと、前の `Action` が最後に指定した `Action` で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="460e5-210">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="460e5-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span><span class="sxs-lookup"><span data-stu-id="460e5-211">**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**</span></span>  
<span data-ttu-id="460e5-212">各 HTTPS エンドポイントに対して実行するように、構成の `Action` を指定します。</span><span class="sxs-lookup"><span data-stu-id="460e5-212">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="460e5-213">`ConfigureHttpsDefaults` を複数回呼び出すと、前の `Action` が最後に指定した `Action` で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="460e5-213">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="460e5-214">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="460e5-214">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="460e5-215">入力として [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を受け取る Kestrel を設定するための構成ローダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="460e5-215">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="460e5-216">構成のスコープは、Kestrel の構成セクションにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-216">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="460e5-217">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="460e5-217">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="460e5-218">HTTPS を使用するように Kestrel を構成します。</span><span class="sxs-lookup"><span data-stu-id="460e5-218">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="460e5-219">`ListenOptions.UseHttps` 拡張機能:</span><span class="sxs-lookup"><span data-stu-id="460e5-219">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="460e5-220">`UseHttps` &ndash; 既定の証明書で HTTPS を使うように Kestrel を構成します。</span><span class="sxs-lookup"><span data-stu-id="460e5-220">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="460e5-221">既定の証明書が構成されていない場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="460e5-221">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="460e5-222">`ListenOptions.UseHttps` パラメーター:</span><span class="sxs-lookup"><span data-stu-id="460e5-222">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="460e5-223">`filename` は、アプリのコンテンツ ファイルが格納されているディレクトリを基準とする、証明書ファイルのパスとファイル名です。</span><span class="sxs-lookup"><span data-stu-id="460e5-223">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="460e5-224">`password` は、X.509 証明書データにアクセスするために必要なパスワードです。</span><span class="sxs-lookup"><span data-stu-id="460e5-224">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="460e5-225">`configureOptions` は、`HttpsConnectionAdapterOptions` を構成するための `Action` です。</span><span class="sxs-lookup"><span data-stu-id="460e5-225">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="460e5-226">`ListenOptions` を返します。</span><span class="sxs-lookup"><span data-stu-id="460e5-226">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="460e5-227">`storeName` は、証明書の読み込み元の証明書ストアです。</span><span class="sxs-lookup"><span data-stu-id="460e5-227">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="460e5-228">`subject` は、証明書のサブジェクト名です。</span><span class="sxs-lookup"><span data-stu-id="460e5-228">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="460e5-229">`allowInvalid` は、無効な証明書 (自己署名証明書など) を考慮する必要があるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="460e5-229">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="460e5-230">`location` は、証明書を読み込むストアの場所です。</span><span class="sxs-lookup"><span data-stu-id="460e5-230">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="460e5-231">`serverCertificate` は、X.509 証明書です。</span><span class="sxs-lookup"><span data-stu-id="460e5-231">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="460e5-232">運用環境では、HTTPS を明示的に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-232">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="460e5-233">少なくとも、既定の証明書を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-233">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="460e5-234">サポートされている構成を次に説明します。</span><span class="sxs-lookup"><span data-stu-id="460e5-234">Supported configurations described next:</span></span>

* <span data-ttu-id="460e5-235">構成なし</span><span class="sxs-lookup"><span data-stu-id="460e5-235">No configuration</span></span>
* <span data-ttu-id="460e5-236">構成から既定の証明書を置き換える</span><span class="sxs-lookup"><span data-stu-id="460e5-236">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="460e5-237">コードで既定値を変更する</span><span class="sxs-lookup"><span data-stu-id="460e5-237">Change the defaults in code</span></span>

<span data-ttu-id="460e5-238">*構成なし*</span><span class="sxs-lookup"><span data-stu-id="460e5-238">*No configuration*</span></span>

<span data-ttu-id="460e5-239">Kestrel は、`http://localhost:5000` と `https://localhost:5001` (既定の証明書が使用可能な場合) でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="460e5-239">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="460e5-240">以下を使用して URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="460e5-240">Specify URLs using the:</span></span>

* <span data-ttu-id="460e5-241">`ASPNETCORE_URLS` 環境変数。</span><span class="sxs-lookup"><span data-stu-id="460e5-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="460e5-242">`--urls` コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="460e5-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="460e5-243">`urls` ホスト構成キー。</span><span class="sxs-lookup"><span data-stu-id="460e5-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="460e5-244">`UseUrls` 拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="460e5-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="460e5-245">詳しくは、「[サーバーの URL](xref:fundamentals/host/web-host#server-urls)」および「[構成のオーバーライド](xref:fundamentals/host/web-host#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="460e5-245">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="460e5-246">これらの方法を使うと、1 つまたは複数の HTTP エンドポイントおよび HTTPS エンドポイント (既定の証明書が使用可能な場合は HTTPS) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-246">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="460e5-247">セミコロン区切りのリストとして値を構成します (例: `"Urls": "http://localhost:8000;http://localhost:8001"`)。</span><span class="sxs-lookup"><span data-stu-id="460e5-247">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="460e5-248">*構成から既定の証明書を置き換える*</span><span class="sxs-lookup"><span data-stu-id="460e5-248">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="460e5-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は既定で `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` を呼び出して Kestrel の構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="460e5-249">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="460e5-250">Kestrel は、既定の HTTPS アプリ設定構成スキーマを使用できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-250">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="460e5-251">ディスク上のファイルまたは証明書ストアから、URL や使用する証明書など、複数のエンドポイントを構成します。</span><span class="sxs-lookup"><span data-stu-id="460e5-251">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="460e5-252">以下の *appsettings.json* の例では、次のことが行われています。</span><span class="sxs-lookup"><span data-stu-id="460e5-252">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="460e5-253">**AllowInvalid** を `true` に設定し、の無効な証明書 (自己署名証明書など) の使用を許可します。</span><span class="sxs-lookup"><span data-stu-id="460e5-253">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="460e5-254">証明書 (後の例では **HttpsDefaultCert**) が指定されていないすべての HTTPS エンドポイントは、**[証明書]** > **[既定]** または開発証明書で定義されている証明書にフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="460e5-254">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="460e5-255">証明書ノードの **Path** と **Password** を使用する代わりの方法は、証明書ストアのフィールドを使って証明書を指定することです。</span><span class="sxs-lookup"><span data-stu-id="460e5-255">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="460e5-256">たとえば、**[証明書]** > **[既定]** の証明書は次のように指定できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-256">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="460e5-257">スキーマに関する注意事項:</span><span class="sxs-lookup"><span data-stu-id="460e5-257">Schema notes:</span></span>

* <span data-ttu-id="460e5-258">エンドポイント名は大文字と小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-258">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="460e5-259">たとえば、`HTTPS` と `Https` は有効です。</span><span class="sxs-lookup"><span data-stu-id="460e5-259">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="460e5-260">`Url` パラメーターは、エンドポイントごとに必要です。</span><span class="sxs-lookup"><span data-stu-id="460e5-260">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="460e5-261">このパラメーターの形式は、1 つの値に制限されることを除き、最上位レベルの `Urls` 構成パラメーターと同じです。</span><span class="sxs-lookup"><span data-stu-id="460e5-261">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="460e5-262">これらのエンドポイントは、最上位レベルの `Urls` 構成での定義に追加されるのではなく、それを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="460e5-262">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="460e5-263">コードで `Listen` を使用して定義されているエンドポイントは、構成セクションで定義されているエンドポイントに累積されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-263">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="460e5-264">`Certificate` セクションは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="460e5-264">The `Certificate` section is optional.</span></span> <span data-ttu-id="460e5-265">`Certificate` セクションを指定しないと、前述のシナリオで定義した既定値が使用されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-265">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="460e5-266">既定値を使用できない場合、サーバーにより例外がスローされ、開始できません。</span><span class="sxs-lookup"><span data-stu-id="460e5-266">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="460e5-267">`Certificate` セクションは、**Path**&ndash;**Password** 証明書と **Subject**&ndash;**Store** 証明書の両方をサポートします。</span><span class="sxs-lookup"><span data-stu-id="460e5-267">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="460e5-268">ポートが競合しない限り、この方法で任意の数のエンドポイントを定義できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-268">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="460e5-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` が `.Endpoint(string name, options => { })` メソッドで返す `KestrelConfigurationLoader` を使用して、構成されているエンドポイントの設定を補足できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-269">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="460e5-270">`KestrelServerOptions.ConfigurationLoader` に直接アクセスして、既存のローダー ([WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) によって提供されるものなど) の反復を維持することもできます。</span><span class="sxs-lookup"><span data-stu-id="460e5-270">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="460e5-271">カスタム設定を読み取ることができるように、各エンドポイントの構成セクションを `Endpoint` メソッドのオプションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-271">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="460e5-272">別のセクションで `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` を再び呼び出すことにより、複数の構成を読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="460e5-272">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="460e5-273">前のインスタンスで `Load` を明示的に呼び出していない限り、最後の構成のみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-273">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="460e5-274">既定の構成セクションを置き換えることができるように、メタパッケージは `Load` を呼び出しません。</span><span class="sxs-lookup"><span data-stu-id="460e5-274">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="460e5-275">`KestrelConfigurationLoader` はミラー、`KestrelServerOptions` から API の `Listen` ファミリを `Endpoint` オーバーロードとしてミラーリングするので、コードと構成のエンドポイントを同じ場所で構成できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-275">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="460e5-276">これらのオーバーロードでは名前は使用されず、構成からの既定の設定のみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-276">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="460e5-277">*コードで既定値を変更する*</span><span class="sxs-lookup"><span data-stu-id="460e5-277">*Change the defaults in code*</span></span>

<span data-ttu-id="460e5-278">`ConfigureEndpointDefaults` および`ConfigureHttpsDefaults` を使用して、`ListenOptions` および `HttpsConnectionAdapterOptions` の既定の設定を変更できます (前のシナリオで指定した既定の証明書のオーバーライドなど)。</span><span class="sxs-lookup"><span data-stu-id="460e5-278">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="460e5-279">エンドポイントを構成する前に、`ConfigureEndpointDefaults` および `ConfigureHttpsDefaults` を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="460e5-280">*Kestrel による SNI のサポート*</span><span class="sxs-lookup"><span data-stu-id="460e5-280">*Kestrel support for SNI*</span></span>

<span data-ttu-id="460e5-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) を使用すると、同じ IP アドレスとポートで複数のドメインをホストできます。</span><span class="sxs-lookup"><span data-stu-id="460e5-281">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="460e5-282">SNI が機能するためには、サーバーが正しい証明書を提供できるように、クライアントは TLS ハンドシェイクの間にセキュリティで保護されたセッションのホスト名をサーバーに送信します。</span><span class="sxs-lookup"><span data-stu-id="460e5-282">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="460e5-283">クライアントは、TLS ハンドシェイクに続くセキュリティで保護されたセッション中に、提供された証明書をサーバーとの暗号化された通信に使用します。</span><span class="sxs-lookup"><span data-stu-id="460e5-283">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="460e5-284">Kestrel は、`ServerCertificateSelector` コールバックによって SNI をサポートします。</span><span class="sxs-lookup"><span data-stu-id="460e5-284">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="460e5-285">コールバックは接続ごとに 1 回呼び出されるので、アプリはそれを使って、ホスト名を検査し、適切な証明書を選択できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-285">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="460e5-286">SNI のサポートには、次が必要です。</span><span class="sxs-lookup"><span data-stu-id="460e5-286">SNI support requires:</span></span>

* <span data-ttu-id="460e5-287">ターゲット フレームワーク `netcoreapp2.1` で実行される必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-287">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="460e5-288">`netcoreapp2.0` および `net461` の場合は、コールバックは呼び出されますが、`name` は常に `null` です。</span><span class="sxs-lookup"><span data-stu-id="460e5-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="460e5-289">クライアントが TLS ハンドシェイクでホスト名パラメーターを提供しない場合も、`name` は `null` です。</span><span class="sxs-lookup"><span data-stu-id="460e5-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="460e5-290">すべての Web サイトは、同じ Kestrel インスタンスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-290">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="460e5-291">Kestrel では、リバース プロキシは使用しない、複数のインスタンスでの IP アドレスとポートの共有はサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="460e5-291">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
WebHost.CreateDefaultBuilder()
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
    });
```

::: moniker-end

<span data-ttu-id="460e5-292">**TCP ソケットにバインドする**</span><span class="sxs-lookup"><span data-stu-id="460e5-292">**Bind to a TCP socket**</span></span>

<span data-ttu-id="460e5-293">[Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) メソッドは TCP ソケットにバインドし、オプションのラムダにより SSL 証明書を構成できます。</span><span class="sxs-lookup"><span data-stu-id="460e5-293">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="460e5-294">例では、[ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) を使用して、エンドポイントに対する SSL が構成されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-294">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="460e5-295">同じ API を使用して、特定のエンドポイントに対する他の Kestrel 設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="460e5-295">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="460e5-296">**UNIX ソケットにバインドする**</span><span class="sxs-lookup"><span data-stu-id="460e5-296">**Bind to a Unix socket**</span></span>

<span data-ttu-id="460e5-297">次の例に示すように、Nginx でのパフォーマンス向上のため、[ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) で UNIX ソケットをリッスンします。</span><span class="sxs-lookup"><span data-stu-id="460e5-297">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="460e5-298">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="460e5-298">**Port 0**</span></span>

<span data-ttu-id="460e5-299">ポート番号 `0` を指定すると、Kestrel は使用可能なポートに動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="460e5-299">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="460e5-300">次の例では、Kestrel が実行時に実際にバインドするポートを特定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="460e5-300">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="460e5-301">アプリを実行すると、コンソール ウィンドウの出力で、アプリがアクセスできる動的なポートが示されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-301">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

<span data-ttu-id="460e5-302">**UseUrls、--url コマンド ライン引数、url ホスト構成キー、および ASPNETCORE_URLS 環境変数の制限事項**</span><span class="sxs-lookup"><span data-stu-id="460e5-302">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="460e5-303">次の方法でエンドポイントを構成します。</span><span class="sxs-lookup"><span data-stu-id="460e5-303">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="460e5-304">UseUrls</span><span class="sxs-lookup"><span data-stu-id="460e5-304">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="460e5-305">`--urls` コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="460e5-305">`--urls` command-line argument</span></span>
* <span data-ttu-id="460e5-306">`urls` ホスト構成キー</span><span class="sxs-lookup"><span data-stu-id="460e5-306">`urls` host configuration key</span></span>
* <span data-ttu-id="460e5-307">`ASPNETCORE_URLS` 環境変数</span><span class="sxs-lookup"><span data-stu-id="460e5-307">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="460e5-308">コードを Kestrel 以外のサーバーで機能させるには、これらのメソッドが有用です。</span><span class="sxs-lookup"><span data-stu-id="460e5-308">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="460e5-309">ただし、次の制限事項に注意してください。</span><span class="sxs-lookup"><span data-stu-id="460e5-309">However, be aware of these limitations:</span></span>

* <span data-ttu-id="460e5-310">HTTPS エンドポイントの構成で (たとえば、前に説明したように `KestrelServerOptions` 構成または構成ファイルを使用して) 既定の証明書が提供されていない場合、これらの方法で SSL を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="460e5-310">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="460e5-311">`Listen` と `UseUrls` 両方の方法を同時に使用すると、`Listen` エンドポイントが `UseUrls` エンドポイントをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="460e5-311">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="460e5-312">**IIS エンドポイントの構成**</span><span class="sxs-lookup"><span data-stu-id="460e5-312">**IIS endpoint configuration**</span></span>

<span data-ttu-id="460e5-313">IIS を使用するときは、IIS オーバーライド バインドに対する URL バインドを、`Listen` または `UseUrls` によって設定します。</span><span class="sxs-lookup"><span data-stu-id="460e5-313">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="460e5-314">詳しくは、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="460e5-314">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="460e5-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="460e5-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="460e5-316">既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="460e5-316">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="460e5-317">以下を使用して Kestrel の URL プレフィックスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="460e5-317">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="460e5-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) 拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="460e5-318">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="460e5-319">`--urls` コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="460e5-319">`--urls` command-line argument</span></span>
* <span data-ttu-id="460e5-320">`urls` ホスト構成キー</span><span class="sxs-lookup"><span data-stu-id="460e5-320">`urls` host configuration key</span></span>
* <span data-ttu-id="460e5-321">ASP.NET Core 構成システム (`ASPNETCORE_URLS` 環境変数など)</span><span class="sxs-lookup"><span data-stu-id="460e5-321">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="460e5-322">これらのメソッドについて詳しくは、[ホスティング](xref:fundamentals/host/index)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="460e5-322">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

<span data-ttu-id="460e5-323">**IIS エンドポイントの構成**</span><span class="sxs-lookup"><span data-stu-id="460e5-323">**IIS endpoint configuration**</span></span>

<span data-ttu-id="460e5-324">IIS を使用するときは、IIS オーバーライド バインドに対する URL バインドを、`UseUrls` によって設定します。</span><span class="sxs-lookup"><span data-stu-id="460e5-324">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="460e5-325">詳しくは、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="460e5-325">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="460e5-326">トランスポート構成</span><span class="sxs-lookup"><span data-stu-id="460e5-326">Transport configuration</span></span>

<span data-ttu-id="460e5-327">ASP.NET Core 2.1 のリリースにより、Kestrel の既定のトランスポートは、Libuv に基づかなくなり、代わりにマネージド ソケットに基づくようになりました。</span><span class="sxs-lookup"><span data-stu-id="460e5-327">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="460e5-328">これは、[WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) を呼び出す 2.1 にアップグレードする ASP.NET Core 2.0 アプリにとっては破壊的変更であり、以下のいずれかのパッケージに依存しています。</span><span class="sxs-lookup"><span data-stu-id="460e5-328">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="460e5-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (パッケージの直接の参照)</span><span class="sxs-lookup"><span data-stu-id="460e5-329">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="460e5-330">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="460e5-330">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="460e5-331">[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) メタパッケージを使用し Libuv を使用する必要のある ASP.NET Core 2.1 以降のプロジェクトでは、次が必要です。</span><span class="sxs-lookup"><span data-stu-id="460e5-331">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="460e5-332">アプリのプロジェクト ファイルに [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) パッケージの依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="460e5-332">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* <span data-ttu-id="460e5-333">[WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="460e5-333">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="460e5-334">URL プレフィックス</span><span class="sxs-lookup"><span data-stu-id="460e5-334">URL prefixes</span></span>

<span data-ttu-id="460e5-335">`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、または `ASPNETCORE_URLS` 環境変数を使用する場合、URL プレフィックスは次のいずれかの形式となります。</span><span class="sxs-lookup"><span data-stu-id="460e5-335">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="460e5-336">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="460e5-336">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="460e5-337">HTTP URL プレフィックスのみが有効です。</span><span class="sxs-lookup"><span data-stu-id="460e5-337">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="460e5-338">`UseUrls` を使用して URL バインドを構成する場合、kestrel は SSL をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="460e5-338">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="460e5-339">IPv4 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-339">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="460e5-340">`0.0.0.0` は、すべての IPv4 アドレスにバインドする特別なケースです。</span><span class="sxs-lookup"><span data-stu-id="460e5-340">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="460e5-341">IPv6 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-341">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="460e5-342">IPv6 の `[::]` は IPv4 の `0.0.0.0` に相当します。</span><span class="sxs-lookup"><span data-stu-id="460e5-342">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="460e5-343">ホスト名とポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-343">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="460e5-344">ホスト名、`*`、および `+` は特別ではありません。</span><span class="sxs-lookup"><span data-stu-id="460e5-344">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="460e5-345">有効な IP アドレスまたは `localhost` と認識されないものはすべて、IPv4 および IPv6 のすべての IP にバインドします。</span><span class="sxs-lookup"><span data-stu-id="460e5-345">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="460e5-346">異なるホスト名を同じポート上の異なる ASP.NET Core アプリにバインドするには、[HTTP.sys](xref:fundamentals/servers/httpsys) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="460e5-346">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="460e5-347">ホスト フィルタリングがリバース プロキシを使っていない場合は、[ホスト フィルタリング](#host-filtering)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="460e5-347">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="460e5-348">`localhost` 名とポート番号、またはループバック IP とポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-348">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="460e5-349">`localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。</span><span class="sxs-lookup"><span data-stu-id="460e5-349">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="460e5-350">要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。</span><span class="sxs-lookup"><span data-stu-id="460e5-350">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="460e5-351">他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="460e5-351">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="460e5-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="460e5-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="460e5-353">IPv4 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-353">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="460e5-354">`0.0.0.0` は、すべての IPv4 アドレスにバインドする特別なケースです。</span><span class="sxs-lookup"><span data-stu-id="460e5-354">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="460e5-355">IPv6 アドレスとポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-355">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="460e5-356">IPv6 の `[::]` は IPv4 の `0.0.0.0` に相当します。</span><span class="sxs-lookup"><span data-stu-id="460e5-356">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="460e5-357">ホスト名とポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-357">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="460e5-358">ホスト名、`*`、および `+` は特別ではありません。</span><span class="sxs-lookup"><span data-stu-id="460e5-358">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="460e5-359">認識された IP アドレスまたは `localhost` ではないものはいずれも、IPv4 および IPv6 の IP にバインドします。</span><span class="sxs-lookup"><span data-stu-id="460e5-359">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="460e5-360">異なるホスト名を同じポート上の異なる ASP.NET Core アプリにバインドするには、[WebListener](xref:fundamentals/servers/weblistener) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="460e5-360">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="460e5-361">`localhost` 名とポート番号、またはループバック IP とポート番号</span><span class="sxs-lookup"><span data-stu-id="460e5-361">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="460e5-362">`localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。</span><span class="sxs-lookup"><span data-stu-id="460e5-362">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="460e5-363">要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。</span><span class="sxs-lookup"><span data-stu-id="460e5-363">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="460e5-364">他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="460e5-364">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="460e5-365">UNIX ソケット</span><span class="sxs-lookup"><span data-stu-id="460e5-365">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="460e5-366">**ポート 0**</span><span class="sxs-lookup"><span data-stu-id="460e5-366">**Port 0**</span></span>

<span data-ttu-id="460e5-367">ポート番号 `0` を指定すると、Kestrel は使用可能なポートに動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="460e5-367">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="460e5-368">ポート `0` へのバインドは、`localhost` を除き、任意のホスト名または IP で許可されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-368">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="460e5-369">アプリを実行すると、コンソール ウィンドウの出力で、アプリがアクセスできる動的なポートが示されます。</span><span class="sxs-lookup"><span data-stu-id="460e5-369">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="460e5-370">**SSL 用の URL プレフィックス**</span><span class="sxs-lookup"><span data-stu-id="460e5-370">**URL prefixes for SSL**</span></span>

<span data-ttu-id="460e5-371">`UseHttps` 拡張メソッドを呼び出す場合は、`https:` に URL プレフィックスを必ず含めます。</span><span class="sxs-lookup"><span data-stu-id="460e5-371">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="460e5-372">同じポート上で、HTTPS と HTTP をホストすることはできません。</span><span class="sxs-lookup"><span data-stu-id="460e5-372">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="460e5-373">ホスト フィルタリング</span><span class="sxs-lookup"><span data-stu-id="460e5-373">Host filtering</span></span>

<span data-ttu-id="460e5-374">Kestrel は `http://example.com:5000` などのプレフィックスに基づく構成をサポートしますが、Kestrel はほとんどのホスト名を無視します。</span><span class="sxs-lookup"><span data-stu-id="460e5-374">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="460e5-375">ホスト `localhost` は、ループバック アドレスへのバインドに使用される特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="460e5-375">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="460e5-376">明示的な IP アドレス以外のすべてのホストは、すべてのパブリック IP アドレスにバインドします。</span><span class="sxs-lookup"><span data-stu-id="460e5-376">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="460e5-377">この情報のいずれも、要求の `Host` ヘッダーの検証には使用されません。</span><span class="sxs-lookup"><span data-stu-id="460e5-377">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="460e5-378">これを解決するには、ホスト ヘッダー フィルタリングを使用するリバース プロキシの背後でホストします。</span><span class="sxs-lookup"><span data-stu-id="460e5-378">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="460e5-379">これは、ASP.NET Core 1.x の Kestrel に対してのみサポートされるシナリオです。</span><span class="sxs-lookup"><span data-stu-id="460e5-379">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="460e5-380">これを解決するには、ミドルウェアを使用して、`Host` ヘッダーによって要求をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="460e5-380">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

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

<span data-ttu-id="460e5-381">前述の `HostFilteringMiddleware` を `Startup.Configure` で登録します。</span><span class="sxs-lookup"><span data-stu-id="460e5-381">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="460e5-382">[ミドルウェアの登録の順序](xref:fundamentals/middleware/index#order)が重要であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="460e5-382">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#order) is important.</span></span> <span data-ttu-id="460e5-383">この登録は、診断ミドルウェア (`app.UseExceptionHandler` など) の登録の直後に行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-383">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="460e5-384">このミドルウェアには、*appsettings.json*/*appsettings.\<>.json* に、`AllowedHosts` キーを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-384">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="460e5-385">この値は、ポート番号を含まないホスト名のセミコロン区切りリストです。</span><span class="sxs-lookup"><span data-stu-id="460e5-385">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="460e5-386">これを解決するには、Host Filtering Middleware を使用します。</span><span class="sxs-lookup"><span data-stu-id="460e5-386">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="460e5-387">Host Filtering Middleware は、[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) に含まれる、[Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) パッケージによって提供されています。</span><span class="sxs-lookup"><span data-stu-id="460e5-387">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="460e5-388">このミドルウェアは、[AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering) を呼び出す、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) によって追加されています。</span><span class="sxs-lookup"><span data-stu-id="460e5-388">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="460e5-389">Host Filtering Middleware は既定では無効です。</span><span class="sxs-lookup"><span data-stu-id="460e5-389">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="460e5-390">このミドルウェアを有効にするには、*appsettings.json*/*appsettings.\<>.json* に、`AllowedHosts` キーを定義します。</span><span class="sxs-lookup"><span data-stu-id="460e5-390">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="460e5-391">この値は、ポート番号を含まないホスト名のセミコロン区切りリストです。</span><span class="sxs-lookup"><span data-stu-id="460e5-391">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="460e5-392">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="460e5-392">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="460e5-393">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) には、[ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) オプションもあります。</span><span class="sxs-lookup"><span data-stu-id="460e5-393">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="460e5-394">Forwarded Headers Middleware および Host Filtering Middleware には、異なるシナリオ用に類似した機能があります。</span><span class="sxs-lookup"><span data-stu-id="460e5-394">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="460e5-395">リバース プロキシ サーバーまたはロード バランサーを使用して要求を転送するとき、ホスト ヘッダーが保存されていない場合、Forwarded Headers Middleware に `AllowedHosts` を設定するのが適切です。</span><span class="sxs-lookup"><span data-stu-id="460e5-395">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="460e5-396">Kestrel がエッジ サーバーとして使用されていたり、ホスト ヘッダーが直接転送されたりしている場合、Host Filtering Middleware に `AllowedHosts` を設定するのが適切です。</span><span class="sxs-lookup"><span data-stu-id="460e5-396">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="460e5-397">Forwarded Headers Middleware の詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="460e5-397">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="460e5-398">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="460e5-398">Additional resources</span></span>

* [<span data-ttu-id="460e5-399">HTTPS の適用</span><span class="sxs-lookup"><span data-stu-id="460e5-399">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="460e5-400">Kestrel ソース コード</span><span class="sxs-lookup"><span data-stu-id="460e5-400">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* <span data-ttu-id="460e5-401">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)(RFC 7230: メッセージの構文と経路制御 (セクション 5.4: ホスト))</span><span class="sxs-lookup"><span data-stu-id="460e5-401">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)</span></span>
* [<span data-ttu-id="460e5-402">プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する</span><span class="sxs-lookup"><span data-stu-id="460e5-402">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
