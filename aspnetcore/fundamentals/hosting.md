---
title: ASP.NET Core でのホスティング
author: guardrex
description: ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 3dab68da1b2b24351d40266429bf25f26ba467bf
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="e5425-103">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="e5425-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="e5425-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e5425-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e5425-105">ASP.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="e5425-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="e5425-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="e5425-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e5425-107">少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="e5425-108">ホストの設定</span><span class="sxs-lookup"><span data-stu-id="e5425-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="e5425-110">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="e5425-111">通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="e5425-112">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="e5425-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="e5425-113">一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="e5425-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="e5425-114">`CreateDefaultBuilder` では次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="e5425-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="e5425-115">[Kestrel](servers/kestrel.md) を Web サーバーとして構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="e5425-116">Kestrel の既定のオプションについては、[「ASP.NET Core の Kestrel Web サーバーの実装の概要」の「Kestrel オプション」セクション](xref:fundamentals/servers/kestrel#kestrel-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="e5425-117">[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="e5425-118">次の場所から省略可能な構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="e5425-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="e5425-119">*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="e5425-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="e5425-120">*appsettings.{Environment}.json*。</span><span class="sxs-lookup"><span data-stu-id="e5425-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="e5425-121">`Development` 環境でアプリが実行される場合に使用される[ユーザー シークレット](xref:security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="e5425-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="e5425-122">環境変数。</span><span class="sxs-lookup"><span data-stu-id="e5425-122">Environment variables.</span></span>
  * <span data-ttu-id="e5425-123">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="e5425-123">Command-line arguments.</span></span>
* <span data-ttu-id="e5425-124">コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="e5425-125">ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e5425-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="e5425-126">IIS の背後での実行時に、[IIS 統合](xref:host-and-deploy/iis/index)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e5425-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="e5425-127">[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)の使用時にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="e5425-128">このモジュールは、IIS と Kestrel の間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="e5425-129">また、[スタートアップ エラーをキャプチャする](#capture-startup-errors)ようにアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="e5425-130">IIS の既定のオプションについては、[「IIS を使用した Windows での ASP.NET Core のホスト」の「IIS のオプション」セクション](xref:host-and-deploy/iis/index#iis-options)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="e5425-131">アプリの環境が開発の場合、[ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="e5425-132">詳しくは、「[スコープの検証](#scope-validation)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e5425-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="e5425-133">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="e5425-134">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="e5425-135">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="e5425-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="e5425-136">アプリの構成の詳細については、「[ASP.NET Core の構成](xref:fundamentals/configuration/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="e5425-137">静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e5425-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="e5425-138">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="e5425-140">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="e5425-141">通常、ホストの作成はアプリのエントリ ポイントの `Main` メソッドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="e5425-142">プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。</span><span class="sxs-lookup"><span data-stu-id="e5425-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="e5425-143">`WebHostBuilder` には、[IServer を実装するサーバー](servers/index.md)が必要です。</span><span class="sxs-lookup"><span data-stu-id="e5425-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="e5425-144">組み込みサーバーは [Kestrel](servers/kestrel.md) と [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 より前のリリースでは [WebListener](xref:fundamentals/servers/weblistener) と呼ばれていた) です。</span><span class="sxs-lookup"><span data-stu-id="e5425-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="e5425-145">この例では、[UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)で Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="e5425-146">*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="e5425-147">`UseContentRoot` の既定のコンテンツ ルートは、[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="e5425-148">プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="e5425-149">これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。</span><span class="sxs-lookup"><span data-stu-id="e5425-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="e5425-150">IIS をリバース プロキシとして使用するには、ホストのビルド時に [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e5425-150">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="e5425-151">`UseIISIntegration` では、[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) の場合のように*サーバー*は構成されません。</span><span class="sxs-lookup"><span data-stu-id="e5425-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="e5425-152">`UseIISIntegration` は、Kestrel と IIS の間にリバース プロキシを作成するために [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を使用する際にサーバーがリッスンする基本パスとポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="e5425-153">IIS と ASP.NET Core を一緒に使用するには、`UseKestrel` と `UseIISIntegration` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5425-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="e5425-154">`UseIISIntegration` は、IIS または IIS Express の背後で実行されている場合にのみアクティブになります。</span><span class="sxs-lookup"><span data-stu-id="e5425-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="e5425-155">詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="e5425-156">ホスト (および ASP.NET Core アプリ) を構成する最小限の実装には、アプリの要求パイプラインの構成とサーバーの指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e5425-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="e5425-157">ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。</span><span class="sxs-lookup"><span data-stu-id="e5425-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="e5425-158">`Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5425-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="e5425-159">詳細については、「[ASP.NET Core でのアプリケーションのスタートアップ](startup.md)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="e5425-160">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="e5425-161">`WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="e5425-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="e5425-162">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="e5425-162">Host configuration values</span></span>

<span data-ttu-id="e5425-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="e5425-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="e5425-164">`ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。</span><span class="sxs-lookup"><span data-stu-id="e5425-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="e5425-165">たとえば、`ASPNETCORE_URLS` のようにします。</span><span class="sxs-lookup"><span data-stu-id="e5425-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="e5425-166">`CaptureStartupErrors` などの明示的なメソッド。</span><span class="sxs-lookup"><span data-stu-id="e5425-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="e5425-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="e5425-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="e5425-168">`UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="e5425-169">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="e5425-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="e5425-170">詳細については、次のセクションの「[構成のオーバーライド](#overriding-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="e5425-171">スタートアップ エラーをキャプチャする</span><span class="sxs-lookup"><span data-stu-id="e5425-171">Capture Startup Errors</span></span>

<span data-ttu-id="e5425-172">この設定では、スタートアップ エラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="e5425-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="e5425-173">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="e5425-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="e5425-174">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="e5425-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e5425-175">**既定値**: アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="e5425-176">**次を使用して設定**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="e5425-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="e5425-177">**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="e5425-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="e5425-178">`false` の場合、起動時にエラーが発生するとホストが終了します。</span><span class="sxs-lookup"><span data-stu-id="e5425-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="e5425-179">`true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。</span><span class="sxs-lookup"><span data-stu-id="e5425-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="e5425-182">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="e5425-182">Content Root</span></span>

<span data-ttu-id="e5425-183">この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="e5425-184">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="e5425-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="e5425-185">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="e5425-185">**Type**: *string*</span></span>  
<span data-ttu-id="e5425-186">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="e5425-187">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="e5425-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="e5425-188">**環境変数**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="e5425-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="e5425-189">コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="e5425-190">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="e5425-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="e5425-193">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="e5425-193">Detailed Errors</span></span>

<span data-ttu-id="e5425-194">詳細なエラーをキャプチャするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="e5425-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="e5425-195">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="e5425-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="e5425-196">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="e5425-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e5425-197">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="e5425-197">**Default**: false</span></span>  
<span data-ttu-id="e5425-198">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e5425-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e5425-199">**環境変数**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="e5425-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="e5425-200">有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="e5425-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="e5425-203">環境</span><span class="sxs-lookup"><span data-stu-id="e5425-203">Environment</span></span>

<span data-ttu-id="e5425-204">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-204">Sets the app's environment.</span></span>

<span data-ttu-id="e5425-205">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="e5425-205">**Key**: environment</span></span>  
<span data-ttu-id="e5425-206">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="e5425-206">**Type**: *string*</span></span>  
<span data-ttu-id="e5425-207">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="e5425-207">**Default**: Production</span></span>  
<span data-ttu-id="e5425-208">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="e5425-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="e5425-209">**環境変数**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="e5425-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="e5425-210">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="e5425-210">The environment can be set to any value.</span></span> <span data-ttu-id="e5425-211">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e5425-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="e5425-212">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="e5425-212">Values aren't case sensitive.</span></span> <span data-ttu-id="e5425-213">既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。</span><span class="sxs-lookup"><span data-stu-id="e5425-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="e5425-214">[Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="e5425-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="e5425-215">詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-215">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="e5425-218">ホスティング スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="e5425-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="e5425-219">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="e5425-220">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="e5425-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="e5425-221">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="e5425-221">**Type**: *string*</span></span>  
<span data-ttu-id="e5425-222">**既定値**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="e5425-222">**Default**: Empty string</span></span>  
<span data-ttu-id="e5425-223">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e5425-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e5425-224">**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e5425-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="e5425-225">起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="e5425-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="e5425-226">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="e5425-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="e5425-227">構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e5425-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="e5425-228">ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e5425-231">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="e5425-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="e5425-232">ホスティング URL を優先する</span><span class="sxs-lookup"><span data-stu-id="e5425-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="e5425-233">`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="e5425-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="e5425-234">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="e5425-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="e5425-235">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="e5425-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e5425-236">**既定値**: true</span><span class="sxs-lookup"><span data-stu-id="e5425-236">**Default**: true</span></span>  
<span data-ttu-id="e5425-237">**次を使用して設定**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="e5425-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="e5425-238">**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="e5425-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="e5425-239">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="e5425-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e5425-242">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="e5425-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="e5425-243">ホスティング スタートアップを回避する</span><span class="sxs-lookup"><span data-stu-id="e5425-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="e5425-244">アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。</span><span class="sxs-lookup"><span data-stu-id="e5425-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="e5425-245">詳細については、「[Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration)」(外部アセンブリからアプリを拡張する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-245">See [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="e5425-246">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="e5425-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="e5425-247">**型**: *ブール* (`true` または `1`)</span><span class="sxs-lookup"><span data-stu-id="e5425-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="e5425-248">**既定値**: false</span><span class="sxs-lookup"><span data-stu-id="e5425-248">**Default**: false</span></span>  
<span data-ttu-id="e5425-249">**次を使用して設定**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e5425-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e5425-250">**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="e5425-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="e5425-251">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="e5425-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e5425-254">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="e5425-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="e5425-255">サーバーの URL</span><span class="sxs-lookup"><span data-stu-id="e5425-255">Server URLs</span></span>

<span data-ttu-id="e5425-256">サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="e5425-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="e5425-257">**キー**: urls</span><span class="sxs-lookup"><span data-stu-id="e5425-257">**Key**: urls</span></span>  
<span data-ttu-id="e5425-258">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="e5425-258">**Type**: *string*</span></span>  
<span data-ttu-id="e5425-259">**既定値**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="e5425-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="e5425-260">**次を使用して設定**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="e5425-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="e5425-261">**環境変数**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="e5425-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="e5425-262">サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="e5425-263">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="e5425-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="e5425-264">"\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。</span><span class="sxs-lookup"><span data-stu-id="e5425-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="e5425-265">プロトコル (`http://` または `https://`) は各 URL に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5425-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="e5425-266">サポートされている形式はサーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="e5425-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="e5425-268">Kestrel には独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="e5425-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="e5425-269">詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="e5425-271">シャットダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="e5425-271">Shutdown Timeout</span></span>

<span data-ttu-id="e5425-272">Web ホストがシャットダウンするまで待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="e5425-273">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="e5425-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="e5425-274">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="e5425-274">**Type**: *int*</span></span>  
<span data-ttu-id="e5425-275">**既定値**: 5</span><span class="sxs-lookup"><span data-stu-id="e5425-275">**Default**: 5</span></span>  
<span data-ttu-id="e5425-276">**次を使用して設定**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="e5425-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="e5425-277">**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="e5425-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="e5425-278">キーは `UseSetting` で *int* を受け取りますが (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など)、[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 拡張メソッドは [TimeSpan](/dotnet/api/system.timespan) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="e5425-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="e5425-279">これは ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="e5425-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="e5425-280">タイムアウト期間中、ホスティングは次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="e5425-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="e5425-281">[IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping) をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="e5425-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="e5425-282">ホステッド サービスの停止を試み、停止に失敗したサービスのエラーをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="e5425-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="e5425-283">すべてのホステッド サービスが停止する前にタイムアウト時間が切れた場合、残っているアクティブなサービスはアプリのシャットダウン時に停止します。</span><span class="sxs-lookup"><span data-stu-id="e5425-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="e5425-284">処理が完了していない場合でも、サービスは停止します。</span><span class="sxs-lookup"><span data-stu-id="e5425-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="e5425-285">サービスが停止するまでにさらに時間が必要な場合は、タイムアウト値を増やします。</span><span class="sxs-lookup"><span data-stu-id="e5425-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e5425-288">この機能は ASP.NET Core 1.x では使用できません。</span><span class="sxs-lookup"><span data-stu-id="e5425-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="e5425-289">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="e5425-289">Startup Assembly</span></span>

<span data-ttu-id="e5425-290">`Startup` クラスを検索するアセンブリを決定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="e5425-291">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="e5425-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="e5425-292">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="e5425-292">**Type**: *string*</span></span>  
<span data-ttu-id="e5425-293">**既定値**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="e5425-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="e5425-294">**次を使用して設定**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="e5425-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="e5425-295">**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="e5425-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="e5425-296">名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。</span><span class="sxs-lookup"><span data-stu-id="e5425-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="e5425-297">複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="e5425-300">Web ルート</span><span class="sxs-lookup"><span data-stu-id="e5425-300">Web Root</span></span>

<span data-ttu-id="e5425-301">アプリの静的資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="e5425-302">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="e5425-302">**Key**: webroot</span></span>  
<span data-ttu-id="e5425-303">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="e5425-303">**Type**: *string*</span></span>  
<span data-ttu-id="e5425-304">**既定値**: 指定されていない場合、既定値は "(Content Root)/wwwroot" となります (パスが存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="e5425-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="e5425-305">パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="e5425-306">**次を使用して設定**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="e5425-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="e5425-307">**環境変数**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="e5425-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="e5425-310">構成のオーバーライド</span><span class="sxs-lookup"><span data-stu-id="e5425-310">Overriding configuration</span></span>

<span data-ttu-id="e5425-311">[Configuration](xref:fundamentals/configuration/index) を使用して、ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="e5425-312">次の例では、ホスト構成はオプションで *hosting.json* ファイルに指定されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="e5425-313">*hosting.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="e5425-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="e5425-314">(`config` の) ビルド構成は、`UseConfiguration` でホストを構成する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e5425-316">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="e5425-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="e5425-317">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="e5425-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e5425-319">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="e5425-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="e5425-320">次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。</span><span class="sxs-lookup"><span data-stu-id="e5425-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="e5425-321">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="e5425-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="e5425-322">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="e5425-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="e5425-323">`UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。</span><span class="sxs-lookup"><span data-stu-id="e5425-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="e5425-324">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="e5425-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="e5425-325">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="e5425-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="e5425-326">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="e5425-327">特定の URL でホストを実行するように指定する場合、[dotnet run](/dotnet/core/tools/dotnet-run) の実行時にコマンド プロンプトから必要な値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="e5425-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="e5425-328">コマンドライン引数は *hosting.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="e5425-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="e5425-329">ホストの起動</span><span class="sxs-lookup"><span data-stu-id="e5425-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e5425-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e5425-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e5425-331">**実行**</span><span class="sxs-lookup"><span data-stu-id="e5425-331">**Run**</span></span>

<span data-ttu-id="e5425-332">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="e5425-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="e5425-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="e5425-333">**Start**</span></span>

<span data-ttu-id="e5425-334">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="e5425-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="e5425-335">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="e5425-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="e5425-336">アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。</span><span class="sxs-lookup"><span data-stu-id="e5425-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="e5425-337">これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="e5425-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="e5425-338">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="e5425-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="e5425-339">次のように `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="e5425-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="e5425-340">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="e5425-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="e5425-341">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="e5425-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e5425-342">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="e5425-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e5425-343">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="e5425-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="e5425-344">次のように、URL と `RequestDelegate` で始まります。</span><span class="sxs-lookup"><span data-stu-id="e5425-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="e5425-345">アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="e5425-346">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="e5425-346">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="e5425-347">次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="e5425-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="e5425-348">この例では、以下のブラウザー要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="e5425-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="e5425-349">要求</span><span class="sxs-lookup"><span data-stu-id="e5425-349">Request</span></span>                                    | <span data-ttu-id="e5425-350">応答</span><span class="sxs-lookup"><span data-stu-id="e5425-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="e5425-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="e5425-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="e5425-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="e5425-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="e5425-353">"ooops!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="e5425-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="e5425-354">"Uh oh!" という文字列がある場合は例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="e5425-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="e5425-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="e5425-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="e5425-356">Hello World!</span><span class="sxs-lookup"><span data-stu-id="e5425-356">Hello World!</span></span>                             |

<span data-ttu-id="e5425-357">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="e5425-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e5425-358">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="e5425-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e5425-359">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="e5425-359">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="e5425-360">次のように、URL と `IRouteBuilder` のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="e5425-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="e5425-361">アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action&lt;IRouteBuilder&gt; routeBuilder)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-361">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="e5425-362">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="e5425-362">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="e5425-363">次のように、デリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="e5425-364">"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。</span><span class="sxs-lookup"><span data-stu-id="e5425-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="e5425-365">`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。</span><span class="sxs-lookup"><span data-stu-id="e5425-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="e5425-366">アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="e5425-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="e5425-367">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="e5425-367">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="e5425-368">次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="e5425-369">アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action&lt;IApplicationBuilder&gt; app)** と同じ結果が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-369">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e5425-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e5425-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e5425-371">**実行**</span><span class="sxs-lookup"><span data-stu-id="e5425-371">**Run**</span></span>

<span data-ttu-id="e5425-372">`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="e5425-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="e5425-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="e5425-373">**Start**</span></span>

<span data-ttu-id="e5425-374">`Start` メソッドを呼び出して、ブロックせずにホストを実行します。</span><span class="sxs-lookup"><span data-stu-id="e5425-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="e5425-375">URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="e5425-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="e5425-376">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="e5425-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="e5425-377">[IHostingEnvironment インターフェイス](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="e5425-377">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="e5425-378">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="e5425-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="e5425-379">[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)を使用して、環境に基づいて起動時にアプリを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="e5425-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="e5425-380">あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。</span><span class="sxs-lookup"><span data-stu-id="e5425-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="e5425-381">`IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="e5425-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="e5425-382">詳細については、[複数の環境での使用](xref:fundamentals/environments)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="e5425-382">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="e5425-383">処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="e5425-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="e5425-384">カスタムの[ミドルウェア](xref:fundamentals/middleware/index#writing-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="e5425-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="e5425-385">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="e5425-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="e5425-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="e5425-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="e5425-387">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="e5425-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="e5425-388">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="e5425-388">Cancellation Token</span></span>    | <span data-ttu-id="e5425-389">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="e5425-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="e5425-390">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="e5425-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="e5425-391">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="e5425-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="e5425-392">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="e5425-392">Requests may still be processing.</span></span> <span data-ttu-id="e5425-393">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="e5425-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="e5425-394">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="e5425-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="e5425-395">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5425-395">All requests should be processed.</span></span> <span data-ttu-id="e5425-396">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="e5425-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="e5425-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="e5425-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="e5425-398">以下のクラスは、`Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="e5425-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="e5425-399">スコープの検証</span><span class="sxs-lookup"><span data-stu-id="e5425-399">Scope validation</span></span>

<span data-ttu-id="e5425-400">ASP.NET Core 2.0 以降では、アプリの環境が開発の場合、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) を `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e5425-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="e5425-401">`ValidateScopes` が `true` に設定されていると、既定のサービス プロバイダーはチェックを実行して次のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e5425-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="e5425-402">スコープ サービスが、ルート サービス プロバイダーによって直接的または間接的に解決されない。</span><span class="sxs-lookup"><span data-stu-id="e5425-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="e5425-403">スコープ サービスが、シングルトンに直接または間接に挿入されない。</span><span class="sxs-lookup"><span data-stu-id="e5425-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="e5425-404">[BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) が呼び出されると、ルート サービス プロバイダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="e5425-405">ルート サービス プロバイダーの有効期間は、プロバイダーがアプリで開始されるとアプリとサーバーの有効期間に対応し、アプリのシャットダウンには破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="e5425-406">スコープ サービスは、それを作成したコンテナーによって破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="e5425-407">ルート コンテナーにスコープ サービスが作成されると、サービスはアプリ/サーバーのシャットダウン時に、ルート コンテナーによってのみ破棄されるため、サービスの有効期間は実質的にシングルトンに昇格されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="e5425-408">`BuildServiceProvider` が呼び出されると、サービス スコープの検証がこれらの状況をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="e5425-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="e5425-409">運用環境も含めて、常にスコープを検証するには、ホスト ビルダー上の [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) で [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) を構成します。</span><span class="sxs-lookup"><span data-stu-id="e5425-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="e5425-410">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e5425-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="e5425-411">**適用対象: ASP.NET Core 2.0 のみ**</span><span class="sxs-lookup"><span data-stu-id="e5425-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="e5425-412">`UseStartup` や `Configure` を呼び出すのではなく、依存関係挿入コンテナーに直接 `IStartup` を挿入して、ホストをビルドする場合があります。</span><span class="sxs-lookup"><span data-stu-id="e5425-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="e5425-413">ホストがこのようにビルドされている場合、以下のエラーが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="e5425-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="e5425-414">これは、`HostingStartupAttributes` をスキャンするために [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (現在のアセンブリ) が必要であるために発生します。</span><span class="sxs-lookup"><span data-stu-id="e5425-414">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="e5425-415">アプリで `IStartup` を手動で依存関係挿入コンテナーに挿入する場合は、指定されたアセンブリ名を使用して `WebHostBuilder` への次の呼び出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5425-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="e5425-416">あるいは、ダミーの `Configure` を `WebHostBuilder` に追加します。この場合、`applicationName`(`ApplicationKey`) が自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="e5425-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="e5425-417">**注**: これは、ASP.NET Core 2.0 リリースでのみ、およびアプリが `UseStartup` や `Configure` を呼び出さない場合にのみ必要になります。</span><span class="sxs-lookup"><span data-stu-id="e5425-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="e5425-418">詳細については、[お知らせのコメント「Microsoft.Extensions.PlatformAbstractions has been removed」](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Microsoft.Extensions.PlatformAbstractions が削除される) と [StartupInjection のサンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e5425-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5425-419">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e5425-419">Additional resources</span></span>

* [<span data-ttu-id="e5425-420">IIS による Windows 上のホスト</span><span class="sxs-lookup"><span data-stu-id="e5425-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e5425-421">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="e5425-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="e5425-422">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="e5425-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="e5425-423">Windows サービスでのホスト</span><span class="sxs-lookup"><span data-stu-id="e5425-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
