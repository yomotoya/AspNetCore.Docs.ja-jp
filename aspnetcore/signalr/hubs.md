---
title: ASP.NET Core SignalR のハブを使用します。
author: tdykstra
description: ASP.NET Core SignalR のハブを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: e583676ab0eed45aeaf6391d8cdf8c1485aa914e
ms.sourcegitcommit: e7e1e531b80b3f4117ff119caadbebf4dcf5dcb7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2018
ms.locfileid: "44510338"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="b60a9-103">ASP.NET core SignalR のハブの使用</span><span class="sxs-lookup"><span data-stu-id="b60a9-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="b60a9-104">によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="b60a9-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="b60a9-105">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="b60a9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="b60a9-106">SignalR ハブとは</span><span class="sxs-lookup"><span data-stu-id="b60a9-106">What is a SignalR hub</span></span>

<span data-ttu-id="b60a9-107">SignalR ハブの API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="b60a9-108">サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="b60a9-109">クライアント コードでは、サーバーから呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="b60a9-110">SignalR は、バック グラウンドでクライアントとサーバーとサーバーからクライアントのリアルタイム通信を可能にするすべての処理されます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="b60a9-111">SignalR ハブを構成します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-111">Configure SignalR hubs</span></span>

<span data-ttu-id="b60a9-112">SignalR のミドルウェアがサービスによっては、呼び出すことによって構成されている必要があります`services.AddSignalR`します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="b60a9-113">SignalR の機能を ASP.NET Core アプリを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b60a9-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="b60a9-114">作成し、ハブの使用</span><span class="sxs-lookup"><span data-stu-id="b60a9-114">Create and use hubs</span></span>

<span data-ttu-id="b60a9-115">継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="b60a9-116">クライアントとして定義されているメソッドを呼び出すことができます`public`します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="b60a9-117">戻り値の型と複合型と配列を含む c# メソッドの場合と、パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="b60a9-118">SignalR では、複雑なオブジェクトと、パラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="b60a9-119">コンテキスト オブジェクト</span><span class="sxs-lookup"><span data-stu-id="b60a9-119">The Context object</span></span>

<span data-ttu-id="b60a9-120">`Hub`クラスには、`Context`接続に関する情報を次のプロパティを含むプロパティ。</span><span class="sxs-lookup"><span data-stu-id="b60a9-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="b60a9-121">プロパティ</span><span class="sxs-lookup"><span data-stu-id="b60a9-121">Property</span></span> | <span data-ttu-id="b60a9-122">説明</span><span class="sxs-lookup"><span data-stu-id="b60a9-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="b60a9-123">SignalR によって割り当てられている接続の一意の ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="b60a9-124">接続ごとに 1 つの接続 ID があります。</span><span class="sxs-lookup"><span data-stu-id="b60a9-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="b60a9-125">取得、[ユーザー識別子](xref:signalr/groups)します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="b60a9-126">既定では、SignalR を使用して、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`に関連付けられたユーザー識別子として接続します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="b60a9-127">取得、`ClaimsPrincipal`現在のユーザーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="b60a9-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="b60a9-128">この接続のスコープ内のデータを共有するために使用できるキー/値コレクションを取得します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="b60a9-129">このコレクションに格納できるデータおよび接続の別のハブ メソッド呼び出し間で保持されます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="b60a9-130">接続で使用できる機能のコレクションを取得します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="b60a9-131">ここでは、このコレクションは必要ありません、ほとんどのシナリオでまだ詳しく記載されていないためです。</span><span class="sxs-lookup"><span data-stu-id="b60a9-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="b60a9-132">取得、`CancellationToken`接続が中止されたときに通知します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="b60a9-133">`Hub.Context` また、次のメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="b60a9-134">メソッド</span><span class="sxs-lookup"><span data-stu-id="b60a9-134">Method</span></span> | <span data-ttu-id="b60a9-135">説明</span><span class="sxs-lookup"><span data-stu-id="b60a9-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="b60a9-136">返します、`HttpContext`接続、または`null`接続が HTTP 要求に関連付けられていない場合。</span><span class="sxs-lookup"><span data-stu-id="b60a9-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="b60a9-137">HTTP 接続で HTTP ヘッダーやクエリ文字列などの情報を取得するのにこのメソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="b60a9-138">接続を中止します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="b60a9-139">クライアント オブジェクト</span><span class="sxs-lookup"><span data-stu-id="b60a9-139">The Clients object</span></span>

<span data-ttu-id="b60a9-140">`Hub`クラスには、`Clients`サーバーとクライアント間の通信は、次のプロパティを含むプロパティ。</span><span class="sxs-lookup"><span data-stu-id="b60a9-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="b60a9-141">プロパティ</span><span class="sxs-lookup"><span data-stu-id="b60a9-141">Property</span></span> | <span data-ttu-id="b60a9-142">説明</span><span class="sxs-lookup"><span data-stu-id="b60a9-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="b60a9-143">接続されているすべてのクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="b60a9-144">ハブ メソッドを呼び出したクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="b60a9-145">メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="b60a9-146">`Hub.Clients` また、次のメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="b60a9-147">メソッド</span><span class="sxs-lookup"><span data-stu-id="b60a9-147">Method</span></span> | <span data-ttu-id="b60a9-148">説明</span><span class="sxs-lookup"><span data-stu-id="b60a9-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="b60a9-149">指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="b60a9-150">特定の接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="b60a9-151">接続されているクライアントが特定のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="b60a9-152">指定したグループのすべての接続にメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="b60a9-153">指定された接続を除く、指定されたグループのすべての接続にメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="b60a9-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="b60a9-154">接続の複数のグループにメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="b60a9-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="b60a9-155">ハブ メソッドを呼び出したクライアントを除く、接続のグループにメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="b60a9-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="b60a9-156">特定のユーザーに関連付けられているすべての接続にメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="b60a9-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="b60a9-157">指定されたユーザーに関連付けられているすべての接続にメソッドを呼び出します</span><span class="sxs-lookup"><span data-stu-id="b60a9-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="b60a9-158">プロパティまたはメソッドでは、上記のテーブルごとにオブジェクトを返します、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b60a9-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="b60a9-159">`SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="b60a9-160">クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-160">Send messages to clients</span></span>

<span data-ttu-id="b60a9-161">特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b60a9-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="b60a9-162">次の例では、`SendMessageToCaller`ハブ メソッドを呼び出した接続にメッセージを送信するメソッドに示します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="b60a9-163">`SendMessageToGroups`メソッドに格納されているグループにメッセージを送信する、`List`という`groups`します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="b60a9-164">接続のイベントの処理</span><span class="sxs-lookup"><span data-stu-id="b60a9-164">Handle events for a connection</span></span>

<span data-ttu-id="b60a9-165">SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`接続を管理するための仮想メソッド。</span><span class="sxs-lookup"><span data-stu-id="b60a9-165">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="b60a9-166">上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッド。</span><span class="sxs-lookup"><span data-stu-id="b60a9-166">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="b60a9-167">エラー処理</span><span class="sxs-lookup"><span data-stu-id="b60a9-167">Handle errors</span></span>

<span data-ttu-id="b60a9-168">ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="b60a9-168">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="b60a9-169">JavaScript クライアントで、`invoke`メソッドを返します。 を[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)します。</span><span class="sxs-lookup"><span data-stu-id="b60a9-169">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="b60a9-170">クライアントが、ハンドラーでエラーを受信すると promise を使用して、接続されている`catch`、呼び出されるは、JavaScript として渡される`Error`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b60a9-170">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="b60a9-171">関連資料</span><span class="sxs-lookup"><span data-stu-id="b60a9-171">Related resources</span></span>

* [<span data-ttu-id="b60a9-172">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="b60a9-172">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="b60a9-173">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="b60a9-173">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b60a9-174">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="b60a9-174">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
