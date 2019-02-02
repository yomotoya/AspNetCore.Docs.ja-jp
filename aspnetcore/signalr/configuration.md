---
title: ASP.NET Core SignalR の構成
author: bradygaster
description: ASP.NET Core SignalR アプリケーションを構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/29/2019
uid: signalr/configuration
ms.openlocfilehash: ce970199984cdb8333ed1fd51f744dcda2df9c61
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667610"
---
# <a name="aspnet-core-signalr-configuration"></a>ASP.NET Core SignalR の構成

## <a name="jsonmessagepack-serialization-options"></a>JSON/MessagePack シリアル化オプション

ASP.NET Core SignalR には、メッセージをエンコードするための 2 つのプロトコルがサポートされています。[JSON](https://www.json.org/)と[MessagePack](https://msgpack.org/index.html)します。 各プロトコルには、シリアル化の構成オプションがあります。

JSON のシリアル化を使用して、サーバーで構成できます、 [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol)拡張メソッドは後に、追加する[AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で、`Startup.ConfigureServices`メソッド。 `AddJsonProtocol`メソッドが受信するデリゲートを受け取り、`options`オブジェクト。 [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings)そのオブジェクトのプロパティは、JSON.NET`JsonSerializerSettings`引数のシリアル化の構成し、戻り値を使用できるオブジェクト。 参照してください、 [JSON.NET のドキュメント](https://www.newtonsoft.com/json/help/html/Introduction.htm)の詳細。

たとえば、「キャメル ケース」の既定名ではなく"PascalCase"プロパティの名前を使用するシリアライザーを構成する次のコードを使用します。

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

.NET クライアントで同じ`AddJsonProtocol`に拡張メソッドが存在する[HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)します。 `Microsoft.Extensions.DependencyInjection`拡張メソッドを解決するのには名前空間をインポートする必要があります。

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> この時点で、JavaScript クライアント内で JSON のシリアル化を構成することはできません。

### <a name="messagepack-serialization-options"></a>MessagePack シリアル化オプション

デリゲートを提供することで MessagePack シリアル化を構成することができます、 [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol)呼び出します。 参照してください[SignalR で MessagePack](xref:signalr/messagepackhubprotocol)の詳細。

> [!NOTE]
> この時点で、JavaScript クライアント内で MessagePack シリアル化を構成することはできません。

## <a name="configure-server-options"></a>サーバー オプションを構成します。

次の表では、SignalR ハブを構成するためのオプションについて説明します。

| オプション | 既定値 | 説明 |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 秒 | サーバーは、クライアントを検討してください、この間隔で (キープ アライブを含む) メッセージを受信していない場合は切断されます。 実際にマーク済みであるため、これを実装する方法、切断されたクライアントの場合は、このタイムアウト間隔よりも長い時間がかかります。 推奨値は二重、`KeepAliveInterval`値。|
| `HandshakeTimeout` | 15 秒 | クライアントがこの時間間隔内で最初のハンドシェイク メッセージを送信しない場合、接続は閉じられます。 これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。 ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。 |
| `KeepAliveInterval` | 15 秒 | サーバーがこの間隔内でメッセージを送信していない場合、接続を開いたままに ping メッセージが自動的に送信されます。 変更するときに`KeepAliveInterval`、変更、 `ServerTimeout` / `serverTimeoutInMilliseconds`クライアントに設定します。 推奨される`ServerTimeout` / `serverTimeoutInMilliseconds`値が double、`KeepAliveInterval`値。  |
| `SupportedProtocols` | インストールされているすべてのプロトコル | このハブでサポートされるプロトコル。 既定では、サーバーに登録されているプロトコルをすべて許可されますが、プロトコルは、個々 のハブに対する特定のプロトコルを無効にするには、この一覧から削除できます。 |
| `EnableDetailedErrors` | `false` | 場合`true`詳細のハブ メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。 既定値は`false`、これらの例外メッセージは、機密情報を含めることができます。 |

オプションのデリゲートを提供することですべてのハブのオプションを構成でき、`AddSignalR`呼び出す`Startup.ConfigureServices`します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

1 つのハブのオプションで提供されるグローバル オプションを上書き`AddSignalR`を使用して構成できると[AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

使用`HttpConnectionDispatcherOptions`トランスポートおよびメモリ バッファーの管理に関連する高度な設定を構成します。 これらのオプションを構成するデリゲートを渡すことによって[MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub)します。

| オプション | 既定値 | 説明 |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | クライアントから受信したバイトの最大数をサーバーのバッファー。 この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。 |
| `AuthorizationData` | データが自動的に収集、`Authorize`ハブ クラスに適用される属性。 | 一連の[IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata)オブジェクトは、クライアントがハブへの接続を承認されているかどうかを判断するために使用します。 |
| `TransportMaxBufferSize` | 32 KB | アプリによって送信されたバイト数の最大数をサーバーのバッファー。 この値を増やすことによりより大きなメッセージを送信するサーバーが、メモリ消費量に悪影響を与えることができます。 |
| `Transports` | すべてのトランスポートが有効になります。 | ビットマスク`HttpTransportType`トランスポートを制限する値をクライアントが接続に使用できます。 |
| `LongPolling` | 以下を参照してください。 | 長いポーリングのトランスポートに固有の追加オプション。 |
| `WebSockets` | 以下を参照してください。 | Websocket トランスポートに固有の追加オプション。 |

ポーリングの長いトランスポートを使用して構成できるその他のオプションを持つ、`LongPolling`プロパティ。

| オプション | 既定値 | 説明 |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 秒 | 1 つのポーリング要求を終了する前に、クライアントに送信するメッセージのサーバーが待機する最長時間。 新しいポーリング要求をより頻繁に発行するクライアントは、この値を小さきます。 |

WebSocket トランスポートが追加のオプションを使用して構成できる、`WebSockets`プロパティ。

| オプション | 既定値 | 説明 |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 秒 | サーバーが閉じ、クライアントがこの時間間隔内に閉じるが失敗した場合、接続は終了します。 |
| `SubProtocolSelector` | `null` | 設定するために使用するデリゲート、`Sec-WebSocket-Protocol`ヘッダーをカスタム値。 デリゲートは、入力としてクライアントによって要求された値を受け取るし、目的の値を返します。 |

## <a name="configure-client-options"></a>クライアント オプションを構成します。

クライアント オプションを構成できる、 `HubConnectionBuilder` (.NET と JavaScript の両方のクライアントで使用できる)、型ではなく、`HubConnection`自体。

### <a name="configure-logging"></a>ログを構成します。

.NET を使用してクライアントでログ記録が構成されている、`ConfigureLogging`メソッド。 サーバー上にある、同じ方法でログ記録プロバイダーおよびフィルターを登録できます。 参照してください、 [ASP.NET Core でのログ記録](xref:fundamentals/logging/index)詳細についてはドキュメントです。

> [!NOTE]
> ログ プロバイダーを登録するために必要なパッケージをインストールする必要があります。 参照してください、[組み込みのログ プロバイダー](xref:fundamentals/logging/index#built-in-logging-providers)完全な一覧については、ドキュメントのセクション。

たとえば、コンソールのログ記録を有効にするインストール、 `Microsoft.Extensions.Logging.Console` NuGet パッケージ。 呼び出す、`AddConsole`拡張メソッド。

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

同様に、JavaScript クライアント内で`configureLogging`メソッドが存在します。 提供、`LogLevel`を生成するログ メッセージの最小レベルを示す値。 ブラウザーのコンソール ウィンドウには、ログが書き込まれます。

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> 完全ログ記録を無効にするには指定`signalR.LogLevel.None`で、`configureLogging`メソッド。

JavaScript クライアントに利用可能なログ レベルは、以下に示します。 ログ レベルをこれらの値のいずれかに設定でメッセージのログ記録を有効になります**以上**レベル。

| レベル | 説明 |
| ----- | ----------- |
| `None` | メッセージは記録されません。 |
| `Critical` | アプリ全体で障害を示すメッセージ。 |
| `Error` | 現在の操作の失敗を示すメッセージ。 |
| `Warning` | 致命的でない問題を示すメッセージ。 |
| `Information` | 情報メッセージ。 |
| `Debug` | 診断メッセージのデバッグに役立ちます。 |
| `Trace` | 非常に詳細な診断メッセージが特定の問題を診断するために設計されています。 |

### <a name="configure-allowed-transports"></a>許可されているトランスポートを構成します。

SignalR によって使用されるトランスポートで構成できる、`WithUrl`を呼び出す (`withUrl` JavaScript で)。 値のビットごとの OR`HttpTransportType`のみ指定されたトランスポートを使用するクライアントを制限するために使用できます。 既定では、すべてのトランスポートが有効にします。

たとえば、Server-Sent イベント転送を無効にすることを許可する WebSockets および長いポーリングの接続。

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

設定して、JavaScript クライアントでのトランスポートが構成されている、`transport`フィールドに指定されたオプションのオブジェクトに`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>ベアラー認証を構成します。

SignalR 要求と共に認証データを提供するには、使用、`AccessTokenProvider`オプション (`accessTokenFactory` JavaScript で) 必要なアクセス トークンを返す関数を指定します。 .NET クライアントでこのアクセス トークンとして渡される HTTP「ベアラー認証」トークン (を使用して、`Authorization`ヘッダーの型を持つ`Bearer`)。 アクセス トークンをベアラー トークンとして使用、JavaScript クライアントで**を除く**ブラウザー Api が (具体的には、Server-Sent イベントと Websocket の要求) のヘッダーを適用する機能を制限するような場合。 このような場合は、アクセス トークンはクエリ文字列値として指定`access_token`します。

.NET クライアントで、`AccessTokenProvider`でオプションのデリゲートを使用してオプションを指定することができます`WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

設定して、JavaScript クライアントでアクセス トークンが構成されている、`accessTokenFactory`フィールド内のオプション オブジェクトに`withUrl`:

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

タイムアウトとキープ アライブ動作を構成するための追加のオプションがあります、`HubConnection`オブジェクト自体。

| .NET オプション | JavaScript のオプション | 既定値 | 説明 |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | 30 秒 (30,000 ミリ秒) | サーバーの利用状況のタイムアウト。 サーバーは、この間隔でメッセージを送信していない、クライアントと見なされ、サーバーが切断されているトリガー、`Closed`イベント (`onclose` JavaScript で)。 この値は、サーバーから送信される ping メッセージに十分な大きさである必要があります**と**タイムアウト間隔内に、クライアントによって受信されます。 推奨値は数値、サーバーに少なくとも 2 倍の`KeepAliveInterval`の ping が到着するまでの値。 |
| `HandshakeTimeout` | 構成できません。 | 15 秒 | サーバーの初期ハンドシェイクのタイムアウト。 サーバーは、この間隔でハンドシェイクの応答を送信しない場合、は、クライアントがキャンセル ハンドシェイクとトリガー、`Closed`イベント (`onclose` JavaScript で)。 これは、重大なネットワーク待機時間が原因のハンドシェイクのタイムアウト エラーが発生している場合にのみ変更する高度な設定です。 ハンドシェイク プロセスの詳細については、次を参照してください。、 [SignalR ハブ プロトコル仕様](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)します。 |

として、.NET クライアントでタイムアウト値が指定された`TimeSpan`値。 JavaScript クライアントでは、タイムアウト値は、期間 (ミリ秒) を示す数値として指定されます。

### <a name="configure-additional-options"></a>追加のオプションを構成します。

追加のオプションを構成することができます、 `WithUrl` (`withUrl` JavaScript で) メソッドを`HubConnectionBuilder`:

| .NET オプション | JavaScript のオプション | 既定値 | 説明 |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | HTTP 要求でベアラー認証トークンとして提供されている文字列を返す関数。 |
| `SkipNegotiation` | `skipNegotiation` | `false` | これを設定`true`ネゴシエーションの手順をスキップします。 **Websocket トランスポートがトランスポートには、のみの有効な場合にのみサポート**します。 Azure SignalR サービスを使用する場合は、この設定を有効にすることはできません。 |
| `ClientCertificates` | 構成できない * | Empty | 要求の認証に送信する TLS 証明書のコレクション。 |
| `Cookies` | 構成できない * | Empty | すべての HTTP 要求を送信する HTTP クッキーのコレクション。 |
| `Credentials` | 構成できない * | Empty | すべての HTTP 要求を送信する資格情報です。 |
| `CloseTimeout` | 構成できない * | 5 秒 | Websocket だけです。 最長時間、クライアントは、サーバーが閉じる要求を確認するための終了後に待機します。 サーバーは、この時間内終了を確認は、クライアントが切断されます。 |
| `Headers` | 構成できない * | Empty | すべての HTTP 要求を送信する追加の HTTP ヘッダーのディクショナリ。 |
| `HttpMessageHandlerFactory` | 構成できない * | `null` | 構成または置換するために使用するデリゲート、 `HttpMessageHandler` HTTP 要求を送信するために使用します。 WebSocket 接続に使用されません。 このデリゲートは、null 以外の値を返す必要があり、既定値をパラメーターとして受け取ります。 その既定値の設定を変更し、それを返すかが返さ新しい`HttpMessageHandler`インスタンス。 **ときは、ハンドラーを置き換える、指定したハンドラーから保持する設定をコピーすることを確認して、それ以外の場合、(Cookie やヘッダー) の構成オプションが新しいハンドラーを適用されません。** |
| `Proxy` | 構成できない * | `null` | HTTP 要求を送信するときに使用する HTTP プロキシ。 |
| `UseDefaultCredentials` | 構成できない * | `false` | HTTP と Websocket の要求の既定の資格情報を送信するこのブール値を設定します。 これにより、Windows 認証を使用できます。 |
| `WebSocketConfiguration` | 構成できない * | `null` | 追加の WebSocket オプションを構成するために使用するデリゲート。 インスタンスを受け取る[ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions)オプションを構成できます。 |

アスタリスク (*) でマークされたオプションはブラウザー Api の制限により、JavaScript クライアントで構成できます。

指定されたオプションのデリゲートで、.NET クライアントでこれらのオプションを変更できます`WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

指定された JavaScript オブジェクトで、JavaScript クライアントでこれらのオプションを指定できる`withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a>その他の技術情報

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
