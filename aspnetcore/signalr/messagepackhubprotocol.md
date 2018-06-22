---
title: ASP.NET Core 用 SignalR で MessagePack ハブ プロトコルを使用します。
author: rachelappel
description: ASP.NET Core SignalR を MessagePack ハブ プロトコルを追加します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 702c77502868d6666cb2634b6959f029e036d14e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274990"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="2bb8f-103">ASP.NET Core 用 SignalR で MessagePack ハブ プロトコルを使用します。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="2bb8f-104">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="2bb8f-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="2bb8f-105">この記事で説明されているトピックを使い慣れているリーダーが前提としています[開始](xref:tutorials/signalr)です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="2bb8f-106">MessagePack とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-106">What is MessagePack?</span></span>

<span data-ttu-id="2bb8f-107">[MessagePack](https://msgpack.org/index.html)高速でコンパクトなバイナリのシリアル化形式です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="2bb8f-108">パフォーマンスと帯域幅が、問題になると比較すると、小さいメッセージを作成するため便利です[JSON](https://www.json.org/)です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="2bb8f-109">バイナリ形式になっているためメッセージは読み取り可能バイト MessagePack パーサー経由で渡される限り、ネットワーク トレースとログを確認するとき</span><span class="sxs-lookup"><span data-stu-id="2bb8f-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="2bb8f-110">SignalR は MessagePack 形式の組み込みサポートし、使用するには、クライアントとサーバーの Api を提供します。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="2bb8f-111">サーバーで MessagePack を構成します。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="2bb8f-112">MessagePack ハブ サーバーでプロトコルを有効にするには、インストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`アプリのパッケージです。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="2bb8f-113">Startup.cs ファイルで追加`AddMessagePackProtocol`を`AddSignalR`呼び出し、サーバー上の MessagePack サポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="2bb8f-114">JSON は既定で有効にします。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-114">JSON is enabled by default.</span></span> <span data-ttu-id="2bb8f-115">MessagePack を追加すると、JSON と MessagePack の両方のクライアントのサポートができます。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="2bb8f-116">MessagePack が、データを書式設定方法をカスタマイズする`AddMessagePackProtocol`オプションを構成するデリゲートを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="2bb8f-117">そのデリゲート内、 `FormatterResolvers` MessagePack シリアル化オプションを構成するプロパティを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="2bb8f-118">競合回避モジュールの動作方法の詳細については、MessagePack ライブラリを参照してください。 [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="2bb8f-119">属性は、処理方法を定義するシリアル化オブジェクトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="2bb8f-120">クライアントで MessagePack を構成します。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-120">Configure MessagePack on the client</span></span>

### <a name="net-client"></a><span data-ttu-id="2bb8f-121">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="2bb8f-121">.NET client</span></span>

<span data-ttu-id="2bb8f-122">.NET クライアントで MessagePack を有効にするには、インストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージと呼び出し`AddMessagePackProtocol`で`HubConnectionBuilder`です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-122">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="2bb8f-123">これは、`AddMessagePackProtocol`呼び出しが、サーバーと同じようにオプションを構成するデリゲートを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-123">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="2bb8f-124">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="2bb8f-124">JavaScript client</span></span>

<span data-ttu-id="2bb8f-125">Javascript クライアント MessagePack サポートによって提供されます、 `@aspnet/signalr-protocol-msgpack` NPM パッケージです。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-125">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="2bb8f-126">Npm パッケージをインストールすると、モジュール、JavaScript のモジュール ローダー経由で直接使用インポートしたりできますが、ブラウザーを参照して、 *node_modules\\ @aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* ファイル。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-126">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="2bb8f-127">ブラウザーで、`msgpack5`ライブラリが参照することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-127">In a browser the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="2bb8f-128">使用して、`<script>`タグへの参照を作成します。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-128">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="2bb8f-129">ライブラリはあります*node_modules\msgpack5\dist\msgpack5.js*です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-129">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="2bb8f-130">使用する場合、`<script>`要素、順序は重要です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-130">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="2bb8f-131">場合*signalr プロトコル-msgpack.js*する前に参照される*msgpack5.js*MessagePack で接続しようとするときにエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-131">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="2bb8f-132">*signalr.js*も必要とする前に*signalr プロトコル-msgpack.js*です。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-132">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="2bb8f-133">追加`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`を`HubConnectionBuilder`はサーバーに接続するときに、MessagePack プロトコルを使用するクライアントを構成します。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-133">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="2bb8f-134">この時点では、JavaScript クライアントで MessagePack プロトコルの構成オプションはありません。</span><span class="sxs-lookup"><span data-stu-id="2bb8f-134">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="2bb8f-135">関連資料</span><span class="sxs-lookup"><span data-stu-id="2bb8f-135">Related resources</span></span>

* [<span data-ttu-id="2bb8f-136">開始するには</span><span class="sxs-lookup"><span data-stu-id="2bb8f-136">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="2bb8f-137">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="2bb8f-137">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="2bb8f-138">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="2bb8f-138">JavaScript client</span></span>](xref:signalr/javascript-client)
