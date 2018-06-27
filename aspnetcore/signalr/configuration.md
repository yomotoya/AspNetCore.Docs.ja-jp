---
title: ASP.NET Core SignalR の構成
author: rachelappel
description: ASP.NET Core SignalR アプリケーションを構成する方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961984"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET Core SignalR の構成

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack シリアル化オプション

ASP.NET Core SignalR は、メッセージをエンコードするための 2 つのプロトコルをサポートしています: [JSON](https://www.json.org/)と[MessagePack](https://msgpack.org/index.html)です。 各プロトコルには、シリアル化の構成オプションがあります。

JSON のシリアル化を使用して、サーバーで構成できます。、 [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)拡張メソッドは、後に追加する[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で、`Startup.ConfigureServices`メソッドです。 `AddJsonProtocol`メソッドは、受信するデリゲートを受け取る、`options`オブジェクト。 [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)そのオブジェクトのプロパティには、JSON.NET`JsonSerializerSettings`と戻り値の引数のシリアル化の構成に使用できるオブジェクト。 参照してください、 [JSON.NET ドキュメント](https://www.newtonsoft.com/json/help/html/Introduction.htm)詳細についてはします。

たとえば、名の代わりに、既定値「キャメル ケース」、"PascalCase"プロパティの名前を使用するシリアライザーを構成する次のコードを使用します。

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

.NET クライアントで、同じ`AddJsonHubProtocol`の拡張メソッドが存在する[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)です。 `Microsoft.Extensions.DependencyInjection`拡張メソッドを解決するのには名前空間をインポートする必要があります。

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> この時点での JavaScript クライアントでの JSON のシリアル化を構成することはありません。

### <a name="messagepack-serialization-options"></a>MessagePack シリアル化オプション

デリゲートを提供することによって MessagePack シリアル化を構成することができます、 [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼び出します。 参照してください[SignalR で MessagePack](xref:signalr/messagepackhubprotocol)詳細についてはします。

> [!NOTE]
> この時点での JavaScript クライアントで MessagePack シリアル化を構成することはありません。

## <a name="configure-server-options"></a>サーバー オプションを構成します。

次の表では、SignalR ハブの構成オプションについて説明します。

| オプション | 説明 |
| ------ | ----------- |
| `HandshakeTimeout` | クライアントがこの時間間隔内で最初のハンドシェイク メッセージを送信しない場合、接続は閉じられます。 |
| `KeepAliveInterval` | サーバーは、この時間内でメッセージを送信していない場合、接続を開いたままに ping メッセージは自動的に送信します。 |
| `SupportedProtocols` | このハブでサポートされるプロトコル。 既定では、サーバーに登録されているすべてのプロトコルが許可されるが、プロトコルは、個々 のハブの特定のプロトコルを無効にするには、この一覧から削除できます。 |
| `EnableDetailedErrors` | 場合`true`、詳細なハブ メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。 既定値は`false`ように、これらの例外メッセージは機密性の高い情報を含めることができます。 |

オプション デリゲートを提供することですべてのハブのオプションを構成でき、`AddSignalR`で呼び出す`Startup.ConfigureServices`です。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

1 つのハブのオプションで提供されるグローバル オプションを上書き`AddSignalR`を使用して構成するには[ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

使用して`HttpConnectionDispatcherOptions`トランスポートとメモリ バッファーの管理に関連する高度な設定を構成します。 これらのオプションを構成するデリゲートを渡すことによって[ `MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)です。

| オプション | 説明 |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | クライアントから受信したバイトの最大数をサーバーのバッファー。 この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。 既定値は、32 KB です。 |
| `AuthorizationData` | 一連の[ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)オブジェクトが、クライアントがハブへの接続を承認されているかどうかを判断するために使用します。 既定では、この値が格納されますから、`Authorize`ハブ クラスに適用される属性です。 |
| `TransportMaxBufferSize` | アプリによって送信されたバイト数の最大数をサーバーのバッファー。 この値を増やすことによりより大きなメッセージを送信するサーバーが、メモリ消費量に悪影響を与えることができます。 既定値は、32 KB です。 |
| `Transports` | ビットマスク`HttpTransportType`値をトランスポートを制限するクライアントを使用して接続します。 既定では、すべてのトランスポートが有効にします。 |
| `LongPolling` | 長いポーリング トランスポートに固有の追加オプション。 |
| `WebSockets` | Websocket トランスポートに固有の追加オプション。 |

長いポーリングのトランスポートを使用して構成できる追加のオプションがあります、`LongPolling`プロパティ。

| オプション | 説明 |
| ------ | ----------- |
| `PollTimeout` | 時間の最大量、サーバーは 1 つのポーリング要求を終了する前に、クライアントに送信するメッセージを待機します。 新しいポーリング要求をより頻繁に発行するクライアントは、この値を小さきます。 既定値は、90 秒です。 |

WebSocket トランスポートを使用して構成できる追加のオプションがあります、`WebSockets`プロパティ。

| オプション | 説明 |
| ------ | ----------- |
| `CloseTimeout` | サーバーが閉じ、クライアントがこの時間間隔内に閉じるが失敗した場合、接続は終了します。 |
| `SubProtocolSelector` | 設定するために使用されるデリゲート、`Sec-WebSocket-Protocol`ヘッダーをカスタム値。 デリゲートは、入力としてクライアントによって要求された値を受け取るし、目的の値を返します。 |

## <a name="configure-client-options"></a>クライアントのオプションを構成します。

クライアントのオプションを構成することができます、 `HubConnectionBuilder` (.NET と JavaScript の両方のクライアントで使用できる)、型ではなく、`HubConnection`自体です。

### <a name="configure-logging"></a>ログを構成します。

使用して、.NET クライアントでログ記録が構成されている、`ConfigureLogging`メソッドです。 サーバー上にある、同じ方法でログ記録プロバイダーおよびフィルターを登録できます。 参照してください、 [ASP.NET Core でのログ記録](xref:fundamentals/logging/index#how-to-add-providers)詳細についてはドキュメントです。

> [!NOTE]
> ログ プロバイダーを登録するために必要なパッケージをインストールする必要があります。 参照してください、[組み込みのログ記録プロバイダー](xref:fundamentals/logging/index#built-in-logging-providers)一覧についてはドキュメントの「します。

たとえば、コンソールのログ記録を有効にするをインストール、 `Microsoft.Extensions.Logging.Console` NuGet パッケージです。 呼び出す、`AddConsole`拡張メソッド。

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

JavaScript クライアントで、類似した`configureLogging`メソッドが存在します。 提供、`LogLevel`生成するためにログ メッセージの最小レベルを示す値。 ブラウザーのコンソール ウィンドウには、ログが書き込まれます。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> 完全ログ記録を無効にするには指定`signalR.LogLevel.None`で、`configureLogging`メソッドです。

ログ レベルは、JavaScript クライアントには次のとおりです。 ログ レベルをこれらの値のいずれかに設定でメッセージのログ記録を有効になります**以上**レベル。

| レベル | 説明 |
| ----- | ----------- |
| `None` | メッセージは記録されません。 |
| `Critical` | アプリ全体の失敗を示すメッセージ。 |
| `Error` | 現在の操作の失敗を示すメッセージ。 |
| `Warning` | 致命的でない問題を示すメッセージ。 |
| `Information` | 情報メッセージです。 |
| `Debug` | 診断メッセージのデバッグに役立ちます。 |
| `Trace` | 非常に詳細な診断メッセージが特定の問題を診断するために設計されています。 |

### <a name="configure-allowed-transports"></a>許可されているトランスポートを構成します。

SignalR によって使用されるトランスポートを構成することができます、`WithUrl`呼び出し (`withUrl` JavaScript で)。 値のビット演算 OR`HttpTransportType`だけ、指定されたトランスポートを使用するクライアントを制限するために使用できます。 既定では、すべてのトランスポートが有効にします。

たとえば、Server-Sent イベント転送を無効にするが、接続を許可する Websocket と長いポーリングします。

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

設定して、JavaScript クライアントで、トランスポートが構成されている、`transport`オプション オブジェクトに提供されるフィールド`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>ベアラー認証を構成します。

SignalR 要求と共に認証データを提供するには、使用、`AccessTokenProvider`オプション (`accessTokenFactory` JavaScript で) を対象とするアクセス トークンを返す関数を指定します。 .NET クライアントで、このアクセス トークンとして渡される HTTP「ベアラー認証」トークン (を使用して、`Authorization`の型を持つヘッダー `Bearer`)。 JavaScript クライアントで、アクセス トークンはベアラー トークンとして使用**を除く**まれにブラウザーの Api が (具体的には、Server-Sent イベントと Websocket 要求) 内のヘッダーを適用する機能を制限します。 このような場合は、アクセス トークンがクエリ文字列値として提供される`access_token`です。

.NET クライアントで、`AccessTokenProvider`でオプションのデリゲートを使用してオプションを指定することができます`WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

JavaScript クライアントで、アクセス トークンが構成されている設定によって、`accessTokenFactory`フィールド内のオプション オブジェクトに`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>タイムアウトとキープ アライブ オプションを構成します。

タイムアウトとキープ アライブ動作の構成の追加オプションが 利用可能な`HubConnection`オブジェクト自体。

| .NET オプション | JavaScript のオプション | 説明 |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | サーバーの利用状況のタイムアウト期間。 サーバーは、この間隔ですべてのメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`Closed`イベント (`onclose` JavaScript で)。 |
| `HandshakeTimeout` | 構成できません。 | サーバーの初期ハンドシェイクのタイムアウト期間。 クライアントが、ハンドシェイクおよびトリガーを取り消すサーバーがこの間隔のハンドシェイクの応答を送信しない場合、`Closed`イベント (`onclose` JavaScript で)。 |

.NET クライアントで、タイムアウトは値として指定`TimeSpan`値。 JavaScript クライアントでは、タイムアウト値を数値として指定します。 数値は、時刻の値 (ミリ秒単位) を表します。

### <a name="configure-additional-options"></a>追加のオプションを構成します。

追加のオプションを構成することができます、 `WithUrl` (`withUrl` JavaScript で) メソッドを`HubConnectionBuilder`:

| .NET オプション | JavaScript のオプション | 説明 |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | HTTP 要求にベアラー認証トークンとして提供される文字列を返す関数。 |
| `SkipNegotiation` | `skipNegotiation` | これを設定して`true`ネゴシエーションの手順をスキップします。 **Websocket トランスポートが、唯一の有効なトランスポートの場合のみサポート**です。 Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。 |
| `ClientCertificates` | 構成できない * | 要求の認証に送信する TLS 証明書のコレクション。 |
| `Cookies` | 構成できない * | すべての HTTP 要求を送信する HTTP クッキーのコレクション。 |
| `Credentials` | 構成できない * | すべての HTTP 要求を送信する資格情報。 |
| `CloseTimeout` | 構成できない * | Websocket でのみ使用します。 最大時間数、クライアントは、サーバーが閉じる要求を確認するための終了後に待機します。 サーバーは、この時間内、閉じるを認識しない、クライアントが切断されます。 |
| `Headers` | 構成できない * | すべての HTTP 要求を送信する追加の HTTP ヘッダーのディクショナリ。 |
| `HttpMessageHandlerFactory` | 構成できない * | 構成または置換するために使用するデリゲート、 `HttpMessageHandler` HTTP 要求を送信するために使用します。 WebSocket 接続に使用されません。 このデリゲートは null 以外の値を返す必要があり、既定値をパラメーターとして受け取ります。 その既定値の設定を変更し、取得、またはまったく新しい返す`HttpMessageHandler`インスタンス。 |
| `Proxy` | 構成できない * | HTTP 要求を送信するときに使用する HTTP プロキシ。 |
| `UseDefaultCredentials` | 構成できない * | HTTP や Websocket の要求の既定の資格情報を送信するこのブール値を設定します。 これにより、Windows 認証を使用できます。 |
| `WebSocketConfiguration` | 構成できない * | 追加の WebSocket オプションを構成するために使用するデリゲート。 インスタンスを受け取り[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)オプションの構成を使用できます。 |

アスタリスク (*) でマークされた」オプションは、ブラウザーの Api の制限のための JavaScript クライアントで構成できます。

.NET クライアントで、これらのオプションでは変更に提供されるオプション デリゲート`WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

指定された JavaScript オブジェクトで、JavaScript クライアントで、これらのオプションを指定できます`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>その他の技術情報

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
