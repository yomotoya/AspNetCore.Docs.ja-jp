---
title: .NET での汎用ホスト
author: guardrex
description: アプリの起動と有効期間の管理を行う ASP.NET Core の汎用ホストについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: bb6afe59fcad685d18cdc9c8d90cfcc7b3a6541d
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809196"
---
# <a name="net-generic-host"></a>.NET での汎用ホスト

作成者: [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core アプリはホストを構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。

この記事では、.NET Core 汎用ホスト (<xref:Microsoft.Extensions.Hosting.HostBuilder>) について取り上げます。

Web ホスト API から HTTP パイプラインを切り離して、多様なホスト シナリオを有効にするという点で、汎用ホストは Web ホストと異なります。 メッセージング、バックグラウンド タスク、その他の HTTP ワークロードで汎用ホストを使用し、構成、依存関係の挿入 (DI)、ログなどの横断的機能によるメリットを受けることができます。

ASP.NET Core 3.0 以降、汎用ホストは HTTP ワークロードと非 HTTP ワークロードの両方で推奨されます。 HTTP サーバー実装が含まれている場合、それは <xref:Microsoft.Extensions.Hosting.IHostedService> の実装として実行されます。 <xref:Microsoft.Extensions.Hosting.IHostedService> は、他のワークロードにも使用できるインターフェイスです。

Web ホストは Web アプリの推奨ホストではなくなりますが、下位互換性のために引き続き利用できます。

> [!NOTE]
> この記事の残りの部分は 3.0 向けに更新されていません。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core アプリはホストを構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。

この記事では、HTTP 要求を処理しないアプリに使用される、ASP.NET Core の汎用ホスト (<xref:Microsoft.Extensions.Hosting.HostBuilder>) について説明します。

汎用ホストの目的は、Web ホスト API から HTTP パイプラインを切り離して、多様なホスト シナリオを有効にすることです。 メッセージング、バックグラウンド タスク、汎用ホストに基づくその他の HTTP ワークロードに対して、構成、依存関係の挿入 (DI)、ログなどの横断的機能によるメリットがあります。

汎用ホストは ASP.NET Core 2.1 の新機能であり、Web ホスティングのシナリオには適していません。 Web ホスティングのシナリオの場合は、[Web ホスト](xref:fundamentals/host/web-host)を使ってください。 汎用ホストは将来のリリースで Web ホストに代わるものであり、HTTP と非 HTTP 両方のシナリオのプライマリ ホスト API として機能するようになります。

::: moniker-end

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

[Visual Studio Code](https://code.visualstudio.com/) でサンプル アプリを実行するときは、"*外部ターミナルまたは統合ターミナル*" を使います。 `internalConsole` ではサンプルを実行しないでください。

Visual Studio Code でコンソールを設定するには:

1. *.vscode/launch.json* ファイルを開きます。
1. **.NET Core Launch (console)** の構成で、**console** エントリを探します。 値を `externalTerminal` または `integratedTerminal` に設定します。

## <a name="introduction"></a>はじめに

汎用ホスト ライブラリは、<xref:Microsoft.Extensions.Hosting> 名前空間で使用でき、[Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) パッケージで提供されます。 `Microsoft.Extensions.Hosting` パッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます (ASP.NET Core 2.1 以降)。

<xref:Microsoft.Extensions.Hosting.IHostedService> がコード実行のエントリ ポイントです。 <xref:Microsoft.Extensions.Hosting.IHostedService> の各実装は、[ConfigureServices でのサービス登録](#configureservices)の順序で実行されます。 ホストが開始するときは <xref:Microsoft.Extensions.Hosting.IHostedService> ごとに <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> が呼び出され、ホストが正常に終了するときは登録と逆の順序で <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> が呼び出されます。

## <a name="set-up-a-host"></a>ホストを設定する

<xref:Microsoft.Extensions.Hosting.IHostBuilder> は、ライブラリとアプリがホストの初期化、ビルド、実行に使用するメイン コンポーネントです。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a>オプション

<xref:Microsoft.Extensions.Hosting.IHost> の <xref:Microsoft.Extensions.Hosting.HostOptions> 構成オプションです。

### <a name="shutdown-timeout"></a>シャットダウン タイムアウト

<xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> によって <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> のタイムアウトが設定されます。 既定値は 5 秒です。

次に示す `Program.Main` のオプション構成では、既定の 5 秒のシャットダウン タイムアウトが 20 秒に延長されます。

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a>既定のサービス

次のサービスは、ホストの初期化中に登録されます。

* [環境](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [構成](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [オプション](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [ログ](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>ホストの構成

ホストの構成は、次のようにして作成されます。

* <xref:Microsoft.Extensions.Hosting.IHostBuilder> で拡張メソッドを呼び出して、[コンテンツ ルート](#content-root)と[環境](#environment)を設定する。
* <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> の構成プロバイダーから構成を読み取る。

### <a name="extension-methods"></a>拡張メソッド

### <a name="application-key-name"></a>アプリケーション キー (名前)

[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) プロパティは、ホストの構築時にホストの構成から設定されます。 値を明示的に設定するには、[HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey) を使用します。

**キー**: applicationName  
**型**: *文字列*  
**既定値**:アプリのエントリ ポイントを含むアセンブリの名前。  
**次を使用して設定**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**環境変数**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configurehostconfiguration))

### <a name="content-root"></a>コンテンツ ルート

この設定では、ホストがコンテンツ ファイルの検索を開始する場所を決定します。

**キー**: contentRoot  
**型**: *文字列*  
**既定値**:既定でアプリ アセンブリが存在するフォルダーに設定されます。  
**次を使用して設定**: `UseContentRoot`  
**環境変数**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configurehostconfiguration))

パスが存在しない場合は、ホストを起動できません。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>環境

アプリの[環境](xref:fundamentals/environments)を設定します。

**キー**: 環境  
**型**: *文字列*  
**既定値**:実稼働  
**次を使用して設定**: `UseEnvironment`  
**環境変数**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configurehostconfiguration))

環境は任意の値に設定することができます。 フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。 値は大文字と小文字が区別されません。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> は <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用して、ホストの <xref:Microsoft.Extensions.Configuration.IConfiguration> を作成します。 ホストの構成は、<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> をアプリのビルド プロセスで使用するための初期化に使用されます。

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> を複数回呼び出して結果を追加できます。 ホストは、指定されたキーで最後に値を設定したオプションを使用します。

ホストの構成は自動的にアプリの構成に送られます ([ConfigureAppConfiguration](#configureappconfiguration) とアプリの残りの部分)。

既定ではプロバイダーが含まれていません。 次のような、アプリが <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> で必要とする構成プロバイダーを明示的に指定する必要があります。

* ファイルの構成 (*hostsettings.json* ファイルからなど)。
* 環境変数の構成。
* コマンドライン引数の構成。
* その他に必要なすべての構成プロバイダー。

ホストのファイル構成は、いずれかの[ファイル構成プロバイダー](xref:fundamentals/configuration/index#file-configuration-provider)に対する呼び出しの前に `SetBasePath` のアプリのベース パスを指定することで有効になります。 サンプル アプリは、JSON ファイル (*hostsettings.json*) を使用し、<xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> を呼び出して、ファイルのホスト構成設定を使用します。

ホストの[環境変数の構成](xref:fundamentals/configuration/index#environment-variables-configuration-provider)を追加するには、ホスト ビルダーで <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> を呼び出します。 `AddEnvironmentVariables` は、オプションのユーザー定義プレフィックスを受け入れます。 サンプル アプリは、`PREFIX_` のプレフィックスを使用します。 環境変数が読み取られると、プレフィックスは削除されます。 サンプル アプリのホストが構成されると、`PREFIX_ENVIRONMENT` の環境変数の値が `environment` キーのホスト構成値になります。

開発中に [Visual Studio](https://www.visualstudio.com/) を使用している、または `dotnet run` を使用してアプリを実行している場合は、環境変数を *Properties/launchSettings.json* ファイルに設定することができます。 [Visual Studio Code](https://code.visualstudio.com/) では、開発中に環境変数を *.vscode/launch.json* ファイルに設定することができます。 詳細については、「<xref:fundamentals/environments>」を参照してください。

[コマンドラインの構成](xref:fundamentals/configuration/index#command-line-configuration-provider)は、<xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>を呼び出すことで追加されます。 コマンドラインの構成は、コマンドライン引数を許可して以前の構成プロバイダーから提供された構成をオーバーライドするために最後に追加されます。

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

追加の構成は、[applicationName](#application-key-name) と [contentRoot](#content-root) キーで指定することができます。

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> を使う `HostBuilder` の構成の例:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

アプリの構成は、<xref:Microsoft.Extensions.Hosting.IHostBuilder> の実装で <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を呼び出すことで作成されます。 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> は <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用して、アプリの <xref:Microsoft.Extensions.Configuration.IConfiguration> を作成します。 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を複数回呼び出して結果を追加できます。 アプリは、指定されたキーで最後に値を設定したオプションを使用します。 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> によって作成された構成は、以降の操作に対する [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) において、および <xref:Microsoft.Extensions.Hosting.IHost.Services*>サービス内で使用できます。

アプリの構成は、[ConfigureHostConfiguration](#configurehostconfiguration) によって提供されたホストの構成を自動的に受信します。

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> を使うアプリ構成の例:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

設定ファイルを出力ディレクトリに移動するには、プロジェクト ファイルの設定ファイルを [MSBuild プロジェクト項目](/visualstudio/msbuild/common-msbuild-project-items) として指定します。 サンプル アプリはその JSON アプリ設定ファイルと、次の `<Content>` 項目を含む *hostsettings.json* を移動します。

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> は、アプリの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーにサービスを追加します。 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> を複数回呼び出して結果を追加できます。

ホストされるサービスは、<xref:Microsoft.Extensions.Hosting.IHostedService> インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。 詳細については、「<xref:fundamentals/host/hosted-services>」を参照してください。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)では、`AddHostedService` 拡張メソッドを使って、有効期間イベント `LifetimeEventsHostedService` と時刻指定付きバックグラウンド タスク `TimedHostedService` のサービスをアプリに追加します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> は、指定された <xref:Microsoft.Extensions.Logging.ILoggingBuilder> を構成するためのデリゲートを追加します。 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> を複数回呼び出して結果を追加できます。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> では、Ctrl + C/SIGINT または SIGTERM がリッスンされ、<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> が呼び出されてシャットダウン プロセスが開始されます。 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> は、[RunAsync](#runasync) や [WaitForShutdownAsync](#waitforshutdownasync) などの拡張機能のブロックを解除します。 <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> は、既定の有効期間の実装として事前に登録されています。 最後に登録された有効期間が使用されます。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>コンテナーの構成

他のコンテナーの接続をサポートするため、ホストは <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601> を受け入れることができます。 ファクトリの提供は、DI コンテナーの登録の一部ではなく、具象 DI コンテナーの作成に使用されるホストの組み込みです。 [UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) は、アプリのサービス プロバイダーの作成に使われる既定のファクトリをオーバーライドします。

カスタム コンテナーの構成は、<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> メソッドによって管理されます。 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> は、基盤のホスト API を基にしてコンテナーを構成するための、厳密に型指定されたエクスペリエンスを提供します。 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> を複数回呼び出して結果を追加できます。

アプリのサービス コンテナーを作成します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

サービス コンテナーのファクトリを提供します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

ファクトリを使って、アプリのカスタム サービス コンテナーを構成します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>機能拡張

ホスト拡張機能は、<xref:Microsoft.Extensions.Hosting.IHostBuilder> 上の拡張メソッドによって実行されます。 次の例では、拡張メソッドが <xref:fundamentals/host/hosted-services> で示される [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) の例を使って <xref:Microsoft.Extensions.Hosting.IHostBuilder> の実装を拡張する方法を示しています。

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

アプリは `UseHostedService` 拡張メソッドを確率して `T` に渡されたホステッド サービスを登録します。

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

## <a name="manage-the-host"></a>ホストを管理する

<xref:Microsoft.Extensions.Hosting.IHost> の実装は、サービス コンテナーに登録されている <xref:Microsoft.Extensions.Hosting.IHostedService> の実装の開始と停止を行います。

### <a name="run"></a>実行

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> はアプリを実行し、ホストがシャットダウンされるまで呼び出し元のスレッドをブロックします。

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

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> はアプリを実行し、キャンセル トークンまたはシャットダウンがトリガーされると完了する <xref:System.Threading.Tasks.Task> を返します。

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

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> は、コンソールのサポートを有効にし、ホストをビルドして開始した後、Ctrl + C/SIGINT または SIGTERM がシャットダウンするのを待機します。

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

### <a name="start-and-stopasync"></a>Start と StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> は、ホストを同期的に開始します。

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> は、指定されたタイムアウト内でホストの停止を試みます。

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

### <a name="startasync-and-stopasync"></a>StartAsync と StopAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> がアプリを起動します。

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> がアプリを停止します。

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

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> は、<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (Ctrl + C/SIGINT または SIGTERM をリッスンする) などの <xref:Microsoft.Extensions.Hosting.IHostLifetime> を介してトリガーされます。 <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> は <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> を呼び出します。

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

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> が返す <xref:System.Threading.Tasks.Task> は、提供されたトークンによってシャットダウンがトリガーされると完了し、<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> を呼び出します。

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

### <a name="external-control"></a>外部コントロール

ホストの外部コントロールは、外部から呼び出すことができるメソッドを使って実現できます。

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

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> は <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> の開始時に呼び出され、これが完了するまで待機してから続行します。 これを使って、外部イベントによって通知されるまで開始を遅らせることができます。

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment インターフェイス

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> は、アプリのホスティング環境に関する情報を提供します。 プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> を取得します。

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

詳細については、「<xref:fundamentals/environments>」を参照してください。

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime インターフェイス

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> は、正常なシャットダウンの要求など、起動後とシャットダウンのアクティビティに対応します。 インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する <xref:System.Action> メソッドを登録するために使用されるキャンセル トークンです。

| キャンセル トークン | トリガーのタイミング |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | ホストが完全に起動されたとき。 |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | ホストが正常なシャットダウンを完了しているとき。 すべての要求を処理する必要があります。 このイベントが完了するまで、シャットダウンはブロックされます。 |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | ホストが正常なシャットダウンを行っているとき。 要求がまだ処理されている可能性がある場合。 このイベントが完了するまで、シャットダウンはブロックされます。 |

コンストラクターは任意のクラスに <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> サービスを挿入します。 [サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)は、`LifetimeEventsHostedService` クラス (<xref:Microsoft.Extensions.Hosting.IHostedService> の実装) へのコンストラクターの挿入を使って、イベントを登録します。

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> は、アプリの終了を要求します。 以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> を使ってアプリを正常にシャットダウンします。

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

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/host/hosted-services>
