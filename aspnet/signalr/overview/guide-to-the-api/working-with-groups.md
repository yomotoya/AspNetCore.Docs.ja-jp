---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: SignalR でグループの操作 |Microsoft ドキュメント
author: pfletcher
description: このトピックでは、ハブ API でのグループ メンバーシップ情報を永続化する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042223"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="1eaa5-103">SignalR でグループの操作</span><span class="sxs-lookup"><span data-stu-id="1eaa5-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="1eaa5-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1eaa5-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1eaa5-105">このトピックでは、ユーザーをグループに追加し、グループ メンバーシップ情報を永続化する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="1eaa5-106">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="1eaa5-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="1eaa5-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1eaa5-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="1eaa5-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="1eaa5-108">.NET 4.5</span></span>
> - <span data-ttu-id="1eaa5-109">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="1eaa5-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="1eaa5-110">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="1eaa5-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="1eaa5-111">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="1eaa5-112">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="1eaa5-112">Questions and comments</span></span>
> 
> <span data-ttu-id="1eaa5-113">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="1eaa5-114">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="1eaa5-115">概要</span><span class="sxs-lookup"><span data-stu-id="1eaa5-115">Overview</span></span>

<span data-ttu-id="1eaa5-116">SignalR でグループは、接続しているクライアントの指定されたサブセットにメッセージをブロードキャストする方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="1eaa5-117">グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="1eaa5-118">グループを明示的に作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="1eaa5-119">実際には、初めて Groups.Add への呼び出しでその名前を指定したグループが自動的に作成し、メンバーシップから最後の接続を削除する場合は削除します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="1eaa5-120">グループの使用の概要については、次を参照してください。[ハブ クラスからグループのメンバーシップを管理する方法](hubs-api-guide-server.md#groupsfromhub)Hubs api - Server ガイドです。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="1eaa5-121">グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="1eaa5-122">SignalR では、クライアントと、パブリッシュ/サブスクライブ モデルに基づくグループにメッセージを送信し、サーバーがグループまたはグループ メンバーシップの一覧を管理しません。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="1eaa5-123">これにより、スケーラビリティを最大化 SignalR を保持するいずれかの状態は、新しいノードに反映する必要が web ファームにノードを追加するたびにできます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="1eaa5-124">使用してグループにユーザーを追加すると、`Groups.Add`メソッド、ユーザーが、現在の接続の間、そのグループに送信されるメッセージを受け取りますが、そのグループ内のユーザーのメンバーシップは期間を超えて保持されません、現在の接続。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="1eaa5-125">グループとグループ メンバーシップに関する情報を完全に保持する場合は、データベースや Azure テーブル ストレージなどのリポジトリにそのデータを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="1eaa5-126">そのため、アプリケーション、ユーザーが接続するたびにするリポジトリから、ユーザーが属するグループを取得し、それらのグループにそのユーザーを手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="1eaa5-127">再接続時に一時的に中断した後、ユーザーに自動的に再参加以前に割り当てられているグループ。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="1eaa5-128">グループを自動的に再参加は、再接続時に、新しい接続を確立するときではなく時にのみ適用します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="1eaa5-129">デジタル署名されたトークンは、以前に割り当てられているグループの一覧を含む、クライアントから渡されます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="1eaa5-130">ユーザーが要求されたグループに属しているかどうかを確認する場合は、既定の動作をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="1eaa5-131">このトピックには、次のセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="1eaa5-132">追加して、ユーザーを削除します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="1eaa5-133">グループのメンバーの呼び出し</span><span class="sxs-lookup"><span data-stu-id="1eaa5-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="1eaa5-134">グループ メンバーシップをデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="1eaa5-135">Azure テーブル ストレージでファイルを格納するグループのメンバーシップ</span><span class="sxs-lookup"><span data-stu-id="1eaa5-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="1eaa5-136">再接続するときに、グループ メンバーシップを確認します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="1eaa5-137">追加して、ユーザーを削除します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-137">Adding and removing users</span></span>

<span data-ttu-id="1eaa5-138">呼び出すを追加またはグループからユーザーを削除する、[追加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)または[削除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)メソッド、およびユーザーの接続の id とグループ名のパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="1eaa5-139">接続の終了時に、グループからユーザーを手動で削除する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="1eaa5-140">次の例は、`Groups.Add`と`Groups.Remove`ハブ メソッドで使用される方法です。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="1eaa5-141">`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="1eaa5-142">クライアントをグループに追加し、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、Groups.Add メソッドが最初に終了するかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="1eaa5-143">次のコード例を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="1eaa5-144">一般に、含める必要はありません`await`を呼び出すときに、`Groups.Remove`メソッドを削除しようとしている接続の id が使用可能な不要になった可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="1eaa5-145">その場合は、`TaskCanceledException`が、要求がタイムアウトした後にスローされます。追加できるかどうか、アプリケーションがグループにメッセージを送信する前に、ユーザー、グループから削除されていることを確認する必要があります、 `await` Groups.Remove、および、catch の前に、`TaskCanceledException`スローされる例外。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="1eaa5-146">グループのメンバーの呼び出し</span><span class="sxs-lookup"><span data-stu-id="1eaa5-146">Calling members of a group</span></span>

<span data-ttu-id="1eaa5-147">次の例に示すように、すべてのグループのメンバーまたはグループの唯一の指定したメンバーにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="1eaa5-148">**すべて**指定したグループ内のクライアントを接続します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="1eaa5-149">指定したグループ内のクライアントが接続されているすべて **、指定されたクライアントを除く**接続 ID によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="1eaa5-150">指定したグループ内のクライアントが接続されているすべて**呼び出し元のクライアントを除く**です。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="1eaa5-151">グループ メンバーシップをデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-151">Storing group membership in a database</span></span>

<span data-ttu-id="1eaa5-152">次の例では、データベース内のグループとユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="1eaa5-153">任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="1eaa5-154">これらのエンティティ モデルは、データベースのテーブルとフィールドに対応します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="1eaa5-155">データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="1eaa5-156">この例には、という名前のクラスが含まれています。`ConversationRoom`スポーツまたはガーデニングなどのさまざまな主題メッセージ交換に参加できるようにするアプリケーションに一意などれか。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="1eaa5-157">この例では、接続するためのクラスも含まれます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="1eaa5-158">接続クラスは、グループ メンバーシップを追跡するため、どうしても必要はありませんが、ユーザーを追跡する堅牢なソリューションの一部では多くの場合。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="1eaa5-159">次に、ハブでは、データベースから、グループとユーザー情報を取得し、適切なグループにユーザーを手動で追加できます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="1eaa5-160">この例では、ユーザー接続を追跡するためのコードは含まれません。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="1eaa5-161">この例では、`await`する前にキーワードが適用されていない`Groups.Add`グループのメンバーに、メッセージがすぐに送信されないためです。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="1eaa5-162">適用するグループのすべてのメンバーに、新しいメンバーを追加した後すぐにメッセージを送信する場合、`await`キーワード、非同期操作が完了したかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="1eaa5-163">Azure テーブル ストレージでファイルを格納するグループのメンバーシップ</span><span class="sxs-lookup"><span data-stu-id="1eaa5-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="1eaa5-164">Azure テーブル ストレージを使用して、グループとユーザーの情報を格納するは、データベースを使用してに似ています。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="1eaa5-165">次の例では、ユーザー名とグループ名を格納するテーブル エンティティを示します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="1eaa5-166">ハブで、ユーザーが接続するときに割り当てられているグループを取得します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="1eaa5-167">再接続するときに、グループ メンバーシップを確認します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="1eaa5-168">既定では、SignalR 自動的に再ユーザー、グループに割り当てます適切な接続は削除され、接続がタイムアウトする前に再び確立した場合など、一時的に中断から再接続するときにします。、再接続するときに、ユーザーのグループの情報がトークンで渡され、サーバーでそのトークンを検証します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="1eaa5-169">グループへのユーザーの再参加の検証プロセスについては、次を参照してください。[再接続するときにグループの再参加](../security/introduction-to-security.md#rejoingroup)です。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="1eaa5-170">一般に、グループに再接続を自動的に再参加の既定の動作を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="1eaa5-171">SignalR のグループは、機密データへのアクセスを制限するためのセキュリティ メカニズムとしてのものではありません。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="1eaa5-172">ただし、再接続するときに、アプリケーションはユーザーのグループ メンバーシップを再確認する必要があります、既定の動作をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="1eaa5-173">既定の動作を変更することができます、負担、データベースに追加各再接続ではなく、ユーザーが接続するときにだけ、ユーザーのグループ メンバーシップを取得する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="1eaa5-174">グループのメンバーシップを確認する必要がある場合は、再接続、次に示すように、割り当てられているグループの一覧を返す新しいハブ パイプライン モジュールを作成します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="1eaa5-175">その後、そのモジュールを以下に示すように、ハブ パイプラインに追加します。</span><span class="sxs-lookup"><span data-stu-id="1eaa5-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
