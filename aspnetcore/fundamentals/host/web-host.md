---
title: ASP.NET Core の Web ホスト
author: guardrex
description: ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: b391b5e514e750f64f30d33cf4eb91e489242eba
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64888977"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="3d1e2-103">ASP.NET Core の Web ホスト</span><span class="sxs-lookup"><span data-stu-id="3d1e2-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="3d1e2-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3d1e2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3d1e2-105">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="3d1e2-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3d1e2-107">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="3d1e2-108">ホストでは、ログ記録、依存関係挿入、構成を設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="3d1e2-109">このトピックのバージョン 1.1 では、[ASP.NET Core Web ホスト (バージョン 1.1、PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf) をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-109">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="3d1e2-110">この記事では ASP.NET Core Web ホスト (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>) を取り上げます。これは Web アプリをホストするためのものです。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-110">This article covers the ASP.NET Core Web Host (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), which is for hosting web apps.</span></span> <span data-ttu-id="3d1e2-111">.NET 汎用ホスト ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) の詳細については、「<xref:fundamentals/host/generic-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-111">For information about the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="3d1e2-112">この記事では ASP.NET Core Web ホスト ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) を取り上げます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-112">This article covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span></span> <span data-ttu-id="3d1e2-113">ASP.NET Core 3.0 では、汎用ホストが Web ホストに取って代わります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-113">In ASP.NET Core 3.0, Generic Host replaces Web Host.</span></span> <span data-ttu-id="3d1e2-114">詳細については、「[ホスト](xref:fundamentals/index#host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-114">For more information, see [The host](xref:fundamentals/index#host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="3d1e2-115">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="3d1e2-115">Set up a host</span></span>

<span data-ttu-id="3d1e2-116">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-116">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="3d1e2-117">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-117">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="3d1e2-118">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-118">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="3d1e2-119">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-119">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="3d1e2-120">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-120">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="3d1e2-121">アプリのホスティング構成プロバイダーを使用して [Kestrel](xref:fundamentals/servers/kestrel) サーバーを Web サーバーとして構成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-121">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="3d1e2-122">Kestrel サーバーの既定のオプションについては、<xref:fundamentals/servers/kestrel#kestrel-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-122">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="3d1e2-123">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-123">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="3d1e2-124">次から[ ホスト構成](#host-configuration-values)を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-124">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="3d1e2-125">`ASPNETCORE_` のプレフィックスが付いた環境変数 (たとえば、`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-125">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="3d1e2-126">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-126">Command-line arguments.</span></span>
* <span data-ttu-id="3d1e2-127">次の順序でアプリの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-127">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="3d1e2-128">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-128">*appsettings.json*.</span></span>
  * <span data-ttu-id="3d1e2-129">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-129">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="3d1e2-130">エントリ アセンブリを使用して `Development` 環境でアプリが実行される場合に使用される[シークレット マネージャー](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-130">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="3d1e2-131">環境変数。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-131">Environment variables.</span></span>
  * <span data-ttu-id="3d1e2-132">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-132">Command-line arguments.</span></span>
* <span data-ttu-id="3d1e2-133">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-133">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="3d1e2-134">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-134">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="3d1e2-135">[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)を使用して IIS の背後で実行されている場合は、`CreateDefaultBuilder` によってアプリのベース アドレスとポートが構成される [IIS の統合](xref:host-and-deploy/iis/index)が有効になります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-135">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="3d1e2-136">IIS の統合により、[スタートアップ エラーをキャプチャする](#capture-startup-errors)アプリも構成されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-136">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="3d1e2-137">IIS の既定のオプションについては、<xref:host-and-deploy/iis/index#iis-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-137">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="3d1e2-138">アプリの環境が開発の場合、[ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-138">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="3d1e2-139">詳しくは、「[スコープの検証](#scope-validation)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-139">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="3d1e2-140">`CreateDefaultBuilder` によって定義された構成は、[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)、そして [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のその他のメソッドと拡張メソッドによってオーバーライドされ、拡張されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-140">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="3d1e2-141">以下に、いくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-141">A few examples follow:</span></span>

* <span data-ttu-id="3d1e2-142">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) はアプリの追加の `IConfiguration` を指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-142">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="3d1e2-143">次の `ConfigureAppConfiguration` の呼び出しによりデリゲートが追加され、*appsettings.xml* ファイルにアプリの構成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-143">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="3d1e2-144">`ConfigureAppConfiguration` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-144">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="3d1e2-145">この構成はホスト (たとえば、サーバーの URL や環境など) には適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-145">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="3d1e2-146">「[ホストの構成値](#host-configuration-values)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-146">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="3d1e2-147">次の `ConfigureLogging` の呼び出しによりデリゲートが追加され、最小ログ記録レベル ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) が [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel) に構成されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-147">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="3d1e2-148">この設定により、`CreateDefaultBuilder` で構成された *appsettings.Development.json* (`LogLevel.Debug`) および *appsettings.Production.json* (`LogLevel.Error`) の設定がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-148">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="3d1e2-149">`ConfigureLogging` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-149">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="3d1e2-150">次の `ConfigureKestrel` の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-150">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="3d1e2-151">次の [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-151">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="3d1e2-152">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-152">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="3d1e2-153">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-153">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="3d1e2-154">これは、[Visual Studio](https://visualstudio.microsoft.com) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-154">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="3d1e2-155">アプリの構成の詳細については、<xref:fundamentals/configuration/index> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-155">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="3d1e2-156">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-156">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="3d1e2-157">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-157">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="3d1e2-158">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-158">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="3d1e2-159">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-159">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="3d1e2-160">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-160">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="3d1e2-161">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-161">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="3d1e2-162">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-162">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="3d1e2-163">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="3d1e2-163">Host configuration values</span></span>

<span data-ttu-id="3d1e2-164">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-164">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="3d1e2-165">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-165">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="3d1e2-166">たとえば、`ASPNETCORE_ENVIRONMENT` のようにします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-166">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="3d1e2-167">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) や [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) などの拡張機能 (「[構成をオーバーライドする](#override-configuration)」のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-167">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="3d1e2-168">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-168">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="3d1e2-169">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-169">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="3d1e2-170">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-170">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="3d1e2-171">詳しくは、次のセクション「[構成をオーバーライドする](#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-171">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="3d1e2-172">アプリケーション キー (名前)</span><span class="sxs-lookup"><span data-stu-id="3d1e2-172">Application Key (Name)</span></span>

<span data-ttu-id="3d1e2-173">ホストの構築時に [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) または [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) が呼び出されると、[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) プロパティが自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-173">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="3d1e2-174">この値はアプリのエントリ ポイントを含むアセンブリの名前に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-174">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="3d1e2-175">値を明示的に設定するには、[WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-175">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="3d1e2-176">**キー**: applicationName</span><span class="sxs-lookup"><span data-stu-id="3d1e2-176">**Key**: applicationName</span></span>  
<span data-ttu-id="3d1e2-177">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-177">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-178">**既定値**:アプリのエントリ ポイントを含むアセンブリの名前。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-178">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="3d1e2-179">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-179">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3d1e2-180">**環境変数**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-180">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="3d1e2-181">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="3d1e2-181">Capture Startup Errors</span></span>

<span data-ttu-id="3d1e2-182">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-182">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="3d1e2-183">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="3d1e2-183">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="3d1e2-184">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="3d1e2-184">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3d1e2-185">**既定値**:アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-185">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="3d1e2-186">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-186">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="3d1e2-187">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-187">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="3d1e2-188">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-188">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="3d1e2-189">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-189">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="3d1e2-190">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="3d1e2-190">Content Root</span></span>

<span data-ttu-id="3d1e2-191">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-191">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="3d1e2-192">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="3d1e2-192">**Key**: contentRoot</span></span>  
<span data-ttu-id="3d1e2-193">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-193">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-194">**既定値**:既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-194">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="3d1e2-195">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-195">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="3d1e2-196">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-196">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="3d1e2-197">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-197">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="3d1e2-198">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-198">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="3d1e2-199">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="3d1e2-199">Detailed Errors</span></span>

<span data-ttu-id="3d1e2-200">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-200">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="3d1e2-201">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="3d1e2-201">**Key**: detailedErrors</span></span>  
<span data-ttu-id="3d1e2-202">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="3d1e2-202">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3d1e2-203">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="3d1e2-203">**Default**: false</span></span>  
<span data-ttu-id="3d1e2-204">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-204">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3d1e2-205">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-205">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="3d1e2-206">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-206">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="3d1e2-207">環境</span><span class="sxs-lookup"><span data-stu-id="3d1e2-207">Environment</span></span>

<span data-ttu-id="3d1e2-208">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-208">Sets the app's environment.</span></span>

<span data-ttu-id="3d1e2-209">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="3d1e2-209">**Key**: environment</span></span>  
<span data-ttu-id="3d1e2-210">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-210">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-211">**既定値**:実稼働</span><span class="sxs-lookup"><span data-stu-id="3d1e2-211">**Default**: Production</span></span>  
<span data-ttu-id="3d1e2-212">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-212">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="3d1e2-213">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-213">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="3d1e2-214">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-214">The environment can be set to any value.</span></span> <span data-ttu-id="3d1e2-215">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-215">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="3d1e2-216">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-216">Values aren't case sensitive.</span></span> <span data-ttu-id="3d1e2-217">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-217">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="3d1e2-218">[Visual Studio](https://visualstudio.microsoft.com) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-218">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="3d1e2-219">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-219">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="3d1e2-220">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="3d1e2-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="3d1e2-221">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="3d1e2-222">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="3d1e2-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="3d1e2-223">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-223">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-224">**既定値**:空の文字列</span><span class="sxs-lookup"><span data-stu-id="3d1e2-224">**Default**: Empty string</span></span>  
<span data-ttu-id="3d1e2-225">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3d1e2-226">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="3d1e2-227">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="3d1e2-228">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-228">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="3d1e2-229">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-229">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="3d1e2-230">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="3d1e2-230">HTTPS Port</span></span>

<span data-ttu-id="3d1e2-231">HTTPS リダイレクト ポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-231">Set the HTTPS redirect port.</span></span> <span data-ttu-id="3d1e2-232">[HTTPS の適用](xref:security/enforcing-ssl)に使用されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-232">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3d1e2-233">**キー**: https_port **型**: *文字列*
**既定値**:既定値は設定されていません。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-233">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="3d1e2-234">**次を使用して設定**:`UseSetting`
**環境変数**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-234">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="3d1e2-235">除外するホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="3d1e2-235">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="3d1e2-236">起動時に除外するホスティング スタートアップ アセンブリのセミコロン区切り文字列。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-236">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="3d1e2-237">**キー**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="3d1e2-237">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="3d1e2-238">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-238">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-239">**既定値**:空の文字列</span><span class="sxs-lookup"><span data-stu-id="3d1e2-239">**Default**: Empty string</span></span>  
<span data-ttu-id="3d1e2-240">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-240">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3d1e2-241">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-241">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="3d1e2-242">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="3d1e2-242">Prefer Hosting URLs</span></span>

<span data-ttu-id="3d1e2-243">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-243">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="3d1e2-244">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="3d1e2-244">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="3d1e2-245">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="3d1e2-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3d1e2-246">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="3d1e2-246">**Default**: true</span></span>  
<span data-ttu-id="3d1e2-247">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-247">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="3d1e2-248">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-248">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="3d1e2-249">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="3d1e2-249">Prevent Hosting Startup</span></span>

<span data-ttu-id="3d1e2-250">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-250">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="3d1e2-251">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-251">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="3d1e2-252">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="3d1e2-252">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="3d1e2-253">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="3d1e2-253">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3d1e2-254">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="3d1e2-254">**Default**: false</span></span>  
<span data-ttu-id="3d1e2-255">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-255">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="3d1e2-256">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-256">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="3d1e2-257">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="3d1e2-257">Server URLs</span></span>

<span data-ttu-id="3d1e2-258">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="3d1e2-259">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="3d1e2-259">**Key**: urls</span></span>  
<span data-ttu-id="3d1e2-260">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-260">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-261">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="3d1e2-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="3d1e2-262">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="3d1e2-263">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="3d1e2-264">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="3d1e2-265">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="3d1e2-266">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="3d1e2-267">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="3d1e2-268">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-268">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="3d1e2-269">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-269">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="3d1e2-270">詳細については、「<xref:fundamentals/servers/kestrel#endpoint-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-270">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="3d1e2-271">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="3d1e2-271">Shutdown Timeout</span></span>

<span data-ttu-id="3d1e2-272">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-272">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="3d1e2-273">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="3d1e2-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="3d1e2-274">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-274">**Type**: *int*</span></span>  
<span data-ttu-id="3d1e2-275">**既定値**:5</span><span class="sxs-lookup"><span data-stu-id="3d1e2-275">**Default**: 5</span></span>  
<span data-ttu-id="3d1e2-276">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="3d1e2-277">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="3d1e2-278">キーは `UseSetting` で *int* を受け取りますが (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など)、[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 拡張メソッドは [TimeSpan](/dotnet/api/system.timespan) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="3d1e2-279">タイムアウト期間中、ホスティングは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-279">During the timeout period, hosting:</span></span>

* <span data-ttu-id="3d1e2-280">[IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-280">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="3d1e2-281">ホステッド サービスの停止を試み、停止に失敗したサービスのエラーをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-281">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="3d1e2-282">すべてのホステッド サービスが停止する前にタイムアウト時間が切れた場合、残っているアクティブなサービスはアプリのシャットダウン時に停止します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-282">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="3d1e2-283">処理が完了していない場合でも、サービスは停止します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-283">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="3d1e2-284">サービスが停止するまでにさらに時間が必要な場合は、タイムアウト値を増やします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-284">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="3d1e2-285">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="3d1e2-285">Startup Assembly</span></span>

<span data-ttu-id="3d1e2-286">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-286">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="3d1e2-287">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="3d1e2-287">**Key**: startupAssembly</span></span>  
<span data-ttu-id="3d1e2-288">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-288">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-289">**既定値**:アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="3d1e2-289">**Default**: The app's assembly</span></span>  
<span data-ttu-id="3d1e2-290">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-290">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="3d1e2-291">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-291">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="3d1e2-292">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-292">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="3d1e2-293">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-293">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="3d1e2-294">Web ルート</span><span class="sxs-lookup"><span data-stu-id="3d1e2-294">Web Root</span></span>

<span data-ttu-id="3d1e2-295">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-295">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="3d1e2-296">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="3d1e2-296">**Key**: webroot</span></span>  
<span data-ttu-id="3d1e2-297">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="3d1e2-297">**Type**: *string*</span></span>  
<span data-ttu-id="3d1e2-298">**既定値**:指定されていない場合、既定値は "(コンテンツ ルート)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-298">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="3d1e2-299">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-299">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="3d1e2-300">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-300">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="3d1e2-301">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="3d1e2-301">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="3d1e2-302">構成をオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="3d1e2-302">Override configuration</span></span>

<span data-ttu-id="3d1e2-303">[Configuration](xref:fundamentals/configuration/index) を使用して、Web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-303">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="3d1e2-304">次の例では、ホスト構成はオプションで *hostsettings.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-304">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="3d1e2-305">*hostsettings.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-305">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="3d1e2-306">(`config` の) ビルド構成は、[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-306">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="3d1e2-307">`IWebHostBuilder` 構成はアプリの構成に追加されますが、その逆は当てはまりません&mdash;`ConfigureAppConfiguration` は `IWebHostBuilder` の構成に影響しません。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-307">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="3d1e2-308">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hostsettings.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-308">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            });
    }
}
```

<span data-ttu-id="3d1e2-309">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3d1e2-309">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="3d1e2-310">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-310">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3d1e2-311">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-311">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="3d1e2-312">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-312">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="3d1e2-313">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-313">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="3d1e2-314">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-314">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3d1e2-315">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-315">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="3d1e2-316">`UseConfiguration` は、指定された `IConfiguration` からホスト ビルダーの構成へのキーのみをコピーします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-316">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="3d1e2-317">そのため、JSON、INI、XML 設定のファイルに `reloadOnChange: true` を設定しても影響はありません。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-317">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="3d1e2-318">特定の URL でホストを実行するように指定する場合、[dotnet run](/dotnet/core/tools/dotnet-run) の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-318">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="3d1e2-319">コマンドライン引数は *hostsettings.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-319">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="3d1e2-320">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="3d1e2-320">Manage the host</span></span>

<span data-ttu-id="3d1e2-321">**実行**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-321">**Run**</span></span>

<span data-ttu-id="3d1e2-322">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-322">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="3d1e2-323">**[開始]**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-323">**Start**</span></span>

<span data-ttu-id="3d1e2-324">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-324">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="3d1e2-325">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-325">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="3d1e2-326">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-326">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="3d1e2-327">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-327">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="3d1e2-328">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-328">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="3d1e2-329">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-329">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3d1e2-330">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-330">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3d1e2-331">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-331">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3d1e2-332">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-332">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3d1e2-333">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-333">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="3d1e2-334">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-334">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3d1e2-335">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-335">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="3d1e2-336">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-336">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="3d1e2-337">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-337">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3d1e2-338">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-338">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="3d1e2-339">要求</span><span class="sxs-lookup"><span data-stu-id="3d1e2-339">Request</span></span>                                    | <span data-ttu-id="3d1e2-340">応答</span><span class="sxs-lookup"><span data-stu-id="3d1e2-340">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="3d1e2-341">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="3d1e2-341">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="3d1e2-342">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="3d1e2-342">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="3d1e2-343">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-343">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="3d1e2-344">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-344">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="3d1e2-345">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="3d1e2-345">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="3d1e2-346">Hello World!</span><span class="sxs-lookup"><span data-stu-id="3d1e2-346">Hello World!</span></span>                             |

<span data-ttu-id="3d1e2-347">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3d1e2-348">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3d1e2-349">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-349">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="3d1e2-350">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-350">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3d1e2-351">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action&lt;IRouteBuilder&gt; routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-351">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="3d1e2-352">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-352">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="3d1e2-353">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-353">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3d1e2-354">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-354">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3d1e2-355">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-355">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3d1e2-356">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-356">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3d1e2-357">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="3d1e2-357">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="3d1e2-358">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-358">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3d1e2-359">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action&lt;IApplicationBuilder&gt; app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-359">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="3d1e2-360">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="3d1e2-360">IHostingEnvironment interface</span></span>

<span data-ttu-id="3d1e2-361">[IHostingEnvironment インターフェイス](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-361">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="3d1e2-362">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-362">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="3d1e2-363">[規則ベースのアプローチ](xref:fundamentals/environments#environment-based-startup-class-and-methods)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-363">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="3d1e2-364">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-364">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="3d1e2-365">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-365">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="3d1e2-366">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-366">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="3d1e2-367">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-367">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="3d1e2-368">カスタムの[ミドルウェア](xref:fundamentals/middleware/write)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-368">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="3d1e2-369">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="3d1e2-369">IApplicationLifetime interface</span></span>

<span data-ttu-id="3d1e2-370">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-370">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="3d1e2-371">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-371">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="3d1e2-372">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="3d1e2-372">Cancellation Token</span></span>    | <span data-ttu-id="3d1e2-373">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="3d1e2-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="3d1e2-374">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="3d1e2-374">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="3d1e2-375">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-375">The host has fully started.</span></span> |
| [<span data-ttu-id="3d1e2-376">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="3d1e2-376">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="3d1e2-377">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-377">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="3d1e2-378">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-378">All requests should be processed.</span></span> <span data-ttu-id="3d1e2-379">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-379">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="3d1e2-380">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="3d1e2-380">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="3d1e2-381">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-381">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="3d1e2-382">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-382">Requests may still be processing.</span></span> <span data-ttu-id="3d1e2-383">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-383">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="3d1e2-384">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-384">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="3d1e2-385">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-385">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="3d1e2-386">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="3d1e2-386">Scope validation</span></span>

<span data-ttu-id="3d1e2-387">アプリの環境が開発の場合、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-387">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="3d1e2-388">`ValidateScopes` が `true` に設定されていると、既定のサービス プロバイダーはチェックを実行して次のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-388">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="3d1e2-389">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-389">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="3d1e2-390">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-390">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="3d1e2-391">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-391">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="3d1e2-392">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-392">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="3d1e2-393">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-393">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="3d1e2-394">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-394">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="3d1e2-395">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-395">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="3d1e2-396">運用環境も含めて、常にスコープを検証するには、ホスト ビルダー上の [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) で [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) を構成します。</span><span class="sxs-lookup"><span data-stu-id="3d1e2-396">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="3d1e2-397">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3d1e2-397">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
