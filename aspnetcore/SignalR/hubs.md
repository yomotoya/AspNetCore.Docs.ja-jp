---
title: ASP.NET Core SignalR ハブを使用します。
author: rachelappel
description: ASP.NET Core SignalR ハブを使用する方法を説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: e23d7ef6d5e5e93d5fc69ad4c845a6a896836170
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="e76d9-103">ASP.NET Core の SignalR でハブを使用します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="e76d9-104">によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="e76d9-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="e76d9-105">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="e76d9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="e76d9-106">SignalR ハブとは</span><span class="sxs-lookup"><span data-stu-id="e76d9-106">What is a SignalR hub</span></span>

<span data-ttu-id="e76d9-107">SignalR ハブ API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="e76d9-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="e76d9-108">サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="e76d9-109">クライアント コードでは、サーバーから呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="e76d9-110">バック グラウンドでリアルタイムのクライアントとサーバーおよびサーバーからクライアントへの通信を可能にするすべての SignalR によって行われます。</span><span class="sxs-lookup"><span data-stu-id="e76d9-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="e76d9-111">SignalR ハブを構成します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-111">Configure SignalR hubs</span></span>

<span data-ttu-id="e76d9-112">SignalR のミドルウェアが一部のサービスでは、呼び出すことによって構成を必要と`services.AddSignalR`です。</span><span class="sxs-lookup"><span data-stu-id="e76d9-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=37)]

<span data-ttu-id="e76d9-113">SignalR の機能を ASP.NET Core アプリケーションを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e76d9-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=56-59)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="e76d9-114">作成し、ハブの使用</span><span class="sxs-lookup"><span data-stu-id="e76d9-114">Create and use hubs</span></span>

<span data-ttu-id="e76d9-115">継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="e76d9-116">クライアントとして定義されているメソッドを呼び出すことができます`public`です。</span><span class="sxs-lookup"><span data-stu-id="e76d9-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="e76d9-117">戻り値の型と複合型アレイなど、任意の c# のメソッドの場合と、パラメーターを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="e76d9-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="e76d9-118">SignalR では、複雑なオブジェクトおよびパラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="e76d9-119">クライアント オブジェクト</span><span class="sxs-lookup"><span data-stu-id="e76d9-119">The Clients object</span></span>

<span data-ttu-id="e76d9-120">各インスタンス、`Hub`クラスという名前のプロパティには`Clients`サーバーとクライアント間の通信は、次のメンバーを格納しています。</span><span class="sxs-lookup"><span data-stu-id="e76d9-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="e76d9-121">プロパティ</span><span class="sxs-lookup"><span data-stu-id="e76d9-121">Property</span></span> | <span data-ttu-id="e76d9-122">説明</span><span class="sxs-lookup"><span data-stu-id="e76d9-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="e76d9-123">接続されているすべてのクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="e76d9-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="e76d9-124">ハブ メソッドを呼び出したクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="e76d9-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="e76d9-125">メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="e76d9-125">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="e76d9-126">さらに、`Hub`クラスには、次のメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e76d9-126">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="e76d9-127">メソッド</span><span class="sxs-lookup"><span data-stu-id="e76d9-127">Method</span></span> | <span data-ttu-id="e76d9-128">説明</span><span class="sxs-lookup"><span data-stu-id="e76d9-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="e76d9-129">指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="e76d9-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="e76d9-130">特定の接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="e76d9-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="e76d9-131">特定の接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="e76d9-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="e76d9-132">指定したグループ内のすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-132">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="e76d9-133">指定した接続を除く、指定したグループ内のすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-133">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="e76d9-134">接続の複数のグループにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-134">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="e76d9-135">ハブ メソッドを呼び出したクライアントを除く、接続のグループにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-135">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="e76d9-136">特定のユーザーに関連付けられているすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-136">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="e76d9-137">指定されたユーザーに関連付けられているすべての接続にメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-137">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="e76d9-138">各プロパティまたは上記のテーブル内のメソッドを持つオブジェクトを返します、`SendAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e76d9-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="e76d9-139">`SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="e76d9-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="e76d9-140">クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-140">Send messages to clients</span></span>

<span data-ttu-id="e76d9-141">特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e76d9-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="e76d9-142">次の例では、次に、`SendMessageToCaller`ハブ メソッドを呼び出した接続にメッセージを送信するメソッドに示します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-142">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="e76d9-143">`SendMessageToGroups`メソッドに格納されているグループにメッセージを送信する、`List`という`groups`です。</span><span class="sxs-lookup"><span data-stu-id="e76d9-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="e76d9-144">接続のイベントの処理</span><span class="sxs-lookup"><span data-stu-id="e76d9-144">Handle events for a connection</span></span>

<span data-ttu-id="e76d9-145">SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`仮想メソッドを管理し、接続を追跡します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="e76d9-146">上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e76d9-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="e76d9-147">エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="e76d9-147">Handle errors</span></span>

<span data-ttu-id="e76d9-148">ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="e76d9-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="e76d9-149">JavaScript クライアントで、`invoke`メソッドを返します、 [JavaScript の約束](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)です。</span><span class="sxs-lookup"><span data-stu-id="e76d9-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="e76d9-150">Promise を使用して、接続されているクライアントがハンドラーでエラーを受信すると`catch`、呼び出され、JavaScript に渡されることが`Error`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e76d9-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=22)]

## <a name="related-resources"></a><span data-ttu-id="e76d9-151">関連資料</span><span class="sxs-lookup"><span data-stu-id="e76d9-151">Related resources</span></span>

[<span data-ttu-id="e76d9-152">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="e76d9-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)