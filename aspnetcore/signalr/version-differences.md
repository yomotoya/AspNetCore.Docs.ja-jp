---
title: SignalR と ASP.NET Core SignalR の違い
author: tdykstra
description: SignalR と ASP.NET Core SignalR の違い
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: ea2de2606a99de70fa645c0c42303525fea0a44e
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325537"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>ASP.NET SignalR、ASP.NET Core SignalR の相違点

ASP.NET Core SignalR は、クライアントまたは ASP.NET SignalR のサーバーとの互換性はありません。 この記事では、削除または ASP.NET Core SignalR で変更された機能について説明します。

## <a name="how-to-identify-the-signalr-version"></a>SignalR のバージョンを識別する方法

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| サーバーの NuGet パッケージ | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| クライアントの NuGet パッケージ | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| クライアントの npm パッケージ | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| サーバー アプリの種類 | ASP.NET (System.Web) または OWIN 自己ホスト | ASP.NET Core |
| サーバーがサポートされているプラットフォーム | .NET framework 4.5 またはそれ以降 | .NET Framework 4.6.1 以降<br>.NET core 2.1 以降 |

## <a name="feature-differences"></a>機能の相違点

### <a name="automatic-reconnects"></a>自動再接続

自動再接続は、ASP.NET Core SignalR でサポートされていません。 クライアントが切断された場合は、再接続する場合は、ユーザーは明示的に新しい接続を開始する必要があります。 ASP.NET SignalR、SignalR は、接続が削除された場合、サーバーに再接続しようとします。 

### <a name="protocol-support"></a>プロトコルのサポート

ASP.NET Core SignalR は、JSON、ほかに基づく新しいバイナリ プロトコルをサポート[MessagePack](xref:signalr/messagepackhubprotocol)します。 さらに、カスタム プロトコルを作成できます。

## <a name="differences-on-the-server"></a>サーバー上の相違点

ASP.NET Core SignalR のサーバー側のライブラリに含まれる、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)パッケージの一部である、 **ASP.NET Core Web アプリケーション**Razor と MVC の両方のテンプレートプロジェクト。

呼び出すことによって構成する必要がありますので、ASP.NET Core SignalR は ASP.NET Core のミドルウェア、 [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr)で`Startup.ConfigureServices`します。

```csharp
services.AddSignalR()
```

ルーティングを構成するには、ハブ内にルートをマップ、 [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr)でメソッドを呼び出す、`Startup.Configure`メソッド。

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>スティッキー セッションが必要になりました

により、どのようにスケール アウトは、ASP.NET SignalR で作業した、クライアントは再接続し、ファーム内の任意のサーバーにメッセージを送信でした。 再接続をサポートしていないほか、スケール アウト モデルの変更によりこれがサポートされていません。 クライアントは、サーバーに接続すると、接続の間の同じサーバーと対話する必要があります。

### <a name="single-hub-per-connection"></a>接続ごとに 1 つのハブ

ASP.NET Core signalr では、接続モデルを簡略化されました。 複数のハブへのアクセスを共有するために使用されている 1 つの接続ではなく、1 つのハブに直接接続が行われます。

### <a name="streaming"></a>ストリーム

ASP.NET Core SignalR なりました[ストリーミング データ](xref:signalr/streaming)クライアント ハブから。

### <a name="state"></a>状態

進行状況メッセージのサポートと、クライアントと (HubState と呼ばれる多くの場合)、ハブ間で任意の状態を渡す機能が削除されました。 現時点では、ハブ プロキシの対応はありません。

## <a name="differences-on-the-client"></a>クライアント上の相違点

### <a name="typescript"></a>TypeScript

ASP.NET Core SignalR クライアントがで記述された[TypeScript](https://www.typescriptlang.org/)します。 使用する場合は、JavaScript または TypeScript で記述することができます、 [JavaScript クライアント](xref:signalr/javascript-client)します。

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>ホストされている JavaScript クライアント[npm](https://www.npmjs.com/)

以前のバージョンで、JavaScript クライアントは、Visual Studio で NuGet パッケージから取得されました。 Core のバージョン、 [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) npm パッケージには、JavaScript ライブラリが含まれています。 このパッケージに含まれていない、 **ASP.NET Core Web アプリケーション**テンプレート。 Npm を入手してインストールを使用して、 `@aspnet/signalr` npm パッケージ。

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

JQuery に依存関係は削除されたが、プロジェクトは、jQuery を引き続き使用できます。

### <a name="javascript-client-method-syntax"></a>JavaScript クライアント メソッドの構文

JavaScript 構文は、SignalR の以前のバージョンから変更されました。 使用してではなく、`$connection`オブジェクトを使用して接続を作成、 [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用して、[で](/javascript/api/@aspnet/signalr/HubConnection#on)ハブで呼び出すことができるクライアント メソッドを指定します。

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

クライアントのメソッドを作成した後、ハブの接続を開始します。 チェーンを[キャッチ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)ログまたはエラーを処理するメソッド。

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>ハブ プロキシ

ハブ プロキシが自動的に生成されます。 代わりに、メソッド名に渡される、[呼び出す](/javascript/api/%40aspnet/signalr/hubconnection#invoke)を文字列としての API。

### <a name="net-and-other-clients"></a>.NET およびその他のクライアント

`Microsoft.AspNetCore.SignalR.Client` NuGet パッケージには、ASP.NET Core SignalR の .NET クライアント ライブラリが含まれています。

使用して、 [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)作成し、ハブへの接続のインスタンスを構築します。

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>スケール アウトの違い

ASP.NET SignalR は、SQL Server と Redis の両方をサポートします。 ASP.NET Core SignalR は、Azure SignalR サービスと Redis の両方をサポートします。

### <a name="aspnet"></a>ASP.NET

* [Azure Service Bus による SignalR スケール アウト](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [Redis による SignalR スケール アウト](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SQL Server による SignalR スケール アウト](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Azure SignalR サービス](/azure/azure-signalr/)

## <a name="additional-resources"></a>その他の技術情報

* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [.NET クライアント](xref:signalr/dotnet-client)
* [サポートされているプラットフォーム](xref:signalr/supported-platforms)
