---
title: .NET での汎用ホスト
author: guardrex
description: アプリの起動と有効期間の管理を行う .NET の汎用ホストについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 9943c9dd2d6dd67a79186ee880b181a5915d06be
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505714"
---
# <a name="net-generic-host"></a><span data-ttu-id="1dbf7-103">.NET での汎用ホスト</span><span class="sxs-lookup"><span data-stu-id="1dbf7-103">.NET Generic Host</span></span>

<span data-ttu-id="1dbf7-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1dbf7-105">.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="1dbf7-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1dbf7-107">このトピックでは、HTTP 要求の処理を行わないアプリをホストするのに便利な、ASP.NET Core の汎用ホスト (<xref:Microsoft.Extensions.Hosting.HostBuilder>) について説明します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-107">This topic covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="1dbf7-108">Web ホスト (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>) の対象範囲については、<xref:fundamentals/host/web-host> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-108">For coverage of the Web Host (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="1dbf7-109">汎用ホストの目的は、Web ホスト API から HTTP パイプラインを切り離して、多様なホスト シナリオを有効にすることです。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="1dbf7-110">メッセージング、バックグラウンド タスク、および汎用ホストに基づくその他の HTTP ワークロードに対して、構成、依存関係の挿入 (DI)、ログなどの横断的機能によるメリットがあります。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="1dbf7-111">汎用ホストは ASP.NET Core 2.1 の新機能であり、Web ホスティングのシナリオには適していません。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="1dbf7-112">Web ホスティングのシナリオの場合は、[Web ホスト](xref:fundamentals/host/web-host)を使ってください。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="1dbf7-113">汎用ホストは開発中であり、将来のリリースでは Web ホストも汎用ホストに置き換えられて、汎用ホストが HTTP と非 HTTP 両方のシナリオのプライマリ ホスト API として機能するようになります。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="1dbf7-114">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1dbf7-115">[Visual Studio Code](https://code.visualstudio.com/) でサンプル アプリを実行するときは、"*外部ターミナルまたは統合ターミナル*" を使います。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="1dbf7-116">`internalConsole` ではサンプルを実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="1dbf7-117">Visual Studio Code でコンソールを設定するには:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="1dbf7-118">*.vscode/launch.json* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="1dbf7-119">**.NET Core Launch (console)** の構成で、**console** エントリを探します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="1dbf7-120">値を `externalTerminal` または `integratedTerminal` に設定します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="1dbf7-121">はじめに</span><span class="sxs-lookup"><span data-stu-id="1dbf7-121">Introduction</span></span>

<span data-ttu-id="1dbf7-122">汎用ホスト ライブラリは、<xref:Microsoft.Extensions.Hosting> 名前空間で使用でき、[Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) パッケージで提供されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-122">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="1dbf7-123">`Microsoft.Extensions.Hosting` パッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます (ASP.NET Core 2.1 以降)。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="1dbf7-124"><xref:Microsoft.Extensions.Hosting.IHostedService> がコード実行のエントリ ポイントです。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-124"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="1dbf7-125">`IHostedService` の各実装は、[ConfigureServices でのサービス登録](#configureservices)の順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="1dbf7-126">ホストが開始するときは `IHostedService` ごとに <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> が呼び出され、ホストが正常に終了するときは登録と逆の順序で <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each `IHostedService` when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="1dbf7-127">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="1dbf7-127">Set up a host</span></span>

<span data-ttu-id="1dbf7-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> は、ライブラリとアプリがホストの初期化、ビルド、実行に使用するメイン コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="1dbf7-129">既定のサービス</span><span class="sxs-lookup"><span data-stu-id="1dbf7-129">Default services</span></span>

<span data-ttu-id="1dbf7-130">次のサービスは、ホストの初期化中に登録されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="1dbf7-131">[環境](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="1dbf7-132">[構成](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="1dbf7-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="1dbf7-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="1dbf7-135">[オプション](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="1dbf7-136">[ログ](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="1dbf7-137">ホストの構成</span><span class="sxs-lookup"><span data-stu-id="1dbf7-137">Host configuration</span></span>

<span data-ttu-id="1dbf7-138">ホストの構成は、次のようにして作成されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-138">Host configuration is created by:</span></span>

* <span data-ttu-id="1dbf7-139"><xref:Microsoft.Extensions.Hosting.IHostBuilder> で拡張メソッドを呼び出して、[コンテンツ ルート](#content-root)と[環境](#environment)を設定する。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-139">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="1dbf7-140"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> の構成プロバイダーから構成を読み取る。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-140">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="1dbf7-141">拡張メソッド</span><span class="sxs-lookup"><span data-stu-id="1dbf7-141">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="1dbf7-142">アプリケーション キー (名前)</span><span class="sxs-lookup"><span data-stu-id="1dbf7-142">Application key (name)</span></span>

<span data-ttu-id="1dbf7-143">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) プロパティは、ホストの構築時にホストの構成から設定されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-143">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="1dbf7-144">値を明示的に設定するには、[HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey) を使用します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-144">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="1dbf7-145">**キー**: applicationName</span><span class="sxs-lookup"><span data-stu-id="1dbf7-145">**Key**: applicationName</span></span>  
<span data-ttu-id="1dbf7-146">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="1dbf7-146">**Type**: *string*</span></span>  
<span data-ttu-id="1dbf7-147">**既定**: アプリのエントリ ポイントを含むアセンブリの名前。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-147">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="1dbf7-148">**次を使用して設定**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="1dbf7-148">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="1dbf7-149">**環境変数**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="1dbf7-149">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

### <a name="content-root"></a><span data-ttu-id="1dbf7-150">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="1dbf7-150">Content root</span></span>

<span data-ttu-id="1dbf7-151">この設定では、ホストがコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-151">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="1dbf7-152">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="1dbf7-152">**Key**: contentRoot</span></span>  
<span data-ttu-id="1dbf7-153">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="1dbf7-153">**Type**: *string*</span></span>  
<span data-ttu-id="1dbf7-154">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-154">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1dbf7-155">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1dbf7-155">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="1dbf7-156">**環境変数**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="1dbf7-156">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="1dbf7-157">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-157">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="1dbf7-158">環境</span><span class="sxs-lookup"><span data-stu-id="1dbf7-158">Environment</span></span>

<span data-ttu-id="1dbf7-159">アプリの[環境](xref:fundamentals/environments)を設定します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-159">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1dbf7-160">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="1dbf7-160">**Key**: environment</span></span>  
<span data-ttu-id="1dbf7-161">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="1dbf7-161">**Type**: *string*</span></span>  
<span data-ttu-id="1dbf7-162">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="1dbf7-162">**Default**: Production</span></span>  
<span data-ttu-id="1dbf7-163">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1dbf7-163">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="1dbf7-164">**環境変数**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="1dbf7-164">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="1dbf7-165">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-165">The environment can be set to any value.</span></span> <span data-ttu-id="1dbf7-166">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-166">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1dbf7-167">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-167">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="1dbf7-168">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="1dbf7-168">ConfigureHostConfiguration</span></span>

<span data-ttu-id="1dbf7-169">`ConfigureHostConfiguration` は <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用して、ホストの <xref:Microsoft.Extensions.Configuration.IConfiguration> を作成します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-169">`ConfigureHostConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="1dbf7-170">ホストの構成は、<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> をアプリのビルド プロセスで使用するための初期化に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-170">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="1dbf7-171">`ConfigureHostConfiguration` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-171">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="1dbf7-172">ホストは、指定されたキーで最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-172">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="1dbf7-173">ホストの構成は自動的にアプリの構成に送られます ([ConfigureAppConfiguration](#configureappconfiguration) とアプリの残りの部分)。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-173">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="1dbf7-174">既定ではプロバイダーが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-174">No providers are included by default.</span></span> <span data-ttu-id="1dbf7-175">次のような、アプリが `ConfigureHostConfiguration` で必要とする構成プロバイダーを明示的に指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-175">You must explicitly specify whatever configuration providers the app requires in `ConfigureHostConfiguration`, including:</span></span>

* <span data-ttu-id="1dbf7-176">ファイルの構成 (*hostsettings.json* ファイルからなど)。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-176">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="1dbf7-177">環境変数の構成。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-177">Environment variable configuration.</span></span>
* <span data-ttu-id="1dbf7-178">コマンドライン引数の構成。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-178">Command-line argument configuration.</span></span>
* <span data-ttu-id="1dbf7-179">その他に必要なすべての構成プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-179">Any other required configuration providers.</span></span>

<span data-ttu-id="1dbf7-180">ホストのファイル構成は、いずれかの[ファイル構成プロバイダー](xref:fundamentals/configuration/index#file-configuration-provider)に対する呼び出しの前に `SetBasePath` のアプリのベース パスを指定することで有効になります。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-180">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="1dbf7-181">サンプル アプリは、JSON ファイル (*hostsettings.json*) を使用し、<xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> を呼び出して、ファイルのホスト構成設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-181">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="1dbf7-182">ホストの[環境変数の構成](xref:fundamentals/configuration/index#environment-variables-configuration-provider)を追加するには、ホスト ビルダーで <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-182">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="1dbf7-183">`AddEnvironmentVariables` は、オプションのユーザー定義プレフィックスを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-183">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="1dbf7-184">サンプル アプリは、`PREFIX_` のプレフィックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-184">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="1dbf7-185">環境変数が読み取られると、プレフィックスは削除されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-185">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="1dbf7-186">サンプル アプリのホストが構成されると、`PREFIX_ENVIRONMENT` の環境変数の値が `environment` キーのホスト構成値になります。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-186">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="1dbf7-187">開発中に [Visual Studio](https://www.visualstudio.com/) を使用している、または `dotnet run` を使用してアプリを実行している場合は、環境変数を *Properties/launchSettings.json* ファイルに設定することができます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-187">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="1dbf7-188">[Visual Studio Code](https://code.visualstudio.com/) では、開発中に環境変数を *.vscode/launch.json* ファイルに設定することができます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-188">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="1dbf7-189">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-189">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="1dbf7-190">[コマンドラインの構成](xref:fundamentals/configuration/index#command-line-configuration-provider)は、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>を呼び出すことで追加されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-190">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="1dbf7-191">コマンドラインの構成は、コマンドライン引数を許可して以前の構成プロバイダーから提供された構成をオーバーライドするために最後に追加されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-191">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="1dbf7-192">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-192">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="1dbf7-193">追加の構成は、[applicationName](#application-key-name) と [contentRoot](#content-root) キーで指定することができます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-193">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="1dbf7-194">`ConfigureHostConfiguration` を使う `HostBuilder` の構成の例:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-194">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="1dbf7-195">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1dbf7-195">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1dbf7-196">アプリの構成は、<xref:Microsoft.Extensions.Hosting.IHostBuilder> の実装で <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出すことで作成されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-196">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="1dbf7-197">`ConfigureAppConfiguration` は <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用して、アプリの <xref:Microsoft.Extensions.Configuration.IConfiguration> を作成します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-197">`ConfigureAppConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="1dbf7-198">`ConfigureAppConfiguration` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-198">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="1dbf7-199">アプリは、指定されたキーで最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-199">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="1dbf7-200">`ConfigureAppConfiguration` によって作成された構成は、以降の操作に対する [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) において、および <xref:Microsoft.Extensions.Hosting.IHost.Services*>サービス内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-200">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="1dbf7-201">アプリの構成は、[ConfigureHostConfiguration](#configurehostconfiguration) によって提供されたホストの構成を自動的に受信します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-201">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="1dbf7-202">`ConfigureAppConfiguration` を使うアプリ構成の例:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-202">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="1dbf7-203">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-203">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="1dbf7-204">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-204">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="1dbf7-205">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-205">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="1dbf7-206">設定ファイルを出力ディレクトリに移動するには、プロジェクト ファイルの設定ファイルを [MSBuild プロジェクト項目](/visualstudio/msbuild/common-msbuild-project-items) として指定します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-206">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="1dbf7-207">サンプル アプリはその JSON アプリ設定ファイルと、次の `<Content>` 項目を含む *hostsettings.json* を移動します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-207">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="1dbf7-208">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="1dbf7-208">ConfigureServices</span></span>

<span data-ttu-id="1dbf7-209"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> は、アプリの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーにサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-209"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1dbf7-210">`ConfigureServices` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-210">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="1dbf7-211">ホストされるサービスは、<xref:Microsoft.Extensions.Hosting.IHostedService> インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-211">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="1dbf7-212">詳細については、「<xref:fundamentals/host/hosted-services>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-212">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="1dbf7-213">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)では、`AddHostedService` 拡張メソッドを使って、有効期間イベント `LifetimeEventsHostedService` と時刻指定付きバックグラウンド タスク `TimedHostedService` のサービスをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-213">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="1dbf7-214">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="1dbf7-214">ConfigureLogging</span></span>

<span data-ttu-id="1dbf7-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> は、指定された <xref:Microsoft.Extensions.Logging.ILoggingBuilder> を構成するためのデリゲートを追加します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="1dbf7-216">`ConfigureLogging` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-216">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="1dbf7-217">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="1dbf7-217">UseConsoleLifetime</span></span>

<span data-ttu-id="1dbf7-218"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> は、`Ctrl+C`/SIGINT または SIGTERM をリッスンし、<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> を呼び出してシャットダウン プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-218"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for `Ctrl+C`/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="1dbf7-219">`UseConsoleLifetime` は、[RunAsync](#runasync) や [WaitForShutdownAsync](#waitforshutdownasync) などの拡張機能のブロックを解除します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-219">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="1dbf7-220"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> は、既定の有効期間の実装として事前に登録されています。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-220"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="1dbf7-221">最後に登録された有効期間が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-221">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="1dbf7-222">コンテナーの構成</span><span class="sxs-lookup"><span data-stu-id="1dbf7-222">Container configuration</span></span>

<span data-ttu-id="1dbf7-223">他のコンテナーの接続をサポートするため、ホストは <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1> を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-223">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span></span> <span data-ttu-id="1dbf7-224">ファクトリの提供は、DI コンテナーの登録の一部ではなく、具象 DI コンテナーの作成に使用されるホストの組み込みです。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-224">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="1dbf7-225">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) は、アプリのサービス プロバイダーの作成に使われる既定のファクトリをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-225">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="1dbf7-226">カスタム コンテナーの構成は、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> メソッドによって管理されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-226">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="1dbf7-227">`ConfigureContainer` は、基盤のホスト API を基にしてコンテナーを構成するための、厳密に型指定されたエクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-227">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="1dbf7-228">`ConfigureContainer` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-228">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="1dbf7-229">アプリのサービス コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-229">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="1dbf7-230">サービス コンテナーのファクトリを提供します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-230">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="1dbf7-231">ファクトリを使って、アプリのカスタム サービス コンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-231">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="1dbf7-232">機能拡張</span><span class="sxs-lookup"><span data-stu-id="1dbf7-232">Extensibility</span></span>

<span data-ttu-id="1dbf7-233">ホスト拡張機能は、`IHostBuilder` 上の拡張メソッドによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-233">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="1dbf7-234">次の例では、拡張メソッドが <xref:fundamentals/host/hosted-services> で示される [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) の例を使って `IHostBuilder` の実装を拡張する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-234">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="1dbf7-235">アプリは `UseHostedService` 拡張メソッドを確率して `T` に渡されたホステッド サービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-235">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="1dbf7-236">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="1dbf7-236">Manage the host</span></span>

<span data-ttu-id="1dbf7-237"><xref:Microsoft.Extensions.Hosting.IHost> の実装は、サービス コンテナーに登録されている `IHostedService` の実装の開始と停止を行います。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-237">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="1dbf7-238">実行</span><span class="sxs-lookup"><span data-stu-id="1dbf7-238">Run</span></span>

<span data-ttu-id="1dbf7-239"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> はアプリを実行し、ホストがシャットダウンされるまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-239"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="1dbf7-240">RunAsync</span><span class="sxs-lookup"><span data-stu-id="1dbf7-240">RunAsync</span></span>

<span data-ttu-id="1dbf7-241"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> はアプリを実行し、キャンセル トークンまたはシャットダウンがトリガーされると完了する `Task` を返します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-241"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="1dbf7-242">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="1dbf7-242">RunConsoleAsync</span></span>

<span data-ttu-id="1dbf7-243"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> は、コンソールのサポートを有効にし、ホストをビルドして開始した後、`Ctrl+C`/SIGINT または SIGTERM がシャットダウンするのを待機します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-243"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="1dbf7-244">Start と StopAsync</span><span class="sxs-lookup"><span data-stu-id="1dbf7-244">Start and StopAsync</span></span>

<span data-ttu-id="1dbf7-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> は、ホストを同期的に開始します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="1dbf7-246"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> は、指定されたタイムアウト内でホストの停止を試みます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-246"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="1dbf7-247">StartAsync と StopAsync</span><span class="sxs-lookup"><span data-stu-id="1dbf7-247">StartAsync and StopAsync</span></span>

<span data-ttu-id="1dbf7-248"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> がアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-248"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="1dbf7-249"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> がアプリを停止します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-249"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="1dbf7-250">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="1dbf7-250">WaitForShutdown</span></span>

<span data-ttu-id="1dbf7-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> は、<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (`Ctrl+C`/SIGINT or SIGTERM をリッスンする) などの <xref:Microsoft.Extensions.Hosting.IHostLifetime> を介してトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="1dbf7-252">`WaitForShutdown` は <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-252">`WaitForShutdown` calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="1dbf7-253">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="1dbf7-253">WaitForShutdownAsync</span></span>

<span data-ttu-id="1dbf7-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> が返す `Task` は、提供されたトークンによってシャットダウンがトリガーされると完了し、<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a `Task` that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="1dbf7-255">外部コントロール</span><span class="sxs-lookup"><span data-stu-id="1dbf7-255">External control</span></span>

<span data-ttu-id="1dbf7-256">ホストの外部コントロールは、外部から呼び出すことができるメソッドを使って実現できます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-256">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="1dbf7-257"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> は <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> の開始時に呼び出され、これが完了するまで待機してから続行します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-257"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="1dbf7-258">これを使って、外部イベントによって通知されるまで開始を遅らせることができます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-258">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1dbf7-259">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="1dbf7-259">IHostingEnvironment interface</span></span>

<span data-ttu-id="1dbf7-260"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> は、アプリのホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-260"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="1dbf7-261">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-261">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="1dbf7-262">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-262">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1dbf7-263">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="1dbf7-263">IApplicationLifetime interface</span></span>

<span data-ttu-id="1dbf7-264"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> は、正常なシャットダウンの要求など、起動後とシャットダウンのアクティビティに対応します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-264"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="1dbf7-265">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-265">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="1dbf7-266">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="1dbf7-266">Cancellation Token</span></span> | <span data-ttu-id="1dbf7-267">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="1dbf7-267">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="1dbf7-268">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-268">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="1dbf7-269">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-269">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1dbf7-270">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-270">All requests should be processed.</span></span> <span data-ttu-id="1dbf7-271">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-271">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="1dbf7-272">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-272">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1dbf7-273">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-273">Requests may still be processing.</span></span> <span data-ttu-id="1dbf7-274">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-274">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="1dbf7-275">コンストラクターは任意のクラスに `IApplicationLifetime` サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-275">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="1dbf7-276">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)は、`LifetimeEventsHostedService` クラス (`IHostedService` の実装) へのコンストラクターの挿入を使って、イベントを登録します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-276">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="1dbf7-277">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="1dbf7-277">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="1dbf7-278"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-278"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="1dbf7-279">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="1dbf7-279">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="1dbf7-280">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1dbf7-280">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="1dbf7-281">GitHub のホスティング リポジトリ サンプル</span><span class="sxs-lookup"><span data-stu-id="1dbf7-281">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
