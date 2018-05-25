---
title: ASP.NET Core の Web ホスト
author: guardrex
description: ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ced2a766359894b9b83164c12a3ab69aa13c93a0
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="0db1b-103">ASP.NET Core の Web ホスト</span><span class="sxs-lookup"><span data-stu-id="0db1b-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="0db1b-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0db1b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0db1b-105">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="0db1b-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0db1b-107">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="0db1b-108">このトピックでは、Web アプリをホストするのに便利な ASP.NET Core の Web ホスト ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) について説明します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-108">This topic covers the ASP.NET Core Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="0db1b-109">.NET 汎用 Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) の対象範囲については、[汎用ホスト](xref:fundamentals/host/generic-host)に関するトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-109">For coverage of the .NET Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="0db1b-110">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="0db1b-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="0db1b-112">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-112">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0db1b-113">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0db1b-114">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="0db1b-115">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="0db1b-116">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="0db1b-117">[Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server.</span></span> <span data-ttu-id="0db1b-118">Kestrel の既定のオプションについては、[「ASP.NET Core の Kestrel Web サーバーの実装の概要」の「Kestrel オプション」セクション](xref:fundamentals/servers/kestrel#kestrel-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="0db1b-119">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="0db1b-120">次の場所から省略可能な構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-120">Loads optional configuration from:</span></span>
  * <span data-ttu-id="0db1b-121">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="0db1b-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="0db1b-122">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="0db1b-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="0db1b-123">`Development` 環境でアプリが実行される場合に使用される[ユーザー シークレット](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="0db1b-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="0db1b-124">環境変数。</span><span class="sxs-lookup"><span data-stu-id="0db1b-124">Environment variables.</span></span>
  * <span data-ttu-id="0db1b-125">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="0db1b-125">Command-line arguments.</span></span>
* <span data-ttu-id="0db1b-126">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="0db1b-127">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="0db1b-128">IIS の背後での実行時に、[IIS 統合](xref:host-and-deploy/iis/index)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="0db1b-129">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)の使用時にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0db1b-130">このモジュールは、IIS と Kestrel の間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="0db1b-131">また、[スタートアップ エラーをキャプチャする](#capture-startup-errors)ようにアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="0db1b-132">IIS の既定のオプションについては、[「IIS を使用した Windows での ASP.NET Core のホスト」の「IIS のオプション」セクション](xref:host-and-deploy/iis/index#iis-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="0db1b-133">アプリの環境が開発の場合、[ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="0db1b-134">詳しくは、「[スコープの検証](#scope-validation)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="0db1b-135">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0db1b-136">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0db1b-137">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0db1b-138">アプリの構成の詳細については、「[ASP.NET Core の構成](xref:fundamentals/configuration/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="0db1b-139">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="0db1b-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="0db1b-140">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="0db1b-142">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0db1b-143">通常、ホストの作成はアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0db1b-144">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="0db1b-145">`WebHostBuilder` には、[IServer を実装するサーバー](xref:fundamentals/servers/index)が必要です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="0db1b-146">組み込みサーバーは [Kestrel](xref:fundamentals/servers/kestrel) と [HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET Core 2.0 より前のリリースでは [WebListener](xref:fundamentals/servers/weblistener) と呼ばれていた) です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="0db1b-147">この例では、[UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)で Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="0db1b-148">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0db1b-149">`UseContentRoot` の既定のコンテンツ ルートは、[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="0db1b-150">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0db1b-151">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0db1b-152">IIS をリバース プロキシとして使用するには、ホストのビルド時に [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="0db1b-153">`UseIISIntegration` では、[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) の場合のように*サーバー*は構成されません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="0db1b-154">`UseIISIntegration` は、Kestrel と IIS の間にリバース プロキシを作成するために [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を使用する際にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="0db1b-155">IIS と ASP.NET Core を一緒に使用するには、`UseKestrel` と `UseIISIntegration` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="0db1b-156">`UseIISIntegration` は、IIS または IIS Express の背後で実行されている場合にのみアクティブになります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="0db1b-157">詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="0db1b-158">ホスト (および ASP.NET Core アプリ) を構成する最小限の実装には、アプリの要求パイプラインの構成とサーバーの指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="0db1b-159">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="0db1b-160">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="0db1b-161">詳細については、「[ASP.NET Core でのアプリケーションのスタートアップ](xref:fundamentals/startup)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="0db1b-162">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="0db1b-163">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="0db1b-164">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="0db1b-164">Host configuration values</span></span>

<span data-ttu-id="0db1b-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="0db1b-166">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="0db1b-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="0db1b-167">たとえば、`ASPNETCORE_ENVIRONMENT` のようにします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="0db1b-168">[HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) などの明示的なメソッド。</span><span class="sxs-lookup"><span data-stu-id="0db1b-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="0db1b-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="0db1b-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="0db1b-170">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="0db1b-171">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="0db1b-172">詳しくは、次のセクション「[構成をオーバーライドする](#override-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="0db1b-173">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="0db1b-173">Capture Startup Errors</span></span>

<span data-ttu-id="0db1b-174">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="0db1b-175">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="0db1b-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="0db1b-176">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="0db1b-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0db1b-177">**既定値**: アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="0db1b-178">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="0db1b-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="0db1b-179">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="0db1b-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="0db1b-180">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0db1b-181">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="0db1b-184">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="0db1b-184">Content Root</span></span>

<span data-ttu-id="0db1b-185">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="0db1b-186">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="0db1b-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="0db1b-187">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="0db1b-187">**Type**: *string*</span></span>  
<span data-ttu-id="0db1b-188">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="0db1b-189">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="0db1b-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="0db1b-190">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="0db1b-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="0db1b-191">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="0db1b-192">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-193">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="0db1b-195">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="0db1b-195">Detailed Errors</span></span>

<span data-ttu-id="0db1b-196">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="0db1b-197">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="0db1b-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="0db1b-198">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="0db1b-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0db1b-199">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="0db1b-199">**Default**: false</span></span>  
<span data-ttu-id="0db1b-200">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0db1b-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0db1b-201">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="0db1b-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="0db1b-202">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="0db1b-205">環境</span><span class="sxs-lookup"><span data-stu-id="0db1b-205">Environment</span></span>

<span data-ttu-id="0db1b-206">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-206">Sets the app's environment.</span></span>

<span data-ttu-id="0db1b-207">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="0db1b-207">**Key**: environment</span></span>  
<span data-ttu-id="0db1b-208">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="0db1b-208">**Type**: *string*</span></span>  
<span data-ttu-id="0db1b-209">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="0db1b-209">**Default**: Production</span></span>  
<span data-ttu-id="0db1b-210">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="0db1b-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="0db1b-211">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="0db1b-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="0db1b-212">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-212">The environment can be set to any value.</span></span> <span data-ttu-id="0db1b-213">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0db1b-214">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-214">Values aren't case sensitive.</span></span> <span data-ttu-id="0db1b-215">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="0db1b-216">[Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="0db1b-217">詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="0db1b-220">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="0db1b-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="0db1b-221">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="0db1b-222">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="0db1b-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="0db1b-223">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="0db1b-223">**Type**: *string*</span></span>  
<span data-ttu-id="0db1b-224">**既定値**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="0db1b-224">**Default**: Empty string</span></span>  
<span data-ttu-id="0db1b-225">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0db1b-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0db1b-226">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0db1b-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="0db1b-227">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="0db1b-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="0db1b-228">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="0db1b-229">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="0db1b-230">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-231">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0db1b-233">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="0db1b-234">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="0db1b-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="0db1b-235">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="0db1b-236">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="0db1b-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="0db1b-237">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="0db1b-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0db1b-238">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="0db1b-238">**Default**: true</span></span>  
<span data-ttu-id="0db1b-239">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="0db1b-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="0db1b-240">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="0db1b-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="0db1b-241">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-242">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0db1b-244">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="0db1b-245">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="0db1b-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="0db1b-246">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="0db1b-247">詳しくは、「[Enhance an app from an external assembly in ASP.NET Core with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)」(ASP.NET Core 内で IHostingStartup を使用して外部アセンブリからアプリを拡張する) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="0db1b-248">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0db1b-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="0db1b-249">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="0db1b-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0db1b-250">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="0db1b-250">**Default**: false</span></span>  
<span data-ttu-id="0db1b-251">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0db1b-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0db1b-252">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="0db1b-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="0db1b-253">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0db1b-256">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="0db1b-257">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="0db1b-257">Server URLs</span></span>

<span data-ttu-id="0db1b-258">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="0db1b-259">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="0db1b-259">**Key**: urls</span></span>  
<span data-ttu-id="0db1b-260">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="0db1b-260">**Type**: *string*</span></span>  
<span data-ttu-id="0db1b-261">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="0db1b-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="0db1b-262">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="0db1b-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="0db1b-263">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="0db1b-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="0db1b-264">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="0db1b-265">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0db1b-266">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="0db1b-267">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0db1b-268">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-269">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="0db1b-270">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="0db1b-271">詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="0db1b-273">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="0db1b-273">Shutdown Timeout</span></span>

<span data-ttu-id="0db1b-274">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="0db1b-275">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="0db1b-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="0db1b-276">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="0db1b-276">**Type**: *int*</span></span>  
<span data-ttu-id="0db1b-277">**既定値**: 5</span><span class="sxs-lookup"><span data-stu-id="0db1b-277">**Default**: 5</span></span>  
<span data-ttu-id="0db1b-278">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="0db1b-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="0db1b-279">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="0db1b-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="0db1b-280">キーは `UseSetting` で *int* を受け取りますが (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など)、[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 拡張メソッドは [TimeSpan](/dotnet/api/system.timespan) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="0db1b-281">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="0db1b-282">タイムアウト期間中、ホスティングは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="0db1b-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="0db1b-283">[IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="0db1b-284">ホステッド サービスの停止を試み、停止に失敗したサービスのエラーをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="0db1b-285">すべてのホステッド サービスが停止する前にタイムアウト時間が切れた場合、残っているアクティブなサービスはアプリのシャットダウン時に停止します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="0db1b-286">処理が完了していない場合でも、サービスは停止します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="0db1b-287">サービスが停止するまでにさらに時間が必要な場合は、タイムアウト値を増やします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-288">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0db1b-290">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="0db1b-291">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="0db1b-291">Startup Assembly</span></span>

<span data-ttu-id="0db1b-292">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="0db1b-293">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="0db1b-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="0db1b-294">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="0db1b-294">**Type**: *string*</span></span>  
<span data-ttu-id="0db1b-295">**既定値**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="0db1b-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="0db1b-296">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="0db1b-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="0db1b-297">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="0db1b-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="0db1b-298">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="0db1b-299">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="0db1b-302">Web ルート</span><span class="sxs-lookup"><span data-stu-id="0db1b-302">Web Root</span></span>

<span data-ttu-id="0db1b-303">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="0db1b-304">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="0db1b-304">**Key**: webroot</span></span>  
<span data-ttu-id="0db1b-305">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="0db1b-305">**Type**: *string*</span></span>  
<span data-ttu-id="0db1b-306">**既定値**: 指定されていない場合、既定値は "(Content Root)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="0db1b-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="0db1b-307">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="0db1b-308">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="0db1b-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="0db1b-309">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="0db1b-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="0db1b-312">構成をオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="0db1b-312">Override configuration</span></span>

<span data-ttu-id="0db1b-313">[Configuration](xref:fundamentals/configuration/index) を使用して、ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="0db1b-314">次の例では、ホスト構成はオプションで *hosting.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="0db1b-315">*hosting.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="0db1b-316">(`config` の) ビルド構成は、`UseConfiguration` でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-317">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0db1b-318">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="0db1b-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="0db1b-319">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="0db1b-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0db1b-321">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="0db1b-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="0db1b-322">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="0db1b-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

---

> [!NOTE]
> <span data-ttu-id="0db1b-323">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="0db1b-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="0db1b-324">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="0db1b-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="0db1b-325">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="0db1b-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="0db1b-326">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="0db1b-327">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="0db1b-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="0db1b-328">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="0db1b-329">特定の URL でホストを実行するように指定する場合、[dotnet run](/dotnet/core/tools/dotnet-run) の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="0db1b-330">コマンドライン引数は *hosting.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="0db1b-331">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="0db1b-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0db1b-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0db1b-333">**実行**</span><span class="sxs-lookup"><span data-stu-id="0db1b-333">**Run**</span></span>

<span data-ttu-id="0db1b-334">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0db1b-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="0db1b-335">**Start**</span></span>

<span data-ttu-id="0db1b-336">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0db1b-337">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="0db1b-338">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="0db1b-339">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="0db1b-340">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="0db1b-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="0db1b-341">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0db1b-342">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="0db1b-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0db1b-343">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0db1b-344">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0db1b-345">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="0db1b-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="0db1b-346">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0db1b-347">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="0db1b-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0db1b-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="0db1b-349">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="0db1b-350">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="0db1b-351">要求</span><span class="sxs-lookup"><span data-stu-id="0db1b-351">Request</span></span>                                    | <span data-ttu-id="0db1b-352">応答</span><span class="sxs-lookup"><span data-stu-id="0db1b-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="0db1b-353">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="0db1b-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="0db1b-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="0db1b-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="0db1b-355">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="0db1b-356">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="0db1b-357">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="0db1b-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="0db1b-358">Hello World!</span><span class="sxs-lookup"><span data-stu-id="0db1b-358">Hello World!</span></span>                             |

<span data-ttu-id="0db1b-359">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0db1b-360">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0db1b-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0db1b-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="0db1b-362">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="0db1b-363">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action&lt;IRouteBuilder&gt; routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="0db1b-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="0db1b-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="0db1b-365">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="0db1b-366">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="0db1b-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0db1b-367">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0db1b-368">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0db1b-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="0db1b-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="0db1b-370">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="0db1b-371">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action&lt;IApplicationBuilder&gt; app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0db1b-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0db1b-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0db1b-373">**実行**</span><span class="sxs-lookup"><span data-stu-id="0db1b-373">**Run**</span></span>

<span data-ttu-id="0db1b-374">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0db1b-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="0db1b-375">**Start**</span></span>

<span data-ttu-id="0db1b-376">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0db1b-377">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="0db1b-378">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="0db1b-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="0db1b-379">[IHostingEnvironment インターフェイス](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="0db1b-380">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="0db1b-381">[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="0db1b-382">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="0db1b-383">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="0db1b-384">詳細については、[複数の環境での使用](xref:fundamentals/environments)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="0db1b-385">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="0db1b-386">カスタムの[ミドルウェア](xref:fundamentals/middleware/index#writing-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="0db1b-387">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="0db1b-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="0db1b-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="0db1b-389">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="0db1b-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="0db1b-390">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="0db1b-390">Cancellation Token</span></span>    | <span data-ttu-id="0db1b-391">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="0db1b-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="0db1b-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="0db1b-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="0db1b-393">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="0db1b-393">The host has fully started.</span></span> |
| [<span data-ttu-id="0db1b-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="0db1b-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="0db1b-395">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="0db1b-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="0db1b-396">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-396">All requests should be processed.</span></span> <span data-ttu-id="0db1b-397">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="0db1b-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="0db1b-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="0db1b-399">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="0db1b-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="0db1b-400">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="0db1b-400">Requests may still be processing.</span></span> <span data-ttu-id="0db1b-401">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-401">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="0db1b-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="0db1b-403">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="0db1b-404">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="0db1b-404">Scope validation</span></span>

<span data-ttu-id="0db1b-405">ASP.NET Core 2.0 以降では、アプリの環境が開発の場合、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="0db1b-406">`ValidateScopes` が `true` に設定されていると、既定のサービス プロバイダーはチェックを実行して次のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="0db1b-407">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="0db1b-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="0db1b-408">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="0db1b-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="0db1b-409">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="0db1b-410">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="0db1b-411">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="0db1b-412">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="0db1b-413">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="0db1b-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="0db1b-414">運用環境も含めて、常にスコープを検証するには、ホスト ビルダー上の [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) で [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) を構成します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="0db1b-415">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="0db1b-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="0db1b-416">**適用対象: ASP.NET Core 2.0 のみ**</span><span class="sxs-lookup"><span data-stu-id="0db1b-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="0db1b-417">`UseStartup` や `Configure` を呼び出すのではなく、依存関係挿入コンテナーに直接 `IStartup` を挿入して、ホストをビルドする場合があります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="0db1b-418">ホストがこのようにビルドされている場合、以下のエラーが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="0db1b-419">これは、`HostingStartupAttributes` をスキャンするために [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (現在のアセンブリ) が必要であるために発生します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="0db1b-420">アプリで `IStartup` を手動で依存関係挿入コンテナーに挿入する場合は、指定されたアセンブリ名を使用して `WebHostBuilder` への次の呼び出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="0db1b-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="0db1b-421">あるいは、ダミーの `Configure` を `WebHostBuilder` に追加します。この場合、`applicationName`(`ApplicationKey`) が自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="0db1b-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="0db1b-422">**注**: これは、ASP.NET Core 2.0 リリースでのみ、およびアプリが `UseStartup` や `Configure` を呼び出さない場合にのみ必要になります。</span><span class="sxs-lookup"><span data-stu-id="0db1b-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="0db1b-423">詳細については、[お知らせのコメント「Microsoft.Extensions.PlatformAbstractions has been removed」](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Microsoft.Extensions.PlatformAbstractions が削除される) と [StartupInjection のサンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0db1b-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0db1b-424">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0db1b-424">Additional resources</span></span>

* [<span data-ttu-id="0db1b-425">IIS による Windows 上のホスト</span><span class="sxs-lookup"><span data-stu-id="0db1b-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="0db1b-426">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="0db1b-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="0db1b-427">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="0db1b-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="0db1b-428">Windows サービスでのホスト</span><span class="sxs-lookup"><span data-stu-id="0db1b-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
