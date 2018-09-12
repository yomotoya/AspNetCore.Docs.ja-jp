---
title: .NET での汎用ホスト
author: guardrex
description: アプリの起動と有効期間の管理を行う .NET の汎用ホストについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: e19a8a78b4c02fbae3d3acd23ee357c6003c35cf
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2018
ms.locfileid: "44039966"
---
# <a name="net-generic-host"></a><span data-ttu-id="8bbc1-103">.NET での汎用ホスト</span><span class="sxs-lookup"><span data-stu-id="8bbc1-103">.NET Generic Host</span></span>

<span data-ttu-id="8bbc1-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8bbc1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8bbc1-105">.NET Core アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="8bbc1-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="8bbc1-107">このトピックでは、HTTP 要求の処理を行わないアプリをホストするのに便利な、ASP.NET Core の汎用ホスト ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) について説明します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="8bbc1-108">Web ホスト ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) の対象範囲については、<xref:fundamentals/host/web-host> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="8bbc1-109">汎用ホストの目的は、Web ホスト API から HTTP パイプラインを切り離して、多様なホスト シナリオを有効にすることです。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="8bbc1-110">メッセージング、バックグラウンド タスク、および汎用ホストに基づくその他の HTTP ワークロードに対して、構成、依存関係の挿入 (DI)、ログなどの横断的機能によるメリットがあります。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="8bbc1-111">汎用ホストは ASP.NET Core 2.1 の新機能であり、Web ホスティングのシナリオには適していません。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="8bbc1-112">Web ホスティングのシナリオの場合は、[Web ホスト](xref:fundamentals/host/web-host)を使ってください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="8bbc1-113">汎用ホストは開発中であり、将来のリリースでは Web ホストも汎用ホストに置き換えられて、汎用ホストが HTTP と非 HTTP 両方のシナリオのプライマリ ホスト API として機能するようになります。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="8bbc1-114">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8bbc1-115">[Visual Studio Code](https://code.visualstudio.com/) でサンプル アプリを実行するときは、"*外部ターミナルまたは統合ターミナル*" を使います。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="8bbc1-116">`internalConsole` ではサンプルを実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="8bbc1-117">Visual Studio Code でコンソールを設定するには:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="8bbc1-118">*.vscode/launch.json* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="8bbc1-119">**.NET Core Launch (console)** の構成で、**console** エントリを探します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="8bbc1-120">値を `externalTerminal` または `integratedTerminal` に設定します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="8bbc1-121">はじめに</span><span class="sxs-lookup"><span data-stu-id="8bbc1-121">Introduction</span></span>

<span data-ttu-id="8bbc1-122">汎用ホスト ライブラリは、[Microsoft.Extensions.Hosting 名前空間](/dotnet/api/microsoft.extensions.hosting)で使用でき、[Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) パッケージで提供されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="8bbc1-123">`Microsoft.Extensions.Hosting` パッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます (ASP.NET Core 2.1 以降)。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="8bbc1-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) がコード実行のエントリ ポイントです。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="8bbc1-125">`IHostedService` の各実装は、[ConfigureServices でのサービス登録](#configureservices)の順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="8bbc1-126">ホストが開始するときは `IHostedService` ごとに [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) が呼び出され、ホストが正常に終了するときは登録と逆の順序で [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="8bbc1-127">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="8bbc1-127">Set up a host</span></span>

<span data-ttu-id="8bbc1-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) は、ライブラリとアプリがホストの初期化、ビルド、実行に使用するメイン コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="8bbc1-129">ホストの構成</span><span class="sxs-lookup"><span data-stu-id="8bbc1-129">Host configuration</span></span>

<span data-ttu-id="8bbc1-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) は、以下の方法でホストの構成値を設定します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="8bbc1-131">構成ビルダー</span><span class="sxs-lookup"><span data-stu-id="8bbc1-131">Configuration builder</span></span>
* <span data-ttu-id="8bbc1-132">拡張メソッドの構成</span><span class="sxs-lookup"><span data-stu-id="8bbc1-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="8bbc1-133">構成ビルダー</span><span class="sxs-lookup"><span data-stu-id="8bbc1-133">Configuration builder</span></span>

<span data-ttu-id="8bbc1-134">ホスト ビルダーの構成は、[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) の実装上で [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) を呼び出すことによって作成します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="8bbc1-135">`ConfigureHostConfiguration` は、[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使ってホストの [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を作成します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="8bbc1-136">構成ビルダーは、アプリのビルド プロセスで使用するために [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) を初期化します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="8bbc1-137">環境変数の構成は、既定では追加されません。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="8bbc1-138">環境変数からホストを構成するには、ホスト ビルダーで [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="8bbc1-139">`AddEnvironmentVariables` は、オプションのユーザー定義プレフィックスを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="8bbc1-140">サンプル アプリは、`PREFIX_` のプレフィックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="8bbc1-141">環境変数が読み取られると、プレフィックスは削除されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="8bbc1-142">サンプル アプリのホストが構成されると、`PREFIX_ENVIRONMENT` の環境変数の値が `environment` キーのホスト構成値になります。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="8bbc1-143">開発中に [Visual Studio](https://www.visualstudio.com/) を使用している、または `dotnet run` を使用してアプリを実行している場合は、環境変数を *Properties/launchSettings.json* ファイルに設定することができます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="8bbc1-144">[Visual Studio Code](https://code.visualstudio.com/) では、開発中に環境変数を *.vscode/launch.json* ファイルに設定することができます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="8bbc1-145">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-145">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="8bbc1-146">`ConfigureHostConfiguration` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8bbc1-147">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="8bbc1-148">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="8bbc1-149">`ConfigureHostConfiguration` を使う `HostBuilder` の構成の例:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="8bbc1-150">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 拡張メソッドでは、現在、[GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (たとえば、`.AddConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="8bbc1-151">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (`section:environment` など)。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="8bbc1-152">`AddConfiguration` メソッドには、`HostBuilder` のキー (`environment` など) と一致するキーを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="8bbc1-153">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="8bbc1-154">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="8bbc1-155">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="8bbc1-156">拡張メソッドの構成</span><span class="sxs-lookup"><span data-stu-id="8bbc1-156">Extension method configuration</span></span>

<span data-ttu-id="8bbc1-157">`IHostBuilder` の実装上で拡張メソッドを呼び出して、コンテンツ ルートと環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="8bbc1-158">アプリケーション キー (名前)</span><span class="sxs-lookup"><span data-stu-id="8bbc1-158">Application Key (Name)</span></span>

<span data-ttu-id="8bbc1-159">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) プロパティは、ホストの構築時にホストの構成から設定されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-159">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="8bbc1-160">値を明示的に設定するには、[HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey) を使用します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-160">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="8bbc1-161">**キー**: applicationName</span><span class="sxs-lookup"><span data-stu-id="8bbc1-161">**Key**: applicationName</span></span>  
<span data-ttu-id="8bbc1-162">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8bbc1-162">**Type**: *string*</span></span>  
<span data-ttu-id="8bbc1-163">**既定**: アプリのエントリ ポイントを含むアセンブリの名前。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-163">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="8bbc1-164">**次を使用して設定**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="8bbc1-164">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="8bbc1-165">**環境変数**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="8bbc1-165">**Environment variable**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="8bbc1-166">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="8bbc1-166">Content Root</span></span>

<span data-ttu-id="8bbc1-167">この設定では、ホストがコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-167">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="8bbc1-168">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="8bbc1-168">**Key**: contentRoot</span></span>  
<span data-ttu-id="8bbc1-169">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8bbc1-169">**Type**: *string*</span></span>  
<span data-ttu-id="8bbc1-170">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-170">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="8bbc1-171">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="8bbc1-171">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="8bbc1-172">**環境変数**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="8bbc1-172">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="8bbc1-173">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-173">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="8bbc1-174">環境</span><span class="sxs-lookup"><span data-stu-id="8bbc1-174">Environment</span></span>

<span data-ttu-id="8bbc1-175">アプリの[環境](xref:fundamentals/environments)を設定します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-175">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8bbc1-176">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="8bbc1-176">**Key**: environment</span></span>  
<span data-ttu-id="8bbc1-177">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="8bbc1-177">**Type**: *string*</span></span>  
<span data-ttu-id="8bbc1-178">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="8bbc1-178">**Default**: Production</span></span>  
<span data-ttu-id="8bbc1-179">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="8bbc1-179">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="8bbc1-180">**環境変数**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="8bbc1-180">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="8bbc1-181">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-181">The environment can be set to any value.</span></span> <span data-ttu-id="8bbc1-182">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-182">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="8bbc1-183">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-183">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="8bbc1-184">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="8bbc1-184">ConfigureAppConfiguration</span></span>

<span data-ttu-id="8bbc1-185">アプリ ビルダーの構成は、[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) の実装上で [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) を呼び出すことによって作成します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-185">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="8bbc1-186">`ConfigureAppConfiguration` は、[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使ってアプリの [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を作成します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-186">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="8bbc1-187">`ConfigureAppConfiguration` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-187">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="8bbc1-188">アプリは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-188">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="8bbc1-189">`ConfigureAppConfiguration` によって作成された構成は、以降の操作に対する [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) において、および [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services) 内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-189">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="8bbc1-190">`ConfigureAppConfiguration` を使うアプリ構成の例:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-190">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="8bbc1-191">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-191">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="8bbc1-192">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-192">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="8bbc1-193">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-193">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="8bbc1-194">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 拡張メソッドでは、現在、[GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (たとえば、`.AddConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-194">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="8bbc1-195">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (`section:Logging:LogLevel:Default` など)。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-195">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="8bbc1-196">`AddConfiguration` メソッドでは、構成キーが完全に一致している必要があります (`Logging:LogLevel:Default` など)。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-196">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="8bbc1-197">キーにセクション名があると、セクションの値でアプリを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-197">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="8bbc1-198">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-198">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="8bbc1-199">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-199">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="8bbc1-200">設定ファイルを出力ディレクトリに移動するには、プロジェクト ファイルの設定ファイルを [MSBuild プロジェクト項目](/visualstudio/msbuild/common-msbuild-project-items) として指定します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-200">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="8bbc1-201">サンプル アプリはその JSON アプリ設定ファイルと、次の **&lt;Content:&gt;** 項目を含む *hostsettings.json* を移動します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-201">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="8bbc1-202">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="8bbc1-202">ConfigureServices</span></span>

<span data-ttu-id="8bbc1-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) は、アプリの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーにサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8bbc1-204">`ConfigureServices` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-204">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="8bbc1-205">ホストされるサービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-205">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="8bbc1-206">詳細については、「<xref:fundamentals/host/hosted-services>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-206">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="8bbc1-207">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)では、`AddHostedService` 拡張メソッドを使って、有効期間イベント `LifetimeEventsHostedService` と時刻指定付きバックグラウンド タスク `TimedHostedService` のサービスをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="8bbc1-208">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="8bbc1-208">ConfigureLogging</span></span>

<span data-ttu-id="8bbc1-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) は、提供された [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) を構成するためのデリゲートを追加します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="8bbc1-210">`ConfigureLogging` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-210">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="8bbc1-211">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="8bbc1-211">UseConsoleLifetime</span></span>

<span data-ttu-id="8bbc1-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) は、`Ctrl+C`/SIGINT または SIGTERM をリッスンし、[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) を呼び出してシャットダウン プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="8bbc1-213">`UseConsoleLifetime` は、[RunAsync](#runasync) や [WaitForShutdownAsync](#waitforshutdownasync) などの拡張機能のブロックを解除します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-213">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="8bbc1-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) は、既定の有効期間の実装として事前に登録されています。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="8bbc1-215">最後に登録された有効期間が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-215">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="8bbc1-216">コンテナーの構成</span><span class="sxs-lookup"><span data-stu-id="8bbc1-216">Container configuration</span></span>

<span data-ttu-id="8bbc1-217">他のコンテナーの接続をサポートするため、ホストは [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1) を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-217">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="8bbc1-218">ファクトリの提供は、DI コンテナーの登録の一部ではなく、具象 DI コンテナーの作成に使用されるホストの組み込みです。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-218">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="8bbc1-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) は、アプリのサービス プロバイダーの作成に使われる既定のファクトリをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="8bbc1-220">カスタム コンテナーの構成は、[ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) メソッドによって管理されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-220">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="8bbc1-221">`ConfigureContainer` は、基盤のホスト API を基にしてコンテナーを構成するための、厳密に型指定されたエクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-221">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="8bbc1-222">`ConfigureContainer` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-222">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="8bbc1-223">アプリのサービス コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-223">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="8bbc1-224">サービス コンテナーのファクトリを提供します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-224">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="8bbc1-225">ファクトリを使って、アプリのカスタム サービス コンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-225">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="8bbc1-226">機能拡張</span><span class="sxs-lookup"><span data-stu-id="8bbc1-226">Extensibility</span></span>

<span data-ttu-id="8bbc1-227">ホスト拡張機能は、`IHostBuilder` 上の拡張メソッドによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-227">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="8bbc1-228">次の例では、拡張メソッドが <xref:fundamentals/host/hosted-services> で示される [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) の例を使って `IHostBuilder` の実装を拡張する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-228">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="8bbc1-229">アプリは `UseHostedService` 拡張メソッドを確率して `T` に渡されたホステッド サービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-229">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="8bbc1-230">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="8bbc1-230">Manage the host</span></span>

<span data-ttu-id="8bbc1-231">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) の実装は、サービス コンテナーに登録されている `IHostedService` の実装の開始と停止を行います。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-231">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="8bbc1-232">実行</span><span class="sxs-lookup"><span data-stu-id="8bbc1-232">Run</span></span>

<span data-ttu-id="8bbc1-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) はアプリを実行し、ホストがシャットダウンされるまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="8bbc1-234">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8bbc1-234">RunAsync</span></span>

<span data-ttu-id="8bbc1-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) はアプリを実行し、キャンセル トークンまたはシャットダウンがトリガーされると完了する `Task` を返します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="8bbc1-236">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="8bbc1-236">RunConsoleAsync</span></span>

<span data-ttu-id="8bbc1-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) は、コンソールのサポートを有効にし、ホストをビルドして開始した後、`Ctrl+C`/SIGINT または SIGTERM がシャットダウンするのを待機します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="8bbc1-238">Start と StopAsync</span><span class="sxs-lookup"><span data-stu-id="8bbc1-238">Start and StopAsync</span></span>

<span data-ttu-id="8bbc1-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) は、ホストを同期的に開始します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="8bbc1-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) は、指定されたタイムアウト内でホストの停止を試みます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="8bbc1-241">StartAsync と StopAsync</span><span class="sxs-lookup"><span data-stu-id="8bbc1-241">StartAsync and StopAsync</span></span>

<span data-ttu-id="8bbc1-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) はアプリを開始します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="8bbc1-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) はアプリを停止します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="8bbc1-244">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="8bbc1-244">WaitForShutdown</span></span>

<span data-ttu-id="8bbc1-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) は、[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (`Ctrl+C`/SIGINT または SIGTERM をリッスンします) などの [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime) によってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="8bbc1-246">`WaitForShutdown` は [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-246">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="8bbc1-247">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="8bbc1-247">WaitForShutdownAsync</span></span>

<span data-ttu-id="8bbc1-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) が返す `Task` は、提供されたトークンによってシャットダウンがトリガーされると完了し、[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="8bbc1-249">外部コントロール</span><span class="sxs-lookup"><span data-stu-id="8bbc1-249">External control</span></span>

<span data-ttu-id="8bbc1-250">ホストの外部コントロールは、外部から呼び出すことができるメソッドを使って実現できます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-250">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="8bbc1-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) は [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) の開始時に呼び出されて、StartAsync が完了するまで待機してから続行します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="8bbc1-252">これを使って、外部イベントによって通知されるまで開始を遅らせることができます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-252">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="8bbc1-253">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="8bbc1-253">IHostingEnvironment interface</span></span>

<span data-ttu-id="8bbc1-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) は、アプリのホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="8bbc1-255">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-255">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="8bbc1-256">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-256">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="8bbc1-257">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="8bbc1-257">IApplicationLifetime interface</span></span>

<span data-ttu-id="8bbc1-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) は、正常なシャットダウンの要求など、起動後とシャットダウンのアクティビティに対応します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="8bbc1-259">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-259">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="8bbc1-260">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="8bbc1-260">Cancellation Token</span></span> | <span data-ttu-id="8bbc1-261">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="8bbc1-261">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="8bbc1-262">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="8bbc1-262">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="8bbc1-263">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-263">The host has fully started.</span></span> |
| [<span data-ttu-id="8bbc1-264">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="8bbc1-264">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="8bbc1-265">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-265">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="8bbc1-266">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-266">All requests should be processed.</span></span> <span data-ttu-id="8bbc1-267">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-267">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="8bbc1-268">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="8bbc1-268">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="8bbc1-269">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-269">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="8bbc1-270">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-270">Requests may still be processing.</span></span> <span data-ttu-id="8bbc1-271">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-271">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="8bbc1-272">コンストラクターは任意のクラスに `IApplicationLifetime` サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-272">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="8bbc1-273">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)は、`LifetimeEventsHostedService` クラス (`IHostedService` の実装) へのコンストラクターの挿入を使って、イベントを登録します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-273">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="8bbc1-274">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="8bbc1-274">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="8bbc1-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="8bbc1-276">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="8bbc1-276">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="8bbc1-277">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8bbc1-277">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="8bbc1-278">GitHub のホスティング リポジトリ サンプル</span><span class="sxs-lookup"><span data-stu-id="8bbc1-278">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
