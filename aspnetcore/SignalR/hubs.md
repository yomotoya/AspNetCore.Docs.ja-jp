---
title: ASP.NET Core SignalR ハブを使用します。
author: rachelappel
description: ASP.NET Core SignalR ハブを使用する方法を説明します。
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 73397ba6951f2752653575920bdf739439874366
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="4047e-103">ASP.NET Core の SignalR でハブを使用します。</span><span class="sxs-lookup"><span data-stu-id="4047e-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="4047e-104">によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="4047e-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="4047e-105">SignalR ハブとは</span><span class="sxs-lookup"><span data-stu-id="4047e-105">What is a SignalR hub</span></span>

<span data-ttu-id="4047e-106">SignalR ハブ API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="4047e-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="4047e-107">サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="4047e-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="4047e-108">クライアント コードでは、サーバーから呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="4047e-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="4047e-109">バック グラウンドでリアルタイムのクライアントとサーバーおよびサーバーからクライアントへの通信を可能にするすべての SignalR によって行われます。</span><span class="sxs-lookup"><span data-stu-id="4047e-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="4047e-110">SignalR ハブを構成します。</span><span class="sxs-lookup"><span data-stu-id="4047e-110">Configure SignalR hubs</span></span>

<span data-ttu-id="4047e-111">SignalR のミドルウェアが一部のサービスでは、呼び出すことによって構成を必要と`services.AddSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="4047e-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="4047e-112">SignalR の機能を ASP.NET Core アプリケーションを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4047e-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="4047e-113">作成し、ハブの使用</span><span class="sxs-lookup"><span data-stu-id="4047e-113">Create and use hubs</span></span>

<span data-ttu-id="4047e-114">継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="4047e-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="4047e-115">クライアントとして定義されているメソッドを呼び出すことができます`public`です。</span><span class="sxs-lookup"><span data-stu-id="4047e-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="4047e-116">戻り値の型と複合型アレイなど、任意の c# のメソッドの場合と、パラメーターを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4047e-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="4047e-117">SignalR では、複雑なオブジェクトおよびパラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。</span><span class="sxs-lookup"><span data-stu-id="4047e-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="4047e-118">クライアント オブジェクト</span><span class="sxs-lookup"><span data-stu-id="4047e-118">The Clients object</span></span>

<span data-ttu-id="4047e-119">各インスタンス、`Hub`クラスという名前のプロパティには`Clients`サーバーとクライアント間の通信は、次のメンバーを格納しています。</span><span class="sxs-lookup"><span data-stu-id="4047e-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="4047e-120">プロパティ</span><span class="sxs-lookup"><span data-stu-id="4047e-120">Property</span></span> | <span data-ttu-id="4047e-121">説明</span><span class="sxs-lookup"><span data-stu-id="4047e-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="4047e-122">接続されているすべてのクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4047e-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="4047e-123">ハブ メソッドを呼び出したクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4047e-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="4047e-124">メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4047e-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="4047e-125">さらに、`Hub`クラスには、次のメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4047e-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="4047e-126">メソッド</span><span class="sxs-lookup"><span data-stu-id="4047e-126">Method</span></span> | <span data-ttu-id="4047e-127">説明</span><span class="sxs-lookup"><span data-stu-id="4047e-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="4047e-128">指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4047e-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="4047e-129">特定の接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4047e-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="4047e-130">特定の接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4047e-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="4047e-131">指定したグループ内のすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="4047e-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="4047e-132">指定した接続を除く、指定したグループ内のすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="4047e-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="4047e-133">接続の複数のグループにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="4047e-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="4047e-134">ハブ メソッドを呼び出したクライアントを除く、接続のグループにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="4047e-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="4047e-135">特定のユーザーに関連付けられているすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="4047e-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="4047e-136">指定されたユーザーに関連付けられているすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="4047e-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="4047e-137">各プロパティまたは上記のテーブル内のメソッドを持つオブジェクトを返します、`SendAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4047e-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="4047e-138">`SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4047e-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="4047e-139">クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="4047e-139">Send messages to clients</span></span>

<span data-ttu-id="4047e-140">特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4047e-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="4047e-141">次の例では、次に、`SendMessageToCaller`ハブ メソッドを呼び出した接続にメッセージを送信するメソッドに示します。</span><span class="sxs-lookup"><span data-stu-id="4047e-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="4047e-142">`SendMessageToGroups`メソッドに格納されているグループにメッセージを送信する、`List`という`groups`です。</span><span class="sxs-lookup"><span data-stu-id="4047e-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="4047e-143">接続のイベントの処理</span><span class="sxs-lookup"><span data-stu-id="4047e-143">Handle events for a connection</span></span>

<span data-ttu-id="4047e-144">SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`仮想メソッドを管理し、接続を追跡します。</span><span class="sxs-lookup"><span data-stu-id="4047e-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="4047e-145">上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4047e-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="4047e-146">エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="4047e-146">Handle errors</span></span>

<span data-ttu-id="4047e-147">ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="4047e-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="4047e-148">JavaScript クライアントで、`invoke`メソッドを返します、 [JavaScript の約束](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)です。</span><span class="sxs-lookup"><span data-stu-id="4047e-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="4047e-149">Promise を使用して、接続されているクライアントがハンドラーでエラーを受信すると`catch`、呼び出され、JavaScript に渡されることが`Error`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4047e-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="4047e-150">関連資料</span><span class="sxs-lookup"><span data-stu-id="4047e-150">Related resources</span></span>

[<span data-ttu-id="4047e-151">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="4047e-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)