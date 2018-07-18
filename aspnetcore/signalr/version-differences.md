---
title: SignalR と ASP.NET Core SignalR の違い
author: tdykstra
description: SignalR と ASP.NET Core SignalR の違い
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095009"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>SignalR と ASP.NET Core SignalR の違い

ASP.NET Core SignalR は、クライアントまたは ASP.NET SignalR のサーバーと互換性がありません。 この記事では、削除されたか、ASP.NET Core SignalR で変更する機能を説明します。

## <a name="feature-differences"></a>機能の相違点

### <a name="automatic-reconnects"></a>自動再接続

自動再接続がサポートされていません。 以前は、SignalR は、接続が切断された場合、サーバーに再接続しようとします。 ここでは、クライアントが切断された場合に再接続する場合は、ユーザーは新しい接続を開始に明示的にする必要があります。

### <a name="protocol-support"></a>プロトコルのサポート

ASP.NET Core SignalR は、JSON、ほかに基づく新しいバイナリ プロトコルをサポート[MessagePack](xref:signalr/messagepackhubprotocol)します。 さらに、カスタム プロトコルを作成できます。

## <a name="differences-on-the-server"></a>サーバー上の相違点

SignalR のサーバー側のライブラリに含まれる、`Microsoft.AspNetCore.App`パッケージの一部である、 **ASP.NET Core Web アプリケーション**Razor と MVC の両方のプロジェクトのテンプレート。

呼び出すことによって構成する必要がありますので、SignalR は ASP.NET Core のミドルウェア、`AddSignalR`で`Startup.ConfigureServices`します。

```csharp
services.AddSignalR();
```

ルーティングを構成するには、ハブ内にルートをマップ、`UseSignalR`でメソッドを呼び出す、`Startup.Configure`メソッド。

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>スティッキー セッションが必要になりました

により、どのようにスケール アウトは、SignalR の以前のバージョンで作業した、クライアントは再接続し、ファーム内の任意のサーバーにメッセージを送信でした。 再接続をサポートしていないほか、スケール アウト モデルの変更によりこれがサポートされていません。 次に、クライアントがサーバーに接続すると、接続の間のと同じサーバーの対話が必要があります。

### <a name="single-hub-per-connection"></a>接続ごとに 1 つのハブ

ASP.NET Core signalr では、接続モデルを簡略化されました。 複数のハブへのアクセスを共有するために使用されている 1 つの接続ではなく、1 つのハブに直接接続が行われます。

### <a name="streaming"></a>ストリーム

SignalR ようになりました[ストリーミング データ](xref:signalr/streaming)クライアントにハブから。

### <a name="state"></a>状態

進行状況メッセージのサポートと、クライアントと (HubState と呼ばれる多くの場合)、ハブ間で任意の状態を渡す機能が削除されました。 現時点では、ハブ プロキシの対応はありません。

## <a name="differences-on-the-client"></a>クライアント上の相違点

### <a name="typescript"></a>TypeScript

ASP.NET Core のバージョンの SignalR が記述された[TypeScript](https://www.typescriptlang.org/)します。 使用する場合は、JavaScript または TypeScript で記述することができます、 [JavaScript クライアント](xref:signalr/javascript-client)します。

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>ホストされている JavaScript クライアント[npm](https://www.npmjs.com/)

以前のバージョンで、JavaScript クライアントは、Visual Studio で NuGet パッケージから取得されました。 Core のバージョン、 [ @aspnet/signalr npm パッケージ](https://www.npmjs.com/package/@aspnet/signalr)JavaScript ライブラリが含まれています。 このパッケージに含まれていない、 **ASP.NET Core Web アプリケーション**テンプレート。 Npm を入手してインストールを使用して、 `@aspnet/signalr` npm パッケージ。

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

JQuery に依存関係は削除されたが、プロジェクトは、jQuery を引き続き使用できます。

### <a name="javascript-client-method-syntax"></a>JavaScript クライアント メソッドの構文

JavaScript 構文は、SignalR の以前のバージョンから変更されました。 使用してではなく、`$connection`オブジェクトを使用して接続を作成、 `HubConnectionBuilder` API。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用`connection.on`ハブで呼び出すことができるクライアント メソッドを指定します。

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

クライアントのメソッドを作成した後、ハブの接続を開始します。 チェーンを`catch`ログまたはエラーを処理するメソッド。

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>ハブ プロキシ

ハブ プロキシが自動的に生成されます。 代わりに、メソッド名に渡される、`invoke`を文字列としての API。

### <a name="net-and-other-clients"></a>.NET およびその他のクライアント

`Microsoft.AspNetCore.SignalR.Client` NuGet パッケージには、ASP.NET Core SignalR の .NET クライアント ライブラリが含まれています。

使用して、`HubConnectionBuilder`作成し、ハブへの接続のインスタンスを構築します。

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>その他の技術情報

* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [.NET クライアント](xref:signalr/dotnet-client)
* [サポートされているプラットフォーム](xref:signalr/supported-platforms)
