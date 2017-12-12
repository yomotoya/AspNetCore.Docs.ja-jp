---
title: "ASP.NET Core でのホスティング"
author: guardrex
description: "これは、アプリの起動と有効期間の管理を担当する ASP.NET Core の web ホストについて説明します。"
keywords: "ASP.NET Core web ホスト、IWebHost、WebHostBuilder、IHostingEnvironment、IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7deccf135ddd21729206ebed58ddc8aca52c1deb
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="hosting-in-aspnet-core"></a>ASP.NET Core でのホスティング

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core アプリは、アプリの起動と有効期間の管理を担当する*ホスト*を構成して起動します。 少なくともは、ホストは、サーバーおよび要求処理パイプラインを構成します。

## <a name="setting-up-a-host"></a>ホストの設定

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。 これは、アプリのエントリ ポイントで通常実行、`Main`メソッドです。 プロジェクト テンプレートで`Main`内にある*Program.cs*です。 一般的な*Program.cs*呼び出し[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)ホストの設定を開始します。

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`次のタスクを実行します。

* 構成[Kestrel](servers/kestrel.md) web サーバーとします。 Kestrel 既定のオプションを参照してください。 [Kestrel ASP.NET Core での web サーバーの実装のセクションで [オプション]、Kestrel](xref:fundamentals/servers/kestrel#kestrel-options)です。
* コンテンツのルートに設定[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)です。
* 読み込みオプションの構成:
  * *される appsettings.json*です。
  * *appsettings です。{環境} .json*です。
  * [ユーザーの機密情報](xref:security/app-secrets)アプリを実行するときに、`Development`環境。
  * 環境変数。
  * コマンドライン引数。
* 構成[ログ](xref:fundamentals/logging/index)コンソールとデバッグ出力の[ログ フィルター](xref:fundamentals/logging/index#log-filtering)のログ記録の構成セクションで指定された規則、*される appsettings.json*または*appsettings です。{環境} .json*ファイル。
* により、IIS の背後にある実行中、 [IIS 統合](xref:publishing/iis)のベース パスおよびポートを構成することによって、サーバーがリッスンを使用する場合、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)です。 モジュールでは、IIS と Kestrel リバース プロキシを作成します。 アプリの構成も行います[スタートアップ エラーがキャプチャされる](#capture-startup-errors)です。 IIS の既定のオプションを参照してください。 [、IIS が IIS と Windows 上のホスト ASP.NET Core のセクションをオプション](xref:publishing/iis#iis-options)です。

*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。 既定のコンテンツのルートは[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)です。 これは、結果、ルート フォルダーから、アプリが開始されたときに、コンテンツのルートとして、web プロジェクトのルート フォルダーを使用して、(たとえば、呼び出し[実行 dotnet](/dotnet/core/tools/dotnet-run)プロジェクト フォルダーから)。 これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。

参照してください[ASP.NET Core の構成](xref:fundamentals/configuration/index)アプリの構成についての詳細。

> [!NOTE]
> 静的なを使用する代わりとして`CreateDefaultBuilder`からホストを作成するメソッド[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core でサポートされているアプローチは、2.x です。 詳細については、ASP.NET Core 1.x タブを参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

インスタンスを使用してホストを作成[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)です。 これは、アプリのエントリ ポイントで通常実行、`Main`メソッドです。 プロジェクト テンプレートで`Main`内にある*Program.cs*です。 次*Program.cs*を使用する方法を示します`WebHostBuilder`ホストを作成します。

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`必要があります、 [IServer を実装しているサーバー](servers/index.md)です。 組み込みのサーバーが[Kestrel](servers/kestrel.md)と[HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 のリリースでは、前に、HTTP.sys が呼び出された[WebListener](xref:fundamentals/servers/weblistener))。 この例では、 [UseKestrel 拡張メソッド](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)Kestrel サーバーを指定します。

*コンテンツのルート*MVC ビュー ファイルなどのコンテンツ ファイルのホストを検索する場所を決定します。 指定された既定のコンテンツのルート`UseContentRoot`は[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1)です。 これは、結果、ルート フォルダーから、アプリが開始されたときに、コンテンツのルートとして、web プロジェクトのルート フォルダーを使用して、(たとえば、呼び出し[実行 dotnet](/dotnet/core/tools/dotnet-run)プロジェクト フォルダーから)。 これは、既定で使用される[Visual Studio](https://www.visualstudio.com/)と[dotnet 新しいテンプレート](/dotnet/core/tools/dotnet-new)です。

リバース プロキシとして、IIS を使用するには、呼び出す[UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)ホストの作成の一部として。 `UseIISIntegration`構成されない、*サーバー*と同様、 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)はします。 `UseIISIntegration`基本パスとを使用する場合、サーバーがリッスンするポートを構成、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)Kestrel と IIS のリバース プロキシを作成します。 ASP.NET Core で IIS を使用して、両方を指定する必要があります`UseKestrel`と`UseIISIntegration`です。 `UseIISIntegration`IIS または IIS Express の内側で実行されるときにのみアクティブにします。 詳細については、次を参照してください。 [Introduction to ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)と[ASP.NET Core モジュール構成の参照](xref:hosting/aspnet-core-module)です。

ホスト (および ASP.NET Core アプリケーション) を構成する最小限の実装には、サーバーおよびアプリケーションの要求パイプラインの構成の指定が含まれます。

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

ホストをセットアップするときに使用できる[構成](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)と[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1)メソッドです。 指定した場合、`Startup`クラスを定義する必要がありますが、`Configure`メソッドです。 詳細については、次を参照してください。[で ASP.NET Core アプリケーションの起動](startup.md)です。 複数回呼び出す`ConfigureServices`互いに追加します。 複数回呼び出す`Configure`または`UseStartup`上、`WebHostBuilder`以前の設定を置き換えます。

## <a name="host-configuration-values"></a>ホストの構成値

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)を直接設定することもできます、ホストでは、ほとんどの利用可能な構成値を設定するためのメソッドを提供[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting)と関連するキー。 使用して値を設定するときに`UseSetting`種類にかかわらず、(引用符で囲まれた) 文字列として値を設定します。

### <a name="capture-startup-errors"></a>スタートアップ エラーがキャプチャします。

この設定は、スタートアップのエラーのキャプチャを制御します。

**キー**: captureStartupErrors  
**型**: *bool* (`true`または`1`)  
**既定**: 既定値は`false`ここで、既定値は、IIS の背後にある Kestrel でアプリを実行する場合を除き、`true`です。  
**使用して設定**:`CaptureStartupErrors`

ときに`false`起動の結果、終了ホスト中のエラーです。 ときに`true`ホストが起動中に例外をキャプチャし、サーバーを起動しようとしています。

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

### <a name="content-root"></a>コンテンツのルート

この設定は、ASP.NET Core が MVC ビューなどのコンテンツ ファイルの検索を開始位置を決定します。 

**キー**: contentRoot  
**型**:*文字列*  
**既定の**: アプリのアセンブリが存在するフォルダーの既定値です。  
**使用して設定**:`UseContentRoot`

コンテンツのルートはのベース パスとしても使用される、 [Web ルート設定](#web-root)です。 パスが存在しない場合、ホストは、開始に失敗します。

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

エラーをキャプチャする必要がある場合を判断します。

**キー**: detailedErrors  
**型**: *bool* (`true`または`1`)  
**既定の**: false  
**使用して設定**:`UseSetting`

有効にすると (場合や、<a href="#environment">環境</a>に設定されている`Development`)、アプリは、詳細な例外をキャプチャします。

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
**型**:*文字列*  
**既定の**: 実稼働環境  
**使用して設定**:`UseEnvironment`

設定することができます、*環境*任意の値にします。 フレームワークによって定義された値が含まれます`Development`、 `Staging`、および`Production`です。 値は、大文字と小文字が区別されません。 既定では、*環境*から読み取られた、`ASPNETCORE_ENVIRONMENT`環境変数。 使用する場合[Visual Studio](https://www.visualstudio.com/)環境変数を設定することがあります、 *launchSettings.json*ファイル。 詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。

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

### <a name="hosting-startup-assemblies"></a>ホストしているスタートアップ アセンブリ

アプリのホスティング スタートアップ アセンブリを設定します。

**キー**: hostingStartupAssemblies  
**型**:*文字列*  
**既定の**: 空の文字列  
**使用して設定**:`UseSetting`

起動時にロードするスタートアップ アセンブリをホストしているセミコロンで区切られた文字列。 この機能は ASP.NET Core 2.0 の新機能です。

既定の構成値は、空の文字列、ホスティング スタートアップ アセンブリは常に、アプリのアセンブリを含めます。 スタートアップ アセンブリのホスティングを提供する場合は、アプリが起動中に、一般的なサービスを構築するときに読み込むためのアプリのアセンブリに追加されます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能では使用できません ASP.NET Core 1.x です。

---

### <a name="prefer-hosting-urls"></a>Url のホストを優先します。

構成されている Url に、ホストがリッスンするかどうかを示す、`WebHostBuilder`で構成されているものではなく、`IServer`実装します。

**キー**: preferHostingUrls  
**型**: *bool* (`true`または`1`)  
**既定の**: true  
**使用して設定**:`PreferHostingUrls`

この機能は ASP.NET Core 2.0 の新機能です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能では使用できません ASP.NET Core 1.x です。

---

### <a name="prevent-hosting-startup"></a>スタートアップをホストしているようにします。

ホストしているアプリのアセンブリを含め、スタートアップ アセンブリの自動読み込みができなくなります。

**キー**: preventHostingStartup  
**型**: *bool* (`true`または`1`)  
**既定の**: false  
**使用して設定**:`UseSetting`

この機能は ASP.NET Core 2.0 の新機能です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能では使用できません ASP.NET Core 1.x です。

---

### <a name="server-urls"></a>サーバーの Url

IP アドレスまたはサーバーが要求をリッスンするポートとプロトコルとなるホスト アドレスを示します。

**キー**: url  
**型**:*文字列*  
**既定の**: http://localhost:5000  
**使用して設定**:`UseUrls`

セミコロンで区切られたに設定 (;) の URL の一覧のプレフィックスに、サーバーが応答する必要があります。 たとえば、`http://localhost:123` のようにします。 使用して"\*"を示す、サーバーは、上の IP アドレスまたはホスト名の指定したポートとプロトコルを使用して要求をリッスンする必要があります (たとえば、 `http://*:5000`)。 プロトコル (`http://`または`https://`) 各 url を含める必要があります。 サポートされている形式は、サーバー間で異なります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel には、独自のエンドポイント構成 API があります。 詳細については、「[Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)」 (ASP.NET Core への Kestrel Web サーバーの実装) を参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>シャット ダウン タイムアウト

Web ホストのシャット ダウンを待機する時間を指定します。

**キー**: shutdownTimeoutSeconds  
**型**: *int*  
**既定の**: 5  
**使用して設定**:`UseShutdownTimeout`

キーを受け入れますが、 *int*で`UseSetting`(たとえば、 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) では、`UseShutdownTimeout`拡張メソッドは、`TimeSpan`です。 この機能は ASP.NET Core 2.0 の新機能です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

この機能では使用できません ASP.NET Core 1.x です。

---

### <a name="startup-assembly"></a>スタートアップ アセンブリ

検索対象となるアセンブリの決定、`Startup`クラスです。

**キー**: startupAssembly  
**型**:*文字列*  
**既定の**: アプリのアセンブリ  
**使用して設定**:`UseStartup`

名前でアセンブリを参照することができます (`string`) または型 (`TStartup`)。 複数`UseStartup`メソッドが呼び出されると、最後の 1 つが優先されます。

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

アプリの静的な資産への相対パスを設定します。

**キー**: webroot  
**型**:*文字列*  
**既定**: が指定されていない、既定値は"(Content Root) wwwroot/"パスが存在する場合は、します。 パスが存在しない、no-op ファイル プロバイダーが使用されます。  
**使用して設定**:`UseWebRoot`

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

## <a name="overriding-configuration"></a>構成をオーバーライドします。

使用して[構成](xref:fundamentals/configuration/index)ホストを構成します。 次の例では、ホストの構成は必要に応じてで指定された、 *hosting.json*ファイル。 すべての構成から読み込まれました、 *hosting.json*ファイルはコマンドライン引数によってオーバーライドされる可能性があります。 ビルドの構成 (で`config`) でホストを構成するために使用`UseConfiguration`です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。

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

によって提供される構成をオーバーライドする`UseUrls`で*hosting.json* config 最初、コマンドラインの引数の構成 2。

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
> `UseConfiguration`拡張メソッドは現在によって返される構成セクションを解析できる`GetSection`(たとえば、`.UseConfiguration(Configuration.GetSection("section"))`です。 `GetSection`メソッド要求のセクションに構成キーをフィルター処理が残りますセクションの名前のキーに (たとえば、 `section:urls`、 `section:environment`)。 `UseConfiguration`メソッドに一致するキーが必要ですが、`WebHostBuilder`キー (たとえば、 `urls`、 `environment`)。 キーにセクション名の有無は、セクションの値が、ホストを構成することを防止します。 この問題は、将来のリリースで対処される予定です。 詳細と回避策については、次を参照してください。[フル キーを使用して WebHostBuilder.UseConfiguration に構成セクションを渡して](https://github.com/aspnet/Hosting/issues/839)です。

特定の URL で実行するホストを指定するには、でしたに合格する目的の値で、コマンド プロンプトから実行するときに`dotnet run`です。 コマンドラインの引数よりも優先、`urls`値から、 *hosting.json*ファイル、および、サーバーはポート 8080 でリッスンします。

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a>重要度の順序

一部の`WebHostBuilder`場合の設定は環境変数から読み込ま最初に設定します。 これらの環境変数の形式を使用する`ASPNETCORE_{configurationKey}`です。 既定では、サーバーがリッスンする Url を設定するに設定する`ASPNETCORE_URLS`です。

構成を指定することによってこれらの環境変数の値のいずれかをオーバーライドすることができます (を使用して`UseConfiguration`) または値を明示的に設定して (を使用して`UseSetting`または明示的な拡張メソッドのいずれかのように`UseUrls`)。 ホストは、どちらのオプションが最後の値を設定を使用します。 プログラムで 1 つの値に設定されて、既定の URL が構成をオーバーライドすることを許可する場合は、URL を設定した後のコマンドライン構成を使用できます。 参照してください[オーバーライド構成](#overriding-configuration)です。

## <a name="starting-the-host"></a>ホストの開始

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**実行**

`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。

```csharp
host.Run();
```

**Start**

非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Url の一覧を渡す場合、`Start`メソッド、指定された Url のリッスンがします。

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

初期化の事前構成済みの既定値を使用して、新しいホストを開始できます`CreateDefaultBuilder`便利な静的メソッドを使用します。 これらのメソッドは、コンソール出力とサーバーを起動[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)改行 (Ctrl-C/SIGINT または SIGTERM) まで待機します。

**開始 (RequestDelegate アプリ)**

始まる、 `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには `WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。 アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。

**開始 (文字列 url、RequestDelegate アプリ)**

URL を使用して起動し、 `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

同じ結果を生成する**開始 (RequestDelegate app)**、上の応答をアプリを除く`http://localhost:8080`です。

**開始 (アクション<IRouteBuilder>routeBuilder)**

インスタンスを使用して`IRouteBuilder`([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) ルーティング ミドルウェアを使用します。

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

次のブラウザーの要求の例を使用します。

| 要求                                    | 応答                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | こんにちは, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | ブエノスアイレス dias、Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | "Ooops!"は文字列で例外をスローします。 |
| `http://localhost:5000/throw`              | 文字列を使用して例外をスロー"Uh $embedded$!" |
| `http://localhost:5000/Sante/Kevin`        | Sante、Kevin!                            |
| `http://localhost:5000`                    | ハローワールド！                             |

`WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。 アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。

**開始 (文字列の url をアクション<IRouteBuilder>routeBuilder)**

インスタンスおよび URL を使用して`IRouteBuilder`:

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

同じ結果**開始 (アクション<IRouteBuilder>routeBuilder)**、アプリを除く応答`http://localhost:8080`です。

**始める (アクション<IApplicationBuilder>アプリ)**

構成するデリゲートを指定、 `IApplicationBuilder`:

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

要求を作成するのには、ブラウザー `http://localhost:5000` "Hello World!"の応答を受信するには `WaitForShutdown`改行 (Ctrl-C/SIGINT または SIGTERM) が発行されるまでブロックします。 アプリで、表示、`Console.WriteLine`メッセージされキー押下を終了するの待機します。

**始める (文字列の url をアクション<IApplicationBuilder>アプリ)**

URL および構成するデリゲートを提供する`IApplicationBuilder`:

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

同じ結果を生成する**で始める (アクション<IApplicationBuilder>アプリ)**、上の応答をアプリを除く`http://localhost:8080`です。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**実行**

`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。

```csharp
host.Run();
```

**Start**

非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Url の一覧を渡す場合、`Start`メソッド、指定された Url のリッスンがします。


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

[IHostingEnvironment インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)アプリの web ホスティング環境に関する情報を提供します。 使用することができます[コンス トラクター インジェクション](xref:fundamentals/dependency-injection)を取得する、`IHostingEnvironment`プロパティと拡張メソッドを使用するのには。

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

使用することができます、[規則ベースのアプローチ](xref:fundamentals/environments#startup-conventions)環境に基づいて起動時に、アプリを構成します。 また、挿入することができます、`IHostingEnvironment`に、`Startup`で使用するためのコンス トラクター `ConfigureServices`:

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
> 加え、`IsDevelopment`の拡張メソッドで`IHostingEnvironment`提供`IsStaging`、 `IsProduction`、および`IsEnvironment(string environmentName)`メソッドです。 参照してください[複数の環境で作業](xref:fundamentals/environments)詳細についてはします。

`IHostingEnvironment`サービスは直接にも挿入され、`Configure`処理パイプラインを設定するためのメソッド。

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

挿入することできます`IHostingEnvironment`に、`Invoke`メソッド カスタムを作成するときに[ミドルウェア](xref:fundamentals/middleware#writing-middleware):

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

[IApplicationLifetime インターフェイス](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime)後スタートアップおよびシャット ダウンのアクティビティを実行することができます。 インターフェイス上の 3 つのプロパティは、キャンセル トークンに登録することができます、`Action`スタートアップおよびシャット ダウン イベントを定義するメソッド。 `StopApplication`メソッドです。

| キャンセル トークン    | &#8230; をトリガー |
| --------------------- | --------------------- |
| `ApplicationStarted`  | ホストが完全に開始されました。 |
| `ApplicationStopping` | ホストが正常なシャット ダウンを実行しています。 要求はまだ処理中です。 このイベントが完了するまで、ブロックをシャット ダウンします。 |
| `ApplicationStopped`  | ホストが正常なシャット ダウンを完了しています。 すべての要求を完全に処理される必要があります。 このイベントが完了するまで、ブロックをシャット ダウンします。 |

| メソッド            | 操作                                           |
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

**ASP.NET Core 2.0 のみに適用されます。**

挿入することで、ホストをビルドする場合`IStartup`呼び出しではなく、依存性の注入コンテナーに直接`UseStartup`または`Configure`、次のエラーが発生する可能性があります:`Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`です。

これは、ために発生、 [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey)をスキャンする (現在のアセンブリ) が必要な`HostingStartupAttributes`します。 手動で挿入した場合`IStartup`依存関係の注入コンテナー内に、次の呼び出しを追加、`WebHostBuilder`指定されたアセンブリ名で。

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

代わりに、ダミーの追加`Configure`を`WebHostBuilder`、設定する、 `applicationName`(`ApplicationKey`) に自動的に。

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**注**: これは、ASP.NET Core 2.0 リリースで必要なだけ場合にのみ呼び出すことがない`UseStartup`または`Configure`です。

詳細については、次を参照してください。[お知らせ: Microsoft.Extensions.PlatformAbstractions がされました (コメント) を削除](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)と[StartupInjection サンプル](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)です。

## <a name="additional-resources"></a>その他の技術情報

* [IIS を使用して Windows への公開します。](../publishing/iis.md)
* [Nginx を使用して Linux への公開します。](../publishing/linuxproduction.md)
* [Apache を使用して Linux への公開します。](../publishing/apache-proxy.md)
* [Windows サービスのホスト](xref:hosting/windows-service)
