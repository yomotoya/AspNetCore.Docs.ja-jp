---
title: ASP.NET Core モジュール構成リファレンス
author: guardrex
description: ASP.NET Core アプリをホストするための ASP.NET Core モジュールを構成する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 32fbf2b19da2d088847279f447f9a72cedcf8085
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570179"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="38f46-103">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="38f46-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="38f46-104">著者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="38f46-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="38f46-105">このドキュメントでは、ASP.NET Core アプリをホストするための ASP.NET Core モジュールの構成方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="38f46-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="38f46-106">ASP.NET Core モジュールの概要とインストールの説明については、「[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="38f46-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="38f46-107">ホスティング モデル</span><span class="sxs-lookup"><span data-stu-id="38f46-107">Hosting model</span></span>

<span data-ttu-id="38f46-108">.NET Core 2.2 以降で実行されるアプリの場合、モジュールでは、インプロセス ホスティング モデルがサポートされています。このモデルを使用するとリバース プロキシ (アウト ホスティング) と比較してパフォーマンスが改善されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="38f46-109">詳細については、「<xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="38f46-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="38f46-110">インプロセス ホスティングは既存のアプリではオプトインになっていますが、[dotnet new](/dotnet/core/tools/dotnet-new) テンプレートは既定ではすべての IIS および IIS Express シナリオにおいてインプロセス ホスティング モデルに設定されています。</span><span class="sxs-lookup"><span data-stu-id="38f46-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="38f46-111">アプリに対してインプロセス ホスティングを構成するには、そのアプリのプロジェクト ファイルに `<AspNetCoreHostingModel>` プロパティ (*MyApp.csproj* など) を追加して、値を `inprocess` に設定します。(アウト プロセス ホスティングは `outofprocess` を使用して設定します)。</span><span class="sxs-lookup"><span data-stu-id="38f46-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="38f46-112">インプロセスでホストする場合は、次の特性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="38f46-113">[Kestrel サーバー](xref:fundamentals/servers/kestrel)は使用されません。</span><span class="sxs-lookup"><span data-stu-id="38f46-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="38f46-114">カスタム <xref:Microsoft.AspNetCore.Hosting.Server.IServer> 実装である `IISHttpServer` は、アプリのサーバーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="38f46-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="38f46-115">[requestTimeout 属性](#attributes-of-the-aspnetcore-element) は、インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="38f46-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="38f46-116">アプリ プールをアプリ間で共有することはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="38f46-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="38f46-117">アプリごとに 1 つのアプリ プールを使用します。</span><span class="sxs-lookup"><span data-stu-id="38f46-117">Use one app pool per app.</span></span>

* <span data-ttu-id="38f46-118">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用したとき、または [app_offline.htm ファイルを手動で配置内に置いた](xref:host-and-deploy/iis/index#locked-deployment-files)ときには、開いている接続があると、アプリがすぐにシャットダウンされない場合があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="38f46-119">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="38f46-120">アプリとインストールされているランタイム (x64 または x86) のアーキテクチャ (ビット) は、アプリ プールのアーキテクチャと一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="38f46-121">([CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) ではなく) `WebHostBuilder` を使用してアプリのホストを手動で設定するときに、アプリがこれまで Kestrel サーバー上で直接実行されていた場合 (セルフホステッド)、`UseKestrel` を呼び出してから `UseIISIntegration` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="38f46-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="38f46-122">順序を逆にすると、ホストは開始に失敗します。</span><span class="sxs-lookup"><span data-stu-id="38f46-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="38f46-123">クライアントの切断が検出されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-123">Client disconnects are detected.</span></span> <span data-ttu-id="38f46-124">クライアントが切断されると、[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) キャンセル トークンが取り消されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="38f46-125">`Directory.GetCurrentDirectory()` はアプリケーション ディレクトリではなく IIS によって開始するプロセスのワーカー ディレクトリを返します (*w3wp.exe* に対して *C:\Windows\System32\inetsrv*など)。</span><span class="sxs-lookup"><span data-stu-id="38f46-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="38f46-126">ホスティング モデルの変更</span><span class="sxs-lookup"><span data-stu-id="38f46-126">Hosting model changes</span></span>

<span data-ttu-id="38f46-127">`hostingModel` 設定が *web.config* ファイル内で変更された場合 (「[web.config での構成](#configuration-with-webconfig)」セクションを参照)、モジュールによって IIS 用のワーカー プロセスがリサイクルされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="38f46-128">IIS Express の場合、モジュールによってワーカー プロセスのリサイクルは行われませんが、代わりに、現在の IIS Express プロセスの正常なシャットダウンがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="38f46-129">アプリに対して次の要求が出されると、IIS Express の新しいプロセスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="38f46-130">プロセス名</span><span class="sxs-lookup"><span data-stu-id="38f46-130">Process name</span></span>

<span data-ttu-id="38f46-131">`Process.GetCurrentProcess().ProcessName` から、`w3wp`/`iisexpress` (インプロセス) または `dotnet` (アウト プロセス) がレポートされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="38f46-132">web.config での構成</span><span class="sxs-lookup"><span data-stu-id="38f46-132">Configuration with web.config</span></span>

<span data-ttu-id="38f46-133">ASP.NET Core モジュールは、サイトの *web.config* ファイルの `system.webServer` ノードの `aspNetCore` セクションを使って構成します。</span><span class="sxs-lookup"><span data-stu-id="38f46-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="38f46-134">次に示す *web.config* ファイルは、[フレームワークに依存する展開](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)用に発行されたもので、サイトの要求を処理するように ASP.NET Core モジュールを構成します。</span><span class="sxs-lookup"><span data-stu-id="38f46-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="38f46-135">次の *web.config* は、[自己完結型の展開](/dotnet/articles/core/deploying/#self-contained-deployments-scd)用に発行されたものです。</span><span class="sxs-lookup"><span data-stu-id="38f46-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="38f46-136"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> プロパティは、`false` に設定されます。これは、[\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 要素内で指定された設定が、アプリのサブディレクトリにあるアプリによって継承されないことを示します。</span><span class="sxs-lookup"><span data-stu-id="38f46-136">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="38f46-137">アプリが [Azure App Service](https://azure.microsoft.com/services/app-service/) に対して展開されると、`stdoutLogFile` パスは `\\?\%home%\LogFiles\stdout` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-137">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="38f46-138">パスは stdout ログを *LogFiles* フォルダーに保存します。これは、サービスによって自動的に作成される場所です。</span><span class="sxs-lookup"><span data-stu-id="38f46-138">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="38f46-139">サブアプリでの *web.config* ファイルの構成に関する重要な注意事項については、「[サブアプリケーション構成](xref:host-and-deploy/iis/index#sub-application-configuration)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="38f46-139">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="38f46-140">aspNetCore 要素の属性</span><span class="sxs-lookup"><span data-stu-id="38f46-140">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="38f46-141">属性</span><span class="sxs-lookup"><span data-stu-id="38f46-141">Attribute</span></span> | <span data-ttu-id="38f46-142">説明</span><span class="sxs-lookup"><span data-stu-id="38f46-142">Description</span></span> | <span data-ttu-id="38f46-143">既定値</span><span class="sxs-lookup"><span data-stu-id="38f46-143">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="38f46-144">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-144">Optional string attribute.</span></span></p><p><span data-ttu-id="38f46-145">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="38f46-145">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="38f46-146">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-146">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-147">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-147">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="38f46-148">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-148">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-149">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-149">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="38f46-150">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="38f46-150">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="38f46-151">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-151">Optional string attribute.</span></span></p><p><span data-ttu-id="38f46-152">ホスティング モデルをインプロセス (`inprocess`) またはアウト プロセス (`outofprocess`) として指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-152">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="38f46-153">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-153">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-154">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-154">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="38f46-155">&dagger;インプロセス ホスティングの場合、値は `1` に制限されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-155">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="38f46-156">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="38f46-156">Default: `1`</span></span><br><span data-ttu-id="38f46-157">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="38f46-157">Min: `1`</span></span><br><span data-ttu-id="38f46-158">最大値: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="38f46-158">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="38f46-159">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-159">Required string attribute.</span></span></p><p><span data-ttu-id="38f46-160">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="38f46-160">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="38f46-161">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="38f46-161">Relative paths are supported.</span></span> <span data-ttu-id="38f46-162">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-162">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="38f46-163">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-163">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-164">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-164">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="38f46-165">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="38f46-165">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="38f46-166">インプロセス ホスティングではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="38f46-166">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="38f46-167">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="38f46-167">Default: `10`</span></span><br><span data-ttu-id="38f46-168">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-168">Min: `0`</span></span><br><span data-ttu-id="38f46-169">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="38f46-169">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="38f46-170">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-170">Optional timespan attribute.</span></span></p><p><span data-ttu-id="38f46-171">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-171">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="38f46-172">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-172">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="38f46-173">インプロセス ホスティングには適用されません。</span><span class="sxs-lookup"><span data-stu-id="38f46-173">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="38f46-174">インプロセス ホスティングの場合、アプリによって要求が処理されるまでモジュールは待機します。</span><span class="sxs-lookup"><span data-stu-id="38f46-174">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="38f46-175">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-175">Default: `00:02:00`</span></span><br><span data-ttu-id="38f46-176">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-176">Min: `00:00:00`</span></span><br><span data-ttu-id="38f46-177">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-177">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="38f46-178">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-178">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-179">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="38f46-179">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="38f46-180">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="38f46-180">Default: `10`</span></span><br><span data-ttu-id="38f46-181">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-181">Min: `0`</span></span><br><span data-ttu-id="38f46-182">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="38f46-182">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="38f46-183">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-183">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-184">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="38f46-184">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="38f46-185">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="38f46-185">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="38f46-186">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="38f46-186">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="38f46-187">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="38f46-187">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="38f46-188">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="38f46-188">Default: `120`</span></span><br><span data-ttu-id="38f46-189">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-189">Min: `0`</span></span><br><span data-ttu-id="38f46-190">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="38f46-190">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="38f46-191">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-191">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-192">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-192">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="38f46-193">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-193">Optional string attribute.</span></span></p><p><span data-ttu-id="38f46-194">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-194">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="38f46-195">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="38f46-195">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="38f46-196">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="38f46-196">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="38f46-197">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-197">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="38f46-198">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-198">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="38f46-199">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-199">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="38f46-200">属性</span><span class="sxs-lookup"><span data-stu-id="38f46-200">Attribute</span></span> | <span data-ttu-id="38f46-201">説明</span><span class="sxs-lookup"><span data-stu-id="38f46-201">Description</span></span> | <span data-ttu-id="38f46-202">既定値</span><span class="sxs-lookup"><span data-stu-id="38f46-202">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="38f46-203">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-203">Optional string attribute.</span></span></p><p><span data-ttu-id="38f46-204">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="38f46-204">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="38f46-205">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-206">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-206">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="38f46-207">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-207">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-208">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-208">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="38f46-209">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="38f46-209">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="38f46-210">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-211">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-211">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="38f46-212">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="38f46-212">Default: `1`</span></span><br><span data-ttu-id="38f46-213">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="38f46-213">Min: `1`</span></span><br><span data-ttu-id="38f46-214">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="38f46-214">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="38f46-215">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-215">Required string attribute.</span></span></p><p><span data-ttu-id="38f46-216">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="38f46-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="38f46-217">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="38f46-217">Relative paths are supported.</span></span> <span data-ttu-id="38f46-218">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="38f46-219">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-220">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="38f46-221">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="38f46-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="38f46-222">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="38f46-222">Default: `10`</span></span><br><span data-ttu-id="38f46-223">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-223">Min: `0`</span></span><br><span data-ttu-id="38f46-224">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="38f46-224">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="38f46-225">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-225">Optional timespan attribute.</span></span></p><p><span data-ttu-id="38f46-226">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-226">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="38f46-227">ASP.NET Core 2.1 以降のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は時間、分、および秒単位で指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-227">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="38f46-228">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-228">Default: `00:02:00`</span></span><br><span data-ttu-id="38f46-229">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-229">Min: `00:00:00`</span></span><br><span data-ttu-id="38f46-230">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-230">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="38f46-231">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-231">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-232">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="38f46-232">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="38f46-233">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="38f46-233">Default: `10`</span></span><br><span data-ttu-id="38f46-234">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-234">Min: `0`</span></span><br><span data-ttu-id="38f46-235">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="38f46-235">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="38f46-236">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-236">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-237">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="38f46-237">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="38f46-238">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="38f46-238">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="38f46-239">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="38f46-239">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="38f46-240">値 0 (ゼロ) は無限のタイムアウトと見なされ**ません**。</span><span class="sxs-lookup"><span data-stu-id="38f46-240">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="38f46-241">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="38f46-241">Default: `120`</span></span><br><span data-ttu-id="38f46-242">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-242">Min: `0`</span></span><br><span data-ttu-id="38f46-243">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="38f46-243">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="38f46-244">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-244">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-245">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-245">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="38f46-246">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-246">Optional string attribute.</span></span></p><p><span data-ttu-id="38f46-247">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-247">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="38f46-248">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="38f46-248">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="38f46-249">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="38f46-249">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="38f46-250">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-250">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="38f46-251">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-251">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="38f46-252">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-252">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="38f46-253">属性</span><span class="sxs-lookup"><span data-stu-id="38f46-253">Attribute</span></span> | <span data-ttu-id="38f46-254">説明</span><span class="sxs-lookup"><span data-stu-id="38f46-254">Description</span></span> | <span data-ttu-id="38f46-255">既定値</span><span class="sxs-lookup"><span data-stu-id="38f46-255">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="38f46-256">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-256">Optional string attribute.</span></span></p><p><span data-ttu-id="38f46-257">**processPath** において指定されている実行可能ファイルへの引数です。</span><span class="sxs-lookup"><span data-stu-id="38f46-257">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="38f46-258">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-259">true の場合、**502.5 - 処理エラー** ページは抑制され、*web.config* で構成されている 502 状態コード ページが優先されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-259">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="38f46-260">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-260">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-261">true の場合、トークンは、%ASPNETCORE_PORT% でリッスンしている子プロセスに、要求ごとの 'MS-ASPNETCORE-WINAUTHTOKEN' ヘッダーとして転送されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-261">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="38f46-262">要求ごとのこのトークンで CloseHandle を呼び出すのは、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="38f46-262">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="38f46-263">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-264">アプリごとにスピンアップすることができる **processPath** 設定内で指定したプロセスのインスタンス数が指定されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-264">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="38f46-265">既定値: `1`</span><span class="sxs-lookup"><span data-stu-id="38f46-265">Default: `1`</span></span><br><span data-ttu-id="38f46-266">最小値: `1`</span><span class="sxs-lookup"><span data-stu-id="38f46-266">Min: `1`</span></span><br><span data-ttu-id="38f46-267">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="38f46-267">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="38f46-268">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-268">Required string attribute.</span></span></p><p><span data-ttu-id="38f46-269">HTTP 要求をリッスンするプロセスを起動する実行可能ファイルへのパスです。</span><span class="sxs-lookup"><span data-stu-id="38f46-269">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="38f46-270">相対パスがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="38f46-270">Relative paths are supported.</span></span> <span data-ttu-id="38f46-271">パスが `.` で始まる場合、パスはサイトのルートを基準とする相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-271">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="38f46-272">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-272">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-273">**processPath** で指定されているプロセスが 1 分間にクラッシュできる回数を指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-273">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="38f46-274">この制限を超えた場合、モジュールは、1 分間の残りの間、プロセスの起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="38f46-274">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="38f46-275">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="38f46-275">Default: `10`</span></span><br><span data-ttu-id="38f46-276">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-276">Min: `0`</span></span><br><span data-ttu-id="38f46-277">最大値: `100`</span><span class="sxs-lookup"><span data-stu-id="38f46-277">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="38f46-278">省略可能な期間属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-278">Optional timespan attribute.</span></span></p><p><span data-ttu-id="38f46-279">%ASPNETCORE_PORT% でリッスンしているプロセスからの応答を ASP.NET Core モジュールが待機する期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-279">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="38f46-280">ASP.NET Core 2.0 以前のリリースに付属する ASP.NET Core モジュールのバージョンでは、`requestTimeout` は整数でのみ指定する必要があります。そうしないと、既定値の 2 分に設定されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-280">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="38f46-281">既定値: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-281">Default: `00:02:00`</span></span><br><span data-ttu-id="38f46-282">最小値: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-282">Min: `00:00:00`</span></span><br><span data-ttu-id="38f46-283">最大値: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="38f46-283">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="38f46-284">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-284">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-285">*app_offline.htm* ファイルが検出されたときに、モジュールが実行可能ファイルの正常なシャットダウンを待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="38f46-285">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="38f46-286">既定値: `10`</span><span class="sxs-lookup"><span data-stu-id="38f46-286">Default: `10`</span></span><br><span data-ttu-id="38f46-287">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-287">Min: `0`</span></span><br><span data-ttu-id="38f46-288">最大値: `600`</span><span class="sxs-lookup"><span data-stu-id="38f46-288">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="38f46-289">省略可能な整数属性</span><span class="sxs-lookup"><span data-stu-id="38f46-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="38f46-290">ポートでリッスンするプロセスを実行可能ファイルが開始するのをモジュールが待機する秒単位の期間です。</span><span class="sxs-lookup"><span data-stu-id="38f46-290">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="38f46-291">この制限時間を超えた場合、モジュールはプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="38f46-291">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="38f46-292">最後のローリング分においてアプリが **rapidFailsPerMinute** 回だけ開始を失敗しているのでないかぎり、モジュールは、新しい要求を受け取るとプロセスの再起動を試み、それ以降に受信した要求に対してプロセスの再起動を試み続けます。</span><span class="sxs-lookup"><span data-stu-id="38f46-292">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="38f46-293">既定値: `120`</span><span class="sxs-lookup"><span data-stu-id="38f46-293">Default: `120`</span></span><br><span data-ttu-id="38f46-294">最小値: `0`</span><span class="sxs-lookup"><span data-stu-id="38f46-294">Min: `0`</span></span><br><span data-ttu-id="38f46-295">最大値: `3600`</span><span class="sxs-lookup"><span data-stu-id="38f46-295">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="38f46-296">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="38f46-296">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="38f46-297">true の場合、**processPath** で指定されているプロセスの **stdout** と **stderr** は、**stdoutLogFile** で指定されているファイルにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-297">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="38f46-298">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="38f46-298">Optional string attribute.</span></span></p><p><span data-ttu-id="38f46-299">**processPath** で指定されているプロセスからの **stdout** と **stderr** がログに記録される相対ファイル パスまたは絶対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-299">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="38f46-300">相対パスの基準はサイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="38f46-300">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="38f46-301">`.` で始まっているパスはすべてサイト ルートに対する相対パスであり、他のすべてのパスは絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="38f46-301">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="38f46-302">モジュールがログ ファイルを作成するためには、パスで指定されているすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-302">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="38f46-303">アンダースコアの区切り記号を使って、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) が、**stdoutLogFile** パスの最後のセグメントに追加されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-303">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="38f46-304">たとえば、値として `.\logs\stdout` を指定し、2018 年 2 月 5 日の 19:41:32 にプロセス ID 1934 で保存すると、stdout ログは *logs* フォルダーに *stdout_20180205194132_1934.log* として保存されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-304">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="38f46-305">環境変数の設定</span><span class="sxs-lookup"><span data-stu-id="38f46-305">Setting environment variables</span></span>

<span data-ttu-id="38f46-306">`processPath` 属性で、プロセスに対する環境変数を指定できます。</span><span class="sxs-lookup"><span data-stu-id="38f46-306">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="38f46-307">`environmentVariables` コレクション要素の `environmentVariable` 子要素で、環境変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="38f46-307">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="38f46-308">このセクションで設定された環境変数は、システム環境変数より優先されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-308">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="38f46-309">次の例では、2 つの環境変数を設定しています。</span><span class="sxs-lookup"><span data-stu-id="38f46-309">The following example sets two environment variables.</span></span> <span data-ttu-id="38f46-310">`ASPNETCORE_ENVIRONMENT` は、`Development` に対するアプリの環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="38f46-310">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="38f46-311">開発者は、アプリの例外をデバッグするときに[開発者例外ページ](xref:fundamentals/error-handling)を強制的に読み込むため、*web.config* ファイルでこの値を一時的に設定できます。</span><span class="sxs-lookup"><span data-stu-id="38f46-311">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="38f46-312">`CONFIG_DIR` はユーザー定義の環境変数の例です。開発者はここに、アプリの構成ファイルを読み込むためのパスを形成するために起動時に値を読み取るコードを記述してあります。</span><span class="sxs-lookup"><span data-stu-id="38f46-312">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="38f46-313">インターネットなどの信頼されていないネットワークにアクセスできないステージング サーバーやテスト サーバーでは、`ASPNETCORE_ENVIRONMENT` 環境変数を `Development` に設定するだけです。</span><span class="sxs-lookup"><span data-stu-id="38f46-313">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="38f46-314">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="38f46-314">app_offline.htm</span></span>

<span data-ttu-id="38f46-315">*app_offline.htm* という名前のファイルがアプリのルート ディレクトリで検出された場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、受信要求の処理を停止することを試みます。</span><span class="sxs-lookup"><span data-stu-id="38f46-315">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="38f46-316">`shutdownTimeLimit` で定義されている秒数が経過してもまだアプリが実行している場合、ASP.NET Core モジュールは実行中のプロセスを強制終了します。</span><span class="sxs-lookup"><span data-stu-id="38f46-316">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="38f46-317">*app_offline.htm* ファイルが存在している間、ASP.NET Core モジュールは、*app_offline.htm* ファイルの内容を返送することで、要求に応答します。</span><span class="sxs-lookup"><span data-stu-id="38f46-317">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="38f46-318">*app_offline.htm* ファイルが削除されると、次の要求によってアプリが起動されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-318">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="38f46-319">アウト プロセス ホスティング モデルを使用するときは、開いている接続があると、アプリがすぐにシャットダウンされない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-319">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="38f46-320">たとえば、WebSocket 接続では、アプリのシャットダウンが遅れる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-320">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="38f46-321">起動エラー ページ</span><span class="sxs-lookup"><span data-stu-id="38f46-321">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="38f46-322">インプロセス ホスティングでもアウト プロセス ホスティングでも、アプリの起動に失敗すると、カスタム エラー ページが生成されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-322">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="38f46-323">ASP.NET Core モジュールが、インプロセスまたはアウト プロセスのどちらかの要求ハンドラーの検索に失敗した場合、*500.0 - インプロセス/アウト プロセス ハンドラーの読み込みエラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-323">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="38f46-324">インプロセス ホスティングで、ASP.NET Core モジュールによるアプリの起動が失敗すると、*500.30 - 開始エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-324">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="38f46-325">アウト プロセス ホスティングで、ASP.NET Core モジュールがバックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-325">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="38f46-326">このページを抑制して、既定の IIS 5xx 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。</span><span class="sxs-lookup"><span data-stu-id="38f46-326">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="38f46-327">カスタム エラー メッセージの構成方法について詳しくは、「[HTTP エラー \<httpErrors](/iis/configuration/system.webServer/httpErrors/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="38f46-327">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="38f46-328">ASP.NET Core モジュールが、バックエンド プロセスの起動に失敗した場合、またはバックエンド プロセスは開始しても構成されているポートでのリッスンに失敗した場合は、*502.5 処理エラー*状態コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-328">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="38f46-329">このページを抑制して、既定の IIS 502 状態コード ページに戻すには、`disableStartUpErrorPage` 属性を使います。</span><span class="sxs-lookup"><span data-stu-id="38f46-329">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="38f46-330">カスタム エラー メッセージの構成方法について詳しくは、「[HTTP エラー \<httpErrors](/iis/configuration/system.webServer/httpErrors/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="38f46-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 処理エラーの状態コード ページ](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="38f46-332">ログの作成とリダイレクト</span><span class="sxs-lookup"><span data-stu-id="38f46-332">Log creation and redirection</span></span>

<span data-ttu-id="38f46-333">`aspNetCore` 要素の `stdoutLogEnabled` 属性および `stdoutLogFile` 属性が設定されている場合は、stdout および stderr コンソール出力が ASP.NET Core モジュールによってディスクにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="38f46-333">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="38f46-334">モジュールがログ ファイルを作成するためには、`stdoutLogFile` パスのすべてのフォルダーが存在している必要があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-334">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="38f46-335">アプリ プールは、ログが書き込まれる場所への書き込みアクセス権を持っている必要があります (書き込みアクセス許可を提供するには、`IIS AppPool\<app_pool_name>` を使います)。</span><span class="sxs-lookup"><span data-stu-id="38f46-335">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="38f46-336">プロセスのリサイクル/再起動が発生しない場合、ログは循環されません。</span><span class="sxs-lookup"><span data-stu-id="38f46-336">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="38f46-337">ログが使用するディスク領域を制限するのは、ホストの役割です。</span><span class="sxs-lookup"><span data-stu-id="38f46-337">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="38f46-338">stdout ログの使用は、アプリ起動時の問題をトラブルシューティングする場合にのみ推奨されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-338">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="38f46-339">一般的なアプリ ログの目的には、stdout ログを使わないでください。</span><span class="sxs-lookup"><span data-stu-id="38f46-339">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="38f46-340">ASP.NET Core アプリでのルーチン ログの場合は、ログ ファイルのサイズを制限し、ログをローテーションするログ ライブラリを使います。</span><span class="sxs-lookup"><span data-stu-id="38f46-340">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="38f46-341">詳しくは、「[サードパーティ製のログ プロバイダー](xref:fundamentals/logging/index#third-party-logging-providers)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="38f46-341">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="38f46-342">ログ ファイルの作成時には、タイムスタンプとファイルの拡張子が自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-342">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="38f46-343">ログ ファイル名は、タイムスタンプ、プロセス ID、およびファイル拡張子 (*.log*) を `stdoutLogFile` パスの最後のセグメント (通常は *stdout*) にアンダースコアで区切って追加することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-343">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="38f46-344">`stdoutLogFile` パスが *stdout* で終わっている場合、PID が 1934 で 2018 年 2 月 5 日の 19:42:32 に作成されたアプリのログのファイル名は、*stdout_20180205194132_1934.log* になります。</span><span class="sxs-lookup"><span data-stu-id="38f46-344">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="38f46-345">`stdoutLogEnabled` が false の場合は、アプリの起動時に発生するエラーがキャプチャされ、30 KB までイベント ログに出力されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-345">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="38f46-346">起動後、すべての追加のログが破棄されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-346">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="38f46-347">次の `aspNetCore` 要素の例では、Azure App Service でホストされているアプリの stdout ログを構成しています。</span><span class="sxs-lookup"><span data-stu-id="38f46-347">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="38f46-348">ローカル ログには、ローカル パスまたはネットワーク共有パスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="38f46-348">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="38f46-349">AppPool のユーザー ID に、指定されたパスへの書き込みアクセス許可があることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="38f46-349">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="38f46-350">強化された診断ログ</span><span class="sxs-lookup"><span data-stu-id="38f46-350">Enhanced diagnostic logs</span></span>

<span data-ttu-id="38f46-351">ASP.NET Core モジュールは、強化された診断ログを提供するよう構成できます。</span><span class="sxs-lookup"><span data-stu-id="38f46-351">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="38f46-352">*web.config* で、`<aspNetCore>` 要素に `<handlerSettings>` 要素を追加します。`debugLevel` を `TRACE` に設定すると、診断情報が再現性の高いものになります。</span><span class="sxs-lookup"><span data-stu-id="38f46-352">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="38f46-353">デバッグ レベル (`debugLevel`) 値には、レベルと場所の両方を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="38f46-353">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="38f46-354">レベルは次のとおりです (情報量が少ないものから多いものへの順)。</span><span class="sxs-lookup"><span data-stu-id="38f46-354">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="38f46-355">ERROR</span><span class="sxs-lookup"><span data-stu-id="38f46-355">ERROR</span></span>
* <span data-ttu-id="38f46-356">WARNING</span><span class="sxs-lookup"><span data-stu-id="38f46-356">WARNING</span></span>
* <span data-ttu-id="38f46-357">INFO</span><span class="sxs-lookup"><span data-stu-id="38f46-357">INFO</span></span>
* <span data-ttu-id="38f46-358">TRACE</span><span class="sxs-lookup"><span data-stu-id="38f46-358">TRACE</span></span>

<span data-ttu-id="38f46-359">場所は次のとおりです (複数の場所を指定できます)。</span><span class="sxs-lookup"><span data-stu-id="38f46-359">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="38f46-360">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="38f46-360">CONSOLE</span></span>
* <span data-ttu-id="38f46-361">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="38f46-361">EVENTLOG</span></span>
* <span data-ttu-id="38f46-362">ファイル</span><span class="sxs-lookup"><span data-stu-id="38f46-362">FILE</span></span>

<span data-ttu-id="38f46-363">ハンドラー設定は、次の環境変数を使用して指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="38f46-363">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="38f46-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; デバッグ ログ ファイルへのパス </span><span class="sxs-lookup"><span data-stu-id="38f46-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="38f46-365">(既定: *aspnetcore debug.log*)。</span><span class="sxs-lookup"><span data-stu-id="38f46-365">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="38f46-366">`ASPNETCORE_MODULE_DEBUG` &ndash; デバッグ レベルの設定。</span><span class="sxs-lookup"><span data-stu-id="38f46-366">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="38f46-367">配置内でデバッグ ログを、問題のトラブルシューティングに必要な時間よりも長く有効のままに**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="38f46-367">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="38f46-368">ログのサイズは制限されていません。</span><span class="sxs-lookup"><span data-stu-id="38f46-368">The size of the log isn't limited.</span></span> <span data-ttu-id="38f46-369">デバッグ ログを有効のままにすると、使用可能なディスク領域が使い果たされ、サーバーまたはアプリ サービスがクラッシュする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="38f46-369">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="38f46-370">*web.config* ファイルでの `aspNetCore` 要素の例については、「[web.config での構成](#configuration-with-webconfig)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="38f46-370">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="38f46-371">プロキシの構成で HTTP プロトコルとペアリング トークンを使用する</span><span class="sxs-lookup"><span data-stu-id="38f46-371">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="38f46-372">*アウト プロセス ホスティングにのみ適用されます。*</span><span class="sxs-lookup"><span data-stu-id="38f46-372">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="38f46-373">ASP.NET Core モジュールと Kestrel の間に作成されるプロキシは、HTTP プロトコルを使います。</span><span class="sxs-lookup"><span data-stu-id="38f46-373">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="38f46-374">HTTP を使うと、パフォーマンスが最適化されます。この場合、モジュールと Kestrel の間のトラフィックは、ネットワーク インターフェイスから切り離されたループバック アドレスで発生します。</span><span class="sxs-lookup"><span data-stu-id="38f46-374">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="38f46-375">モジュールと Kestrel の間のトラフィックが、サーバーから離れた場所で傍受される危険はありません。</span><span class="sxs-lookup"><span data-stu-id="38f46-375">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="38f46-376">ペアリング トークンを使用すると、Kestrel によって受信される要求が IIS によってプロキシされたものであり、他のソースからのものでないことを保証できます。</span><span class="sxs-lookup"><span data-stu-id="38f46-376">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="38f46-377">ペアリング トークンは、モジュールによって作成されて、環境変数 (`ASPNETCORE_TOKEN`) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-377">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="38f46-378">ペアリング トークンはまた、プロキシされたすべての要求のヘッダー (`MSAspNetCoreToken`) にも設定されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-378">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="38f46-379">IIS ミドルウェアは、受信した各要求をチェックし、ペアリング トークン ヘッダーの値が環境変数の値と一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="38f46-379">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="38f46-380">トークンの値が一致しない場合、要求はログに記録され、拒否されます。</span><span class="sxs-lookup"><span data-stu-id="38f46-380">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="38f46-381">ペアリング トークン環境変数およびモジュールと Kestrel の間のトラフィックには、サーバーから離れた場所からアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="38f46-381">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="38f46-382">ペアリング トークンの値がわからなければ、攻撃者は IIS ミドルウェアのチェックをバイパスする要求を送信できません。</span><span class="sxs-lookup"><span data-stu-id="38f46-382">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="38f46-383">IIS 共有構成での ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="38f46-383">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="38f46-384">ASP.NET Core モジュールのインストーラーは、**SYSTEM** アカウントのアクセス許可を使って実行します。</span><span class="sxs-lookup"><span data-stu-id="38f46-384">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="38f46-385">ローカル システム アカウントには、IIS 共有構成によって使われる共有パスに対する変更アクセス許可がないため、インストーラーが共有上の *applicationHost.config* 内のモジュール設定を構成しようとすると、アクセス拒否エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="38f46-385">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="38f46-386">IIS 共有構成を使うときは、次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="38f46-386">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="38f46-387">IIS 共有構成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="38f46-387">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="38f46-388">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="38f46-388">Run the installer.</span></span>
1. <span data-ttu-id="38f46-389">更新された *applicationHost.config* ファイルを共有にエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="38f46-389">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="38f46-390">IIS 共有構成を再び有効にします。</span><span class="sxs-lookup"><span data-stu-id="38f46-390">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="38f46-391">モジュールのバージョンとホスティング バンドル インストーラーのログ</span><span class="sxs-lookup"><span data-stu-id="38f46-391">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="38f46-392">インストールされている ASP.NET Core モジュールのバージョンを確認するには:</span><span class="sxs-lookup"><span data-stu-id="38f46-392">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="38f46-393">ホスティング システム上で、*%windir%\System32\inetsrv* に移動します。</span><span class="sxs-lookup"><span data-stu-id="38f46-393">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="38f46-394">*aspnetcore.dll* ファイルを探します。</span><span class="sxs-lookup"><span data-stu-id="38f46-394">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="38f46-395">ファイルを右クリックし、コンテキスト メニューの **[プロパティ]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="38f46-395">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="38f46-396">**[詳細]** タブを選びます。**[ファイル バージョン]** と **[製品バージョン]** が、インストールされているモジュールのバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="38f46-396">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="38f46-397">モジュールのホスティング バンドル インストーラーのログは、*C:\\Users\\%UserName%\\AppData\\Local\\Temp* にあります。ファイルの名前は、*dd_DotNetCoreWinSvrHosting__\<タイムスタンプ>_000_AspNetCoreModule_x64.log* です。</span><span class="sxs-lookup"><span data-stu-id="38f46-397">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="38f46-398">モジュール、スキーマ、構成ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="38f46-398">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="38f46-399">Module</span><span class="sxs-lookup"><span data-stu-id="38f46-399">Module</span></span>

<span data-ttu-id="38f46-400">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="38f46-400">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="38f46-401">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-401">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="38f46-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="38f46-403">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-403">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="38f46-404">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-404">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="38f46-405">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="38f46-405">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="38f46-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="38f46-407">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-407">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="38f46-408">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-408">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="38f46-409">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="38f46-409">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="38f46-410">Schema</span><span class="sxs-lookup"><span data-stu-id="38f46-410">Schema</span></span>

<span data-ttu-id="38f46-411">**IIS**</span><span class="sxs-lookup"><span data-stu-id="38f46-411">**IIS**</span></span>

   * <span data-ttu-id="38f46-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="38f46-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="38f46-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="38f46-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="38f46-414">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="38f46-414">**IIS Express**</span></span>

   * <span data-ttu-id="38f46-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="38f46-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="38f46-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="38f46-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="38f46-417">構成</span><span class="sxs-lookup"><span data-stu-id="38f46-417">Configuration</span></span>

<span data-ttu-id="38f46-418">**IIS**</span><span class="sxs-lookup"><span data-stu-id="38f46-418">**IIS**</span></span>

   * <span data-ttu-id="38f46-419">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="38f46-419">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="38f46-420">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="38f46-420">**IIS Express**</span></span>

   * <span data-ttu-id="38f46-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="38f46-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="38f46-422">ファイルは、*applicationHost.config* ファイルで *aspnetcore* を検索することにより見つかります。</span><span class="sxs-lookup"><span data-stu-id="38f46-422">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
