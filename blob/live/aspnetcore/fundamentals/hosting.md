---
title: "ASP.NET Core でのホスティング"
author: guardrex
description: "これは、アプリの起動と有効期間の管理を担当する ASP.NET Core の web ホストについて説明します。"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7f6712073002b73ca4ddd7586718c81e62cacbc2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="0f017-103">ASP.NET Core でのホスティング</span><span class="sxs-lookup"><span data-stu-id="0f017-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="0f017-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0f017-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0f017-105">ASP.NET Core アプリケーションの構成および起動、*ホスト*です。</span><span class="sxs-lookup"><span data-stu-id="0f017-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="0f017-106">ホストは、アプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="0f017-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0f017-107">少なくともは、ホストは、サーバーおよび要求処理パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="0f017-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="0f017-108">ホストの設定</span><span class="sxs-lookup"><span data-stu-id="0f017-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0f017-110">インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0f017-111">これは、アプリのエントリ ポイントで通常実行、`Main`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0f017-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0f017-112">プロジェクト テンプレートで`Main`内にある*Program.cs*です。</span><span class="sxs-lookup"><span data-stu-id="0f017-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="0f017-113">一般的な*Program.cs*呼び出し[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)ホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="0f017-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="0f017-114">`CreateDefaultBuilder`次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="0f017-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="0f017-115">構成[Kestrel](servers/kestrel.md) web サーバーとします。</span><span class="sxs-lookup"><span data-stu-id="0f017-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="0f017-116">Kestrel 既定のオプションを参照してください。 [Kestrel ASP.NET Core での web サーバーの実装のセクションで [オプション]、Kestrel](xref:fundamentals/servers/kestrel#kestrel-options)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="0f017-117">によって返されるパスへのコンテンツのルートを設定[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="0f017-118">読み込みオプションの構成:</span><span class="sxs-lookup"><span data-stu-id="0f017-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="0f017-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0f017-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="0f017-120">*appsettings です。{環境} .json*です。</span><span class="sxs-lookup"><span data-stu-id="0f017-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="0f017-121">[ユーザーの機密情報](xref:security/app-secrets)アプリを実行するときに、`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="0f017-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="0f017-122">環境変数。</span><span class="sxs-lookup"><span data-stu-id="0f017-122">Environment variables.</span></span>
  * <span data-ttu-id="0f017-123">コマンドライン引数。</span><span class="sxs-lookup"><span data-stu-id="0f017-123">Command-line arguments.</span></span>
* <span data-ttu-id="0f017-124">構成[ログ](xref:fundamentals/logging/index)コンソールとデバッグ出力用です。</span><span class="sxs-lookup"><span data-stu-id="0f017-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="0f017-125">ログ記録が含まれています[ログ フィルター](xref:fundamentals/logging/index#log-filtering)のログ記録の構成セクションで指定された規則、*される appsettings.json*または*appsettings {。環境} .json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0f017-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="0f017-126">により、IIS の背後にある実行中、 [IIS 統合](xref:host-and-deploy/iis/index)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="0f017-127">基本パスとを使用する場合、サーバーがリッスンするポートを構成、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0f017-128">モジュールでは、IIS と Kestrel リバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f017-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="0f017-129">アプリの構成も行います[スタートアップ エラーがキャプチャされる](#capture-startup-errors)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="0f017-130">IIS の既定のオプションを参照してください。 [、IIS が IIS と Windows 上のホスト ASP.NET Core のセクションをオプション](xref:host-and-deploy/iis/index#iis-options)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="0f017-131">*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0f017-132">プロジェクトのルート フォルダーから、アプリが開始されると、プロジェクトのルート フォルダーは、コンテンツのルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="0f017-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0f017-133">これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0f017-134">アプリの構成の詳細については、次を参照してください。 [ASP.NET Core の構成](xref:fundamentals/configuration/index)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="0f017-135">静的なを使用する代わりとして`CreateDefaultBuilder`からホストを作成するメソッド[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core でサポートされているアプローチは、2.x です。</span><span class="sxs-lookup"><span data-stu-id="0f017-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="0f017-136">詳細については、ASP.NET Core 1.x タブを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f017-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0f017-138">インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0f017-139">アプリのエントリ ポイントで通常実行されるホストを作成する、`Main`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0f017-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0f017-140">プロジェクト テンプレートで`Main`内にある*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0f017-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="0f017-141">`WebHostBuilder`必要があります、 [IServer を実装しているサーバー](servers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="0f017-142">組み込みのサーバーが[Kestrel](servers/kestrel.md)と[HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 のリリースでは、前に、HTTP.sys が呼び出された[WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="0f017-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="0f017-143">この例では、 [UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="0f017-144">*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0f017-145">既定のコンテンツのルートが取得`UseContentRoot`によって[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="0f017-146">プロジェクトのルート フォルダーから、アプリが開始されると、プロジェクトのルート フォルダーは、コンテンツのルートとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="0f017-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0f017-147">これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0f017-148">リバース プロキシとして、IIS を使用するには、呼び出す[UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)ホストの作成の一部として。</span><span class="sxs-lookup"><span data-stu-id="0f017-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="0f017-149">`UseIISIntegration`構成されない、*サーバー*と同様、 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)はします。</span><span class="sxs-lookup"><span data-stu-id="0f017-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="0f017-150">`UseIISIntegration`基本パスとを使用する場合、サーバーがリッスンするポートを構成、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)Kestrel と IIS の間のリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f017-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="0f017-151">IIS で ASP.NET Core を使用する`UseKestrel`と`UseIISIntegration`指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f017-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="0f017-152">`UseIISIntegration`IIS または IIS Express の内側で実行されるときにのみアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="0f017-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="0f017-153">詳細については、次を参照してください。 [Introduction to ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)と[ASP.NET Core モジュール構成の参照](xref:host-and-deploy/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="0f017-154">ホスト (および ASP.NET Core アプリケーション) を構成する最小限の実装には、サーバーおよびアプリケーションの要求パイプラインの構成の指定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0f017-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="0f017-155">設定をホストする場合に[構成](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)と[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1)メソッドを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="0f017-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="0f017-156">場合、`Startup`クラスを指定すると、それを定義する必要があります、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0f017-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="0f017-157">詳細については、次を参照してください。[で ASP.NET Core アプリケーションの起動](startup.md)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="0f017-158">複数回呼び出す`ConfigureServices`互いに追加します。</span><span class="sxs-lookup"><span data-stu-id="0f017-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="0f017-159">複数回呼び出す`Configure`または`UseStartup`上、`WebHostBuilder`以前の設定を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0f017-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="0f017-160">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="0f017-160">Host configuration values</span></span>

<span data-ttu-id="0f017-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)ホストの構成値を設定する次の方法に依存しています。</span><span class="sxs-lookup"><span data-stu-id="0f017-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="0f017-162">形式の環境変数を含むホスト ビルダー構成`ASPNETCORE_{configurationKey}`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="0f017-163">たとえば、`ASPNETCORE_URLS` のようにします。</span><span class="sxs-lookup"><span data-stu-id="0f017-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="0f017-164">明示的なメソッドは、よう`CaptureStartupErrors`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="0f017-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting)と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="0f017-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="0f017-166">使用して値を設定するときに`UseSetting`種類に関係なく文字列として値を設定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="0f017-167">ホストは、どちらのオプションが最後の値を設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="0f017-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="0f017-168">詳細については、次を参照してください。[オーバーライド構成](#overriding-configuration)次のセクションでします。</span><span class="sxs-lookup"><span data-stu-id="0f017-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="0f017-169">スタートアップ エラーがキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="0f017-169">Capture Startup Errors</span></span>

<span data-ttu-id="0f017-170">この設定は、スタートアップのエラーのキャプチャを制御します。</span><span class="sxs-lookup"><span data-stu-id="0f017-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="0f017-171">**Key**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="0f017-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="0f017-172">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="0f017-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0f017-173">**既定**: 既定値は`false`ここで、既定値は、IIS の背後にある Kestrel でアプリを実行する場合を除き、`true`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="0f017-174">**使用して設定**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="0f017-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="0f017-175">**環境変数**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="0f017-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="0f017-176">ときに`false`起動の結果、終了ホスト中のエラーです。</span><span class="sxs-lookup"><span data-stu-id="0f017-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0f017-177">ときに`true`ホストが起動中に例外をキャプチャし、サーバーを起動しようとしています。</span><span class="sxs-lookup"><span data-stu-id="0f017-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="0f017-180">コンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="0f017-180">Content Root</span></span>

<span data-ttu-id="0f017-181">この設定は、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始位置を決定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="0f017-182">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="0f017-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="0f017-183">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="0f017-183">**Type**: *string*</span></span>  
<span data-ttu-id="0f017-184">**既定の**: アプリのアセンブリが存在するフォルダーの既定値です。</span><span class="sxs-lookup"><span data-stu-id="0f017-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="0f017-185">**使用して設定**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="0f017-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="0f017-186">**環境変数**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="0f017-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="0f017-187">コンテンツのルートはのベース パスとしても使用される、 [Web ルート設定](#web-root)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="0f017-188">パスが存在しない場合、ホストは、開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="0f017-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="0f017-191">詳細なエラー</span><span class="sxs-lookup"><span data-stu-id="0f017-191">Detailed Errors</span></span>

<span data-ttu-id="0f017-192">エラーをキャプチャする必要がある場合を判断します。</span><span class="sxs-lookup"><span data-stu-id="0f017-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="0f017-193">**キー**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="0f017-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="0f017-194">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="0f017-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0f017-195">**既定の**: false</span><span class="sxs-lookup"><span data-stu-id="0f017-195">**Default**: false</span></span>  
<span data-ttu-id="0f017-196">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0f017-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0f017-197">**環境変数**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="0f017-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="0f017-198">有効にすると (場合や、<a href="#environment">環境</a>に設定されている`Development`)、アプリは、詳細な例外をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="0f017-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="0f017-201">環境</span><span class="sxs-lookup"><span data-stu-id="0f017-201">Environment</span></span>

<span data-ttu-id="0f017-202">アプリの環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-202">Sets the app's environment.</span></span>

<span data-ttu-id="0f017-203">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="0f017-203">**Key**: environment</span></span>  
<span data-ttu-id="0f017-204">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="0f017-204">**Type**: *string*</span></span>  
<span data-ttu-id="0f017-205">**既定の**: 実稼働環境</span><span class="sxs-lookup"><span data-stu-id="0f017-205">**Default**: Production</span></span>  
<span data-ttu-id="0f017-206">**使用して設定**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="0f017-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="0f017-207">**環境変数**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="0f017-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="0f017-208">環境は、任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="0f017-208">The environment can be set to any value.</span></span> <span data-ttu-id="0f017-209">フレームワークによって定義された値が含まれます`Development`、 `Staging`、および`Production`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0f017-210">値は、大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="0f017-210">Values aren't case sensitive.</span></span> <span data-ttu-id="0f017-211">既定では、*環境*から読み取られた、`ASPNETCORE_ENVIRONMENT`環境変数。</span><span class="sxs-lookup"><span data-stu-id="0f017-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="0f017-212">使用する場合[Visual Studio](https://www.visualstudio.com/)環境変数を設定することがあります、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0f017-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="0f017-213">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f017-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="0f017-216">ホストしているスタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="0f017-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="0f017-217">アプリのホスティング スタートアップ アセンブリを設定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="0f017-218">**キー**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="0f017-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="0f017-219">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="0f017-219">**Type**: *string*</span></span>  
<span data-ttu-id="0f017-220">**既定の**: 空の文字列</span><span class="sxs-lookup"><span data-stu-id="0f017-220">**Default**: Empty string</span></span>  
<span data-ttu-id="0f017-221">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0f017-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0f017-222">**環境変数**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0f017-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="0f017-223">起動時にロードするスタートアップ アセンブリをホストしているセミコロンで区切られた文字列。</span><span class="sxs-lookup"><span data-stu-id="0f017-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="0f017-224">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0f017-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="0f017-225">既定の構成値は、空の文字列、ホスティング スタートアップ アセンブリは常に、アプリのアセンブリを含めます。</span><span class="sxs-lookup"><span data-stu-id="0f017-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="0f017-226">ホスティングのスタートアップ アセンブリが提供されている場合は、アプリが起動中に、一般的なサービスを構築するときに読み込むためのアプリのアセンブリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="0f017-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0f017-229">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="0f017-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="0f017-230">Url のホストを優先します。</span><span class="sxs-lookup"><span data-stu-id="0f017-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="0f017-231">構成されている Url に、ホストがリッスンするかどうかを示す、`WebHostBuilder`で構成されているものではなく、`IServer`実装します。</span><span class="sxs-lookup"><span data-stu-id="0f017-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="0f017-232">**キー**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="0f017-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="0f017-233">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="0f017-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0f017-234">**既定の**: true</span><span class="sxs-lookup"><span data-stu-id="0f017-234">**Default**: true</span></span>  
<span data-ttu-id="0f017-235">**使用して設定**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="0f017-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="0f017-236">**環境変数**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="0f017-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="0f017-237">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0f017-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0f017-240">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="0f017-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="0f017-241">スタートアップをホストしているようにします。</span><span class="sxs-lookup"><span data-stu-id="0f017-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="0f017-242">ホストしているアプリのアセンブリで構成されているスタートアップ アセンブリのホストを含め、スタートアップ アセンブリの自動読み込みができなくなります。</span><span class="sxs-lookup"><span data-stu-id="0f017-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="0f017-243">参照してください[IHostingStartup を使用して外部のアセンブリからのアプリの機能の追加](xref:host-and-deploy/ihostingstartup)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="0f017-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="0f017-244">**キー**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0f017-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="0f017-245">**型**: *bool* (`true`または`1`)</span><span class="sxs-lookup"><span data-stu-id="0f017-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0f017-246">**既定の**: false</span><span class="sxs-lookup"><span data-stu-id="0f017-246">**Default**: false</span></span>  
<span data-ttu-id="0f017-247">**使用して設定**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0f017-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0f017-248">**環境変数**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="0f017-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="0f017-249">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0f017-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0f017-252">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="0f017-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="0f017-253">サーバーの Url</span><span class="sxs-lookup"><span data-stu-id="0f017-253">Server URLs</span></span>

<span data-ttu-id="0f017-254">IP アドレスまたはサーバーが要求をリッスンするポートとプロトコルとなるホスト アドレスを示します。</span><span class="sxs-lookup"><span data-stu-id="0f017-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="0f017-255">**キー**: url</span><span class="sxs-lookup"><span data-stu-id="0f017-255">**Key**: urls</span></span>  
<span data-ttu-id="0f017-256">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="0f017-256">**Type**: *string*</span></span>  
<span data-ttu-id="0f017-257">**既定の**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="0f017-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="0f017-258">**使用して設定**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="0f017-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="0f017-259">**環境変数**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="0f017-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="0f017-260">セミコロンで区切られたに設定 (;) の URL の一覧のプレフィックスに、サーバーが応答する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f017-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="0f017-261">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="0f017-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0f017-262">使用して"\*"を示す、サーバーは、上の IP アドレスまたはホスト名の指定したポートとプロトコルを使用して要求をリッスンする必要があります (たとえば、 `http://*:5000`)。</span><span class="sxs-lookup"><span data-stu-id="0f017-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="0f017-263">プロトコル (`http://`または`https://`) 各 url を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f017-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0f017-264">サポートされている形式は、サーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="0f017-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="0f017-266">Kestrel には、独自のエンドポイント構成 API があります。</span><span class="sxs-lookup"><span data-stu-id="0f017-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="0f017-267">詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f017-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="0f017-269">シャット ダウン タイムアウト</span><span class="sxs-lookup"><span data-stu-id="0f017-269">Shutdown Timeout</span></span>

<span data-ttu-id="0f017-270">Web ホストのシャット ダウンを待機する時間を指定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="0f017-271">**キー**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="0f017-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="0f017-272">**型**: *int*</span><span class="sxs-lookup"><span data-stu-id="0f017-272">**Type**: *int*</span></span>  
<span data-ttu-id="0f017-273">**既定の**: 5</span><span class="sxs-lookup"><span data-stu-id="0f017-273">**Default**: 5</span></span>  
<span data-ttu-id="0f017-274">**使用して設定**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="0f017-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="0f017-275">**環境変数**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="0f017-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="0f017-276">キーを受け入れますが、 *int*で`UseSetting`(たとえば、 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) では、`UseShutdownTimeout`拡張メソッドは、`TimeSpan`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="0f017-277">この機能は ASP.NET Core 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="0f017-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0f017-280">この機能では使用できません ASP.NET Core 1.x です。</span><span class="sxs-lookup"><span data-stu-id="0f017-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="0f017-281">スタートアップ アセンブリ</span><span class="sxs-lookup"><span data-stu-id="0f017-281">Startup Assembly</span></span>

<span data-ttu-id="0f017-282">検索対象となるアセンブリの決定、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="0f017-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="0f017-283">**キー**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="0f017-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="0f017-284">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="0f017-284">**Type**: *string*</span></span>  
<span data-ttu-id="0f017-285">**既定の**: アプリのアセンブリ</span><span class="sxs-lookup"><span data-stu-id="0f017-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="0f017-286">**使用して設定**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="0f017-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="0f017-287">**環境変数**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="0f017-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="0f017-288">名前でアセンブリ (`string`) または型 (`TStartup`) を参照できます。</span><span class="sxs-lookup"><span data-stu-id="0f017-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="0f017-289">複数`UseStartup`メソッドが呼び出されると、最後の 1 つが優先されます。</span><span class="sxs-lookup"><span data-stu-id="0f017-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="0f017-292">Web ルート</span><span class="sxs-lookup"><span data-stu-id="0f017-292">Web Root</span></span>

<span data-ttu-id="0f017-293">アプリの静的な資産への相対パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="0f017-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="0f017-294">**キー**: webroot</span><span class="sxs-lookup"><span data-stu-id="0f017-294">**Key**: webroot</span></span>  
<span data-ttu-id="0f017-295">**型**:*文字列*</span><span class="sxs-lookup"><span data-stu-id="0f017-295">**Type**: *string*</span></span>  
<span data-ttu-id="0f017-296">**既定**: が指定されていない、既定値は"(Content Root) wwwroot/"パスが存在する場合は、します。</span><span class="sxs-lookup"><span data-stu-id="0f017-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="0f017-297">パスが存在しない、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0f017-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="0f017-298">**使用して設定**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="0f017-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="0f017-299">**環境変数**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="0f017-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="0f017-302">構成をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="0f017-302">Overriding configuration</span></span>

<span data-ttu-id="0f017-303">使用して[構成](xref:fundamentals/configuration/index)ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="0f017-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="0f017-304">次の例では、ホストの構成は必要に応じてで指定された、 *hosting.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0f017-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="0f017-305">すべての構成から読み込まれました、 *hosting.json*ファイルはコマンドライン引数によってオーバーライドされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0f017-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="0f017-306">ビルドの構成 (で`config`) でホストを構成するために使用`UseConfiguration`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0f017-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="0f017-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="0f017-309">によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。</span><span class="sxs-lookup"><span data-stu-id="0f017-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0f017-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="0f017-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="0f017-312">によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。</span><span class="sxs-lookup"><span data-stu-id="0f017-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="0f017-313">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)拡張メソッドは現在によって返される構成セクションを解析できる`GetSection`(たとえば、`.UseConfiguration(Configuration.GetSection("section"))`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="0f017-314">`GetSection`メソッド要求のセクションに構成キーをフィルター処理が残りますセクションの名前のキーに (たとえば、 `section:urls`、 `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="0f017-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="0f017-315">`UseConfiguration`メソッドに一致するキーが必要ですが、`WebHostBuilder`キー (たとえば、 `urls`、 `environment`)。</span><span class="sxs-lookup"><span data-stu-id="0f017-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="0f017-316">キーにセクション名の有無は、セクションの値が、ホストを構成することを防止します。</span><span class="sxs-lookup"><span data-stu-id="0f017-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="0f017-317">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="0f017-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="0f017-318">詳細と回避策については、次を参照してください。[フル キーを使用して WebHostBuilder.UseConfiguration に構成セクションを渡して](https://github.com/aspnet/Hosting/issues/839)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="0f017-319">特定の URL で実行するホストを指定する目的の値に渡すできるコマンド プロンプトから実行するときに`dotnet run`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="0f017-320">コマンドラインの引数よりも優先、`urls`値から、 *hosting.json*ファイル、および、サーバーはポート 8080 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="0f017-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="0f017-321">ホストの開始</span><span class="sxs-lookup"><span data-stu-id="0f017-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0f017-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0f017-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0f017-323">**実行**</span><span class="sxs-lookup"><span data-stu-id="0f017-323">**Run**</span></span>

<span data-ttu-id="0f017-324">`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="0f017-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0f017-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="0f017-325">**Start**</span></span>

<span data-ttu-id="0f017-326">非ブロッキングの方法で呼び出すことによって、ホストを実行、`Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0f017-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0f017-327">Url の一覧が渡された場合、`Start`メソッド、指定された Url のリッスンがします。</span><span class="sxs-lookup"><span data-stu-id="0f017-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="0f017-328">アプリが初期化およびの事前構成済みの既定値を使用して、新しいホストを開始`CreateDefaultBuilder`便利な静的メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="0f017-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="0f017-329">これらのメソッドは、コンソール出力とサーバーを起動[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)改行 (Ctrl-C/SIGINT または SIGTERM) まで待機します。</span><span class="sxs-lookup"><span data-stu-id="0f017-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="0f017-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="0f017-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="0f017-331">始まる、 `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="0f017-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0f017-332">要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには</span><span class="sxs-lookup"><span data-stu-id="0f017-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0f017-333">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="0f017-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0f017-334">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="0f017-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0f017-335">**開始 (文字列 url、RequestDelegate アプリ)**</span><span class="sxs-lookup"><span data-stu-id="0f017-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="0f017-336">URL を使用して起動し、 `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="0f017-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0f017-337">同じ結果を生成する**開始 (RequestDelegate app)**、上の応答をアプリを除く`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="0f017-338">**開始 (アクション<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0f017-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="0f017-339">インスタンスを使用して`IRouteBuilder`([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) ルーティング ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="0f017-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="0f017-340">次のブラウザーの要求の例を使用します。</span><span class="sxs-lookup"><span data-stu-id="0f017-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="0f017-341">要求</span><span class="sxs-lookup"><span data-stu-id="0f017-341">Request</span></span>                                    | <span data-ttu-id="0f017-342">応答</span><span class="sxs-lookup"><span data-stu-id="0f017-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="0f017-343">こんにちは, Martin!</span><span class="sxs-lookup"><span data-stu-id="0f017-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="0f017-344">ブエノスアイレス dias、Catrina!</span><span class="sxs-lookup"><span data-stu-id="0f017-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="0f017-345">"Ooops!"は文字列で例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="0f017-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="0f017-346">文字列を使用して例外をスロー"Uh $embedded$!"</span><span class="sxs-lookup"><span data-stu-id="0f017-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="0f017-347">Sante、Kevin!</span><span class="sxs-lookup"><span data-stu-id="0f017-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="0f017-348">ハローワールド！</span><span class="sxs-lookup"><span data-stu-id="0f017-348">Hello World!</span></span>                             |

<span data-ttu-id="0f017-349">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="0f017-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0f017-350">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="0f017-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0f017-351">**開始 (文字列の url をアクション<IRouteBuilder>routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0f017-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="0f017-352">インスタンスおよび URL を使用して`IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0f017-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="0f017-353">同じ結果**開始 (アクション<IRouteBuilder>routeBuilder)**、アプリを除く応答`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="0f017-354">**始める (アクション<IApplicationBuilder>アプリ)**</span><span class="sxs-lookup"><span data-stu-id="0f017-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="0f017-355">構成するデリゲートを指定、 `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0f017-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="0f017-356">要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには</span><span class="sxs-lookup"><span data-stu-id="0f017-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0f017-357">`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。</span><span class="sxs-lookup"><span data-stu-id="0f017-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0f017-358">アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。</span><span class="sxs-lookup"><span data-stu-id="0f017-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0f017-359">**始める (文字列の url をアクション<IApplicationBuilder>アプリ)**</span><span class="sxs-lookup"><span data-stu-id="0f017-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="0f017-360">URL および構成するデリゲートを提供する`IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0f017-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="0f017-361">同じ結果を生成する**で始める (アクション<IApplicationBuilder>アプリ)**、上の応答をアプリを除く`http://localhost:8080`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0f017-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0f017-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0f017-363">**実行**</span><span class="sxs-lookup"><span data-stu-id="0f017-363">**Run**</span></span>

<span data-ttu-id="0f017-364">`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="0f017-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0f017-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="0f017-365">**Start**</span></span>

<span data-ttu-id="0f017-366">非ブロッキングの方法で呼び出すことによって、ホストを実行、`Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0f017-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0f017-367">Url の一覧が渡された場合、`Start`メソッド、指定された Url のリッスンがします。</span><span class="sxs-lookup"><span data-stu-id="0f017-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="0f017-368">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="0f017-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="0f017-369">[IHostingEnvironment インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)アプリの web ホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="0f017-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="0f017-370">使用して[コンス トラクター インジェクション](xref:fundamentals/dependency-injection)を取得する、`IHostingEnvironment`プロパティと拡張メソッドを使用するのには。</span><span class="sxs-lookup"><span data-stu-id="0f017-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="0f017-371">A[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)環境に基づいて起動時にアプリを構成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="0f017-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="0f017-372">また、挿入、`IHostingEnvironment`に、`Startup`で使用するためのコンス トラクター `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0f017-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="0f017-373">加え、`IsDevelopment`の拡張メソッドで`IHostingEnvironment`提供`IsStaging`、 `IsProduction`、および`IsEnvironment(string environmentName)`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0f017-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="0f017-374">参照してください[複数の環境で作業](xref:fundamentals/environments)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="0f017-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="0f017-375">`IHostingEnvironment`サービスは直接にも挿入され、`Configure`処理パイプラインを設定するためのメソッド。</span><span class="sxs-lookup"><span data-stu-id="0f017-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="0f017-376">`IHostingEnvironment`挿入されることができます、`Invoke`メソッド カスタムを作成するときに[ミドルウェア](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="0f017-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="0f017-377">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="0f017-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="0f017-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime)では、アクティビティの後の起動とシャット ダウンします。</span><span class="sxs-lookup"><span data-stu-id="0f017-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="0f017-379">インターフェイスで次の 3 つのプロパティは、登録に使用されるキャンセル トークン`Action`スタートアップおよびシャット ダウン イベントを定義するメソッド。</span><span class="sxs-lookup"><span data-stu-id="0f017-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="0f017-380">`StopApplication`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0f017-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="0f017-381">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="0f017-381">Cancellation Token</span></span>    | <span data-ttu-id="0f017-382">&#8230; をトリガー</span><span class="sxs-lookup"><span data-stu-id="0f017-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="0f017-383">ホストが完全に開始されました。</span><span class="sxs-lookup"><span data-stu-id="0f017-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="0f017-384">ホストが正常なシャット ダウンを実行しています。</span><span class="sxs-lookup"><span data-stu-id="0f017-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="0f017-385">要求はまだ処理中です。</span><span class="sxs-lookup"><span data-stu-id="0f017-385">Requests may still be processing.</span></span> <span data-ttu-id="0f017-386">このイベントが完了するまで、ブロックをシャット ダウンします。</span><span class="sxs-lookup"><span data-stu-id="0f017-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="0f017-387">ホストが正常なシャット ダウンを完了しています。</span><span class="sxs-lookup"><span data-stu-id="0f017-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="0f017-388">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f017-388">All requests should be processed.</span></span> <span data-ttu-id="0f017-389">このイベントが完了するまで、ブロックをシャット ダウンします。</span><span class="sxs-lookup"><span data-stu-id="0f017-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="0f017-390">メソッド</span><span class="sxs-lookup"><span data-stu-id="0f017-390">Method</span></span>            | <span data-ttu-id="0f017-391">アクション</span><span class="sxs-lookup"><span data-stu-id="0f017-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="0f017-392">現在のアプリケーションの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="0f017-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="0f017-393">System.ArgumentException のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="0f017-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="0f017-394">**ASP.NET Core 2.0 のみに適用されます。**</span><span class="sxs-lookup"><span data-stu-id="0f017-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="0f017-395">挿入することで、ホストをビルドすることがあります`IStartup`呼び出しではなく、依存性の注入コンテナーに直接`UseStartup`または`Configure`:</span><span class="sxs-lookup"><span data-stu-id="0f017-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="0f017-396">ホストは、この方法で構築は、次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="0f017-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="0f017-397">これは、ために発生、 [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey)をスキャンする (現在のアセンブリ) が必要な`HostingStartupAttributes`します。</span><span class="sxs-lookup"><span data-stu-id="0f017-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="0f017-398">場合は、アプリを手動で挿入`IStartup`依存関係の注入コンテナー内に、次の呼び出しを追加`WebHostBuilder`で指定されたアセンブリ名。</span><span class="sxs-lookup"><span data-stu-id="0f017-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="0f017-399">代わりに、ダミーの追加`Configure`を`WebHostBuilder`、設定する、 `applicationName`(`ApplicationKey`) に自動的に。</span><span class="sxs-lookup"><span data-stu-id="0f017-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="0f017-400">**注**: これは、ASP.NET Core 2.0 リリースで必要なだけ場合にのみ、アプリを呼び出しません`UseStartup`または`Configure`です。</span><span class="sxs-lookup"><span data-stu-id="0f017-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="0f017-401">詳細については、次を参照してください。[お知らせ: Microsoft.Extensions.PlatformAbstractions がされました (コメント) を削除](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)と[StartupInjection サンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)です。</span><span class="sxs-lookup"><span data-stu-id="0f017-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0f017-402">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0f017-402">Additional resources</span></span>

* [<span data-ttu-id="0f017-403">IIS による Windows 上のホスト</span><span class="sxs-lookup"><span data-stu-id="0f017-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="0f017-404">Nginx による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="0f017-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="0f017-405">Apache による Linux でのホスト</span><span class="sxs-lookup"><span data-stu-id="0f017-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="0f017-406">Windows サービスのホスト</span><span class="sxs-lookup"><span data-stu-id="0f017-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
