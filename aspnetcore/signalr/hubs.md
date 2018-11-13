---
title: ASP.NET Core SignalR のハブを使用します。
author: tdykstra
description: ASP.NET Core SignalR のハブを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/hubs
ms.openlocfilehash: 0413d354307208726f4252f431ac59526effed08
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569920"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="48096-103">ASP.NET core SignalR のハブの使用</span><span class="sxs-lookup"><span data-stu-id="48096-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="48096-104">によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="48096-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="48096-105">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="48096-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="48096-106">SignalR ハブとは</span><span class="sxs-lookup"><span data-stu-id="48096-106">What is a SignalR hub</span></span>

<span data-ttu-id="48096-107">SignalR ハブの API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="48096-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="48096-108">サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="48096-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="48096-109">クライアント コードでは、サーバーから呼び出されるメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="48096-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="48096-110">SignalR は、バック グラウンドでクライアントとサーバーとサーバーからクライアントのリアルタイム通信を可能にするすべての処理されます。</span><span class="sxs-lookup"><span data-stu-id="48096-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="48096-111">SignalR ハブを構成します。</span><span class="sxs-lookup"><span data-stu-id="48096-111">Configure SignalR hubs</span></span>

<span data-ttu-id="48096-112">SignalR のミドルウェアがサービスによっては、呼び出すことによって構成されている必要があります`services.AddSignalR`します。</span><span class="sxs-lookup"><span data-stu-id="48096-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="48096-113">SignalR の機能を ASP.NET Core アプリを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="48096-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="48096-114">作成し、ハブの使用</span><span class="sxs-lookup"><span data-stu-id="48096-114">Create and use hubs</span></span>

<span data-ttu-id="48096-115">継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="48096-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="48096-116">クライアントとして定義されているメソッドを呼び出すことができます`public`します。</span><span class="sxs-lookup"><span data-stu-id="48096-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="48096-117">戻り値の型と複合型と配列を含む c# メソッドの場合と、パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="48096-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="48096-118">SignalR では、複雑なオブジェクトと、パラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。</span><span class="sxs-lookup"><span data-stu-id="48096-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="48096-119">ハブは、一時的なものです。</span><span class="sxs-lookup"><span data-stu-id="48096-119">Hubs are transient:</span></span>
> * <span data-ttu-id="48096-120">ハブ クラスのプロパティの状態を保存しないでください。</span><span class="sxs-lookup"><span data-stu-id="48096-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="48096-121">すべてのハブ メソッド呼び出しは、新しいハブ インスタンスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="48096-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="48096-122">使用`await`ままになります、ハブに依存する非同期のメソッドを呼び出すとき。</span><span class="sxs-lookup"><span data-stu-id="48096-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="48096-123">などのメソッドではたとえば、`Clients.All.SendAsync(...)`が失敗することがなく呼び出された場合`await`ハブ メソッドの完了と`SendAsync`が完了するとします。</span><span class="sxs-lookup"><span data-stu-id="48096-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="48096-124">コンテキスト オブジェクト</span><span class="sxs-lookup"><span data-stu-id="48096-124">The Context object</span></span>

<span data-ttu-id="48096-125">`Hub`クラスには、`Context`接続に関する情報を次のプロパティを含むプロパティ。</span><span class="sxs-lookup"><span data-stu-id="48096-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="48096-126">プロパティ</span><span class="sxs-lookup"><span data-stu-id="48096-126">Property</span></span> | <span data-ttu-id="48096-127">説明</span><span class="sxs-lookup"><span data-stu-id="48096-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="48096-128">SignalR によって割り当てられている接続の一意の ID を取得します。</span><span class="sxs-lookup"><span data-stu-id="48096-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="48096-129">接続ごとに 1 つの接続 ID があります。</span><span class="sxs-lookup"><span data-stu-id="48096-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="48096-130">取得、[ユーザー識別子](xref:signalr/groups)します。</span><span class="sxs-lookup"><span data-stu-id="48096-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="48096-131">既定では、SignalR を使用して、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`に関連付けられたユーザー識別子として接続します。</span><span class="sxs-lookup"><span data-stu-id="48096-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="48096-132">取得、`ClaimsPrincipal`現在のユーザーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="48096-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="48096-133">この接続のスコープ内のデータを共有するために使用できるキー/値コレクションを取得します。</span><span class="sxs-lookup"><span data-stu-id="48096-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="48096-134">このコレクションに格納できるデータおよび接続の別のハブ メソッド呼び出し間で保持されます。</span><span class="sxs-lookup"><span data-stu-id="48096-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="48096-135">接続で使用できる機能のコレクションを取得します。</span><span class="sxs-lookup"><span data-stu-id="48096-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="48096-136">ここでは、このコレクションは必要ありません、ほとんどのシナリオでまだ詳しく記載されていないためです。</span><span class="sxs-lookup"><span data-stu-id="48096-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="48096-137">取得、`CancellationToken`接続が中止されたときに通知します。</span><span class="sxs-lookup"><span data-stu-id="48096-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="48096-138">`Hub.Context` また、次のメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="48096-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="48096-139">メソッド</span><span class="sxs-lookup"><span data-stu-id="48096-139">Method</span></span> | <span data-ttu-id="48096-140">説明</span><span class="sxs-lookup"><span data-stu-id="48096-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="48096-141">返します、`HttpContext`接続、または`null`接続が HTTP 要求に関連付けられていない場合。</span><span class="sxs-lookup"><span data-stu-id="48096-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="48096-142">HTTP 接続で HTTP ヘッダーやクエリ文字列などの情報を取得するのにこのメソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="48096-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="48096-143">接続を中止します。</span><span class="sxs-lookup"><span data-stu-id="48096-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="48096-144">クライアント オブジェクト</span><span class="sxs-lookup"><span data-stu-id="48096-144">The Clients object</span></span>

<span data-ttu-id="48096-145">`Hub`クラスには、`Clients`サーバーとクライアント間の通信は、次のプロパティを含むプロパティ。</span><span class="sxs-lookup"><span data-stu-id="48096-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="48096-146">プロパティ</span><span class="sxs-lookup"><span data-stu-id="48096-146">Property</span></span> | <span data-ttu-id="48096-147">説明</span><span class="sxs-lookup"><span data-stu-id="48096-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="48096-148">接続されているすべてのクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="48096-149">ハブ メソッドを呼び出したクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="48096-150">メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-150">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="48096-151">`Hub.Clients` また、次のメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="48096-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="48096-152">メソッド</span><span class="sxs-lookup"><span data-stu-id="48096-152">Method</span></span> | <span data-ttu-id="48096-153">説明</span><span class="sxs-lookup"><span data-stu-id="48096-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="48096-154">指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="48096-155">特定の接続されているクライアントのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="48096-156">接続されているクライアントが特定のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="48096-157">指定したグループ内のすべての接続のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="48096-158">指定された接続を除く、指定したグループ内のすべての接続のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="48096-159">接続のグループを複数のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="48096-160">ハブ メソッドを呼び出したクライアントを除く、接続のグループのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="48096-161">特定のユーザーに関連付けられているすべての接続のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="48096-162">指定されたユーザーに関連付けられているすべての接続のメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="48096-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="48096-163">プロパティまたはメソッドでは、上記のテーブルごとにオブジェクトを返します、`SendAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="48096-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="48096-164">`SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="48096-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="48096-165">クライアントにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="48096-165">Send messages to clients</span></span>

<span data-ttu-id="48096-166">特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="48096-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="48096-167">次の例では、次の 3 つのハブ メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="48096-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="48096-168">`SendMessage` 使用して、接続されているすべてのクライアントにメッセージを送信`Clients.All`します。</span><span class="sxs-lookup"><span data-stu-id="48096-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="48096-169">`SendMessageToCaller` 使用して、呼び出し元にメッセージを送信`Clients.Caller`します。</span><span class="sxs-lookup"><span data-stu-id="48096-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="48096-170">`SendMessageToGroups` すべてのクライアントにメッセージを送信、`SignalR Users`グループ。</span><span class="sxs-lookup"><span data-stu-id="48096-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="48096-171">厳密に型指定されたハブ</span><span class="sxs-lookup"><span data-stu-id="48096-171">Strongly typed hubs</span></span>

<span data-ttu-id="48096-172">使用する欠点`SendAsync`が呼び出されるクライアント メソッドを指定する文字列に依存していること。</span><span class="sxs-lookup"><span data-stu-id="48096-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="48096-173">これにより、メソッド名のスペルが正しい場合、ランタイム エラー コードを開くまたはクライアントから不足しています。</span><span class="sxs-lookup"><span data-stu-id="48096-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="48096-174">使用する代わりに`SendAsync`を厳密に型が、`Hub`で<xref:Microsoft.AspNetCore.SignalR.Hub`1>します。</span><span class="sxs-lookup"><span data-stu-id="48096-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="48096-175">次の例では、`ChatHub`クライアント メソッドがアウトというインターフェイスに抽出された`IChatClient`します。</span><span class="sxs-lookup"><span data-stu-id="48096-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="48096-176">リファクタリングの前に、このインターフェイスを使用する`ChatHub`例。</span><span class="sxs-lookup"><span data-stu-id="48096-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="48096-177">使用して`Hub<IChatClient>`クライアント メソッドのコンパイル時にチェックできるようにします。</span><span class="sxs-lookup"><span data-stu-id="48096-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="48096-178">これにより、問題のため、マジック文字列を使用することで`Hub<T>`のみ、インターフェイスで定義されたメソッドへのアクセスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="48096-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="48096-179">厳密に型指定を使用して`Hub<T>`を使用する機能を無効にします。`SendAsync`します。</span><span class="sxs-lookup"><span data-stu-id="48096-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="48096-180">ハブ メソッドの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="48096-180">Change the name of a hub method</span></span>

<span data-ttu-id="48096-181">既定では、サーバー ハブのメソッド名は、.NET メソッドの名前です。</span><span class="sxs-lookup"><span data-stu-id="48096-181">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="48096-182">ただし、使用することができます、 [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute)この既定の設定を変更し、手動で、メソッドの名前を指定する属性。</span><span class="sxs-lookup"><span data-stu-id="48096-182">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="48096-183">クライアントは、メソッドを呼び出すときに、.NET のメソッド名の代わりにこの名前を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="48096-183">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="48096-184">接続のイベントの処理</span><span class="sxs-lookup"><span data-stu-id="48096-184">Handle events for a connection</span></span>

<span data-ttu-id="48096-185">SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`接続を管理するための仮想メソッド。</span><span class="sxs-lookup"><span data-stu-id="48096-185">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="48096-186">上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッド。</span><span class="sxs-lookup"><span data-stu-id="48096-186">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="48096-187">上書き、`OnDisconnectedAsync`クライアントが切断されたときにアクションを実行する仮想メソッド。</span><span class="sxs-lookup"><span data-stu-id="48096-187">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="48096-188">クライアントが意図的に切断した場合 (呼び出して`connection.stop()`など)、`exception`パラメーターになります`null`します。</span><span class="sxs-lookup"><span data-stu-id="48096-188">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="48096-189">ただし、エラー (ネットワーク障害の場合) などのため、クライアントが切断されている場合は、`exception`パラメーターには、例外、エラーを説明します。</span><span class="sxs-lookup"><span data-stu-id="48096-189">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="48096-190">エラー処理</span><span class="sxs-lookup"><span data-stu-id="48096-190">Handle errors</span></span>

<span data-ttu-id="48096-191">ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="48096-191">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="48096-192">JavaScript クライアントで、`invoke`メソッドを返します。 を[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)します。</span><span class="sxs-lookup"><span data-stu-id="48096-192">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="48096-193">クライアントが、ハンドラーでエラーを受信すると promise を使用して、接続されている`catch`、呼び出されるは、JavaScript として渡される`Error`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="48096-193">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="48096-194">既定では場合、ハブ、例外がスロー SignalR を返し、一般的なエラー メッセージをクライアントにします。</span><span class="sxs-lookup"><span data-stu-id="48096-194">By default, if your Hub throws an exception, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="48096-195">例えば:</span><span class="sxs-lookup"><span data-stu-id="48096-195">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="48096-196">多くの場合、予期しない例外には、データベース接続が失敗したときにトリガーされます例外内のデータベース サーバーの名前などの機密情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="48096-196">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="48096-197">SignalR は、セキュリティ対策として、既定でこれらの詳細なエラー メッセージを公開しません。</span><span class="sxs-lookup"><span data-stu-id="48096-197">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="48096-198">参照してください、[セキュリティに関する考慮事項に関する記事](xref:signalr/security#exceptions)理由の詳細については、例外の詳細が表示されません。</span><span class="sxs-lookup"><span data-stu-id="48096-198">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="48096-199">ある場合、例外的な条件を*は*クライアントに反映されるまでは、使用することができます、`HubException`クラス。</span><span class="sxs-lookup"><span data-stu-id="48096-199">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="48096-200">スローする場合、 `HubException` 、SignalR hub メソッドから**は**メッセージ全体を未変更の状態、クライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="48096-200">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="48096-201">SignalR のみを送信、`Message`クライアントに例外のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="48096-201">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="48096-202">スタック トレースと例外の他のプロパティは、クライアントに使用できません。</span><span class="sxs-lookup"><span data-stu-id="48096-202">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="48096-203">関連資料</span><span class="sxs-lookup"><span data-stu-id="48096-203">Related resources</span></span>

* [<span data-ttu-id="48096-204">ASP.NET Core SignalR の概要</span><span class="sxs-lookup"><span data-stu-id="48096-204">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="48096-205">JavaScript クライアント</span><span class="sxs-lookup"><span data-stu-id="48096-205">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="48096-206">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="48096-206">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
