---
title: ASP.NET Core モジュール構成リファレンス
author: guardrex
description: ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ca86b1548c7c28a64fd391617b2e8290c1c264cf
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191361"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="74155-103">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="74155-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="74155-104">著者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="74155-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="74155-105">このドキュメントでは、ASP.NET Core アプリをホストするための ASP.NET Core モジュールの構成方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="74155-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="74155-106">ASP.NET Core モジュールの概要とインストールの説明については、「[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="74155-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="74155-107">ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="74155-107">Hosting model</span></span>

<span data-ttu-id="74155-108">.NET Core 2.2 以降で実行されるアプリの場合、モジュールでは、インプロセス ホスティング モデルがサポートされています。このモデルを使用するとリバース プロキシ (アウト ホスティング) と比較してパフォーマンスが改善されます。</span><span class="sxs-lookup"><span data-stu-id="74155-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="74155-109">詳細については、「<xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="74155-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="74155-110">インプロセス ホスティングは既存のアプリではオプトインになっていますが、[dotnet new](/dotnet/core/tools/dotnet-new) テンプレートは既定ではすべての IIS および IIS Express シナリオにおいてインプロセス ホスティング モデルに設定されています。</span><span class="sxs-lookup"><span data-stu-id="74155-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="74155-111">アプリに対してインプロセス ホスティングを構成するには、そのアプリのプロジェクト ファイルに `<AspNetCoreHostingModel>` プロパティ (*MyApp.csproj* など) を追加して、値を `inprocess` に設定します。(アウト プロセス ホスティングは `outofprocess` を使用して設定します)。</span><span class="sxs-lookup"><span data-stu-id="74155-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="74155-112">インプロセスでホストする場合は、次の特性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="74155-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="74155-113">[Kestrel サーバー](xref:fundamentals/servers/kestrel)は使用されません。</span><span class="sxs-lookup"><span data-stu-id="74155-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="74155-114">カスタム <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 実装である `IISHttpServer` は、アプリのサーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="74155-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="74155-115">[requestTimeout 属性](#attributes-of-the-aspnetcore-element) は、インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="74155-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="74155-116">アプリ プールをアプリ間で共有することはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="74155-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="74155-117">アプリごとに 1 つのアプリ プールを使用します。</span><span class="sxs-lookup"><span data-stu-id="74155-117">Use one app pool per app.</span></span>

* <span data-ttu-id="74155-118">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用したとき、または [app_offline.htm ファイルを手動で配置内に置いた](xref:host-and-deploy/iis/index#locked-deployment-files)ときには、開いている接続があると、アプリがすぐにシャットダウンされない場合があります。</span><span class="sxs-lookup"><span data-stu-id="74155-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="74155-119">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="74155-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="74155-120">アプリとインストールされているランタイム (x64 または x86) のアーキテクチャ (ビット) は、アプリ プールのアーキテクチャと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="74155-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="74155-121">([CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) ではなく) `WebHostBuilder` を使用してアプリのホストを手動で設定するときに、アプリがこれまで Kestrel サーバー上で直接実行されていた場合 (セルフホステッド)、`UseKestrel` を呼び出してから `UseIISIntegration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="74155-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="74155-122">順序を逆にすると、ホストは開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="74155-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="74155-123">クライアントの切断が検出されます。</span><span class="sxs-lookup"><span data-stu-id="74155-123">Client disconnects are detected.</span></span> <span data-ttu-id="74155-124">クライアントが切断されると、[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) キャンセル トークンが取り消されます。</span><span class="sxs-lookup"><span data-stu-id="74155-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="74155-125">`Directory.GetCurrentDirectory()` はアプリケーション ディレクトリではなく IIS によって開始するプロセスのワーカー ディレクトリを返します (*w3wp.exe* に対して *C:\Windows\System32\inetsrv*など)。</span><span class="sxs-lookup"><span data-stu-id="74155-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="74155-126">ホスティング モデルの変更</span><span class="sxs-lookup"><span data-stu-id="74155-126">Hosting model changes</span></span>

<span data-ttu-id="74155-127">`hostingModel` 設定が *web.config* ファイル内で変更された場合 (「[web.config での構成](#configuration-with-webconfig)」セクションを参照)、モジュールによって IIS 用のワーカー プロセスがリサイクルされます。</span><span class="sxs-lookup"><span data-stu-id="74155-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="74155-128">IIS Express の場合、モジュールによってワーカー プロセスのリサイクルは行われませんが、代わりに、現在の IIS Express プロセスの正常なシャットダウンがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="74155-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="74155-129">アプリに対して次の要求が出されると、IIS Express の新しいプロセスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="74155-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="74155-130">プロセス名</span><span class="sxs-lookup"><span data-stu-id="74155-130">Process name</span></span>

<span data-ttu-id="74155-131">`Process.GetCurrentProcess().ProcessName` から、`w3wp`/`iisexpress` (インプロセス) または `dotnet` (アウト プロセス) がレポートされます。</span><span class="sxs-lookup"><span data-stu-id="74155-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="74155-132">web.config での構成</span><span class="sxs-lookup"><span data-stu-id="74155-132">Configuration with web.config</span></span>

<span data-ttu-id="74155-133">ASP.NET Core モジュールは、サイトの *web.config* ファイルの `system.webServer` ノードの `aspNetCore` セクションを使って構成します。</span><span class="sxs-lookup"><span data-stu-id="74155-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="74155-134">次に示す *web.config* ファイルは、[フレームワークに依存する展開](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)用に発行されたもので、サイトの要求を処理するように ASP.NET Core モジュールを構成します。</span><span class="sxs-lookup"><span data-stu-id="74155-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="74155-135">次の *web.config* は、[自己完結型の展開](/dotnet/articles/core/deploying/#self-contained-deployments-scd)用に発行されたものです。</span><span class="sxs-lookup"><span data-stu-id="74155-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="74155-136">アプリが [Azure App Service](https://azure.microsoft.com/services/app-service/) に対して展開されると、`stdoutLogFile` パスは `\\?\%home%\LogFiles\stdout` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="74155-136">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="74155-137">パスは stdout ログを *LogFiles* フォルダーに保存します。これは、サービスによって自動的に作成される場所です。</span><span class="sxs-lookup"><span data-stu-id="74155-137">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="74155-138">サブアプリでの *web.config* ファイルの構成に関する重要な注意事項については、「[サブアプリケーション構成](xref:host-and-deploy/iis/index#sub-application-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="74155-138">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="74155-139">aspNetCore 要素の属性</span><span class="sxs-lookup"><span data-stu-id="74155-139">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="74155-140">属性</span><span class="sxs-lookup"><span data-stu-id="74155-140">Attribute</span></span> | <span data-ttu-id="74155-141">説明</span><span class="sxs-lookup"><span data-stu-id="74155-141">Description</span></span> | <span data-ttu-id="74155-142">既定値</span><span class="sxs-lookup"><span data-stu-id="74155-142">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="74155-143">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="74155-143">Optional string attribute.</span></span></p><p><span data-ttu-id="74155-144">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="74155-144">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="74155-145">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-145">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-146">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="74155-146">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="74155-147">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-148">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="74155-148">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="74155-149">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="74155-149">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="74155-150">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="74155-150">Optional string attribute.</span></span></p><p><span data-ttu-id="74155-151">ホスティング モデルをインプロセス (`inprocess`) またはアウト プロセス (`outofprocess`) として指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-151">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="74155-152">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-152">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-153">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="74155-153">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="74155-154">&dagger;インプロセス ホスティングの場合、値は `1` に制限されます。</span><span class="sxs-lookup"><span data-stu-id="74155-154">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="74155-155">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="74155-155">Default: `1`</span></span><br><span data-ttu-id="74155-156">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="74155-156">Min: `1`</span></span><br><span data-ttu-id="74155-157">最大値: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="74155-157">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="74155-158">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-158">Required string attribute.</span></span></p><p><span data-ttu-id="74155-159">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="74155-159">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="74155-160">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="74155-160">Relative paths are supported.</span></span> <span data-ttu-id="74155-161">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="74155-161">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="74155-162">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-162">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-163">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-163">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="74155-164">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="74155-164">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="74155-165">インプロセス ホスティングではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="74155-165">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="74155-166">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="74155-166">Default: `10`</span></span><br><span data-ttu-id="74155-167">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-167">Min: `0`</span></span><br><span data-ttu-id="74155-168">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="74155-168">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="74155-169">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="74155-169">Optional timespan attribute.</span></span></p><p><span data-ttu-id="74155-170">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-170">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="74155-171">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-171">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="74155-172">インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="74155-172">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="74155-173">インプロセス ホスティングの場合、アプリによって要求が処理されるまでモジュールは待機します。</span><span class="sxs-lookup"><span data-stu-id="74155-173">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="74155-174">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="74155-174">Default: `00:02:00`</span></span><br><span data-ttu-id="74155-175">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="74155-175">Min: `00:00:00`</span></span><br><span data-ttu-id="74155-176">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="74155-176">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="74155-177">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-177">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-178">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="74155-178">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="74155-179">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="74155-179">Default: `10`</span></span><br><span data-ttu-id="74155-180">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-180">Min: `0`</span></span><br><span data-ttu-id="74155-181">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="74155-181">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="74155-182">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-183">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="74155-183">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="74155-184">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="74155-184">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="74155-185">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="74155-185">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="74155-186">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="74155-186">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="74155-187">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="74155-187">Default: `120`</span></span><br><span data-ttu-id="74155-188">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-188">Min: `0`</span></span><br><span data-ttu-id="74155-189">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="74155-189">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="74155-190">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-191">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="74155-191">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="74155-192">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="74155-192">Optional string attribute.</span></span></p><p><span data-ttu-id="74155-193">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-193">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="74155-194">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="74155-194">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="74155-195">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="74155-195">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="74155-196">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="74155-196">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="74155-197">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="74155-197">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="74155-198">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="74155-198">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="74155-199">属性</span><span class="sxs-lookup"><span data-stu-id="74155-199">Attribute</span></span> | <span data-ttu-id="74155-200">説明</span><span class="sxs-lookup"><span data-stu-id="74155-200">Description</span></span> | <span data-ttu-id="74155-201">既定値</span><span class="sxs-lookup"><span data-stu-id="74155-201">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="74155-202">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="74155-202">Optional string attribute.</span></span></p><p><span data-ttu-id="74155-203">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="74155-203">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="74155-204">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-204">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-205">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="74155-205">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="74155-206">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-206">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-207">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="74155-207">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="74155-208">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="74155-208">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="74155-209">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-210">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="74155-210">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="74155-211">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="74155-211">Default: `1`</span></span><br><span data-ttu-id="74155-212">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="74155-212">Min: `1`</span></span><br><span data-ttu-id="74155-213">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="74155-213">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="74155-214">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-214">Required string attribute.</span></span></p><p><span data-ttu-id="74155-215">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="74155-215">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="74155-216">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="74155-216">Relative paths are supported.</span></span> <span data-ttu-id="74155-217">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="74155-217">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="74155-218">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-218">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-219">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-219">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="74155-220">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="74155-220">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="74155-221">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="74155-221">Default: `10`</span></span><br><span data-ttu-id="74155-222">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-222">Min: `0`</span></span><br><span data-ttu-id="74155-223">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="74155-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="74155-224">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="74155-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="74155-225">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="74155-226">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="74155-227">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="74155-227">Default: `00:02:00`</span></span><br><span data-ttu-id="74155-228">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="74155-228">Min: `00:00:00`</span></span><br><span data-ttu-id="74155-229">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="74155-229">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="74155-230">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-230">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-231">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="74155-231">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="74155-232">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="74155-232">Default: `10`</span></span><br><span data-ttu-id="74155-233">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-233">Min: `0`</span></span><br><span data-ttu-id="74155-234">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="74155-234">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="74155-235">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-235">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-236">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="74155-236">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="74155-237">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="74155-237">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="74155-238">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="74155-238">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="74155-239">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="74155-239">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="74155-240">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="74155-240">Default: `120`</span></span><br><span data-ttu-id="74155-241">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-241">Min: `0`</span></span><br><span data-ttu-id="74155-242">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="74155-242">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="74155-243">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-243">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-244">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="74155-244">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="74155-245">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="74155-245">Optional string attribute.</span></span></p><p><span data-ttu-id="74155-246">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-246">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="74155-247">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="74155-247">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="74155-248">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="74155-248">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="74155-249">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="74155-249">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="74155-250">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="74155-250">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="74155-251">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="74155-251">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="74155-252">属性</span><span class="sxs-lookup"><span data-stu-id="74155-252">Attribute</span></span> | <span data-ttu-id="74155-253">説明</span><span class="sxs-lookup"><span data-stu-id="74155-253">Description</span></span> | <span data-ttu-id="74155-254">既定値</span><span class="sxs-lookup"><span data-stu-id="74155-254">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="74155-255">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="74155-255">Optional string attribute.</span></span></p><p><span data-ttu-id="74155-256">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="74155-256">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="74155-257">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-257">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-258">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="74155-258">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="74155-259">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-260">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="74155-260">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="74155-261">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="74155-261">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="74155-262">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-263">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="74155-263">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="74155-264">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="74155-264">Default: `1`</span></span><br><span data-ttu-id="74155-265">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="74155-265">Min: `1`</span></span><br><span data-ttu-id="74155-266">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="74155-266">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="74155-267">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-267">Required string attribute.</span></span></p><p><span data-ttu-id="74155-268">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="74155-268">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="74155-269">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="74155-269">Relative paths are supported.</span></span> <span data-ttu-id="74155-270">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="74155-270">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="74155-271">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-272">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-272">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="74155-273">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="74155-273">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="74155-274">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="74155-274">Default: `10`</span></span><br><span data-ttu-id="74155-275">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-275">Min: `0`</span></span><br><span data-ttu-id="74155-276">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="74155-276">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="74155-277">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="74155-277">Optional timespan attribute.</span></span></p><p><span data-ttu-id="74155-278">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-278">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="74155-279">ASP.NET Core 2.0 以前のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は整数でのみ指定する必要があります。そうしないと、既定値の 2 分に設定されます。</span><span class="sxs-lookup"><span data-stu-id="74155-279">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="74155-280">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="74155-280">Default: `00:02:00`</span></span><br><span data-ttu-id="74155-281">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="74155-281">Min: `00:00:00`</span></span><br><span data-ttu-id="74155-282">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="74155-282">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="74155-283">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-283">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-284">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="74155-284">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="74155-285">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="74155-285">Default: `10`</span></span><br><span data-ttu-id="74155-286">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-286">Min: `0`</span></span><br><span data-ttu-id="74155-287">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="74155-287">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="74155-288">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="74155-288">Optional integer attribute.</span></span></p><p><span data-ttu-id="74155-289">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="74155-289">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="74155-290">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="74155-290">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="74155-291">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="74155-291">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="74155-292">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="74155-292">Default: `120`</span></span><br><span data-ttu-id="74155-293">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="74155-293">Min: `0`</span></span><br><span data-ttu-id="74155-294">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="74155-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="74155-295">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="74155-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="74155-296">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="74155-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="74155-297">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="74155-297">Optional string attribute.</span></span></p><p><span data-ttu-id="74155-298">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="74155-299">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="74155-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="74155-300">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="74155-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="74155-301">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="74155-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="74155-302">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="74155-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="74155-303">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="74155-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="74155-304">環境変数の設定</span><span class="sxs-lookup"><span data-stu-id="74155-304">Setting environment variables</span></span>

<span data-ttu-id="74155-305">`processPath` 属性で、プロセスに対する環境変数を指定できます。</span><span class="sxs-lookup"><span data-stu-id="74155-305">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="74155-306">`environmentVariables` コレクション要素の `environmentVariable` 子要素で、環境変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="74155-306">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="74155-307">このセクションで設定された環境変数は、システム環境変数より優先されます。</span><span class="sxs-lookup"><span data-stu-id="74155-307">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="74155-308">次の例では、2 つの環境変数を設定しています。</span><span class="sxs-lookup"><span data-stu-id="74155-308">The following example sets two environment variables.</span></span> <span data-ttu-id="74155-309">`ASPNETCORE_ENVIRONMENT` は、`Development` に対するアプリの環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="74155-309">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="74155-310">開発者は、アプリの例外をデバッグするときに[開発者例外ページ](xref:fundamentals/error-handling)を強制的に読み込むため、*web.config* ファイルでこの値を一時的に設定できます。</span><span class="sxs-lookup"><span data-stu-id="74155-310">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="74155-311">`CONFIG_DIR` はユーザー定義の環境変数の例です。開発者はここに、アプリの構成ファイルを読み込むためのパスを形成するために起動時に値を読み取るコードを記述してあります。</span><span class="sxs-lookup"><span data-stu-id="74155-311">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="74155-312">インターネットなどの信頼されていないネットワークにアクセスできないステージング サーバーやテスト サーバーでは、`ASPNETCORE_ENVIRONMENT` 環境変数を `Development` に設定するだけです。</span><span class="sxs-lookup"><span data-stu-id="74155-312">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="74155-313">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="74155-313">app_offline.htm</span></span>

<span data-ttu-id="74155-314">*app_offline.htm* という名前のファイルがアプリのルート ディレクトリで検出された場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、受信要求の処理を停止することを試みます。</span><span class="sxs-lookup"><span data-stu-id="74155-314">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="74155-315">`shutdownTimeLimit` で定義されている秒数が経過してもまだアプリが実行している場合、ASP.NET Core モジュールは実行中のプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="74155-315">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="74155-316">*app_offline.htm* ファイルが存在している間、ASP.NET Core モジュールは、*app_offline.htm* ファイルの内容を返送することで、要求に応答します。</span><span class="sxs-lookup"><span data-stu-id="74155-316">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="74155-317">*app_offline.htm* ファイルが削除されると、次の要求によってアプリが起動されます。</span><span class="sxs-lookup"><span data-stu-id="74155-317">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="74155-318">アウト プロセス ホスティング モデルを使用するときは、開いている接続があると、アプリがすぐにシャットダウンされない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="74155-318">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="74155-319">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="74155-319">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="74155-320">起動エラー ページ</span><span class="sxs-lookup"><span data-stu-id="74155-320">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="74155-321">インプロセス ホスティングでもアウト プロセス ホスティングでも、アプリの起動に失敗すると、カスタム エラー ページが生成されます。</span><span class="sxs-lookup"><span data-stu-id="74155-321">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="74155-322">ASP.NET Core モジュールが、インプロセスまたはアウト プロセスのどちらかの要求ハンドラーの検索に失敗した場合、*500.0 - インプロセス/アウト プロセス ハンドラーの読み込みエラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="74155-322">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="74155-323">インプロセス ホスティングで、ASP.NET Core モジュールによるアプリの起動が失敗すると、*500.30 - 開始エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="74155-323">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="74155-324">アウト プロセス ホスティングで、ASP.NET Core モジュールがバックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="74155-324">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="74155-325">このページを抑制して、既定の IIS 5xx 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。</span><span class="sxs-lookup"><span data-stu-id="74155-325">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="74155-326">カスタム エラー メッセージの構成方法について詳しくは、[HTTP エラー &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/) に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="74155-326">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="74155-327">ASP.NET Core モジュールが、バックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="74155-327">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="74155-328">このページを抑制して、既定の IIS 502 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。</span><span class="sxs-lookup"><span data-stu-id="74155-328">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="74155-329">カスタム エラー メッセージの構成方法について詳しくは、[HTTP エラー &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/) に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="74155-329">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 処理エラーの状態コード ページ](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="74155-331">ログの作成とリダイレクト</span><span class="sxs-lookup"><span data-stu-id="74155-331">Log creation and redirection</span></span>

<span data-ttu-id="74155-332">`aspNetCore` 要素の `stdoutLogEnabled` 属性および `stdoutLogFile` 属性が設定されている場合は、stdout および stderr コンソール出力が ASP.NET Core モジュールによってディスクにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="74155-332">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="74155-333">モジュールがログ ファイルを作成するためには、`stdoutLogFile` パスのすべてのフォルダーが存在している必要があります。</span><span class="sxs-lookup"><span data-stu-id="74155-333">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="74155-334">アプリ プールは、ログが書き込まれる場所への書き込みアクセス権を持っている必要があります (書き込みアクセス許可を提供するには、`IIS AppPool\<app_pool_name>` を使います)。</span><span class="sxs-lookup"><span data-stu-id="74155-334">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="74155-335">プロセスのリサイクル/再起動が発生しない場合、ログは循環されません。</span><span class="sxs-lookup"><span data-stu-id="74155-335">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="74155-336">ログが使用するディスク領域を制限するのは、ホストの役割です。</span><span class="sxs-lookup"><span data-stu-id="74155-336">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="74155-337">stdout ログの使用は、アプリ起動時の問題をトラブルシューティングする場合にのみ推奨されます。</span><span class="sxs-lookup"><span data-stu-id="74155-337">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="74155-338">一般的なアプリ ログの目的には、stdout ログを使わないでください。</span><span class="sxs-lookup"><span data-stu-id="74155-338">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="74155-339">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="74155-339">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="74155-340">詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="74155-340">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="74155-341">ログ ファイルの作成時には、タイムスタンプとファイルの拡張子が自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="74155-341">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="74155-342">ログ ファイル名は、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) を `stdoutLogFile` パスの最後のセグメント (通常は *stdout*) にアンダースコアで区切って追加することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="74155-342">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="74155-343">`stdoutLogFile` パスが *stdout* で終わっている場合、PID が 1934 で 2018 年 2 月 5 日の 19:42:32 に作成されたアプリのログのファイル名は、*stdout_20180205194132_1934.log* になります。</span><span class="sxs-lookup"><span data-stu-id="74155-343">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="74155-344">`stdoutLogEnabled` が false の場合は、アプリの起動時に発生するエラーがキャプチャされ、30 KB までイベント ログに出力されます。</span><span class="sxs-lookup"><span data-stu-id="74155-344">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="74155-345">起動後、すべての追加のログが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="74155-345">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="74155-346">次の `aspNetCore` 要素の例では、Azure App Service でホストされているアプリの stdout ログを構成しています。</span><span class="sxs-lookup"><span data-stu-id="74155-346">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="74155-347">ローカル ログには、ローカル パスまたはネットワーク共有パスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="74155-347">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="74155-348">AppPool のユーザー ID に、指定されたパスへの書き込みアクセス許可があることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="74155-348">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="74155-349">強化された診断ログ</span><span class="sxs-lookup"><span data-stu-id="74155-349">Enhanced diagnostic logs</span></span>

<span data-ttu-id="74155-350">ASP.NET Core モジュールは、強化された診断ログを提供するよう構成できます。</span><span class="sxs-lookup"><span data-stu-id="74155-350">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="74155-351">*web.config* で、`<aspNetCore>` 要素に `<handlerSettings>` 要素を追加します。`debugLevel` を `TRACE` に設定すると、診断情報が再現性の高いものになります。</span><span class="sxs-lookup"><span data-stu-id="74155-351">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="74155-352">デバッグ レベル (`debugLevel`) 値には、レベルと場所の両方を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="74155-352">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="74155-353">レベルは次のとおりです (情報量が少ないものから多いものへの順)。</span><span class="sxs-lookup"><span data-stu-id="74155-353">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="74155-354">ERROR</span><span class="sxs-lookup"><span data-stu-id="74155-354">ERROR</span></span>
* <span data-ttu-id="74155-355">WARNING</span><span class="sxs-lookup"><span data-stu-id="74155-355">WARNING</span></span>
* <span data-ttu-id="74155-356">INFO</span><span class="sxs-lookup"><span data-stu-id="74155-356">INFO</span></span>
* <span data-ttu-id="74155-357">TRACE</span><span class="sxs-lookup"><span data-stu-id="74155-357">TRACE</span></span>

<span data-ttu-id="74155-358">場所は次のとおりです (複数の場所を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="74155-358">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="74155-359">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="74155-359">CONSOLE</span></span>
* <span data-ttu-id="74155-360">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="74155-360">EVENTLOG</span></span>
* <span data-ttu-id="74155-361">ファイル</span><span class="sxs-lookup"><span data-stu-id="74155-361">FILE</span></span>

<span data-ttu-id="74155-362">ハンドラー設定は、次の環境変数を使用して指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="74155-362">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="74155-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; デバッグ ログ ファイルへのパス </span><span class="sxs-lookup"><span data-stu-id="74155-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="74155-364">(既定: *aspnetcore debug.log*)。</span><span class="sxs-lookup"><span data-stu-id="74155-364">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="74155-365">`ASPNETCORE_MODULE_DEBUG` &ndash; デバッグ レベルの設定。</span><span class="sxs-lookup"><span data-stu-id="74155-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="74155-366">配置内でデバッグ ログを、問題のトラブルシューティングに必要な時間よりも長く有効のままに**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="74155-366">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="74155-367">ログのサイズは制限されていません。</span><span class="sxs-lookup"><span data-stu-id="74155-367">The size of the log isn't limited.</span></span> <span data-ttu-id="74155-368">デバッグ ログを有効のままにすると、使用可能なディスク領域が使い果たされ、サーバーまたはアプリ サービスがクラッシュする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="74155-368">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="74155-369">*web.config* ファイルでの `aspNetCore` 要素の例については、「[web.config での構成](#configuration-with-webconfig)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="74155-369">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="74155-370">プロキシの構成で HTTP プロトコルとペアリング トークンを使用する</span><span class="sxs-lookup"><span data-stu-id="74155-370">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="74155-371">*アウト プロセス ホスティングにのみ適用されます。*</span><span class="sxs-lookup"><span data-stu-id="74155-371">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="74155-372">ASP.NET Core モジュールと Kestrel の間に作成されるプロキシは、HTTP プロトコルを使います。</span><span class="sxs-lookup"><span data-stu-id="74155-372">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="74155-373">HTTP を使うと、パフォーマンスが最適化されます。この場合、モジュールと Kestrel の間のトラフィックは、ネットワーク インターフェイスから切り離されたループバック アドレスで発生します。</span><span class="sxs-lookup"><span data-stu-id="74155-373">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="74155-374">モジュールと Kestrel の間のトラフィックが、サーバーから離れた場所で傍受される危険はありません。</span><span class="sxs-lookup"><span data-stu-id="74155-374">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="74155-375">ペアリング トークンを使用すると、Kestrel によって受信される要求が IIS によってプロキシされたものであり、他のソースからのものでないことを保証できます。</span><span class="sxs-lookup"><span data-stu-id="74155-375">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="74155-376">ペアリング トークンは、モジュールによって作成されて、環境変数 (`ASPNETCORE_TOKEN`) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="74155-376">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="74155-377">ペアリング トークンはまた、プロキシされたすべての要求のヘッダー (`MSAspNetCoreToken`) にも設定されます。</span><span class="sxs-lookup"><span data-stu-id="74155-377">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="74155-378">IIS ミドルウェアは、受信した各要求をチェックし、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="74155-378">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="74155-379">トークンの値が一致しない場合、要求はログに記録され、拒否されます。</span><span class="sxs-lookup"><span data-stu-id="74155-379">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="74155-380">ペアリング トークン環境変数およびモジュールと Kestrel の間のトラフィックには、サーバーから離れた場所からアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="74155-380">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="74155-381">ペアリング トークンの値がわからなければ、攻撃者は IIS ミドルウェアのチェックをバイパスする要求を送信できません。</span><span class="sxs-lookup"><span data-stu-id="74155-381">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="74155-382">IIS 共有構成での ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="74155-382">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="74155-383">ASP.NET Core モジュールのインストーラーは、**SYSTEM** アカウントのアクセス許可を使って実行します。</span><span class="sxs-lookup"><span data-stu-id="74155-383">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="74155-384">ローカル システム アカウントには、IIS 共有構成によって使われる共有パスに対する変更アクセス許可がないため、インストーラーが共有上の *applicationHost.config* 内のモジュール設定を構成しようとすると、アクセス拒否エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="74155-384">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="74155-385">IIS 共有構成を使うときは、次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="74155-385">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="74155-386">IIS 共有構成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="74155-386">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="74155-387">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="74155-387">Run the installer.</span></span>
1. <span data-ttu-id="74155-388">更新された *applicationHost.config* ファイルを共有にエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="74155-388">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="74155-389">IIS 共有構成を再び有効にします。</span><span class="sxs-lookup"><span data-stu-id="74155-389">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="74155-390">モジュールのバージョンとホスティング バンドル インストーラーのログ</span><span class="sxs-lookup"><span data-stu-id="74155-390">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="74155-391">インストールされている ASP.NET Core モジュールのバージョンを確認するには:</span><span class="sxs-lookup"><span data-stu-id="74155-391">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="74155-392">ホスティング システム上で、*%windir%\System32\inetsrv* に移動します。</span><span class="sxs-lookup"><span data-stu-id="74155-392">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="74155-393">*aspnetcore.dll* ファイルを探します。</span><span class="sxs-lookup"><span data-stu-id="74155-393">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="74155-394">ファイルを右クリックし、コンテキスト メニューの **[プロパティ]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="74155-394">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="74155-395">**[詳細]** タブを選びます。**[ファイル バージョン]** と **[製品バージョン]** が、インストールされているモジュールのバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="74155-395">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="74155-396">モジュールのホスティング バンドル インストーラーのログは、*C:\\Users\\%UserName%\\AppData\\Local\\Temp* にあります。ファイルの名前は、*dd_DotNetCoreWinSvrHosting__\<タイムスタンプ>_000_AspNetCoreModule_x64.log* です。</span><span class="sxs-lookup"><span data-stu-id="74155-396">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="74155-397">モジュール、スキーマ、構成ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="74155-397">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="74155-398">Module</span><span class="sxs-lookup"><span data-stu-id="74155-398">Module</span></span>

<span data-ttu-id="74155-399">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="74155-399">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="74155-400">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="74155-400">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="74155-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="74155-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="74155-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="74155-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="74155-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="74155-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="74155-404">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="74155-404">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="74155-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="74155-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="74155-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="74155-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="74155-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="74155-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="74155-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="74155-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="74155-409">Schema</span><span class="sxs-lookup"><span data-stu-id="74155-409">Schema</span></span>

<span data-ttu-id="74155-410">**IIS**</span><span class="sxs-lookup"><span data-stu-id="74155-410">**IIS**</span></span>

   * <span data-ttu-id="74155-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="74155-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="74155-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="74155-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="74155-413">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="74155-413">**IIS Express**</span></span>

   * <span data-ttu-id="74155-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="74155-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="74155-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="74155-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="74155-416">構成</span><span class="sxs-lookup"><span data-stu-id="74155-416">Configuration</span></span>

<span data-ttu-id="74155-417">**IIS**</span><span class="sxs-lookup"><span data-stu-id="74155-417">**IIS**</span></span>

   * <span data-ttu-id="74155-418">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="74155-418">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="74155-419">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="74155-419">**IIS Express**</span></span>

   * <span data-ttu-id="74155-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="74155-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="74155-421">ファイルは、*applicationHost.config* ファイルで *aspnetcore* を検索することにより見つかります。</span><span class="sxs-lookup"><span data-stu-id="74155-421">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>