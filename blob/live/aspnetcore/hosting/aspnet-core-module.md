---
title: "ASP.NET Core モジュール構成の参照"
author: guardrex
description: "ASP.NET Core アプリケーションをホストする ASP.NET Core モジュールを構成する方法。"
keywords: "ASP.NET Core、ancm コア モジュール、iis、stdout のログ記録、環境変数、環境変数、subapplication、subapp、appoffline app_offline、502、スキーマ"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: 277e63a5663aca622e8252d6c6be1671e57cbf68
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/21/2017
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="0e6f7-104">ASP.NET Core モジュール構成の参照</span><span class="sxs-lookup"><span data-stu-id="0e6f7-104">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="0e6f7-105">によって[Luke Latham](https://github.com/guardrex)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、および[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="0e6f7-105">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="0e6f7-106">このドキュメントでは、ASP.NET Core アプリケーションをホストする ASP.NET Core モジュールを構成する方法の詳細を説明します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-106">This document provides details on how to configure the ASP.NET Core Module for hosting ASP.NET Core applications.</span></span> <span data-ttu-id="0e6f7-107">ASP.NET Core モジュールとインストール手順の概要については、次を参照してください。、 [ASP.NET コア モジュールの概要](xref:fundamentals/servers/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-107">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-via-webconfig"></a><span data-ttu-id="0e6f7-108">Web.config を使用して構成</span><span class="sxs-lookup"><span data-stu-id="0e6f7-108">Configuration via web.config</span></span>

<span data-ttu-id="0e6f7-109">ASP.NET Core モジュールは、サイトまたはアプリケーションを介して構成*web.config*ファイルし、それ自体が`aspNetCore`構成セクション内で`system.webServer`です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-109">The ASP.NET Core Module is configured via a site or application *web.config* file and has its own `aspNetCore` configuration section within `system.webServer`.</span></span> <span data-ttu-id="0e6f7-110">次に例を示します*web.config*ファイルを`Microsoft.NET.Sdk.Web`SDK は、プロジェクトが発行された場合、[フレームワークに依存する展開](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd)プレース ホルダーの付いた、`processPath`と`arguments`:</span><span class="sxs-lookup"><span data-stu-id="0e6f7-110">Here's an example *web.config* file that the `Microsoft.NET.Sdk.Web` SDK will provide when the project is published for a [framework-dependent deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) with placeholders for the `processPath` and `arguments`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="0e6f7-111">*Web.config*次の例は、[自己完結型の配置](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd)を[Azure App Service](https://azure.microsoft.com/services/app-service/)です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-111">The *web.config* example below is for a [self-contained deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) to the [Azure App Service](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="0e6f7-112">詳細については、次を参照してください。[を IIS に発行](xref:publishing/iis)です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-112">For more information, see [Publishing to IIS](xref:publishing/iis).</span></span> <span data-ttu-id="0e6f7-113">参照してください[サブ アプリケーションの構成](xref:publishing/iis#configuration-of-sub-applications)の構成に関連する重要なメモを*web.config*サブ アプリケーション内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-113">See [Configuration of sub-applications](xref:publishing/iis#configuration-of-sub-applications) for an important note pertaining to the configuration of *web.config* files in sub-applications.</span></span>

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="0e6f7-114">AspNetCore 要素の属性</span><span class="sxs-lookup"><span data-stu-id="0e6f7-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="0e6f7-115">属性</span><span class="sxs-lookup"><span data-stu-id="0e6f7-115">Attribute</span></span> | <span data-ttu-id="0e6f7-116">説明</span><span class="sxs-lookup"><span data-stu-id="0e6f7-116">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0e6f7-117">processPath</span><span class="sxs-lookup"><span data-stu-id="0e6f7-117">processPath</span></span> | <p><span data-ttu-id="0e6f7-118">必須の文字列属性です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-118">Required string attribute.</span></span></p><p><span data-ttu-id="0e6f7-119">HTTP 要求をリッスンしているプロセスを起動する実行可能ファイルへのパス。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-119">Path to the executable that will launch a process listening for HTTP requests.</span></span> <span data-ttu-id="0e6f7-120">相対パスがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-120">Relative paths are supported.</span></span> <span data-ttu-id="0e6f7-121">パスが始まる場合 '.'、パスはサイトのルートの相対パスであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-121">If the path begins with '.', the path is considered to be relative to the site root.</span></span></p><p><span data-ttu-id="0e6f7-122">既定値はありません。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-122">There is no default value.</span></span></p> |
| <span data-ttu-id="0e6f7-123">引数</span><span class="sxs-lookup"><span data-stu-id="0e6f7-123">arguments</span></span> | <p><span data-ttu-id="0e6f7-124">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-124">Optional string attribute.</span></span></p><p><span data-ttu-id="0e6f7-125">指定された実行可能ファイルへの引数**processPath**です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-125">Arguments to the executable specified in **processPath**.</span></span></p><p><span data-ttu-id="0e6f7-126">既定値は空の文字列です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-126">The default value is an empty string.</span></span></p> |
| <span data-ttu-id="0e6f7-127">startupTimeLimit</span><span class="sxs-lookup"><span data-stu-id="0e6f7-127">startupTimeLimit</span></span> | <p><span data-ttu-id="0e6f7-128">省略可能な整数属性です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-128">Optional integer attribute.</span></span></p><p><span data-ttu-id="0e6f7-129">モジュールがポートでリッスンしているプロセスを開始する実行可能ファイルを待機する秒単位で期間です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-129">Duration in seconds that the module will wait for the executable to start a process listening on the port.</span></span> <span data-ttu-id="0e6f7-130">この制限時間を超えた場合、モジュールは、プロセスを終了します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-130">If this time limit is exceeded, the module will kill the process.</span></span> <span data-ttu-id="0e6f7-131">モジュールは、新しい要求を受信しは、アプリケーションの起動に失敗しない限り、後続の受信要求の処理を再開しようとしています引き続き、プロセスをもう一度起動しようとします。 **rapidFailsPerMinute**数。最後のローリング分の回数。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-131">The module will attempt to launch the process again when it receives a new request and will continue to attempt to restart the process on subsequent incoming requests unless the application fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="0e6f7-132">既定値は 120 です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-132">The default value is 120.</span></span></p> |
| <span data-ttu-id="0e6f7-133">shutdownTimeLimit</span><span class="sxs-lookup"><span data-stu-id="0e6f7-133">shutdownTimeLimit</span></span> | <p><span data-ttu-id="0e6f7-134">省略可能な整数属性です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-134">Optional integer attribute.</span></span></p><p><span data-ttu-id="0e6f7-135">対象のモジュールが正常にシャット ダウンする実行可能ファイルの待機秒単位で期間ときに、 *app_offline.htm*ファイルが検出されました。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-135">Duration in seconds for which the module will wait for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p><p><span data-ttu-id="0e6f7-136">既定値は 10 です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-136">The default value is 10.</span></span></p> |
| <span data-ttu-id="0e6f7-137">rapidFailsPerMinute</span><span class="sxs-lookup"><span data-stu-id="0e6f7-137">rapidFailsPerMinute</span></span> | <p><span data-ttu-id="0e6f7-138">省略可能な整数属性です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-138">Optional integer attribute.</span></span></p><p><span data-ttu-id="0e6f7-139">プロセスが指定された回数を指定**processPath**は 1 分あたりのクラッシュを許可します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-139">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="0e6f7-140">この制限を超えた場合、モジュールは、プロセスが 1 分間の残りの部分の起動を停止します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-140">If this limit is exceeded, the module will stop launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="0e6f7-141">既定値は 10 です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-141">The default value is 10.</span></span></p> |
| <span data-ttu-id="0e6f7-142">requestTimeout</span><span class="sxs-lookup"><span data-stu-id="0e6f7-142">requestTimeout</span></span> | <p><span data-ttu-id="0e6f7-143">省略可能な timespan 属性です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-143">Optional timespan attribute.</span></span></p><p><span data-ttu-id="0e6f7-144">ASP.NET Core モジュールが ASPNETCORE_PORT % でリッスンしているプロセスからの応答の待機期間を指定します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-144">Specifies the duration for which the ASP.NET Core Module will wait for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="0e6f7-145">既定値は、"00:02:00" です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-145">The default value is "00:02:00".</span></span></p><p><span data-ttu-id="0e6f7-146">`requestTimeout`必要がありますで指定する整数の分だけ、それ以外の場合、既定値は 2 分です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-146">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> |
| <span data-ttu-id="0e6f7-147">stdoutLogEnabled</span><span class="sxs-lookup"><span data-stu-id="0e6f7-147">stdoutLogEnabled</span></span> | <p><span data-ttu-id="0e6f7-148">省略可能な Boolean 属性です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-148">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0e6f7-149">True の場合、 **stdout**と**stderr**で指定されたプロセスの**processPath**で指定されたファイルにリダイレクトされます**stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="0e6f7-149">If true, **stdout** and **stderr** for the process specified in **processPath** will be redirected to the file specified in **stdoutLogFile**.</span></span></p><p><span data-ttu-id="0e6f7-150">既定値は false です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-150">The default value is false.</span></span></p> |
| <span data-ttu-id="0e6f7-151">stdoutLogFile</span><span class="sxs-lookup"><span data-stu-id="0e6f7-151">stdoutLogFile</span></span> | <p><span data-ttu-id="0e6f7-152">省略可能な文字列属性。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-152">Optional string attribute.</span></span></p><p><span data-ttu-id="0e6f7-153">対象の相対パスまたは絶対ファイル パスを指定**stdout**と**stderr**で指定されたプロセスから**processPath**ログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-153">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** will be logged.</span></span> <span data-ttu-id="0e6f7-154">相対パスは、サイトのルートです。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-154">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="0e6f7-155">以降で任意のパス '.' サイト ルートに対する相対パスおよびその他のすべてのパスが絶対パスとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-155">Any path starting with '.' will be relative to the site root and all other paths will be treated as absolute paths.</span></span> <span data-ttu-id="0e6f7-156">モジュール、ログ ファイルを作成するためにパスで提供されるすべてのフォルダーが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-156">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="0e6f7-157">プロセス ID、タイムスタンプ (*yyyyMdhms*)、ファイル拡張子と (*.log*) アンダー スコアで区切り記号が追加、最後のセグメント、 **stdoutLogFile**提供します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-157">The process ID, timestamp (*yyyyMdhms*), and file extension (*.log*) with underscore delimiters are added to the last segment of the **stdoutLogFile** provided.</span></span></p><p><span data-ttu-id="0e6f7-158">既定値は `aspnetcore-stdout` です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-158">The default value is `aspnetcore-stdout`.</span></span></p> |
| <span data-ttu-id="0e6f7-159">forwardWindowsAuthToken</span><span class="sxs-lookup"><span data-stu-id="0e6f7-159">forwardWindowsAuthToken</span></span> | <span data-ttu-id="0e6f7-160">true または false。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-160">true or false.</span></span></p><p><span data-ttu-id="0e6f7-161">True の場合、トークンが 1 回の要求ヘッダー ' MS ASPNETCORE WINAUTHTOKEN' として ASPNETCORE_PORT % でリッスンしている子プロセスに転送されます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-161">If true, the token will be forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="0e6f7-162">このトークン要求ごとに CloseHandle を呼び出すには、そのプロセスの役割です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-162">It is the responsibility of that process to call CloseHandle on this token per request.</span></span></p><p><span data-ttu-id="0e6f7-163">既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-163">The default value is true.</span></span></p> |
| <span data-ttu-id="0e6f7-164">disableStartUpErrorPage</span><span class="sxs-lookup"><span data-stu-id="0e6f7-164">disableStartUpErrorPage</span></span> | <span data-ttu-id="0e6f7-165">true または false。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-165">true or false.</span></span></p><p><span data-ttu-id="0e6f7-166">True の場合、 **502.5 - プロセス エラー**ページは表示されずで構成されている 502 ステータス コード ページ、 *web.config*が優先されます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-166">If true, the **502.5 - Process Failure** page will be suppressed, and the 502 status code page configured in your *web.config* will take precedence.</span></span></p><p><span data-ttu-id="0e6f7-167">既定値は false です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-167">The default value is false.</span></span></p> |

### <a name="setting-environment-variables"></a><span data-ttu-id="0e6f7-168">環境変数の設定</span><span class="sxs-lookup"><span data-stu-id="0e6f7-168">Setting environment variables</span></span>

<span data-ttu-id="0e6f7-169">ASP.NET Core モジュールによりで指定されたプロセスの環境変数を指定する、`processPath`属性を 1 つ以上指定することによって`environmentVariable`の子要素、`environmentVariables`コレクション要素の下、`aspNetCore`要素。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-169">The ASP.NET Core Module allows you specify environment variables for the process specified in the `processPath` attribute by specifying them in one or more `environmentVariable` child elements of an `environmentVariables` collection element under the `aspNetCore` element.</span></span> <span data-ttu-id="0e6f7-170">このセクションで設定されている環境変数よりも優先システム プロセスの環境変数。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-170">Environment variables set in this section take precedence over system environment variables for the process.</span></span>

<span data-ttu-id="0e6f7-171">次の例では、2 つの環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-171">The example below sets two environment variables.</span></span> <span data-ttu-id="0e6f7-172">`ASPNETCORE_ENVIRONMENT`アプリケーションの環境を構成`Development`です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-172">`ASPNETCORE_ENVIRONMENT` will configure the application's environment to `Development`.</span></span> <span data-ttu-id="0e6f7-173">開発者は、この値にへの設定は一時的に可能性があります、 *web.config*を強制するためにファイル、[開発者例外ページ](xref:fundamentals/error-handling)アプリ例外をデバッグするときに読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-173">A developer may temporarily set this value in the *web.config* file in order to force the [developer exception page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="0e6f7-174">`CONFIG_DIR`スタートアップ時にアプリの構成ファイルを読み込むために、パスを形成する値を読み取るコードを記述は、ユーザー定義の環境変数の例を示します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-174">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that will read the value on startup to form a path in order to load the app's configuration file.</span></span>

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

## <a name="appofflinehtm"></a><span data-ttu-id="0e6f7-175">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="0e6f7-175">app_offline.htm</span></span>

<span data-ttu-id="0e6f7-176">名前のファイルを配置する場合*app_offline.htm* web アプリケーションのディレクトリのルート ディレクトリにある ASP.NET Core モジュールは、アプリを正常にシャット ダウンしようし、着信要求の処理を停止します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-176">If you place a file with the name *app_offline.htm* at the root of a web application directory, the ASP.NET Core Module will attempt to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="0e6f7-177">後に、アプリが実行中の場合`shutdownTimeLimit`数 (秒単位) の ASP.NET Core モジュールが実行中のプロセスを終了します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-177">If the app is still running after `shutdownTimeLimit` number of seconds, the ASP.NET Core Module will kill the running process.</span></span>

<span data-ttu-id="0e6f7-178">中に、 *app_offline.htm*ファイルが存在 ASP.NET Core モジュールは要求に応答の内容を送信することによって、 *app_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-178">While the *app_offline.htm* file is present, the ASP.NET Core Module will respond to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="0e6f7-179">1 回、 *app_offline.htm*ファイルが削除され、次の要求が要求に応答し、アプリケーションを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-179">Once the *app_offline.htm* file is removed, the next request loads the application, which then responds to requests.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="0e6f7-180">スタートアップ エラー ページ</span><span class="sxs-lookup"><span data-stu-id="0e6f7-180">Start-up error page</span></span>

<span data-ttu-id="0e6f7-181">バックエンド プロセスまたはバックエンドの処理が開始されるが、構成されたポートでリッスンに失敗するを起動する ASP.NET Core モジュールが失敗した場合は、HTTP 502.5 ステータス コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-181">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, you will see an HTTP 502.5 status code page.</span></span> <span data-ttu-id="0e6f7-182">このページを抑制して、既定の IIS 502 ステータス コード ページを元に戻すを使用して、`disableStartUpErrorPage`属性。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-182">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="0e6f7-183">カスタム エラー メッセージを構成する方法については、次を参照してください。 [HTTP エラー `<httpErrors>`](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/)です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-183">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).</span></span>

![502 状態 ページ](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="0e6f7-185">ログの作成とリダイレクト</span><span class="sxs-lookup"><span data-stu-id="0e6f7-185">Log creation and redirection</span></span>

<span data-ttu-id="0e6f7-186">ASP.NET Core モジュール リダイレクト`stdout`と`stderr`を設定する場合はディスクへのログ、`stdoutLogEnabled`と`stdoutLogFile`の属性、`aspNetCore`要素。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-186">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if you set the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element.</span></span> <span data-ttu-id="0e6f7-187">すべてのフォルダ、`stdoutLogFile`パスは、モジュール、ログ ファイルを作成するために存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-187">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="0e6f7-188">タイムスタンプとファイルの拡張機能は、ログ ファイルの作成時に自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-188">A timestamp and file extension will be added automatically when the log file is created.</span></span> <span data-ttu-id="0e6f7-189">プロセスのリサイクル/再起動が発生しない限り、ログは回転しません。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-189">Logs are not rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="0e6f7-190">ホストは、ログの使用ディスク領域を制限するの役割です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-190">It is the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="0e6f7-191">使用して、`stdout`ログは、アプリケーションのスタートアップの問題のトラブルシューティングとない一般的なアプリケーションのログ記録のためにのみ推奨されます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-191">Using the `stdout` log is only recommended for troubleshooting application startup issues and not for general application logging purposes.</span></span>

<span data-ttu-id="0e6f7-192">ログ ファイルの名前は、プロセス ID (PID)、タイムスタンプを追加することによって作成されて (*yyyyMdhms*)、ファイル拡張子と (*.log*) の最後のセグメントを`stdoutLogFile`パス (通常*stdout*) アンダー スコアで区切られます。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-192">The log file name is composed by appending the process ID (PID), timestamp (*yyyyMdhms*), and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="0e6f7-193">たとえば場合、`stdoutLogFile`パスの終わりに*stdout*、12時 05分: 02 で 8/10/2017 で作成された 10652 の PID を使用してアプリのログ ファイル名である*stdout_10652_20178101252.log*です。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-193">For example if the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 10652 created on 8/10/2017 at 12:05:02 has the file name *stdout_10652_20178101252.log*.</span></span>

<span data-ttu-id="0e6f7-194">サンプルを次に示します`aspNetCore`を構成する要素`stdout`ログ記録します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-194">Here's a sample `aspNetCore` element that configures `stdout` logging.</span></span> <span data-ttu-id="0e6f7-195">`stdoutLogFile`の例に示すパスは Azure App Service に適しています。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-195">The `stdoutLogFile` path shown in the example is appropriate for the Azure App Service.</span></span> <span data-ttu-id="0e6f7-196">ローカル パスまたはネットワーク共有のパスがローカル ログもかまわないです。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-196">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="0e6f7-197">AppPool ユーザー id が指定されたパスに書き込む権限を持っていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-197">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```
<span data-ttu-id="0e6f7-198">参照してください[web.config を使用して構成](#configuration-via-webconfig)の例については、`aspNetCore`内の要素、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-198">See [Configuration via web.config](#configuration-via-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="0e6f7-199">IIS と ASP.NET Core モジュール構成を共有します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-199">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="0e6f7-200">ASP.NET Core モジュール インストーラーの実行の権限を持つ、**システム**アカウント。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-200">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="0e6f7-201">インストーラーが、アクセス拒否エラーのモジュールの設定を構成しようとするときに達するため、ローカル システム アカウントが変更しません IIS 共有構成で使用される共有のパスに対するアクセス許可、 *applicationHost.config*共有します。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-201">Because the local system account does not have modify permission for the share path which is used by the IIS Shared Configuration, the installer will hit an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span>

<span data-ttu-id="0e6f7-202">IIS 共有構成を無効にする、インストーラーを実行、更新されたエクスポートのサポートされていないの回避策は*applicationHost.config*共有ファイルおよび IIS 共有構成を再度有効にします。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-202">The unsupported workaround is to disable the IIS Shared Configuration, run the installer, export the updated *applicationHost.config* file to the share, and re-enable the IIS Shared Configuration.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="0e6f7-203">モジュール、スキーマ、および構成ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="0e6f7-203">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="0e6f7-204">Module</span><span class="sxs-lookup"><span data-stu-id="0e6f7-204">Module</span></span>

<span data-ttu-id="0e6f7-205">**IIS (x86 または amd64):**</span><span class="sxs-lookup"><span data-stu-id="0e6f7-205">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="0e6f7-206">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0e6f7-206">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="0e6f7-207">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0e6f7-207">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="0e6f7-208">**IIS Express (x86 または amd64):**</span><span class="sxs-lookup"><span data-stu-id="0e6f7-208">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="0e6f7-209">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0e6f7-209">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="0e6f7-210">%Programfiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0e6f7-210">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="0e6f7-211">Schema</span><span class="sxs-lookup"><span data-stu-id="0e6f7-211">Schema</span></span>

<span data-ttu-id="0e6f7-212">**IIS**</span><span class="sxs-lookup"><span data-stu-id="0e6f7-212">**IIS**</span></span>

   * <span data-ttu-id="0e6f7-213">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="0e6f7-213">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="0e6f7-214">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="0e6f7-214">**IIS Express**</span></span>

   * <span data-ttu-id="0e6f7-215">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="0e6f7-215">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="0e6f7-216">構成</span><span class="sxs-lookup"><span data-stu-id="0e6f7-216">Configuration</span></span>

<span data-ttu-id="0e6f7-217">**IIS**</span><span class="sxs-lookup"><span data-stu-id="0e6f7-217">**IIS**</span></span>

   * <span data-ttu-id="0e6f7-218">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="0e6f7-218">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="0e6f7-219">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="0e6f7-219">**IIS Express**</span></span>

   * <span data-ttu-id="0e6f7-220">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="0e6f7-220">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="0e6f7-221">検索することができます*aspnetcore.dll*で、 *applicationHost.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-221">You can search for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="0e6f7-222">IIS Express を*applicationHost.config*ファイルは既定では存在しません。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-222">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="0e6f7-223">ファイルが作成された*{アプリケーション ルート}\.vs\config* Visual Studio ソリューションで任意の web アプリケーション プロジェクトを開始するとします。</span><span class="sxs-lookup"><span data-stu-id="0e6f7-223">The file is created at *{application root}\.vs\config* when you start any web application project in the Visual Studio solution.</span></span>
