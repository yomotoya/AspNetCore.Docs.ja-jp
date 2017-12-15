---
title: "ASP.NET Core でのホスティング"
author: guardrex
description: "これは、アプリの起動と有効期間の管理を担当する ASP.NET Core の web ホストについて説明します。"
keywords: "ASP.NET Core web ホスト、IWebHost、WebHostBuilder、IHostingEnvironment、IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: dfec2a67112d40b528b97c847da3dda8ef1e63bd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="3b4ff-104">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="3b4ff-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="3b4ff-105">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3b4ff-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3b4ff-106">ASP.NET Core アプリは、アプリの起動と有効期間の管理を担当する*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3b4ff-107">少なくともは、ホストは、サーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="3b4ff-108">ホストの設定</span><span class="sxs-lookup"><span data-stu-id="3b4ff-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3b4ff-110">インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="3b4ff-111">これは、アプリのエントリ ポイントで通常実行、`Main`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="3b4ff-112">プロジェクト テンプレートで`Main`内にある*Program.cs*です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="3b4ff-113">一般的な*Program.cs*呼び出し[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)ホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="3b4ff-114">`CreateDefaultBuilder`次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="3b4ff-115">構成[Kestrel](servers/kestrel.md) web サーバーとします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="3b4ff-116">Kestrel 既定のオプションを参照してください。 [Kestrel ASP.NET Core での web サーバーの実装のセクションで [オプション]、Kestrel](xref:fundamentals/servers/kestrel#kestrel-options)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="3b4ff-117">コンテンツのルートに設定[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="3b4ff-118">読み込みオプションの構成:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="3b4ff-119">*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="3b4ff-120">*appsettings です。{環境} .json*です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="3b4ff-121">[ユーザーの機密情報](xref:security/app-secrets)アプリを実行するときに、`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="3b4ff-122">環境変数。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-122">Environment variables.</span></span>
  * <span data-ttu-id="3b4ff-123">コマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-123">Command-line arguments.</span></span>
* <span data-ttu-id="3b4ff-124">構成[ログ](xref:fundamentals/logging/index)コンソールとデバッグ出力の[ログ フィルター](xref:fundamentals/logging/index#log-filtering)のログ記録の構成セクションで指定された規則、*される appsettings.json*または*appsettings です。{環境} .json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="3b4ff-125">により、IIS の背後にある実行中、 [IIS 統合](xref:publishing/iis)のベース パスおよびポートを構成することによって、サーバーがリッスンを使用する場合、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="3b4ff-126">モジュールでは、IIS と Kestrel リバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="3b4ff-127">アプリの構成も行います[スタートアップ エラーがキャプチャされる](#capture-startup-errors)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="3b4ff-128">IIS の既定のオプションを参照してください。 [、IIS が IIS と Windows 上のホスト ASP.NET Core のセクションをオプション](xref:publishing/iis#iis-options)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="3b4ff-129">*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="3b4ff-130">既定のコンテンツのルートは[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="3b4ff-131">これは、結果、ルート フォルダーから、アプリが開始されたときに、コンテンツのルートとして、web プロジェクトのルート フォルダーを使用して、(たとえば、呼び出し[実行 dotnet](/dotnet/core/tools/dotnet-run)プロジェクト フォルダーから)。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="3b4ff-132">これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="3b4ff-133">参照してください[ASP.NET Core の構成](xref:fundamentals/configuration/index)アプリの構成についての詳細。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="3b4ff-134">静的なを使用する代わりとして`CreateDefaultBuilder`からホストを作成するメソッド[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core でサポートされているアプローチは、2.x です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="3b4ff-135">詳細については、ASP.NET Core 1.x のタブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b4ff-137">インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="3b4ff-138">これは、アプリのエントリ ポイントで通常実行、`Main`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="3b4ff-139">プロジェクト テンプレートで`Main`内にある*Program.cs*です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="3b4ff-140">次*Program.cs*を使用する方法を示します`WebHostBuilder`ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="3b4ff-141">`WebHostBuilder`必要があります、 [IServer を実装しているサーバー](servers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="3b4ff-142">組み込みのサーバーが[Kestrel](servers/kestrel.md)と[HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 のリリースでは、前に、HTTP.sys が呼び出された[WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="3b4ff-143">この例では、 [UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="3b4ff-144">*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="3b4ff-145">指定された既定のコンテンツのルート`UseContentRoot`は[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="3b4ff-146">これは、結果、ルート フォルダーから、アプリが開始されたときに、コンテンツのルートとして、web プロジェクトのルート フォルダーを使用して、(たとえば、呼び出し[実行 dotnet](/dotnet/core/tools/dotnet-run)プロジェクト フォルダーから)。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="3b4ff-147">これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="3b4ff-148">リバース プロキシとして、IIS を使用するには、呼び出す[UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)ホストの作成の一部として。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="3b4ff-149">`UseIISIntegration`構成されない、*サーバー*と同様、 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)はします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="3b4ff-150">`UseIISIntegration`基本パスとを使用する場合、サーバーがリッスンするポートを構成、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)Kestrel と IIS のリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="3b4ff-151">ASP.NET Core で IIS を使用して、両方を指定する必要があります`UseKestrel`と`UseIISIntegration`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="3b4ff-152">`UseIISIntegration`IIS または IIS Express の内側で実行されるときにのみアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="3b4ff-153">詳細については、次を参照してください。 [Introduction to ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)と[ASP.NET Core モジュール構成の参照](xref:hosting/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="3b4ff-154">ホスト (および ASP.NET Core アプリケーション) を構成する最小限の実装には、サーバーおよびアプリケーションの要求パイプラインの構成の指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="3b4ff-155">ホストをセットアップするときに使用できる[構成](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)と[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="3b4ff-156">指定した場合、`Startup`クラスを定義する必要がありますが、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="3b4ff-157">詳細については、次を参照してください。[で ASP.NET Core アプリケーションの起動](startup.md)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="3b4ff-158">複数回呼び出す`ConfigureServices`互いに追加します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="3b4ff-159">複数回呼び出す`Configure`または`UseStartup`上、`WebHostBuilder`以前の設定を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="3b4ff-160">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="3b4ff-160">Host configuration values</span></span>

<span data-ttu-id="3b4ff-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)を直接設定することもできます、ホストでは、ほとんどの利用可能な構成値を設定するためのメソッドを提供[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting)と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="3b4ff-162">使用して値を設定するときに`UseSetting`種類にかかわらず、(引用符で囲まれた) 文字列として値を設定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="3b4ff-163">スタートアップ エラーがキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-163">Capture Startup Errors</span></span>

<span data-ttu-id="3b4ff-164">この設定は、スタートアップのエラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="3b4ff-165">**キー**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="3b4ff-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="3b4ff-166">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="3b4ff-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3b4ff-167">**既定**: 既定値は`false`ここで、既定値は、IIS の背後にある Kestrel でアプリを実行する場合を除き、`true`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="3b4ff-168">**使用して設定**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="3b4ff-169">ときに`false`起動の結果、終了ホスト中のエラーです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="3b4ff-170">ときに`true`ホストが起動中に例外をキャプチャし、サーバーを起動しようとしています。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="3b4ff-173">コンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="3b4ff-173">Content Root</span></span>

<span data-ttu-id="3b4ff-174">この設定は、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始位置を決定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="3b4ff-175">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="3b4ff-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="3b4ff-176">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="3b4ff-176">**Type**: *string*</span></span>  
<span data-ttu-id="3b4ff-177">**既定の**: アプリのアセンブリが存在するフォルダーの既定値です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="3b4ff-178">**使用して設定**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="3b4ff-179">コンテンツのルートはのベース パスとしても使用される、 [Web ルート設定](#web-root)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="3b4ff-180">パスが存在しない場合、ホストは、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="3b4ff-183">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="3b4ff-183">Detailed Errors</span></span>

<span data-ttu-id="3b4ff-184">エラーをキャプチャする必要がある場合を判断します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="3b4ff-185">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="3b4ff-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="3b4ff-186">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="3b4ff-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3b4ff-187">**既定の**: false</span><span class="sxs-lookup"><span data-stu-id="3b4ff-187">**Default**: false</span></span>  
<span data-ttu-id="3b4ff-188">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="3b4ff-189">有効にすると (場合や、<a href="#environment">環境</a>に設定されている`Development`)、アプリは、詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="3b4ff-192">環境</span><span class="sxs-lookup"><span data-stu-id="3b4ff-192">Environment</span></span>

<span data-ttu-id="3b4ff-193">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-193">Sets the app's environment.</span></span>

<span data-ttu-id="3b4ff-194">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="3b4ff-194">**Key**: environment</span></span>  
<span data-ttu-id="3b4ff-195">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="3b4ff-195">**Type**: *string*</span></span>  
<span data-ttu-id="3b4ff-196">**既定の**: 実稼働環境</span><span class="sxs-lookup"><span data-stu-id="3b4ff-196">**Default**: Production</span></span>  
<span data-ttu-id="3b4ff-197">**使用して設定**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="3b4ff-198">設定することができます、*環境*任意の値にします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="3b4ff-199">フレームワークによって定義された値が含まれます`Development`、 `Staging`、および`Production`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="3b4ff-200">値は、大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-200">Values aren't case sensitive.</span></span> <span data-ttu-id="3b4ff-201">既定では、*環境*から読み取られた、`ASPNETCORE_ENVIRONMENT`環境変数。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="3b4ff-202">使用する場合[Visual Studio](https://www.visualstudio.com/)環境変数を設定することがあります、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="3b4ff-203">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="3b4ff-206">ホストしているスタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="3b4ff-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="3b4ff-207">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="3b4ff-208">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="3b4ff-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="3b4ff-209">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="3b4ff-209">**Type**: *string*</span></span>  
<span data-ttu-id="3b4ff-210">**既定の**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="3b4ff-210">**Default**: Empty string</span></span>  
<span data-ttu-id="3b4ff-211">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="3b4ff-212">起動時にロードするスタートアップ アセンブリをホストしているセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="3b4ff-213">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="3b4ff-214">既定の構成値は、空の文字列、ホスティング スタートアップ アセンブリは常に、アプリのアセンブリを含めます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="3b4ff-215">スタートアップ アセンブリのホスティングを提供する場合は、アプリが起動中に、一般的なサービスを構築するときに読み込むためのアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b4ff-218">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="3b4ff-219">Url のホストを優先します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="3b4ff-220">構成されている Url に、ホストがリッスンするかどうかを示す、`WebHostBuilder`で構成されているものではなく、`IServer`実装します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="3b4ff-221">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="3b4ff-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="3b4ff-222">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="3b4ff-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3b4ff-223">**既定の**: true</span><span class="sxs-lookup"><span data-stu-id="3b4ff-223">**Default**: true</span></span>  
<span data-ttu-id="3b4ff-224">**使用して設定**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="3b4ff-225">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b4ff-228">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="3b4ff-229">スタートアップをホストしているようにします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="3b4ff-230">ホストしているアプリのアセンブリで構成されているスタートアップ アセンブリのホストを含め、スタートアップ アセンブリの自動読み込みができなくなります。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-230">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="3b4ff-231">参照してください[IHostingStartup を使用して外部のアセンブリからのアプリの機能の追加](xref:hosting/ihostingstartup)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-231">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="3b4ff-232">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="3b4ff-232">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="3b4ff-233">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="3b4ff-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="3b4ff-234">**既定の**: false</span><span class="sxs-lookup"><span data-stu-id="3b4ff-234">**Default**: false</span></span>  
<span data-ttu-id="3b4ff-235">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-235">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="3b4ff-236">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-236">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-237">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-237">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-238">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-238">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b4ff-239">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-239">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="3b4ff-240">サーバーの Url</span><span class="sxs-lookup"><span data-stu-id="3b4ff-240">Server URLs</span></span>

<span data-ttu-id="3b4ff-241">IP アドレスまたはサーバーが要求をリッスンするポートとプロトコルとなるホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-241">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="3b4ff-242">**キー**: url</span><span class="sxs-lookup"><span data-stu-id="3b4ff-242">**Key**: urls</span></span>  
<span data-ttu-id="3b4ff-243">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="3b4ff-243">**Type**: *string*</span></span>  
<span data-ttu-id="3b4ff-244">**既定の**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="3b4ff-244">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="3b4ff-245">**使用して設定**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-245">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="3b4ff-246">セミコロンで区切られたに設定 (;) の URL の一覧のプレフィックスに、サーバーが応答する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-246">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="3b4ff-247">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-247">For example, `http://localhost:123`.</span></span> <span data-ttu-id="3b4ff-248">使用して"\*"を示す、サーバーは、上の IP アドレスまたはホスト名の指定したポートとプロトコルを使用して要求をリッスンする必要があります (たとえば、 `http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-248">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="3b4ff-249">プロトコル (`http://`または`https://`) 各 url を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-249">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="3b4ff-250">サポートされている形式は、サーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-250">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="3b4ff-252">Kestrel には、独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-252">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="3b4ff-253">詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-253">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="3b4ff-255">シャット ダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="3b4ff-255">Shutdown Timeout</span></span>

<span data-ttu-id="3b4ff-256">Web ホストのシャット ダウンを待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-256">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="3b4ff-257">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="3b4ff-257">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="3b4ff-258">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="3b4ff-258">**Type**: *int*</span></span>  
<span data-ttu-id="3b4ff-259">**既定の**: 5</span><span class="sxs-lookup"><span data-stu-id="3b4ff-259">**Default**: 5</span></span>  
<span data-ttu-id="3b4ff-260">**使用して設定**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-260">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="3b4ff-261">キーを受け入れますが、 *int*で`UseSetting`(たとえば、 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) では、`UseShutdownTimeout`拡張メソッドは、`TimeSpan`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-261">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="3b4ff-262">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-262">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-263">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-263">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-264">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-264">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b4ff-265">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-265">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="3b4ff-266">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="3b4ff-266">Startup Assembly</span></span>

<span data-ttu-id="3b4ff-267">検索対象となるアセンブリの決定、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-267">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="3b4ff-268">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="3b4ff-268">**Key**: startupAssembly</span></span>  
<span data-ttu-id="3b4ff-269">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="3b4ff-269">**Type**: *string*</span></span>  
<span data-ttu-id="3b4ff-270">**既定の**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="3b4ff-270">**Default**: The app's assembly</span></span>  
<span data-ttu-id="3b4ff-271">**使用して設定**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-271">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="3b4ff-272">名前でアセンブリを参照することができます (`string`) または型 (`TStartup`)。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-272">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="3b4ff-273">複数`UseStartup`メソッドが呼び出されると、最後の 1 つが優先されます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-273">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-274">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-274">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-275">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-275">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="3b4ff-276">Web ルート</span><span class="sxs-lookup"><span data-stu-id="3b4ff-276">Web Root</span></span>

<span data-ttu-id="3b4ff-277">アプリの静的な資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-277">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="3b4ff-278">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="3b4ff-278">**Key**: webroot</span></span>  
<span data-ttu-id="3b4ff-279">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="3b4ff-279">**Type**: *string*</span></span>  
<span data-ttu-id="3b4ff-280">**既定**: が指定されていない、既定値は"(Content Root) wwwroot/"パスが存在する場合は、します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-280">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="3b4ff-281">パスが存在しない、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-281">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="3b4ff-282">**使用して設定**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="3b4ff-282">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-283">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-284">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-284">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="3b4ff-285">構成をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-285">Overriding configuration</span></span>

<span data-ttu-id="3b4ff-286">使用して[構成](xref:fundamentals/configuration/index)ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-286">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="3b4ff-287">次の例では、ホストの構成は必要に応じてで指定された、 *hosting.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-287">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="3b4ff-288">すべての構成から読み込まれました、 *hosting.json*ファイルはコマンドライン引数によってオーバーライドされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-288">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="3b4ff-289">ビルドの構成 (で`config`) でホストを構成するために使用`UseConfiguration`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-289">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3b4ff-291">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="3b4ff-292">によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-293">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-293">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b4ff-294">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-294">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="3b4ff-295">によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-295">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="3b4ff-296">`UseConfiguration`拡張メソッドは現在によって返される構成セクションを解析できる`GetSection`(たとえば、`.UseConfiguration(Configuration.GetSection("section"))`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-296">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3b4ff-297">`GetSection`メソッド要求のセクションに構成キーをフィルター処理が残りますセクションの名前のキーに (たとえば、 `section:urls`、 `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-297">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="3b4ff-298">`UseConfiguration`メソッドに一致するキーが必要ですが、`WebHostBuilder`キー (たとえば、 `urls`、 `environment`)。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-298">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="3b4ff-299">キーにセクション名の有無は、セクションの値が、ホストを構成することを防止します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-299">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="3b4ff-300">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-300">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3b4ff-301">詳細と回避策については、次を参照してください。[フル キーを使用して WebHostBuilder.UseConfiguration に構成セクションを渡して](https://github.com/aspnet/Hosting/issues/839)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-301">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="3b4ff-302">特定の URL で実行するホストを指定するには、でしたに合格する目的の値で、コマンド プロンプトから実行するときに`dotnet run`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-302">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="3b4ff-303">コマンドラインの引数よりも優先、`urls`値から、 *hosting.json*ファイル、および、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-303">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="3b4ff-304">重要度の順序</span><span class="sxs-lookup"><span data-stu-id="3b4ff-304">Ordering importance</span></span>

<span data-ttu-id="3b4ff-305">一部の`WebHostBuilder`場合の設定は環境変数から読み込ま最初に設定します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-305">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="3b4ff-306">これらの環境変数の形式を使用する`ASPNETCORE_{configurationKey}`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-306">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="3b4ff-307">既定では、サーバーがリッスンする Url を設定するに設定する`ASPNETCORE_URLS`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-307">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="3b4ff-308">構成を指定することによってこれらの環境変数の値のいずれかをオーバーライドすることができます (を使用して`UseConfiguration`) または値を明示的に設定して (を使用して`UseSetting`または明示的な拡張メソッドのいずれかのように`UseUrls`)。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-308">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="3b4ff-309">ホストは、どちらのオプションが最後の値を設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-309">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="3b4ff-310">プログラムで 1 つの値に設定されて、既定の URL が構成をオーバーライドすることを許可する場合は、URL を設定した後のコマンドライン構成を使用できます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-310">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="3b4ff-311">参照してください[オーバーライド構成](#overriding-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-311">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="3b4ff-312">ホストの開始</span><span class="sxs-lookup"><span data-stu-id="3b4ff-312">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b4ff-313">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-313">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3b4ff-314">**実行**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-314">**Run**</span></span>

<span data-ttu-id="3b4ff-315">`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-315">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="3b4ff-316">**Start**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-316">**Start**</span></span>

<span data-ttu-id="3b4ff-317">非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-317">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="3b4ff-318">Url の一覧を渡す場合、`Start`メソッド、指定された Url のリッスンがします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-318">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="3b4ff-319">初期化の事前構成済みの既定値を使用して、新しいホストを開始できます`CreateDefaultBuilder`便利な静的メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-319">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="3b4ff-320">これらのメソッドは、コンソール出力とサーバーを起動[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)改行 (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-320">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="3b4ff-321">**開始 (RequestDelegate アプリ)**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-321">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="3b4ff-322">始まる、 `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-322">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3b4ff-323">要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには</span><span class="sxs-lookup"><span data-stu-id="3b4ff-323">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3b4ff-324">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-324">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3b4ff-325">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-325">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3b4ff-326">**開始 (文字列 url、RequestDelegate アプリ)**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-326">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="3b4ff-327">URL を使用して起動し、 `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-327">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="3b4ff-328">同じ結果を生成する**開始 (RequestDelegate app)**、上の応答をアプリを除く`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-328">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="3b4ff-329">**開始 (アクション<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-329">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="3b4ff-330">インスタンスを使用して`IRouteBuilder`([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) ルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-330">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="3b4ff-331">次のブラウザーの要求の例を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-331">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="3b4ff-332">要求</span><span class="sxs-lookup"><span data-stu-id="3b4ff-332">Request</span></span>                                    | <span data-ttu-id="3b4ff-333">応答</span><span class="sxs-lookup"><span data-stu-id="3b4ff-333">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="3b4ff-334">こんにちは, Martin!</span><span class="sxs-lookup"><span data-stu-id="3b4ff-334">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="3b4ff-335">ブエノスアイレス dias、Catrina!</span><span class="sxs-lookup"><span data-stu-id="3b4ff-335">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="3b4ff-336">"Ooops!"は文字列で例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-336">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="3b4ff-337">文字列を使用して例外をスロー"Uh $embedded$!"</span><span class="sxs-lookup"><span data-stu-id="3b4ff-337">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="3b4ff-338">Sante、Kevin!</span><span class="sxs-lookup"><span data-stu-id="3b4ff-338">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="3b4ff-339">ハローワールド！</span><span class="sxs-lookup"><span data-stu-id="3b4ff-339">Hello World!</span></span>                             |

<span data-ttu-id="3b4ff-340">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-340">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3b4ff-341">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-341">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3b4ff-342">**開始 (文字列の url をアクション<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-342">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="3b4ff-343">インスタンスおよび URL を使用して`IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-343">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="3b4ff-344">同じ結果**開始 (アクション<IRouteBuilder>routeBuilder)**、アプリを除く応答`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-344">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="3b4ff-345">**始める (アクション<IApplicationBuilder>アプリ)**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-345">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="3b4ff-346">構成するデリゲートを指定、 `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-346">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="3b4ff-347">要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには</span><span class="sxs-lookup"><span data-stu-id="3b4ff-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="3b4ff-348">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="3b4ff-349">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="3b4ff-350">**始める (文字列の url をアクション<IApplicationBuilder>アプリ)**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-350">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="3b4ff-351">URL および構成するデリゲートを提供する`IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-351">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="3b4ff-352">同じ結果を生成する**で始める (アクション<IApplicationBuilder>アプリ)**、上の応答をアプリを除く`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-352">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b4ff-353">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b4ff-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3b4ff-354">**実行**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-354">**Run**</span></span>

<span data-ttu-id="3b4ff-355">`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-355">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="3b4ff-356">**Start**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-356">**Start**</span></span>

<span data-ttu-id="3b4ff-357">非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-357">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="3b4ff-358">Url の一覧を渡す場合、`Start`メソッド、指定された Url のリッスンがします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-358">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="3b4ff-359">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="3b4ff-359">IHostingEnvironment interface</span></span>

<span data-ttu-id="3b4ff-360">[IHostingEnvironment インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)アプリの web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-360">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="3b4ff-361">使用することができます[コンス トラクター インジェクション](xref:fundamentals/dependency-injection)を取得する、`IHostingEnvironment`プロパティと拡張メソッドを使用するのには。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-361">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="3b4ff-362">使用することができます、[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)環境に基づいて起動時に、アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-362">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="3b4ff-363">また、挿入することができます、`IHostingEnvironment`に、`Startup`で使用するためのコンス トラクター `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3b4ff-363">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="3b4ff-364">加え、`IsDevelopment`の拡張メソッドで`IHostingEnvironment`提供`IsStaging`、 `IsProduction`、および`IsEnvironment(string environmentName)`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-364">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="3b4ff-365">参照してください[複数の環境で作業](xref:fundamentals/environments)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-365">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="3b4ff-366">`IHostingEnvironment`サービスは直接にも挿入され、`Configure`処理パイプラインを設定するためのメソッド。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-366">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="3b4ff-367">挿入することできます`IHostingEnvironment`に、`Invoke`メソッド カスタムを作成するときに[ミドルウェア](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="3b4ff-367">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="3b4ff-368">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="3b4ff-368">IApplicationLifetime interface</span></span>

<span data-ttu-id="3b4ff-369">[IApplicationLifetime インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime)後スタートアップおよびシャット ダウンのアクティビティを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-369">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="3b4ff-370">インターフェイス上の 3 つのプロパティは、キャンセル トークンに登録することができます、`Action`スタートアップおよびシャット ダウン イベントを定義するメソッド。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-370">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="3b4ff-371">`StopApplication`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-371">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="3b4ff-372">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="3b4ff-372">Cancellation Token</span></span>    | <span data-ttu-id="3b4ff-373">&#8230; をトリガー</span><span class="sxs-lookup"><span data-stu-id="3b4ff-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="3b4ff-374">ホストが完全に開始されました。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-374">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="3b4ff-375">ホストが正常なシャット ダウンを実行しています。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-375">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="3b4ff-376">要求はまだ処理中です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-376">Requests may still be processing.</span></span> <span data-ttu-id="3b4ff-377">このイベントが完了するまで、ブロックをシャット ダウンします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-377">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="3b4ff-378">ホストが正常なシャット ダウンを完了しています。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="3b4ff-379">すべての要求を完全に処理される必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-379">All requests should be completely processed.</span></span> <span data-ttu-id="3b4ff-380">このイベントが完了するまで、ブロックをシャット ダウンします。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-380">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="3b4ff-381">メソッド</span><span class="sxs-lookup"><span data-stu-id="3b4ff-381">Method</span></span>            | <span data-ttu-id="3b4ff-382">操作</span><span class="sxs-lookup"><span data-stu-id="3b4ff-382">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="3b4ff-383">現在のアプリケーションの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-383">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="3b4ff-384">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="3b4ff-384">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="3b4ff-385">**ASP.NET Core 2.0 のみに適用されます。**</span><span class="sxs-lookup"><span data-stu-id="3b4ff-385">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="3b4ff-386">挿入することで、ホストをビルドする場合`IStartup`呼び出しではなく、依存性の注入コンテナーに直接`UseStartup`または`Configure`、次のエラーが発生する可能性があります:`Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-386">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="3b4ff-387">これは、ために発生、 [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey)をスキャンする (現在のアセンブリ) が必要な`HostingStartupAttributes`します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-387">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="3b4ff-388">手動で挿入した場合`IStartup`依存関係の注入コンテナー内に、次の呼び出しを追加、`WebHostBuilder`指定されたアセンブリ名で。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-388">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="3b4ff-389">代わりに、ダミーの追加`Configure`を`WebHostBuilder`、設定する、 `applicationName`(`ApplicationKey`) に自動的に。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-389">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="3b4ff-390">**注**: これは、ASP.NET Core 2.0 リリースで必要なだけ場合にのみ呼び出すことがない`UseStartup`または`Configure`です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-390">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="3b4ff-391">詳細については、次を参照してください。[お知らせ: Microsoft.Extensions.PlatformAbstractions がされました (コメント) を削除](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)と[StartupInjection サンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)です。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-391">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b4ff-392">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3b4ff-392">Additional resources</span></span>

* [<span data-ttu-id="3b4ff-393">IIS を使用して Windows への公開します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-393">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="3b4ff-394">Nginx を使用して Linux への公開します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-394">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="3b4ff-395">Apache を使用して Linux への公開します。</span><span class="sxs-lookup"><span data-stu-id="3b4ff-395">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="3b4ff-396">Windows サービスのホスト</span><span class="sxs-lookup"><span data-stu-id="3b4ff-396">Host in a Windows Service</span></span>](xref:hosting/windows-service)
