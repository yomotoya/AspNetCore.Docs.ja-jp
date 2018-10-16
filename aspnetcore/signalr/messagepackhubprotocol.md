---
title: ASP.NET core SignalR で MessagePack Hub プロトコルを使用します。
author: tdykstra
description: ASP.NET Core SignalR には、MessagePack Hub プロトコルを追加します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/messagepackhubprotocol
ms.openlocfilehash: 0874afc5493eca5d43dfde30bb28aedc1f193744
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325582"
---
# <a name="use-messagepack-hub-protocol-in-signalr-for-aspnet-core"></a><span data-ttu-id="ad3bd-103">ASP.NET core SignalR で MessagePack Hub プロトコルを使用します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-103">Use MessagePack Hub Protocol in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="ad3bd-104">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="ad3bd-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="ad3bd-105">この記事では、リーダーがで説明するトピックを熟知前提としています。[開始](xref:tutorials/signalr)します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-105">This article assumes the reader is familiar with the topics covered in [Get Started](xref:tutorials/signalr).</span></span>

## <a name="what-is-messagepack"></a><span data-ttu-id="ad3bd-106">MessagePack とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-106">What is MessagePack?</span></span>

<span data-ttu-id="ad3bd-107">[MessagePack](https://msgpack.org/index.html)は高速でコンパクトなバイナリ シリアル化形式です。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-107">[MessagePack](https://msgpack.org/index.html) is a binary serialization format that is fast and compact.</span></span> <span data-ttu-id="ad3bd-108">パフォーマンスと帯域幅が、問題になると比較して小さいメッセージを作成するため、場合に便利です[JSON](https://www.json.org/)します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-108">It's useful when performance and bandwidth are a concern because it creates smaller messages compared to [JSON](https://www.json.org/).</span></span> <span data-ttu-id="ad3bd-109">バイナリ形式であるため、メッセージは、読み取り可能なバイトが MessagePack パーサーから渡されていない限り、ネットワーク トレースとログを見ると。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-109">Because it's a binary format, messages are unreadable when looking at network traces and logs unless the bytes are passed through a MessagePack parser.</span></span> <span data-ttu-id="ad3bd-110">SignalR は、MessagePack 形式の組み込みサポートを備え、使用するには、クライアントとサーバーの Api を提供します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-110">SignalR has built-in support for the MessagePack format, and provides APIs for the client and server to use.</span></span>

## <a name="configure-messagepack-on-the-server"></a><span data-ttu-id="ad3bd-111">MessagePack をサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-111">Configure MessagePack on the server</span></span>

<span data-ttu-id="ad3bd-112">サーバーの MessagePack Hub プロトコルを有効にするにはインストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージをアプリにします。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-112">To enable the MessagePack Hub Protocol on the server, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package in your app.</span></span> <span data-ttu-id="ad3bd-113">Startup.cs ファイルで追加`AddMessagePackProtocol`を`AddSignalR`呼び出し、サーバー上の MessagePack サポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-113">In the Startup.cs file add `AddMessagePackProtocol` to the `AddSignalR` call to enable MessagePack support on the server.</span></span>

> [!NOTE]
> <span data-ttu-id="ad3bd-114">JSON は、既定で有効です。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-114">JSON is enabled by default.</span></span> <span data-ttu-id="ad3bd-115">MessagePack を追加すると、JSON、MessagePack の両方のクライアントのサポートができます。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-115">Adding MessagePack enables support for both JSON and MessagePack clients.</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol();
```

<span data-ttu-id="ad3bd-116">MessagePack が、データを書式設定する方法をカスタマイズする`AddMessagePackProtocol`オプションを構成するデリゲートを受け取る。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-116">To customize how MessagePack will format your data, `AddMessagePackProtocol` takes a delegate for configuring options.</span></span> <span data-ttu-id="ad3bd-117">そのデリゲートで、 `FormatterResolvers` MessagePack シリアル化オプションを構成するプロパティを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-117">In that delegate, the `FormatterResolvers` property can be used to configure MessagePack serialization options.</span></span> <span data-ttu-id="ad3bd-118">競合回避モジュールの動作方法の詳細については、MessagePack ライブラリを参照してください。 [MessagePack CSharp](https://github.com/neuecc/MessagePack-CSharp)します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-118">For more information on how the resolvers work, visit the MessagePack library at [MessagePack-CSharp](https://github.com/neuecc/MessagePack-CSharp).</span></span> <span data-ttu-id="ad3bd-119">属性は、処理方法を定義するシリアル化するオブジェクトで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-119">Attributes can be used on the objects you want to serialize to define how they should be handled.</span></span>

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

## <a name="configure-messagepack-on-the-client"></a><span data-ttu-id="ad3bd-120">MessagePack client を構成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-120">Configure MessagePack on the client</span></span>

> [!NOTE]
> <span data-ttu-id="ad3bd-121">JSON は、サポートされているクライアントの既定で有効です。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-121">JSON is enabled by default for the supported clients.</span></span> <span data-ttu-id="ad3bd-122">クライアントは、1 つのプロトコルのみをサポートできます。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-122">Clients can only support a single protocol.</span></span> <span data-ttu-id="ad3bd-123">構成されたプロトコルを MessagePack サポートは置き換える前に追加します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-123">Adding MessagePack support will replace any previously configured protocols.</span></span>

### <a name="net-client"></a><span data-ttu-id="ad3bd-124">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="ad3bd-124">.NET client</span></span>

<span data-ttu-id="ad3bd-125">MessagePack、.NET クライアントを有効にするにはインストール、`Microsoft.AspNetCore.SignalR.Protocols.MessagePack`パッケージと呼び出し`AddMessagePackProtocol`で`HubConnectionBuilder`します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-125">To enable MessagePack in the .NET Client, install the `Microsoft.AspNetCore.SignalR.Protocols.MessagePack` package and call `AddMessagePackProtocol` on `HubConnectionBuilder`.</span></span>

```csharp
var hubConnection = new HubConnectionBuilder()
                        .WithUrl("/chatHub")
                        .AddMessagePackProtocol()
                        .Build();
```

> [!NOTE]
> <span data-ttu-id="ad3bd-126">これは、`AddMessagePackProtocol`呼び出しは、サーバーと同様のオプションを構成するためのデリゲートを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-126">This `AddMessagePackProtocol` call takes a delegate for configuring options just like the server.</span></span>

### <a name="javascript-client"></a><span data-ttu-id="ad3bd-127">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="ad3bd-127">JavaScript client</span></span>

<span data-ttu-id="ad3bd-128">Javascript クライアント MessagePack サポートが提供、 `@aspnet/signalr-protocol-msgpack` NPM パッケージ。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-128">MessagePack support for the Javascript client is provided by the `@aspnet/signalr-protocol-msgpack` NPM package.</span></span>

```console
npm install @aspnet/signalr-protocol-msgpack
```

<span data-ttu-id="ad3bd-129">Npm パッケージをインストールした後、モジュール、JavaScript モジュール ローダーを使用して直接使用したり参照することで、ブラウザーにインポート、 *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* ファイル。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-129">After installing the npm package, the module can be used directly via a JavaScript module loader or imported into the browser by referencing the *node_modules\\@aspnet\signalr-protocol-msgpack\dist\browser\signalr-protocol-msgpack.js* file.</span></span> <span data-ttu-id="ad3bd-130">ブラウザーで、`msgpack5`ライブラリが参照することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-130">In a browser, the `msgpack5` library must also be referenced.</span></span> <span data-ttu-id="ad3bd-131">使用して、`<script>`参照を作成するタグ。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-131">Use a `<script>` tag to create a reference.</span></span> <span data-ttu-id="ad3bd-132">ライブラリに掲載する*node_modules\msgpack5\dist\msgpack5.js*します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-132">The library can be found at *node_modules\msgpack5\dist\msgpack5.js*.</span></span>

> [!NOTE]
> <span data-ttu-id="ad3bd-133">使用する場合、`<script>`要素の順序は重要です。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-133">When using the `<script>` element, the order is important.</span></span> <span data-ttu-id="ad3bd-134">場合*signalr-プロトコル-msgpack.js*する前に参照が*msgpack5.js*MessagePack で接続する際にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-134">If *signalr-protocol-msgpack.js* is referenced before *msgpack5.js*, an error occurs when trying to connect with MessagePack.</span></span> <span data-ttu-id="ad3bd-135">*signalr.js*する必要がありますも*signalr-プロトコル-msgpack.js*します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-135">*signalr.js* is also required before *signalr-protocol-msgpack.js*.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="ad3bd-136">追加`.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())`を`HubConnectionBuilder`MessagePack プロトコルを使用して、サーバーに接続するときにクライアントを構成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-136">Adding `.withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())` to the `HubConnectionBuilder` will configure the client to use the MessagePack protocol when connecting to a server.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withHubProtocol(new signalR.protocols.msgpack.MessagePackHubProtocol())
    .build();
```

> [!NOTE]
> <span data-ttu-id="ad3bd-137">この時点では、JavaScript クライアントでは、MessagePack プロトコルの構成オプションはありません。</span><span class="sxs-lookup"><span data-stu-id="ad3bd-137">At this time, there are no configuration options for the MessagePack protocol on the JavaScript client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="ad3bd-138">関連資料</span><span class="sxs-lookup"><span data-stu-id="ad3bd-138">Related resources</span></span>

* [<span data-ttu-id="ad3bd-139">開始するには</span><span class="sxs-lookup"><span data-stu-id="ad3bd-139">Get Started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ad3bd-140">.NET クライアント</span><span class="sxs-lookup"><span data-stu-id="ad3bd-140">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ad3bd-141">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="ad3bd-141">JavaScript client</span></span>](xref:signalr/javascript-client)
