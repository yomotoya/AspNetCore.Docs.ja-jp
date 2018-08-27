---
title: ASP.NET Core の Web ホスト
author: guardrex
description: ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: dfef2bf21f325f11d147379f75a8d81a8bd05eec
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2018
ms.locfileid: "41886747"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="8d1c1-103">ASP.NET Core の Web ホスト</span><span class="sxs-lookup"><span data-stu-id="8d1c1-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="8d1c1-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8d1c1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8d1c1-105">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="8d1c1-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8d1c1-107">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="8d1c1-108">このトピックでは、Web アプリをホストするのに便利な ASP.NET Core の Web ホスト ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) について説明します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="8d1c1-109">.NET での汎用ホスト ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) の対象範囲については、<xref:fundamentals/host/generic-host> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="8d1c1-110">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="8d1c1-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8d1c1-111">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="8d1c1-112">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="8d1c1-113">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="8d1c1-114">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="8d1c1-115">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="8d1c1-116">[Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、アプリのホスティング構成プロバイダーを使用してサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="8d1c1-117">Kestrel の既定のオプションについては、<xref:fundamentals/servers/kestrel#kestrel-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="8d1c1-118">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="8d1c1-119">次から[ ホスト構成](#host-configuration-values)を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="8d1c1-120">`ASPNETCORE_` のプレフィックスが付いた環境変数 (たとえば、`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="8d1c1-121">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-121">Command-line arguments.</span></span>
* <span data-ttu-id="8d1c1-122">次からアプリの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="8d1c1-123">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="8d1c1-124">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="8d1c1-125">エントリ アセンブリを使用して `Development` 環境でアプリが実行される場合に使用される[ユーザー シークレット](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="8d1c1-126">環境変数。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-126">Environment variables.</span></span>
  * <span data-ttu-id="8d1c1-127">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-127">Command-line arguments.</span></span>
* <span data-ttu-id="8d1c1-128">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="8d1c1-129">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="8d1c1-130">IIS の背後での実行時に、[IIS 統合](xref:host-and-deploy/iis/index)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="8d1c1-131">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)の使用時にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="8d1c1-132">このモジュールは、IIS と Kestrel の間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="8d1c1-133">また、[スタートアップ エラーをキャプチャする](#capture-startup-errors)ようにアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="8d1c1-134">IIS の既定のオプションについては、<xref:host-and-deploy/iis/index#iis-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="8d1c1-135">アプリの環境が開発の場合、[ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="8d1c1-136">詳しくは、「[スコープの検証](#scope-validation)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="8d1c1-137">`CreateDefaultBuilder` によって定義された構成は、[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)、そして [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のその他のメソッドと拡張メソッドによってオーバーライドされ、拡張されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="8d1c1-138">以下に、いくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-138">A few examples follow:</span></span>

* <span data-ttu-id="8d1c1-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) はアプリの追加の `IConfiguration` を指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="8d1c1-140">次の `ConfigureAppConfiguration` の呼び出しによりデリゲートが追加され、*appsettings.xml* ファイルにアプリの構成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="8d1c1-141">`ConfigureAppConfiguration` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="8d1c1-142">この構成はホスト (たとえば、サーバーの URL や環境など) には適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="8d1c1-143">「[ホストの構成値](#host-configuration-values)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="8d1c1-144">次の `ConfigureLogging` の呼び出しによりデリゲートが追加され、最小ログ記録レベル ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) が [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel) に構成されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="8d1c1-145">この設定により、`CreateDefaultBuilder` で構成された *appsettings.Development.json* (`LogLevel.Debug`) および *appsettings.Production.json* (`LogLevel.Error`) の設定がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="8d1c1-146">`ConfigureLogging` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="8d1c1-147">次の [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="8d1c1-148">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="8d1c1-149">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="8d1c1-150">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="8d1c1-151">アプリの構成の詳細については、<xref:fundamentals/configuration/index> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="8d1c1-152">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="8d1c1-153">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8d1c1-154">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-154">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="8d1c1-155">通常、ホストの作成はアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-155">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="8d1c1-156">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-156">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="8d1c1-157">`WebHostBuilder` には、[IServer を実装するサーバー](xref:fundamentals/servers/index)が必要です。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-157">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="8d1c1-158">組み込みサーバーは [Kestrel](xref:fundamentals/servers/kestrel) と [HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET Core 2.0 より前のリリースでは [WebListener](xref:fundamentals/servers/weblistener) と呼ばれていた) です。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-158">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="8d1c1-159">この例では、[UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)で Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-159">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="8d1c1-160">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-160">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="8d1c1-161">`UseContentRoot` の既定のコンテンツ ルートは、[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-161">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="8d1c1-162">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-162">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="8d1c1-163">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-163">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="8d1c1-164">IIS をリバース プロキシとして使用するには、ホストのビルド時に [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-164">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="8d1c1-165">`UseIISIntegration` では、[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) の場合のように*サーバー*は構成されません。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-165">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="8d1c1-166">`UseIISIntegration` は、Kestrel と IIS の間にリバース プロキシを作成するために [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を使用する際にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-166">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="8d1c1-167">IIS と ASP.NET Core を一緒に使用するには、`UseKestrel` と `UseIISIntegration` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-167">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="8d1c1-168">`UseIISIntegration` は、IIS または IIS Express の背後で実行されている場合にのみアクティブになります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-168">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="8d1c1-169">詳細については、<xref:fundamentals/servers/aspnet-core-module> および <xref:host-and-deploy/aspnet-core-module> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-169">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="8d1c1-170">ホスト (および ASP.NET Core アプリ) を構成する最小限の実装には、アプリの要求パイプラインの構成とサーバーの指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-170">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

::: moniker-end

<span data-ttu-id="8d1c1-171">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-171">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="8d1c1-172">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-172">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="8d1c1-173">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-173">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="8d1c1-174">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-174">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="8d1c1-175">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-175">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="8d1c1-176">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="8d1c1-176">Host configuration values</span></span>

<span data-ttu-id="8d1c1-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="8d1c1-178">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-178">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="8d1c1-179">たとえば、`ASPNETCORE_ENVIRONMENT` のようにします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-179">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="8d1c1-180">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) や [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) などの拡張機能 (「[構成をオーバーライドする](#override-configuration)」のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-180">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="8d1c1-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="8d1c1-182">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-182">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="8d1c1-183">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-183">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="8d1c1-184">詳しくは、次のセクション「[構成をオーバーライドする](#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-184">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="8d1c1-185">アプリケーション キー (名前)</span><span class="sxs-lookup"><span data-stu-id="8d1c1-185">Application Key (Name)</span></span>

<span data-ttu-id="8d1c1-186">ホストの構築時に [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) または [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) が呼び出されると、[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) プロパティが自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-186">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="8d1c1-187">この値はアプリのエントリ ポイントを含むアセンブリの名前に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-187">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="8d1c1-188">値を明示的に設定するには、[WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) を使用します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-188">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="8d1c1-189">**キー**: applicationName</span><span class="sxs-lookup"><span data-stu-id="8d1c1-189">**Key**: applicationName</span></span>  
<span data-ttu-id="8d1c1-190">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-190">**Type**: *string*</span></span>  
<span data-ttu-id="8d1c1-191">**既定**: アプリのエントリ ポイントを含むアセンブリの名前。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-191">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="8d1c1-192">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-192">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8d1c1-193">**環境変数**: `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-193">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="8d1c1-194">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="8d1c1-194">Capture Startup Errors</span></span>

<span data-ttu-id="8d1c1-195">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-195">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="8d1c1-196">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="8d1c1-196">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="8d1c1-197">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="8d1c1-197">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8d1c1-198">**既定値**: アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-198">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="8d1c1-199">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-199">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="8d1c1-200">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-200">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="8d1c1-201">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-201">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="8d1c1-202">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-202">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="8d1c1-203">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="8d1c1-203">Content Root</span></span>

<span data-ttu-id="8d1c1-204">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-204">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="8d1c1-205">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="8d1c1-205">**Key**: contentRoot</span></span>  
<span data-ttu-id="8d1c1-206">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-206">**Type**: *string*</span></span>  
<span data-ttu-id="8d1c1-207">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-207">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="8d1c1-208">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-208">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="8d1c1-209">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-209">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="8d1c1-210">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-210">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="8d1c1-211">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-211">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="8d1c1-212">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="8d1c1-212">Detailed Errors</span></span>

<span data-ttu-id="8d1c1-213">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-213">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="8d1c1-214">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="8d1c1-214">**Key**: detailedErrors</span></span>  
<span data-ttu-id="8d1c1-215">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="8d1c1-215">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8d1c1-216">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="8d1c1-216">**Default**: false</span></span>  
<span data-ttu-id="8d1c1-217">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-217">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8d1c1-218">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-218">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="8d1c1-219">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-219">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="8d1c1-220">環境</span><span class="sxs-lookup"><span data-stu-id="8d1c1-220">Environment</span></span>

<span data-ttu-id="8d1c1-221">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-221">Sets the app's environment.</span></span>

<span data-ttu-id="8d1c1-222">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="8d1c1-222">**Key**: environment</span></span>  
<span data-ttu-id="8d1c1-223">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-223">**Type**: *string*</span></span>  
<span data-ttu-id="8d1c1-224">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="8d1c1-224">**Default**: Production</span></span>  
<span data-ttu-id="8d1c1-225">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-225">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="8d1c1-226">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-226">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="8d1c1-227">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-227">The environment can be set to any value.</span></span> <span data-ttu-id="8d1c1-228">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-228">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8d1c1-229">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-229">Values aren't case sensitive.</span></span> <span data-ttu-id="8d1c1-230">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-230">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="8d1c1-231">[Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-231">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="8d1c1-232">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-232">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="8d1c1-233">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="8d1c1-233">Hosting Startup Assemblies</span></span>

<span data-ttu-id="8d1c1-234">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-234">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="8d1c1-235">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="8d1c1-235">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="8d1c1-236">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-236">**Type**: *string*</span></span>  
<span data-ttu-id="8d1c1-237">**既定値**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="8d1c1-237">**Default**: Empty string</span></span>  
<span data-ttu-id="8d1c1-238">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8d1c1-239">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="8d1c1-240">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-240">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="8d1c1-241">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-241">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="8d1c1-242">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-242">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="8d1c1-243">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="8d1c1-243">HTTPS Port</span></span>

<span data-ttu-id="8d1c1-244">HTTPS リダイレクト ポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-244">Set the HTTPS redirect port.</span></span> <span data-ttu-id="8d1c1-245">[HTTPS の適用](xref:security/enforcing-ssl)に使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-245">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="8d1c1-246">**キー**: https_port **型**: *string*
**既定値**: 既定値は設定されません。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-246">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="8d1c1-247">**次を使用して設定**: `UseSetting`
**環境変数**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-247">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="8d1c1-248">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="8d1c1-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="8d1c1-249">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="8d1c1-250">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="8d1c1-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="8d1c1-251">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="8d1c1-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8d1c1-252">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="8d1c1-252">**Default**: true</span></span>  
<span data-ttu-id="8d1c1-253">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="8d1c1-254">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="8d1c1-255">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="8d1c1-255">Prevent Hosting Startup</span></span>

<span data-ttu-id="8d1c1-256">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-256">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="8d1c1-257">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-257">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="8d1c1-258">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="8d1c1-258">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="8d1c1-259">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="8d1c1-259">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8d1c1-260">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="8d1c1-260">**Default**: false</span></span>  
<span data-ttu-id="8d1c1-261">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-261">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="8d1c1-262">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-262">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="8d1c1-263">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="8d1c1-263">Server URLs</span></span>

<span data-ttu-id="8d1c1-264">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-264">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="8d1c1-265">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="8d1c1-265">**Key**: urls</span></span>  
<span data-ttu-id="8d1c1-266">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-266">**Type**: *string*</span></span>  
<span data-ttu-id="8d1c1-267">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="8d1c1-267">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="8d1c1-268">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-268">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="8d1c1-269">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-269">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="8d1c1-270">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-270">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="8d1c1-271">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-271">For example, `http://localhost:123`.</span></span> <span data-ttu-id="8d1c1-272">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-272">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="8d1c1-273">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-273">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="8d1c1-274">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-274">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="8d1c1-275">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-275">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="8d1c1-276">詳細については、「<xref:fundamentals/servers/kestrel#endpoint-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-276">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="8d1c1-277">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="8d1c1-277">Shutdown Timeout</span></span>

<span data-ttu-id="8d1c1-278">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-278">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="8d1c1-279">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="8d1c1-279">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="8d1c1-280">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-280">**Type**: *int*</span></span>  
<span data-ttu-id="8d1c1-281">**既定値**: 5</span><span class="sxs-lookup"><span data-stu-id="8d1c1-281">**Default**: 5</span></span>  
<span data-ttu-id="8d1c1-282">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-282">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="8d1c1-283">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-283">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="8d1c1-284">キーは `UseSetting` で *int* を受け取りますが (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など)、[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 拡張メソッドは [TimeSpan](/dotnet/api/system.timespan) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-284">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="8d1c1-285">タイムアウト期間中、ホスティングは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-285">During the timeout period, hosting:</span></span>

* <span data-ttu-id="8d1c1-286">[IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-286">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="8d1c1-287">ホステッド サービスの停止を試み、停止に失敗したサービスのエラーをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-287">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="8d1c1-288">すべてのホステッド サービスが停止する前にタイムアウト時間が切れた場合、残っているアクティブなサービスはアプリのシャットダウン時に停止します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-288">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="8d1c1-289">処理が完了していない場合でも、サービスは停止します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-289">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="8d1c1-290">サービスが停止するまでにさらに時間が必要な場合は、タイムアウト値を増やします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-290">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="8d1c1-291">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="8d1c1-291">Startup Assembly</span></span>

<span data-ttu-id="8d1c1-292">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="8d1c1-293">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="8d1c1-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="8d1c1-294">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-294">**Type**: *string*</span></span>  
<span data-ttu-id="8d1c1-295">**既定値**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="8d1c1-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="8d1c1-296">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="8d1c1-297">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="8d1c1-298">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="8d1c1-299">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="8d1c1-300">Web ルート</span><span class="sxs-lookup"><span data-stu-id="8d1c1-300">Web Root</span></span>

<span data-ttu-id="8d1c1-301">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="8d1c1-302">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="8d1c1-302">**Key**: webroot</span></span>  
<span data-ttu-id="8d1c1-303">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8d1c1-303">**Type**: *string*</span></span>  
<span data-ttu-id="8d1c1-304">**既定値**: 指定されていない場合、既定値は "(Content Root)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="8d1c1-305">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="8d1c1-306">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="8d1c1-307">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="8d1c1-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="8d1c1-308">構成をオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="8d1c1-308">Override configuration</span></span>

<span data-ttu-id="8d1c1-309">[Configuration](xref:fundamentals/configuration/index) を使用して、Web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-309">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="8d1c1-310">次の例では、ホスト構成はオプションで *hostsettings.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-310">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="8d1c1-311">*hostsettings.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-311">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="8d1c1-312">(`config` の) ビルド構成は、[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-312">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="8d1c1-313">`IWebHostBuilder` 構成はアプリの構成に追加されますが、その逆は当てはまりません&mdash;`ConfigureAppConfiguration` は `IWebHostBuilder` の構成に影響しません。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-313">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8d1c1-314">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hostsettings.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-314">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            })
            .Build();
    }
}
```

<span data-ttu-id="8d1c1-315">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8d1c1-315">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8d1c1-316">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hostsettings.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-316">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

<span data-ttu-id="8d1c1-317">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8d1c1-317">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="8d1c1-318">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-318">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="8d1c1-319">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-319">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="8d1c1-320">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-320">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="8d1c1-321">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-321">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="8d1c1-322">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-322">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="8d1c1-323">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-323">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="8d1c1-324">`UseConfiguration` は、指定された `IConfiguration` からホスト ビルダーの構成へのキーのみをコピーします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-324">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="8d1c1-325">そのため、JSON、INI、XML 設定のファイルに `reloadOnChange: true` を設定しても影響はありません。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-325">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="8d1c1-326">特定の URL でホストを実行するように指定する場合、[dotnet run](/dotnet/core/tools/dotnet-run) の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-326">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="8d1c1-327">コマンドライン引数は *hostsettings.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-327">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="8d1c1-328">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="8d1c1-328">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8d1c1-329">**実行**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-329">**Run**</span></span>

<span data-ttu-id="8d1c1-330">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-330">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="8d1c1-331">**Start**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-331">**Start**</span></span>

<span data-ttu-id="8d1c1-332">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-332">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="8d1c1-333">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-333">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="8d1c1-334">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-334">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="8d1c1-335">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-335">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="8d1c1-336">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-336">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="8d1c1-337">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-337">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="8d1c1-338">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-338">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="8d1c1-339">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-339">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8d1c1-340">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-340">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8d1c1-341">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-341">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="8d1c1-342">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-342">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="8d1c1-343">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-343">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="8d1c1-344">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-344">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="8d1c1-345">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-345">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="8d1c1-346">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-346">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="8d1c1-347">要求</span><span class="sxs-lookup"><span data-stu-id="8d1c1-347">Request</span></span>                                    | <span data-ttu-id="8d1c1-348">応答</span><span class="sxs-lookup"><span data-stu-id="8d1c1-348">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="8d1c1-349">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="8d1c1-349">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="8d1c1-350">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="8d1c1-350">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="8d1c1-351">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-351">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="8d1c1-352">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-352">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="8d1c1-353">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="8d1c1-353">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="8d1c1-354">Hello World!</span><span class="sxs-lookup"><span data-stu-id="8d1c1-354">Hello World!</span></span>                             |

<span data-ttu-id="8d1c1-355">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-355">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8d1c1-356">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-356">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8d1c1-357">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-357">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="8d1c1-358">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-358">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="8d1c1-359">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action&lt;IRouteBuilder&gt; routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-359">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="8d1c1-360">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-360">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="8d1c1-361">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-361">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="8d1c1-362">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-362">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="8d1c1-363">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-363">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8d1c1-364">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-364">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8d1c1-365">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-365">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="8d1c1-366">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-366">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="8d1c1-367">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action&lt;IApplicationBuilder&gt; app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-367">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8d1c1-368">**実行**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-368">**Run**</span></span>

<span data-ttu-id="8d1c1-369">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-369">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="8d1c1-370">**Start**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-370">**Start**</span></span>

<span data-ttu-id="8d1c1-371">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-371">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="8d1c1-372">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-372">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="8d1c1-373">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="8d1c1-373">IHostingEnvironment interface</span></span>

<span data-ttu-id="8d1c1-374">[IHostingEnvironment インターフェイス](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-374">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="8d1c1-375">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-375">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="8d1c1-376">[規則ベースのアプローチ](xref:fundamentals/environments#environment-based-startup-class-and-methods)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-376">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="8d1c1-377">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-377">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="8d1c1-378">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-378">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="8d1c1-379">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-379">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="8d1c1-380">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-380">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
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

<span data-ttu-id="8d1c1-381">カスタムの[ミドルウェア](xref:fundamentals/middleware/index#write-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-381">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="8d1c1-382">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="8d1c1-382">IApplicationLifetime interface</span></span>

<span data-ttu-id="8d1c1-383">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-383">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="8d1c1-384">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-384">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="8d1c1-385">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="8d1c1-385">Cancellation Token</span></span>    | <span data-ttu-id="8d1c1-386">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="8d1c1-386">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="8d1c1-387">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="8d1c1-387">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="8d1c1-388">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-388">The host has fully started.</span></span> |
| [<span data-ttu-id="8d1c1-389">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="8d1c1-389">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="8d1c1-390">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-390">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="8d1c1-391">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-391">All requests should be processed.</span></span> <span data-ttu-id="8d1c1-392">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-392">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="8d1c1-393">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="8d1c1-393">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="8d1c1-394">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-394">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="8d1c1-395">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-395">Requests may still be processing.</span></span> <span data-ttu-id="8d1c1-396">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="8d1c1-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="8d1c1-398">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-398">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="8d1c1-399">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="8d1c1-399">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8d1c1-400">アプリの環境が開発の場合、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-400">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="8d1c1-401">`ValidateScopes` が `true` に設定されていると、既定のサービス プロバイダーはチェックを実行して次のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="8d1c1-402">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="8d1c1-403">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="8d1c1-404">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="8d1c1-405">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="8d1c1-406">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="8d1c1-407">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="8d1c1-408">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="8d1c1-409">運用環境も含めて、常にスコープを検証するには、ホスト ビルダー上の [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) で [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) を構成します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="8d1c1-410">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="8d1c1-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="8d1c1-411">**アプリが `UseStartup` または `Configure` の呼び出しを行わない場合、次は ASP.NET Core 2.0 アプリにのみ適用されます。**</span><span class="sxs-lookup"><span data-stu-id="8d1c1-411">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="8d1c1-412">`UseStartup` や `Configure` を呼び出すのではなく、依存関係挿入コンテナーに直接 `IStartup` を挿入して、ホストをビルドする場合があります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="8d1c1-413">ホストがこのようにビルドされている場合、以下のエラーが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="8d1c1-414">これは、`HostingStartupAttributes` をスキャンするためにアプリ名 (現在のアセンブリの名前) が必要であるために発生します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-414">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="8d1c1-415">アプリで `IStartup` を手動で依存関係挿入コンテナーに挿入する場合は、指定されたアセンブリ名を使用して `WebHostBuilder` への次の呼び出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="8d1c1-416">あるいは、ダミーの `Configure` を `WebHostBuilder` に追加します。この場合、アプリ名は自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="8d1c1-417">詳細については、[お知らせのコメント「Microsoft.Extensions.PlatformAbstractions has been removed」](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Microsoft.Extensions.PlatformAbstractions が削除される) と [StartupInjection のサンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8d1c1-417">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8d1c1-418">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8d1c1-418">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
