---
title: .NET での汎用ホスト
author: guardrex
description: アプリの起動と有効期間の管理を行う .NET の汎用ホストについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 40d297257895a4defeb89cef9c5ec6deea64a985
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033356"
---
# <a name="net-generic-host"></a>.NET での汎用ホスト

作成者: [Luke Latham](https://github.com/guardrex)

.NET アプリは*ホスト*を構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。 このトピックでは、HTTP 要求の処理を行わないアプリをホストするのに便利な、ASP.NET Core の汎用ホスト ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)) について説明します。 Web ホスト ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) については、[Web ホスト](xref:fundamentals/host/web-host)に関するトピックをご覧ください。

汎用ホストの目的は、Web ホスト API から HTTP パイプラインを切り離して、多様なホスト シナリオを有効にすることです。 メッセージング、バックグラウンド タスク、および汎用ホストに基づくその他の HTTP ワークロードに対して、構成、依存関係の挿入 (DI)、ログなどの横断的機能によるメリットがあります。

汎用ホストは ASP.NET Core 2.1 の新機能であり、Web ホスティングのシナリオには適していません。 Web ホスティングのシナリオの場合は、[Web ホスト](xref:fundamentals/host/web-host)を使ってください。 汎用ホストは開発中であり、将来のリリースでは Web ホストも汎用ホストに置き換えられて、汎用ホストが HTTP と非 HTTP 両方のシナリオのプライマリ ホスト API として機能するようになります。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

[Visual Studio Code](https://code.visualstudio.com/) でサンプル アプリを実行するときは、"*外部ターミナルまたは統合ターミナル*" を使います。 `internalConsole` ではサンプルを実行しないでください。

Visual Studio Code でコンソールを設定するには:

1. *.vscode/launch.json* ファイルを開きます。
1. **.NET Core Launch (console)** の構成で、**console** エントリを探します。 値を `externalTerminal` または `integratedTerminal` に設定します。

## <a name="introduction"></a>はじめに

汎用ホスト ライブラリは、[Microsoft.Extensions.Hosting 名前空間](/dotnet/api/microsoft.extensions.hosting)で使用でき、[Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) パッケージで提供されます。 `Microsoft.Extensions.Hosting` パッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれます (ASP.NET Core 2.1 以降)。

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) がコード実行のエントリ ポイントです。 `IHostedService` の各実装は、[ConfigureServices でのサービス登録](#configureservices)の順序で実行されます。 ホストが開始するときは `IHostedService` ごとに [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) が呼び出され、ホストが正常に終了するときは登録と逆の順序で [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) が呼び出されます。

## <a name="set-up-a-host"></a>ホストを設定する

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) は、ライブラリとアプリがホストの初期化、ビルド、実行に使用するメイン コンポーネントです。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>ホストの構成

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) は、以下の方法でホストの構成値を設定します。

* 構成ビルダー
* 拡張メソッドの構成

### <a name="configuration-builder"></a>構成ビルダー

ホスト ビルダーの構成は、[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) の実装上で [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) を呼び出すことによって作成します。 `ConfigureHostConfiguration` は、[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使ってホストの [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を作成します。 構成ビルダーは、アプリのビルド プロセスで使用するために [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) を初期化します。

環境変数の構成は、既定では追加されません。 環境変数からホストを構成するには、ホスト ビルダーで [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) を呼び出します。 `AddEnvironmentVariables` は、オプションのユーザー定義プレフィックスを受け入れます。 サンプル アプリは、`PREFIX_` のプレフィックスを使用します。 環境変数が読み取られると、プレフィックスは削除されます。 サンプル アプリのホストが構成されると、`PREFIX_ENVIRONMENT` の環境変数の値が `environment` キーのホスト構成値になります。

開発中に [Visual Studio](https://www.visualstudio.com/) を使用している、または `dotnet run` を使用してアプリを実行している場合は、環境変数を *Properties/launchSettings.json* ファイルに設定することができます。 [Visual Studio Code](https://code.visualstudio.com/) では、開発中に環境変数を *.vscode/launch.json* ファイルに設定することができます。 詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。

`ConfigureHostConfiguration` を複数回呼び出して結果を追加できます。 ホストは、最後に値を設定したオプションを使用します。

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

`ConfigureHostConfiguration` を使う `HostBuilder` の構成の例:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 拡張メソッドでは、現在、[GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (たとえば、`.AddConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。 `GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (`section:environment` など)。 `AddConfiguration` メソッドには、`HostBuilder` のキー (`environment` など) と一致するキーを渡す必要があります。 キーにセクション名があると、セクションの値でホストを構成できなくなります。 この問題は、将来のリリースで対処される予定です。 詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。

### <a name="extension-method-configuration"></a>拡張メソッドの構成

`IHostBuilder` の実装上で拡張メソッドを呼び出して、コンテンツ ルートと環境を構成します。

#### <a name="content-root"></a>コンテンツ ルート

この設定では、ホストがコンテンツ ファイルの検索を開始する場所を決定します。

**キー**: contentRoot  
**型**: *文字列*  
**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。  
**次を使用して設定**: `UseContentRoot`  
**環境変数**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))

パスが存在しない場合は、ホストを起動できません。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>環境

アプリの[環境](xref:fundamentals/environments)を設定します。

**キー**: 環境  
**型**: *文字列*  
**既定値**: Production  
**次を使用して設定**: `UseEnvironment`  
**環境変数**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` は[オプションであり、ユーザー定義です](#configuration-builder))

環境は任意の値に設定することができます。 フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。 値は大文字と小文字が区別されません。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

アプリ ビルダーの構成は、[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) の実装上で [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) を呼び出すことによって作成します。 `ConfigureAppConfiguration` は、[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使ってアプリの [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を作成します。 `ConfigureAppConfiguration` を複数回呼び出して結果を追加できます。 アプリは、最後に値を設定したオプションを使用します。 `ConfigureAppConfiguration` によって作成された構成は、以降の操作に対する [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) において、および [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services) 内で使用できます。

`ConfigureAppConfiguration` を使うアプリ構成の例:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 拡張メソッドでは、現在、[GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (たとえば、`.AddConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。 `GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (`section:Logging:LogLevel:Default` など)。 `AddConfiguration` メソッドでは、構成キーが完全に一致している必要があります (`Logging:LogLevel:Default` など)。 キーにセクション名があると、セクションの値でアプリを構成できなくなります。 この問題は、将来のリリースで対処される予定です。 詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) は、アプリの[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーにサービスを追加します。 `ConfigureServices` を複数回呼び出して結果を追加できます。

ホストされるサービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。 詳しくは、「[ASP.NET Core でホステッド サービスを使用するバックグラウンド タスク](xref:fundamentals/host/hosted-services)」をご覧ください。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)では、`AddHostedService` 拡張メソッドを使って、有効期間イベント `LifetimeEventsHostedService` と時刻指定付きバックグラウンド タスク `TimedHostedService` のサービスをアプリに追加します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) は、提供された [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) を構成するためのデリゲートを追加します。 `ConfigureLogging` を複数回呼び出して結果を追加できます。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) は、`Ctrl+C`/SIGINT または SIGTERM をリッスンし、[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) を呼び出してシャットダウン プロセスを開始します。 `UseConsoleLifetime` は、[RunAsync](#runasync) や [WaitForShutdownAsync](#waitforshutdownasync) などの拡張機能のブロックを解除します。 [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) は、既定の有効期間の実装として事前に登録されています。 最後に登録された有効期間が使用されます。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>コンテナーの構成

他のコンテナーの接続をサポートするため、ホストは [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1) を受け入れることができます。 ファクトリの提供は、DI コンテナーの登録の一部ではなく、具象 DI コンテナーの作成に使用されるホストの組み込みです。 [UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) は、アプリのサービス プロバイダーの作成に使われる既定のファクトリをオーバーライドします。

カスタム コンテナーの構成は、[ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) メソッドによって管理されます。 `ConfigureContainer` は、基盤のホスト API を基にしてコンテナーを構成するための、厳密に型指定されたエクスペリエンスを提供します。 `ConfigureContainer` を複数回呼び出して結果を追加できます。

アプリのサービス コンテナーを作成します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

サービス コンテナーのファクトリを提供します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

ファクトリを使って、アプリのカスタム サービス コンテナーを構成します。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>機能拡張

ホスト拡張機能は、`IHostBuilder` 上の拡張メソッドによって実行されます。 次の例では、拡張メソッドが [RabbitMQ](https://www.rabbitmq.com/) を使って `IHostBuilder` の実装を拡張する方法を示しています。 (アプリの別の場所にある) 拡張メソッドが RabbitMQ の `IHostedService` を登録します。

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>ホストを管理する

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) の実装は、サービス コンテナーに登録されている `IHostedService` の実装の開始と停止を行います。

### <a name="run"></a>実行

[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) はアプリを実行し、ホストがシャットダウンされるまで呼び出し元のスレッドをブロックします。

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

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) はアプリを実行し、キャンセル トークンまたはシャットダウンがトリガーされると完了する `Task` を返します。

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

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) は、コンソールのサポートを有効にし、ホストをビルドして開始した後、`Ctrl+C`/SIGINT または SIGTERM がシャットダウンするのを待機します。

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

[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) は、ホストを同期的に開始します。

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) は、指定されたタイムアウト内でホストの停止を試みます。

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

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) はアプリを開始します。

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) はアプリを停止します。

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

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) は、[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (`Ctrl+C`/SIGINT または SIGTERM をリッスンします) などの [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime) によってトリガーされます。 `WaitForShutdown` は [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) を呼び出します。

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

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) が返す `Task` は、提供されたトークンによってシャットダウンがトリガーされると完了し、[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) を呼び出します。

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) は [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) の開始時に呼び出されて、StartAsync が完了するまで待機してから続行します。 これを使って、外部イベントによって通知されるまで開始を遅らせることができます。

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment インターフェイス

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) は、アプリのホスティング環境に関する情報を提供します。 プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。

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

詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime インターフェイス

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) は、正常なシャットダウンの要求など、起動後とシャットダウンのアクティビティに対応します。 インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。

| キャンセル トークン | トリガーのタイミング |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | ホストが完全に起動されたとき。 |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | ホストが正常なシャットダウンを完了しているとき。 すべての要求を処理する必要があります。 このイベントが完了するまで、シャットダウンはブロックされます。 |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | ホストが正常なシャットダウンを行っているとき。 要求がまだ処理されている可能性がある場合。 このイベントが完了するまで、シャットダウンはブロックされます。 |

コンストラクターは任意のクラスに `IApplicationLifetime` サービスを挿入します。 [サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)は、`LifetimeEventsHostedService` クラス (`IHostedService` の実装) へのコンストラクターの挿入を使って、イベントを登録します。

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) は、アプリの終了を要求します。 以下のクラスは、クラスの `Shutdown` メソッドが呼び出されると、`StopApplication` を使ってアプリを正常にシャットダウンします。

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

* [ホストされるサービスを使用するバックグラウンド タスク](xref:fundamentals/host/hosted-services)
* [GitHub のホスティング リポジトリ サンプル](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
