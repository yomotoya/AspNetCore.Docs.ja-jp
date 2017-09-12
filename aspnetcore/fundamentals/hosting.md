---
title: "ASP.NET Core でのホスティング"
author: guardrex
description: "これは、アプリの起動と有効期間の管理を担当する ASP.NET Core の web ホストについて説明します。"
keywords: "ASP.NET Core web ホスト、IWebHost、WebHostBuilder、IHostingEnvironment、IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/10/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1a789ff1bc6b3e3af99419e7d74d3fb46bb2345
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="8cf30-104">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="8cf30-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="8cf30-105">によって[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8cf30-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8cf30-106">ASP.NET Core アプリケーションの構成および起動、*ホスト*、これはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8cf30-107">少なくともは、ホストは、サーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="8cf30-108">ホストの設定</span><span class="sxs-lookup"><span data-stu-id="8cf30-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8cf30-110">インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="8cf30-111">これは、アプリのエントリ ポイントで通常実行、`Main`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="8cf30-112">プロジェクト テンプレートで`Main`内にある*Program.cs*です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="8cf30-113">一般的な*Program.cs*呼び出し[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)ホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="8cf30-114">`CreateDefaultBuilder`次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="8cf30-115">構成[Kestrel](servers/kestrel.md) web サーバーとします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span>
* <span data-ttu-id="8cf30-116">コンテンツのルートに設定[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-116">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="8cf30-117">読み込みオプションの構成:</span><span class="sxs-lookup"><span data-stu-id="8cf30-117">Loads optional configuration from:</span></span>
  * <span data-ttu-id="8cf30-118">*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-118">*appsettings.json*.</span></span>
  * <span data-ttu-id="8cf30-119">*appsettings です。{環境} .json*です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-119">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="8cf30-120">[ユーザーの機密情報](xref:security/app-secrets)アプリを実行するときに、`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="8cf30-120">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="8cf30-121">環境変数。</span><span class="sxs-lookup"><span data-stu-id="8cf30-121">Environment variables.</span></span>
  * <span data-ttu-id="8cf30-122">コマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="8cf30-122">Command-line arguments.</span></span>
* <span data-ttu-id="8cf30-123">構成[ログ](xref:fundamentals/logging)コンソールとデバッグ出力の[ログ フィルター](xref:fundamentals/logging#log-filtering)のログ記録の構成セクションで指定された規則、*される appsettings.json*または*appsettings です。{環境} .json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8cf30-123">Configures [logging](xref:fundamentals/logging) for console and debug output with [log filtering](xref:fundamentals/logging#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="8cf30-124">IIS の背後にあるを実行するときのベース パスおよびを使用する場合、サーバーがリッスンするポートを構成することによって IIS 統合を有効に、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-124">When running behind IIS, enables IIS integration by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="8cf30-125">モジュールでは、Kestrel と IIS のリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-125">The module creates a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="8cf30-126">アプリの構成も行います[スタートアップ エラーがキャプチャされる](#capture-startup-errors)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-126">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span>

<span data-ttu-id="8cf30-127">*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-127">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="8cf30-128">既定のコンテンツのルートは[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-128">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="8cf30-129">これは、結果、ルート フォルダーから、アプリが開始されたときに、コンテンツのルートとして、web プロジェクトのルート フォルダーを使用して、(たとえば、呼び出し[実行 dotnet](/dotnet/core/tools/dotnet-run)プロジェクト フォルダーから)。</span><span class="sxs-lookup"><span data-stu-id="8cf30-129">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="8cf30-130">これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-130">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="8cf30-131">参照してください[ASP.NET Core の構成](xref:fundamentals/configuration)アプリの構成についての詳細。</span><span class="sxs-lookup"><span data-stu-id="8cf30-131">See [Configuration in ASP.NET Core](xref:fundamentals/configuration) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="8cf30-132">静的なを使用する代わりとして`CreateDefaultBuilder`からホストを作成するメソッド[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core でサポートされているアプローチは、2.x です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-132">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="8cf30-133">詳細については、ASP.NET Core 1.x タブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8cf30-133">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8cf30-135">インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-135">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="8cf30-136">これは、アプリのエントリ ポイントで通常実行、`Main`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-136">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="8cf30-137">プロジェクト テンプレートで`Main`内にある*Program.cs*です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-137">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="8cf30-138">次*Program.cs*を使用する方法を示します`WebHostBuilder`ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-138">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="8cf30-139">`WebHostBuilder`必要があります、 [IServer を実装しているサーバー](servers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-139">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="8cf30-140">組み込みのサーバーが[Kestrel](servers/kestrel.md)と[HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 のリリースでは、前に、HTTP.sys が呼び出された[WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="8cf30-140">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="8cf30-141">この例では、 [UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-141">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="8cf30-142">*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-142">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="8cf30-143">指定された既定のコンテンツのルート`UseContentRoot`は[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-143">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="8cf30-144">これは、結果、ルート フォルダーから、アプリが開始されたときに、コンテンツのルートとして、web プロジェクトのルート フォルダーを使用して、(たとえば、呼び出し[実行 dotnet](/dotnet/core/tools/dotnet-run)プロジェクト フォルダーから)。</span><span class="sxs-lookup"><span data-stu-id="8cf30-144">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="8cf30-145">これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-145">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="8cf30-146">リバース プロキシとして、IIS を使用するには、呼び出す[UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)ホストの作成の一部として。</span><span class="sxs-lookup"><span data-stu-id="8cf30-146">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="8cf30-147">`UseIISIntegration`構成されない、*サーバー*と同様、 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)はします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-147">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="8cf30-148">`UseIISIntegration`基本パスとを使用する場合、サーバーがリッスンするポートを構成、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)Kestrel と IIS のリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-148">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="8cf30-149">ASP.NET Core で IIS を使用して、両方を指定する必要があります`UseKestrel`と`UseIISIntegration`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-149">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="8cf30-150">`UseIISIntegration`IIS または IIS Express の内側で実行されるときにのみアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-150">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="8cf30-151">詳細については、次を参照してください。 [Introduction to ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)と[ASP.NET Core モジュール構成の参照](xref:hosting/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-151">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="8cf30-152">ホスト (および ASP.NET Core アプリケーション) を構成する最小限の実装には、サーバーおよびアプリケーションの要求パイプラインの構成の指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-152">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="8cf30-153">ホストをセットアップするときに使用できる[構成](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)と[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-153">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="8cf30-154">指定した場合、`Startup`クラスを定義する必要がありますが、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-154">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="8cf30-155">詳細については、次を参照してください。[で ASP.NET Core アプリケーションの起動](startup.md)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-155">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="8cf30-156">複数回呼び出す`ConfigureServices`互いに追加します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-156">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="8cf30-157">複数回呼び出す`Configure`または`UseStartup`上、`WebHostBuilder`以前の設定を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-157">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="8cf30-158">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="8cf30-158">Host configuration values</span></span>

<span data-ttu-id="8cf30-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)を直接設定することもできます、ホストでは、ほとんどの利用可能な構成値を設定するためのメソッドを提供[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting)と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="8cf30-159">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="8cf30-160">使用して値を設定するときに`UseSetting`種類にかかわらず、(引用符で囲まれた) 文字列として値を設定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-160">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="8cf30-161">スタートアップ エラーがキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-161">Capture Startup Errors</span></span>

<span data-ttu-id="8cf30-162">この設定は、スタートアップのエラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-162">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="8cf30-163">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="8cf30-163">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="8cf30-164">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="8cf30-164">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8cf30-165">**既定**: 既定値は`false`ここで、既定値は、IIS の背後にある Kestrel でアプリを実行する場合を除き、`true`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-165">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="8cf30-166">**使用して設定**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="8cf30-166">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="8cf30-167">ときに`false`起動の結果、終了ホスト中のエラーです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-167">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="8cf30-168">ときに`true`ホストが起動中に例外をキャプチャし、サーバーを起動しようとしています。</span><span class="sxs-lookup"><span data-stu-id="8cf30-168">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="8cf30-171">コンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="8cf30-171">Content Root</span></span>

<span data-ttu-id="8cf30-172">この設定は、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始位置を決定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-172">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="8cf30-173">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="8cf30-173">**Key**: contentRoot</span></span>  
<span data-ttu-id="8cf30-174">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="8cf30-174">**Type**: *string*</span></span>  
<span data-ttu-id="8cf30-175">**既定の**: アプリのアセンブリが存在するフォルダーの既定値です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-175">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="8cf30-176">**使用して設定**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="8cf30-176">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="8cf30-177">コンテンツのルートはのベース パスとしても使用される、 [Web ルート設定](#web-root)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-177">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="8cf30-178">パスが存在しない場合、ホストは、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-178">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="8cf30-181">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="8cf30-181">Detailed Errors</span></span>

<span data-ttu-id="8cf30-182">エラーをキャプチャする必要がある場合を判断します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-182">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="8cf30-183">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="8cf30-183">**Key**: detailedErrors</span></span>  
<span data-ttu-id="8cf30-184">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="8cf30-184">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8cf30-185">**既定の**: false</span><span class="sxs-lookup"><span data-stu-id="8cf30-185">**Default**: false</span></span>  
<span data-ttu-id="8cf30-186">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8cf30-186">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="8cf30-187">有効にすると (場合や、<a href="#environment">環境</a>に設定されている`Development`)、アプリは、詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-187">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-189">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-189">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="8cf30-190">環境</span><span class="sxs-lookup"><span data-stu-id="8cf30-190">Environment</span></span>

<span data-ttu-id="8cf30-191">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-191">Sets the app's environment.</span></span>

<span data-ttu-id="8cf30-192">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="8cf30-192">**Key**: environment</span></span>  
<span data-ttu-id="8cf30-193">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="8cf30-193">**Type**: *string*</span></span>  
<span data-ttu-id="8cf30-194">**既定の**: 実稼働環境</span><span class="sxs-lookup"><span data-stu-id="8cf30-194">**Default**: Production</span></span>  
<span data-ttu-id="8cf30-195">**使用して設定**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="8cf30-195">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="8cf30-196">設定することができます、*環境*任意の値にします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-196">You can set the *Environment* to any value.</span></span> <span data-ttu-id="8cf30-197">フレームワークによって定義された値が含まれます`Development`、 `Staging`、および`Production`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-197">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8cf30-198">値は、大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="8cf30-198">Values aren't case sensitive.</span></span> <span data-ttu-id="8cf30-199">既定では、*環境*から読み取られた、`ASPNETCORE_ENVIRONMENT`環境変数。</span><span class="sxs-lookup"><span data-stu-id="8cf30-199">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="8cf30-200">使用する場合[Visual Studio](https://www.visualstudio.com/)環境変数を設定することがあります、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8cf30-200">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="8cf30-201">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8cf30-201">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-202">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-203">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="8cf30-204">ホストしているスタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="8cf30-204">Hosting Startup Assemblies</span></span>

<span data-ttu-id="8cf30-205">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-205">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="8cf30-206">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="8cf30-206">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="8cf30-207">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="8cf30-207">**Type**: *string*</span></span>  
<span data-ttu-id="8cf30-208">**既定の**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="8cf30-208">**Default**: Empty string</span></span>  
<span data-ttu-id="8cf30-209">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8cf30-209">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="8cf30-210">起動時にロードするスタートアップ アセンブリをホストしているセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="8cf30-210">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="8cf30-211">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-211">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="8cf30-212">既定の構成値は、空の文字列、ホスティング スタートアップ アセンブリは常に、アプリのアセンブリを含めます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-212">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="8cf30-213">スタートアップ アセンブリのホスティングを提供する場合は、アプリが起動中に、一般的なサービスを構築するときに読み込むためのアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-213">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8cf30-216">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-216">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="8cf30-217">Url のホストを優先します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-217">Prefer Hosting URLs</span></span>

<span data-ttu-id="8cf30-218">構成されている Url に、ホストがリッスンするかどうかを示す、`WebHostBuilder`で構成されているものではなく、`IServer`実装します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-218">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="8cf30-219">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="8cf30-219">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="8cf30-220">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="8cf30-220">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8cf30-221">**既定の**: true</span><span class="sxs-lookup"><span data-stu-id="8cf30-221">**Default**: true</span></span>  
<span data-ttu-id="8cf30-222">**使用して設定**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="8cf30-222">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="8cf30-223">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-223">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-224">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-224">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8cf30-226">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-226">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="8cf30-227">スタートアップをホストしているようにします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-227">Prevent Hosting Startup</span></span>

<span data-ttu-id="8cf30-228">ホストしているアプリのアセンブリを含め、スタートアップ アセンブリの自動読み込みができなくなります。</span><span class="sxs-lookup"><span data-stu-id="8cf30-228">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="8cf30-229">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="8cf30-229">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="8cf30-230">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="8cf30-230">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="8cf30-231">**既定の**: false</span><span class="sxs-lookup"><span data-stu-id="8cf30-231">**Default**: false</span></span>  
<span data-ttu-id="8cf30-232">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="8cf30-232">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="8cf30-233">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-233">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-234">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-234">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-235">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-235">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8cf30-236">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-236">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="8cf30-237">サーバーの Url</span><span class="sxs-lookup"><span data-stu-id="8cf30-237">Server URLs</span></span>

<span data-ttu-id="8cf30-238">IP アドレスまたはサーバーが要求をリッスンするポートとプロトコルとなるホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-238">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="8cf30-239">**キー**: url</span><span class="sxs-lookup"><span data-stu-id="8cf30-239">**Key**: urls</span></span>  
<span data-ttu-id="8cf30-240">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="8cf30-240">**Type**: *string*</span></span>  
<span data-ttu-id="8cf30-241">**既定の**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="8cf30-241">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="8cf30-242">**使用して設定**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="8cf30-242">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="8cf30-243">セミコロンで区切られたに設定 (;) の URL の一覧のプレフィックスに、サーバーが応答する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cf30-243">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="8cf30-244">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-244">For example, `http://localhost:123`.</span></span> <span data-ttu-id="8cf30-245">使用して"\*"を示す、サーバーは、上の IP アドレスまたはホスト名の指定したポートとプロトコルを使用して要求をリッスンする必要があります (たとえば、 `http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="8cf30-245">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="8cf30-246">プロトコル (`http://`または`https://`) 各 url を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cf30-246">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="8cf30-247">サポートされている形式は、サーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="8cf30-247">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="8cf30-249">Kestrel には、独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="8cf30-249">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="8cf30-250">詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8cf30-250">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="8cf30-252">シャット ダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="8cf30-252">Shutdown Timeout</span></span>

<span data-ttu-id="8cf30-253">Web ホストのシャット ダウンを待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-253">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="8cf30-254">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="8cf30-254">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="8cf30-255">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="8cf30-255">**Type**: *int*</span></span>  
<span data-ttu-id="8cf30-256">**既定の**: 5</span><span class="sxs-lookup"><span data-stu-id="8cf30-256">**Default**: 5</span></span>  
<span data-ttu-id="8cf30-257">**使用して設定**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="8cf30-257">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="8cf30-258">キーを受け入れますが、 *int*で`UseSetting`(たとえば、 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) では、`UseShutdownTimeout`拡張メソッドは、`TimeSpan`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-258">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="8cf30-259">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-259">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-260">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-260">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-261">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-261">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8cf30-262">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-262">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="8cf30-263">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="8cf30-263">Startup Assembly</span></span>

<span data-ttu-id="8cf30-264">検索対象となるアセンブリの決定、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-264">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="8cf30-265">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="8cf30-265">**Key**: startupAssembly</span></span>  
<span data-ttu-id="8cf30-266">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="8cf30-266">**Type**: *string*</span></span>  
<span data-ttu-id="8cf30-267">**既定の**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="8cf30-267">**Default**: The app's assembly</span></span>  
<span data-ttu-id="8cf30-268">**使用して設定**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="8cf30-268">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="8cf30-269">名前でアセンブリを参照することができます (`string`) または型 (`TStartup`)。</span><span class="sxs-lookup"><span data-stu-id="8cf30-269">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="8cf30-270">複数`UseStartup`メソッドが呼び出されると、最後の 1 つが優先されます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-270">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-271">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-271">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="8cf30-273">Web ルート</span><span class="sxs-lookup"><span data-stu-id="8cf30-273">Web Root</span></span>

<span data-ttu-id="8cf30-274">アプリの静的な資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-274">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="8cf30-275">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="8cf30-275">**Key**: webroot</span></span>  
<span data-ttu-id="8cf30-276">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="8cf30-276">**Type**: *string*</span></span>  
<span data-ttu-id="8cf30-277">**既定**: が指定されていない、既定値は"(Content Root) wwwroot/"パスが存在する場合は、します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-277">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="8cf30-278">パスが存在しない、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-278">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="8cf30-279">**使用して設定**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="8cf30-279">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-280">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-280">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-281">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-281">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="8cf30-282">構成をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-282">Overriding configuration</span></span>

<span data-ttu-id="8cf30-283">使用して[構成](configuration.md)ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-283">Use [Configuration](configuration.md) to configure the host.</span></span> <span data-ttu-id="8cf30-284">次の例では、ホストの構成は必要に応じてで指定された、 *hosting.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8cf30-284">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="8cf30-285">すべての構成から読み込まれました、 *hosting.json*ファイルはコマンドライン引数によってオーバーライドされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8cf30-285">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="8cf30-286">ビルドの構成 (で`config`) でホストを構成するために使用`UseConfiguration`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-286">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-287">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-287">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8cf30-288">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="8cf30-288">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="8cf30-289">によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。</span><span class="sxs-lookup"><span data-stu-id="8cf30-289">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-290">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-290">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8cf30-291">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="8cf30-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="8cf30-292">によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。</span><span class="sxs-lookup"><span data-stu-id="8cf30-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="8cf30-293">`UseConfiguration`拡張メソッドは現在によって返される構成セクションを解析できる`GetSection`(たとえば、`.UseConfiguration(Configuration.GetSection("section"))`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-293">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="8cf30-294">`GetSection`メソッド要求のセクションに構成キーをフィルター処理が残りますセクションの名前のキーに (たとえば、 `section:urls`、 `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="8cf30-294">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="8cf30-295">`UseConfiguration`メソッドに一致するキーが必要ですが、`WebHostBuilder`キー (たとえば、 `urls`、 `environment`)。</span><span class="sxs-lookup"><span data-stu-id="8cf30-295">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="8cf30-296">キーにセクション名の有無は、セクションの値が、ホストを構成することを防止します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-296">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="8cf30-297">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-297">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="8cf30-298">詳細と回避策については、次を参照してください。[フル キーを使用して WebHostBuilder.UseConfiguration に構成セクションを渡して](https://github.com/aspnet/Hosting/issues/839)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-298">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="8cf30-299">特定の URL で実行するホストを指定するには、でしたに合格する目的の値で、コマンド プロンプトから実行するときに`dotnet run`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-299">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="8cf30-300">コマンドラインの引数よりも優先、`urls`値から、 *hosting.json*ファイル、および、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-300">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="8cf30-301">重要度の順序</span><span class="sxs-lookup"><span data-stu-id="8cf30-301">Ordering importance</span></span>

<span data-ttu-id="8cf30-302">一部の`WebHostBuilder`場合の設定は環境変数から読み込ま最初に設定します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-302">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="8cf30-303">これらの環境変数の形式を使用する`ASPNETCORE_{configurationKey}`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-303">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="8cf30-304">既定では、サーバーがリッスンする Url を設定するに設定する`ASPNETCORE_URLS`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-304">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="8cf30-305">構成を指定することによってこれらの環境変数の値のいずれかをオーバーライドすることができます (を使用して`UseConfiguration`) または値を明示的に設定して (を使用して`UseSetting`または明示的な拡張メソッドのいずれかのように`UseUrls`)。</span><span class="sxs-lookup"><span data-stu-id="8cf30-305">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="8cf30-306">ホストは、どちらのオプションが最後の値を設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-306">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="8cf30-307">プログラムで 1 つの値に設定されて、既定の URL が構成をオーバーライドすることを許可する場合は、URL を設定した後のコマンドライン構成を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-307">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="8cf30-308">参照してください[オーバーライド構成](#overriding-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-308">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="8cf30-309">ホストの開始</span><span class="sxs-lookup"><span data-stu-id="8cf30-309">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8cf30-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8cf30-311">**実行**</span><span class="sxs-lookup"><span data-stu-id="8cf30-311">**Run**</span></span>

<span data-ttu-id="8cf30-312">`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-312">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="8cf30-313">**Start**</span><span class="sxs-lookup"><span data-stu-id="8cf30-313">**Start**</span></span>

<span data-ttu-id="8cf30-314">非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="8cf30-314">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="8cf30-315">Url の一覧を渡す場合、`Start`メソッド、指定された Url のリッスンがします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-315">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="8cf30-316">初期化の事前構成済みの既定値を使用して、新しいホストを開始できます`CreateDefaultBuilder`便利な静的メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-316">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="8cf30-317">これらのメソッドは、コンソール出力とサーバーを起動[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)改行 (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-317">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="8cf30-318">**開始 (RequestDelegate アプリ)**</span><span class="sxs-lookup"><span data-stu-id="8cf30-318">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="8cf30-319">始まる、 `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="8cf30-319">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="8cf30-320">要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには</span><span class="sxs-lookup"><span data-stu-id="8cf30-320">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="8cf30-321">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-321">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8cf30-322">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-322">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8cf30-323">**開始 (文字列 url、RequestDelegate アプリ)**</span><span class="sxs-lookup"><span data-stu-id="8cf30-323">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="8cf30-324">URL を使用して起動し、 `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="8cf30-324">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="8cf30-325">同じ結果を生成する**開始 (RequestDelegate app)**、上の応答をアプリを除く`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-325">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="8cf30-326">**開始 (アクション<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="8cf30-326">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="8cf30-327">インスタンスを使用して`IRouteBuilder`([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) ルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-327">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="8cf30-328">次のブラウザーの要求の例を使用します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-328">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="8cf30-329">要求</span><span class="sxs-lookup"><span data-stu-id="8cf30-329">Request</span></span>                                    | <span data-ttu-id="8cf30-330">応答</span><span class="sxs-lookup"><span data-stu-id="8cf30-330">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="8cf30-331">こんにちは, Martin!</span><span class="sxs-lookup"><span data-stu-id="8cf30-331">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="8cf30-332">ブエノスアイレス dias、Catrina!</span><span class="sxs-lookup"><span data-stu-id="8cf30-332">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="8cf30-333">"Ooops!"は文字列で例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-333">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="8cf30-334">文字列を使用して例外をスロー"Uh $embedded$!"</span><span class="sxs-lookup"><span data-stu-id="8cf30-334">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="8cf30-335">Sante、Kevin!</span><span class="sxs-lookup"><span data-stu-id="8cf30-335">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="8cf30-336">ハローワールド！</span><span class="sxs-lookup"><span data-stu-id="8cf30-336">Hello World!</span></span>                             |

<span data-ttu-id="8cf30-337">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-337">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8cf30-338">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-338">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8cf30-339">**開始 (文字列の url をアクション<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="8cf30-339">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="8cf30-340">インスタンスおよび URL を使用して`IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8cf30-340">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="8cf30-341">同じ結果**開始 (アクション<IRouteBuilder>routeBuilder)**、アプリを除く応答`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-341">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="8cf30-342">**始める (アクション<IApplicationBuilder>アプリ)**</span><span class="sxs-lookup"><span data-stu-id="8cf30-342">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="8cf30-343">構成するデリゲートを指定、 `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8cf30-343">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="8cf30-344">要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには</span><span class="sxs-lookup"><span data-stu-id="8cf30-344">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="8cf30-345">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-345">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="8cf30-346">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-346">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="8cf30-347">**始める (文字列の url をアクション<IApplicationBuilder>アプリ)**</span><span class="sxs-lookup"><span data-stu-id="8cf30-347">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="8cf30-348">URL および構成するデリゲートを提供する`IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8cf30-348">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="8cf30-349">同じ結果を生成する**で始める (アクション<IApplicationBuilder>アプリ)**、上の応答をアプリを除く`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-349">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8cf30-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8cf30-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8cf30-351">**実行**</span><span class="sxs-lookup"><span data-stu-id="8cf30-351">**Run**</span></span>

<span data-ttu-id="8cf30-352">`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-352">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="8cf30-353">**Start**</span><span class="sxs-lookup"><span data-stu-id="8cf30-353">**Start**</span></span>

<span data-ttu-id="8cf30-354">非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="8cf30-354">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="8cf30-355">Url の一覧を渡す場合、`Start`メソッド、指定された Url のリッスンがします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-355">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="8cf30-356">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="8cf30-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="8cf30-357">[IHostingEnvironment インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)アプリの web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-357">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="8cf30-358">使用することができます[コンス トラクター インジェクション](xref:fundamentals/dependency-injection)を取得する、`IHostingEnvironment`プロパティと拡張メソッドを使用するのには。</span><span class="sxs-lookup"><span data-stu-id="8cf30-358">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="8cf30-359">使用することができます、[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)環境に基づいて起動時に、アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-359">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="8cf30-360">また、挿入することができます、`IHostingEnvironment`に、`Startup`で使用するためのコンス トラクター `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8cf30-360">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="8cf30-361">加え、`IsDevelopment`の拡張メソッドで`IHostingEnvironment`提供`IsStaging`、 `IsProduction`、および`IsEnvironment(string environmentName)`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="8cf30-362">参照してください[複数の環境で作業](xref:fundamentals/environments)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-362">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="8cf30-363">`IHostingEnvironment`サービスは直接にも挿入され、`Configure`処理パイプラインを設定するためのメソッド。</span><span class="sxs-lookup"><span data-stu-id="8cf30-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="8cf30-364">挿入することできます`IHostingEnvironment`に、`Invoke`メソッド カスタムを作成するときに[ミドルウェア](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="8cf30-364">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="8cf30-365">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="8cf30-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="8cf30-366">[IApplicationLifetime インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime)後スタートアップおよびシャット ダウンのアクティビティを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="8cf30-366">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="8cf30-367">インターフェイス上の 3 つのプロパティは、キャンセル トークンに登録することができます、`Action`スタートアップおよびシャット ダウン イベントを定義するメソッド。</span><span class="sxs-lookup"><span data-stu-id="8cf30-367">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="8cf30-368">`StopApplication`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8cf30-368">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="8cf30-369">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="8cf30-369">Cancellation Token</span></span>    | <span data-ttu-id="8cf30-370">&#8230; をトリガー</span><span class="sxs-lookup"><span data-stu-id="8cf30-370">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="8cf30-371">ホストが完全に開始されました。</span><span class="sxs-lookup"><span data-stu-id="8cf30-371">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="8cf30-372">ホストが正常なシャット ダウンを実行しています。</span><span class="sxs-lookup"><span data-stu-id="8cf30-372">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="8cf30-373">要求はまだ処理中です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-373">Requests may still be processing.</span></span> <span data-ttu-id="8cf30-374">このイベントが完了するまで、ブロックをシャット ダウンします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-374">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="8cf30-375">ホストが正常なシャット ダウンを完了しています。</span><span class="sxs-lookup"><span data-stu-id="8cf30-375">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="8cf30-376">すべての要求を完全に処理される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cf30-376">All requests should be completely processed.</span></span> <span data-ttu-id="8cf30-377">このイベントが完了するまで、ブロックをシャット ダウンします。</span><span class="sxs-lookup"><span data-stu-id="8cf30-377">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="8cf30-378">メソッド</span><span class="sxs-lookup"><span data-stu-id="8cf30-378">Method</span></span>            | <span data-ttu-id="8cf30-379">操作</span><span class="sxs-lookup"><span data-stu-id="8cf30-379">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="8cf30-380">現在のアプリケーションの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-380">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="8cf30-381">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="8cf30-381">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="8cf30-382">**ASP.NET Core 2.0 のみに適用されます。**</span><span class="sxs-lookup"><span data-stu-id="8cf30-382">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="8cf30-383">挿入することで、ホストをビルドする場合`IStartup`呼び出しではなく、依存性の注入コンテナーに直接`UseStartup`または`Configure`、次のエラーが発生する可能性があります:`Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-383">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="8cf30-384">これは、ために発生、 [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey)をスキャンする (現在のアセンブリ) が必要な`HostingStartupAttributes`します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-384">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="8cf30-385">手動で挿入した場合`IStartup`依存関係の注入コンテナー内に、次の呼び出しを追加、`WebHostBuilder`指定されたアセンブリ名で。</span><span class="sxs-lookup"><span data-stu-id="8cf30-385">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="8cf30-386">代わりに、ダミーの追加`Configure`を`WebHostBuilder`、設定する、 `applicationName`(`ApplicationKey`) に自動的に。</span><span class="sxs-lookup"><span data-stu-id="8cf30-386">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="8cf30-387">**注**: これは、ASP.NET Core 2.0 リリースで必要なだけ場合にのみ呼び出すことがない`UseStartup`または`Configure`です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-387">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="8cf30-388">詳細については、次を参照してください。[お知らせ: Microsoft.Extensions.PlatformAbstractions がされました (コメント) を削除](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)と[StartupInjection サンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)です。</span><span class="sxs-lookup"><span data-stu-id="8cf30-388">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8cf30-389">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8cf30-389">Additional resources</span></span>

* [<span data-ttu-id="8cf30-390">IIS を使用して Windows への公開します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-390">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="8cf30-391">Nginx を使用して Linux への公開します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-391">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="8cf30-392">Apache を使用して Linux への公開します。</span><span class="sxs-lookup"><span data-stu-id="8cf30-392">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="8cf30-393">Windows サービスのホスト</span><span class="sxs-lookup"><span data-stu-id="8cf30-393">Host in a Windows Service</span></span>](xref:hosting/windows-service)
