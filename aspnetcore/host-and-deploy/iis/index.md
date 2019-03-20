---
title: IIS を使用した Windows での ASP.NET Core のホスト
author: guardrex
description: Windows Server インターネット インフォメーション サービス (IIS) での ASP.NET Core アプリをホストする方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2019
uid: host-and-deploy/iis/index
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="762d6-103">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="762d6-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="762d6-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="762d6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="762d6-105">.NET Core ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="762d6-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="762d6-106">サポートされるオペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="762d6-106">Supported operating systems</span></span>

<span data-ttu-id="762d6-107">次のオペレーティング システムがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="762d6-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="762d6-108">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="762d6-108">Windows 7 or later</span></span>
* <span data-ttu-id="762d6-109">Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="762d6-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="762d6-110">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (旧称 WebListener) は、IIS が含まれるリバース プロキシ構成では動作しません。</span><span class="sxs-lookup"><span data-stu-id="762d6-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="762d6-111">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="762d6-112">Azure でのホスティングの情報については、「<xref:host-and-deploy/azure-apps/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="762d6-113">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="762d6-113">Supported platforms</span></span>

<span data-ttu-id="762d6-114">32 ビット (x86) および 64 ビット (x64) デプロイ用に発行されるアプリがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="762d6-114">Apps published for 32-bit (x86) and 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="762d6-115">アプリが以下の場合を除き、32 ビット アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="762d6-115">Deploy a 32-bit app unless the app:</span></span>

* <span data-ttu-id="762d6-116">64 ビット アプリ用の大規模な仮想メモリ アドレス空間が必要。</span><span class="sxs-lookup"><span data-stu-id="762d6-116">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="762d6-117">大規模な IIS スタック サイズが必要。</span><span class="sxs-lookup"><span data-stu-id="762d6-117">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="762d6-118">64 ビットのネイティブの依存関係がある。</span><span class="sxs-lookup"><span data-stu-id="762d6-118">Has 64-bit native dependencies.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="762d6-119">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="762d6-119">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="762d6-120">IISIntegration コンポーネントを有効にする</span><span class="sxs-lookup"><span data-stu-id="762d6-120">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="762d6-121">一般的な *Program.cs* では、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="762d6-121">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="762d6-122">一般的な *Program.cs* では、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="762d6-122">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="762d6-123">**インプロセス ホスティング モデル**</span><span class="sxs-lookup"><span data-stu-id="762d6-123">**In-process hosting model**</span></span>

<span data-ttu-id="762d6-124">`CreateDefaultBuilder` では、`UseIIS` メソッドを呼び出し、[CoreCLR](/dotnet/standard/glossary#coreclr) を起動して IIS ワーカー プロセス (*w3wp.exe* または *iisexpress.exe*) 内のアプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="762d6-124">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="762d6-125">パフォーマンス テストは、.NET Core アプリのインプロセス ホスティングでは、アプリのアウトプロセス ホスティングや [Kestrel](xref:fundamentals/servers/kestrel) サーバーへのプロキシ要求に比べ、スループットの要求が大幅に高くなることを示しています。</span><span class="sxs-lookup"><span data-stu-id="762d6-125">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="762d6-126">.NET Framework をターゲットにする ASP.NET Core アプリではインプロセス ホスティング モデルはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="762d6-126">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="762d6-127">**アウトプロセス ホスティング モデル**</span><span class="sxs-lookup"><span data-stu-id="762d6-127">**Out-of-process hosting model**</span></span>

<span data-ttu-id="762d6-128">IIS でのアウトプロセス ホスティングの場合、`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) サーバーを Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="762d6-128">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="762d6-129">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-129">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="762d6-130">`CreateDefaultBuilder` では <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-130">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="762d6-131">`UseIISIntegration` では、localhost の IP アドレス (`127.0.0.1`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-131">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="762d6-132">動的なポートが 1234 である場合、Kestrel は `127.0.0.1:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="762d6-132">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="762d6-133">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="762d6-133">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="762d6-134">Kestrel の Listen API</span><span class="sxs-lookup"><span data-stu-id="762d6-134">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="762d6-135">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="762d6-135">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="762d6-136">モジュールを使用する場合、`UseUrls` または Kestrel の `Listen` API を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="762d6-136">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="762d6-137">`UseUrls` または `Listen` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-137">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="762d6-138">インプロセスおよびアウトプロセス ホスティング モデルの詳細については、「[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)」および「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-138">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="762d6-139">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) サーバーを Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="762d6-139">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="762d6-140">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-140">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="762d6-141">`CreateDefaultBuilder` では <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-141">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="762d6-142">`UseIISIntegration` では、localhost の IP アドレス (`127.0.0.1`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-142">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="762d6-143">動的なポートが 1234 である場合、Kestrel は `127.0.0.1:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="762d6-143">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="762d6-144">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="762d6-144">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="762d6-145">Kestrel の Listen API</span><span class="sxs-lookup"><span data-stu-id="762d6-145">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="762d6-146">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="762d6-146">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="762d6-147">モジュールを使用する場合、`UseUrls` または Kestrel の `Listen` API を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="762d6-147">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="762d6-148">`UseUrls` または `Listen` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-148">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="762d6-149">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) サーバーを Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="762d6-149">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="762d6-150">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-150">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="762d6-151">`CreateDefaultBuilder` では <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-151">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="762d6-152">`UseIISIntegration` では、localhost の IP アドレス (`localhost`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-152">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="762d6-153">動的なポートが 1234 である場合、Kestrel は `localhost:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="762d6-153">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="762d6-154">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="762d6-154">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="762d6-155">Kestrel の Listen API</span><span class="sxs-lookup"><span data-stu-id="762d6-155">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="762d6-156">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="762d6-156">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="762d6-157">モジュールを使用する場合、`UseUrls` または Kestrel の `Listen` API を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="762d6-157">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="762d6-158">`UseUrls` または `Listen` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-158">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="762d6-159">アプリの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。</span><span class="sxs-lookup"><span data-stu-id="762d6-159">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="762d6-160"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> 拡張メソッドを <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> に追加して、IIS 統合ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-160">Use IIS Integration middleware by adding the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension method to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="762d6-161"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> と <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> の両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="762d6-161">Both <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> and <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> are required.</span></span> <span data-ttu-id="762d6-162">`UseIISIntegration` を呼び出すコードはコードの移植性に影響しません。</span><span class="sxs-lookup"><span data-stu-id="762d6-162">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="762d6-163">アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。</span><span class="sxs-lookup"><span data-stu-id="762d6-163">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="762d6-164">ASP.NET Core モジュールでは、バックエンド プロセスに割り当てる動的なポートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-164">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="762d6-165">`UseIISIntegration` では、localhost の IP アドレス (`localhost`) で動的なポートをリッスンするように Kestrel が構成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-165">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="762d6-166">動的なポートが 1234 である場合、Kestrel は `localhost:1234` でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="762d6-166">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="762d6-167">この構成によって、以下から提供されるその他の URL 構成が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="762d6-167">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="762d6-168">[構成](xref:fundamentals/configuration/index) (または[コマンド ラインの --urls オプション](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="762d6-168">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="762d6-169">モジュールを使用する場合、`UseUrls` を呼び出す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="762d6-169">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="762d6-170">`UseUrls` が呼び出されると、IIS なしでアプリを実行しているときのみ、Kestrel で指定したポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-170">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="762d6-171">ASP.NET Core 1.0 アプリ内で `UseUrls` 呼び出される場合、モジュールに構成されたポートが上書きされないように、 **を呼び出す**前に`UseIISIntegration`呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-171">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="762d6-172">この呼び出しの順序は ASP.NET Core 1.1 では必要ありません。モジュール設定は `UseUrls` をオーバーライドするからです。</span><span class="sxs-lookup"><span data-stu-id="762d6-172">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="762d6-173">ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/index#host)」(ASP.NET Core でのホスティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-173">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

### <a name="iis-options"></a><span data-ttu-id="762d6-174">IIS のオプション</span><span class="sxs-lookup"><span data-stu-id="762d6-174">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="762d6-175">**インプロセス ホスティング モデル**</span><span class="sxs-lookup"><span data-stu-id="762d6-175">**In-process hosting model**</span></span>

<span data-ttu-id="762d6-176">IIS サーバーのオプションを構成するには、<xref:Microsoft.AspNetCore.Builder.IISServerOptions> 用のサービス構成を <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> に含めます。</span><span class="sxs-lookup"><span data-stu-id="762d6-176">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="762d6-177">次の例では、AutomaticAuthentication が無効になります。</span><span class="sxs-lookup"><span data-stu-id="762d6-177">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="762d6-178">オプション</span><span class="sxs-lookup"><span data-stu-id="762d6-178">Option</span></span>                         | <span data-ttu-id="762d6-179">既定値</span><span class="sxs-lookup"><span data-stu-id="762d6-179">Default</span></span> | <span data-ttu-id="762d6-180">設定</span><span class="sxs-lookup"><span data-stu-id="762d6-180">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="762d6-181">`true` の場合、IIS サーバーが [Windows 認証](xref:security/authentication/windowsauth)によって認証された `HttpContext.User` を設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-181">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="762d6-182">`false` の場合、サーバーは `HttpContext.User` の ID を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときにチャレンジに応答します。</span><span class="sxs-lookup"><span data-stu-id="762d6-182">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="762d6-183">`AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-183">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="762d6-184">詳細については、[Windows 認証](xref:security/authentication/windowsauth)に関する記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-184">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="762d6-185">ログイン ページでユーザーに表示名が表示されるように設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-185">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="762d6-186">**アウトプロセス ホスティング モデル**</span><span class="sxs-lookup"><span data-stu-id="762d6-186">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="762d6-187">IIS オプションを構成するには、<xref:Microsoft.AspNetCore.Builder.IISOptions> 用のサービス構成を <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> に含めます。</span><span class="sxs-lookup"><span data-stu-id="762d6-187">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="762d6-188">次の例では、アプリが `HttpContext.Connection.ClientCertificate` を設定できません。</span><span class="sxs-lookup"><span data-stu-id="762d6-188">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="762d6-189">オプション</span><span class="sxs-lookup"><span data-stu-id="762d6-189">Option</span></span>                         | <span data-ttu-id="762d6-190">既定値</span><span class="sxs-lookup"><span data-stu-id="762d6-190">Default</span></span> | <span data-ttu-id="762d6-191">設定</span><span class="sxs-lookup"><span data-stu-id="762d6-191">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="762d6-192">`true` の場合、IIS 統合ミドルウェアが、[Windows 認証](xref:security/authentication/windowsauth)によって `HttpContext.User` を設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-192">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="762d6-193">`false` の場合、ミドルウェアは `HttpContext.User` の ID を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="762d6-193">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="762d6-194">`AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-194">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="762d6-195">詳細については、「[Windows 認証](xref:security/authentication/windowsauth)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-195">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="762d6-196">ログイン ページでユーザーに表示名が表示されるように設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-196">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="762d6-197">`true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-197">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="762d6-198">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="762d6-198">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="762d6-199">要求が発生したスキーム (HTTP/HTTPS) とリモート IP アドレスを転送するために、Forwarded Headers Middleware を構成する IIS 統合ミドルウェアと、ASP.NET Core モジュールが構成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-199">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="762d6-200">追加のプロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-200">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="762d6-201">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-201">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="762d6-202">web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="762d6-202">web.config file</span></span>

<span data-ttu-id="762d6-203">*web.config* ファイルでは、[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)を設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-203">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="762d6-204">*web.config* ファイルの作成、変換、および公開は、プロジェクトの公開時に、MSBuild ターゲット (`_TransformWebConfig`) によって処理されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-204">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="762d6-205">このターゲットは、Web SDK ターゲット (`Microsoft.NET.Sdk.Web`) に存在します。</span><span class="sxs-lookup"><span data-stu-id="762d6-205">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="762d6-206">SDK は、プロジェクト ファイルの先頭で設定されています。</span><span class="sxs-lookup"><span data-stu-id="762d6-206">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="762d6-207">プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して作成され、[発行された出力](xref:host-and-deploy/directory-structure)に移行されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-207">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="762d6-208">プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは ASP.NET Core モジュールを構成するための正しい *processPath* と *arguments* を使用して作成され、発行された出力に移行されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-208">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="762d6-209">変換によりファイル内の IIS 構成の設定が変わることはありません。</span><span class="sxs-lookup"><span data-stu-id="762d6-209">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="762d6-210">*web.config* ファイルは、アクティブな IIS モジュールを制御する追加の IIS 構成設定を提供する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-210">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="762d6-211">ASP.NET Core アプリを使用して要求を処理できる IIS モジュールの詳細については、[IIS モジュール](xref:host-and-deploy/iis/modules)のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-211">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="762d6-212">Web SDK によって *web.config* ファイルが変換されないようにするため、**\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルで使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-212">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="762d6-213">Web SDK ファイルの変換を無効にすると、 *processPath*と*引数*開発者によって手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-213">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="762d6-214">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-214">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="762d6-215">web.config ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="762d6-215">web.config file location</span></span>

<span data-ttu-id="762d6-216">[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)を正しく設定するためには、展開されるアプリのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-216">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="762d6-217">これは、IIS に提供される web サイトの物理パスと同じ場所です。</span><span class="sxs-lookup"><span data-stu-id="762d6-217">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="762d6-218">*Web.config* ファイルは、Web 配置を使用して複数のアプリの発行を有効にするため、アプリのルートで必要です。</span><span class="sxs-lookup"><span data-stu-id="762d6-218">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="762d6-219">アプリの物理パスには、*\<assembly>.runtimeconfig.json*、*\<assembly>.xml* (XML ドキュメントのコメント)、*\<assembly>.deps.json* などの機密性の高いファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="762d6-219">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="762d6-220">*web.config* ファイルが存在し、サイトは通常どおり起動した場合、IIS は、これらの機密性の高いファイルが要求された場合にファイルを提供しません。</span><span class="sxs-lookup"><span data-stu-id="762d6-220">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="762d6-221">*web.config*ファイルが存在しないか、不適切な名前が付けられているか、または通常の起動用にサイトを構成できない場合、IIS が機密性の高いファイルを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-221">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="762d6-222">***web.config*ファイルは、展開環境に常に存在し、適切な名前が付けられ、通常の起動用にサイトを構成できる必要があります。*web.config* ファイルを運用環境の展開から削除しないでください。**</span><span class="sxs-lookup"><span data-stu-id="762d6-222">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="762d6-223">web.config を変換する</span><span class="sxs-lookup"><span data-stu-id="762d6-223">Transform web.config</span></span>

<span data-ttu-id="762d6-224">発行時に *web.config* を変換する必要がある場合 (たとえば、構成、プロファイル、環境に基づいて環境変数を設定する場合) は、<xref:host-and-deploy/iis/transform-webconfig> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-224">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="762d6-225">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="762d6-225">IIS configuration</span></span>

<span data-ttu-id="762d6-226">**Windows Server オペレーティング システム**</span><span class="sxs-lookup"><span data-stu-id="762d6-226">**Windows Server operating systems**</span></span>

<span data-ttu-id="762d6-227">**Web サーバー (IIS)** サーバーの役割を有効にし、役割のサービスを設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-227">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="762d6-228">**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-228">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="762d6-229">**[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="762d6-229">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="762d6-231">**[機能]** のステップの後に、**[役割サービスの]** のステップで Web サーバー (IIS) を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="762d6-231">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="762d6-232">希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-232">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![[役割サービスの選択] のステップで既定の役割サービスを選択します。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="762d6-234">**Windows 認証 (任意)**</span><span class="sxs-lookup"><span data-stu-id="762d6-234">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="762d6-235">Windows 認証を有効にするには、**[Web サーバー]** > **[セキュリティ]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="762d6-235">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="762d6-236">**[Windows 認証]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-236">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="762d6-237">詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-237">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="762d6-238">**Websocket (省略可能)**</span><span class="sxs-lookup"><span data-stu-id="762d6-238">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="762d6-239">WebSockets は、ASP.NET Core 1.1 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="762d6-239">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="762d6-240">WebSocket を有効にするには、**[Web サーバー]** > **[アプリケーション開発]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="762d6-240">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="762d6-241">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-241">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="762d6-242">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-242">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="762d6-243">**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="762d6-243">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="762d6-244">**Web サーバー (IIS)** の役割をインストールした後、サーバーと IIS の再起動は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="762d6-244">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="762d6-245">**Windows デスクトップ オペレーティング システム**</span><span class="sxs-lookup"><span data-stu-id="762d6-245">**Windows desktop operating systems**</span></span>

<span data-ttu-id="762d6-246">**[IIS 管理コンソール]** と **[World Wide Web サービス]** を有効にします。</span><span class="sxs-lookup"><span data-stu-id="762d6-246">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="762d6-247">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="762d6-247">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="762d6-248">**インターネット インフォメーション サービス (IIS)** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="762d6-248">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="762d6-249">**[Web 管理ツール]** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="762d6-249">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="762d6-250">**[IIS 管理コンソール]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="762d6-250">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="762d6-251">**[World Wide Web サービス]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="762d6-251">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="762d6-252">**[World Wide Web サービス]** の既定の機能をそのまま使用するか、IIS 機能をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="762d6-252">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="762d6-253">**Windows 認証 (任意)**</span><span class="sxs-lookup"><span data-stu-id="762d6-253">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="762d6-254">Windows 認証を有効にするには、**[World Wide Web サービス]** > **[セキュリティ]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="762d6-254">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="762d6-255">**[Windows 認証]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-255">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="762d6-256">詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-256">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="762d6-257">**Websocket (省略可能)**</span><span class="sxs-lookup"><span data-stu-id="762d6-257">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="762d6-258">WebSockets は、ASP.NET Core 1.1 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="762d6-258">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="762d6-259">WebSocket を有効にするには、**[World Wide Web サービス]** > **[アプリケーション開発機能]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="762d6-259">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="762d6-260">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-260">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="762d6-261">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-261">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="762d6-262">IIS のインストールに再起動が必要な場合は、システムを再起動します。</span><span class="sxs-lookup"><span data-stu-id="762d6-262">If the IIS installation requires a restart, restart the system.</span></span>

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="762d6-264">.NET Core ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="762d6-264">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="762d6-265">ホスティング システムに *.NET Core ホスティング バンドル*をインストールします。</span><span class="sxs-lookup"><span data-stu-id="762d6-265">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="762d6-266">このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-266">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="762d6-267">このモジュールでは、ASP.NET Core アプリが IIS の背後で実行できるようになります。</span><span class="sxs-lookup"><span data-stu-id="762d6-267">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="762d6-268">システムにインターネット接続が設定されていない場合は、.NET Core ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="762d6-268">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="762d6-269">ホスティング バンドルが IIS の前にインストールされている場合、バンドルのインストールを修復する必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-269">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="762d6-270">IIS をインストールした後に、ホスティング バンドル インストーラーをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="762d6-270">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="762d6-271">直接ダウンロード (現在のバージョン)</span><span class="sxs-lookup"><span data-stu-id="762d6-271">Direct download (current version)</span></span>

<span data-ttu-id="762d6-272">次のリンクを使用してインストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="762d6-272">Download the installer using the following link:</span></span>

[<span data-ttu-id="762d6-273">現在の .NET Core ホスティング バンドルのインストーラー (直接ダウンロード)</span><span class="sxs-lookup"><span data-stu-id="762d6-273">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="762d6-274">以前のバージョンのインストーラー</span><span class="sxs-lookup"><span data-stu-id="762d6-274">Earlier versions of the installer</span></span>

<span data-ttu-id="762d6-275">以前のバージョンのインストーラーを入手するには:</span><span class="sxs-lookup"><span data-stu-id="762d6-275">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="762d6-276">[.NET のダウンロードのアーカイブ](https://www.microsoft.com/net/download/archives)に移動します。</span><span class="sxs-lookup"><span data-stu-id="762d6-276">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="762d6-277">**[.NET Core]** の下で、.NET Core のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-277">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="762d6-278">**[Run apps - Runtime]\(アプリの実行 - ランタイム\)** 列から、目的の .NET Core ランタイム バージョンの行を探します。</span><span class="sxs-lookup"><span data-stu-id="762d6-278">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="762d6-279">**[Runtime & Hosting Bundle]\(ランタイム & ホスティング バンドル\)** のリンクを使用してインストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="762d6-279">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="762d6-280">一部のインストーラーには、有効期限 (EOL) に達して Microsoft によるサポートが終了した、リリース バージョンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="762d6-280">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="762d6-281">詳細については、[サポート ポリシー](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="762d6-281">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="762d6-282">ホスティング バンドルをインストールする</span><span class="sxs-lookup"><span data-stu-id="762d6-282">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="762d6-283">サーバーでインストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="762d6-283">Run the installer on the server.</span></span> <span data-ttu-id="762d6-284">管理者のコマンド シェルからインストーラーを実行する場合、次のパラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="762d6-284">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="762d6-285">`OPT_NO_ANCM=1` &ndash; ASP.NET Core モジュールのインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="762d6-285">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="762d6-286">`OPT_NO_RUNTIME=1` &ndash; .NET Core ランタイムのインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="762d6-286">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="762d6-287">`OPT_NO_SHAREDFX=1` &ndash; ASP.NET Shared Framework (ASP.NET ランタイム) のインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="762d6-287">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="762d6-288">`OPT_NO_X86=1` &ndash; x86 ランタイムのインストールをスキップします。</span><span class="sxs-lookup"><span data-stu-id="762d6-288">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="762d6-289">32 ビット アプリをホストしない場合は、このパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-289">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="762d6-290">今後、32 ビットと 64 ビットのアプリ両方をホストする可能性がある場合は、このパラメーターを使用せずに、両方のランタイムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="762d6-290">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="762d6-291">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; 共有構成 (*applicationHost.config*) が IIS のインストールと同じマシン上にある場合、IIS 共有構成を使うためのチェックを無効にします。</span><span class="sxs-lookup"><span data-stu-id="762d6-291">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="762d6-292">"*ASP.NET Core 2.2 以降の Hosting Bundler インストーラーに対してのみ使用できます。*"</span><span class="sxs-lookup"><span data-stu-id="762d6-292">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="762d6-293">詳細については、「<xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-293">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="762d6-294">システムを再起動するか、コマンド シェルから **net stop was /y** に続けて **net start w3svc** を実行します。</span><span class="sxs-lookup"><span data-stu-id="762d6-294">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command shell.</span></span> <span data-ttu-id="762d6-295">IIS を再起動すると、インストーラーによって行われたシステム パスへの変更 (環境変数) が取得されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-295">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="762d6-296">Windows ホスティング バンドルのインストーラーでインストールを完了するために、IIS によるリセットが必要であることを検知した場合、インストーラーによって IIS がリセットされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-296">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="762d6-297">インストーラーで IIS のリセットがトリガーされた場合、IIS アプリ プールと Web サイトがすべて再起動されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-297">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="762d6-298">IIS 共有構成の詳細については、「[ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-298">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="762d6-299">Visual Studio で発行する場合の Web 配置のインストール</span><span class="sxs-lookup"><span data-stu-id="762d6-299">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="762d6-300">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してサーバーにアプリを展開する場合、Web 配置の最新バージョンをサーバーにインストールします。</span><span class="sxs-lookup"><span data-stu-id="762d6-300">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="762d6-301">Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="762d6-301">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="762d6-302">WebPI を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="762d6-302">The preferred method is to use WebPI.</span></span> <span data-ttu-id="762d6-303">WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="762d6-303">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="762d6-304">IIS サイトを作成する</span><span class="sxs-lookup"><span data-stu-id="762d6-304">Create the IIS site</span></span>

1. <span data-ttu-id="762d6-305">ホスト システムで、アプリの公開フォルダーとファイルを格納するフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="762d6-305">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="762d6-306">アプリの展開レイアウトについては、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-306">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="762d6-307">IIS マネージャーの **[接続]** パネルで、サーバーのノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="762d6-307">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="762d6-308">**[サイト]** フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="762d6-308">Right-click the **Sites** folder.</span></span> <span data-ttu-id="762d6-309">コンテキスト メニューで **[Web サイトの追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-309">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="762d6-310">**[サイト名]** を指定し、**[物理パス]** にはアプリの配置フォルダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-310">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="762d6-311">**[バインド]** の構成を指定して **[OK]** を選択し、Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="762d6-311">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="762d6-313">最上位のワイルドカードのバインド ( `http://*:80/` と `http://+:80` ) は使用しては **いけません** 。</span><span class="sxs-lookup"><span data-stu-id="762d6-313">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="762d6-314">最上位のワイルドカードのバインドは、セキュリティの脆弱性に対してアプリを切り開くことができます。</span><span class="sxs-lookup"><span data-stu-id="762d6-314">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="762d6-315">これは、強力と脆弱の両方のワイルドカードに適用されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-315">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="762d6-316">ワイルドカードではなく、明示的なホスト名を使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-316">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="762d6-317">全体の親ドメインを制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.mysub.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="762d6-317">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="762d6-318">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-318">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="762d6-319">サーバーのノードでは、**[アプリケーション プール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-319">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="762d6-320">サイトのアプリケーション プールを右クリックし、コンテキスト メニューから **[基本設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-320">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="762d6-321">**[アプリケーション プールの編集]** ウィンドウで、**[.NET CLR バージョン]** を **[マネージド コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-321">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![.NET CLR バージョンとして [マネージド コードなし] を設定します。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="762d6-323">ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。</span><span class="sxs-lookup"><span data-stu-id="762d6-323">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="762d6-324">ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。</span><span class="sxs-lookup"><span data-stu-id="762d6-324">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="762d6-325">**[.NET CLR バージョン]** の **[マネージド コードなし]** への設定は、任意です。</span><span class="sxs-lookup"><span data-stu-id="762d6-325">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="762d6-326">*ASP.NET Core 2.2 以降*:[インプロセス ホスティング モデル](xref:fundamentals/servers/index#in-process-hosting-model)を使用する 64 ビット (x64) の[自己完結型展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合、32 ビット (x86) プロセス用のアプリケーション プールを無効にします。</span><span class="sxs-lookup"><span data-stu-id="762d6-326">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="762d6-327">IIS マネージャー > **[アプリケーション プール]** の **[操作]** サイドバーで、**[アプリケーション プールの既定値の設定]** または **[詳細設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-327">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="762d6-328">**[32 ビット アプリケーションの有効化]** を探し、値を `False` に設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-328">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="762d6-329">この設定は[アウトプロセス ホスティング](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)で展開されたアプリには影響しません。</span><span class="sxs-lookup"><span data-stu-id="762d6-329">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="762d6-330">プロセス モデル ID に適切なアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="762d6-330">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="762d6-331">アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="762d6-331">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="762d6-332">たとえば、アプリケーション プールには、アプリがファイルの読み取りおよび書き込みを行うフォルダーへの読み取りおよび書き込みアクセスが必要です。</span><span class="sxs-lookup"><span data-stu-id="762d6-332">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="762d6-333">**Windows 認証の構成 (任意)**</span><span class="sxs-lookup"><span data-stu-id="762d6-333">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="762d6-334">詳細については、「[Windows 認証を構成する](xref:security/authentication/windowsauth)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-334">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="762d6-335">アプリを配置する</span><span class="sxs-lookup"><span data-stu-id="762d6-335">Deploy the app</span></span>

<span data-ttu-id="762d6-336">ホスト システム上に作成したフォルダーにアプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="762d6-336">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="762d6-337">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="762d6-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="762d6-338">Visual Studio からのアプリの展開</span><span class="sxs-lookup"><span data-stu-id="762d6-338">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="762d6-339">Web 配置に使用する発行プロファイルの作成方法については、「[Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)」 (ASP.NET Core アプリ展開用の Visual Studio の発行プロファイル) のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-339">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="762d6-340">ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。</span><span class="sxs-lookup"><span data-stu-id="762d6-340">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[発行] ダイアログ ページ](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="762d6-342">Visual Studio 外部での Web 配置</span><span class="sxs-lookup"><span data-stu-id="762d6-342">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="762d6-343">Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="762d6-343">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="762d6-344">詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-344">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="762d6-345">Web 配置の代替手段</span><span class="sxs-lookup"><span data-stu-id="762d6-345">Alternatives to Web Deploy</span></span>

<span data-ttu-id="762d6-346">手動コピー、Xcopy、Robocopy、PowerShell などの任意の方法を使用して、ホスティング システムにアプリを移動します。</span><span class="sxs-lookup"><span data-stu-id="762d6-346">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="762d6-347">IIS への ASP.NET Core の展開の詳細については、「[IIS 管理者用の展開リソース](#deployment-resources-for-iis-administrators)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-347">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="762d6-348">Web サイトを閲覧する</span><span class="sxs-lookup"><span data-stu-id="762d6-348">Browse the website</span></span>

![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="762d6-350">ロックされた展開ファイル</span><span class="sxs-lookup"><span data-stu-id="762d6-350">Locked deployment files</span></span>

<span data-ttu-id="762d6-351">アプリが実行中は、展開フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-351">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="762d6-352">展開中にロックされたファイルを上書きすることはできません。</span><span class="sxs-lookup"><span data-stu-id="762d6-352">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="762d6-353">展開でロックされているファイルをリリースするには、次の**いずれか**の方法を使用してアプリ プールを停止します。</span><span class="sxs-lookup"><span data-stu-id="762d6-353">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="762d6-354">Web 配置を使用し、プロジェクト ファイル内の `Microsoft.NET.Sdk.Web` を参照して停止します。</span><span class="sxs-lookup"><span data-stu-id="762d6-354">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="762d6-355">*app_offline.htm* ファイルは、Web アプリのディレクトリのルートに配置されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-355">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="762d6-356">ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="762d6-356">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="762d6-357">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-357">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="762d6-358">サーバー上の IIS マネージャーで手動によりアプリ プールを停止します。</span><span class="sxs-lookup"><span data-stu-id="762d6-358">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="762d6-359">PowerShell を使用して *app_offline.html* をドロップします (PowerShell 5 以降が必要)。</span><span class="sxs-lookup"><span data-stu-id="762d6-359">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="762d6-360">データの保護</span><span class="sxs-lookup"><span data-stu-id="762d6-360">Data protection</span></span>

<span data-ttu-id="762d6-361">[ASP.NET Core データ保護スタック](xref:security/data-protection/introduction)は、認証で使用されるミドルウェアを含め、いくつかの [ASP.NET Core](xref:fundamentals/middleware/index) ミドルウェアによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-361">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="762d6-362">データ保護 API がユーザーのコードから呼び出されない場合でも、配置スクリプトを使用するかユーザー コードで、永続的な暗号化[キー ストア](xref:security/data-protection/implementation/key-management)を作成するようにデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-362">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="762d6-363">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-363">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="762d6-364">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="762d6-364">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="762d6-365">すべての cookie ベースの認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="762d6-365">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="762d6-366">ユーザーは、次回の要求時に再度サインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-366">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="762d6-367">キーリングで保護されているデータは、いずれも復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="762d6-367">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="762d6-368">これには、[CSRF トークン](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)と [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="762d6-368">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="762d6-369">キーリングを保持するために IIS でのデータ保護を構成するには、次の**いずれか**の方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-369">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="762d6-370">**データ保護のレジストリ キーを作成する**</span><span class="sxs-lookup"><span data-stu-id="762d6-370">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="762d6-371">ASP.NET Core アプリで使用されるデータ保護キーは、アプリ外部のレジストリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-371">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="762d6-372">特定のアプリのキーを保持するには、アプリ プール用のレジストリ キーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-372">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="762d6-373">非 Web ファーム IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="762d6-373">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="762d6-374">このスクリプトは、HKLM レジストリにレジストリ キーを作成します。そのキーは、アプリのアプリ プールのワーカー プロセス アカウントのみがアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="762d6-374">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="762d6-375">キーは保存時に、DPAPI を使用して、コンピューター全体に適用するキーによって暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-375">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="762d6-376">Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="762d6-376">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="762d6-377">既定では、データ保護キーは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="762d6-377">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="762d6-378">ネットワーク共有のファイルのアクセス許可が、アプリを実行する Windows アカウントに限定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="762d6-378">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="762d6-379">保存時のキーを保護するために、X509 証明書を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="762d6-379">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="762d6-380">ユーザーが証明書をアップロードできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="762d6-380">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="762d6-381">詳細については、「[ASP.NET Core データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-381">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="762d6-382">**ユーザー プロファイルを読み込むための IIS アプリケーション プールを構成する**</span><span class="sxs-lookup"><span data-stu-id="762d6-382">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="762d6-383">この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="762d6-383">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="762d6-384">**[ユーザー プロファイルの読み込み]** を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-384">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="762d6-385">`True` に設定すると、キーはユーザー プロファイル ディレクトリに格納され、ユーザー アカウントに固有のキーと共にデータ保護 API を使って保護されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-385">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="762d6-386">キーは *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* フォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-386">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="762d6-387">アプリ プールの [setProfileEnvironment 属性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)も有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-387">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="762d6-388">`setProfileEnvironment` の既定値は `true`です。</span><span class="sxs-lookup"><span data-stu-id="762d6-388">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="762d6-389">一部のシナリオ (たとえば、Windows OS) では、`setProfileEnvironment` は `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-389">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="762d6-390">キーが期待どおりにユーザー プロファイル ディレクトリに格納されていない場合:</span><span class="sxs-lookup"><span data-stu-id="762d6-390">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="762d6-391">*%windir%/system32/inetsrv/config* フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="762d6-391">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="762d6-392">*applicationHost.config* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="762d6-392">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="762d6-393">`<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 要素を探します。</span><span class="sxs-lookup"><span data-stu-id="762d6-393">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="762d6-394">`setProfileEnvironment` 属性 (その規定値は `true` です) が存在しないことを確認するか、属性の値を明示的に `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-394">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="762d6-395">**ファイル システムをキー リング ストアとして使用する**</span><span class="sxs-lookup"><span data-stu-id="762d6-395">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="762d6-396">[ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。</span><span class="sxs-lookup"><span data-stu-id="762d6-396">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="762d6-397">X509 証明書を使用してキー リングを保護し、その証明書が信頼された証明書であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="762d6-397">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="762d6-398">証明書が自己署名されている場合は、信頼されたルート ストアにその証明書を配置します。</span><span class="sxs-lookup"><span data-stu-id="762d6-398">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="762d6-399">Web ファームで IIS を使用する場合:</span><span class="sxs-lookup"><span data-stu-id="762d6-399">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="762d6-400">すべてのコンピューターがアクセスできるファイル共有を使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-400">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="762d6-401">X509 証明書を各コンピューターに配置します。</span><span class="sxs-lookup"><span data-stu-id="762d6-401">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="762d6-402">[コード内にデータ保護](xref:security/data-protection/configuration/overview)を構成します。</span><span class="sxs-lookup"><span data-stu-id="762d6-402">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="762d6-403">**コンピューター全体に適用するデータ保護ポリシーを設定する**</span><span class="sxs-lookup"><span data-stu-id="762d6-403">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="762d6-404">データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。</span><span class="sxs-lookup"><span data-stu-id="762d6-404">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="762d6-405">詳細については、「<xref:security/data-protection/introduction>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-405">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="762d6-406">仮想ディレクトリ</span><span class="sxs-lookup"><span data-stu-id="762d6-406">Virtual Directories</span></span>

<span data-ttu-id="762d6-407">ASP.NET Core アプリでは [IIS 仮想ディレクトリ](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="762d6-407">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="762d6-408">アプリは[サブアプリケーション](#sub-applications)としてホスティングできます。</span><span class="sxs-lookup"><span data-stu-id="762d6-408">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="762d6-409">サブアプリケーション</span><span class="sxs-lookup"><span data-stu-id="762d6-409">Sub-applications</span></span>

<span data-ttu-id="762d6-410">ASP.NET Core アプリは [IIS サブアプリケーション (サブアプリ)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications) としてホスティングできます。</span><span class="sxs-lookup"><span data-stu-id="762d6-410">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="762d6-411">サブアプリのパスは、ルート アプリの URL の一部になります。</span><span class="sxs-lookup"><span data-stu-id="762d6-411">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="762d6-412">サブアプリに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="762d6-412">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="762d6-413">このモジュールをサブアプリの *web.config* ファイルにハンドラーとして追加すると、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する *500.19 内部サーバー エラー* が返されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-413">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="762d6-414">次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルを示しています。</span><span class="sxs-lookup"><span data-stu-id="762d6-414">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="762d6-415">ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホスティングする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除します。</span><span class="sxs-lookup"><span data-stu-id="762d6-415">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="762d6-416">サブアプリ内にある静的資産のリンクでは、チルダとスラッシュの表記 (`~/`) を使う必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-416">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="762d6-417">チルダとスラッシュの表記により[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)がトリガーされ、作成される相対リンクにサブアプリのパスベースが付加されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-417">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="762d6-418">`/subapp_path` にあるサブアプリの場合、`src="~/image.png"` を使ってリンクされる画像は `src="/subapp_path/image.png"` として作成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-418">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="762d6-419">ルート アプリの静的ファイル ミドルウェアでは、静的ファイル要求は処理されません。</span><span class="sxs-lookup"><span data-stu-id="762d6-419">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="762d6-420">この要求は、サブアプリの静的ファイル ミドルウェアによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-420">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="762d6-421">静的資産の `src` 属性が絶対パス (たとえば `src="/image.png"`) に設定されている場合、リンクはサブアプリのパスベースなしで作成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-421">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="762d6-422">ルート アプリの静的ファイル ミドルウェアでは、ルート アプリの [Web ルート](xref:fundamentals/index#web-root)から資産を提供しようとしますが、ルート アプリから静的資産を利用できる場合を除いて *404 - Not Found* 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-422">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="762d6-423">ある ASP.NET Core アプリを別の ASP.NET Core アプリの下でサブアプリとしてホスティングするには:</span><span class="sxs-lookup"><span data-stu-id="762d6-423">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="762d6-424">サブアプリ用のアプリ プールを確立します。</span><span class="sxs-lookup"><span data-stu-id="762d6-424">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="762d6-425">**[.NET CLR バージョン]** を **[マネージド コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="762d6-425">Set the **.NET CLR Version** to **No Managed Code**.</span></span>

1. <span data-ttu-id="762d6-426">ルート サイトを IIS マネージャーに追加し、サブアプリをルート サイトの下のフォルダー内に置きます。</span><span class="sxs-lookup"><span data-stu-id="762d6-426">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="762d6-427">IIS マネージャーでサブアプリのフォルダーを右クリックし、**[アプリケーションへの変換]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-427">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="762d6-428">**[アプリケーションの追加]** ダイアログ ボックスで、**[アプリケーション プール]** に対して **[選択]** ボタンを使い、作成したアプリケーション プールをサブアプリ用に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="762d6-428">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="762d6-429">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-429">Select **OK**.</span></span>

<span data-ttu-id="762d6-430">サブアプリに対して個別のアプリ プールを割り当てることは、インプロセス ホスティング モデルを使用する場合必須となります。</span><span class="sxs-lookup"><span data-stu-id="762d6-430">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="762d6-431">インプロセス ホスティング モデルと ASP.NET Core モジュールの構成について詳しくは、<xref:host-and-deploy/aspnet-core-module> と <xref:host-and-deploy/aspnet-core-module> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="762d6-431">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="762d6-432">web.config による IIS の構成</span><span class="sxs-lookup"><span data-stu-id="762d6-432">Configuration of IIS with web.config</span></span>

<span data-ttu-id="762d6-433">IIS の構成は ASP.NET Core モジュールを使用した ASP.NET Core アプリで機能する IIS シナリオの *web.config* の `<system.webServer>` セクションによる影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="762d6-433">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="762d6-434">たとえば、IIS の構成は動的な圧縮で機能します。</span><span class="sxs-lookup"><span data-stu-id="762d6-434">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="762d6-435">IIS が動的な圧縮を使用するようにサーバー レベルで構成されている場合、アプリの *web.config* ファイルの `<urlCompression>` 要素を使用すると、それを ASP.NET Core アプリに対して無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="762d6-435">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="762d6-436">詳細については、[\<system.webServer> の構成リファレンス](/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」および「[IIS モジュールと ASP.NET Core](xref:host-and-deploy/iis/modules)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-436">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="762d6-437">分離されたアプリ プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定するには、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-437">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="762d6-438">web.config の構成のセクション</span><span class="sxs-lookup"><span data-stu-id="762d6-438">Configuration sections of web.config</span></span>

<span data-ttu-id="762d6-439">*web.config* の ASP.NET 4.x アプリの構成セクションは、ASP.NET Core アプリの構成では使用されません。</span><span class="sxs-lookup"><span data-stu-id="762d6-439">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="762d6-440">ASP.NET Core アプリは、他の構成プロバイダーを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-440">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="762d6-441">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-441">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="762d6-442">アプリケーション プール</span><span class="sxs-lookup"><span data-stu-id="762d6-442">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="762d6-443">アプリケーション プールの分離は、ホスティング モデルによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-443">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="762d6-444">インプロセス ホスティング &ndash; 別のアプリケーション プールでアプリケーションを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-444">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="762d6-445">アウトプロセス ホスティング &ndash; アプリケーションをそれぞれ専用のアプリケーションプールで実行して、アプリケーションを相互に分離することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="762d6-445">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="762d6-446">IIS の **[Web サイトの追加]** ダイアログの既定では、アプリケーションごとに 1 つのアプリケーション プールです。</span><span class="sxs-lookup"><span data-stu-id="762d6-446">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="762d6-447">**[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-447">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="762d6-448">サイトが追加されるときに、そのサイト名を使用して新しいアプリ プールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-448">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="762d6-449">サーバーで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリ プールで実行して、アプリを相互に分離することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="762d6-449">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="762d6-450">IIS の **[Web サイトの追加]** ダイアログはこの構成の既定です。</span><span class="sxs-lookup"><span data-stu-id="762d6-450">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="762d6-451">**[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-451">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="762d6-452">サイトが追加されるときに、そのサイト名を使用して新しいアプリ プールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-452">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="762d6-453">アプリケーション プール ID</span><span class="sxs-lookup"><span data-stu-id="762d6-453">Application Pool Identity</span></span>

<span data-ttu-id="762d6-454">アプリ プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリを実行できます。</span><span class="sxs-lookup"><span data-stu-id="762d6-454">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="762d6-455">IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリ プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。</span><span class="sxs-lookup"><span data-stu-id="762d6-455">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="762d6-456">IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="762d6-456">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![アプリケーション プールの [詳細設定] ダイアログ](index/_static/apppool-identity.png)

<span data-ttu-id="762d6-458">IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。</span><span class="sxs-lookup"><span data-stu-id="762d6-458">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="762d6-459">この ID を使用してリソースを保護することができます。</span><span class="sxs-lookup"><span data-stu-id="762d6-459">Resources can be secured using this identity.</span></span> <span data-ttu-id="762d6-460">ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。</span><span class="sxs-lookup"><span data-stu-id="762d6-460">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="762d6-461">アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリを含むディレクトリのアクセス制御リスト (ACL) を変更します。</span><span class="sxs-lookup"><span data-stu-id="762d6-461">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="762d6-462">エクスプローラーを開き、そのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="762d6-462">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="762d6-463">そのディレクトリを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-463">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="762d6-464">**[セキュリティ]** タブの **[編集]** ボタンを選択し、**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="762d6-464">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="762d6-465">**[場所]** ボタンを選択し、システムが選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="762d6-465">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="762d6-466">**[選択するオブジェクト名を入力してください]** 領域に、「**IIS AppPool\\<app_pool_name>**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="762d6-466">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="762d6-467">**[名前の確認]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-467">Select the **Check Names** button.</span></span> <span data-ttu-id="762d6-468">*DefaultAppPool* で、**IIS AppPool\DefaultAppPool** を使用して名前を確認します。</span><span class="sxs-lookup"><span data-stu-id="762d6-468">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="762d6-469">**[名前の確認]** ボタンが選択されているときには、**DefaultAppPool** の値が、オブジェクト名領域に表示されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-469">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="762d6-470">オブジェクト名の領域に直接アプリケーション プール名を入力することはできません。</span><span class="sxs-lookup"><span data-stu-id="762d6-470">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="762d6-471">オブジェクト名を確認するときには、**IIS AppPool\\<app_pool_name>** の形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-471">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択する前に、オブジェクト名領域で "DefaultAppPool" のアプリ プール名が "IIS AppPool\" に適用されます。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="762d6-473">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="762d6-473">Select **OK**.</span></span>

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択した後に、オブジェクト名 "DefaultAppPool" がオブジェクト名領域に表示されます。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="762d6-475">読み取り &amp; 実行アクセス許可は、既定で付与される必要があります。</span><span class="sxs-lookup"><span data-stu-id="762d6-475">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="762d6-476">必要に応じて、追加のアクセス許可を提供します。</span><span class="sxs-lookup"><span data-stu-id="762d6-476">Provide additional permissions as needed.</span></span>

<span data-ttu-id="762d6-477">**ICACLS** ツールを使用してコマンド プロンプトでアクセス許可を付与することもできます。</span><span class="sxs-lookup"><span data-stu-id="762d6-477">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="762d6-478">たとえば、*DefaultAppPool* を使用する場合、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-478">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="762d6-479">詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-479">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="762d6-480">HTTP/2 のサポート</span><span class="sxs-lookup"><span data-stu-id="762d6-480">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="762d6-481">[HTTP/2](https://httpwg.org/specs/rfc7540.html) は、次の IIS 展開シナリオでの ASP.NET Core でサポートされます。</span><span class="sxs-lookup"><span data-stu-id="762d6-481">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="762d6-482">インプロセス</span><span class="sxs-lookup"><span data-stu-id="762d6-482">In-process</span></span>
  * <span data-ttu-id="762d6-483">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="762d6-483">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="762d6-484">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="762d6-484">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="762d6-485">アウトプロセス</span><span class="sxs-lookup"><span data-stu-id="762d6-485">Out-of-process</span></span>
  * <span data-ttu-id="762d6-486">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="762d6-486">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="762d6-487">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、[Kestrel サーバー](xref:fundamentals/servers/kestrel)へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-487">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="762d6-488">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="762d6-488">TLS 1.2 or later connection</span></span>

<span data-ttu-id="762d6-489">インプロセスの展開の場合、HTTP/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) によって `HTTP/2` が報告されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-489">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="762d6-490">アウトプロセスの展開の場合、HTTP/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) によって `HTTP/1.1` が報告されます。</span><span class="sxs-lookup"><span data-stu-id="762d6-490">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="762d6-491">インプロセス ホスティング モデルとアウトプロセス ホスティング モデルの詳細については、<xref:host-and-deploy/aspnet-core-module> トピックと <xref:host-and-deploy/aspnet-core-module> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-491">For more information on the in-process and out-of-process hosting models, see the <xref:host-and-deploy/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="762d6-492">次の基本要件を満たすアウトプロセスの展開には、[HTTP/2](https://httpwg.org/specs/rfc7540.html) がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="762d6-492">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="762d6-493">Windows Server 2016/Windows 10 以降、IIS 10 以降</span><span class="sxs-lookup"><span data-stu-id="762d6-493">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="762d6-494">一般向けのエッジ サーバーでは HTTP/2 を使用しますが、[Kestrel サーバー](xref:fundamentals/servers/kestrel)へのリバース プロキシ接続では HTTP/1.1 を使用します。</span><span class="sxs-lookup"><span data-stu-id="762d6-494">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="762d6-495">ターゲット フレームワーク:HTTP/2 接続は IIS によって完全に処理されるため、アウトプロセスの展開には適用できません。</span><span class="sxs-lookup"><span data-stu-id="762d6-495">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="762d6-496">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="762d6-496">TLS 1.2 or later connection</span></span>

<span data-ttu-id="762d6-497">Http/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) が `HTTP/1.1` を報告します。</span><span class="sxs-lookup"><span data-stu-id="762d6-497">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="762d6-498">HTTP/2 は既定で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="762d6-498">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="762d6-499">HTTP/2 接続が確立されない場合、接続は HTTP/1.1 にフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="762d6-499">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="762d6-500">IIS 展開での HTTP/2 構成の詳細については、[IIS での HTTP/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="762d6-500">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="762d6-501">CORS プレフライト要求</span><span class="sxs-lookup"><span data-stu-id="762d6-501">CORS preflight requests</span></span>

<span data-ttu-id="762d6-502">"*このセクションは、.NET Framework をターゲットにした ASP.NET Core アプリにのみ適用されます。*"</span><span class="sxs-lookup"><span data-stu-id="762d6-502">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="762d6-503">.NET Framework をターゲットにした ASP.NET Core アプリの場合、IIS では既定で OPTIONS 要求がアプリに渡されません。</span><span class="sxs-lookup"><span data-stu-id="762d6-503">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="762d6-504">OPTIONS 要求を渡すように *web.config* でアプリの IIS のハンドラーを構成する方法については、[ASP.NET Web API 2 でのクロスオリジン要求の有効化:CORS のしくみ](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="762d6-504">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="762d6-505">IIS 管理者用の展開リソース</span><span class="sxs-lookup"><span data-stu-id="762d6-505">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="762d6-506">IIS ドキュメントでの IIS の詳細について説明します。</span><span class="sxs-lookup"><span data-stu-id="762d6-506">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="762d6-507">IIS ドキュメント </span><span class="sxs-lookup"><span data-stu-id="762d6-507">IIS documentation</span></span>](/iis)

<span data-ttu-id="762d6-508">.NET Core アプリの展開モデルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="762d6-508">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="762d6-509">.NET Core アプリケーションの展開</span><span class="sxs-lookup"><span data-stu-id="762d6-509">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="762d6-510">Kestrel Web サーバーが IIS または IIS Express をリバース プロキシ サーバーとして使用できるようにするための ASP.NET Core モジュールについて説明します。</span><span class="sxs-lookup"><span data-stu-id="762d6-510">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="762d6-511">ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="762d6-511">ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="762d6-512">ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="762d6-512">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="762d6-513">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="762d6-513">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="762d6-514">発行された ASP.NET Core アプリのディレクトリ構造について説明します。</span><span class="sxs-lookup"><span data-stu-id="762d6-514">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="762d6-515">ディレクトリの構造</span><span class="sxs-lookup"><span data-stu-id="762d6-515">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="762d6-516">ASP.NET Core アプリに対してアクティブおよび非アクティブな IIS モジュールについて、さらに IIS モジュールの管理方法についてを把握します。</span><span class="sxs-lookup"><span data-stu-id="762d6-516">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="762d6-517">IIS モジュール</span><span class="sxs-lookup"><span data-stu-id="762d6-517">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="762d6-518">ASP.NET Core アプリの IIS 展開に関する問題を診断する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="762d6-518">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="762d6-519">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="762d6-519">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="762d6-520">IIS で ASP.NET Core アプリをホストする場合の一般的なエラーを識別します。</span><span class="sxs-lookup"><span data-stu-id="762d6-520">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="762d6-521">Azure App Service と IIS の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="762d6-521">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="762d6-522">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="762d6-522">Additional resources</span></span>

* <xref:test/troubleshoot>
* [<span data-ttu-id="762d6-523">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="762d6-523">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="762d6-524">Microsoft IIS 公式サイト</span><span class="sxs-lookup"><span data-stu-id="762d6-524">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="762d6-525">Windows Server テクニカル コンテンツ ライブラリ</span><span class="sxs-lookup"><span data-stu-id="762d6-525">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="762d6-526">IIS での HTTP/2</span><span class="sxs-lookup"><span data-stu-id="762d6-526">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
