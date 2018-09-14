---
title: ASP.NET Core SignalR のハブを使用します。
author: tdykstra
description: ASP.NET Core SignalR のハブを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 17e3ee23967bc1097a3121b3e3e5b58cebe3887d
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538363"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="87abc-103">ASP.NET core SignalR のハブの使用</span><span class="sxs-lookup"><span data-stu-id="87abc-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="87abc-104">によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="87abc-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="87abc-105">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="87abc-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="87abc-106">SignalR ハブとは</span><span class="sxs-lookup"><span data-stu-id="87abc-106">What is a SignalR hub</span></span>

<span data-ttu-id="87abc-107">SignalR ハブの API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="87abc-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="87abc-108">サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="87abc-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="87abc-109">クライアント コードでは、サーバーから呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="87abc-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="87abc-110">SignalR は、バック グラウンドでクライアントとサーバーとサーバーからクライアントのリアルタイム通信を可能にするすべての処理されます。</span><span class="sxs-lookup"><span data-stu-id="87abc-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="87abc-111">SignalR ハブを構成します。</span><span class="sxs-lookup"><span data-stu-id="87abc-111">Configure SignalR hubs</span></span>

<span data-ttu-id="87abc-112">SignalR のミドルウェアがサービスによっては、呼び出すことによって構成されている必要があります`services.AddSignalR`します。</span><span class="sxs-lookup"><span data-stu-id="87abc-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="87abc-113">SignalR の機能を ASP.NET Core アプリを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87abc-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="87abc-114">作成し、ハブの使用</span><span class="sxs-lookup"><span data-stu-id="87abc-114">Create and use hubs</span></span>

<span data-ttu-id="87abc-115">継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="87abc-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="87abc-116">クライアントとして定義されているメソッドを呼び出すことができます`public`します。</span><span class="sxs-lookup"><span data-stu-id="87abc-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="87abc-117">戻り値の型と複合型と配列を含む c# メソッドの場合と、パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="87abc-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="87abc-118">SignalR では、複雑なオブジェクトと、パラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。</span><span class="sxs-lookup"><span data-stu-id="87abc-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="87abc-119">コンテキスト オブジェクト</span><span class="sxs-lookup"><span data-stu-id="87abc-119">The Context object</span></span>

<span data-ttu-id="87abc-120">`Hub`クラスには、`Context`接続に関する情報を次のプロパティを含むプロパティ。</span><span class="sxs-lookup"><span data-stu-id="87abc-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="87abc-121">プロパティ</span><span class="sxs-lookup"><span data-stu-id="87abc-121">Property</span></span> | <span data-ttu-id="87abc-122">説明</span><span class="sxs-lookup"><span data-stu-id="87abc-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="87abc-123">SignalR によって割り当てられている接続の一意の ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="87abc-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="87abc-124">接続ごとに 1 つの接続 ID があります。</span><span class="sxs-lookup"><span data-stu-id="87abc-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="87abc-125">取得、[ユーザー識別子](xref:signalr/groups)します。</span><span class="sxs-lookup"><span data-stu-id="87abc-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="87abc-126">既定では、SignalR を使用して、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`に関連付けられたユーザー識別子として接続します。</span><span class="sxs-lookup"><span data-stu-id="87abc-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="87abc-127">取得、`ClaimsPrincipal`現在のユーザーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="87abc-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="87abc-128">この接続のスコープ内のデータを共有するために使用できるキー/値コレクションを取得します。</span><span class="sxs-lookup"><span data-stu-id="87abc-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="87abc-129">このコレクションに格納できるデータおよび接続の別のハブ メソッド呼び出し間で保持されます。</span><span class="sxs-lookup"><span data-stu-id="87abc-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="87abc-130">接続で使用できる機能のコレクションを取得します。</span><span class="sxs-lookup"><span data-stu-id="87abc-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="87abc-131">ここでは、このコレクションは必要ありません、ほとんどのシナリオでまだ詳しく記載されていないためです。</span><span class="sxs-lookup"><span data-stu-id="87abc-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="87abc-132">取得、`CancellationToken`接続が中止されたときに通知します。</span><span class="sxs-lookup"><span data-stu-id="87abc-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="87abc-133">`Hub.Context` また、次のメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="87abc-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="87abc-134">メソッド</span><span class="sxs-lookup"><span data-stu-id="87abc-134">Method</span></span> | <span data-ttu-id="87abc-135">説明</span><span class="sxs-lookup"><span data-stu-id="87abc-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="87abc-136">返します、`HttpContext`接続、または`null`接続が HTTP 要求に関連付けられていない場合。</span><span class="sxs-lookup"><span data-stu-id="87abc-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="87abc-137">HTTP 接続で HTTP ヘッダーやクエリ文字列などの情報を取得するのにこのメソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="87abc-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="87abc-138">接続を中止します。</span><span class="sxs-lookup"><span data-stu-id="87abc-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="87abc-139">クライアント オブジェクト</span><span class="sxs-lookup"><span data-stu-id="87abc-139">The Clients object</span></span>

<span data-ttu-id="87abc-140">`Hub`クラスには、`Clients`サーバーとクライアント間の通信は、次のプロパティを含むプロパティ。</span><span class="sxs-lookup"><span data-stu-id="87abc-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="87abc-141">プロパティ</span><span class="sxs-lookup"><span data-stu-id="87abc-141">Property</span></span> | <span data-ttu-id="87abc-142">説明</span><span class="sxs-lookup"><span data-stu-id="87abc-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="87abc-143">接続されているすべてのクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="87abc-144">ハブ メソッドを呼び出したクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="87abc-145">メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="87abc-146">`Hub.Clients` また、次のメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="87abc-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="87abc-147">メソッド</span><span class="sxs-lookup"><span data-stu-id="87abc-147">Method</span></span> | <span data-ttu-id="87abc-148">説明</span><span class="sxs-lookup"><span data-stu-id="87abc-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="87abc-149">指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="87abc-150">特定の接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="87abc-151">接続されているクライアントが特定のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="87abc-152">指定したグループのすべての接続にメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="87abc-153">指定された接続を除く、指定されたグループのすべての接続にメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="87abc-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="87abc-154">接続の複数のグループにメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="87abc-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="87abc-155">ハブ メソッドを呼び出したクライアントを除く、接続のグループにメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="87abc-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="87abc-156">特定のユーザーに関連付けられているすべての接続にメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="87abc-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="87abc-157">指定されたユーザーに関連付けられているすべての接続にメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="87abc-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="87abc-158">プロパティまたはメソッドでは、上記のテーブルごとにオブジェクトを返します、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87abc-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="87abc-159">`SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="87abc-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="87abc-160">クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="87abc-160">Send messages to clients</span></span>

<span data-ttu-id="87abc-161">特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="87abc-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="87abc-162">次の例では、`SendMessageToCaller`ハブ メソッドを呼び出した接続にメッセージを送信するメソッドに示します。</span><span class="sxs-lookup"><span data-stu-id="87abc-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="87abc-163">`SendMessageToGroups`メソッドに格納されているグループにメッセージを送信する、`List`という`groups`します。</span><span class="sxs-lookup"><span data-stu-id="87abc-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="87abc-164">厳密に型指定されたハブ</span><span class="sxs-lookup"><span data-stu-id="87abc-164">Strongly typed hubs</span></span>

<span data-ttu-id="87abc-165">使用する欠点`SendAsync`が呼び出されるクライアント メソッドを指定する文字列に依存していること。</span><span class="sxs-lookup"><span data-stu-id="87abc-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="87abc-166">これにより、メソッド名のスペルが正しい場合、ランタイム エラー コードを開くまたはクライアントから不足しています。</span><span class="sxs-lookup"><span data-stu-id="87abc-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="87abc-167">使用する代わりに`SendAsync`を厳密に型が、`Hub`で<xref:Microsoft.AspNetCore.SignalR.Hub`1>します。</span><span class="sxs-lookup"><span data-stu-id="87abc-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="87abc-168">次の例では、`ChatHub`クライアント メソッドがアウトというインターフェイスに抽出された`IChatClient`します。</span><span class="sxs-lookup"><span data-stu-id="87abc-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="87abc-169">リファクタリングの前に、このインターフェイスを使用する`ChatHub`例。</span><span class="sxs-lookup"><span data-stu-id="87abc-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="87abc-170">使用して`Hub<IChatClient>`クライアント メソッドのコンパイル時にチェックできるようにします。</span><span class="sxs-lookup"><span data-stu-id="87abc-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="87abc-171">これにより、問題のため、マジック文字列を使用することで`Hub<T>`のみ、インターフェイスで定義されたメソッドへのアクセスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="87abc-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="87abc-172">厳密に型指定を使用して`Hub<T>`を使用する機能を無効にします。`SendAsync`します。</span><span class="sxs-lookup"><span data-stu-id="87abc-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="87abc-173">接続のイベントの処理</span><span class="sxs-lookup"><span data-stu-id="87abc-173">Handle events for a connection</span></span>

<span data-ttu-id="87abc-174">SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`接続を管理するための仮想メソッド。</span><span class="sxs-lookup"><span data-stu-id="87abc-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="87abc-175">上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッド。</span><span class="sxs-lookup"><span data-stu-id="87abc-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="87abc-176">エラー処理</span><span class="sxs-lookup"><span data-stu-id="87abc-176">Handle errors</span></span>

<span data-ttu-id="87abc-177">ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="87abc-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="87abc-178">JavaScript クライアントで、`invoke`メソッドを返します。 を[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)します。</span><span class="sxs-lookup"><span data-stu-id="87abc-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="87abc-179">クライアントが、ハンドラーでエラーを受信すると promise を使用して、接続されている`catch`、呼び出されるは、JavaScript として渡される`Error`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="87abc-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="87abc-180">関連資料</span><span class="sxs-lookup"><span data-stu-id="87abc-180">Related resources</span></span>

* [<span data-ttu-id="87abc-181">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="87abc-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="87abc-182">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="87abc-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="87abc-183">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="87abc-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
