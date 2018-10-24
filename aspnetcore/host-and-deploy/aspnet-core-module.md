---
title: ASP.NET Core モジュール構成リファレンス
author: guardrex
description: ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ae19b26bc86c9da7a61f3117aaae1844115593a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913282"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="d1897-103">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="d1897-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="d1897-104">著者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="d1897-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="d1897-105">このドキュメントでは、ASP.NET Core アプリをホストするための ASP.NET Core モジュールの構成方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d1897-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="d1897-106">ASP.NET Core モジュールの概要とインストールの説明については、「[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d1897-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="d1897-107">ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="d1897-107">Hosting model</span></span>

<span data-ttu-id="d1897-108">.NET Core 2.2 以降で実行されるアプリの場合、モジュールでは、インプロセス ホスティング モデルがサポートされています。このモデルを使用するとリバース プロキシ (アウト ホスティング) と比較してパフォーマンスが改善されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="d1897-109">詳細については、「<xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d1897-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="d1897-110">インプロセス ホスティングは既存のアプリではオプトインになっていますが、[dotnet new](/dotnet/core/tools/dotnet-new) テンプレートは既定ではすべての IIS および IIS Express シナリオにおいてインプロセス ホスティング モデルに設定されています。</span><span class="sxs-lookup"><span data-stu-id="d1897-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="d1897-111">アプリに対してインプロセス ホスティングを構成するには、そのアプリのプロジェクト ファイルに `<AspNetCoreModuleHostingModel>` プロパティを追加して、値を `inprocess` に設定します。(アウト プロセス ホスティングは `outofprocess` を使用して設定します)。</span><span class="sxs-lookup"><span data-stu-id="d1897-111">To configure an app for in-process hosting, add the `<AspNetCoreModuleHostingModel>` property to the app's project file with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

<span data-ttu-id="d1897-112">インプロセスでホストする場合は、次の特性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="d1897-113">[Kestrel サーバー](xref:fundamentals/servers/kestrel)は使用されません。</span><span class="sxs-lookup"><span data-stu-id="d1897-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="d1897-114">カスタム <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 実装である `IISHttpServer` は、アプリのサーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="d1897-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="d1897-115">[requestTimeout 属性](#attributes-of-the-aspnetcore-element) は、インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="d1897-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="d1897-116">アプリ プールをアプリ間で共有することはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="d1897-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="d1897-117">アプリごとに 1 つのアプリ プールを使用します。</span><span class="sxs-lookup"><span data-stu-id="d1897-117">Use one app pool per app.</span></span>

* <span data-ttu-id="d1897-118">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用したとき、または [app_offline.htm ファイルを手動で配置内に置いた](xref:host-and-deploy/iis/index#locked-deployment-files)ときには、開いている接続があると、アプリがすぐにシャットダウンされない場合があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="d1897-119">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="d1897-120">アプリとインストールされているランタイム (x64 または x86) のアーキテクチャ (ビット) は、アプリ プールのアーキテクチャと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="d1897-121">([CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) ではなく) `WebHostBuilder` を使用してアプリのホストを手動で設定するときに、アプリがこれまで Kestrel サーバー上で直接実行されていた場合 (セルフホステッド)、`UseKestrel` を呼び出してから `UseIISIntegration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d1897-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="d1897-122">順序を逆にすると、ホストは開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="d1897-122">If the order is reversed, the host fails to start.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="d1897-123">ホスティング モデルの変更</span><span class="sxs-lookup"><span data-stu-id="d1897-123">Hosting model changes</span></span>

<span data-ttu-id="d1897-124">`hostingModel` 設定が *web.config* ファイル内で変更された場合 (「[web.config での構成](#configuration-with-webconfig)」セクションを参照)、モジュールによって IIS 用のワーカー プロセスがリサイクルされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-124">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="d1897-125">IIS Express の場合、モジュールによってワーカー プロセスのリサイクルは行われませんが、代わりに、現在の IIS Express プロセスの正常なシャットダウンがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-125">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="d1897-126">アプリに対して次の要求が出されると、IIS Express の新しいプロセスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-126">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="d1897-127">プロセス名</span><span class="sxs-lookup"><span data-stu-id="d1897-127">Process name</span></span>

<span data-ttu-id="d1897-128">`Process.GetCurrentProcess().ProcessName` から、`w3wp` (インプロセス) または `dotnet` (アウト プロセス) のいずれかがレポートされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-128">`Process.GetCurrentProcess().ProcessName` reports either `w3wp` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="d1897-129">web.config での構成</span><span class="sxs-lookup"><span data-stu-id="d1897-129">Configuration with web.config</span></span>

<span data-ttu-id="d1897-130">ASP.NET Core モジュールは、サイトの *web.config* ファイルの `system.webServer` ノードの `aspNetCore` セクションを使って構成します。</span><span class="sxs-lookup"><span data-stu-id="d1897-130">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="d1897-131">次に示す *web.config* ファイルは、[フレームワークに依存する展開](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)用に発行されたもので、サイトの要求を処理するように ASP.NET Core モジュールを構成します。</span><span class="sxs-lookup"><span data-stu-id="d1897-131">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
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

<span data-ttu-id="d1897-132">次の *web.config* は、[自己完結型の展開](/dotnet/articles/core/deploying/#self-contained-deployments-scd)用に発行されたものです。</span><span class="sxs-lookup"><span data-stu-id="d1897-132">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
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

<span data-ttu-id="d1897-133">アプリが [Azure App Service](https://azure.microsoft.com/services/app-service/) に対して展開されると、`stdoutLogFile` パスは `\\?\%home%\LogFiles\stdout` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-133">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="d1897-134">パスは stdout ログを *LogFiles* フォルダーに保存します。これは、サービスによって自動的に作成される場所です。</span><span class="sxs-lookup"><span data-stu-id="d1897-134">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="d1897-135">サブアプリでの *web.config* ファイルの構成に関する重要な注意事項については、「[サブアプリケーション構成](xref:host-and-deploy/iis/index#sub-application-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d1897-135">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="d1897-136">aspNetCore 要素の属性</span><span class="sxs-lookup"><span data-stu-id="d1897-136">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="d1897-137">属性</span><span class="sxs-lookup"><span data-stu-id="d1897-137">Attribute</span></span> | <span data-ttu-id="d1897-138">説明</span><span class="sxs-lookup"><span data-stu-id="d1897-138">Description</span></span> | <span data-ttu-id="d1897-139">既定値</span><span class="sxs-lookup"><span data-stu-id="d1897-139">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="d1897-140">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-140">Optional string attribute.</span></span></p><p><span data-ttu-id="d1897-141">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="d1897-141">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="d1897-142">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-142">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-143">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-143">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="d1897-144">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-144">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-145">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-145">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="d1897-146">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="d1897-146">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="d1897-147">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-147">Optional string attribute.</span></span></p><p><span data-ttu-id="d1897-148">ホスティング モデルをインプロセス (`inprocess`) またはアウト プロセス (`outofprocess`) として指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-148">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="d1897-149">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-149">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-150">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-150">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="d1897-151">&dagger;インプロセス ホスティングの場合、値は `1` に制限されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-151">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="d1897-152">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="d1897-152">Default: `1`</span></span><br><span data-ttu-id="d1897-153">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="d1897-153">Min: `1`</span></span><br><span data-ttu-id="d1897-154">最大値: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="d1897-154">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="d1897-155">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-155">Required string attribute.</span></span></p><p><span data-ttu-id="d1897-156">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="d1897-156">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="d1897-157">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d1897-157">Relative paths are supported.</span></span> <span data-ttu-id="d1897-158">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-158">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="d1897-159">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-159">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-160">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-160">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="d1897-161">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="d1897-161">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="d1897-162">インプロセス ホスティングではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="d1897-162">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="d1897-163">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="d1897-163">Default: `10`</span></span><br><span data-ttu-id="d1897-164">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-164">Min: `0`</span></span><br><span data-ttu-id="d1897-165">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="d1897-165">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="d1897-166">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-166">Optional timespan attribute.</span></span></p><p><span data-ttu-id="d1897-167">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-167">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="d1897-168">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-168">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="d1897-169">インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="d1897-169">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="d1897-170">インプロセス ホスティングの場合、アプリによって要求が処理されるまでモジュールは待機します。</span><span class="sxs-lookup"><span data-stu-id="d1897-170">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="d1897-171">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-171">Default: `00:02:00`</span></span><br><span data-ttu-id="d1897-172">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-172">Min: `00:00:00`</span></span><br><span data-ttu-id="d1897-173">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-173">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="d1897-174">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-174">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-175">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="d1897-175">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="d1897-176">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="d1897-176">Default: `10`</span></span><br><span data-ttu-id="d1897-177">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-177">Min: `0`</span></span><br><span data-ttu-id="d1897-178">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="d1897-178">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="d1897-179">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-179">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-180">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="d1897-180">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="d1897-181">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="d1897-181">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="d1897-182">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="d1897-182">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="d1897-183">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="d1897-183">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="d1897-184">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="d1897-184">Default: `120`</span></span><br><span data-ttu-id="d1897-185">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-185">Min: `0`</span></span><br><span data-ttu-id="d1897-186">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="d1897-186">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="d1897-187">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-187">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-188">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-188">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="d1897-189">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-189">Optional string attribute.</span></span></p><p><span data-ttu-id="d1897-190">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-190">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="d1897-191">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="d1897-191">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="d1897-192">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="d1897-192">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="d1897-193">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-193">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d1897-194">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-194">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="d1897-195">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-195">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="d1897-196">属性</span><span class="sxs-lookup"><span data-stu-id="d1897-196">Attribute</span></span> | <span data-ttu-id="d1897-197">説明</span><span class="sxs-lookup"><span data-stu-id="d1897-197">Description</span></span> | <span data-ttu-id="d1897-198">既定値</span><span class="sxs-lookup"><span data-stu-id="d1897-198">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="d1897-199">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-199">Optional string attribute.</span></span></p><p><span data-ttu-id="d1897-200">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="d1897-200">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="d1897-201">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-201">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-202">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-202">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="d1897-203">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-204">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-204">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="d1897-205">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="d1897-205">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="d1897-206">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-206">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-207">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-207">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="d1897-208">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="d1897-208">Default: `1`</span></span><br><span data-ttu-id="d1897-209">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="d1897-209">Min: `1`</span></span><br><span data-ttu-id="d1897-210">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="d1897-210">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="d1897-211">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-211">Required string attribute.</span></span></p><p><span data-ttu-id="d1897-212">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="d1897-212">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="d1897-213">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d1897-213">Relative paths are supported.</span></span> <span data-ttu-id="d1897-214">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-214">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="d1897-215">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-215">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-216">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-216">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="d1897-217">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="d1897-217">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="d1897-218">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="d1897-218">Default: `10`</span></span><br><span data-ttu-id="d1897-219">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-219">Min: `0`</span></span><br><span data-ttu-id="d1897-220">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="d1897-220">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="d1897-221">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-221">Optional timespan attribute.</span></span></p><p><span data-ttu-id="d1897-222">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-222">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="d1897-223">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-223">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="d1897-224">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-224">Default: `00:02:00`</span></span><br><span data-ttu-id="d1897-225">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-225">Min: `00:00:00`</span></span><br><span data-ttu-id="d1897-226">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-226">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="d1897-227">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-228">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="d1897-228">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="d1897-229">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="d1897-229">Default: `10`</span></span><br><span data-ttu-id="d1897-230">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-230">Min: `0`</span></span><br><span data-ttu-id="d1897-231">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="d1897-231">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="d1897-232">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-232">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-233">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="d1897-233">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="d1897-234">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="d1897-234">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="d1897-235">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="d1897-235">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="d1897-236">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="d1897-236">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="d1897-237">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="d1897-237">Default: `120`</span></span><br><span data-ttu-id="d1897-238">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-238">Min: `0`</span></span><br><span data-ttu-id="d1897-239">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="d1897-239">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="d1897-240">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-240">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-241">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-241">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="d1897-242">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-242">Optional string attribute.</span></span></p><p><span data-ttu-id="d1897-243">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-243">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="d1897-244">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="d1897-244">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="d1897-245">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="d1897-245">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="d1897-246">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-246">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d1897-247">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-247">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="d1897-248">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-248">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="d1897-249">属性</span><span class="sxs-lookup"><span data-stu-id="d1897-249">Attribute</span></span> | <span data-ttu-id="d1897-250">説明</span><span class="sxs-lookup"><span data-stu-id="d1897-250">Description</span></span> | <span data-ttu-id="d1897-251">既定値</span><span class="sxs-lookup"><span data-stu-id="d1897-251">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="d1897-252">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-252">Optional string attribute.</span></span></p><p><span data-ttu-id="d1897-253">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="d1897-253">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="d1897-254">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-254">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-255">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-255">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="d1897-256">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-257">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-257">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="d1897-258">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="d1897-258">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="d1897-259">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-259">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-260">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-260">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="d1897-261">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="d1897-261">Default: `1`</span></span><br><span data-ttu-id="d1897-262">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="d1897-262">Min: `1`</span></span><br><span data-ttu-id="d1897-263">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="d1897-263">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="d1897-264">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-264">Required string attribute.</span></span></p><p><span data-ttu-id="d1897-265">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="d1897-265">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="d1897-266">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d1897-266">Relative paths are supported.</span></span> <span data-ttu-id="d1897-267">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-267">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="d1897-268">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-268">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-269">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-269">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="d1897-270">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="d1897-270">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="d1897-271">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="d1897-271">Default: `10`</span></span><br><span data-ttu-id="d1897-272">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-272">Min: `0`</span></span><br><span data-ttu-id="d1897-273">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="d1897-273">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="d1897-274">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-274">Optional timespan attribute.</span></span></p><p><span data-ttu-id="d1897-275">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-275">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="d1897-276">ASP.NET Core 2.0 以前のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は整数でのみ指定する必要があります。そうしないと、既定値の 2 分に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-276">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="d1897-277">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-277">Default: `00:02:00`</span></span><br><span data-ttu-id="d1897-278">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-278">Min: `00:00:00`</span></span><br><span data-ttu-id="d1897-279">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="d1897-279">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="d1897-280">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-281">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="d1897-281">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="d1897-282">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="d1897-282">Default: `10`</span></span><br><span data-ttu-id="d1897-283">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-283">Min: `0`</span></span><br><span data-ttu-id="d1897-284">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="d1897-284">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="d1897-285">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="d1897-285">Optional integer attribute.</span></span></p><p><span data-ttu-id="d1897-286">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="d1897-286">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="d1897-287">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="d1897-287">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="d1897-288">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="d1897-288">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="d1897-289">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="d1897-289">Default: `120`</span></span><br><span data-ttu-id="d1897-290">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="d1897-290">Min: `0`</span></span><br><span data-ttu-id="d1897-291">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="d1897-291">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="d1897-292">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="d1897-292">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="d1897-293">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-293">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="d1897-294">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="d1897-294">Optional string attribute.</span></span></p><p><span data-ttu-id="d1897-295">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-295">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="d1897-296">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="d1897-296">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="d1897-297">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="d1897-297">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="d1897-298">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-298">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d1897-299">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-299">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="d1897-300">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-300">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="d1897-301">環境変数の設定</span><span class="sxs-lookup"><span data-stu-id="d1897-301">Setting environment variables</span></span>

<span data-ttu-id="d1897-302">`processPath` 属性で、プロセスに対する環境変数を指定できます。</span><span class="sxs-lookup"><span data-stu-id="d1897-302">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="d1897-303">`environmentVariables` コレクション要素の `environmentVariable` 子要素で、環境変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="d1897-303">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="d1897-304">このセクションで設定された環境変数は、システム環境変数より優先されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-304">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="d1897-305">次の例では、2 つの環境変数を設定しています。</span><span class="sxs-lookup"><span data-stu-id="d1897-305">The following example sets two environment variables.</span></span> <span data-ttu-id="d1897-306">`ASPNETCORE_ENVIRONMENT` は、`Development` に対するアプリの環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="d1897-306">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="d1897-307">開発者は、アプリの例外をデバッグするときに[開発者例外ページ](xref:fundamentals/error-handling)を強制的に読み込むため、*web.config* ファイルでこの値を一時的に設定できます。</span><span class="sxs-lookup"><span data-stu-id="d1897-307">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="d1897-308">`CONFIG_DIR` はユーザー定義の環境変数の例です。開発者はここに、アプリの構成ファイルを読み込むためのパスを形成するために起動時に値を読み取るコードを記述してあります。</span><span class="sxs-lookup"><span data-stu-id="d1897-308">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="d1897-309">インターネットなどの信頼されていないネットワークにアクセスできないステージング サーバーやテスト サーバーでは、`ASPNETCORE_ENVIRONMENT` 環境変数を `Development` に設定するだけです。</span><span class="sxs-lookup"><span data-stu-id="d1897-309">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="d1897-310">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="d1897-310">app_offline.htm</span></span>

<span data-ttu-id="d1897-311">*app_offline.htm* という名前のファイルがアプリのルート ディレクトリで検出された場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、受信要求の処理を停止することを試みます。</span><span class="sxs-lookup"><span data-stu-id="d1897-311">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="d1897-312">`shutdownTimeLimit` で定義されている秒数が経過してもまだアプリが実行している場合、ASP.NET Core モジュールは実行中のプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="d1897-312">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="d1897-313">*app_offline.htm* ファイルが存在している間、ASP.NET Core モジュールは、*app_offline.htm* ファイルの内容を返送することで、要求に応答します。</span><span class="sxs-lookup"><span data-stu-id="d1897-313">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="d1897-314">*app_offline.htm* ファイルが削除されると、次の要求によってアプリが起動されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-314">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d1897-315">アウト プロセス ホスティング モデルを使用するときは、開いている接続があると、アプリがすぐにシャットダウンされない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-315">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="d1897-316">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-316">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="d1897-317">起動エラー ページ</span><span class="sxs-lookup"><span data-stu-id="d1897-317">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d1897-318">*アウト プロセス ホスティングにのみ適用されます。*</span><span class="sxs-lookup"><span data-stu-id="d1897-318">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="d1897-319">ASP.NET Core モジュールが、バックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-319">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="d1897-320">このページを抑制して、既定の IIS 502 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。</span><span class="sxs-lookup"><span data-stu-id="d1897-320">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="d1897-321">カスタム エラー メッセージの構成方法について詳しくは、[HTTP エラー `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/) に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d1897-321">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 処理エラーの状態コード ページ](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="d1897-323">ログの作成とリダイレクト</span><span class="sxs-lookup"><span data-stu-id="d1897-323">Log creation and redirection</span></span>

<span data-ttu-id="d1897-324">`aspNetCore` 要素の `stdoutLogEnabled` 属性および `stdoutLogFile` 属性が設定されている場合は、stdout および stderr コンソール出力が ASP.NET Core モジュールによってディスクにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="d1897-324">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="d1897-325">モジュールがログ ファイルを作成するためには、`stdoutLogFile` パスのすべてのフォルダーが存在している必要があります。</span><span class="sxs-lookup"><span data-stu-id="d1897-325">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="d1897-326">アプリ プールは、ログが書き込まれる場所への書き込みアクセス権を持っている必要があります (書き込みアクセス許可を提供するには、`IIS AppPool\<app_pool_name>` を使います)。</span><span class="sxs-lookup"><span data-stu-id="d1897-326">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="d1897-327">プロセスのリサイクル/再起動が発生しない場合、ログは循環されません。</span><span class="sxs-lookup"><span data-stu-id="d1897-327">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="d1897-328">ログが使用するディスク領域を制限するのは、ホストの役割です。</span><span class="sxs-lookup"><span data-stu-id="d1897-328">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="d1897-329">stdout ログの使用は、アプリ起動時の問題をトラブルシューティングする場合にのみ推奨されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-329">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="d1897-330">一般的なアプリ ログの目的には、stdout ログを使わないでください。</span><span class="sxs-lookup"><span data-stu-id="d1897-330">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="d1897-331">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="d1897-331">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="d1897-332">詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d1897-332">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="d1897-333">ログ ファイルの作成時には、タイムスタンプとファイルの拡張子が自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-333">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="d1897-334">ログ ファイル名は、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) を `stdoutLogFile` パスの最後のセグメント (通常は *stdout*) にアンダースコアで区切って追加することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-334">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="d1897-335">`stdoutLogFile` パスが *stdout* で終わっている場合、PID が 1934 で 2018 年 2 月 5 日の 19:42:32 に作成されたアプリのログのファイル名は、*stdout_20180205194132_1934.log* になります。</span><span class="sxs-lookup"><span data-stu-id="d1897-335">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="d1897-336">次の `aspNetCore` 要素の例では、Azure App Service でホストされているアプリの stdout ログを構成しています。</span><span class="sxs-lookup"><span data-stu-id="d1897-336">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="d1897-337">ローカル ログには、ローカル パスまたはネットワーク共有パスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d1897-337">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="d1897-338">AppPool のユーザー ID に、指定されたパスへの書き込みアクセス許可があることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d1897-338">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

<span data-ttu-id="d1897-339">*web.config* ファイルでの `aspNetCore` 要素の例については、「[web.config での構成](#configuration-with-webconfig)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d1897-339">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="d1897-340">プロキシの構成で HTTP プロトコルとペアリング トークンを使用する</span><span class="sxs-lookup"><span data-stu-id="d1897-340">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d1897-341">*アウト プロセス ホスティングにのみ適用されます。*</span><span class="sxs-lookup"><span data-stu-id="d1897-341">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="d1897-342">ASP.NET Core モジュールと Kestrel の間に作成されるプロキシは、HTTP プロトコルを使います。</span><span class="sxs-lookup"><span data-stu-id="d1897-342">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="d1897-343">HTTP を使うと、パフォーマンスが最適化されます。この場合、モジュールと Kestrel の間のトラフィックは、ネットワーク インターフェイスから切り離されたループバック アドレスで発生します。</span><span class="sxs-lookup"><span data-stu-id="d1897-343">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="d1897-344">モジュールと Kestrel の間のトラフィックが、サーバーから離れた場所で傍受される危険はありません。</span><span class="sxs-lookup"><span data-stu-id="d1897-344">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="d1897-345">ペアリング トークンを使用すると、Kestrel によって受信される要求が IIS によってプロキシされたものであり、他のソースからのものでないことを保証できます。</span><span class="sxs-lookup"><span data-stu-id="d1897-345">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="d1897-346">ペアリング トークンは、モジュールによって作成されて、環境変数 (`ASPNETCORE_TOKEN`) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-346">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="d1897-347">ペアリング トークンはまた、プロキシされたすべての要求のヘッダー (`MSAspNetCoreToken`) にも設定されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-347">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="d1897-348">IIS ミドルウェアは、受信した各要求をチェックし、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d1897-348">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="d1897-349">トークンの値が一致しない場合、要求はログに記録され、拒否されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-349">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="d1897-350">ペアリング トークン環境変数およびモジュールと Kestrel の間のトラフィックには、サーバーから離れた場所からアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d1897-350">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="d1897-351">ペアリング トークンの値がわからなければ、攻撃者は IIS ミドルウェアのチェックをバイパスする要求を送信できません。</span><span class="sxs-lookup"><span data-stu-id="d1897-351">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="d1897-352">IIS 共有構成での ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="d1897-352">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="d1897-353">ASP.NET Core モジュールのインストーラーは、**SYSTEM** アカウントのアクセス許可を使って実行します。</span><span class="sxs-lookup"><span data-stu-id="d1897-353">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="d1897-354">ローカル システム アカウントには、IIS 共有構成によって使われる共有パスに対する変更アクセス許可がないため、インストーラーが共有上の *applicationHost.config* 内のモジュール設定を構成しようとすると、アクセス拒否エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="d1897-354">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="d1897-355">IIS 共有構成を使うときは、次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="d1897-355">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="d1897-356">IIS 共有構成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="d1897-356">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="d1897-357">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="d1897-357">Run the installer.</span></span>
1. <span data-ttu-id="d1897-358">更新された *applicationHost.config* ファイルを共有にエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="d1897-358">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="d1897-359">IIS 共有構成を再び有効にします。</span><span class="sxs-lookup"><span data-stu-id="d1897-359">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="d1897-360">モジュールのバージョンとホスティング バンドル インストーラーのログ</span><span class="sxs-lookup"><span data-stu-id="d1897-360">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="d1897-361">インストールされている ASP.NET Core モジュールのバージョンを確認するには:</span><span class="sxs-lookup"><span data-stu-id="d1897-361">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="d1897-362">ホスティング システム上で、*%windir%\System32\inetsrv* に移動します。</span><span class="sxs-lookup"><span data-stu-id="d1897-362">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="d1897-363">*aspnetcore.dll* ファイルを探します。</span><span class="sxs-lookup"><span data-stu-id="d1897-363">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="d1897-364">ファイルを右クリックし、コンテキスト メニューの **[プロパティ]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="d1897-364">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="d1897-365">**[詳細]** タブを選びます。**[ファイル バージョン]** と **[製品バージョン]** が、インストールされているモジュールのバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="d1897-365">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="d1897-366">モジュールのホスティング バンドル インストーラーのログは、*C:\\Users\\%UserName%\\AppData\\Local\\Temp* にあります。ファイルの名前は、*dd_DotNetCoreWinSvrHosting__\<タイムスタンプ>_000_AspNetCoreModule_x64.log* です。</span><span class="sxs-lookup"><span data-stu-id="d1897-366">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="d1897-367">モジュール、スキーマ、構成ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="d1897-367">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="d1897-368">Module</span><span class="sxs-lookup"><span data-stu-id="d1897-368">Module</span></span>

<span data-ttu-id="d1897-369">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="d1897-369">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="d1897-370">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d1897-370">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="d1897-371">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d1897-371">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="d1897-372">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="d1897-372">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="d1897-373">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d1897-373">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="d1897-374">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="d1897-374">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="d1897-375">Schema</span><span class="sxs-lookup"><span data-stu-id="d1897-375">Schema</span></span>

<span data-ttu-id="d1897-376">**IIS**</span><span class="sxs-lookup"><span data-stu-id="d1897-376">**IIS**</span></span>

   * <span data-ttu-id="d1897-377">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="d1897-377">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="d1897-378">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="d1897-378">**IIS Express**</span></span>

   * <span data-ttu-id="d1897-379">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="d1897-379">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="d1897-380">構成</span><span class="sxs-lookup"><span data-stu-id="d1897-380">Configuration</span></span>

<span data-ttu-id="d1897-381">**IIS**</span><span class="sxs-lookup"><span data-stu-id="d1897-381">**IIS**</span></span>

   * <span data-ttu-id="d1897-382">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="d1897-382">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="d1897-383">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="d1897-383">**IIS Express**</span></span>

   * <span data-ttu-id="d1897-384">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="d1897-384">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="d1897-385">ファイルは、*applicationHost.config* ファイルで *aspnetcore.dll* を検索することにより見つかります。</span><span class="sxs-lookup"><span data-stu-id="d1897-385">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="d1897-386">IIS Express の場合、*applicationHost.config* ファイルは既定では存在しません。</span><span class="sxs-lookup"><span data-stu-id="d1897-386">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="d1897-387">ファイルは、Visual Studio ソリューションにおいて Web アプリ プロジェクトを開始すると、*\<アプリケーション ルート>\\.vs\\config* に作成されます。</span><span class="sxs-lookup"><span data-stu-id="d1897-387">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
