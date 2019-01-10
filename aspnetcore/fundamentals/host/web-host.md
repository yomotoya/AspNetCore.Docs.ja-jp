---
title: ASP.NET Core の Web ホスト
author: guardrex
description: ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7215027a083c0ed0bc3b15196e390a31c5dcfc14
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637847"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="2926e-103">ASP.NET Core の Web ホスト</span><span class="sxs-lookup"><span data-stu-id="2926e-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="2926e-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2926e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="2926e-105">このトピックのバージョン 1.1 では、[ASP.NET Core Web ホスト (バージョン 1.1、PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf) をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2926e-105">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="2926e-106">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="2926e-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="2926e-107">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="2926e-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="2926e-108">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-108">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="2926e-109">このトピックでは、Web アプリをホストするのに便利な ASP.NET Core の Web ホスト ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) について説明します。</span><span class="sxs-lookup"><span data-stu-id="2926e-109">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="2926e-110">.NET での汎用ホスト ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) の対象範囲については、<xref:fundamentals/host/generic-host> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-110">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="2926e-111">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="2926e-111">Set up a host</span></span>

<span data-ttu-id="2926e-112">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="2926e-113">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="2926e-114">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="2926e-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="2926e-115">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="2926e-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="2926e-116">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="2926e-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="2926e-117">アプリのホスティング構成プロバイダーを使用して [Kestrel](xref:fundamentals/servers/kestrel) サーバーを Web サーバーとして構成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="2926e-118">Kestrel サーバーの既定のオプションについては、<xref:fundamentals/servers/kestrel#kestrel-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-118">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="2926e-119">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="2926e-120">次から[ ホスト構成](#host-configuration-values)を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="2926e-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="2926e-121">`ASPNETCORE_` のプレフィックスが付いた環境変数 (たとえば、`ASPNETCORE_ENVIRONMENT`)。</span><span class="sxs-lookup"><span data-stu-id="2926e-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="2926e-122">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="2926e-122">Command-line arguments.</span></span>
* <span data-ttu-id="2926e-123">次の順序でアプリの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="2926e-123">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="2926e-124">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="2926e-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="2926e-125">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="2926e-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="2926e-126">エントリ アセンブリを使用して `Development` 環境でアプリが実行される場合に使用される[シークレット マネージャー](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="2926e-126">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="2926e-127">環境変数。</span><span class="sxs-lookup"><span data-stu-id="2926e-127">Environment variables.</span></span>
  * <span data-ttu-id="2926e-128">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="2926e-128">Command-line arguments.</span></span>
* <span data-ttu-id="2926e-129">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="2926e-130">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2926e-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="2926e-131">[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)を使用して IIS の背後で実行されている場合は、`CreateDefaultBuilder` によってアプリのベース アドレスとポートが構成される [IIS の統合](xref:host-and-deploy/iis/index)が有効になります。</span><span class="sxs-lookup"><span data-stu-id="2926e-131">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="2926e-132">IIS の統合により、[スタートアップ エラーをキャプチャする](#capture-startup-errors)アプリも構成されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-132">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="2926e-133">IIS の既定のオプションについては、<xref:host-and-deploy/iis/index#iis-options> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-133">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="2926e-134">アプリの環境が開発の場合、[ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-134">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="2926e-135">詳しくは、「[スコープの検証](#scope-validation)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="2926e-135">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="2926e-136">`CreateDefaultBuilder` によって定義された構成は、[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)、そして [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) のその他のメソッドと拡張メソッドによってオーバーライドされ、拡張されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-136">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="2926e-137">以下に、いくつかの例を示します。</span><span class="sxs-lookup"><span data-stu-id="2926e-137">A few examples follow:</span></span>

* <span data-ttu-id="2926e-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) はアプリの追加の `IConfiguration` を指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="2926e-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="2926e-139">次の `ConfigureAppConfiguration` の呼び出しによりデリゲートが追加され、*appsettings.xml* ファイルにアプリの構成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2926e-139">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="2926e-140">`ConfigureAppConfiguration` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="2926e-140">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="2926e-141">この構成はホスト (たとえば、サーバーの URL や環境など) には適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-141">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="2926e-142">「[ホストの構成値](#host-configuration-values)」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-142">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="2926e-143">次の `ConfigureLogging` の呼び出しによりデリゲートが追加され、最小ログ記録レベル ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) が [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel) に構成されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-143">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="2926e-144">この設定により、`CreateDefaultBuilder` で構成された *appsettings.Development.json* (`LogLevel.Debug`) および *appsettings.Production.json* (`LogLevel.Error`) の設定がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="2926e-144">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2926e-145">`ConfigureLogging` は複数回呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="2926e-145">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="2926e-146">次の `ConfigureKestrel` の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="2926e-146">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="2926e-147">次の [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) の呼び出しにより、Kestrel が `CreateDefaultBuilder` によって構成されたときに確立された既定の [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) サイズである 30,000,000 バイトがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="2926e-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="2926e-148">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="2926e-149">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="2926e-150">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="2926e-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="2926e-151">アプリの構成の詳細については、<xref:fundamentals/configuration/index> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="2926e-152">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2926e-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="2926e-153">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="2926e-154">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="2926e-154">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="2926e-155">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2926e-155">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="2926e-156">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-156">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="2926e-157">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-157">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="2926e-158">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="2926e-158">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="2926e-159">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="2926e-159">Host configuration values</span></span>

<span data-ttu-id="2926e-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="2926e-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="2926e-161">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="2926e-161">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="2926e-162">たとえば、`ASPNETCORE_ENVIRONMENT` のようにします。</span><span class="sxs-lookup"><span data-stu-id="2926e-162">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="2926e-163">[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) や [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) などの拡張機能 (「[構成をオーバーライドする](#override-configuration)」のセクションを参照)。</span><span class="sxs-lookup"><span data-stu-id="2926e-163">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="2926e-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="2926e-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="2926e-165">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-165">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="2926e-166">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="2926e-166">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="2926e-167">詳しくは、次のセクション「[構成をオーバーライドする](#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="2926e-167">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="2926e-168">アプリケーション キー (名前)</span><span class="sxs-lookup"><span data-stu-id="2926e-168">Application Key (Name)</span></span>

<span data-ttu-id="2926e-169">ホストの構築時に [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) または [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) が呼び出されると、[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) プロパティが自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-169">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="2926e-170">この値はアプリのエントリ ポイントを含むアセンブリの名前に設定されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-170">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="2926e-171">値を明示的に設定するには、[WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey) を使用します。</span><span class="sxs-lookup"><span data-stu-id="2926e-171">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="2926e-172">**キー**: applicationName</span><span class="sxs-lookup"><span data-stu-id="2926e-172">**Key**: applicationName</span></span>  
<span data-ttu-id="2926e-173">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-173">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-174">**既定値**:アプリのエントリ ポイントを含むアセンブリの名前。</span><span class="sxs-lookup"><span data-stu-id="2926e-174">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="2926e-175">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2926e-175">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="2926e-176">**環境変数**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="2926e-176">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="2926e-177">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="2926e-177">Capture Startup Errors</span></span>

<span data-ttu-id="2926e-178">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="2926e-178">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="2926e-179">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="2926e-179">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="2926e-180">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="2926e-180">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2926e-181">**既定値**:アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-181">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="2926e-182">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="2926e-182">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="2926e-183">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="2926e-183">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="2926e-184">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="2926e-184">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="2926e-185">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="2926e-185">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="2926e-186">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="2926e-186">Content Root</span></span>

<span data-ttu-id="2926e-187">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-187">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="2926e-188">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="2926e-188">**Key**: contentRoot</span></span>  
<span data-ttu-id="2926e-189">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-189">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-190">**既定値**:既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-190">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="2926e-191">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="2926e-191">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="2926e-192">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="2926e-192">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="2926e-193">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-193">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="2926e-194">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="2926e-194">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="2926e-195">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="2926e-195">Detailed Errors</span></span>

<span data-ttu-id="2926e-196">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="2926e-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="2926e-197">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="2926e-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="2926e-198">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="2926e-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2926e-199">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="2926e-199">**Default**: false</span></span>  
<span data-ttu-id="2926e-200">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2926e-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="2926e-201">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="2926e-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="2926e-202">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="2926e-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="2926e-203">環境</span><span class="sxs-lookup"><span data-stu-id="2926e-203">Environment</span></span>

<span data-ttu-id="2926e-204">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-204">Sets the app's environment.</span></span>

<span data-ttu-id="2926e-205">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="2926e-205">**Key**: environment</span></span>  
<span data-ttu-id="2926e-206">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-206">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-207">**既定値**:実稼働</span><span class="sxs-lookup"><span data-stu-id="2926e-207">**Default**: Production</span></span>  
<span data-ttu-id="2926e-208">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="2926e-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="2926e-209">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="2926e-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="2926e-210">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="2926e-210">The environment can be set to any value.</span></span> <span data-ttu-id="2926e-211">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2926e-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="2926e-212">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="2926e-212">Values aren't case sensitive.</span></span> <span data-ttu-id="2926e-213">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="2926e-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="2926e-214">[Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="2926e-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="2926e-215">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-215">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="2926e-216">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="2926e-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="2926e-217">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="2926e-218">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="2926e-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="2926e-219">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-219">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-220">**既定値**:空の文字列</span><span class="sxs-lookup"><span data-stu-id="2926e-220">**Default**: Empty string</span></span>  
<span data-ttu-id="2926e-221">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2926e-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="2926e-222">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="2926e-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="2926e-223">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="2926e-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="2926e-224">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2926e-224">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="2926e-225">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-225">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="2926e-226">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="2926e-226">HTTPS Port</span></span>

<span data-ttu-id="2926e-227">HTTPS リダイレクト ポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-227">Set the HTTPS redirect port.</span></span> <span data-ttu-id="2926e-228">[HTTPS の適用](xref:security/enforcing-ssl)に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-228">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="2926e-229">**キー**: https_port **型**: *文字列*
**既定値**:既定値は設定されていません。</span><span class="sxs-lookup"><span data-stu-id="2926e-229">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="2926e-230">**次を使用して設定**:`UseSetting`
**環境変数**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="2926e-230">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="2926e-231">除外するホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="2926e-231">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="2926e-232">起動時に除外するホスティング スタートアップ アセンブリのセミコロン区切り文字列。</span><span class="sxs-lookup"><span data-stu-id="2926e-232">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="2926e-233">**キー**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="2926e-233">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="2926e-234">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-234">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-235">**既定値**:空の文字列</span><span class="sxs-lookup"><span data-stu-id="2926e-235">**Default**: Empty string</span></span>  
<span data-ttu-id="2926e-236">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2926e-236">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="2926e-237">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="2926e-237">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="2926e-238">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="2926e-238">Prefer Hosting URLs</span></span>

<span data-ttu-id="2926e-239">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="2926e-239">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="2926e-240">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="2926e-240">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="2926e-241">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="2926e-241">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2926e-242">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="2926e-242">**Default**: true</span></span>  
<span data-ttu-id="2926e-243">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="2926e-243">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="2926e-244">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="2926e-244">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="2926e-245">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="2926e-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="2926e-246">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="2926e-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="2926e-247">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-247">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="2926e-248">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="2926e-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="2926e-249">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="2926e-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2926e-250">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="2926e-250">**Default**: false</span></span>  
<span data-ttu-id="2926e-251">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2926e-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="2926e-252">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="2926e-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="2926e-253">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="2926e-253">Server URLs</span></span>

<span data-ttu-id="2926e-254">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="2926e-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="2926e-255">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="2926e-255">**Key**: urls</span></span>  
<span data-ttu-id="2926e-256">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-256">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-257">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="2926e-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="2926e-258">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="2926e-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="2926e-259">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="2926e-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="2926e-260">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="2926e-261">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="2926e-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="2926e-262">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="2926e-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="2926e-263">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="2926e-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="2926e-264">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="2926e-264">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="2926e-265">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="2926e-265">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="2926e-266">詳細については、「<xref:fundamentals/servers/kestrel#endpoint-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-266">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="2926e-267">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="2926e-267">Shutdown Timeout</span></span>

<span data-ttu-id="2926e-268">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-268">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="2926e-269">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="2926e-269">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="2926e-270">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="2926e-270">**Type**: *int*</span></span>  
<span data-ttu-id="2926e-271">**既定値**:5</span><span class="sxs-lookup"><span data-stu-id="2926e-271">**Default**: 5</span></span>  
<span data-ttu-id="2926e-272">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="2926e-272">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="2926e-273">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="2926e-273">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="2926e-274">キーは `UseSetting` で *int* を受け取りますが (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など)、[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 拡張メソッドは [TimeSpan](/dotnet/api/system.timespan) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="2926e-274">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="2926e-275">タイムアウト期間中、ホスティングは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="2926e-275">During the timeout period, hosting:</span></span>

* <span data-ttu-id="2926e-276">[IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="2926e-276">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="2926e-277">ホステッド サービスの停止を試み、停止に失敗したサービスのエラーをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="2926e-277">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="2926e-278">すべてのホステッド サービスが停止する前にタイムアウト時間が切れた場合、残っているアクティブなサービスはアプリのシャットダウン時に停止します。</span><span class="sxs-lookup"><span data-stu-id="2926e-278">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="2926e-279">処理が完了していない場合でも、サービスは停止します。</span><span class="sxs-lookup"><span data-stu-id="2926e-279">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="2926e-280">サービスが停止するまでにさらに時間が必要な場合は、タイムアウト値を増やします。</span><span class="sxs-lookup"><span data-stu-id="2926e-280">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="2926e-281">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="2926e-281">Startup Assembly</span></span>

<span data-ttu-id="2926e-282">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="2926e-283">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="2926e-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="2926e-284">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-284">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-285">**既定値**:アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="2926e-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="2926e-286">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="2926e-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="2926e-287">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="2926e-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="2926e-288">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="2926e-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="2926e-289">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="2926e-290">Web ルート</span><span class="sxs-lookup"><span data-stu-id="2926e-290">Web Root</span></span>

<span data-ttu-id="2926e-291">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-291">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="2926e-292">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="2926e-292">**Key**: webroot</span></span>  
<span data-ttu-id="2926e-293">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="2926e-293">**Type**: *string*</span></span>  
<span data-ttu-id="2926e-294">**既定値**:指定されていない場合、既定値は "(コンテンツ ルート)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="2926e-294">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="2926e-295">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-295">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="2926e-296">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="2926e-296">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="2926e-297">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="2926e-297">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="2926e-298">構成をオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="2926e-298">Override configuration</span></span>

<span data-ttu-id="2926e-299">[Configuration](xref:fundamentals/configuration/index) を使用して、Web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-299">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="2926e-300">次の例では、ホスト構成はオプションで *hostsettings.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-300">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="2926e-301">*hostsettings.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2926e-301">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="2926e-302">(`config` の) ビルド構成は、[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-302">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="2926e-303">`IWebHostBuilder` 構成はアプリの構成に追加されますが、その逆は当てはまりません&mdash;`ConfigureAppConfiguration` は `IWebHostBuilder` の構成に影響しません。</span><span class="sxs-lookup"><span data-stu-id="2926e-303">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="2926e-304">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hostsettings.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="2926e-304">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="2926e-305">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2926e-305">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="2926e-306">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="2926e-306">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="2926e-307">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="2926e-307">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="2926e-308">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="2926e-308">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="2926e-309">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="2926e-309">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="2926e-310">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="2926e-310">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="2926e-311">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-311">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="2926e-312">`UseConfiguration` は、指定された `IConfiguration` からホスト ビルダーの構成へのキーのみをコピーします。</span><span class="sxs-lookup"><span data-stu-id="2926e-312">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="2926e-313">そのため、JSON、INI、XML 設定のファイルに `reloadOnChange: true` を設定しても影響はありません。</span><span class="sxs-lookup"><span data-stu-id="2926e-313">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="2926e-314">特定の URL でホストを実行するように指定する場合、[dotnet run](/dotnet/core/tools/dotnet-run) の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="2926e-314">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="2926e-315">コマンドライン引数は *hostsettings.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="2926e-315">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="2926e-316">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="2926e-316">Manage the host</span></span>

<span data-ttu-id="2926e-317">**実行**</span><span class="sxs-lookup"><span data-stu-id="2926e-317">**Run**</span></span>

<span data-ttu-id="2926e-318">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="2926e-318">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="2926e-319">**[開始]**</span><span class="sxs-lookup"><span data-stu-id="2926e-319">**Start**</span></span>

<span data-ttu-id="2926e-320">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="2926e-320">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="2926e-321">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="2926e-321">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="2926e-322">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="2926e-322">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="2926e-323">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="2926e-323">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="2926e-324">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="2926e-324">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="2926e-325">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="2926e-325">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="2926e-326">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="2926e-326">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="2926e-327">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="2926e-327">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="2926e-328">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="2926e-328">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="2926e-329">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="2926e-329">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="2926e-330">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="2926e-330">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="2926e-331">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-331">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="2926e-332">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="2926e-332">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="2926e-333">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="2926e-333">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="2926e-334">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="2926e-334">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="2926e-335">要求</span><span class="sxs-lookup"><span data-stu-id="2926e-335">Request</span></span>                                    | <span data-ttu-id="2926e-336">応答</span><span class="sxs-lookup"><span data-stu-id="2926e-336">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="2926e-337">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="2926e-337">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="2926e-338">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="2926e-338">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="2926e-339">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="2926e-339">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="2926e-340">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="2926e-340">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="2926e-341">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="2926e-341">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="2926e-342">Hello World!</span><span class="sxs-lookup"><span data-stu-id="2926e-342">Hello World!</span></span>                             |

<span data-ttu-id="2926e-343">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="2926e-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="2926e-344">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="2926e-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="2926e-345">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="2926e-345">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="2926e-346">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="2926e-346">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="2926e-347">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action&lt;IRouteBuilder&gt; routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-347">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="2926e-348">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="2926e-348">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="2926e-349">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-349">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="2926e-350">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="2926e-350">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="2926e-351">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="2926e-351">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="2926e-352">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="2926e-352">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="2926e-353">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="2926e-353">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="2926e-354">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-354">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="2926e-355">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action&lt;IApplicationBuilder&gt; app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-355">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="2926e-356">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2926e-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="2926e-357">[IHostingEnvironment インターフェイス](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="2926e-357">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="2926e-358">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="2926e-358">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="2926e-359">[規則ベースのアプローチ](xref:fundamentals/environments#environment-based-startup-class-and-methods)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="2926e-359">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="2926e-360">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="2926e-360">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="2926e-361">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="2926e-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="2926e-362">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2926e-362">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="2926e-363">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="2926e-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="2926e-364">カスタムの[ミドルウェア](xref:fundamentals/middleware/index#write-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="2926e-364">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="2926e-365">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2926e-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="2926e-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="2926e-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="2926e-367">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="2926e-367">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="2926e-368">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="2926e-368">Cancellation Token</span></span>    | <span data-ttu-id="2926e-369">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="2926e-369">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="2926e-370">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="2926e-370">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="2926e-371">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="2926e-371">The host has fully started.</span></span> |
| [<span data-ttu-id="2926e-372">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="2926e-372">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="2926e-373">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="2926e-373">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="2926e-374">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2926e-374">All requests should be processed.</span></span> <span data-ttu-id="2926e-375">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="2926e-375">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="2926e-376">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="2926e-376">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="2926e-377">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="2926e-377">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="2926e-378">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="2926e-378">Requests may still be processing.</span></span> <span data-ttu-id="2926e-379">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="2926e-379">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="2926e-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="2926e-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="2926e-381">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="2926e-381">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="2926e-382">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="2926e-382">Scope validation</span></span>

<span data-ttu-id="2926e-383">アプリの環境が開発の場合、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="2926e-383">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="2926e-384">`ValidateScopes` が `true` に設定されていると、既定のサービス プロバイダーはチェックを実行して次のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="2926e-384">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="2926e-385">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="2926e-385">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="2926e-386">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="2926e-386">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="2926e-387">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-387">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="2926e-388">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-388">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="2926e-389">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-389">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="2926e-390">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="2926e-390">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="2926e-391">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="2926e-391">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="2926e-392">運用環境も含めて、常にスコープを検証するには、ホスト ビルダー上の [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) で [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) を構成します。</span><span class="sxs-lookup"><span data-stu-id="2926e-392">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="2926e-393">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="2926e-393">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
