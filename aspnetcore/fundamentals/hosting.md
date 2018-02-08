---
title: "ASP.NET Core でのホスティング"
author: guardrex
description: "ASP.NET Core アプリの Web ホスト (アプリの起動と有効期間の管理を担当する) について説明します。"
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 78209c8d34fa1a2a164ae333d625feca1e145e89
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="hosting-in-aspnet-core"></a>ASP.NET Core でのホスティング

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core アプリは*ホスト*を構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。 少なくとも、ホストはサーバーおよび要求処理パイプラインを構成します。

## <a name="setting-up-a-host"></a>ホストの設定

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。 通常、これはアプリのエントリ ポイントの `Main` メソッドで実行されます。 プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。 一般的な *Program.cs* では、次のように [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder` では次のタスクを実行します。

* [Kestrel](servers/kestrel.md) を Web サーバーとして構成します。 Kestrel の既定のオプションについては、[「ASP.NET Core の Kestrel Web サーバーの実装の概要」の「Kestrel オプション」セクション](xref:fundamentals/servers/kestrel#kestrel-options)を参照してください。
* [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) によって返されるパスにコンテンツ ルートを設定します。
* 次の場所から省略可能な構成を読み込みます。
  * *appsettings.json*。
  * *appsettings.{Environment}.json*。
  * `Development` 環境でアプリが実行される場合に使用される[ユーザー シークレット](xref:security/app-secrets)。
  * 環境変数。
  * コマンド ライン引数。
* コンソールとデバッグ出力の[ログ](xref:fundamentals/logging/index)を構成します。 ログには、*appsettings.json* または *appsettings.{Environment}.json* ファイルのログ構成セクションで指定される[ログ フィルター](xref:fundamentals/logging/index#log-filtering)規則が含まれます。
* IIS の背後での実行時に、[IIS 統合](xref:host-and-deploy/iis/index)を有効にします。 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)の使用時にサーバーがリッスンする基本パスとポートを構成します。 このモジュールは、IIS と Kestrel の間にリバース プロキシを作成します。 また、[スタートアップ エラーをキャプチャする](#capture-startup-errors)ようにアプリを構成します。 IIS の既定のオプションについては、[「IIS を使用した Windows での ASP.NET Core のホスト」の「IIS のオプション」セクション](xref:host-and-deploy/iis/index#iis-options)を参照してください。

*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。 プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。 これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。

アプリの構成の詳細については、「[ASP.NET Core の構成](xref:fundamentals/configuration/index)」を参照してください。

> [!NOTE]
> 静的な `CreateDefaultBuilder` メソッドを使用する代わりに、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) からホストを作成する方法が ASP.NET Core 2.x でサポートされています。 詳細については、ASP.NET Core 1.x のタブを参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) のインスタンスを使用して、ホストを作成します。 通常、ホストの作成はアプリのエントリ ポイントの `Main` メソッドで実行されます。 プロジェクト テンプレートでは、`Main` は *Program.cs* にあります。

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder` には、[IServer を実装するサーバー](servers/index.md)が必要です。 組み込みサーバーは [Kestrel](servers/kestrel.md) と [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 より前のリリースでは [WebListener](xref:fundamentals/servers/weblistener) と呼ばれていた) です。 この例では、[UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)で Kestrel サーバーを指定します。

*コンテンツ ルート*で、ホストが MVC ビュー ファイルなどのコンテンツ ファイルを検索する場所を決定します。 `UseContentRoot` の既定のコンテンツ ルートは、[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) によって取得されます。 プロジェクトのルート フォルダーからアプリが開始された場合は、そのプロジェクトのルート フォルダーがコンテンツ ルートとして使用されます。 これは、[Visual Studio](https://www.visualstudio.com/) と [dotnet の新しいテンプレート](/dotnet/core/tools/dotnet-new)で使用される既定です。

IIS をリバース プロキシとして使用するには、ホストのビルド時に [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) を呼び出します。 `UseIISIntegration` では、[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) の場合のように*サーバー*は構成されません。 `UseIISIntegration` は、Kestrel と IIS の間にリバース プロキシを作成するために [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を使用する際にサーバーがリッスンする基本パスとポートを構成します。 IIS と ASP.NET Core を一緒に使用するには、`UseKestrel` と `UseIISIntegration` を指定する必要があります。 `UseIISIntegration` は、IIS または IIS Express の背後で実行されている場合にのみアクティブになります。 詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。

ホスト (および ASP.NET Core アプリ) を構成する最小限の実装には、アプリの要求パイプラインの構成とサーバーの指定が含まれます。

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

ホストの構成時に、[Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) および [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) メソッドを指摘することができます。 `Startup` クラスが指定されている場合は、`Configure` メソッドを定義する必要があります。 詳細については、「[ASP.NET Core でのアプリケーションのスタートアップ](startup.md)」を参照してください。 `ConfigureServices` の複数回の呼び出しでは、互いに追加されます。 `WebHostBuilder` での `Configure` または `UseStartup` の複数回の呼び出しでは、以前の設定が置き換えられます。

## <a name="host-configuration-values"></a>ホストの構成値

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) では、ホストの構成値を設定する場合に以下の方法に依存します。

* `ASPNETCORE_{configurationKey}` 形式の環境変数を含む、ホスト ビルダー構成。 たとえば、`ASPNETCORE_URLS` のようにします。
* `CaptureStartupErrors` などの明示的なメソッド。
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) と関連するキー。 `UseSetting` で値を設定すると、値はその型に関係なく、文字列として設定されます。

ホストは、最後に値を設定したオプションを使用します。 詳細については、次のセクションの「[構成のオーバーライド](#overriding-configuration)」を参照してください。

### <a name="capture-startup-errors"></a>スタートアップ エラーをキャプチャする

この設定では、スタートアップ エラーのキャプチャを制御します。

**キー**: captureStartupErrors  
**型**: *ブール* (`true` または `1`)  
**既定値**: アプリが IIS の背後で Kestrel を使用して実行されている場合 (既定値は `true`) を除き、既定では `false` に設定されます。  
**次を使用して設定**: `CaptureStartupErrors`  
**環境変数**: `ASPNETCORE_CAPTURESTARTUPERRORS`

`false` の場合、起動時にエラーが発生するとホストが終了します。 `true` の場合、ホストは起動時に例外をキャプチャして、サーバーを起動しようとします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>コンテンツ ルート

この設定では、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始する場所を決定します。 

**キー**: contentRoot  
**型**: *文字列*  
**既定値**: 既定でアプリ アセンブリが存在するフォルダーに設定されます。  
**次を使用して設定**: `UseContentRoot`  
**環境変数**: `ASPNETCORE_CONTENTROOT`

コンテンツ ルートは、[Web ルート設定](#web-root)の基本パスとしても使用されます。 パスが存在しない場合は、ホストを起動できません。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>詳細なエラー

詳細なエラーをキャプチャするかどうかを判断します。

**キー**: detailedErrors  
**型**: *ブール* (`true` または `1`)  
**既定値**: false  
**次を使用して設定**: `UseSetting`  
**環境変数**: `ASPNETCORE_DETAILEDERRORS`

有効な場合 (または<a href="#environment">環境</a>が `Development` に設定されている場合)、アプリは詳細な例外をキャプチャします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>環境

アプリの環境を設定します。

**キー**: 環境  
**型**: *文字列*  
**既定値**: Production  
**次を使用して設定**: `UseEnvironment`  
**環境変数**: `ASPNETCORE_ENVIRONMENT`

環境は任意の値に設定することができます。 フレームワークで定義された値には `Development`、`Staging`、`Production` が含まれます。 値は大文字と小文字が区別されません。 既定では、*環境*は `ASPNETCORE_ENVIRONMENT` 環境変数から読み取られます。 [Visual Studio](https://www.visualstudio.com/) を使用する場合、環境変数は *launchSettings.json* ファイルで設定できます。 詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>ホスティング スタートアップ アセンブリ

アプリのホスティング スタートアップ アセンブリを設定します。

**キー**: hostingStartupAssemblies  
**型**: *文字列*  
**既定値**: 空の文字列  
**次を使用して設定**: `UseSetting`  
**環境変数**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

起動時に読み込むホスティング スタートアップ アセンブリのセミコロンで区切られた文字列。 これは ASP.NET Core 2.0 の新機能です。

構成値は既定で空の文字列に設定されますが、ホスティング スタートアップ アセンブリには常にアプリのアセンブリが含まれます。 ホスティング スタートアップ アセンブリが提供されている場合、アプリが起動中に共通サービスをビルドしたときに読み込むためにアプリのアセンブリに追加されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能は ASP.NET Core 1.x では使用できません。

---

### <a name="prefer-hosting-urls"></a>ホスティング URL を優先する

`IServer` 実装で構成されているものではなく、`WebHostBuilder` で構成されている URL でホストがリッスンするかどうかを示します。

**キー**: preferHostingUrls  
**型**: *ブール* (`true` または `1`)  
**既定値**: true  
**次を使用して設定**: `PreferHostingUrls`  
**環境変数**: `ASPNETCORE_PREFERHOSTINGURLS`

これは ASP.NET Core 2.0 の新機能です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能は ASP.NET Core 1.x では使用できません。

---

### <a name="prevent-hosting-startup"></a>ホスティング スタートアップを回避する

アプリのアセンブリで構成されているホスティング スタートアップ アセンブリを含む、ホスティング スタートアップ アセンブリの自動読み込みを回避します。 詳細については、「[IHostingStartup を使用した外部アセンブリからのアプリ機能の追加](xref:host-and-deploy/ihostingstartup)」を参照してください。

**キー**: preventHostingStartup  
**型**: *ブール* (`true` または `1`)  
**既定値**: false  
**次を使用して設定**: `UseSetting`  
**環境変数**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

これは ASP.NET Core 2.0 の新機能です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能は ASP.NET Core 1.x では使用できません。

---

### <a name="server-urls"></a>サーバーの URL

サーバーが要求をリッスンする必要があるポートとプロトコルを使用して、IP アドレスまたはホスト アドレスを示します。

**キー**: urls  
**型**: *文字列*  
**既定値**: http://localhost:5000  
**次を使用して設定**: `UseUrls`  
**環境変数**: `ASPNETCORE_URLS`

サーバーが応答する必要がある URL プレフィックスのセミコロン (;) で区切られたリストに設定します。 たとえば、`http://localhost:123` のようにします。 "\*" を使用し、サーバーが指定されたポートとプロトコル (`http://*:5000` など) を使用して IP アドレスまたはホスト名に関する要求をリッスンする必要があることを示します。 プロトコル (`http://` または `https://`) は各 URL に含める必要があります。 サポートされている形式はサーバー間で異なります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel には独自のエンドポイント構成 API があります。 詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>シャットダウン タイムアウト

Web ホストがシャットダウンするまで待機する時間を指定します。

**キー**: shutdownTimeoutSeconds  
**型**: *int*  
**既定値**: 5  
**次を使用して設定**: `UseShutdownTimeout`  
**環境変数**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

キーでは `UseSetting` が指定された *int* (`.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")` など) を受け入れますが、`UseShutdownTimeout` 拡張メソッドでは `TimeSpan` を受け取ります。 これは ASP.NET Core 2.0 の新機能です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能は ASP.NET Core 1.x では使用できません。

---

### <a name="startup-assembly"></a>スタートアップ アセンブリ

`Startup` クラスを検索するアセンブリを決定します。

**キー**: startupAssembly  
**型**: *文字列*  
**既定値**: アプリのアセンブリ  
**次を使用して設定**: `UseStartup`  
**環境変数**: `ASPNETCORE_STARTUPASSEMBLY`

名前 (`string`) または型 (`TStartup`) 別のアセンブリを参照できます。 複数の `UseStartup` メソッドが呼び出された場合は、最後のメソッドが優先されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

### <a name="web-root"></a>Web ルート

アプリの静的資産への相対パスを設定します。

**キー**: webroot  
**型**: *文字列*  
**既定値**: 指定されていない場合、既定値は "(Content Root)/wwwroot" となります (パスが存在する場合)。 パスが存在しない場合は、no-op ファイル プロバイダーが使用されます。  
**次を使用して設定**: `UseWebRoot`  
**環境変数**: `ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>構成のオーバーライド

[Configuration](xref:fundamentals/configuration/index) を使用して、ホストを構成します。 次の例では、ホスト構成はオプションで *hosting.json* ファイルに指定されます。 *hosting.json* ファイルから読み込まれた構成は、コマンド ライン引数でオーバーライドされる場合があります。 (`config` の) ビルド構成は、`UseConfiguration` でホストを構成する場合に使用されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

次のように、`UseUrls` で指定された構成をオーバーライドします (最初の構成は *hosting.json*、2 番目の構成はコマンドライン引数です)。

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
> [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 拡張メソッドでは、現在、`GetSection` (たとえば、`.UseConfiguration(Configuration.GetSection("section"))` によって返される構成セクションを解析することはできません。 `GetSection` メソッドは要求されたセクションに対する構成キーをフィルター処理しますが、キーのセクション名はそのままになります (たとえば、`section:urls`、`section:environment`)。 `UseConfiguration` メソッドは `WebHostBuilder` と一致するキーを予測します (たとえば、`urls`、`environment`)。 キーにセクション名があると、セクションの値でホストを構成できなくなります。 この問題は、将来のリリースで対処される予定です。 詳細と回避策については、「[Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839)」 (WebHostBuilder.UseConfiguration に構成セクションを渡すときにフル キーを使用する) を参照してください。

特定の URL でホストを実行するように指定する場合、`dotnet run` の実行時にコマンド プロンプトから必要な値を渡すことができます。 コマンドライン引数は *hosting.json* ファイルからの `urls` 値をオーバーライドし、サーバーはポート 8080 でリッスンします。

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>ホストの起動

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**実行**

`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。

```csharp
host.Run();
```

**Start**

`Start` メソッドを呼び出して、ブロックせずにホストを実行します。

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。

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

アプリは、便利な静的メソッドを使用し、`CreateDefaultBuilder` の構成済みの既定値を使用して、新しいホストを初期化して起動できます。 これらのメソッドは、コンソール出力なしでサーバーを起動します。その場合、[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) を指定し、中断される (Ctrl-C/SIGINT または SIGTERM) まで待機します。

**Start(RequestDelegate app)**

次のように `RequestDelegate` で始まります。

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。 `WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。 アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。

**Start(string url, RequestDelegate app)**

次のように、URL と `RequestDelegate` で始まります。

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

アプリが `http://localhost:8080` で応答する場合を除き、**Start(RequestDelegate app)** と同じ結果が生成されます。

**Start(Action<IRouteBuilder> routeBuilder)**

次のように、`IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) のインスタンスを使用してルーティング ミドルウェアを使用します。

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

この例では、以下のブラウザー要求を使用します。

| 要求                                    | 応答                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | "ooops!" という文字列がある場合は例外をスローします。 |
| `http://localhost:5000/throw`              | "Uh oh!" という文字列がある場合は例外をスローします。 |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。 アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。

**Start(string url, Action<IRouteBuilder> routeBuilder)**

次のように、URL と `IRouteBuilder` のインスタンスを使用します。

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

アプリが `http://localhost:8080` で応答する場合を除き、**Start(Action<IRouteBuilder> routeBuilder)** と同じ結果が生成されます。

**StartWith(Action<IApplicationBuilder> app)**

次のように、デリゲートを指定して `IApplicationBuilder` を構成します。

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

"Hello World!" という応答を受け取るには、ブラウザで `http://localhost:5000` に対する要求を行います。 `WaitForShutdown` は、中断される (Ctrl-C/SIGINT または SIGTERM) までブロックします。 アプリは `Console.WriteLine` メッセージを表示し、キー入力が終わるまで待機します。

**StartWith(string url, Action<IApplicationBuilder> app)**

次のように、URL とデリゲートを指定して `IApplicationBuilder` を構成します。

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

アプリが `http://localhost:8080` で応答する場合を除き、**StartWith(Action<IApplicationBuilder> app)** と同じ結果が生成されます。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**実行**

`Run` メソッドは Web アプリを起動し、ホストがシャットダウンするまで呼び出し元のスレッドをブロックします。

```csharp
host.Run();
```

**Start**

`Start` メソッドを呼び出して、ブロックせずにホストを実行します。

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

URL のリストが `Start` メソッドに渡された場合、指定された URL でリッスンします。


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

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment インターフェイス

[IHostingEnvironment インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)では、アプリの Web ホスティング環境に関する情報を提供します。 プロパティと拡張メソッドを使用するには、[コンストラクターの挿入](xref:fundamentals/dependency-injection)を使用して `IHostingEnvironment` を取得します。

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

[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)を使用して、環境に基づいて起動時にアプリを構成することができます。 あるいは、次のように `ConfigureServices` で使用するために `IHostingEnvironment` を `Startup` コンストラクターに挿入します。

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
> `IsDevelopment` 拡張メソッドに加え、`IHostingEnvironment` は `IsStaging`、`IsProduction`、`IsEnvironment(string environmentName)` メソッドを提供します。 詳細については、「[複数の環境の使用](xref:fundamentals/environments)」を参照してください。

処理パイプラインを設定するために、次のように `IHostingEnvironment` サービスを `Configure` メソッドに直接挿入することもできます。

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

カスタムの[ミドルウェア](xref:fundamentals/middleware#writing-middleware)を作成する際に、次のように `IHostingEnvironment` を `Invoke` メソッドに挿入することができます。

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

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime インターフェイス

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) は、起動後とシャットダウンのアクティビティに適用できます。 インターフェイスの 3 つのプロパティは、起動とシャットダウン イベントを定義する `Action` メソッドを登録するために使用されるキャンセル トークンです。 `StopApplication` メソッドもあります。

| キャンセル トークン    | トリガーのタイミング |
| --------------------- | --------------------- |
| `ApplicationStarted`  | ホストが完全に起動されたとき。 |
| `ApplicationStopping` | ホストが正常なシャットダウンを行っているとき。 要求がまだ処理されている可能性がある場合。 このイベントが完了するまで、シャットダウンはブロックされます。 |
| `ApplicationStopped`  | ホストが正常なシャットダウンを完了しているとき。 すべての要求を処理する必要があります。 このイベントが完了するまで、シャットダウンはブロックされます。 |

| メソッド            | アクション                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | 現在のアプリケーションの終了を要求します。 |

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

## <a name="troubleshooting-systemargumentexception"></a>System.ArgumentException のトラブルシューティング

**適用対象: ASP.NET Core 2.0 のみ**

`UseStartup` や `Configure` を呼び出すのではなく、依存関係挿入コンテナーに直接 `IStartup` を挿入して、ホストをビルドする場合があります。

```csharp
services.AddSingleton<IStartup, Startup>();
```

ホストがこのようにビルドされている場合、以下のエラーが発生することがあります。

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

これは、`HostingStartupAttributes` をスキャンするために [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (現在のアセンブリ) が必要であるために発生します。 アプリで `IStartup` を手動で依存関係挿入コンテナーに挿入する場合は、指定されたアセンブリ名を使用して `WebHostBuilder` への次の呼び出しを追加します。

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

あるいは、ダミーの `Configure` を `WebHostBuilder` に追加します。この場合、`applicationName`(`ApplicationKey`) が自動的に設定されます。

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**注**: これは、ASP.NET Core 2.0 リリースでのみ、およびアプリが `UseStartup` や `Configure` を呼び出さない場合にのみ必要になります。

詳細については、[お知らせのコメント「Microsoft.Extensions.PlatformAbstractions has been removed」](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Microsoft.Extensions.PlatformAbstractions が削除される) と [StartupInjection のサンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)を参照してください。

## <a name="additional-resources"></a>その他の技術情報

* [IIS による Windows 上のホスト](xref:host-and-deploy/iis/index)
* [Nginx による Linux でのホスト](xref:host-and-deploy/linux-nginx)
* [Apache による Linux でのホスト](xref:host-and-deploy/linux-apache)
* [Windows サービスでのホスト](xref:host-and-deploy/windows-service)
