---
title: IIS を使用した Windows での ASP.NET Core のホスト
author: guardrex
description: Windows Server インターネット インフォメーション サービス (IIS) での ASP.NET Core アプリをホストする方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 5408fb04231a61e0c4c7a91eb15196bf754ddfa7
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191374"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="54ca2-103">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="54ca2-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="54ca2-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="54ca2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="54ca2-105">.NET Core ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="54ca2-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="54ca2-106">サポートされるオペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="54ca2-106">Supported operating systems</span></span>

<span data-ttu-id="54ca2-107">次のオペレーティング システムがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="54ca2-108">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="54ca2-108">Windows 7 or later</span></span>
* <span data-ttu-id="54ca2-109">Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="54ca2-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="54ca2-110">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (以前の名称は [WebListener](xref:fundamentals/servers/weblistener)) が、IIS を含むリバース プロキシ構成で動作しません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="54ca2-111">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="54ca2-112">Azure でのホスティングの情報については、「<xref:host-and-deploy/azure-apps/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="http2-support"></a><span data-ttu-id="54ca2-113">HTTP/2 のサポート</span><span class="sxs-lookup"><span data-stu-id="54ca2-113">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="54ca2-114">[HTTP/2](https://httpwg.org/specs/rfc7540.html) は、次の IIS 展開シナリオでの ASP.NET Core でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-114">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="54ca2-115">インプロセス</span><span class="sxs-lookup"><span data-stu-id="54ca2-115">In-process</span></span>
  * <span data-ttu-id="54ca2-116">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="54ca2-116">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="54ca2-117">ターゲット フレームワーク: .NET Core 2.2 以降</span><span class="sxs-lookup"><span data-stu-id="54ca2-117">Target framework: .NET Core 2.2 or later</span></span>
  * <span data-ttu-id="54ca2-118">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="54ca2-118">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="54ca2-119">アウトプロセス</span><span class="sxs-lookup"><span data-stu-id="54ca2-119">Out-of-process</span></span>
  * <span data-ttu-id="54ca2-120">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="54ca2-120">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="54ca2-121">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、[Kestrel サーバー](xref:fundamentals/servers/kestrel)へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-121">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="54ca2-122">ターゲット フレームワーク: HTTP/2 接続は IIS によって完全に処理されるため、アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-122">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
  * <span data-ttu-id="54ca2-123">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="54ca2-123">TLS 1.2 or later connection</span></span>

<span data-ttu-id="54ca2-124">インプロセスの展開の場合、HTTP/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) によって `HTTP/2` が報告されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-124">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="54ca2-125">アウトプロセスの展開の場合、HTTP/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) によって `HTTP/1.1` が報告されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-125">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="54ca2-126">インプロセス ホスティング モデルとアウトプロセス ホスティング モデルの詳細については、<xref:fundamentals/servers/aspnet-core-module> トピックと <xref:host-and-deploy/aspnet-core-module> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-126">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="54ca2-127">次の基本要件を満たすアウトプロセスの展開には、[HTTP/2](https://httpwg.org/specs/rfc7540.html) がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-127">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="54ca2-128">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="54ca2-128">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="54ca2-129">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、[Kestrel サーバー](xref:fundamentals/servers/kestrel)へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-129">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="54ca2-130">ターゲット フレームワーク: HTTP/2 接続は IIS によって完全に処理されるため、アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-130">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="54ca2-131">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="54ca2-131">TLS 1.2 or later connection</span></span>

<span data-ttu-id="54ca2-132">Http/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) が `HTTP/1.1` を報告します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-132">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="54ca2-133">HTTP/2 は既定で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-133">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="54ca2-134">HTTP/2 接続が確立されない場合、接続は HTTP/1.1 にフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-134">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="54ca2-135">IIS 展開での HTTP/2 構成の詳細については、[IIS での HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-135">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="54ca2-136">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="54ca2-136">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="54ca2-137">IISIntegration コンポーネントを有効にする</span><span class="sxs-lookup"><span data-stu-id="54ca2-137">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="54ca2-138">**インプロセス ホスティング モデル**</span><span class="sxs-lookup"><span data-stu-id="54ca2-138">**In-process hosting model**</span></span>

<span data-ttu-id="54ca2-139">一般的な *Program.cs* では、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-139">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="54ca2-140">`CreateDefaultBuilder` では、`UseIIS` メソッドを呼び出し、[CoreCLR](/dotnet/standard/glossary#coreclr) を起動して IIS ワーカー プロセス (`w3wp.exe`) 内のアプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-140">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (`w3wp.exe`).</span></span> <span data-ttu-id="54ca2-141">パフォーマンス テストは、.NET Core アプリのインプロセス ホスティングでは、アプリのアウトプロセス ホスティングや [Kestrel](xref:fundamentals/servers/kestrel) へのプロキシ要求に比べ、スループットの要求がより高くなることを示しています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-141">Performance tests indicate that hosting a .NET Core app in-process delivers higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="54ca2-142">**アウトプロセス ホスティング モデル**</span><span class="sxs-lookup"><span data-stu-id="54ca2-142">**Out-of-process hosting model**</span></span>

<span data-ttu-id="54ca2-143">一般的な *Program.cs* では、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-143">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="54ca2-144">IIS でのアウトプロセス ホスティングの場合、`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-144">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="54ca2-145">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-145">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="54ca2-146">`CreateDefaultBuilder` では <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-146">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="54ca2-147">`UseIISIntegration` では、localhost の IP アドレス (`127.0.0.1`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-147">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="54ca2-148">動的なポートが 1234 である場合、Kestrel は `127.0.0.1:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-148">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="54ca2-149">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-149">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="54ca2-150">Kestrel の Listen API</span><span class="sxs-lookup"><span data-stu-id="54ca2-150">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="54ca2-151">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="54ca2-151">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="54ca2-152">モジュールを使用する場合、`UseUrls` または Kestrel の `Listen` API を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-152">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="54ca2-153">`UseUrls` または `Listen` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-153">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="54ca2-154">インプロセス ホスティング モデルとアウトプロセス ホスティング モデルの詳細については、<xref:fundamentals/servers/aspnet-core-module> トピックと <xref:host-and-deploy/aspnet-core-module> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-154">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="54ca2-155">一般的な *Program.cs* では、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-155">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="54ca2-156">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-156">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="54ca2-157">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-157">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="54ca2-158">`CreateDefaultBuilder` では [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-158">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="54ca2-159">`UseIISIntegration` では、localhost の IP アドレス (`127.0.0.1`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-159">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="54ca2-160">動的なポートが 1234 である場合、Kestrel は `127.0.0.1:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-160">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="54ca2-161">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-161">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="54ca2-162">Kestrel の Listen API</span><span class="sxs-lookup"><span data-stu-id="54ca2-162">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="54ca2-163">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="54ca2-163">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="54ca2-164">モジュールを使用する場合、`UseUrls` または Kestrel の `Listen` API を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-164">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="54ca2-165">`UseUrls` または `Listen` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-165">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="54ca2-166">一般的な *Program.cs* では、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-166">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host.</span></span> <span data-ttu-id="54ca2-167">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-167">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="54ca2-168">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-168">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="54ca2-169">`CreateDefaultBuilder` では [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-169">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="54ca2-170">`UseIISIntegration` では、localhost の IP アドレス (`localhost`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-170">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="54ca2-171">動的なポートが 1234 である場合、Kestrel は `localhost:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-171">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="54ca2-172">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-172">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="54ca2-173">Kestrel の Listen API</span><span class="sxs-lookup"><span data-stu-id="54ca2-173">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="54ca2-174">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="54ca2-174">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="54ca2-175">モジュールを使用する場合、`UseUrls` または Kestrel の `Listen` API を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-175">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="54ca2-176">`UseUrls` または `Listen` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-176">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="54ca2-177">アプリの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-177">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="54ca2-178">[UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 拡張メソッドを [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に追加して、IIS 統合ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-178">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="54ca2-179">[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) と [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) の両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="54ca2-179">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="54ca2-180">`UseIISIntegration` を呼び出すコードはコードの移植性に影響しません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-180">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="54ca2-181">アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-181">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="54ca2-182">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-182">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="54ca2-183">`UseIISIntegration` では、localhost の IP アドレス (`localhost`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-183">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="54ca2-184">動的なポートが 1234 である場合、Kestrel は `localhost:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-184">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="54ca2-185">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-185">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="54ca2-186">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="54ca2-186">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="54ca2-187">モジュールを使用する場合、`UseUrls` を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-187">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="54ca2-188">`UseUrls` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-188">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="54ca2-189">ASP.NET Core 1.0 アプリ内で `UseUrls` 呼び出される場合、モジュールに構成されたポートが上書きされないように、 **を呼び出す**前に`UseIISIntegration`呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-189">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="54ca2-190">この呼び出しの順序は ASP.NET Core 1.1 では必要ありません。モジュール設定は `UseUrls` をオーバーライドするからです。</span><span class="sxs-lookup"><span data-stu-id="54ca2-190">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="54ca2-191">ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/host/index)」(ASP.NET Core でのホスティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-191">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="54ca2-192">IIS のオプション</span><span class="sxs-lookup"><span data-stu-id="54ca2-192">IIS options</span></span>

<span data-ttu-id="54ca2-193">[IIS](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) のオプションを構成するには、IISOptions のサービス構成を [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) に含めます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-193">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="54ca2-194">次の例では、`HttpContext.Connection.ClientCertificate` を入力するためのクライアント証明書のアプリへの転送は無効になります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-194">In the following example, forwarding client certificates to the app to populate `HttpContext.Connection.ClientCertificate` is disabled:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="54ca2-195">オプション</span><span class="sxs-lookup"><span data-stu-id="54ca2-195">Option</span></span>                         | <span data-ttu-id="54ca2-196">既定値</span><span class="sxs-lookup"><span data-stu-id="54ca2-196">Default</span></span> | <span data-ttu-id="54ca2-197">設定</span><span class="sxs-lookup"><span data-stu-id="54ca2-197">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="54ca2-198">`true` の場合、IIS 統合ミドルウェアが、[Windows 認証](xref:security/authentication/windowsauth)によって `HttpContext.User` を設定します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-198">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="54ca2-199">`false` の場合、ミドルウェアは `HttpContext.User` の ID を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-199">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="54ca2-200">`AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-200">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="54ca2-201">詳細については、「[Windows 認証](xref:security/authentication/windowsauth)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-201">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="54ca2-202">ログイン ページでユーザーに表示名が表示されるように設定します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-202">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="54ca2-203">`true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-203">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="54ca2-204">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="54ca2-204">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="54ca2-205">要求が発生したスキーム (HTTP/HTTPS) とリモート IP アドレスを転送するために、Forwarded Headers Middleware を構成する IIS 統合ミドルウェアと、ASP.NET Core モジュールが構成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-205">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="54ca2-206">追加のプロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-206">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="54ca2-207">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-207">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="54ca2-208">web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="54ca2-208">web.config file</span></span>

<span data-ttu-id="54ca2-209">*web.config* ファイルでは、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を設定します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-209">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="54ca2-210">*web.config* ファイルの作成、変換、および公開は、プロジェクトの公開時に、MSBuild ターゲット (`_TransformWebConfig`) によって処理されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-210">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="54ca2-211">このターゲットは、Web SDK ターゲット (`Microsoft.NET.Sdk.Web`) に存在します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-211">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="54ca2-212">SDK は、プロジェクト ファイルの先頭で設定されています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-212">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="54ca2-213">プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して作成され、[発行された出力](xref:host-and-deploy/directory-structure)に移行されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-213">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="54ca2-214">プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは ASP.NET Core モジュールを構成するための正しい *processPath* と *arguments* を使用して作成され、発行された出力に移行されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-214">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="54ca2-215">変換によりファイル内の IIS 構成の設定が変わることはありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-215">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="54ca2-216">*web.config* ファイルは、アクティブな IIS モジュールを制御する追加の IIS 構成設定を提供する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-216">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="54ca2-217">ASP.NET Core アプリを使用して要求を処理できる IIS モジュールの詳細については、[IIS モジュール](xref:host-and-deploy/iis/modules)のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-217">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="54ca2-218">Web SDK によって *web.config* ファイルが変換されないようにするため、**\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルで使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-218">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="54ca2-219">Web SDK ファイルの変換を無効にすると、 *processPath*と*引数*開発者によって手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-219">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="54ca2-220">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-220">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="54ca2-221">web.config ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="54ca2-221">web.config file location</span></span>

<span data-ttu-id="54ca2-222">IIS と Kestrel サーバーの間にリバース プロキシを作成するには、展開されるアプリのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-222">In order to create the reverse proxy between IIS and the Kestrel server, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="54ca2-223">これは、IIS に提供される web サイトの物理パスと同じ場所です。</span><span class="sxs-lookup"><span data-stu-id="54ca2-223">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="54ca2-224">*Web.config* ファイルは、Web 配置を使用して複数のアプリの発行を有効にするため、アプリのルートで必要です。</span><span class="sxs-lookup"><span data-stu-id="54ca2-224">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="54ca2-225">アプリの物理パスには、*\<assembly>.runtimeconfig.json*、*\<assembly>.xml* (XML ドキュメントのコメント)、*\<assembly>.deps.json* などの機密性の高いファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-225">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="54ca2-226">*web.config* ファイルが存在し、サイトは通常どおり起動した場合、IIS は、これらの機密性の高いファイルが要求された場合にファイルを提供しません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-226">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="54ca2-227">*web.config*ファイルが存在しないか、不適切な名前が付けられているか、または通常の起動用にサイトを構成できない場合、IIS が機密性の高いファイルを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-227">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="54ca2-228">***web.config*ファイルは、展開環境に常に存在し、適切な名前が付けられ、通常の起動用にサイトを構成できる必要があります。*web.config* ファイルを運用環境の展開から削除しないでください。**</span><span class="sxs-lookup"><span data-stu-id="54ca2-228">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="54ca2-229">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="54ca2-229">IIS configuration</span></span>

<span data-ttu-id="54ca2-230">**Windows Server オペレーティング システム**</span><span class="sxs-lookup"><span data-stu-id="54ca2-230">**Windows Server operating systems**</span></span>

<span data-ttu-id="54ca2-231">**Web サーバー (IIS)** サーバーの役割を有効にし、役割のサービスを設定します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-231">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="54ca2-232">**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-232">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="54ca2-233">**[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-233">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="54ca2-235">**[機能]** のステップの後に、**[役割サービスの]** のステップで Web サーバー (IIS) を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-235">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="54ca2-236">希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-236">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![[役割サービスの選択] のステップで既定の役割サービスを選択します。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="54ca2-238">**Windows 認証 (任意)**</span><span class="sxs-lookup"><span data-stu-id="54ca2-238">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="54ca2-239">Windows 認証を有効にするには、**[Web サーバー]** > **[セキュリティ]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-239">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="54ca2-240">**[Windows 認証]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-240">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="54ca2-241">詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-241">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="54ca2-242">**Websocket (省略可能)**</span><span class="sxs-lookup"><span data-stu-id="54ca2-242">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="54ca2-243">WebSockets は、ASP.NET Core 1.1 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-243">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="54ca2-244">Websocket を有効にするには、**[Web サーバー]** > **[アプリケーション開発]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-244">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="54ca2-245">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-245">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="54ca2-246">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-246">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="54ca2-247">**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-247">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="54ca2-248">**Web サーバー (IIS)** の役割をインストールした後、サーバーと IIS の再起動は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-248">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="54ca2-249">**Windows デスクトップ オペレーティング システム**</span><span class="sxs-lookup"><span data-stu-id="54ca2-249">**Windows desktop operating systems**</span></span>

<span data-ttu-id="54ca2-250">**[IIS 管理コンソール]** と **[World Wide Web サービス]** を有効にします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-250">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="54ca2-251">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-251">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="54ca2-252">**インターネット インフォメーション サービス (IIS)** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-252">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="54ca2-253">**[Web 管理ツール]** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-253">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="54ca2-254">**[IIS 管理コンソール]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-254">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="54ca2-255">**[World Wide Web サービス]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-255">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="54ca2-256">**[World Wide Web サービス]** の既定の機能をそのまま使用するか、IIS 機能をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-256">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="54ca2-257">**Windows 認証 (任意)**</span><span class="sxs-lookup"><span data-stu-id="54ca2-257">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="54ca2-258">Windows 認証を有効にするには、**[World Wide Web サービス]** > **[セキュリティ]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-258">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="54ca2-259">**[Windows 認証]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-259">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="54ca2-260">詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-260">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="54ca2-261">**Websocket (省略可能)**</span><span class="sxs-lookup"><span data-stu-id="54ca2-261">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="54ca2-262">WebSockets は、ASP.NET Core 1.1 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-262">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="54ca2-263">Websocket を有効にするには、**[World Wide Web サービス]** > **[アプリケーション開発機能]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-263">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="54ca2-264">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-264">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="54ca2-265">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-265">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="54ca2-266">IIS のインストールに再起動が必要な場合は、システムを再起動します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-266">If the IIS installation requires a restart, restart the system.</span></span>

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="54ca2-268">.NET Core ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="54ca2-268">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="54ca2-269">ホスティング システムに *.NET Core ホスティング バンドル*をインストールします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-269">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="54ca2-270">このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-270">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="54ca2-271">このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-271">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="54ca2-272">システムにインターネット接続が設定されていない場合は、.NET Core ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-272">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54ca2-273">ホスティング バンドルが IIS の前にインストールされている場合、バンドルのインストールを修復する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-273">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="54ca2-274">IIS をインストールした後に、ホスティング バンドル インストーラーをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-274">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="54ca2-275">直接ダウンロード (現在のバージョン)</span><span class="sxs-lookup"><span data-stu-id="54ca2-275">Direct download (current version)</span></span>

<span data-ttu-id="54ca2-276">次のリンクを使用してインストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-276">Download the installer using the following link:</span></span>

[<span data-ttu-id="54ca2-277">現在の .NET Core ホスティング バンドルのインストーラー (直接ダウンロード)</span><span class="sxs-lookup"><span data-stu-id="54ca2-277">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="54ca2-278">以前のバージョンのインストーラー</span><span class="sxs-lookup"><span data-stu-id="54ca2-278">Earlier versions of the installer</span></span>

<span data-ttu-id="54ca2-279">以前のバージョンのインストーラーを入手するには:</span><span class="sxs-lookup"><span data-stu-id="54ca2-279">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="54ca2-280">[.NET のダウンロードのアーカイブ](https://www.microsoft.com/net/download/archives)に移動します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-280">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="54ca2-281">**[.NET Core]** の下で、.NET Core のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-281">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="54ca2-282">**[Run apps - Runtime]\(アプリの実行 - ランタイム\)** 列から、目的の .NET Core ランタイム バージョンの行を探します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-282">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="54ca2-283">**[Runtime & Hosting Bundle]\(ランタイム & ホスティング バンドル\)** のリンクを使用してインストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-283">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="54ca2-284">一部のインストーラーには、有効期限 (EOL) に達して Microsoft によるサポートが終了した、リリース バージョンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-284">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="54ca2-285">詳細については、[サポート ポリシー](https://www.microsoft.com/net/download/dotnet-core/2.0)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-285">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="54ca2-286">ホスティング バンドルをインストールする</span><span class="sxs-lookup"><span data-stu-id="54ca2-286">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="54ca2-287">サーバーでインストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-287">Run the installer on the server.</span></span> <span data-ttu-id="54ca2-288">管理者のコマンド プロンプトからインストーラーを実行する場合、次のスイッチを使用できます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-288">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="54ca2-289">`OPT_NO_ANCM=1` &ndash; ASP.NET Core モジュールのインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-289">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="54ca2-290">`OPT_NO_RUNTIME=1` &ndash; .NET Core ランタイムのインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-290">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="54ca2-291">`OPT_NO_SHAREDFX=1` &ndash; ASP.NET Shared Framework (ASP.NET ランタイム) のインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-291">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="54ca2-292">`OPT_NO_X86=1` &ndash; x86 ランタイムのインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-292">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="54ca2-293">32 ビット アプリをホストしない場合は、このスイッチを使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-293">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="54ca2-294">今後、32 ビット アプリと 64 ビット アプリの両方をホストする可能性がある場合は、このスイッチを使用せずに、両方のランタイムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-294">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="54ca2-295">システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-295">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="54ca2-296">IIS を再起動すると、インストーラーによって行われたシステム パスへの変更 (環境変数) が取得されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-296">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="54ca2-297">Windows ホスティング バンドルのインストーラーでインストールを完了するために、IIS によるリセットが必要であることを検知した場合、インストーラーによって IIS がリセットされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-297">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="54ca2-298">インストーラーで IIS のリセットがトリガーされた場合、IIS アプリ プールと Web サイトがすべて再起動されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-298">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="54ca2-299">IIS 共有構成の詳細については、「[ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-299">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="54ca2-300">Visual Studio で発行する場合の Web 配置のインストール</span><span class="sxs-lookup"><span data-stu-id="54ca2-300">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="54ca2-301">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してサーバーにアプリを展開する場合、Web 配置の最新バージョンをサーバーにインストールします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-301">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="54ca2-302">Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-302">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="54ca2-303">WebPI を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-303">The preferred method is to use WebPI.</span></span> <span data-ttu-id="54ca2-304">WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-304">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="54ca2-305">IIS サイトを作成する</span><span class="sxs-lookup"><span data-stu-id="54ca2-305">Create the IIS site</span></span>

1. <span data-ttu-id="54ca2-306">ホスト システムで、アプリの公開フォルダーとファイルを格納するフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-306">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="54ca2-307">アプリの展開レイアウトについては、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-307">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="54ca2-308">新しいフォルダー内で、stdout ログが有効になっている場合に ASP.NET Core Module stdout ログを保持するための *logs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-308">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="54ca2-309">ペイロードに *logs* フォルダーを含めてアプリを配置する場合は、この手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-309">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="54ca2-310">プロジェクトがローカルでビルドされるときに、MSBuild を有効にして、*logs* フォルダーを自動的に作成する手順については、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-310">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="54ca2-311">stdout ログは、アプリケーションの起動エラーのトラブルシューティングにのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-311">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="54ca2-312">日常的なアプリのログ記録に、stdout ログを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-312">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="54ca2-313">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-313">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="54ca2-314">アプリケーション プールは、ログが書き込まれる場所への書き込みアクセス権を持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-314">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="54ca2-315">ログの場所へのパス上のフォルダーがすべて存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-315">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="54ca2-316">stdout ログの詳細については、「[Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)」(ログの作成とリダイレクト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-316">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="54ca2-317">ASP.NET Core アプリケーションのログ記録の詳細については、「[ログ](xref:fundamentals/logging/index)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-317">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="54ca2-318">**IIS マネージャー**の **[接続]** パネルでサーバーのノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-318">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="54ca2-319">**[サイト]** フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-319">Right-click the **Sites** folder.</span></span> <span data-ttu-id="54ca2-320">コンテキスト メニューで **[Web サイトの追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-320">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="54ca2-321">**[サイト名]** を指定し、**[物理パス]** にはアプリの配置フォルダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-321">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="54ca2-322">**[バインド]** の構成を指定して **[OK]** を選択し、Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-322">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="54ca2-324">最上位のワイルドカードのバインド ( `http://*:80/` と `http://+:80` ) は使用しては **いけません** 。</span><span class="sxs-lookup"><span data-stu-id="54ca2-324">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="54ca2-325">最上位のワイルドカードのバインドは、セキュリティの脆弱性に対してアプリを切り開くことができます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-325">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="54ca2-326">これは、強力と脆弱の両方のワイルドカードに適用されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-326">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="54ca2-327">ワイルドカードではなく、明示的なホスト名を使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-327">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="54ca2-328">全体の親ドメインを制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.mysub.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="54ca2-328">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="54ca2-329">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-329">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="54ca2-330">サーバーのノードでは、**[アプリケーション プール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-330">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="54ca2-331">サイトのアプリケーション プールを右クリックし、コンテキスト メニューから **[基本設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-331">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="54ca2-332">**[アプリケーション プールの編集]** ウィンドウで、**[.NET CLR バージョン]** を **[マネージド コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-332">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![.NET CLR バージョンとして [マネージド コードなし] を設定します。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="54ca2-334">ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-334">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="54ca2-335">ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-335">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="54ca2-336">**[.NET CLR バージョン]** の **[マネージド コードなし]** への設定は、任意です。</span><span class="sxs-lookup"><span data-stu-id="54ca2-336">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="54ca2-337">プロセス モデル ID に適切なアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-337">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="54ca2-338">アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-338">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="54ca2-339">たとえば、アプリケーション プールには、アプリがファイルの読み取りおよび書き込みを行うフォルダーへの読み取りおよび書き込みアクセスが必要です。</span><span class="sxs-lookup"><span data-stu-id="54ca2-339">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="54ca2-340">**Windows 認証の構成 (任意)**</span><span class="sxs-lookup"><span data-stu-id="54ca2-340">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="54ca2-341">詳細については、「[Windows 認証を構成する](xref:security/authentication/windowsauth)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-341">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="54ca2-342">アプリを配置する</span><span class="sxs-lookup"><span data-stu-id="54ca2-342">Deploy the app</span></span>

<span data-ttu-id="54ca2-343">ホスト システム上に作成したフォルダーにアプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-343">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="54ca2-344">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="54ca2-344">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="54ca2-345">Visual Studio からのアプリの展開</span><span class="sxs-lookup"><span data-stu-id="54ca2-345">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="54ca2-346">Web 配置に使用する発行プロファイルの作成方法については、「[Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)」 (ASP.NET Core アプリ展開用の Visual Studio の発行プロファイル) のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-346">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="54ca2-347">ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-347">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[発行] ダイアログ ページ](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="54ca2-349">Visual Studio 外部での Web 配置</span><span class="sxs-lookup"><span data-stu-id="54ca2-349">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="54ca2-350">Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-350">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="54ca2-351">詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-351">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="54ca2-352">Web 配置の代替手段</span><span class="sxs-lookup"><span data-stu-id="54ca2-352">Alternatives to Web Deploy</span></span>

<span data-ttu-id="54ca2-353">手動コピー、Xcopy、Robocopy、PowerShell などの任意の方法を使用して、ホスティング システムにアプリを移動します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-353">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="54ca2-354">IIS への ASP.NET Core の展開の詳細については、「[IIS 管理者用の展開リソース](#deployment-resources-for-iis-administrators)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-354">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="54ca2-355">Web サイトを閲覧する</span><span class="sxs-lookup"><span data-stu-id="54ca2-355">Browse the website</span></span>

![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="54ca2-357">ロックされた展開ファイル</span><span class="sxs-lookup"><span data-stu-id="54ca2-357">Locked deployment files</span></span>

<span data-ttu-id="54ca2-358">アプリが実行中は、展開フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-358">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="54ca2-359">展開中にロックされたファイルを上書きすることはできません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-359">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="54ca2-360">展開でロックされているファイルをリリースするには、次の**いずれか**の方法を使用してアプリ プールを停止します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-360">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="54ca2-361">Web 配置を使用し、プロジェクト ファイル内の `Microsoft.NET.Sdk.Web` を参照して停止します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-361">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="54ca2-362">*app_offline.htm* ファイルは、Web アプリのディレクトリのルートに配置されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-362">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="54ca2-363">ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-363">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="54ca2-364">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-364">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="54ca2-365">サーバー上の IIS マネージャーで手動によりアプリ プールを停止します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-365">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="54ca2-366">PowerShell を使用して、アプリ プールを停止して再起動します (PowerShell 5 以降が必要)。</span><span class="sxs-lookup"><span data-stu-id="54ca2-366">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a><span data-ttu-id="54ca2-367">データの保護</span><span class="sxs-lookup"><span data-stu-id="54ca2-367">Data protection</span></span>

<span data-ttu-id="54ca2-368">[ASP.NET Core データ保護スタック](xref:security/data-protection/introduction)は、認証で使用されるミドルウェアを含め、いくつかの [ASP.NET Core](xref:fundamentals/middleware/index) ミドルウェアによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-368">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="54ca2-369">データ保護 API がユーザーのコードから呼び出されない場合でも、配置スクリプトを使用するかユーザー コードで、永続的な暗号化[キー ストア](xref:security/data-protection/implementation/key-management)を作成するようにデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-369">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="54ca2-370">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-370">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="54ca2-371">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-371">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="54ca2-372">すべての cookie ベースの認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-372">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="54ca2-373">ユーザーは、次回の要求時に再度サインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-373">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="54ca2-374">キーリングで保護されているデータは、いずれも復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-374">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="54ca2-375">これには、[CSRF トークン](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)と [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-375">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="54ca2-376">キーリングを保持するために IIS でのデータ保護を構成するには、次の**いずれか**の方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-376">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="54ca2-377">**データ保護のレジストリ キーを作成する**</span><span class="sxs-lookup"><span data-stu-id="54ca2-377">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="54ca2-378">ASP.NET Core アプリで使用されるデータ保護キーは、アプリ外部のレジストリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-378">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="54ca2-379">特定のアプリのキーを保持するには、アプリ プール用のレジストリ キーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-379">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="54ca2-380">非 Web ファーム IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-380">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="54ca2-381">このスクリプトは、HKLM レジストリにレジストリ キーを作成します。そのキーは、アプリのアプリ プールのワーカー プロセス アカウントのみがアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-381">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="54ca2-382">キーは保存時に、DPAPI を使用して、コンピューター全体に適用するキーによって暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-382">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="54ca2-383">Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-383">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="54ca2-384">既定では、データ保護キーは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-384">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="54ca2-385">ネットワーク共有のファイルのアクセス許可が、アプリを実行する Windows アカウントに限定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-385">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="54ca2-386">保存時のキーを保護するために、X509 証明書を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-386">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="54ca2-387">ユーザーが証明書をアップデートできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-387">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="54ca2-388">詳細については、「[ASP.NET Core データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-388">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="54ca2-389">**ユーザー プロファイルを読み込むための IIS アプリケーション プールを構成する**</span><span class="sxs-lookup"><span data-stu-id="54ca2-389">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="54ca2-390">この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-390">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="54ca2-391">[ユーザー プロファイルの読み込み] を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-391">Set Load User Profile to `True`.</span></span> <span data-ttu-id="54ca2-392">これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-392">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="54ca2-393">**ファイル システムをキー リング ストアとして使用する**</span><span class="sxs-lookup"><span data-stu-id="54ca2-393">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="54ca2-394">[ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-394">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="54ca2-395">X509 証明書を使用してキー リングを保護し、その証明書が信頼された証明書であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-395">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="54ca2-396">証明書が自己署名されている場合は、信頼されたルート ストアにその証明書を配置します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-396">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="54ca2-397">Web ファームで IIS を使用する場合:</span><span class="sxs-lookup"><span data-stu-id="54ca2-397">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="54ca2-398">すべてのコンピューターがアクセスできるファイル共有を使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-398">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="54ca2-399">X509 証明書を各コンピューターに配置します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-399">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="54ca2-400">[コード内にデータ保護](xref:security/data-protection/configuration/overview)を構成します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-400">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="54ca2-401">**コンピューター全体に適用するデータ保護ポリシーを設定する**</span><span class="sxs-lookup"><span data-stu-id="54ca2-401">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="54ca2-402">データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。</span><span class="sxs-lookup"><span data-stu-id="54ca2-402">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="54ca2-403">詳細については、「<xref:security/data-protection/introduction>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-403">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="54ca2-404">サブアプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="54ca2-404">Sub-application configuration</span></span>

<span data-ttu-id="54ca2-405">ルート アプリの下に追加したサブアプリに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-405">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="54ca2-406">このモジュールをサブアプリの *web.config* ファイルにハンドラーとして追加すると、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する *500.19 内部サーバー エラー* が返されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-406">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="54ca2-407">次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルを示しています。</span><span class="sxs-lookup"><span data-stu-id="54ca2-407">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="54ca2-408">ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホストする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-408">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="54ca2-409">ASP.NET Core モジュールを構成する方法の詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-409">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="54ca2-410">web.config による IIS の構成</span><span class="sxs-lookup"><span data-stu-id="54ca2-410">Configuration of IIS with web.config</span></span>

<span data-ttu-id="54ca2-411">IIS の構成は、リバース プロキシ構成に適用されるこれらの IIS 機能の *web.config*の **\<system.webServer>** セクションの影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-411">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="54ca2-412">IIS が動的な圧縮を使用するようにサーバー レベルで構成されている場合、アプリの *web.config* ファイルで **\<urlCompression>** 要素を使用して無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-412">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="54ca2-413">詳細については、[\<system.webServer> の構成リファレンス](/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」および「[IIS モジュールと ASP.NET Core](xref:host-and-deploy/iis/modules)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-413">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="54ca2-414">分離されたアプリ プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定するには、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-414">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="54ca2-415">web.config の構成のセクション</span><span class="sxs-lookup"><span data-stu-id="54ca2-415">Configuration sections of web.config</span></span>

<span data-ttu-id="54ca2-416">*web.config* の ASP.NET 4.x アプリの構成セクションは、ASP.NET Core アプリの構成では使用されません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-416">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="54ca2-417">**\<system.web >**</span><span class="sxs-lookup"><span data-stu-id="54ca2-417">**\<system.web>**</span></span>
* <span data-ttu-id="54ca2-418">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="54ca2-418">**\<appSettings>**</span></span>
* <span data-ttu-id="54ca2-419">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="54ca2-419">**\<connectionStrings>**</span></span>
* <span data-ttu-id="54ca2-420">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="54ca2-420">**\<location>**</span></span>

<span data-ttu-id="54ca2-421">ASP.NET Core アプリは、他の構成プロバイダーを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-421">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="54ca2-422">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-422">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="54ca2-423">アプリケーション プール</span><span class="sxs-lookup"><span data-stu-id="54ca2-423">Application Pools</span></span>

<span data-ttu-id="54ca2-424">サーバーで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリ プールで実行して、アプリを相互に分離することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-424">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="54ca2-425">IIS の **[Web サイトの追加]** ダイアログはこの構成の既定です。</span><span class="sxs-lookup"><span data-stu-id="54ca2-425">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="54ca2-426">**[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-426">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="54ca2-427">サイトが追加されるときに、そのサイト名を使用して新しいアプリ プールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-427">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="54ca2-428">アプリケーション プール ID</span><span class="sxs-lookup"><span data-stu-id="54ca2-428">Application Pool Identity</span></span>

<span data-ttu-id="54ca2-429">アプリ プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリを実行できます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-429">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="54ca2-430">IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリ プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-430">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="54ca2-431">IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-431">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![アプリケーション プールの [詳細設定] ダイアログ](index/_static/apppool-identity.png)

<span data-ttu-id="54ca2-433">IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-433">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="54ca2-434">この ID を使用してリソースを保護することができます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-434">Resources can be secured using this identity.</span></span> <span data-ttu-id="54ca2-435">ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-435">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="54ca2-436">アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリを含むディレクトリのアクセス制御リスト (ACL) を変更します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-436">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="54ca2-437">エクスプローラーを開き、そのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-437">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="54ca2-438">そのディレクトリを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-438">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="54ca2-439">**[セキュリティ]** タブの **[編集]** ボタンを選択し、**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="54ca2-439">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="54ca2-440">**[場所]** ボタンを選択し、システムが選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-440">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="54ca2-441">**[選択するオブジェクト名を入力してください]** 領域に、「**IIS AppPool\\<app_pool_name>**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-441">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="54ca2-442">**[名前の確認]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-442">Select the **Check Names** button.</span></span> <span data-ttu-id="54ca2-443">*DefaultAppPool* で、**IIS AppPool\DefaultAppPool** を使用して名前を確認します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-443">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="54ca2-444">**[名前の確認]** ボタンが選択されているときには、**DefaultAppPool** の値が、オブジェクト名領域に表示されます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-444">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="54ca2-445">オブジェクト名の領域に直接アプリケーション プール名を入力することはできません。</span><span class="sxs-lookup"><span data-stu-id="54ca2-445">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="54ca2-446">オブジェクト名を確認するときには、**IIS AppPool\\<app_pool_name>** の形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-446">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択する前に、オブジェクト名領域で "DefaultAppPool" のアプリ名が "IIS AppPool\" に適用されます。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="54ca2-448">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-448">Select **OK**.</span></span>

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択した後に、オブジェクト名 "DefaultAppPool" がオブジェクト名領域に表示されます。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="54ca2-450">読み取り &amp; 実行アクセス許可は、既定で付与される必要があります。</span><span class="sxs-lookup"><span data-stu-id="54ca2-450">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="54ca2-451">必要に応じて、追加のアクセス許可を提供します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-451">Provide additional permissions as needed.</span></span>

<span data-ttu-id="54ca2-452">**ICACLS** ツールを使用してコマンド プロンプトでアクセス許可を付与することもできます。</span><span class="sxs-lookup"><span data-stu-id="54ca2-452">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="54ca2-453">たとえば、*DefaultAppPool* を使用する場合、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-453">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="54ca2-454">詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="54ca2-454">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="54ca2-455">IIS 管理者用の展開リソース</span><span class="sxs-lookup"><span data-stu-id="54ca2-455">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="54ca2-456">IIS ドキュメントでの IIS の詳細について説明します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-456">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="54ca2-457">IIS ドキュメント </span><span class="sxs-lookup"><span data-stu-id="54ca2-457">IIS documentation</span></span>](/iis)

<span data-ttu-id="54ca2-458">.NET Core アプリの展開モデルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-458">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="54ca2-459">.NET Core アプリケーションの展開</span><span class="sxs-lookup"><span data-stu-id="54ca2-459">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="54ca2-460">Kestrel Web サーバーが IIS または IIS Express をリバース プロキシ サーバーとして使用できるようにするための ASP.NET Core モジュールについて説明します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-460">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="54ca2-461">ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="54ca2-461">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="54ca2-462">ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-462">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="54ca2-463">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="54ca2-463">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="54ca2-464">発行された ASP.NET Core アプリのディレクトリ構造について説明します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-464">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="54ca2-465">ディレクトリの構造</span><span class="sxs-lookup"><span data-stu-id="54ca2-465">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="54ca2-466">ASP.NET Core アプリに対してアクティブおよび非アクティブな IIS モジュールについて、さらに IIS モジュールの管理方法についてを把握します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-466">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="54ca2-467">IIS モジュール</span><span class="sxs-lookup"><span data-stu-id="54ca2-467">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="54ca2-468">ASP.NET Core アプリの IIS 展開に関する問題を診断する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-468">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="54ca2-469">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="54ca2-469">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="54ca2-470">IIS で ASP.NET Core アプリをホストする場合の一般的なエラーを識別します。</span><span class="sxs-lookup"><span data-stu-id="54ca2-470">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="54ca2-471">Azure App Service と IIS の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="54ca2-471">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="54ca2-472">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="54ca2-472">Additional resources</span></span>

* [<span data-ttu-id="54ca2-473">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="54ca2-473">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="54ca2-474">Microsoft IIS 公式サイト</span><span class="sxs-lookup"><span data-stu-id="54ca2-474">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="54ca2-475">Windows Server テクニカル コンテンツ ライブラリ</span><span class="sxs-lookup"><span data-stu-id="54ca2-475">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="54ca2-476">IIS での HTTP/2</span><span class="sxs-lookup"><span data-stu-id="54ca2-476">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
