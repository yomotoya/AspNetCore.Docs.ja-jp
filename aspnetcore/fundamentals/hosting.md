---
title: "ASP.NET Core でのホスティング |Microsoft ドキュメント"
author: ardalis
description: "ASP.NET Core web ホストの概要です。"
keywords: "ASP.NET Core、web ホスト、IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a>ASP.NET Core でのホスティングの概要

によって[Steve Smith](http://ardalis.com)

ASP.NET Core アプリケーションを実行するには、構成および使用してホストを起動する必要があります`WebHostBuilder`です。

## <a name="what-is-a-host"></a>ホストとは何ですか。

ASP.NET Core アプリを必要とする*ホスト*を実行するためにします。 ホストを実装する必要があります、`IWebHost`機能とサービスのコレクションを公開するインターフェイスと`Start`メソッドです。 インスタンスを使用して、ホストが通常作成、 `WebHostBuilder`、ビルドを返す、`WebHost`インスタンス。 `WebHost`要求を処理するサーバーを参照します。 詳細については[サーバー](servers/index.md)です。

### <a name="what-is-the-difference-between-a-host-and-a-server"></a>ホストとサーバー間で違いは何ですか。

ホストは、アプリケーションの起動と有効期間の管理を担当します。 サーバーが HTTP 要求の受け入れを担当します。 ホストの役割の一部には、アプリケーションのサービスとサーバーは、使用可能で正しく構成されていることを確認が含まれています。 サーバーをラップするラッパーとしてホストを検討することができます。 ホストが、特定のサーバーを使用するよう構成します。サーバーでは、そのホストの認識しません。

## <a name="setting-up-a-host"></a>ホストの設定

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

インスタンスを使用してホストを作成する`WebHostBuilder`です。 これは通常、アプリのエントリ ポイントで実行: `public static void Main` (プロジェクト テンプレート内にある、 *Program.cs*ファイル)。 一般的な*Program.cs*ように、以下を使用する方法を示します、`WebHostBuilder`ホストを作成します。

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]

`WebHostBuilder`責任は、アプリのサーバーのブートス トラップは、ホストを作成します。 `WebHostBuilder`実装するサーバーを指定する必要があります`IServer`(`UseKestrel`上記のコードで)。 `UseKestrel`アプリで使用される、Kestrel サーバーを指定します。

サーバーの*コンテンツのルート*MVC ビュー ファイルと同様に、コンテンツ ファイルの検索場所を決定します。 既定のコンテンツのルートは、アプリケーションが実行されるフォルダーです。

> [!NOTE]
> 指定する`Directory.GetCurrentDirectory`のコンテンツのルートは、このフォルダーから、アプリが開始したときに、アプリのコンテンツのルートとしての web プロジェクトのルート フォルダーが使用されます (たとえば、呼び出し`dotnet run`web プロジェクト フォルダーから)。 これは Visual Studio で使用される既定値と`dotnet new`テンプレート。

リバース プロキシとして、IIS を使用するには、呼び出す`UseIISIntegration`ホストの作成の一部として。 

なお`UseIISIntegration`が構成されない、*サーバー*と同様、`UseKestrel`はします。 ASP.NET Core で IIS を使用して、両方を指定する必要があります`UseKestrel`と`UseIISIntegration`です。 `UseKestrel`web サーバーを作成し、アプリをホストします。 `UseIISIntegration`IIS または iis Express が使用する環境変数を検査し、リッスンするポートを使用するヘッダーなどの設定を構成します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

インスタンスを使用してホストを作成する`WebHostBuilder`です。 これは通常、アプリのエントリ ポイントで実行: `public static void Main` (プロジェクト テンプレート内にある、 *Program.cs*ファイル)。 一般的な*Program.cs*呼び出し次に示すように、`CreateDefaultbuilder`ホストを作成します。

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

`CreateDefaultbuilder`インスタンスを作成`WebHostBuilder`をアプリのサーバーをブートス トラップするホストを作成します。 ホストが必要とする[IServer を実装しているサーバー](servers/index.md)です。 組み込みのサーバーが[Kestrel](servers/kestrel.md)と[HTTP.sys](servers/httpsys.md)です。`CreateDefaultbuilder` Kestrel 既定で使用します。

`CreateDefaultbuilder`web サーバーとして Kestrel を構成するだけでなくのセットアップ タスクを実行します。

* コンテンツのルートに設定`Directory.GetCurrentDirectory`です。
* 構成を読み込みます。
  * *される appsettings.json*
  * *appsettings です。\<EnvironmentName > .json*です。
  * 開発環境でアプリを実行するときにユーザーの機密情報
  * 環境変数
  * 指定されたコマンドライン引数
* [ログの構成] セクションで指定されたルールをフィルター処理にコンソールとデバッグの出力のログ記録を構成します。
* IIS の統合を有効にします。
* 開発環境でアプリを実行するときは、開発者の例外のページを追加します。

サーバーの*コンテンツのルート*MVC ビュー ファイルと同様に、コンテンツ ファイルの検索場所を決定します。 既定のコンテンツのルートは、アプリケーションが実行されるフォルダーです。

> [!NOTE]
> 指定する`Directory.GetCurrentDirectory`のコンテンツのルートは、このフォルダーから、アプリが開始したときに、アプリのコンテンツのルートとしての web プロジェクトのルート フォルダーが使用されます (たとえば、呼び出し`dotnet run`web プロジェクト フォルダーから)。 これは Visual Studio で使用される既定値と`dotnet new`テンプレート。

ASP.NET Core を自動的に呼び出して、リバース プロキシとして IIS を使用するときに`UseIISIntegration`ホストの作成の一部として。 詳細については、次を参照してください。 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)です。

なお`UseIISIntegration`が構成されない、*サーバー*と同様、`UseKestrel`はします。 `UseKestrel`web サーバーを作成し、アプリをホストします。 `UseIISIntegration`IIS または iis Express が使用する環境変数を検査し、リッスンするポートを使用するヘッダーなどの設定を構成します。

---

ホスト (と ASP.NET Core アプリケーション) を構成する最小限の実装は、サーバーとアプリケーションの要求パイプラインの構成のみを含めます。

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> ホストをセットアップするときに使用できる`Configure`と`ConfigureServices`メソッドを指定するだけでなく、`Startup`クラス (- これらのメソッドを定義することもあります。 を参照してください[アプリケーションの起動](startup.md))。 複数回呼び出す`ConfigureServices`互いに追加されます。 呼び出しを`Configure`または`UseStartup`以前の設定を置き換えます。

## <a name="configuring-a-host"></a>ホストの構成

`WebHostBuilder`を使用して直接設定することもできるホストのほとんどの利用可能な構成値を設定するためのメソッドを提供`UseSetting`と関連するキー。

### <a name="host-configuration-values"></a>ホストの構成値

**スタートアップ エラーがキャプチャされる**`bool`

キー:`captureStartupErrors`です。 既定値は `false` です。 ときに`false`起動の結果、終了ホスト中のエラーです。 ときに`true`、ホストからすべての例外をキャプチャする、`Startup`クラスし、サーバーを起動しようとしています。 エラー ページが表示されます (ジェネリック、または詳細、詳細なエラーの設定に基づいての下) の要求ごとにします。 使用して設定、`CaptureStartupErrors`メソッドです。

注: Kestrel および IIS で、アプリの実行時に既定の動作はスタートアップ エラーをキャプチャします。 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

**コンテンツのルート**`string`

キー:`contentRoot`です。 既定値は (Kestrel; 用アプリケーション アセンブリが存在するフォルダーIIS web プロジェクトのルート既定で使用されます)。 この設定は、ASP.NET Core の MVC ビューなどのコンテンツ ファイルの検索開始位置を決定します。 ベース パスとしても使用、 [Web ルート設定](#web-root-setting)です。 使用して設定、`UseContentRoot`メソッドです。 パスが存在するか、ホストが起動しなくなります。

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

**詳細なエラー**`bool`

キー:`detailedErrors`です。 既定値は `false` です。 ときに`true`(または"Development"に環境を設定すると)、アプリの一般的なエラー ページだけではなく、起動の例外の詳細が表示されます。 使用して設定`UseSetting`です。

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

詳細なエラー情報を設定すると`false`起動エラーのキャプチャは`true`、サーバーに対するすべての要求に対する応答で汎用エラー ページが表示されます。

![一般的なエラー ページ](hosting/_static/generic-error-page.png)

詳細なエラー情報を設定すると`true`起動エラーのキャプチャは`true`、サーバーに対するすべての要求に対する応答の詳細なエラー ページが表示されます。

![詳細なエラー ページ](hosting/_static/detailed-error-page.png)

**環境**`string`

キー:`environment`です。 既定値は"Production"です。 任意の値に設定することがあります。 フレームワークによって定義された値には、"Development"、「ステージング」および"Production"が含まれます。 値は、大文字小文字を区別はありません。 参照してください[複数の環境で作業](environments.md)です。 使用して設定、`UseEnvironment`メソッドです。

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> 既定では、環境がから読み取られた、`ASPNETCORE_ENVIRONMENT`環境変数。 環境変数を設定することがあります Visual Studio を使用する場合、 *launchSettings.json*ファイル。

<a id="server-urls"></a>

**サーバーの Url**`string`

キー:`urls`です。 セミコロン (;) に設定区切り URL の一覧の前に、サーバーが応答する必要があります。 たとえば、`http://localhost:123` のようにします。 ドメインまたはホスト名が置き換えられます"\*"サーバーは任意の IP アドレスで要求をリッスンか、指定したポートとプロトコルを使用してホストを示すために (たとえば、`http://*:5000`または`https://*:5001`)。 プロトコル (`http://`または`https://`) 各 url を含める必要があります。 プレフィックスは、構成されたサーバーによって解釈されます。サポートされている形式は、サーバー間で異なります。

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

ASP.NET Core 2.0 Kestrel が独自のエンドポイント構成 API ではサポートしていません`https://`で、`urls`文字列。 詳細については、次を参照してください。 [Kestrel 概要](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)です。

**スタートアップ アセンブリ**`string`

キー:`startupAssembly`です。 検索対象となるアセンブリの決定、`Startup`クラスです。 使用して設定、`UseStartup`メソッドです。 特定の種類を使用する代わりに参照`WebHostBuilder.UseStartup<StartupType>`です。 複数`UseStartup`メソッドが呼び出されると、最後の 1 つが優先されます。

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

**ルートの web**`string`

キー:`webroot`です。 既定値は指定されていない場合`(Content Root Path)\wwwroot`存在する場合、します。 このパスが存在しない、no-op ファイル プロバイダーが使用されます。 使用して設定`UseWebRoot`です。

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a>構成をオーバーライドします。

使用して[構成](configuration.md)ホストによって使用される構成値を設定します。 これらの値は、後でオーバーライドされる可能性があります。 使用してこれが指定されて`UseConfiguration`です。

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

上記の例でコマンドライン引数可能性があるに渡された、ホストを構成するかに構成設定を指定できます必要に応じて、 *hosting.json*ファイル。 特定の URL で実行するホストを指定するには、コマンド プロンプトから目的の値で渡す可能性があります。

```console
dotnet run --urls "http://*:5000"
```

`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。

```csharp
host.Run();
```

非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Url の一覧を渡す、`Start`メソッドと、指定された Url でリッスンします。

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

ここでは有効な URL の形式は、使用しているサーバーに依存します。 詳細については、次を参照してください。[サーバー Url](#server-urls)この記事で前述しました。

> [!NOTE]
> `UseConfiguration`拡張メソッドは現在によって返される構成セクションを解析できる`GetSection`(たとえば、`.UseConfiguration(Configuration.GetSection("section"))`です。 `GetSection`メソッド要求のセクションに構成キーをフィルター処理が残りますセクションの名前のキーに (たとえば、 `section:urls`、 `section:environment`)。 `UseConfiguration`メソッドに一致するキーが必要ですが、`WebHostBuilder`キー (たとえば、 `urls`、 `environment`)。 キーにセクション名の有無は、セクションの値が、ホストを構成することを防止します。 この問題は、将来のリリースで対処される予定です。 詳細と回避策については、次を参照してください。[フル キーを使用して WebHostBuilder.UseConfiguration に構成セクションを渡して](https://github.com/aspnet/Hosting/issues/839)です。

### <a name="ordering-importance"></a>重要度の順序

`WebHostBuilder`場合設定が特定の環境変数から読み取る最初に設定します。 これらの環境変数の形式を使用する必要があります`ASPNETCORE_{configurationKey}`ため、設定は既定では、サーバーがリッスン Url を設定する例については、`ASPNETCORE_URLS`です。

これらの環境変数の値のいずれかの構成を指定することによってオーバーライドできます (を使用して`UseConfiguration`) または値を明示的に設定して (を使用して`UseUrls`のインスタンス)。 どちらのオプション設定値が最後に、ホストが使用されます。 このため、`UseIISIntegration`後に表示する必要があります`UseUrls`IIS で提供される動的に 1 つを URL に置き換えます。 プログラムで 1 つの値に設定されて、既定の URL が構成をオーバーライドすることを許可する場合は、次のようにホストを構成する可能性があります。

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a>その他の技術情報

* [IIS を使用して Windows への公開します。](../publishing/iis.md)
* [Nginx を使用して Linux への公開します。](../publishing/linuxproduction.md)
* [Apache を使用して Linux への公開します。](../publishing/apache-proxy.md)
* [Windows サービスのホスト](xref:hosting/windows-service)

