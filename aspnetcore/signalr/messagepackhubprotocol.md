---
title: ASP.NET core SignalR で MessagePack Hub プロトコルを使用します。
author: bradygaster
description: ASP.NET Core SignalR には、MessagePack Hub プロトコルを追加します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 2852ca93c62e706e9a5203625822c2fb954fd2b8
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54835610"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a>ASP.NET core SignalR で MessagePack Hub プロトコルを使用します。

によって[真紀 Brennan](https://github.com/BrennanConroy)

この記事では、リーダーがで説明するトピックを熟知前提としています。[開始](xref:tutorials/signalr)します。

## <a name="what-is-messagepack"></a>MessagePack とは何ですか。

[MessagePack](https://msgpack.org/index.html)は高速でコンパクトなバイナリ シリアル化形式です。 パフォーマンスと帯域幅が、問題になると比較して小さいメッセージを作成するため、場合に便利です[JSON](https://www.json.org/)します。 バイナリ形式であるため、メッセージは、読み取り可能なバイトが MessagePack パーサーから渡されていない限り、ネットワーク トレースとログを見ると。 SignalR は、MessagePack 形式の組み込みサポートを備え、使用するには、クライアントとサーバーの Api を提供します。

## <a name="configure-messagepack-on-the-server"></a>MessagePack をサーバーを構成します。

サーバーの MessagePack Hub プロトコルを有効にするにはインストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージをアプリにします。 Startup.cs ファイルで追加`AddMessagePackProtocol`を`AddSignalR`呼び出し、サーバー上の MessagePack サポートを有効にします。

> [!NOTE]
> JSON は、既定で有効です。 MessagePack を追加すると、JSON、MessagePack の両方のクライアントのサポートができます。

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

MessagePack が、データを書式設定する方法をカスタマイズする`AddMessagePackProtocol`オプションを構成するデリゲートを受け取る。 そのデリゲートで、 `FormatterResolvers` MessagePack シリアル化オプションを構成するプロパティを使用できます。 競合回避モジュールの動作方法の詳細については、MessagePack ライブラリを参照してください。 [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)します。 属性は、処理方法を定義するシリアル化するオブジェクトで使用できます。

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

## <a name="configure-messagepack-on-the-client"></a>MessagePack client を構成します。

> [!NOTE]
> JSON は、サポートされているクライアントの既定で有効です。 クライアントは、1 つのプロトコルのみをサポートできます。 構成されたプロトコルを MessagePack サポートは置き換える前に追加します。

### <a name="net-client"></a>.NET クライアント

MessagePack、.NET クライアントを有効にするにはインストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージと呼び出し`AddMessagePackProtocol`で`HubConnectionBuilder`します。

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> これは、`AddMessagePackProtocol`呼び出しは、サーバーと同様のオプションを構成するためのデリゲートを受け取ります。

### <a name="javascript-client"></a>JavaScript クライアント

Javascript クライアント MessagePack サポートが提供、 `@aspnet/signalr-protocol-msgpack` NPM パッケージ。

```console
npm install @aspnet/signalr-protocol-msgpack
```

Npm パッケージをインストールした後、モジュール、JavaScript モジュール ローダーを使用して直接使用したり参照することで、ブラウザーにインポート、 *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* ファイル。 ブラウザーで、`msgpack5`ライブラリが参照することも必要があります。 使用して、`<script>`参照を作成するタグ。 ライブラリに掲載する*node_modules\msgpack5\dist\msgpack5.js*します。

> [!NOTE]
> 使用する場合、`<script>`要素の順序は重要です。 場合*signalr-プロトコル-msgpack.js*する前に参照が*msgpack5.js*MessagePack で接続する際にエラーが発生します。 *signalr.js*する必要がありますも*signalr-プロトコル-msgpack.js*します。

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

追加`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`を`HubConnectionBuilder`MessagePack プロトコルを使用して、サーバーに接続するときにクライアントを構成します。

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> この時点では、JavaScript クライアントでは、MessagePack プロトコルの構成オプションはありません。

## <a name="related-resources"></a>関連資料

* [開始するには](xref:tutorials/signalr)
* [.NET クライアント](xref:signalr/dotnet-client)
* [JavaScript クライアント](xref:signalr/javascript-client)
