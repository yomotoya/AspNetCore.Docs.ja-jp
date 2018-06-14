---
title: SignalR のユーザーとグループを管理します。
author: rachelappel
description: ASP.NET Core SignalR のユーザーとグループ管理の概要です。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358440"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="a1845-103">SignalR のユーザーとグループを管理します。</span><span class="sxs-lookup"><span data-stu-id="a1845-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="a1845-104">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="a1845-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="a1845-105">SignalR には、特定のユーザーに関連付けられているすべての接続に送信するだけでなく名前付き接続のグループへのメッセージが可能です。</span><span class="sxs-lookup"><span data-stu-id="a1845-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="a1845-106">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a1845-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="a1845-107">SignalR のユーザー</span><span class="sxs-lookup"><span data-stu-id="a1845-107">Users in SignalR</span></span>

<span data-ttu-id="a1845-108">SignalR では、特定のユーザーに関連付けられているすべての接続にメッセージを送信することができます。</span><span class="sxs-lookup"><span data-stu-id="a1845-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="a1845-109">SignalR を使用して既定では、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`ユーザー識別子としての接続に関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="a1845-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="a1845-110">1 人のユーザーには、SignalR アプリケーションを複数の接続を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="a1845-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="a1845-111">たとえば、ユーザーは自分のデスクトップだけでなく、携帯電話で接続でした。</span><span class="sxs-lookup"><span data-stu-id="a1845-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="a1845-112">各デバイスには、個々 の SignalR 接続がすべて同じユーザーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="a1845-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="a1845-113">メッセージは、ユーザーに送信される、すべてのユーザーに関連付けられている接続と、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a1845-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="a1845-114">ユーザー id を渡すことによって、特定のユーザーにメッセージを送信、`User`次の例で示すようにハブ メソッドで機能します。</span><span class="sxs-lookup"><span data-stu-id="a1845-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="a1845-115">ユーザー識別子は、大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="a1845-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="a1845-116">ユーザー識別子を作成することでカスタマイズできる、 `IUserIdProvider`、登録することで`ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="a1845-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="a1845-117">AddSignalR は、カスタムの SignalR サービスを登録する前に呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1845-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="a1845-118">SignalR でグループ</span><span class="sxs-lookup"><span data-stu-id="a1845-118">Groups in SignalR</span></span>

<span data-ttu-id="a1845-119">グループは、名前に関連付けられている接続のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="a1845-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="a1845-120">グループ内のすべての接続にメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="a1845-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="a1845-121">グループは、グループは、アプリケーションによって管理されるため、接続または複数の接続に送信することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a1845-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="a1845-122">接続は、複数のグループのメンバーであることができます。</span><span class="sxs-lookup"><span data-stu-id="a1845-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="a1845-123">これにより、グループ理想的なチャット アプリケーションのような出力を各部屋をグループとして表すことができます。</span><span class="sxs-lookup"><span data-stu-id="a1845-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="a1845-124">接続を追加またはを使用してグループから削除することができます、`AddToGroupAsync`と`RemoveFromGroupAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a1845-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="a1845-125">接続が再接続されると、グループのメンバーシップは保持されません。</span><span class="sxs-lookup"><span data-stu-id="a1845-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="a1845-126">接続が再確立される、グループに再度参加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1845-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="a1845-127">この情報は、アプリケーションは複数のサーバーにスケーリング場合ではないため、グループのメンバーをカウントすることはできません。</span><span class="sxs-lookup"><span data-stu-id="a1845-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="a1845-128">グループ名では大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="a1845-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="a1845-129">関連資料</span><span class="sxs-lookup"><span data-stu-id="a1845-129">Related resources</span></span>

* [<span data-ttu-id="a1845-130">開始するには</span><span class="sxs-lookup"><span data-stu-id="a1845-130">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="a1845-131">ハブ</span><span class="sxs-lookup"><span data-stu-id="a1845-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a1845-132">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="a1845-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
