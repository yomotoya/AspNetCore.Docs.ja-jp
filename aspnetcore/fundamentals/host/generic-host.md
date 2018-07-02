---
title: .NET での汎用ホスト
author: guardrex
description: アプリの起動と有効期間の管理を行う .NET の汎用ホストについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 33e5829ce4a09e132743b4174a588cf232a44775
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276262"
---
# <a name="net-generic-host"></a><span data-ttu-id="1f119-103">.NET での汎用ホスト</span><span class="sxs-lookup"><span data-stu-id="1f119-103">.NET Generic Host</span></span>

<span data-ttu-id="1f119-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1f119-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1f119-105">.NET アプリは*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="1f119-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="1f119-106">ホストはアプリの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="1f119-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1f119-107">このトピックでは、HTTP 要求の処理を行わないアプリをホストするのに便利な、ASP.NET Core の汎用ホスト ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) について説明します。</span><span class="sxs-lookup"><span data-stu-id="1f119-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="1f119-108">Web ホスト ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) については、[Web ホスト](xref:fundamentals/host/web-host)に関するトピックをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1f119-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="1f119-109">汎用ホストの目的は、Web ホスト API から HTTP パイプラインを切り離して、多様なホスト シナリオを有効にすることです。</span><span class="sxs-lookup"><span data-stu-id="1f119-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="1f119-110">メッセージング、バックグラウンド タスク、および汎用ホストに基づくその他の HTTP ワークロードに対して、構成、依存関係の挿入 (DI)、ログなどの横断的機能によるメリットがあります。</span><span class="sxs-lookup"><span data-stu-id="1f119-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="1f119-111">汎用ホストは ASP.NET Core 2.1 の新機能であり、Web ホスティングのシナリオには適していません。</span><span class="sxs-lookup"><span data-stu-id="1f119-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="1f119-112">Web ホスティングのシナリオの場合は、[Web ホスト](xref:fundamentals/host/web-host)を使ってください。</span><span class="sxs-lookup"><span data-stu-id="1f119-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="1f119-113">汎用ホストは開発中であり、将来のリリースでは Web ホストも汎用ホストに置き換えられて、汎用ホストが HTTP と非 HTTP 両方のシナリオのプライマリ ホスト API として機能するようになります。</span><span class="sxs-lookup"><span data-stu-id="1f119-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="1f119-114">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1f119-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1f119-115">[Visual Studio Code](https://code.visualstudio.com/) でサンプル アプリを実行するときは、"*外部ターミナルまたは統合ターミナル*" を使います。</span><span class="sxs-lookup"><span data-stu-id="1f119-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="1f119-116">`internalConsole` ではサンプルを実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="1f119-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="1f119-117">Visual Studio Code でコンソールを設定するには:</span><span class="sxs-lookup"><span data-stu-id="1f119-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="1f119-118">*.vscode/launch.json* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="1f119-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="1f119-119">**.NET Core Launch (console)** の構成で、**console** エントリを探します。</span><span class="sxs-lookup"><span data-stu-id="1f119-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="1f119-120">値を `externalTerminal` または `integratedTerminal` に設定します。</span><span class="sxs-lookup"><span data-stu-id="1f119-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="1f119-121">はじめに</span><span class="sxs-lookup"><span data-stu-id="1f119-121">Introduction</span></span>

<span data-ttu-id="1f119-122">汎用ホスト ライブラリは、[Microsoft.Extensions.Hosting 名前空間](/dotnet/api/microsoft.extensions.hosting)で使用でき、[Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) パッケージで提供されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="1f119-123">`Microsoft.Extensions.Hosting` パッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます (ASP.NET Core 2.1 以降)。</span><span class="sxs-lookup"><span data-stu-id="1f119-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="1f119-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) がコード実行のエントリ ポイントです。</span><span class="sxs-lookup"><span data-stu-id="1f119-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="1f119-125">`IHostedService` の各実装は、[ConfigureServices でのサービス登録](#configureservices)の順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="1f119-126">ホストが開始するときは `IHostedService` ごとに [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) が呼び出され、ホストが正常に終了するときは登録と逆の順序で [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="1f119-127">ホストを設定する</span><span class="sxs-lookup"><span data-stu-id="1f119-127">Set up a host</span></span>

<span data-ttu-id="1f119-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) は、ライブラリとアプリがホストの初期化、ビルド、実行に使用するメイン コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="1f119-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="1f119-129">ホストの構成</span><span class="sxs-lookup"><span data-stu-id="1f119-129">Host configuration</span></span>

<span data-ttu-id="1f119-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) は、以下の方法でホストの構成値を設定します。</span><span class="sxs-lookup"><span data-stu-id="1f119-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="1f119-131">構成ビルダー</span><span class="sxs-lookup"><span data-stu-id="1f119-131">Configuration builder</span></span>
* <span data-ttu-id="1f119-132">拡張メソッドの構成</span><span class="sxs-lookup"><span data-stu-id="1f119-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="1f119-133">構成ビルダー</span><span class="sxs-lookup"><span data-stu-id="1f119-133">Configuration builder</span></span>

<span data-ttu-id="1f119-134">ホスト ビルダーの構成は、[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) の実装上で [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) を呼び出すことによって作成します。</span><span class="sxs-lookup"><span data-stu-id="1f119-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="1f119-135">`ConfigureHostConfiguration` は、[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使ってホストの [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を作成します。</span><span class="sxs-lookup"><span data-stu-id="1f119-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="1f119-136">構成ビルダーは、アプリのビルド プロセスで使用するために [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) を初期化します。</span><span class="sxs-lookup"><span data-stu-id="1f119-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="1f119-137">環境変数の構成は、既定では追加されません。</span><span class="sxs-lookup"><span data-stu-id="1f119-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="1f119-138">環境変数からホストを構成するには、ホスト ビルダーで [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1f119-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="1f119-139">`AddEnvironmentVariables` は、オプションのユーザー定義プレフィックスを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="1f119-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="1f119-140">サンプル アプリは、`PREFIX_` のプレフィックスを使用します。</span><span class="sxs-lookup"><span data-stu-id="1f119-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="1f119-141">環境変数が読み取られると、プレフィックスは削除されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="1f119-142">サンプル アプリのホストが構成されると、`PREFIX_ENVIRONMENT` の環境変数の値が `environment` キーのホスト構成値になります。</span><span class="sxs-lookup"><span data-stu-id="1f119-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="1f119-143">開発中に [Visual Studio](https://www.visualstudio.com/) を使用している、または `dotnet run` を使用してアプリを実行している場合は、環境変数を *Properties/launchSettings.json* ファイルに設定することができます。</span><span class="sxs-lookup"><span data-stu-id="1f119-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="1f119-144">[Visual Studio Code](https://code.visualstudio.com/) では、開発中に環境変数を *.vscode/launch.json* ファイルに設定することができます。</span><span class="sxs-lookup"><span data-stu-id="1f119-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="1f119-145">詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1f119-145">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1f119-146">`ConfigureHostConfiguration` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1f119-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="1f119-147">ホストは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="1f119-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="1f119-148">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1f119-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="1f119-149">`ConfigureHostConfiguration` を使う `HostBuilder` の構成の例:</span><span class="sxs-lookup"><span data-stu-id="1f119-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="1f119-150">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 拡張メソッドでは、現在、[GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (たとえば、`.AddConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="1f119-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="1f119-151">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (`section:environment` など)。</span><span class="sxs-lookup"><span data-stu-id="1f119-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="1f119-152">`AddConfiguration` メソッドには、`HostBuilder` のキー (`environment` など) と一致するキーを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f119-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="1f119-153">キーにセクション名があると、セクションの値でホストを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="1f119-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="1f119-154">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="1f119-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="1f119-155">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1f119-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="1f119-156">拡張メソッドの構成</span><span class="sxs-lookup"><span data-stu-id="1f119-156">Extension method configuration</span></span>

<span data-ttu-id="1f119-157">`IHostBuilder` の実装上で拡張メソッドを呼び出して、コンテンツ ルートと環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="1f119-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="1f119-158">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="1f119-158">Content Root</span></span>

<span data-ttu-id="1f119-159">この設定では、ホストがコンテンツ ファイルの検索を開始する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="1f119-159">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="1f119-160">**キー**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="1f119-160">**Key**: contentRoot</span></span>  
<span data-ttu-id="1f119-161">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="1f119-161">**Type**: *string*</span></span>  
<span data-ttu-id="1f119-162">**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-162">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1f119-163">**次を使用して設定**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1f119-163">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="1f119-164">**環境変数**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="1f119-164">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="1f119-165">パスが存在しない場合は、ホストを起動できません。</span><span class="sxs-lookup"><span data-stu-id="1f119-165">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="1f119-166">環境</span><span class="sxs-lookup"><span data-stu-id="1f119-166">Environment</span></span>

<span data-ttu-id="1f119-167">アプリの[環境](xref:fundamentals/environments)を設定します。</span><span class="sxs-lookup"><span data-stu-id="1f119-167">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1f119-168">**キー**: 環境</span><span class="sxs-lookup"><span data-stu-id="1f119-168">**Key**: environment</span></span>  
<span data-ttu-id="1f119-169">**型**: *文字列*</span><span class="sxs-lookup"><span data-stu-id="1f119-169">**Type**: *string*</span></span>  
<span data-ttu-id="1f119-170">**既定値**: Production</span><span class="sxs-lookup"><span data-stu-id="1f119-170">**Default**: Production</span></span>  
<span data-ttu-id="1f119-171">**次を使用して設定**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1f119-171">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="1f119-172">**環境変数**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="1f119-172">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="1f119-173">環境は任意の値に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="1f119-173">The environment can be set to any value.</span></span> <span data-ttu-id="1f119-174">フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1f119-174">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1f119-175">値は大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="1f119-175">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="1f119-176">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1f119-176">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1f119-177">アプリ ビルダーの構成は、[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) の実装上で [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) を呼び出すことによって作成します。</span><span class="sxs-lookup"><span data-stu-id="1f119-177">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="1f119-178">`ConfigureAppConfiguration` は、[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使ってアプリの [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を作成します。</span><span class="sxs-lookup"><span data-stu-id="1f119-178">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="1f119-179">`ConfigureAppConfiguration` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1f119-179">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="1f119-180">アプリは、最後に値を設定したオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="1f119-180">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="1f119-181">`ConfigureAppConfiguration` によって作成された構成は、以降の操作に対する [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) において、および [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services) 内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="1f119-181">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="1f119-182">`ConfigureAppConfiguration` を使うアプリ構成の例:</span><span class="sxs-lookup"><span data-stu-id="1f119-182">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="1f119-183">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1f119-183">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="1f119-184">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="1f119-184">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="1f119-185">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="1f119-185">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="1f119-186">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 拡張メソッドでは、現在、[GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (たとえば、`.AddConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。</span><span class="sxs-lookup"><span data-stu-id="1f119-186">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="1f119-187">`GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (`section:Logging:LogLevel:Default` など)。</span><span class="sxs-lookup"><span data-stu-id="1f119-187">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="1f119-188">`AddConfiguration` メソッドでは、構成キーが完全に一致している必要があります (`Logging:LogLevel:Default` など)。</span><span class="sxs-lookup"><span data-stu-id="1f119-188">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="1f119-189">キーにセクション名があると、セクションの値でアプリを構成できなくなります。</span><span class="sxs-lookup"><span data-stu-id="1f119-189">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="1f119-190">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="1f119-190">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="1f119-191">詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1f119-191">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="1f119-192">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="1f119-192">ConfigureServices</span></span>

<span data-ttu-id="1f119-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) は、アプリの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーにサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1f119-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1f119-194">`ConfigureServices` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1f119-194">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="1f119-195">ホストされるサービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="1f119-195">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="1f119-196">詳しくは、「[ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク](xref:fundamentals/host/hosted-services)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1f119-196">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="1f119-197">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)では、`AddHostedService` 拡張メソッドを使って、有効期間イベント `LifetimeEventsHostedService` と時刻指定付きバックグラウンド タスク `TimedHostedService` のサービスをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="1f119-197">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="1f119-198">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="1f119-198">ConfigureLogging</span></span>

<span data-ttu-id="1f119-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) は、提供された [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) を構成するためのデリゲートを追加します。</span><span class="sxs-lookup"><span data-stu-id="1f119-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="1f119-200">`ConfigureLogging` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1f119-200">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="1f119-201">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="1f119-201">UseConsoleLifetime</span></span>

<span data-ttu-id="1f119-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) は、`Ctrl+C`/SIGINT または SIGTERM をリッスンし、[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) を呼び出してシャットダウン プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="1f119-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="1f119-203">`UseConsoleLifetime` は、[RunAsync](#runasync) や [WaitForShutdownAsync](#waitforshutdownasync) などの拡張機能のブロックを解除します。</span><span class="sxs-lookup"><span data-stu-id="1f119-203">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="1f119-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) は、既定の有効期間の実装として事前に登録されています。</span><span class="sxs-lookup"><span data-stu-id="1f119-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="1f119-205">最後に登録された有効期間が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-205">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="1f119-206">コンテナーの構成</span><span class="sxs-lookup"><span data-stu-id="1f119-206">Container configuration</span></span>

<span data-ttu-id="1f119-207">他のコンテナーの接続をサポートするため、ホストは [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1) を受け入れることができます。</span><span class="sxs-lookup"><span data-stu-id="1f119-207">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="1f119-208">ファクトリの提供は、DI コンテナーの登録の一部ではなく、具象 DI コンテナーの作成に使用されるホストの組み込みです。</span><span class="sxs-lookup"><span data-stu-id="1f119-208">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="1f119-209">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) は、アプリのサービス プロバイダーの作成に使われる既定のファクトリをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="1f119-209">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="1f119-210">カスタム コンテナーの構成は、[ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) メソッドによって管理されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-210">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="1f119-211">`ConfigureContainer` は、基盤のホスト API を基にしてコンテナーを構成するための、厳密に型指定されたエクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="1f119-211">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="1f119-212">`ConfigureContainer` を複数回呼び出して結果を追加できます。</span><span class="sxs-lookup"><span data-stu-id="1f119-212">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="1f119-213">アプリのサービス コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1f119-213">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="1f119-214">サービス コンテナーのファクトリを提供します。</span><span class="sxs-lookup"><span data-stu-id="1f119-214">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="1f119-215">ファクトリを使って、アプリのカスタム サービス コンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="1f119-215">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="1f119-216">機能拡張</span><span class="sxs-lookup"><span data-stu-id="1f119-216">Extensibility</span></span>

<span data-ttu-id="1f119-217">ホスト拡張機能は、`IHostBuilder` 上の拡張メソッドによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="1f119-217">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="1f119-218">次の例では、拡張メソッドが [RabbitMQ](https://www.rabbitmq.com/) を使って `IHostBuilder` の実装を拡張する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="1f119-218">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="1f119-219">(アプリの別の場所にある) 拡張メソッドが RabbitMQ の `IHostedService` を登録します。</span><span class="sxs-lookup"><span data-stu-id="1f119-219">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="1f119-220">ホストを管理する</span><span class="sxs-lookup"><span data-stu-id="1f119-220">Manage the host</span></span>

<span data-ttu-id="1f119-221">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) の実装は、サービス コンテナーに登録されている `IHostedService` の実装の開始と停止を行います。</span><span class="sxs-lookup"><span data-stu-id="1f119-221">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="1f119-222">実行</span><span class="sxs-lookup"><span data-stu-id="1f119-222">Run</span></span>

<span data-ttu-id="1f119-223">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) はアプリを実行し、ホストがシャットダウンされるまで呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="1f119-223">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="1f119-224">RunAsync</span><span class="sxs-lookup"><span data-stu-id="1f119-224">RunAsync</span></span>

<span data-ttu-id="1f119-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) はアプリを実行し、キャンセル トークンまたはシャットダウンがトリガーされると完了する `Task` を返します。</span><span class="sxs-lookup"><span data-stu-id="1f119-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="1f119-226">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="1f119-226">RunConsoleAsync</span></span>

<span data-ttu-id="1f119-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) は、コンソールのサポートを有効にし、ホストをビルドして開始した後、`Ctrl+C`/SIGINT または SIGTERM がシャットダウンするのを待機します。</span><span class="sxs-lookup"><span data-stu-id="1f119-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="1f119-228">Start と StopAsync</span><span class="sxs-lookup"><span data-stu-id="1f119-228">Start and StopAsync</span></span>

<span data-ttu-id="1f119-229">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) は、ホストを同期的に開始します。</span><span class="sxs-lookup"><span data-stu-id="1f119-229">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="1f119-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) は、指定されたタイムアウト内でホストの停止を試みます。</span><span class="sxs-lookup"><span data-stu-id="1f119-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="1f119-231">StartAsync と StopAsync</span><span class="sxs-lookup"><span data-stu-id="1f119-231">StartAsync and StopAsync</span></span>

<span data-ttu-id="1f119-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) はアプリを開始します。</span><span class="sxs-lookup"><span data-stu-id="1f119-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="1f119-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) はアプリを停止します。</span><span class="sxs-lookup"><span data-stu-id="1f119-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="1f119-234">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="1f119-234">WaitForShutdown</span></span>

<span data-ttu-id="1f119-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) は、[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (`Ctrl+C`/SIGINT または SIGTERM をリッスンします) などの [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime) によってトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="1f119-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="1f119-236">`WaitForShutdown` は [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1f119-236">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="1f119-237">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="1f119-237">WaitForShutdownAsync</span></span>

<span data-ttu-id="1f119-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) が返す `Task` は、提供されたトークンによってシャットダウンがトリガーされると完了し、[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1f119-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="1f119-239">外部コントロール</span><span class="sxs-lookup"><span data-stu-id="1f119-239">External control</span></span>

<span data-ttu-id="1f119-240">ホストの外部コントロールは、外部から呼び出すことができるメソッドを使って実現できます。</span><span class="sxs-lookup"><span data-stu-id="1f119-240">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="1f119-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) は [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) の開始時に呼び出されて、StartAsync が完了するまで待機してから続行します。</span><span class="sxs-lookup"><span data-stu-id="1f119-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="1f119-242">これを使って、外部イベントによって通知されるまで開始を遅らせることができます。</span><span class="sxs-lookup"><span data-stu-id="1f119-242">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1f119-243">IHostingEnvironment インターフェイス</span><span class="sxs-lookup"><span data-stu-id="1f119-243">IHostingEnvironment interface</span></span>

<span data-ttu-id="1f119-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) は、アプリのホスティング環境に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="1f119-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="1f119-245">プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。</span><span class="sxs-lookup"><span data-stu-id="1f119-245">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="1f119-246">詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1f119-246">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1f119-247">IApplicationLifetime インターフェイス</span><span class="sxs-lookup"><span data-stu-id="1f119-247">IApplicationLifetime interface</span></span>

<span data-ttu-id="1f119-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) は、正常なシャットダウンの要求など、起動後とシャットダウンのアクティビティに対応します。</span><span class="sxs-lookup"><span data-stu-id="1f119-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="1f119-249">インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。</span><span class="sxs-lookup"><span data-stu-id="1f119-249">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="1f119-250">キャンセル トークン</span><span class="sxs-lookup"><span data-stu-id="1f119-250">Cancellation Token</span></span> | <span data-ttu-id="1f119-251">トリガーのタイミング</span><span class="sxs-lookup"><span data-stu-id="1f119-251">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="1f119-252">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="1f119-252">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="1f119-253">ホストが完全に起動されたとき。</span><span class="sxs-lookup"><span data-stu-id="1f119-253">The host has fully started.</span></span> |
| [<span data-ttu-id="1f119-254">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="1f119-254">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="1f119-255">ホストが正常なシャットダウンを完了しているとき。</span><span class="sxs-lookup"><span data-stu-id="1f119-255">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1f119-256">すべての要求を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1f119-256">All requests should be processed.</span></span> <span data-ttu-id="1f119-257">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="1f119-257">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="1f119-258">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="1f119-258">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="1f119-259">ホストが正常なシャットダウンを行っているとき。</span><span class="sxs-lookup"><span data-stu-id="1f119-259">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1f119-260">要求がまだ処理されている可能性がある場合。</span><span class="sxs-lookup"><span data-stu-id="1f119-260">Requests may still be processing.</span></span> <span data-ttu-id="1f119-261">このイベントが完了するまで、シャットダウンはブロックされます。</span><span class="sxs-lookup"><span data-stu-id="1f119-261">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="1f119-262">コンストラクターは任意のクラスに `IApplicationLifetime` サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="1f119-262">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="1f119-263">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) は、`LifetimeEventsHostedService` クラス (`IHostedService` の実装) へのコンストラクターの挿入を使って、イベントを登録します。</span><span class="sxs-lookup"><span data-stu-id="1f119-263">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="1f119-264">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="1f119-264">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="1f119-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。</span><span class="sxs-lookup"><span data-stu-id="1f119-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="1f119-266">以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="1f119-266">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="1f119-267">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1f119-267">Additional resources</span></span>

* [<span data-ttu-id="1f119-268">ホストされるサービスを使用するバックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="1f119-268">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="1f119-269">GitHub のホスティング リポジトリ サンプル</span><span class="sxs-lookup"><span data-stu-id="1f119-269">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
