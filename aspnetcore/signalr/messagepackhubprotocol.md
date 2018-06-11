---
title: ASP.NET Core 用 SignalR で MessagePack ハブ プロトコルを使用します。
author: rachelappel
description: ASP.NET Core SignalR を MessagePack ハブ プロトコルを追加します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: b6c33c4da47a19d67bffbaf84f54d59013edadbe
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252480"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>ASP.NET Core 用 SignalR で MessagePack ハブ プロトコルを使用します。

によって[真紀 Brennan](https://github.com/BrennanConroy)

この記事で説明されているトピックを使い慣れているリーダーが前提としています[開始](xref:signalr/get-started)です。

## <a name="what-is-messagepack"></a>MessagePack とは何ですか。

[MessagePack](https://msgpack.org/index.html)高速でコンパクトなバイナリのシリアル化形式です。 パフォーマンスと帯域幅が、問題になると比較すると、小さいメッセージを作成するため便利です[JSON](https://www.json.org/)です。 バイナリ形式になっているためメッセージは読み取り可能バイト MessagePack パーサー経由で渡される限り、ネットワーク トレースとログを確認するとき SignalR は MessagePack 形式の組み込みサポートし、使用するには、クライアントとサーバーの Api を提供します。

## <a name="configure-messagepack-on-the-server"></a>サーバーで MessagePack を構成します。

MessagePack ハブ サーバーでプロトコルを有効にするには、インストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`アプリのパッケージです。 Startup.cs ファイルで追加`AddMessagePackProtocol`を`AddSignalR`呼び出し、サーバー上の MessagePack サポートを有効にします。

> [!NOTE]
> JSON は既定で有効にします。 MessagePack を追加すると、JSON と MessagePack の両方のクライアントのサポートができます。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

MessagePack が、データを書式設定方法をカスタマイズする`AddMessagePackProtocol`オプションを構成するデリゲートを受け取ります。 そのデリゲート内、 `FormatterResolvers` MessagePack シリアル化オプションを構成するプロパティを使用できます。 競合回避モジュールの動作方法の詳細については、MessagePack ライブラリを参照してください。 [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)です。 属性は、処理方法を定義するシリアル化オブジェクトで使用できます。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="configure-messagepack-on-the-client"></a>クライアントで MessagePack を構成します。

### <a name="net-client"></a>.NET クライアント

.NET クライアントで MessagePack を有効にするには、インストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージと呼び出し`AddMessagePackProtocol`で`HubConnectionBuilder`です。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> これは、`AddMessagePackProtocol`呼び出しが、サーバーと同じようにオプションを構成するデリゲートを受け取ります。

### <a name="javascript-client"></a>JavaScript クライアント

Javascript クライアント MessagePack サポートによって提供されます、 `@aspnet/signalr-protocol-msgpack` NPM パッケージです。

```console
npm install @aspnet/signalr-protocol-msgpack
```

Npm パッケージをインストールすると、モジュール、JavaScript のモジュール ローダー経由で直接使用インポートしたりできますが、ブラウザーを参照して、 *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* ファイル。 ブラウザーで、`msgpack5`ライブラリが参照することも必要があります。 使用して、`<script>`タグへの参照を作成します。 ライブラリはあります*node_modules\msgpack5\dist\msgpack5.js*です。

> [!NOTE]
> 使用する場合、`<script>`要素、順序は重要です。 場合*signalr プロトコル-msgpack.js*する前に参照される*msgpack5.js*MessagePack で接続しようとするときにエラーが発生します。 *signalr.js*も必要とする前に*signalr プロトコル-msgpack.js*です。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

追加`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`を`HubConnectionBuilder`はサーバーに接続するときに、MessagePack プロトコルを使用するクライアントを構成します。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> この時点では、JavaScript クライアントで MessagePack プロトコルの構成オプションはありません。

## <a name="related-resources"></a>関連資料

* [開始するには](xref:signalr/get-started)
* [.NET クライアント](xref:signalr/dotnet-client)
* [JavaScript クライアント](xref:signalr/javascript-client)
