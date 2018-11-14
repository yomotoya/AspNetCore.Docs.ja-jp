---
title: ASP.NET Core の Web ホスト
author: guardrex
description: ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: a3601b71c65321af56644eb87c4527d6290e4378
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505818"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="b113f-103">ASP.NET Core の Web ホスト</span><span class="sxs-lookup"><span data-stu-id="b113f-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="b113f-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b113f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b113f-105">このトピックのバージョン 1.1 では、[ASP.NET Core Web ホスト (バージョン 1.1、PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf) をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b113f-105">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

<span data-ttu-id="b113f-106">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="b113f-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="b113f-107">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="b113f-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b113f-108">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-108">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="b113f-109">このトピックでは、Web アプリをホストするのに便利な ASP.NET Core の Web ホスト ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) について説明します。</span><span class="sxs-lookup"><span data-stu-id="b113f-109">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="b113f-110">.NET での汎用ホスト ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) の対象範囲については、<xref:fundamentals/host/generic-host> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-110">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="b113f-111">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="b113f-111">Set up a host</span></span>

<span data-ttu-id="b113f-112">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="b113f-113">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="b113f-114">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="b113f-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="b113f-115">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="b113f-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="b113f-116">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="b113f-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="b113f-117">[Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、アプリのホスティング構成プロバイダーを使用してサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="b113f-118">Kestrel の既定のオプションについては、<xref:fundamentals/servers/kestrel#kestrel-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-118">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="b113f-119">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="b113f-120">次から[ ホスト構成](#host-configuration-values)を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="b113f-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="b113f-121">`ASPNETCORE_` のプレフィックスが付いた環境変数 (たとえば、`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="b113f-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="b113f-122">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="b113f-122">Command-line arguments.</span></span>
* <span data-ttu-id="b113f-123">次の順序でアプリの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="b113f-123">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="b113f-124">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="b113f-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="b113f-125">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="b113f-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="b113f-126">エントリ アセンブリを使用して `Development` 環境でアプリが実行される場合に使用される[シークレット マネージャー](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="b113f-126">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="b113f-127">環境変数。</span><span class="sxs-lookup"><span data-stu-id="b113f-127">Environment variables.</span></span>
  * <span data-ttu-id="b113f-128">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="b113f-128">Command-line arguments.</span></span>
* <span data-ttu-id="b113f-129">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="b113f-130">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b113f-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="b113f-131">IIS の背後での実行時に、[IIS 統合](xref:host-and-deploy/iis/index)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b113f-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="b113f-132">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)の使用時にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="b113f-133">このモジュールは、IIS と Kestrel の間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="b113f-134">また、[スタートアップ エラーをキャプチャする](#capture-startup-errors)ようにアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="b113f-135">IIS の既定のオプションについては、<xref:host-and-deploy/iis/index#iis-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-135">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="b113f-136">アプリの環境が開発の場合、[ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="b113f-137">詳しくは、「[スコープの検証](#scope-validation)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b113f-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="b113f-138">`CreateDefaultBuilder` によって定義された構成は、[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)、そして [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のその他のメソッドと拡張メソッドによってオーバーライドされ、拡張されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="b113f-139">以下に、いくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="b113f-139">A few examples follow:</span></span>

* <span data-ttu-id="b113f-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) はアプリの追加の `IConfiguration` を指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="b113f-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="b113f-141">次の `ConfigureAppConfiguration` の呼び出しによりデリゲートが追加され、*appsettings.xml* ファイルにアプリの構成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b113f-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="b113f-142">`ConfigureAppConfiguration` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="b113f-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="b113f-143">この構成はホスト (たとえば、サーバーの URL や環境など) には適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="b113f-144">「[ホストの構成値](#host-configuration-values)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="b113f-145">次の `ConfigureLogging` の呼び出しによりデリゲートが追加され、最小ログ記録レベル ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) が [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel) に構成されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="b113f-146">この設定により、`CreateDefaultBuilder` で構成された *appsettings.Development.json* (`LogLevel.Debug`) および *appsettings.Production.json* (`LogLevel.Error`) の設定がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="b113f-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b113f-147">`ConfigureLogging` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="b113f-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b113f-148">次の `ConfigureKestrel` の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="b113f-148">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="b113f-149">次の [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="b113f-149">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="b113f-150">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-150">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="b113f-151">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-151">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="b113f-152">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="b113f-152">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="b113f-153">アプリの構成の詳細については、<xref:fundamentals/configuration/index> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-153">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="b113f-154">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="b113f-154">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="b113f-155">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-155">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="b113f-156">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="b113f-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="b113f-157">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b113f-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="b113f-158">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-158">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="b113f-159">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="b113f-160">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="b113f-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="b113f-161">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="b113f-161">Host configuration values</span></span>

<span data-ttu-id="b113f-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="b113f-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="b113f-163">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="b113f-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="b113f-164">たとえば、`ASPNETCORE_ENVIRONMENT` のようにします。</span><span class="sxs-lookup"><span data-stu-id="b113f-164">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="b113f-165">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) や [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) などの拡張機能 (「[構成をオーバーライドする](#override-configuration)」のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="b113f-165">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="b113f-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="b113f-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="b113f-167">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="b113f-168">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="b113f-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="b113f-169">詳しくは、次のセクション「[構成をオーバーライドする](#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b113f-169">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="b113f-170">アプリケーション キー (名前)</span><span class="sxs-lookup"><span data-stu-id="b113f-170">Application Key (Name)</span></span>

<span data-ttu-id="b113f-171">ホストの構築時に [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) または [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) が呼び出されると、[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) プロパティが自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-171">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="b113f-172">この値はアプリのエントリ ポイントを含むアセンブリの名前に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-172">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="b113f-173">値を明示的に設定するには、[WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) を使用します。</span><span class="sxs-lookup"><span data-stu-id="b113f-173">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="b113f-174">**キー**: applicationName</span><span class="sxs-lookup"><span data-stu-id="b113f-174">**Key**: applicationName</span></span>  
<span data-ttu-id="b113f-175">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-175">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-176">**既定**: アプリのエントリ ポイントを含むアセンブリの名前。</span><span class="sxs-lookup"><span data-stu-id="b113f-176">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="b113f-177">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b113f-177">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b113f-178">**環境変数**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="b113f-178">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="b113f-179">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="b113f-179">Capture Startup Errors</span></span>

<span data-ttu-id="b113f-180">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="b113f-180">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="b113f-181">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="b113f-181">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="b113f-182">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="b113f-182">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b113f-183">**既定値**: アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-183">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="b113f-184">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="b113f-184">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="b113f-185">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="b113f-185">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="b113f-186">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="b113f-186">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="b113f-187">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="b113f-187">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="b113f-188">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="b113f-188">Content Root</span></span>

<span data-ttu-id="b113f-189">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-189">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="b113f-190">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="b113f-190">**Key**: contentRoot</span></span>  
<span data-ttu-id="b113f-191">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-191">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-192">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-192">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="b113f-193">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="b113f-193">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="b113f-194">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="b113f-194">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="b113f-195">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-195">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="b113f-196">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="b113f-196">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="b113f-197">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="b113f-197">Detailed Errors</span></span>

<span data-ttu-id="b113f-198">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="b113f-198">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="b113f-199">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="b113f-199">**Key**: detailedErrors</span></span>  
<span data-ttu-id="b113f-200">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="b113f-200">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b113f-201">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="b113f-201">**Default**: false</span></span>  
<span data-ttu-id="b113f-202">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b113f-202">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b113f-203">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="b113f-203">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="b113f-204">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="b113f-204">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="b113f-205">環境</span><span class="sxs-lookup"><span data-stu-id="b113f-205">Environment</span></span>

<span data-ttu-id="b113f-206">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-206">Sets the app's environment.</span></span>

<span data-ttu-id="b113f-207">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="b113f-207">**Key**: environment</span></span>  
<span data-ttu-id="b113f-208">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-208">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-209">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="b113f-209">**Default**: Production</span></span>  
<span data-ttu-id="b113f-210">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="b113f-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="b113f-211">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="b113f-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="b113f-212">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="b113f-212">The environment can be set to any value.</span></span> <span data-ttu-id="b113f-213">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b113f-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="b113f-214">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="b113f-214">Values aren't case sensitive.</span></span> <span data-ttu-id="b113f-215">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="b113f-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="b113f-216">[Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="b113f-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="b113f-217">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-217">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="b113f-218">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="b113f-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="b113f-219">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="b113f-220">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="b113f-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="b113f-221">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-221">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-222">**既定値**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="b113f-222">**Default**: Empty string</span></span>  
<span data-ttu-id="b113f-223">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b113f-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b113f-224">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="b113f-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="b113f-225">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="b113f-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="b113f-226">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b113f-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="b113f-227">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="b113f-228">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="b113f-228">HTTPS Port</span></span>

<span data-ttu-id="b113f-229">HTTPS リダイレクト ポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-229">Set the HTTPS redirect port.</span></span> <span data-ttu-id="b113f-230">[HTTPS の適用](xref:security/enforcing-ssl)に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-230">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="b113f-231">**キー**: https_port **型**: *string*
**既定値**: 既定値は設定されません。</span><span class="sxs-lookup"><span data-stu-id="b113f-231">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="b113f-232">**次を使用して設定**: `UseSetting`
**環境変数**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="b113f-232">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="b113f-233">除外するホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="b113f-233">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="b113f-234">起動時に除外するホスティング スタートアップ アセンブリのセミコロン区切り文字列。</span><span class="sxs-lookup"><span data-stu-id="b113f-234">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="b113f-235">**キー**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="b113f-235">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="b113f-236">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-236">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-237">**既定値**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="b113f-237">**Default**: Empty string</span></span>  
<span data-ttu-id="b113f-238">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b113f-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b113f-239">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="b113f-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="b113f-240">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="b113f-240">Prefer Hosting URLs</span></span>

<span data-ttu-id="b113f-241">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="b113f-241">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="b113f-242">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="b113f-242">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="b113f-243">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="b113f-243">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b113f-244">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="b113f-244">**Default**: true</span></span>  
<span data-ttu-id="b113f-245">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="b113f-245">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="b113f-246">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="b113f-246">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="b113f-247">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="b113f-247">Prevent Hosting Startup</span></span>

<span data-ttu-id="b113f-248">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="b113f-248">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="b113f-249">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-249">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="b113f-250">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="b113f-250">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="b113f-251">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="b113f-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b113f-252">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="b113f-252">**Default**: false</span></span>  
<span data-ttu-id="b113f-253">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b113f-253">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b113f-254">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="b113f-254">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="b113f-255">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="b113f-255">Server URLs</span></span>

<span data-ttu-id="b113f-256">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="b113f-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="b113f-257">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="b113f-257">**Key**: urls</span></span>  
<span data-ttu-id="b113f-258">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-258">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-259">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="b113f-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="b113f-260">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="b113f-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="b113f-261">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="b113f-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="b113f-262">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="b113f-263">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="b113f-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="b113f-264">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="b113f-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="b113f-265">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="b113f-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="b113f-266">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="b113f-266">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="b113f-267">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="b113f-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="b113f-268">詳細については、「<xref:fundamentals/servers/kestrel#endpoint-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-268">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="b113f-269">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="b113f-269">Shutdown Timeout</span></span>

<span data-ttu-id="b113f-270">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-270">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="b113f-271">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="b113f-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="b113f-272">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="b113f-272">**Type**: *int*</span></span>  
<span data-ttu-id="b113f-273">**既定値**: 5</span><span class="sxs-lookup"><span data-stu-id="b113f-273">**Default**: 5</span></span>  
<span data-ttu-id="b113f-274">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="b113f-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="b113f-275">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="b113f-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="b113f-276">キーは `UseSetting` で *int* を受け取りますが (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など)、[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 拡張メソッドは [TimeSpan](/dotnet/api/system.timespan) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="b113f-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="b113f-277">タイムアウト期間中、ホスティングは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="b113f-277">During the timeout period, hosting:</span></span>

* <span data-ttu-id="b113f-278">[IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="b113f-278">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="b113f-279">ホステッド サービスの停止を試み、停止に失敗したサービスのエラーをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="b113f-279">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="b113f-280">すべてのホステッド サービスが停止する前にタイムアウト時間が切れた場合、残っているアクティブなサービスはアプリのシャットダウン時に停止します。</span><span class="sxs-lookup"><span data-stu-id="b113f-280">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="b113f-281">処理が完了していない場合でも、サービスは停止します。</span><span class="sxs-lookup"><span data-stu-id="b113f-281">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="b113f-282">サービスが停止するまでにさらに時間が必要な場合は、タイムアウト値を増やします。</span><span class="sxs-lookup"><span data-stu-id="b113f-282">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="b113f-283">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="b113f-283">Startup Assembly</span></span>

<span data-ttu-id="b113f-284">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-284">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="b113f-285">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="b113f-285">**Key**: startupAssembly</span></span>  
<span data-ttu-id="b113f-286">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-286">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-287">**既定値**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="b113f-287">**Default**: The app's assembly</span></span>  
<span data-ttu-id="b113f-288">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="b113f-288">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="b113f-289">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="b113f-289">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="b113f-290">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="b113f-290">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="b113f-291">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-291">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="b113f-292">Web ルート</span><span class="sxs-lookup"><span data-stu-id="b113f-292">Web Root</span></span>

<span data-ttu-id="b113f-293">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="b113f-294">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="b113f-294">**Key**: webroot</span></span>  
<span data-ttu-id="b113f-295">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="b113f-295">**Type**: *string*</span></span>  
<span data-ttu-id="b113f-296">**既定値**: 指定されていない場合、既定値は "(Content Root)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="b113f-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="b113f-297">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="b113f-298">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="b113f-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="b113f-299">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="b113f-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="b113f-300">構成をオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="b113f-300">Override configuration</span></span>

<span data-ttu-id="b113f-301">[Configuration](xref:fundamentals/configuration/index) を使用して、Web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-301">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="b113f-302">次の例では、ホスト構成はオプションで *hostsettings.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-302">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="b113f-303">*hostsettings.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="b113f-303">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="b113f-304">(`config` の) ビルド構成は、[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-304">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="b113f-305">`IWebHostBuilder` 構成はアプリの構成に追加されますが、その逆は当てはまりません&mdash;`ConfigureAppConfiguration` は `IWebHostBuilder` の構成に影響しません。</span><span class="sxs-lookup"><span data-stu-id="b113f-305">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="b113f-306">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hostsettings.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="b113f-306">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="b113f-307">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b113f-307">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="b113f-308">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="b113f-308">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="b113f-309">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="b113f-309">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="b113f-310">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="b113f-310">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="b113f-311">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="b113f-311">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="b113f-312">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="b113f-312">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="b113f-313">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-313">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="b113f-314">`UseConfiguration` は、指定された `IConfiguration` からホスト ビルダーの構成へのキーのみをコピーします。</span><span class="sxs-lookup"><span data-stu-id="b113f-314">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="b113f-315">そのため、JSON、INI、XML 設定のファイルに `reloadOnChange: true` を設定しても影響はありません。</span><span class="sxs-lookup"><span data-stu-id="b113f-315">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="b113f-316">特定の URL でホストを実行するように指定する場合、[dotnet run](/dotnet/core/tools/dotnet-run) の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b113f-316">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="b113f-317">コマンドライン引数は *hostsettings.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="b113f-317">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="b113f-318">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="b113f-318">Manage the host</span></span>

<span data-ttu-id="b113f-319">**実行**</span><span class="sxs-lookup"><span data-stu-id="b113f-319">**Run**</span></span>

<span data-ttu-id="b113f-320">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="b113f-320">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="b113f-321">**[開始]**</span><span class="sxs-lookup"><span data-stu-id="b113f-321">**Start**</span></span>

<span data-ttu-id="b113f-322">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="b113f-322">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="b113f-323">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="b113f-323">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="b113f-324">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="b113f-324">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="b113f-325">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="b113f-325">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="b113f-326">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="b113f-326">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="b113f-327">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="b113f-327">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b113f-328">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="b113f-328">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b113f-329">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="b113f-329">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b113f-330">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="b113f-330">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b113f-331">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="b113f-331">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="b113f-332">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="b113f-332">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b113f-333">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-333">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="b113f-334">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="b113f-334">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="b113f-335">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="b113f-335">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="b113f-336">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="b113f-336">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="b113f-337">要求</span><span class="sxs-lookup"><span data-stu-id="b113f-337">Request</span></span>                                    | <span data-ttu-id="b113f-338">応答</span><span class="sxs-lookup"><span data-stu-id="b113f-338">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="b113f-339">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="b113f-339">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="b113f-340">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="b113f-340">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="b113f-341">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="b113f-341">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="b113f-342">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="b113f-342">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="b113f-343">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="b113f-343">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="b113f-344">Hello World!</span><span class="sxs-lookup"><span data-stu-id="b113f-344">Hello World!</span></span>                             |

<span data-ttu-id="b113f-345">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="b113f-345">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b113f-346">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="b113f-346">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b113f-347">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="b113f-347">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="b113f-348">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="b113f-348">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="b113f-349">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action&lt;IRouteBuilder&gt; routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-349">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="b113f-350">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="b113f-350">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="b113f-351">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-351">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b113f-352">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="b113f-352">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b113f-353">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="b113f-353">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b113f-354">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="b113f-354">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b113f-355">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="b113f-355">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="b113f-356">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-356">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b113f-357">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action&lt;IApplicationBuilder&gt; app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-357">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="b113f-358">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="b113f-358">IHostingEnvironment interface</span></span>

<span data-ttu-id="b113f-359">[IHostingEnvironment インターフェイス](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="b113f-359">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="b113f-360">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="b113f-360">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="b113f-361">[規則ベースのアプローチ](xref:fundamentals/environments#environment-based-startup-class-and-methods)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="b113f-361">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="b113f-362">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="b113f-362">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="b113f-363">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="b113f-363">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="b113f-364">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b113f-364">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="b113f-365">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="b113f-365">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="b113f-366">カスタムの[ミドルウェア](xref:fundamentals/middleware/index#write-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="b113f-366">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="b113f-367">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="b113f-367">IApplicationLifetime interface</span></span>

<span data-ttu-id="b113f-368">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="b113f-368">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="b113f-369">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="b113f-369">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="b113f-370">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="b113f-370">Cancellation Token</span></span>    | <span data-ttu-id="b113f-371">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="b113f-371">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="b113f-372">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="b113f-372">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="b113f-373">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="b113f-373">The host has fully started.</span></span> |
| [<span data-ttu-id="b113f-374">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="b113f-374">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="b113f-375">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="b113f-375">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="b113f-376">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b113f-376">All requests should be processed.</span></span> <span data-ttu-id="b113f-377">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="b113f-377">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="b113f-378">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="b113f-378">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="b113f-379">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="b113f-379">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="b113f-380">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="b113f-380">Requests may still be processing.</span></span> <span data-ttu-id="b113f-381">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="b113f-381">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="b113f-382">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="b113f-382">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="b113f-383">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="b113f-383">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="b113f-384">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="b113f-384">Scope validation</span></span>

<span data-ttu-id="b113f-385">アプリの環境が開発の場合、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="b113f-385">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="b113f-386">`ValidateScopes` が `true` に設定されていると、既定のサービス プロバイダーはチェックを実行して次のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b113f-386">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="b113f-387">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="b113f-387">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="b113f-388">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="b113f-388">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="b113f-389">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-389">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="b113f-390">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-390">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="b113f-391">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-391">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="b113f-392">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="b113f-392">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="b113f-393">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="b113f-393">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="b113f-394">運用環境も含めて、常にスコープを検証するには、ホスト ビルダー上の [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) で [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) を構成します。</span><span class="sxs-lookup"><span data-stu-id="b113f-394">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="b113f-395">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b113f-395">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
