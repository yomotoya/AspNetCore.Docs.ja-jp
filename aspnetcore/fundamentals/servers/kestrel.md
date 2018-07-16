---
title: ASP.NET Core への Kestrel Web サーバーの実装
author: rick-anderson
description: ASP.NET Core 用のクロスプラットフォーム Web サーバーである Kestrel について説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 62649351271deebcf1ed9d2f8b2258bed3478989
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126944"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core への Kestrel Web サーバーの実装

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)、[Stephen Halter](https://twitter.com/halter73)

Kestrel は、[ASP.NET Core 向けのクロスプラットフォーム Web サーバー](xref:fundamentals/servers/index)です。 Kestrel は、ASP.NET Core のプロジェクト テンプレートに既定で含まれる Web サーバーです。

Kestrel は、次の機能をサポートします。

* HTTPS
* [Websocket](https://github.com/aspnet/websockets) を有効にするために使用される非透過的なアップグレード
* Nginx の背後にある高パフォーマンスの UNIX ソケット

kestrel は、.NET Core がサポートするすべてのプラットフォームおよびバージョンでサポートされます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kestrel とリバース プロキシを使用するタイミング

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel を単独で使用することも、IIS、Nginx、または Apache などの*リバース プロキシ サーバー*と併用することもできます。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。

![リバース プロキシ サーバーなしでインターネットと直接通信する Kestrel](kestrel/_static/kestrel-to-internet2.png)

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリが内部ネットワークからの要求のみを受け入れる場合は、Kestrel をアプリのサーバーとして直接使うことができます。

![内部ネットワークと直接通信する Kestrel](kestrel/_static/kestrel-to-internal.png)

アプリをインターネットに公開する場合は、"*リバース プロキシ サーバー*" として IIS、Nginx、または Apache を使います。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。

![IIS、Nginx、または Apache などのリバース プロキシ サーバーを介してインターネットと間接的に通信する Kestrel](kestrel/_static/kestrel-to-internet.png)

リバース プロキシは、セキュリティ上の理由からエッジ展開 (インターネットからのトラフィックに公開される) で必要となります。 1.x バージョンの Kestrel は、適切なタイムアウト、サイズの制限、同時接続の制限など、攻撃に対する防御の機能が十分ではありません。

---

リバース プロキシのシナリオは、単一のサーバー上で実行される同じ IP とポートを共有する複数のアプリがある場合に存在します。 Kestrel は、複数のプロセスによる同じ IP とポートの共有をサポートしていないため、このシナリオには対応しません。 ポートでリッスンするように Kestrel を構成すると、Kestrel は、要求のホスト ヘッダーに関係なく、そのポートに対するすべてのトラフィックを処理します。 ポートを共有できるリバース プロキシは、一意の IP とポート上の Kestrel に要求を転送することができます。

リバース プロキシ サーバーが必要ない場合であっても、リバース プロキシ サーバーを使用すると次のような利点があります。

* それがホストするアプリの公開されるパブリック サーフェス領域を制限することができます。
* 構成および防御の層を追加します。
* 既存のインフラストラクチャとより適切に統合できる場合があります。
* 負荷分散と SSL の構成を簡略化します。 リバース プロキシ サーバーのみで SSL 証明書が必要です。このサーバーは、プレーンな HTTP を使用して内部ネットワーク上のアプリ サーバーと通信することができます。

> [!WARNING]
> ホスト フィルタリングがリバース プロキシを使っていない場合は、[ホスト フィルタリング](#host-filtering)を有効にする必要があります。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>ASP.NET Core アプリで Kestrel を使用する方法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) パッケージは、[Microsoft.AspNetCore.App metapackage] (xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) に含まれています。

ASP.NET Core プロジェクト テンプレートは既定では Kestrel を使用します。 *Program.cs* 内のテンプレート コードは [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出し、これによってバックグラウンドで [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) が呼び出されます。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet パッケージをインストールします。

`Main` メソッド内の [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) で [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) 拡張メソッドを呼び出して、必要な [Kestrel オプション](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)を指定します (次のセクションを参照)。

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel オプション

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Kestrel Web サーバーには、インターネットに接続する展開で特に有効な制約構成オプションがいくつかあります。 カスタマイズできる重要な制限は次のとおりです。

* クライアントの最大接続数
* 要求本文の最大サイズ
* 要求本文の最小レート

[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) クラスの [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) プロパティで、上記の制約およびその他の制約を設定します。 `Limits` プロパティは、[KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) クラスのインスタンスを保持します。

**クライアントの最大接続数**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

次のコードを使用することで、アプリ全体に対して同時に開かれる TCP 接続の最大数を設定できます。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

HTTP または HTTPS から別のプロトコルにアップグレードされた接続については別個の制限があります (WebSockets 要求に関する制限など)。 接続がアップグレードされた後、それは `MaxConcurrentConnections` 制限に対してカウントされません。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

接続の最大数は既定で無制限 (null) です。

**要求本文の最大サイズ**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

既定の要求本文の最大サイズは、30,000,000 バイトです。これは約 28.6 MB になります。

ASP.NET Core MVC アプリでの制限をオーバーライドする方法としては、次のように、アクション メソッドに対して [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 属性を使用することをお勧めします。

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

次の例では、すべての要求についての制約をアプリに対して構成する方法を示します。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

ミドルウェア内の特定の要求に関する設定をオーバーライドすることができます。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

アプリが要求の読み取りを開始した後で、要求に対する制限を構成しようとすると、例外がスローされます。 `MaxRequestBodySize` プロパティが読み取り専用状態にある (制限を構成するには遅すぎる) かどうかを示す `IsReadOnly` プロパティがあります。

**要求本文の最小レート**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

kestrel はデータが指定のレート (バイト数/秒) で到着しているかどうかを毎秒チェックします。 レートが最小値を下回った場合は接続がタイムアウトになります。猶予期間とは、クライアントによって送信速度を最低ラインまで引き上げられるのを、Kestrel が待機する時間のことです。この期間中、レートはチェックされません。 猶予期間により、TCP のスロースタートのため最初にデータを低速で送信する接続がドロップされるのを回避できます。

既定の最小レートは 240 バイト/秒であり、5 秒の猶予時間が設定されています。

最小レートは応答にも適用されます。 要求制限と応答制限を設定するコードは、プロパティ名およびインターフェイス名に `RequestBody` または `Response` が使用されることを除けば同じです。

次の例では、*Program.cs* で最小データ レートを構成する方法を示します。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

ミドルウェアで要求レートを構成することができます。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

Kestrel のその他のオプションと制限については、以下をご覧ください。

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Kestrel のオプションと制限については、以下をご覧ください。

* [KestrelServerOptions クラス](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a>エンドポイントの構成

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。 Kestrel の URL プレフィックスとポートを構成するには、[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) で [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) または [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) メソッドを呼び出します。 `UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、`ASPNETCORE_URLS` 環境変数も機能しますが、このセクションで後述する制限があります。

`urls` ホスト構成キーは、アプリ構成ではなくホスト構成から取得する必要があります。 ホストは構成が構成ファイルから読み取られるまでに完全に初期化されるため、`urls` キーと値を *appsettings.json* に追加しても、ホストの構成には影響しません。 ただし、*appsettings.json* の `urls` キーをホスト ビルダー [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) と共に使用して、ホストを構成できます。

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

既定では、ASP.NET Core は以下にバインドされます。

* `http://localhost:5000`
* `https://localhost:5001` (ローカル開発証明書が存在する場合)

開発証明書が作成されます。

* [.NET Core SDK](/dotnet/core/sdk) がインストールされるとき。
* 証明書を作成するには [dev-certs ツール](xref:aspnetcore-2.1#https)を使います。

一部のブラウザーでは、ローカル開発証明書を信頼するには、ブラウザーに明示的にアクセス許可を付与する必要があります。

ASP.NET Core 2.1 およびそれより後のプロジェクト テンプレートでは、アプリは HTTPS で実行するように既定で構成され、[HTTPS のリダイレクトと HSTS のサポート](xref:security/enforcing-ssl)が組み込まれます。

Kestrel の URL プレフィックスとポートを構成するには、[KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) で [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) または [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) メソッドを呼び出します。

`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、`ASPNETCORE_URLS` 環境変数も機能しますが、このセクションで後述する制限があります (既定の証明書が、HTTPS エンドポイントの構成に使用できる必要があります)。

ASP.NET Core 2.1 の `KestrelServerOptions` の構成:

**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**  
指定した各エンドポイントに対して実行するように、構成の `Action` を指定します。 `ConfigureEndpointDefaults` を複数回呼び出すと、前の `Action` が最後に指定した `Action` で置き換えられます。

**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**  
各 HTTPS エンドポイントに対して実行するように、構成の `Action` を指定します。 `ConfigureHttpsDefaults` を複数回呼び出すと、前の `Action` が最後に指定した `Action` で置き換えられます。

**Configure(IConfiguration)**  
入力として [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) を受け取る Kestrel を設定するための構成ローダーを作成します。 構成のスコープは、Kestrel の構成セクションにする必要があります。

**ListenOptions.UseHttps**  
HTTPS を使用するように Kestrel を構成します。

`ListenOptions.UseHttps` 拡張機能:

* `UseHttps` &ndash; 既定の証明書で HTTPS を使うように Kestrel を構成します。 既定の証明書が構成されていない場合は、例外がスローされます。
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` パラメーター:

* `filename` は、アプリのコンテンツ ファイルが格納されているディレクトリを基準とする、証明書ファイルのパスとファイル名です。
* `password` は、X.509 証明書データにアクセスするために必要なパスワードです。
* `configureOptions` は、`HttpsConnectionAdapterOptions` を構成するための `Action` です。 `ListenOptions` を返します。
* `storeName` は、証明書の読み込み元の証明書ストアです。
* `subject` は、証明書のサブジェクト名です。
* `allowInvalid` は、無効な証明書 (自己署名証明書など) を考慮する必要があるかどうかを示します。
* `location` は、証明書を読み込むストアの場所です。
* `serverCertificate` は、X.509 証明書です。

運用環境では、HTTPS を明示的に構成する必要があります。 少なくとも、既定の証明書を指定する必要があります。

サポートされている構成を次に説明します。

* 構成なし
* 構成から既定の証明書を置き換える
* コードで既定値を変更する

*構成なし*

Kestrel は、`http://localhost:5000` と `https://localhost:5001` (既定の証明書が使用可能な場合) でリッスンします。

以下を使用して URL を指定します。

* `ASPNETCORE_URLS` 環境変数。
* `--urls` コマンド ライン引数。
* `urls` ホスト構成キー。
* `UseUrls` 拡張メソッド。

詳しくは、「[サーバーの URL](xref:fundamentals/host/web-host#server-urls)」および「[構成のオーバーライド](xref:fundamentals/host/web-host#override-configuration)」をご覧ください。

これらの方法を使うと、1 つまたは複数の HTTP エンドポイントおよび HTTPS エンドポイント (既定の証明書が使用可能な場合は HTTPS) を指定できます。 セミコロン区切りのリストとして値を構成します (例: `"Urls": "http://localhost:8000;http://localhost:8001"`)。

*構成から既定の証明書を置き換える*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) は既定で `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` を呼び出して Kestrel の構成を読み込みます。 Kestrel は、既定の HTTPS アプリ設定構成スキーマを使用できます。 ディスク上のファイルまたは証明書ストアから、URL や使用する証明書など、複数のエンドポイントを構成します。

以下の *appsettings.json* の例では、次のことが行われています。

* **AllowInvalid** を `true` に設定し、の無効な証明書 (自己署名証明書など) の使用を許可します。
* 証明書 (後の例では **HttpsDefaultCert**) が指定されていないすべての HTTPS エンドポイントは、**[証明書]** > **[既定]** または開発証明書で定義されている証明書にフォールバックします。

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

証明書ノードの **Path** と **Password** を使用する代わりの方法は、証明書ストアのフィールドを使って証明書を指定することです。 たとえば、**[証明書]** > **[既定]** の証明書は次のように指定できます。

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

スキーマに関する注意事項:

* エンドポイント名は大文字と小文字が区別されます。 たとえば、`HTTPS` と `Https` は有効です。
* `Url` パラメーターは、エンドポイントごとに必要です。 このパラメーターの形式は、1 つの値に制限されることを除き、最上位レベルの `Urls` 構成パラメーターと同じです。
* これらのエンドポイントは、最上位レベルの `Urls` 構成での定義に追加されるのではなく、それを置き換えます。 コードで `Listen` を使用して定義されているエンドポイントは、構成セクションで定義されているエンドポイントに累積されます。
* `Certificate` セクションは省略可能です。 `Certificate` セクションを指定しないと、前述のシナリオで定義した既定値が使用されます。 既定値を使用できない場合、サーバーにより例外がスローされ、開始できません。
* `Certificate` セクションは、**Path**&ndash;**Password** 証明書と **Subject**&ndash;**Store** 証明書の両方をサポートします。
* ポートが競合しない限り、この方法で任意の数のエンドポイントを定義できます。
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` が `.Endpoint(string name, options => { })` メソッドで返す `KestrelConfigurationLoader` を使用して、構成されているエンドポイントの設定を補足できます。

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  `KestrelServerOptions.ConfigurationLoader` に直接アクセスして、既存のローダー ([WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) によって提供されるものなど) の反復を維持することもできます。

* カスタム設定を読み取ることができるように、各エンドポイントの構成セクションを `Endpoint` メソッドのオプションで使用できます。
* 別のセクションで `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` を再び呼び出すことにより、複数の構成を読み込むことができます。 前のインスタンスで `Load` を明示的に呼び出していない限り、最後の構成のみが使用されます。 既定の構成セクションを置き換えることができるように、メタパッケージは `Load` を呼び出しません。
* `KestrelConfigurationLoader` はミラー、`KestrelServerOptions` から API の `Listen` ファミリを `Endpoint` オーバーロードとしてミラーリングするので、コードと構成のエンドポイントを同じ場所で構成できます。 これらのオーバーロードでは名前は使用されず、構成からの既定の設定のみが使用されます。

*コードで既定値を変更する*

`ConfigureEndpointDefaults` および`ConfigureHttpsDefaults` を使用して、`ListenOptions` および `HttpsConnectionAdapterOptions` の既定の設定を変更できます (前のシナリオで指定した既定の証明書のオーバーライドなど)。 エンドポイントを構成する前に、`ConfigureEndpointDefaults` および `ConfigureHttpsDefaults` を呼び出す必要があります。

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Kestrel による SNI のサポート*

[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) を使用すると、同じ IP アドレスとポートで複数のドメインをホストできます。 SNI が機能するためには、サーバーが正しい証明書を提供できるように、クライアントは TLS ハンドシェイクの間にセキュリティで保護されたセッションのホスト名をサーバーに送信します。 クライアントは、TLS ハンドシェイクに続くセキュリティで保護されたセッション中に、提供された証明書をサーバーとの暗号化された通信に使用します。

Kestrel は、`ServerCertificateSelector` コールバックによって SNI をサポートします。 コールバックは接続ごとに 1 回呼び出されるので、アプリはそれを使って、ホスト名を検査し、適切な証明書を選択できます。

SNI のサポートには、次が必要です。

* ターゲット フレームワーク `netcoreapp2.1` で実行される必要があります。 `netcoreapp2.0` および `net461` の場合は、コールバックは呼び出されますが、`name` は常に `null` です。 クライアントが TLS ハンドシェイクでホスト名パラメーターを提供しない場合も、`name` は `null` です。
* すべての Web サイトは、同じ Kestrel インスタンスで実行されます。 Kestrel では、リバース プロキシは使用しない、複数のインスタンスでの IP アドレスとポートの共有はサポートしていません。

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

**TCP ソケットにバインドする**

[Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) メソッドは TCP ソケットにバインドし、オプションのラムダにより SSL 証明書を構成できます。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

例では、[ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) を使用して、エンドポイントに対する SSL が構成されます。 同じ API を使用して、特定のエンドポイントに対する他の Kestrel 設定を構成します。

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**UNIX ソケットにバインドする**

次の例に示すように、Nginx でのパフォーマンス向上のため、[ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) で UNIX ソケットをリッスンします。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

**ポート 0**

ポート番号 `0` を指定すると、Kestrel は使用可能なポートに動的にバインドします。 次の例では、Kestrel が実行時に実際にバインドするポートを特定する方法を示します。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

アプリを実行すると、コンソール ウィンドウの出力で、アプリがアクセスできる動的なポートが示されます。

```console
Listening on the following addresses: http://127.0.0.1:48508
```

**UseUrls、--url コマンド ライン引数、url ホスト構成キー、および ASPNETCORE_URLS 環境変数の制限事項**

次の方法でエンドポイントを構成します。

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* `--urls` コマンド ライン引数
* `urls` ホスト構成キー
* `ASPNETCORE_URLS` 環境変数

コードを Kestrel 以外のサーバーで機能させるには、これらのメソッドが有用です。 ただし、次の制限事項に注意してください。

* HTTPS エンドポイントの構成で (たとえば、前に説明したように `KestrelServerOptions` 構成または構成ファイルを使用して) 既定の証明書が提供されていない場合、これらの方法で SSL を使用することはできません。
* `Listen` と `UseUrls` 両方の方法を同時に使用すると、`Listen` エンドポイントが `UseUrls` エンドポイントをオーバーライドします。

**IIS エンドポイントの構成**

IIS を使用するときは、IIS オーバーライド バインドに対する URL バインドを、`Listen` または `UseUrls` によって設定します。 詳しくは、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。 以下を使用して Kestrel の URL プレフィックスとポートを構成します。

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) 拡張メソッド
* `--urls` コマンド ライン引数
* `urls` ホスト構成キー
* ASP.NET Core 構成システム (`ASPNETCORE_URLS` 環境変数など)

これらのメソッドについて詳しくは、[ホスティング](xref:fundamentals/host/index)に関するページをご覧ください。

**IIS エンドポイントの構成**

IIS を使用するときは、IIS オーバーライド バインドに対する URL バインドを、`UseUrls` によって設定します。 詳しくは、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」をご覧ください。

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>トランスポート構成

ASP.NET Core 2.1 のリリースにより、Kestrel の既定のトランスポートは、Libuv に基づかなくなり、代わりにマネージド ソケットに基づくようになりました。 これは、[WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) を呼び出す 2.1 にアップグレードする ASP.NET Core 2.0 アプリにとっては破壊的変更であり、以下のいずれかのパッケージに依存しています。

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (パッケージの直接の参照)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) メタパッケージを使用し Libuv を使用する必要のある ASP.NET Core 2.1 以降のプロジェクトでは、次が必要です。

* アプリのプロジェクト ファイルに [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) パッケージの依存関係を追加します。

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) を呼び出します。

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>URL プレフィックス

`UseUrls`、`--urls` コマンドライン引数、`urls` ホスト構成キー、または `ASPNETCORE_URLS` 環境変数を使用する場合、URL プレフィックスは次のいずれかの形式となります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

HTTP URL プレフィックスのみが有効です。 `UseUrls` を使用して URL バインドを構成する場合、kestrel は SSL をサポートしません。

* IPv4 アドレスとポート番号

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` は、すべての IPv4 アドレスにバインドする特別なケースです。

* IPv6 アドレスとポート番号

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  IPv6 の `[::]` は IPv4 の `0.0.0.0` に相当します。

* ホスト名とポート番号

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  ホスト名、`*`、および `+` は特別ではありません。 有効な IP アドレスまたは `localhost` と認識されないものはすべて、IPv4 および IPv6 のすべての IP にバインドします。 異なるホスト名を同じポート上の異なる ASP.NET Core アプリにバインドするには、[HTTP.sys](xref:fundamentals/servers/httpsys) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。

  > [!WARNING]
  > ホスト フィルタリングがリバース プロキシを使っていない場合は、[ホスト フィルタリング](#host-filtering)を有効にします。

* `localhost` 名とポート番号、またはループバック IP とポート番号

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  `localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。 要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。 他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* IPv4 アドレスとポート番号

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` は、すべての IPv4 アドレスにバインドする特別なケースです。

* IPv6 アドレスとポート番号

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  IPv6 の `[::]` は IPv4 の `0.0.0.0` に相当します。

* ホスト名とポート番号

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  ホスト名、`*`、および `+` は特別ではありません。 認識された IP アドレスまたは `localhost` ではないものはいずれも、IPv4 および IPv6 の IP にバインドします。 異なるホスト名を同じポート上の異なる ASP.NET Core アプリにバインドするには、[WebListener](xref:fundamentals/servers/weblistener) を使用するか、または IIS、Nginx、Apache などのリバース プロキシ サーバーを使用します。

* `localhost` 名とポート番号、またはループバック IP とポート番号

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  `localhost` が指定された場合、Kestrel は IPv4 ループバック インターフェイスと IPv6 ループバック インターフェイスの両方へのバインドを試みます。 要求されたポートがいずれかのループバック インターフェイス上の別のサービスで使用中である場合、Kestrel は開始できません。 他の何らかの理由でいずれかのループバック インターフェイスが使用できない場合 (ほとんどの場合は IPv6 がサポートされていないことが理由)、Kestrel は警告をログに記録します。

* UNIX ソケット

  ```
  http://unix:/run/dan-live.sock
  ```

**ポート 0**

ポート番号 `0` を指定すると、Kestrel は使用可能なポートに動的にバインドします。 ポート `0` へのバインドは、`localhost` を除き、任意のホスト名または IP で許可されます。

アプリを実行すると、コンソール ウィンドウの出力で、アプリがアクセスできる動的なポートが示されます。

```console
Now listening on: http://127.0.0.1:48508
```

**SSL 用の URL プレフィックス**

`UseHttps` 拡張メソッドを呼び出す場合は、`https:` に URL プレフィックスを必ず含めます。

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> 同じポート上で、HTTPS と HTTP をホストすることはできません。

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a>ホスト フィルタリング

Kestrel は `http://example.com:5000` などのプレフィックスに基づく構成をサポートしますが、Kestrel はほとんどのホスト名を無視します。 ホスト `localhost` は、ループバック アドレスへのバインドに使用される特殊なケースです。 明示的な IP アドレス以外のすべてのホストは、すべてのパブリック IP アドレスにバインドします。 この情報のいずれも、要求の `Host` ヘッダーの検証には使用されません。

::: moniker range="< aspnetcore-2.0"

これを解決するには、ホスト ヘッダー フィルタリングを使用するリバース プロキシの背後でホストします。 これは、ASP.NET Core 1.x の Kestrel に対してのみサポートされるシナリオです。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

これを解決するには、ミドルウェアを使用して、`Host` ヘッダーによって要求をフィルター処理します。

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

前述の `HostFilteringMiddleware` を `Startup.Configure` で登録します。 [ミドルウェアの登録の順序](xref:fundamentals/middleware/index#ordering)が重要であることに注意してください。 この登録は、診断ミドルウェア (`app.UseExceptionHandler` など) の登録の直後に行う必要があります。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

このミドルウェアには、*appsettings.json*/*appsettings.\<>.json* に、`AllowedHosts` キーを指定する必要があります。 この値は、ポート番号を含まないホスト名のセミコロン区切りリストです。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

これを解決するには、Host Filtering Middleware を使用します。 Host Filtering Middleware は、[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) に含まれる、[Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) パッケージによって提供されています。 このミドルウェアは、[AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering) を呼び出す、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) によって追加されています。

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Host Filtering Middleware は既定では無効です。 このミドルウェアを有効にするには、*appsettings.json*/*appsettings.\<>.json* に、`AllowedHosts` キーを定義します。 この値は、ポート番号を含まないホスト名のセミコロン区切りリストです。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appsettings.json*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) には、[ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) オプションもあります。 Forwarded Headers Middleware および Host Filtering Middleware には、異なるシナリオ用に類似した機能があります。 リバース プロキシ サーバーまたはロード バランサーを使用して要求を転送するとき、ホスト ヘッダーが保存されていない場合、Forwarded Headers Middleware に `AllowedHosts` を設定するのが適切です。 Kestrel がエッジ サーバーとして使用されていたり、ホスト ヘッダーが直接転送されたりしている場合、Host Filtering Middleware に `AllowedHosts` を設定するのが適切です。
>
> Forwarded Headers Middleware の詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。

::: moniker-end

## <a name="additional-resources"></a>その他の技術情報

* [HTTPS の適用](xref:security/enforcing-ssl)
* [Kestrel ソース コード](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)(RFC 7230: メッセージの構文と経路制御 (セクション 5.4: ホスト))
* [プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)
