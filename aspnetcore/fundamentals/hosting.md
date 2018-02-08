---
title: "ASP.NET Core でのホスティング"
author: guardrex
description: "ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。"
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 004487aebe5262a515e2375c30ccd2a84844dff6
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="46843-103">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="46843-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="46843-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="46843-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="46843-105">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="46843-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="46843-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="46843-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="46843-107">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="46843-108">ホストの設定</span><span class="sxs-lookup"><span data-stu-id="46843-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="46843-110">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="46843-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="46843-111">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="46843-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="46843-112">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="46843-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="46843-113">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="46843-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="46843-114">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="46843-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="46843-115">[Kestrel](servers/kestrel.md) を Web サーバーとして構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="46843-116">Kestrel の既定のオプションについては、[「ASP.NET Core の Kestrel Web サーバーの実装の概要」の「Kestrel オプション」セクション](xref:fundamentals/servers/kestrel#kestrel-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="46843-117">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="46843-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="46843-118">次の場所から省略可能な構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="46843-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="46843-119">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="46843-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="46843-120">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="46843-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="46843-121">`Development` 環境でアプリが実行される場合に使用される[ユーザー シークレット](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="46843-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="46843-122">環境変数。</span><span class="sxs-lookup"><span data-stu-id="46843-122">Environment variables.</span></span>
  * <span data-ttu-id="46843-123">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="46843-123">Command-line arguments.</span></span>
* <span data-ttu-id="46843-124">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="46843-125">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="46843-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="46843-126">IIS の背後での実行時に、[IIS 統合](xref:host-and-deploy/iis/index)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="46843-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="46843-127">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)の使用時にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="46843-128">このモジュールは、IIS と Kestrel の間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="46843-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="46843-129">また、[スタートアップ エラーをキャプチャする](#capture-startup-errors)ようにアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="46843-130">IIS の既定のオプションについては、[「IIS を使用した Windows での ASP.NET Core のホスト」の「IIS のオプション」セクション](xref:host-and-deploy/iis/index#iis-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="46843-131">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="46843-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="46843-132">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="46843-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="46843-133">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="46843-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="46843-134">アプリの構成の詳細については、「[ASP.NET Core の構成](xref:fundamentals/configuration/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="46843-135">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="46843-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="46843-136">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="46843-138">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="46843-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="46843-139">通常、ホストの作成はアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="46843-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="46843-140">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="46843-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="46843-141">`WebHostBuilder` には、[IServer を実装するサーバー](servers/index.md)が必要です。</span><span class="sxs-lookup"><span data-stu-id="46843-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="46843-142">組み込みサーバーは [Kestrel](servers/kestrel.md) と [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 より前のリリースでは [WebListener](xref:fundamentals/servers/weblistener) と呼ばれていた) です。</span><span class="sxs-lookup"><span data-stu-id="46843-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="46843-143">この例では、[UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)で Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="46843-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="46843-144">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="46843-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="46843-145">`UseContentRoot` の既定のコンテンツ ルートは、[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="46843-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="46843-146">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="46843-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="46843-147">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="46843-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="46843-148">IIS をリバース プロキシとして使用するには、ホストのビルド時に [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="46843-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="46843-149">`UseIISIntegration` では、[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) の場合のように*サーバー*は構成されません。</span><span class="sxs-lookup"><span data-stu-id="46843-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="46843-150">`UseIISIntegration` は、Kestrel と IIS の間にリバース プロキシを作成するために [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を使用する際にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="46843-151">IIS と ASP.NET Core を一緒に使用するには、`UseKestrel` と `UseIISIntegration` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="46843-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="46843-152">`UseIISIntegration` は、IIS または IIS Express の背後で実行されている場合にのみアクティブになります。</span><span class="sxs-lookup"><span data-stu-id="46843-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="46843-153">詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="46843-154">ホスト (および ASP.NET Core アプリ) を構成する最小限の実装には、アプリの要求パイプラインの構成とサーバーの指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="46843-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="46843-155">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="46843-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="46843-156">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="46843-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="46843-157">詳細については、「[ASP.NET Core でのアプリケーションのスタートアップ](startup.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="46843-158">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="46843-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="46843-159">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="46843-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="46843-160">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="46843-160">Host configuration values</span></span>

<span data-ttu-id="46843-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="46843-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="46843-162">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="46843-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="46843-163">たとえば、`ASPNETCORE_URLS` のようにします。</span><span class="sxs-lookup"><span data-stu-id="46843-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="46843-164">`CaptureStartupErrors` などの明示的なメソッド。</span><span class="sxs-lookup"><span data-stu-id="46843-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="46843-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="46843-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="46843-166">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="46843-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="46843-167">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="46843-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="46843-168">詳細については、次のセクションの「[構成のオーバーライド](#overriding-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="46843-169">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="46843-169">Capture Startup Errors</span></span>

<span data-ttu-id="46843-170">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="46843-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="46843-171">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="46843-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="46843-172">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="46843-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="46843-173">**既定値**: アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="46843-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="46843-174">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="46843-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="46843-175">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="46843-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="46843-176">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="46843-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="46843-177">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="46843-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="46843-180">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="46843-180">Content Root</span></span>

<span data-ttu-id="46843-181">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="46843-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="46843-182">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="46843-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="46843-183">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="46843-183">**Type**: *string*</span></span>  
<span data-ttu-id="46843-184">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="46843-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="46843-185">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="46843-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="46843-186">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="46843-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="46843-187">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="46843-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="46843-188">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="46843-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="46843-191">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="46843-191">Detailed Errors</span></span>

<span data-ttu-id="46843-192">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="46843-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="46843-193">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="46843-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="46843-194">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="46843-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="46843-195">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="46843-195">**Default**: false</span></span>  
<span data-ttu-id="46843-196">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="46843-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="46843-197">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="46843-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="46843-198">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="46843-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="46843-201">環境</span><span class="sxs-lookup"><span data-stu-id="46843-201">Environment</span></span>

<span data-ttu-id="46843-202">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="46843-202">Sets the app's environment.</span></span>

<span data-ttu-id="46843-203">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="46843-203">**Key**: environment</span></span>  
<span data-ttu-id="46843-204">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="46843-204">**Type**: *string*</span></span>  
<span data-ttu-id="46843-205">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="46843-205">**Default**: Production</span></span>  
<span data-ttu-id="46843-206">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="46843-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="46843-207">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="46843-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="46843-208">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="46843-208">The environment can be set to any value.</span></span> <span data-ttu-id="46843-209">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="46843-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="46843-210">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="46843-210">Values aren't case sensitive.</span></span> <span data-ttu-id="46843-211">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="46843-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="46843-212">[Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="46843-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="46843-213">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="46843-216">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="46843-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="46843-217">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="46843-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="46843-218">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="46843-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="46843-219">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="46843-219">**Type**: *string*</span></span>  
<span data-ttu-id="46843-220">**既定値**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="46843-220">**Default**: Empty string</span></span>  
<span data-ttu-id="46843-221">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="46843-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="46843-222">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="46843-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="46843-223">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="46843-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="46843-224">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="46843-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="46843-225">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="46843-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="46843-226">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="46843-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="46843-229">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="46843-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="46843-230">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="46843-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="46843-231">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="46843-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="46843-232">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="46843-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="46843-233">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="46843-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="46843-234">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="46843-234">**Default**: true</span></span>  
<span data-ttu-id="46843-235">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="46843-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="46843-236">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="46843-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="46843-237">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="46843-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="46843-240">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="46843-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="46843-241">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="46843-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="46843-242">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="46843-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="46843-243">詳細については、「[IHostingStartup を使用した外部アセンブリからのアプリ機能の追加](xref:host-and-deploy/ihostingstartup)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="46843-244">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="46843-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="46843-245">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="46843-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="46843-246">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="46843-246">**Default**: false</span></span>  
<span data-ttu-id="46843-247">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="46843-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="46843-248">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="46843-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="46843-249">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="46843-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="46843-252">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="46843-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="46843-253">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="46843-253">Server URLs</span></span>

<span data-ttu-id="46843-254">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="46843-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="46843-255">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="46843-255">**Key**: urls</span></span>  
<span data-ttu-id="46843-256">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="46843-256">**Type**: *string*</span></span>  
<span data-ttu-id="46843-257">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="46843-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="46843-258">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="46843-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="46843-259">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="46843-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="46843-260">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="46843-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="46843-261">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="46843-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="46843-262">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="46843-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="46843-263">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="46843-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="46843-264">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="46843-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="46843-266">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="46843-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="46843-267">詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="46843-269">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="46843-269">Shutdown Timeout</span></span>

<span data-ttu-id="46843-270">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="46843-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="46843-271">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="46843-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="46843-272">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="46843-272">**Type**: *int*</span></span>  
<span data-ttu-id="46843-273">**既定値**: 5</span><span class="sxs-lookup"><span data-stu-id="46843-273">**Default**: 5</span></span>  
<span data-ttu-id="46843-274">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="46843-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="46843-275">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="46843-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="46843-276">キーでは `UseSetting` が指定された *int* (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など) を受け入れますが、`UseShutdownTimeout` 拡張メソッドでは `TimeSpan` を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="46843-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="46843-277">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="46843-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="46843-280">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="46843-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="46843-281">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="46843-281">Startup Assembly</span></span>

<span data-ttu-id="46843-282">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="46843-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="46843-283">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="46843-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="46843-284">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="46843-284">**Type**: *string*</span></span>  
<span data-ttu-id="46843-285">**既定値**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="46843-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="46843-286">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="46843-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="46843-287">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="46843-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="46843-288">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="46843-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="46843-289">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="46843-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="46843-292">Web ルート</span><span class="sxs-lookup"><span data-stu-id="46843-292">Web Root</span></span>

<span data-ttu-id="46843-293">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="46843-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="46843-294">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="46843-294">**Key**: webroot</span></span>  
<span data-ttu-id="46843-295">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="46843-295">**Type**: *string*</span></span>  
<span data-ttu-id="46843-296">**既定値**: 指定されていない場合、既定値は "(Content Root)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="46843-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="46843-297">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="46843-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="46843-298">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="46843-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="46843-299">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="46843-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="46843-302">構成のオーバーライド</span><span class="sxs-lookup"><span data-stu-id="46843-302">Overriding configuration</span></span>

<span data-ttu-id="46843-303">[Configuration](xref:fundamentals/configuration/index) を使用して、ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="46843-304">次の例では、ホスト構成はオプションで *hosting.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="46843-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="46843-305">*hosting.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="46843-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="46843-306">(`config` の) ビルド構成は、`UseConfiguration` でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="46843-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="46843-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="46843-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="46843-309">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="46843-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="46843-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="46843-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="46843-312">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="46843-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="46843-313">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="46843-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="46843-314">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="46843-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="46843-315">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="46843-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="46843-316">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="46843-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="46843-317">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="46843-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="46843-318">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="46843-319">特定の URL でホストを実行するように指定する場合、`dotnet run` の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="46843-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="46843-320">コマンドライン引数は *hosting.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="46843-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="46843-321">ホストの起動</span><span class="sxs-lookup"><span data-stu-id="46843-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="46843-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="46843-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="46843-323">**実行**</span><span class="sxs-lookup"><span data-stu-id="46843-323">**Run**</span></span>

<span data-ttu-id="46843-324">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="46843-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="46843-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="46843-325">**Start**</span></span>

<span data-ttu-id="46843-326">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="46843-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="46843-327">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="46843-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="46843-328">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="46843-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="46843-329">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="46843-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="46843-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="46843-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="46843-331">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="46843-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="46843-332">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="46843-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="46843-333">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="46843-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="46843-334">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="46843-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="46843-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="46843-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="46843-336">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="46843-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="46843-337">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="46843-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="46843-338">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="46843-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="46843-339">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="46843-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="46843-340">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="46843-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="46843-341">要求</span><span class="sxs-lookup"><span data-stu-id="46843-341">Request</span></span>                                    | <span data-ttu-id="46843-342">応答</span><span class="sxs-lookup"><span data-stu-id="46843-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="46843-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="46843-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="46843-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="46843-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="46843-345">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="46843-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="46843-346">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="46843-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="46843-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="46843-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="46843-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="46843-348">Hello World!</span></span>                             |

<span data-ttu-id="46843-349">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="46843-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="46843-350">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="46843-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="46843-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="46843-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="46843-352">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="46843-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="46843-353">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action<IRouteBuilder> routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="46843-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="46843-354">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="46843-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="46843-355">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="46843-356">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="46843-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="46843-357">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="46843-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="46843-358">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="46843-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="46843-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="46843-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="46843-360">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="46843-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="46843-361">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action<IApplicationBuilder> app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="46843-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="46843-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="46843-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="46843-363">**実行**</span><span class="sxs-lookup"><span data-stu-id="46843-363">**Run**</span></span>

<span data-ttu-id="46843-364">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="46843-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="46843-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="46843-365">**Start**</span></span>

<span data-ttu-id="46843-366">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="46843-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="46843-367">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="46843-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="46843-368">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="46843-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="46843-369">[IHostingEnvironment インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="46843-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="46843-370">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="46843-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="46843-371">[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="46843-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="46843-372">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="46843-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="46843-373">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="46843-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="46843-374">詳細については、「[複数の環境の使用](xref:fundamentals/environments)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="46843-375">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="46843-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="46843-376">カスタムの[ミドルウェア](xref:fundamentals/middleware/index#writing-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="46843-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="46843-377">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="46843-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="46843-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="46843-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="46843-379">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="46843-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="46843-380">`StopApplication` メソッドもあります。</span><span class="sxs-lookup"><span data-stu-id="46843-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="46843-381">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="46843-381">Cancellation Token</span></span>    | <span data-ttu-id="46843-382">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="46843-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="46843-383">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="46843-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="46843-384">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="46843-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="46843-385">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="46843-385">Requests may still be processing.</span></span> <span data-ttu-id="46843-386">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="46843-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="46843-387">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="46843-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="46843-388">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="46843-388">All requests should be processed.</span></span> <span data-ttu-id="46843-389">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="46843-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="46843-390">メソッド</span><span class="sxs-lookup"><span data-stu-id="46843-390">Method</span></span>            | <span data-ttu-id="46843-391">アクション</span><span class="sxs-lookup"><span data-stu-id="46843-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="46843-392">現在のアプリケーションの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="46843-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="46843-393">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="46843-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="46843-394">**適用対象: ASP.NET Core 2.0 のみ**</span><span class="sxs-lookup"><span data-stu-id="46843-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="46843-395">`UseStartup` や `Configure` を呼び出すのではなく、依存関係挿入コンテナーに直接 `IStartup` を挿入して、ホストをビルドする場合があります。</span><span class="sxs-lookup"><span data-stu-id="46843-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="46843-396">ホストがこのようにビルドされている場合、以下のエラーが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="46843-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="46843-397">これは、`HostingStartupAttributes` をスキャンするために [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (現在のアセンブリ) が必要であるために発生します。</span><span class="sxs-lookup"><span data-stu-id="46843-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="46843-398">アプリで `IStartup` を手動で依存関係挿入コンテナーに挿入する場合は、指定されたアセンブリ名を使用して `WebHostBuilder` への次の呼び出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="46843-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="46843-399">あるいは、ダミーの `Configure` を `WebHostBuilder` に追加します。この場合、`applicationName`(`ApplicationKey`) が自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="46843-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="46843-400">**注**: これは、ASP.NET Core 2.0 リリースでのみ、およびアプリが `UseStartup` や `Configure` を呼び出さない場合にのみ必要になります。</span><span class="sxs-lookup"><span data-stu-id="46843-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="46843-401">詳細については、[お知らせのコメント「Microsoft.Extensions.PlatformAbstractions has been removed」](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Microsoft.Extensions.PlatformAbstractions が削除される) と [StartupInjection のサンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="46843-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46843-402">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="46843-402">Additional resources</span></span>

* [<span data-ttu-id="46843-403">IIS による Windows 上のホスト</span><span class="sxs-lookup"><span data-stu-id="46843-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="46843-404">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="46843-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="46843-405">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="46843-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="46843-406">Windows サービスでのホスト</span><span class="sxs-lookup"><span data-stu-id="46843-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
