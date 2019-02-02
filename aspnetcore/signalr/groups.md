---
title: SignalR のユーザーとグループを管理します。
author: bradygaster
description: ASP.NET Core SignalR のユーザーとグループの管理の概要です。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667753"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="cd6c4-103">SignalR のユーザーとグループを管理します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="cd6c4-104">によって[真紀 Brennan](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="cd6c4-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="cd6c4-105">SignalR では、特定のユーザーに関連付けられているすべての接続に送信するだけでなく接続の名前付きグループにメッセージを許可します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="cd6c4-106">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="cd6c4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="cd6c4-107">SignalR でのユーザー</span><span class="sxs-lookup"><span data-stu-id="cd6c4-107">Users in SignalR</span></span>

<span data-ttu-id="cd6c4-108">SignalR では、特定のユーザーに関連付けられているすべての接続にメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="cd6c4-109">既定では、SignalR を使用して、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`に関連付けられたユーザー識別子として接続します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="cd6c4-110">1 人のユーザーには、SignalR のアプリに複数の接続を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="cd6c4-111">たとえば、ユーザーが自分のデスクトップと携帯電話接続でした。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="cd6c4-112">各デバイスには、別々 の SignalR 接続がすべて、同じユーザーに関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="cd6c4-113">メッセージは、ユーザーに送信される、すべてのユーザーに関連付けられている接続と、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="cd6c4-114">接続のユーザー id によってアクセスできる、`Context.UserIdentifier`ハブ内のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="cd6c4-115">ユーザー id を渡すことによって、特定のユーザーにメッセージを送信、`User`次の例で示すように、ハブ メソッドで機能します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="cd6c4-116">ユーザー識別子は、大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a><span data-ttu-id="cd6c4-117">SignalR でグループ</span><span class="sxs-lookup"><span data-stu-id="cd6c4-117">Groups in SignalR</span></span>

<span data-ttu-id="cd6c4-118">グループは、名前に関連付けられている接続のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="cd6c4-119">グループ内のすべての接続にメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="cd6c4-120">グループは、グループは、アプリケーションによって管理されるため、接続または複数の接続に送信することをお勧めの方法です。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="cd6c4-121">接続は、複数のグループのメンバーであることができます。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="cd6c4-122">これによりグループ理想的なチャット アプリケーションのようなものは、各部屋をグループとして表現できます。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="cd6c4-123">接続を追加またはを使用してグループから削除することができます、`AddToGroupAsync`と`RemoveFromGroupAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="cd6c4-124">接続が再接続されると、グループ メンバーシップは保持されません。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="cd6c4-125">接続が再確立されているときに、グループに再度参加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="cd6c4-126">この情報は、アプリケーションが複数のサーバーにスケーリングされる場合に使用ではないため、グループのメンバーをカウントすることはできません。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="cd6c4-127">リソースへのアクセスを保護するグループを使用しているときに、使用[認証と承認](xref:signalr/authn-and-authz)ASP.NET Core で機能します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="cd6c4-128">追加した場合のみユーザーをグループに、資格情報がそのグループの有効な場合、そのグループに送信されるメッセージは承認されたユーザーにのみ移動します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="cd6c4-129">ただし、グループは、セキュリティ機能ではありません。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-129">However, groups are not a security feature.</span></span> <span data-ttu-id="cd6c4-130">認証要求には、有効期限と取り消しなど、グループにはない機能があります。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="cd6c4-131">グループにアクセスするユーザーのアクセス許可が取り消された場合は、手動で検出されると、グループから削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="cd6c4-132">グループの名前が大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="cd6c4-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="cd6c4-133">関連資料</span><span class="sxs-lookup"><span data-stu-id="cd6c4-133">Related resources</span></span>

* [<span data-ttu-id="cd6c4-134">開始するには</span><span class="sxs-lookup"><span data-stu-id="cd6c4-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="cd6c4-135">ハブ</span><span class="sxs-lookup"><span data-stu-id="cd6c4-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="cd6c4-136">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="cd6c4-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
