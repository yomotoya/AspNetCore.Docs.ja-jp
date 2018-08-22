---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: SignalR の接続に SignalR ユーザーをマッピング 1.x |Microsoft Docs
author: pfletcher
description: このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。
ms.author: riande
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 3ce651fa523743da536a9b73bb9bb8e21d8845c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826586"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="e8488-103">SignalR の接続に SignalR ユーザーをマッピング 1.x</span><span class="sxs-lookup"><span data-stu-id="e8488-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="e8488-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e8488-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e8488-105">このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="e8488-106">はじめに</span><span class="sxs-lookup"><span data-stu-id="e8488-106">Introduction</span></span>

<span data-ttu-id="e8488-107">ハブに接続する各クライアントでは、一意の接続 id を渡します。この値を取得することができます、`Context.ConnectionId`ハブ コンテキストのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="e8488-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="e8488-108">アプリケーションにユーザーの接続 id を割り当てるし、そのマッピングを保持する場合、次のいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e8488-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="e8488-109">[メモリ内ストレージ](#inmemory)、ディクショナリなど</span><span class="sxs-lookup"><span data-stu-id="e8488-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="e8488-110">ユーザーごとに SignalR グループ</span><span class="sxs-lookup"><span data-stu-id="e8488-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="e8488-111">[永続的な外部の記憶域](#database)など、データベース テーブルまたは Azure テーブル ストレージ</span><span class="sxs-lookup"><span data-stu-id="e8488-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="e8488-112">このトピックでこれらの各実装に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e8488-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="e8488-113">使用する、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`のメソッド、`Hub`ユーザー接続の状態を追跡するクラス。</span><span class="sxs-lookup"><span data-stu-id="e8488-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="e8488-114">アプリケーションに最適な方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="e8488-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="e8488-115">アプリケーションをホストする web サーバーの数。</span><span class="sxs-lookup"><span data-stu-id="e8488-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="e8488-116">かどうかは、現在接続しているユーザーの一覧を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8488-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="e8488-117">かどうかは、アプリケーションまたはサーバーを再起動すると、グループとユーザーの情報を保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8488-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="e8488-118">かどうか、外部のサーバーの呼び出しの待機時間が問題です。</span><span class="sxs-lookup"><span data-stu-id="e8488-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="e8488-119">どちらのアプローチがこれらの考慮事項の動作を次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="e8488-120">複数のサーバー</span><span class="sxs-lookup"><span data-stu-id="e8488-120">More than one server</span></span> | <span data-ttu-id="e8488-121">現在接続しているユーザーの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="e8488-121">Get list of currently connected users</span></span> | <span data-ttu-id="e8488-122">再起動後の情報を永続化します。</span><span class="sxs-lookup"><span data-stu-id="e8488-122">Persist information after restarts</span></span> | <span data-ttu-id="e8488-123">最適なパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="e8488-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="e8488-124">メモリ内</span><span class="sxs-lookup"><span data-stu-id="e8488-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="e8488-125">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="e8488-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="e8488-126">恒久的な外部</span><span class="sxs-lookup"><span data-stu-id="e8488-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="e8488-127">メモリ内ストレージ</span><span class="sxs-lookup"><span data-stu-id="e8488-127">In-memory storage</span></span>

<span data-ttu-id="e8488-128">次の例では、メモリに格納されているディクショナリ内の接続とユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="e8488-129">ディクショナリを使用して、`HashSet`接続 id を格納します。いつでもユーザーには、SignalR アプリケーションへの接続を 1 つ以上の可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e8488-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="e8488-130">たとえば、複数のデバイスまたは 1 つ以上のブラウザー タブで接続されているユーザーは、1 つ以上の接続 id があります。</span><span class="sxs-lookup"><span data-stu-id="e8488-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="e8488-131">場合は、アプリケーションがシャット ダウン、すべての情報が失われたがする再作成ように、ユーザーは、その接続を再確立します。</span><span class="sxs-lookup"><span data-stu-id="e8488-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="e8488-132">メモリ内ストレージには、各サーバーが接続の別のコレクションをことになるため、環境に 1 つ以上の web サーバーが含まれている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="e8488-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="e8488-133">最初の例では、接続に対するユーザーのマッピングを管理するクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="e8488-134">HashSet のキーは、ユーザーの名前になります。</span><span class="sxs-lookup"><span data-stu-id="e8488-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="e8488-135">次の例では、ハブからの接続マッピング クラスを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="e8488-136">変数名で、クラスのインスタンスが格納されている`_connections`します。</span><span class="sxs-lookup"><span data-stu-id="e8488-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="e8488-137">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="e8488-137">Single-user groups</span></span>

<span data-ttu-id="e8488-138">ユーザーごとにグループを作成し、そのユーザーだけに接続するときに、そのグループにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="e8488-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="e8488-139">各グループの名前は、ユーザーの名前です。</span><span class="sxs-lookup"><span data-stu-id="e8488-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="e8488-140">ユーザーが 1 つ以上の接続を持つ場合は、各接続の id がユーザーのグループに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e8488-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="e8488-141">必要がありますいない手動で削除するユーザー、グループからユーザーが切断されたとき。</span><span class="sxs-lookup"><span data-stu-id="e8488-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="e8488-142">この操作は、SignalR フレームワークによって自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="e8488-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="e8488-143">次の例では、シングル ユーザー グループを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="e8488-144">永続的な外部のストレージ</span><span class="sxs-lookup"><span data-stu-id="e8488-144">Permanent, external storage</span></span>

<span data-ttu-id="e8488-145">このトピックでは、接続情報を格納するため、データベースまたは Azure テーブル ストレージを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="e8488-146">このアプローチは、各 web サーバーが、同じデータ リポジトリと対話できるため、複数の web サーバーがある場合は動作します。</span><span class="sxs-lookup"><span data-stu-id="e8488-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="e8488-147">Web サーバー アプリケーションが再起動されるかの処理を停止する場合、`OnDisconnected`メソッドは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="e8488-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="e8488-148">したがって、データ リポジトリが有効になっている接続 id のレコードにあることができます。</span><span class="sxs-lookup"><span data-stu-id="e8488-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="e8488-149">これらの孤立したレコードをクリーンアップするには、アプリケーションに関連する時間枠の外部で作成されたすべての接続を無効にします。</span><span class="sxs-lookup"><span data-stu-id="e8488-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="e8488-150">このセクションの例には、接続が作成された日時を追跡するための値が含まれているバック グラウンド プロセスとして実行するために古いレコードをクリーンアップする方法を表示しません。</span><span class="sxs-lookup"><span data-stu-id="e8488-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="e8488-151">データベース</span><span class="sxs-lookup"><span data-stu-id="e8488-151">Database</span></span>

<span data-ttu-id="e8488-152">次の例では、データベースに接続し、ユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="e8488-153">任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="e8488-154">これらのエンティティ モデルは、データベース テーブルとフィールドに対応します。</span><span class="sxs-lookup"><span data-stu-id="e8488-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="e8488-155">データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e8488-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="e8488-156">最初の例では、多くの接続のエンティティに関連付けることができるユーザー エンティティを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="e8488-157">次に、ハブから、以下に示すコードで各接続の状態を追跡できます。</span><span class="sxs-lookup"><span data-stu-id="e8488-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="e8488-158">Azure テーブル ストレージ</span><span class="sxs-lookup"><span data-stu-id="e8488-158">Azure table storage</span></span>

<span data-ttu-id="e8488-159">Azure テーブル ストレージの次の例では、データベースの例に似ています。</span><span class="sxs-lookup"><span data-stu-id="e8488-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="e8488-160">すべての Azure Table Storage サービスを開始する必要のある情報が含まれません。</span><span class="sxs-lookup"><span data-stu-id="e8488-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="e8488-161">詳しくは、次を参照してください。 [.NET からテーブル ストレージを使用する方法](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)します。</span><span class="sxs-lookup"><span data-stu-id="e8488-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="e8488-162">次の例では、接続情報を格納するためのテーブル エンティティを示します。</span><span class="sxs-lookup"><span data-stu-id="e8488-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="e8488-163">ユーザーの名前で、データをパーティション分割し、ユーザーでは、いつでも複数の接続ができるように、接続 id を使用して各エンティティを識別します。</span><span class="sxs-lookup"><span data-stu-id="e8488-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="e8488-164">ハブでは、各ユーザーの接続の状態を追跡します。</span><span class="sxs-lookup"><span data-stu-id="e8488-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
