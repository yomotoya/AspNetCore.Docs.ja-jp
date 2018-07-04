---
title: ASP.NET Core の Web ホスト
author: guardrex
description: ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 98070f49c98919e7ebff41ecc69c953249977dcc
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314150"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="a23f3-103">ASP.NET Core の Web ホスト</span><span class="sxs-lookup"><span data-stu-id="a23f3-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="a23f3-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a23f3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a23f3-105">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="a23f3-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a23f3-107">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="a23f3-108">このトピックでは、Web アプリをホストするのに便利な ASP.NET Core の Web ホスト ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) について説明します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="a23f3-109">.NET 汎用 Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) の対象範囲については、[汎用ホスト](xref:fundamentals/host/generic-host)に関するトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="a23f3-110">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="a23f3-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a23f3-112">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="a23f3-113">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="a23f3-114">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="a23f3-115">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="a23f3-116">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="a23f3-117">[Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、アプリのホスティング構成プロバイダーを使用してサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="a23f3-118">Kestrel の既定のオプションについては、[「ASP.NET Core の Kestrel Web サーバーの実装の概要」の「Kestrel オプション」セクション](xref:fundamentals/servers/kestrel#kestrel-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="a23f3-119">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="a23f3-120">次から[ ホスト構成](#host-configuration-values)を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="a23f3-121">`ASPNETCORE_` のプレフィックスが付いた環境変数 (たとえば、`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="a23f3-122">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="a23f3-122">Command-line arguments.</span></span>
* <span data-ttu-id="a23f3-123">次からアプリの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-123">Loads app configuration from:</span></span>
  * <span data-ttu-id="a23f3-124">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="a23f3-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="a23f3-125">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="a23f3-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="a23f3-126">エントリ アセンブリを使用して `Development` 環境でアプリが実行される場合に使用される[ユーザー シークレット](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-126">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a23f3-127">環境変数。</span><span class="sxs-lookup"><span data-stu-id="a23f3-127">Environment variables.</span></span>
  * <span data-ttu-id="a23f3-128">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="a23f3-128">Command-line arguments.</span></span>
* <span data-ttu-id="a23f3-129">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="a23f3-130">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="a23f3-131">IIS の背後での実行時に、[IIS 統合](xref:host-and-deploy/iis/index)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="a23f3-132">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)の使用時にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="a23f3-133">このモジュールは、IIS と Kestrel の間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="a23f3-134">また、[スタートアップ エラーをキャプチャする](#capture-startup-errors)ようにアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="a23f3-135">IIS の既定のオプションについては、[「IIS を使用した Windows での ASP.NET Core のホスト」の「IIS のオプション」セクション](xref:host-and-deploy/iis/index#iis-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-135">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="a23f3-136">アプリの環境が開発の場合、[ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="a23f3-137">詳しくは、「[スコープの検証](#scope-validation)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="a23f3-138">`CreateDefaultBuilder` によって定義された構成は、[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)、そして [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のその他のメソッドと拡張メソッドによってオーバーライドされ、拡張されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="a23f3-139">以下に、いくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-139">A few examples follow:</span></span>

* <span data-ttu-id="a23f3-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) はアプリの追加の `IConfiguration` を指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="a23f3-141">次の `ConfigureAppConfiguration` の呼び出しによりデリゲートが追加され、*appsettings.xml* ファイルにアプリの構成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="a23f3-142">`ConfigureAppConfiguration` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="a23f3-143">この構成はホスト (たとえば、サーバーの URL や環境など) には適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="a23f3-144">「[ホストの構成値](#host-configuration-values)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="a23f3-145">次の `ConfigureLogging` の呼び出しによりデリゲートが追加され、最小ログ記録レベル ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) が [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel) に構成されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="a23f3-146">この設定により、`CreateDefaultBuilder` で構成された *appsettings.Development.json* (`LogLevel.Debug`) および *appsettings.Production.json* (`LogLevel.Error`) の設定がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a23f3-147">`ConfigureLogging` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="a23f3-148">次の [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="a23f3-149">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="a23f3-150">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="a23f3-151">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="a23f3-152">アプリの構成の詳細については、「[ASP.NET Core の構成](xref:fundamentals/configuration/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-152">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="a23f3-153">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="a23f3-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="a23f3-154">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-155">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-155">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a23f3-156">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-156">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="a23f3-157">通常、ホストの作成はアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-157">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="a23f3-158">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-158">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="a23f3-159">`WebHostBuilder` には、[IServer を実装するサーバー](xref:fundamentals/servers/index)が必要です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-159">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="a23f3-160">組み込みサーバーは [Kestrel](xref:fundamentals/servers/kestrel) と [HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET Core 2.0 より前のリリースでは [WebListener](xref:fundamentals/servers/weblistener) と呼ばれていた) です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-160">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="a23f3-161">この例では、[UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)で Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-161">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="a23f3-162">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-162">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="a23f3-163">`UseContentRoot` の既定のコンテンツ ルートは、[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-163">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="a23f3-164">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-164">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="a23f3-165">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-165">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="a23f3-166">IIS をリバース プロキシとして使用するには、ホストのビルド時に [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-166">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="a23f3-167">`UseIISIntegration` では、[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) の場合のように*サーバー*は構成されません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-167">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="a23f3-168">`UseIISIntegration` は、Kestrel と IIS の間にリバース プロキシを作成するために [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を使用する際にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-168">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="a23f3-169">IIS と ASP.NET Core を一緒に使用するには、`UseKestrel` と `UseIISIntegration` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-169">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="a23f3-170">`UseIISIntegration` は、IIS または IIS Express の背後で実行されている場合にのみアクティブになります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-170">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="a23f3-171">詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-171">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="a23f3-172">ホスト (および ASP.NET Core アプリ) を構成する最小限の実装には、アプリの要求パイプラインの構成とサーバーの指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-172">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

---

<span data-ttu-id="a23f3-173">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-173">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="a23f3-174">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-174">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="a23f3-175">詳細については、「[ASP.NET Core でのアプリケーションのスタートアップ](xref:fundamentals/startup)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-175">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="a23f3-176">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-176">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="a23f3-177">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-177">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="a23f3-178">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="a23f3-178">Host configuration values</span></span>

<span data-ttu-id="a23f3-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="a23f3-180">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="a23f3-180">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="a23f3-181">たとえば、`ASPNETCORE_ENVIRONMENT` のようにします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-181">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="a23f3-182">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) や [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) などの拡張機能 (「[構成をオーバーライドする](#override-configuration)」のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-182">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="a23f3-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="a23f3-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="a23f3-184">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-184">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="a23f3-185">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-185">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="a23f3-186">詳しくは、次のセクション「[構成をオーバーライドする](#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-186">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="a23f3-187">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="a23f3-187">Capture Startup Errors</span></span>

<span data-ttu-id="a23f3-188">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-188">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="a23f3-189">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="a23f3-189">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="a23f3-190">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="a23f3-190">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a23f3-191">**既定値**: アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-191">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="a23f3-192">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="a23f3-192">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="a23f3-193">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="a23f3-193">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="a23f3-194">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-194">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="a23f3-195">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-195">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="a23f3-198">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="a23f3-198">Content Root</span></span>

<span data-ttu-id="a23f3-199">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-199">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="a23f3-200">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="a23f3-200">**Key**: contentRoot</span></span>  
<span data-ttu-id="a23f3-201">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="a23f3-201">**Type**: *string*</span></span>  
<span data-ttu-id="a23f3-202">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-202">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="a23f3-203">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="a23f3-203">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="a23f3-204">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="a23f3-204">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="a23f3-205">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-205">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="a23f3-206">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-206">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="a23f3-209">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="a23f3-209">Detailed Errors</span></span>

<span data-ttu-id="a23f3-210">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-210">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="a23f3-211">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="a23f3-211">**Key**: detailedErrors</span></span>  
<span data-ttu-id="a23f3-212">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="a23f3-212">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a23f3-213">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="a23f3-213">**Default**: false</span></span>  
<span data-ttu-id="a23f3-214">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a23f3-214">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a23f3-215">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="a23f3-215">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="a23f3-216">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-216">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="a23f3-219">環境</span><span class="sxs-lookup"><span data-stu-id="a23f3-219">Environment</span></span>

<span data-ttu-id="a23f3-220">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-220">Sets the app's environment.</span></span>

<span data-ttu-id="a23f3-221">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="a23f3-221">**Key**: environment</span></span>  
<span data-ttu-id="a23f3-222">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="a23f3-222">**Type**: *string*</span></span>  
<span data-ttu-id="a23f3-223">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="a23f3-223">**Default**: Production</span></span>  
<span data-ttu-id="a23f3-224">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="a23f3-224">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="a23f3-225">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="a23f3-225">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="a23f3-226">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-226">The environment can be set to any value.</span></span> <span data-ttu-id="a23f3-227">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-227">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="a23f3-228">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-228">Values aren't case sensitive.</span></span> <span data-ttu-id="a23f3-229">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-229">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a23f3-230">[Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-230">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="a23f3-231">詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-231">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-232">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-232">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="a23f3-234">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="a23f3-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="a23f3-235">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="a23f3-236">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="a23f3-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="a23f3-237">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="a23f3-237">**Type**: *string*</span></span>  
<span data-ttu-id="a23f3-238">**既定値**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="a23f3-238">**Default**: Empty string</span></span>  
<span data-ttu-id="a23f3-239">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a23f3-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a23f3-240">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="a23f3-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="a23f3-241">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="a23f3-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="a23f3-242">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-242">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="a23f3-243">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-243">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="a23f3-244">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-244">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-245">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-245">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-246">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-246">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23f3-247">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-247">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="a23f3-248">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="a23f3-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="a23f3-249">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="a23f3-250">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="a23f3-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="a23f3-251">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="a23f3-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a23f3-252">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="a23f3-252">**Default**: true</span></span>  
<span data-ttu-id="a23f3-253">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="a23f3-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="a23f3-254">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="a23f3-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="a23f3-255">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-255">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-256">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-256">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23f3-258">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-258">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="a23f3-259">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="a23f3-259">Prevent Hosting Startup</span></span>

<span data-ttu-id="a23f3-260">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-260">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="a23f3-261">詳しくは、「[Enhance an app from an external assembly in ASP.NET Core with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)」(ASP.NET Core 内で IHostingStartup を使用して外部アセンブリからアプリを拡張する) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-261">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="a23f3-262">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="a23f3-262">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="a23f3-263">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="a23f3-263">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a23f3-264">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="a23f3-264">**Default**: false</span></span>  
<span data-ttu-id="a23f3-265">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a23f3-265">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a23f3-266">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="a23f3-266">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="a23f3-267">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-267">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-268">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-268">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23f3-270">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-270">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="a23f3-271">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="a23f3-271">Server URLs</span></span>

<span data-ttu-id="a23f3-272">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="a23f3-273">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="a23f3-273">**Key**: urls</span></span>  
<span data-ttu-id="a23f3-274">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="a23f3-274">**Type**: *string*</span></span>  
<span data-ttu-id="a23f3-275">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="a23f3-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="a23f3-276">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="a23f3-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="a23f3-277">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="a23f3-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="a23f3-278">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="a23f3-279">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="a23f3-280">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="a23f3-281">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="a23f3-282">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-282">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-283">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="a23f3-284">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="a23f3-285">詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-285">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-286">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-286">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="a23f3-287">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="a23f3-287">Shutdown Timeout</span></span>

<span data-ttu-id="a23f3-288">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-288">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="a23f3-289">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="a23f3-289">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="a23f3-290">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="a23f3-290">**Type**: *int*</span></span>  
<span data-ttu-id="a23f3-291">**既定値**: 5</span><span class="sxs-lookup"><span data-stu-id="a23f3-291">**Default**: 5</span></span>  
<span data-ttu-id="a23f3-292">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="a23f3-292">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="a23f3-293">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="a23f3-293">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="a23f3-294">キーは `UseSetting` で *int* を受け取りますが (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など)、[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 拡張メソッドは [TimeSpan](/dotnet/api/system.timespan) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-294">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="a23f3-295">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-295">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="a23f3-296">タイムアウト期間中、ホスティングは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="a23f3-296">During the timeout period, hosting:</span></span>

* <span data-ttu-id="a23f3-297">[IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-297">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="a23f3-298">ホステッド サービスの停止を試み、停止に失敗したサービスのエラーをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-298">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="a23f3-299">すべてのホステッド サービスが停止する前にタイムアウト時間が切れた場合、残っているアクティブなサービスはアプリのシャットダウン時に停止します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-299">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="a23f3-300">処理が完了していない場合でも、サービスは停止します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-300">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="a23f3-301">サービスが停止するまでにさらに時間が必要な場合は、タイムアウト値を増やします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-301">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-302">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-302">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23f3-304">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-304">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="a23f3-305">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="a23f3-305">Startup Assembly</span></span>

<span data-ttu-id="a23f3-306">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-306">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="a23f3-307">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="a23f3-307">**Key**: startupAssembly</span></span>  
<span data-ttu-id="a23f3-308">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="a23f3-308">**Type**: *string*</span></span>  
<span data-ttu-id="a23f3-309">**既定値**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="a23f3-309">**Default**: The app's assembly</span></span>  
<span data-ttu-id="a23f3-310">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="a23f3-310">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="a23f3-311">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="a23f3-311">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="a23f3-312">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-312">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="a23f3-313">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-313">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="a23f3-316">Web ルート</span><span class="sxs-lookup"><span data-stu-id="a23f3-316">Web Root</span></span>

<span data-ttu-id="a23f3-317">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-317">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="a23f3-318">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="a23f3-318">**Key**: webroot</span></span>  
<span data-ttu-id="a23f3-319">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="a23f3-319">**Type**: *string*</span></span>  
<span data-ttu-id="a23f3-320">**既定値**: 指定されていない場合、既定値は "(Content Root)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-320">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="a23f3-321">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-321">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="a23f3-322">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="a23f3-322">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="a23f3-323">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="a23f3-323">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-324">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-324">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="a23f3-326">構成をオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="a23f3-326">Override configuration</span></span>

<span data-ttu-id="a23f3-327">[Configuration](xref:fundamentals/configuration/index) を使用して、Web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-327">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="a23f3-328">次の例では、ホスト構成はオプションで *hostsettings.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-328">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="a23f3-329">*hostsettings.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-329">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="a23f3-330">(`config` の) ビルド構成は、[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-330">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="a23f3-331">`IWebHostBuilder` 構成はアプリの構成に追加されますが、その逆は当てはまりません&mdash;`ConfigureAppConfiguration` は `IWebHostBuilder` の構成に影響しません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-331">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a23f3-333">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hostsettings.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-333">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="a23f3-334">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a23f3-334">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-335">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-335">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23f3-336">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hostsettings.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-336">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="a23f3-337">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a23f3-337">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

---

> [!NOTE]
> <span data-ttu-id="a23f3-338">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-338">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a23f3-339">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-339">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="a23f3-340">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="a23f3-340">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="a23f3-341">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-341">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="a23f3-342">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="a23f3-342">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a23f3-343">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-343">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="a23f3-344">`UseConfiguration` は、指定された `IConfiguration` からホスト ビルダーの構成へのキーのみをコピーします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-344">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="a23f3-345">そのため、JSON、INI、XML 設定のファイルに `reloadOnChange: true` を設定しても影響はありません。</span><span class="sxs-lookup"><span data-stu-id="a23f3-345">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="a23f3-346">特定の URL でホストを実行するように指定する場合、[dotnet run](/dotnet/core/tools/dotnet-run) の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-346">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="a23f3-347">コマンドライン引数は *hostsettings.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-347">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="a23f3-348">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="a23f3-348">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a23f3-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a23f3-350">**実行**</span><span class="sxs-lookup"><span data-stu-id="a23f3-350">**Run**</span></span>

<span data-ttu-id="a23f3-351">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-351">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="a23f3-352">**Start**</span><span class="sxs-lookup"><span data-stu-id="a23f3-352">**Start**</span></span>

<span data-ttu-id="a23f3-353">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-353">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="a23f3-354">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-354">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="a23f3-355">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-355">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="a23f3-356">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-356">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="a23f3-357">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="a23f3-357">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="a23f3-358">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-358">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a23f3-359">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="a23f3-359">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a23f3-360">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-360">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a23f3-361">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-361">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a23f3-362">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="a23f3-362">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="a23f3-363">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-363">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a23f3-364">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-364">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="a23f3-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a23f3-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="a23f3-366">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-366">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="a23f3-367">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-367">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="a23f3-368">要求</span><span class="sxs-lookup"><span data-stu-id="a23f3-368">Request</span></span>                                    | <span data-ttu-id="a23f3-369">応答</span><span class="sxs-lookup"><span data-stu-id="a23f3-369">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="a23f3-370">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="a23f3-370">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="a23f3-371">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="a23f3-371">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="a23f3-372">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-372">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="a23f3-373">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-373">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="a23f3-374">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="a23f3-374">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="a23f3-375">Hello World!</span><span class="sxs-lookup"><span data-stu-id="a23f3-375">Hello World!</span></span>                             |

<span data-ttu-id="a23f3-376">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-376">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a23f3-377">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-377">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a23f3-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a23f3-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="a23f3-379">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-379">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="a23f3-380">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action&lt;IRouteBuilder&gt; routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-380">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="a23f3-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="a23f3-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="a23f3-382">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-382">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="a23f3-383">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="a23f3-383">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a23f3-384">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-384">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a23f3-385">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-385">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a23f3-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="a23f3-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="a23f3-387">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-387">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="a23f3-388">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action&lt;IApplicationBuilder&gt; app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-388">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a23f3-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a23f3-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a23f3-390">**実行**</span><span class="sxs-lookup"><span data-stu-id="a23f3-390">**Run**</span></span>

<span data-ttu-id="a23f3-391">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-391">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="a23f3-392">**Start**</span><span class="sxs-lookup"><span data-stu-id="a23f3-392">**Start**</span></span>

<span data-ttu-id="a23f3-393">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-393">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="a23f3-394">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-394">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="a23f3-395">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="a23f3-395">IHostingEnvironment interface</span></span>

<span data-ttu-id="a23f3-396">[IHostingEnvironment インターフェイス](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-396">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="a23f3-397">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-397">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="a23f3-398">[規則ベースのアプローチ](xref:fundamentals/environments#environment-based-startup-class-and-methods)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-398">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="a23f3-399">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-399">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="a23f3-400">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-400">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="a23f3-401">詳細については、[複数の環境での使用](xref:fundamentals/environments)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-401">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="a23f3-402">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-402">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="a23f3-403">カスタムの[ミドルウェア](xref:fundamentals/middleware/index#writing-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-403">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="a23f3-404">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="a23f3-404">IApplicationLifetime interface</span></span>

<span data-ttu-id="a23f3-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="a23f3-406">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="a23f3-406">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="a23f3-407">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="a23f3-407">Cancellation Token</span></span>    | <span data-ttu-id="a23f3-408">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="a23f3-408">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="a23f3-409">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="a23f3-409">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="a23f3-410">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="a23f3-410">The host has fully started.</span></span> |
| [<span data-ttu-id="a23f3-411">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="a23f3-411">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="a23f3-412">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="a23f3-412">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="a23f3-413">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-413">All requests should be processed.</span></span> <span data-ttu-id="a23f3-414">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-414">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="a23f3-415">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="a23f3-415">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="a23f3-416">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="a23f3-416">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="a23f3-417">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="a23f3-417">Requests may still be processing.</span></span> <span data-ttu-id="a23f3-418">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-418">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="a23f3-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="a23f3-420">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-420">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="a23f3-421">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="a23f3-421">Scope validation</span></span>

<span data-ttu-id="a23f3-422">ASP.NET Core 2.0 以降では、アプリの環境が開発の場合、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-422">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="a23f3-423">`ValidateScopes` が `true` に設定されていると、既定のサービス プロバイダーはチェックを実行して次のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-423">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="a23f3-424">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="a23f3-424">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="a23f3-425">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="a23f3-425">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="a23f3-426">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-426">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="a23f3-427">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-427">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="a23f3-428">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-428">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="a23f3-429">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-429">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="a23f3-430">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="a23f3-430">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="a23f3-431">運用環境も含めて、常にスコープを検証するには、ホスト ビルダー上の [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) で [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) を構成します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-431">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="a23f3-432">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="a23f3-432">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="a23f3-433">**適用対象: ASP.NET Core 2.0 のみ**</span><span class="sxs-lookup"><span data-stu-id="a23f3-433">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="a23f3-434">`UseStartup` や `Configure` を呼び出すのではなく、依存関係挿入コンテナーに直接 `IStartup` を挿入して、ホストをビルドする場合があります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-434">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="a23f3-435">ホストがこのようにビルドされている場合、以下のエラーが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-435">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="a23f3-436">これは、`HostingStartupAttributes` をスキャンするために [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (現在のアセンブリ) が必要であるために発生します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-436">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="a23f3-437">アプリで `IStartup` を手動で依存関係挿入コンテナーに挿入する場合は、指定されたアセンブリ名を使用して `WebHostBuilder` への次の呼び出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="a23f3-437">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="a23f3-438">あるいは、ダミーの `Configure` を `WebHostBuilder` に追加します。この場合、`applicationName`(`ApplicationKey`) が自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="a23f3-438">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="a23f3-439">**注**: これは、ASP.NET Core 2.0 リリースでのみ、およびアプリが `UseStartup` や `Configure` を呼び出さない場合にのみ必要になります。</span><span class="sxs-lookup"><span data-stu-id="a23f3-439">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="a23f3-440">詳細については、[お知らせのコメント「Microsoft.Extensions.PlatformAbstractions has been removed」](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Microsoft.Extensions.PlatformAbstractions が削除される) と [StartupInjection のサンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a23f3-440">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a23f3-441">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a23f3-441">Additional resources</span></span>

* [<span data-ttu-id="a23f3-442">IIS による Windows 上のホスト</span><span class="sxs-lookup"><span data-stu-id="a23f3-442">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="a23f3-443">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="a23f3-443">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="a23f3-444">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="a23f3-444">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="a23f3-445">Windows サービスでのホスト</span><span class="sxs-lookup"><span data-stu-id="a23f3-445">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
