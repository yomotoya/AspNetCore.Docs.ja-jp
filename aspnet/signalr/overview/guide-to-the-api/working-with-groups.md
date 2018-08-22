---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: SignalR でグループの操作 |Microsoft Docs
author: pfletcher
description: このトピックでは、Hub API を使用したグループのメンバーシップ情報を永続化する方法について説明します。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 31dd3239e2e0cd919c23419459bc43ae8e1a6437
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829242"
---
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="3a04e-103">SignalR でグループの操作</span><span class="sxs-lookup"><span data-stu-id="3a04e-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="3a04e-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3a04e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3a04e-105">このトピックでは、ユーザーをグループに追加し、グループのメンバーシップ情報を保持する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3a04e-106">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="3a04e-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="3a04e-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3a04e-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="3a04e-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3a04e-108">.NET 4.5</span></span>
> - <span data-ttu-id="3a04e-109">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="3a04e-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3a04e-110">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="3a04e-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="3a04e-111">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="3a04e-112">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="3a04e-112">Questions and comments</span></span>
> 
> <span data-ttu-id="3a04e-113">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="3a04e-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3a04e-114">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="3a04e-115">概要</span><span class="sxs-lookup"><span data-stu-id="3a04e-115">Overview</span></span>

<span data-ttu-id="3a04e-116">SignalR でグループは、接続されているクライアントのサブセットを指定するメッセージをブロードキャストする方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="3a04e-117">グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。</span><span class="sxs-lookup"><span data-stu-id="3a04e-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="3a04e-118">グループを明示的に作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3a04e-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="3a04e-119">実際には、初めて Groups.Add への呼び出しでその名前を指定するグループを自動的に作成し、そのメンバーシップから最後の接続を削除する場合は削除します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="3a04e-120">グループの使用の概要については、次を参照してください。[ハブ クラスからグループ メンバーシップを管理する方法](hubs-api-guide-server.md#groupsfromhub)Hubs API - サーバー ガイドにします。</span><span class="sxs-lookup"><span data-stu-id="3a04e-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="3a04e-121">グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。</span><span class="sxs-lookup"><span data-stu-id="3a04e-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="3a04e-122">SignalR クライアントおよび、パブリッシュ/サブスクライブ モデルに基づいてグループにメッセージを送信して、サーバーは、グループまたはグループ メンバーシップの一覧を保持しません。</span><span class="sxs-lookup"><span data-stu-id="3a04e-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="3a04e-123">こうため、SignalR を保持する任意の状態が新しいノードに適用するのには web ファームにノードを追加するたびに、スケーラビリティを最大化します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="3a04e-124">使用してグループにユーザーを追加すると、`Groups.Add`メソッドでは、ユーザーは、現在の接続の期間中、そのグループに送信されるメッセージを受け取りますが、そのグループ内のユーザーのメンバーシップは、現在の接続を超えて保持されません。</span><span class="sxs-lookup"><span data-stu-id="3a04e-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="3a04e-125">グループとグループ メンバーシップに関する情報を完全に保持する場合は、データベースまたは Azure テーブル ストレージなどのリポジトリにデータを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a04e-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="3a04e-126">ユーザーがアプリケーションに接続するたびにするリポジトリから、ユーザーが属するグループを取得し、それらのグループにそのユーザーを手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="3a04e-127">一時中断の後再接続時に、ときに、ユーザーが自動的に再結合以前に割り当てられているグループ。</span><span class="sxs-lookup"><span data-stu-id="3a04e-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="3a04e-128">グループを自動的に再参加と、新しい接続を確立するときではなく、再接続時にのみ適用します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="3a04e-129">デジタル署名されたトークンは、以前に割り当てられているグループの一覧を含む、クライアントから渡されます。</span><span class="sxs-lookup"><span data-stu-id="3a04e-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="3a04e-130">ユーザーが要求されたグループに属しているかどうかを確認する場合は、既定の動作をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="3a04e-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="3a04e-131">このトピックには、次のセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="3a04e-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="3a04e-132">追加して、ユーザーを削除します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="3a04e-133">グループのメンバーの呼び出し</span><span class="sxs-lookup"><span data-stu-id="3a04e-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="3a04e-134">グループのメンバーシップをデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="3a04e-135">Azure table storage に格納するグループのメンバーシップ</span><span class="sxs-lookup"><span data-stu-id="3a04e-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="3a04e-136">再接続時にグループ メンバーシップの確認</span><span class="sxs-lookup"><span data-stu-id="3a04e-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="3a04e-137">追加して、ユーザーを削除します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-137">Adding and removing users</span></span>

<span data-ttu-id="3a04e-138">追加またはグループからユーザーを削除を呼び出す、[追加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)または[削除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)メソッド、およびユーザーの接続の id とグループの名前のパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="3a04e-139">接続の終了時に、グループからユーザーを手動で削除する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3a04e-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="3a04e-140">次の例は、`Groups.Add`と`Groups.Remove`ハブ メソッドで使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="3a04e-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="3a04e-141">`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="3a04e-142">クライアント グループを追加して、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、Groups.Add メソッドが最初に終了するかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a04e-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="3a04e-143">次のコード例では、その方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="3a04e-144">一般に、含める必要はありません`await`を呼び出すときに、`Groups.Remove`メソッドを削除しようとしている接続の id が使用可能な不要になった可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="3a04e-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="3a04e-145">その場合は、`TaskCanceledException`が、要求がタイムアウトした後にスローされます。追加できるかどうか、アプリケーションが、ユーザーがグループにメッセージを送信する前に、グループから削除されたことを確認する必要があります、`await`する前に`Groups.Remove`をキャッチし、`TaskCanceledException`がスローされる例外。</span><span class="sxs-lookup"><span data-stu-id="3a04e-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="3a04e-146">グループのメンバーの呼び出し</span><span class="sxs-lookup"><span data-stu-id="3a04e-146">Calling members of a group</span></span>

<span data-ttu-id="3a04e-147">次の例に示すように、すべてのグループのメンバーまたはグループの唯一の指定したメンバーにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="3a04e-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="3a04e-148">**すべて**指定したグループ内のクライアントを接続します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="3a04e-149">指定したグループ内のクライアントが接続されているすべて **、指定されたクライアントを除く**接続 ID によって識別されます。</span><span class="sxs-lookup"><span data-stu-id="3a04e-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="3a04e-150">指定したグループ内のクライアントが接続されているすべて**呼び出し元のクライアントを除く**します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="3a04e-151">グループのメンバーシップをデータベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-151">Storing group membership in a database</span></span>

<span data-ttu-id="3a04e-152">次の例では、データベース内のグループとユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="3a04e-153">任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="3a04e-154">これらのエンティティ モデルは、データベース テーブルとフィールドに対応します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="3a04e-155">データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3a04e-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="3a04e-156">この例には、という名前のクラスが含まれています。`ConversationRoom`スポーツやガーデンなどの異なるサブジェクトについての会話に参加できるアプリケーション固有であります。</span><span class="sxs-lookup"><span data-stu-id="3a04e-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="3a04e-157">この例では、接続するためのクラスも含まれています。</span><span class="sxs-lookup"><span data-stu-id="3a04e-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="3a04e-158">接続クラスは、グループ メンバーシップを追跡するために必須ではありませんが、ユーザーを追跡する堅牢なソリューションの一部では頻繁に。</span><span class="sxs-lookup"><span data-stu-id="3a04e-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="3a04e-159">次に、ハブでは、データベースから、グループとユーザー情報を取得し、適切なグループにユーザーを手動で追加できます。</span><span class="sxs-lookup"><span data-stu-id="3a04e-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="3a04e-160">この例では、ユーザー接続を追跡するためのコードは含まれません。</span><span class="sxs-lookup"><span data-stu-id="3a04e-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="3a04e-161">この例で、`await`キーワードは、前に適用されません`Groups.Add`グループのメンバーにメッセージがすぐに送信されないためです。</span><span class="sxs-lookup"><span data-stu-id="3a04e-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="3a04e-162">適用するグループのすべてのメンバーに、新しいメンバーを追加した後すぐにメッセージを送信する場合、`await`キーワードを非同期操作が完了したかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="3a04e-163">Azure table storage に格納するグループのメンバーシップ</span><span class="sxs-lookup"><span data-stu-id="3a04e-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="3a04e-164">Azure テーブル ストレージを使用して、グループとユーザーの情報を格納するは、データベースを使用してに似ています。</span><span class="sxs-lookup"><span data-stu-id="3a04e-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="3a04e-165">次の例では、ユーザー名とグループ名を格納するテーブル エンティティを示します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="3a04e-166">ハブで、ユーザーが接続するときに割り当てられているグループを取得します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="3a04e-167">再接続時にグループ メンバーシップの確認</span><span class="sxs-lookup"><span data-stu-id="3a04e-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="3a04e-168">既定では、SignalR 自動的に再割り当てをユーザー適切なグループに接続が削除され、接続がタイムアウトする前に再確立するなどの一時中断から再接続時にします。再接続時に、時に、ユーザーのグループの情報がトークンで渡され、サーバーでそのトークンを確認します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="3a04e-169">グループへのユーザーの再参加の検証プロセスについては、次を参照してください。[再接続時にグループの再参加](../security/introduction-to-security.md#rejoingroup)します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="3a04e-170">一般に、グループに再接続を自動的に再参加の既定の動作を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a04e-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="3a04e-171">SignalR のグループは、機密データへのアクセスを制限するためのセキュリティ メカニズムとして意図されていません。</span><span class="sxs-lookup"><span data-stu-id="3a04e-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="3a04e-172">ただし、アプリケーションは、再接続時に、ユーザーのグループ メンバーシップを再確認する必要がある場合、は、既定の動作を上書きできます。</span><span class="sxs-lookup"><span data-stu-id="3a04e-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="3a04e-173">既定の動作を変更することができます、負担、データベースを追加ため各再接続はなく、ユーザーが接続するときに、ユーザーのグループ メンバーシップを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a04e-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="3a04e-174">グループのメンバーシップを確認する必要がある場合は、再接続、次に示すように、割り当てられたグループの一覧を返す新しいハブ パイプライン モジュールを作成します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="3a04e-175">次に、以下の強調表示されている、ハブ パイプラインにそのモジュールを追加します。</span><span class="sxs-lookup"><span data-stu-id="3a04e-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
