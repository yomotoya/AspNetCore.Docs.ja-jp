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
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400672"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="43055-103">ASP.NET core SignalR で MessagePack Hub プロトコルを使用します。</span><span class="sxs-lookup"><span data-stu-id="43055-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="43055-104">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="43055-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="43055-105">この記事では、リーダーがで説明するトピックを熟知前提としています。[開始](xref:tutorials/signalr)します。</span><span class="sxs-lookup"><span data-stu-id="43055-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="43055-106">MessagePack とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="43055-106">What is MessagePack?</span></span>

<span data-ttu-id="43055-107">[MessagePack](https://msgpack.org/index.html)は高速でコンパクトなバイナリ シリアル化形式です。</span><span class="sxs-lookup"><span data-stu-id="43055-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="43055-108">パフォーマンスと帯域幅が、問題になると比較して小さいメッセージを作成するため、場合に便利です[JSON](https://www.json.org/)します。</span><span class="sxs-lookup"><span data-stu-id="43055-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="43055-109">バイナリ形式であるため、メッセージは、読み取り可能なバイトが MessagePack パーサーから渡されていない限り、ネットワーク トレースとログを見ると。</span><span class="sxs-lookup"><span data-stu-id="43055-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="43055-110">SignalR は、MessagePack 形式の組み込みサポートを備え、使用するには、クライアントとサーバーの Api を提供します。</span><span class="sxs-lookup"><span data-stu-id="43055-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="43055-111">MessagePack をサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="43055-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="43055-112">サーバーの MessagePack Hub プロトコルを有効にするにはインストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージをアプリにします。</span><span class="sxs-lookup"><span data-stu-id="43055-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="43055-113">Startup.cs ファイルで追加`AddMessagePackProtocol`を`AddSignalR`呼び出し、サーバー上の MessagePack サポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="43055-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="43055-114">JSON は、既定で有効です。</span><span class="sxs-lookup"><span data-stu-id="43055-114">JSON is enabled by default.</span></span> <span data-ttu-id="43055-115">MessagePack を追加すると、JSON、MessagePack の両方のクライアントのサポートができます。</span><span class="sxs-lookup"><span data-stu-id="43055-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="43055-116">MessagePack が、データを書式設定する方法をカスタマイズする`AddMessagePackProtocol`オプションを構成するデリゲートを受け取る。</span><span class="sxs-lookup"><span data-stu-id="43055-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="43055-117">そのデリゲートで、 `FormatterResolvers` MessagePack シリアル化オプションを構成するプロパティを使用できます。</span><span class="sxs-lookup"><span data-stu-id="43055-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="43055-118">競合回避モジュールの動作方法の詳細については、MessagePack ライブラリを参照してください。 [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)します。</span><span class="sxs-lookup"><span data-stu-id="43055-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="43055-119">属性は、処理方法を定義するシリアル化するオブジェクトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="43055-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="43055-120">MessagePack client を構成します。</span><span class="sxs-lookup"><span data-stu-id="43055-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="43055-121">JSON は、サポートされているクライアントの既定で有効です。</span><span class="sxs-lookup"><span data-stu-id="43055-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="43055-122">クライアントは、1 つのプロトコルのみをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="43055-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="43055-123">構成されたプロトコルを MessagePack サポートは置き換える前に追加します。</span><span class="sxs-lookup"><span data-stu-id="43055-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="43055-124">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="43055-124">.NET client</span></span>

<span data-ttu-id="43055-125">MessagePack、.NET クライアントを有効にするにはインストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージと呼び出し`AddMessagePackProtocol`で`HubConnectionBuilder`します。</span><span class="sxs-lookup"><span data-stu-id="43055-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="43055-126">これは、`AddMessagePackProtocol`呼び出しは、サーバーと同様のオプションを構成するためのデリゲートを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="43055-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="43055-127">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="43055-127">JavaScript client</span></span>

<span data-ttu-id="43055-128">JavaScript クライアント MessagePack サポートが提供、 `@aspnet/signalr-protocol-msgpack` npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="43055-128">MessagePack support for the JavaScript client is provided by the `@aspnet/signalr-protocol-msgpack` npm package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="43055-129">Npm パッケージをインストールした後、モジュール、JavaScript モジュール ローダーを使用して直接使用したり参照することで、ブラウザーにインポート、 *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* ファイル。</span><span class="sxs-lookup"><span data-stu-id="43055-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="43055-130">ブラウザーで、`msgpack5`ライブラリが参照することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="43055-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="43055-131">使用して、`<script>`参照を作成するタグ。</span><span class="sxs-lookup"><span data-stu-id="43055-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="43055-132">ライブラリに掲載する*node_modules\msgpack5\dist\msgpack5.js*します。</span><span class="sxs-lookup"><span data-stu-id="43055-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="43055-133">使用する場合、`<script>`要素の順序は重要です。</span><span class="sxs-lookup"><span data-stu-id="43055-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="43055-134">場合*signalr-プロトコル-msgpack.js*する前に参照が*msgpack5.js*MessagePack で接続する際にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="43055-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="43055-135">*signalr.js*する必要がありますも*signalr-プロトコル-msgpack.js*します。</span><span class="sxs-lookup"><span data-stu-id="43055-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="43055-136">追加`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`を`HubConnectionBuilder`MessagePack プロトコルを使用して、サーバーに接続するときにクライアントを構成します。</span><span class="sxs-lookup"><span data-stu-id="43055-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="43055-137">この時点では、JavaScript クライアントでは、MessagePack プロトコルの構成オプションはありません。</span><span class="sxs-lookup"><span data-stu-id="43055-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="messagepack-quirks"></a><span data-ttu-id="43055-138">MessagePack quirks</span><span class="sxs-lookup"><span data-stu-id="43055-138">MessagePack quirks</span></span>

<span data-ttu-id="43055-139">MessagePack Hub プロトコルを使用する場合の注意すべきいくつかの問題があります。</span><span class="sxs-lookup"><span data-stu-id="43055-139">There are a few issues to be aware of when using the MessagePack Hub Protocol.</span></span>

### <a name="messagepack-is-case-sensitive"></a><span data-ttu-id="43055-140">MessagePack 小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="43055-140">MessagePack is case-sensitive</span></span>

<span data-ttu-id="43055-141">MessagePack プロトコルは、大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="43055-141">The MessagePack protocol is case-sensitive.</span></span> <span data-ttu-id="43055-142">たとえば、次C#クラス。</span><span class="sxs-lookup"><span data-stu-id="43055-142">For example, consider the following C# class:</span></span>

```csharp
public class ChatMessage
{
    public string Sender { get; }
    public string Message { get; }
}
```

<span data-ttu-id="43055-143">使用する必要があります、JavaScript クライアントからの送信時に`PascalCased`プロパティの名前、大文字と小文字が一致する必要がありますので、C#クラスを正確にします。</span><span class="sxs-lookup"><span data-stu-id="43055-143">When sending from the JavaScript client, you must use `PascalCased` property names, since the casing must match the C# class exactly.</span></span> <span data-ttu-id="43055-144">例:</span><span class="sxs-lookup"><span data-stu-id="43055-144">For example:</span></span>

```javascript
connection.invoke("SomeMethod", { Sender: "Sally", Message: "Hello!" });
```

<span data-ttu-id="43055-145">使用して`camelCased`名に正しくバインドされません、C#クラス。</span><span class="sxs-lookup"><span data-stu-id="43055-145">Using `camelCased` names won't properly bind to the C# class.</span></span> <span data-ttu-id="43055-146">使用して、これを回避することができますできます、 `Key` MessagePack プロパティに別の名前を指定する属性。</span><span class="sxs-lookup"><span data-stu-id="43055-146">You can work around this by using the `Key` attribute to specify a different name for the MessagePack property.</span></span> <span data-ttu-id="43055-147">詳細については、[MessagePack CSharp ドキュメント](https://github.com/neuecc/MessagePack-CSharp#object-serialization)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="43055-147">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#object-serialization).</span></span>

### <a name="datetimekind-is-not-preserved-when-serializingdeserializing"></a><span data-ttu-id="43055-148">シリアル化/逆シリアル化時 DateTime.Kind は保持されません。</span><span class="sxs-lookup"><span data-stu-id="43055-148">DateTime.Kind is not preserved when serializing/deserializing</span></span>

<span data-ttu-id="43055-149">MessagePack プロトコルは、エンコードする方法を提供しません、`Kind`の値を`DateTime`します。</span><span class="sxs-lookup"><span data-stu-id="43055-149">The MessagePack protocol doesn't provide a way to encode the `Kind` value of a `DateTime`.</span></span> <span data-ttu-id="43055-150">結果として、日付を逆シリアル化するときに受信日付が UTC 形式で MessagePack ハブのプロトコルが想定します。</span><span class="sxs-lookup"><span data-stu-id="43055-150">As a result, when deserializing a date, the MessagePack Hub Protocol assumes the incoming date is in UTC format.</span></span> <span data-ttu-id="43055-151">使用している場合`DateTime`現地時刻の値、お勧めしますに送信する前に、UTC に変換します。</span><span class="sxs-lookup"><span data-stu-id="43055-151">If you're working with `DateTime` values in local time, we recommend converting to UTC before sending them.</span></span> <span data-ttu-id="43055-152">乗り換え UTC から現地時刻に表示された場合。</span><span class="sxs-lookup"><span data-stu-id="43055-152">Convert them from UTC to local time when you receive them.</span></span>

<span data-ttu-id="43055-153">この制限の詳細については、GitHub を参照してください。 問題[aspnet/SignalR #2632](https://github.com/aspnet/SignalR/issues/2632)します。</span><span class="sxs-lookup"><span data-stu-id="43055-153">For more information on this limitation, see GitHub issue [aspnet/SignalR#2632](https://github.com/aspnet/SignalR/issues/2632).</span></span>

### <a name="datetimeminvalue-is-not-supported-by-messagepack-in-javascript"></a><span data-ttu-id="43055-154">JavaScript で MessagePack によって DateTime.MinValue はサポートされていません</span><span class="sxs-lookup"><span data-stu-id="43055-154">DateTime.MinValue is not supported by MessagePack in JavaScript</span></span>

<span data-ttu-id="43055-155">[Msgpack5](https://github.com/mcollina/msgpack5) SignalR JavaScript クライアントによって使用されるライブラリをサポートしていない、 `timestamp96` MessagePack 内の型。</span><span class="sxs-lookup"><span data-stu-id="43055-155">The [msgpack5](https://github.com/mcollina/msgpack5) library used by the SignalR JavaScript client doesn't support the `timestamp96` type in MessagePack.</span></span> <span data-ttu-id="43055-156">この型は、非常に大きな日付の値 (過去または将来的に非常にまでに非常に早い段階か) のエンコードに使用されます。</span><span class="sxs-lookup"><span data-stu-id="43055-156">This type is used to encode very large date values (either very early in the past or very far in the future).</span></span> <span data-ttu-id="43055-157">値`DateTime.MinValue`は`January 1, 0001`でエンコードする必要があります、`timestamp96`値。</span><span class="sxs-lookup"><span data-stu-id="43055-157">The value of `DateTime.MinValue` is `January 1, 0001` which must be encoded in a `timestamp96` value.</span></span> <span data-ttu-id="43055-158">このため、送信のため`DateTime.MinValue`を javascript クライアントはサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="43055-158">Because of this, sending `DateTime.MinValue` to a JavaScript client isn't supported.</span></span> <span data-ttu-id="43055-159">ときに`DateTime.MinValue`が受信される、JavaScript クライアントで、次のエラーがスローされます。</span><span class="sxs-lookup"><span data-stu-id="43055-159">When `DateTime.MinValue` is received by the JavaScript client, the following error is thrown:</span></span>

```
Uncaught Error: unable to find ext type 255 at decoder.js:427
```

<span data-ttu-id="43055-160">通常、`DateTime.MinValue`エンコードが使用される、「が見つかりません」または`null`値。</span><span class="sxs-lookup"><span data-stu-id="43055-160">Usually, `DateTime.MinValue` is used to encode a "missing" or `null` value.</span></span> <span data-ttu-id="43055-161">MessagePack でその値をエンコードする必要がある場合は、null 許容使用`DateTime`値 (`DateTime?`)、別のエンコードまたは`bool`日付が存在するかどうかを示す値。</span><span class="sxs-lookup"><span data-stu-id="43055-161">If you need to encode that value in MessagePack, use a nullable `DateTime` value (`DateTime?`) or encode a separate `bool` value indicating if the date is present.</span></span>

<span data-ttu-id="43055-162">この制限の詳細については、GitHub を参照してください。 問題[aspnet/SignalR #2228](https://github.com/aspnet/SignalR/issues/2228)します。</span><span class="sxs-lookup"><span data-stu-id="43055-162">For more information on this limitation, see GitHub issue [aspnet/SignalR#2228](https://github.com/aspnet/SignalR/issues/2228).</span></span>

### <a name="messagepack-support-in-ahead-of-time-compilation-environment"></a><span data-ttu-id="43055-163">"Ahead の time"コンパイル環境で MessagePack サポート</span><span class="sxs-lookup"><span data-stu-id="43055-163">MessagePack support in "ahead-of-time" compilation environment</span></span>

<span data-ttu-id="43055-164">[MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp) .NET クライアントおよびサーバーで使用されるライブラリでは、コードの生成を使用して、シリアル化を最適化します。</span><span class="sxs-lookup"><span data-stu-id="43055-164">The [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp) library used by the .NET client and server uses code generation to optimize serialization.</span></span> <span data-ttu-id="43055-165">その結果、既定では"先読み-"コンパイル (Xamarin iOS、Unity など) を使用している環境でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="43055-165">As a result, it isn't supported by default on environments that use "ahead-of-time" compilation (such as Xamarin iOS or Unity).</span></span> <span data-ttu-id="43055-166">これらの環境で「事前コードを生成する」シリアライザー/デシリアライザーによって MessagePack を使用することになります。</span><span class="sxs-lookup"><span data-stu-id="43055-166">It's possible to use MessagePack in these environments by "pre-generating" the serializer/deserializer code.</span></span> <span data-ttu-id="43055-167">詳細については、[MessagePack CSharp ドキュメント](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="43055-167">For more information, see [the MessagePack-CSharp documentation](https://github.com/neuecc/MessagePack-CSharp#pre-code-generationunityxamarin-supports).</span></span> <span data-ttu-id="43055-168">シリアライザーを事前生成した後に渡される構成デリゲートを使用してそれらを登録できます`AddMessagePackProtocol`:</span><span class="sxs-lookup"><span data-stu-id="43055-168">Once you have pre-generated the serializers, you can register them using the configuration delegate passed to `AddMessagePackProtocol`:</span></span>

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

### <a name="type-checks-are-more-strict-in-messagepack"></a><span data-ttu-id="43055-169">型チェックは MessagePack でより厳密です</span><span class="sxs-lookup"><span data-stu-id="43055-169">Type checks are more strict in MessagePack</span></span>

<span data-ttu-id="43055-170">JSON のハブ プロトコルでは、型変換を逆シリアル化中に実行します。</span><span class="sxs-lookup"><span data-stu-id="43055-170">The JSON Hub Protocol will perform type conversions during deserialization.</span></span> <span data-ttu-id="43055-171">たとえば、入力方向のオブジェクトがプロパティの値を持つ場合の数では (`{ foo: 42 }`) が型の .NET クラスのプロパティでは`string`値に変換されます。</span><span class="sxs-lookup"><span data-stu-id="43055-171">For example, if the incoming object has a property value that is a number (`{ foo: 42 }`) but the property on the .NET class is of type `string`, the value will be converted.</span></span> <span data-ttu-id="43055-172">ただし、MessagePack では、この変換は実行されませんし、サーバー側のログ (とコンソールで) 表示できる例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="43055-172">However, MessagePack doesn't perform this conversion and will throw an exception that can be seen in server-side logs (and in the console):</span></span>

```
InvalidDataException: Error binding arguments. Make sure that the types of the provided values match the types of the hub method being invoked.
```

<span data-ttu-id="43055-173">この制限の詳細については、GitHub を参照してください。 問題[aspnet/SignalR #2937](https://github.com/aspnet/SignalR/issues/2937)します。</span><span class="sxs-lookup"><span data-stu-id="43055-173">For more information on this limitation, see GitHub issue [aspnet/SignalR#2937](https://github.com/aspnet/SignalR/issues/2937).</span></span>

## <a name="related-resources"></a><span data-ttu-id="43055-174">関連資料</span><span class="sxs-lookup"><span data-stu-id="43055-174">Related resources</span></span>

* [<span data-ttu-id="43055-175">開始するには</span><span class="sxs-lookup"><span data-stu-id="43055-175">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="43055-176">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="43055-176">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="43055-177">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="43055-177">JavaScript client</span></span>](xref:signalr/javascript-client)
