---
title: ASP.NET core SignalR で MessagePack Hub プロトコルを使用します。
author: bradygaster
description: ASP.NET Core SignalR には、MessagePack Hub プロトコルを追加します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/27/2019
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 7742f6f8bb53fb3c299ff98ae52a0da519ff396c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896519"
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

JavaScript クライアント MessagePack サポートが提供、 `@aspnet/signalr-protocol-msgpack` npm パッケージ。

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

## <a name="messagepack-quirks"></a>MessagePack quirks

MessagePack Hub プロトコルを使用する場合の注意すべきいくつかの問題があります。

### <a name="messagepack-is-case-sensitive"></a>MessagePack 小文字が区別されます。

MessagePack プロトコルは、大文字小文字を区別します。 たとえば、次C#クラス。

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

使用する必要があります、JavaScript クライアントからの送信時に`PascalCased`プロパティの名前、大文字と小文字が一致する必要がありますので、C#クラスを正確にします。 例:

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

使用して`camelCased`名に正しくバインドされません、C#クラス。 使用して、これを回避することができますできます、 `Key` MessagePack プロパティに別の名前を指定する属性。 詳細については、次を参照してください。 [MessagePack CSharp ドキュメント](https://github.com/neuecc/MessagePack-CSharp#object-serialization)します。

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a>シリアル化/逆シリアル化時 DateTime.Kind は保持されません。

MessagePack プロトコルは、エンコードする方法を提供しません、`Kind`の値を`DateTime`します。 結果として、日付を逆シリアル化するときに受信日付が UTC 形式で MessagePack ハブのプロトコルが想定します。 使用している場合`DateTime`現地時刻の値、お勧めしますに送信する前に、UTC に変換します。 乗り換え UTC から現地時刻に表示された場合。

この制限の詳細については、GitHub を参照してください。 問題[aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632)します。

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a>JavaScript で MessagePack によって DateTime.MinValue はサポートされていません

[Msgpack5](https://github.com/mcollina/msgpack5) SignalR JavaScript クライアントによって使用されるライブラリをサポートしていない、 `timestamp96` MessagePack 内の型。 この型は、非常に大きな日付の値 (過去または将来的に非常にまでに非常に早い段階か) のエンコードに使用されます。 値`DateTime.MinValue`は`January 1, 0001`でエンコードする必要があります、`timestamp96`値。 このため、送信のため`DateTime.MinValue`を javascript クライアントはサポートされていません。 ときに`DateTime.MinValue`が受信される、JavaScript クライアントで、次のエラーがスローされます。

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

通常、`DateTime.MinValue`エンコードが使用される、「が見つかりません」または`null`値。 MessagePack でその値をエンコードする必要がある場合は、null 許容使用`DateTime`値 (`DateTime?`)、別のエンコードまたは`bool`日付が存在するかどうかを示す値。

この制限の詳細については、GitHub を参照してください。 問題[aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)します。

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a>"Ahead の time"コンパイル環境で MessagePack サポート

[MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp) .NET クライアントおよびサーバーで使用されるライブラリでは、コードの生成を使用して、シリアル化を最適化します。 その結果、既定では"先読み-"コンパイル (Xamarin iOS、Unity など) を使用している環境でサポートされていません。 これらの環境で「事前コードを生成する」シリアライザー/デシリアライザーによって MessagePack を使用することになります。 詳細については、次を参照してください。 [MessagePack CSharp ドキュメント](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports)します。 シリアライザーを事前生成した後に渡される構成デリゲートを使用してそれらを登録できます`AddMessagePackProtocol`:

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.GeneratedResolver.Instance,
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

### <a name="type-checks-are-more-strict-in-messagepack"></a>型チェックは MessagePack でより厳密です

JSON のハブ プロトコルでは、型変換を逆シリアル化中に実行します。 たとえば、入力方向のオブジェクトがプロパティの値を持つ場合の数では (`{ foo: 42 }`) が型の .NET クラスのプロパティでは`string`値に変換されます。 ただし、MessagePack では、この変換は実行されませんし、サーバー側のログ (とコンソールで) 表示できる例外がスローされます。

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

この制限の詳細については、GitHub を参照してください。 問題[aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)します。

## <a name="related-resources"></a>関連資料

* [開始するには](xref:tutorials/signalr)
* [.NET クライアント](xref:signalr/dotnet-client)
* [JavaScript クライアント](xref:signalr/javascript-client)
