---
title: SignalR と ASP.NET Core SignalR の相違点
author: rachelappel
description: SignalR と ASP.NET Core SignalR の相違点
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090065"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>SignalR と ASP.NET Core SignalR の相違点

ASP.NET Core SignalR は、クライアントまたは ASP.NET SignalR のサーバーと互換性がありません。 この記事では、削除または ASP.NET Core SignalR で変更された機能について説明します。

## <a name="feature-differences"></a>機能の相違点

### <a name="automatic-reconnects"></a>自動再接続

自動再接続がサポートされていません。 以前は、SignalR では、接続が削除された場合、サーバーに再接続しようとしました。 ここでは、クライアントが切断された場合に再接続する場合は、ユーザーは新しい接続を開始に明示的にする必要があります。

### <a name="protocol-support"></a>プロトコルのサポート

ASP.NET Core SignalR に基づく新しいバイナリ プロトコルと同様に、JSON をサポートしている[MessagePack](xref:signalr/messagepackhubprotocol)です。 さらに、カスタム プロトコルを作成することができます。

## <a name="differences-on-the-server"></a>サーバー上の相違点

SignalR のサーバー側のライブラリに含まれる、`Microsoft.AspNetCore.App`パッケージの一部である、 **ASP.NET Core Web アプリケーション**Razor と MVC プロジェクトの両方のテンプレートです。

SignalR には、ASP.NET Core ミドルウェアがあるため呼び出すことによって構成する必要がありますに`AddSignalR`で`Startup.ConfigureServices`です。

```csharp
services.AddSignalR();
```

ルーティングを構成するには、ハブ内にルートをマップ、`UseSignalR`メソッドの呼び出し、`Startup.Configure`メソッドです。

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>スティッキー セッションが必要になりました

どのように対応するスケール アウト SignalR の以前のバージョンで、作業のためクライアントでした再接続し、ファーム内の任意のサーバーにメッセージを送信します。 変更されたのため、スケール アウト モデル、およびに再接続をサポートしていない、これはサポートされていません。 ここで、クライアントがサーバーに接続したら、接続の間の同じサーバーと対話する必要があります。

### <a name="single-hub-per-connection"></a>接続ごとに 1 つのハブ

ASP.NET Core SignalR で接続モデルを簡素化されています。 複数のハブへのアクセスを共有するために使用されている 1 つの接続ではなく、1 つのハブに直接接続が行われます。

### <a name="streaming"></a>ストリーム

SignalR なりました[ストリーミング データ](xref:signalr/streaming)クライアント ハブからです。

### <a name="state"></a>状態

進行状況メッセージのサポートに加えて、クライアントと、ハブ (HubState と呼びます) 間で任意の状態を渡す機能が削除されましたがします。 現時点では、相当するハブ プロキシはありません。

## <a name="differences-on-the-client"></a>クライアント上の相違点

### <a name="typescript"></a>TypeScript

SignalR の ASP.NET Core バージョンがで記述された[TypeScript](https://www.typescriptlang.org/)です。 使用する場合は、JavaScript または TypeScript で記述することができます、 [JavaScript クライアント](xref:signalr/javascript-client)です。

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>JavaScript クライアントがホストされている[npm](https://www.npmjs.com/)

以前のバージョンでの JavaScript クライアントは、Visual Studio での NuGet パッケージから取得されました。 コアのバージョンの[ @aspnet/signalr npm パッケージ](https://www.npmjs.com/package/@aspnet/signalr)JavaScript ライブラリが含まれています。 このパッケージに含まれていない、 **ASP.NET Core Web アプリケーション**テンプレート。 Npm を使用して、入手してインストール、 `@aspnet/signalr` npm パッケージです。

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

JQuery への依存関係が削除されました、ただし、プロジェクトは、jQuery を引き続き使用できます。

### <a name="javascript-client-method-syntax"></a>JavaScript クライアント メソッドの構文

JavaScript 構文は、SignalR の以前のバージョンから変更されました。 使用するのではなく、`$connection`オブジェクトを使用して接続を作成、 `HubConnectionBuilder` API です。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

使用して`connection.on`ハブで呼び出すことができるクライアント メソッドを指定します。

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

クライアントのメソッドを作成すると、ハブ接続を開始します。 チェーン、`catch`ログまたはエラーを処理するメソッド。

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>ハブ プロキシ

ハブ プロキシが自動的に行われなく生成されます。 代わりに、メソッド名に渡される、 `invoke` API には文字列です。

### <a name="net-and-other-clients"></a>.NET およびその他のクライアント

`Microsoft.AspNetCore.SignalR.Client` NuGet パッケージには、ASP.NET Core SignalR 用の .NET クライアント ライブラリが含まれています。

使用して、`HubConnectionBuilder`を作成し、ハブへの接続のインスタンスを構築します。

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
