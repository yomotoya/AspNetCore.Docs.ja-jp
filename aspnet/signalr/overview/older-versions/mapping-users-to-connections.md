---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "SignalR の接続 に SignalR ユーザーをマッピング 1.x |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="50a4d-103">SignalR の接続 に SignalR ユーザーをマッピング 1.x</span><span class="sxs-lookup"><span data-stu-id="50a4d-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="50a4d-104">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="50a4d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="50a4d-105">このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="50a4d-106">はじめに</span><span class="sxs-lookup"><span data-stu-id="50a4d-106">Introduction</span></span>

<span data-ttu-id="50a4d-107">各クライアントがハブに接続するには、一意の接続 id が渡されます。この値を取得することができます、`Context.ConnectionId`ハブ コンテキストのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="50a4d-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="50a4d-108">アプリケーションは、ユーザーの接続 id を割り当てるし、そのマッピングを保持する必要がある場合、は、次のいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="50a4d-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="50a4d-109">[メモリ内ストレージ](#inmemory)、ディクショナリなど</span><span class="sxs-lookup"><span data-stu-id="50a4d-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="50a4d-110">各ユーザーの SignalR グループ</span><span class="sxs-lookup"><span data-stu-id="50a4d-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="50a4d-111">[永続的な外部の記憶域](#database)など、データベース テーブルまたは Azure テーブル ストレージ</span><span class="sxs-lookup"><span data-stu-id="50a4d-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="50a4d-112">これらの各実装は、このトピックに表示されます。</span><span class="sxs-lookup"><span data-stu-id="50a4d-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="50a4d-113">使用する、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`のメソッド、`Hub`ユーザー接続の状態を追跡するクラス。</span><span class="sxs-lookup"><span data-stu-id="50a4d-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="50a4d-114">アプリケーションの最善の方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="50a4d-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="50a4d-115">アプリケーションをホストする web サーバーの数。</span><span class="sxs-lookup"><span data-stu-id="50a4d-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="50a4d-116">かどうかは、現在接続しているユーザーの一覧を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="50a4d-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="50a4d-117">かどうかは、アプリケーションまたはサーバーが再起動されたときに、グループとユーザーの情報を保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="50a4d-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="50a4d-118">かどうか、外部のサーバーの呼び出しの待機時間は、問題です。</span><span class="sxs-lookup"><span data-stu-id="50a4d-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="50a4d-119">どの方法はこれらの考慮事項を次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="50a4d-120">複数のサーバー</span><span class="sxs-lookup"><span data-stu-id="50a4d-120">More than one server</span></span> | <span data-ttu-id="50a4d-121">現在接続しているユーザーの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-121">Get list of currently connected users</span></span> | <span data-ttu-id="50a4d-122">再起動した後の情報を永続化します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-122">Persist information after restarts</span></span> | <span data-ttu-id="50a4d-123">最適なパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="50a4d-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="50a4d-124">メモリ内</span><span class="sxs-lookup"><span data-stu-id="50a4d-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="50a4d-125">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="50a4d-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="50a4d-126">永続的である、外部</span><span class="sxs-lookup"><span data-stu-id="50a4d-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="50a4d-127">メモリ内ストレージ</span><span class="sxs-lookup"><span data-stu-id="50a4d-127">In-memory storage</span></span>

<span data-ttu-id="50a4d-128">次の例では、メモリに格納されているディクショナリ内の接続とユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="50a4d-129">ディクショナリを使用して、`HashSet`接続 id を格納します。いつでも、ユーザーには、SignalR アプリケーションへの接続を複数の可能性があります。</span><span class="sxs-lookup"><span data-stu-id="50a4d-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="50a4d-130">たとえば、複数のデバイスまたは複数のブラウザー タブで接続しているユーザーは、1 つ以上の接続 id があります。</span><span class="sxs-lookup"><span data-stu-id="50a4d-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="50a4d-131">アプリケーションはシャット ダウンした場合のすべての情報が失われますそれが再作成されます。 ユーザーからの接続を再確立するとします。</span><span class="sxs-lookup"><span data-stu-id="50a4d-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="50a4d-132">メモリ内ストレージでは、各サーバーが接続の個別のコレクションがあるため、環境に 1 つ以上の web サーバーが含まれる場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="50a4d-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="50a4d-133">最初の例では、接続するユーザーのマッピングを管理するクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="50a4d-134">HashSet のキーは、ユーザーの名前になります。</span><span class="sxs-lookup"><span data-stu-id="50a4d-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="50a4d-135">次の例では、ハブからの接続のマッピングのクラスを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="50a4d-136">クラスのインスタンスが変数名に格納されている`_connections`です。</span><span class="sxs-lookup"><span data-stu-id="50a4d-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="50a4d-137">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="50a4d-137">Single-user groups</span></span>

<span data-ttu-id="50a4d-138">各ユーザーのグループを作成し、そのユーザーのみと通信するときに、そのグループにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="50a4d-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="50a4d-139">各グループの名前は、ユーザーの名前です。</span><span class="sxs-lookup"><span data-stu-id="50a4d-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="50a4d-140">複数の接続をユーザーには、各接続の id が、ユーザーのグループに追加されます。</span><span class="sxs-lookup"><span data-stu-id="50a4d-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="50a4d-141">手動で削除しないでユーザー グループからユーザーを切断するとします。</span><span class="sxs-lookup"><span data-stu-id="50a4d-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="50a4d-142">この操作は、SignalR フレームワークによって自動的に実行します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="50a4d-143">次の例では、シングル ユーザー グループを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="50a4d-144">永続的な外部のストレージ</span><span class="sxs-lookup"><span data-stu-id="50a4d-144">Permanent, external storage</span></span>

<span data-ttu-id="50a4d-145">このトピックでは、接続情報を格納するため、データベースまたは Azure テーブル ストレージを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="50a4d-146">この方法は、各 web サーバーが同じデータ リポジトリにやり取りできるため、複数の web サーバーがある場合は動作します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="50a4d-147">Web サーバーか、アプリケーションの再起動の処理を停止する場合、`OnDisconnected`メソッドは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="50a4d-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="50a4d-148">したがって、データ リポジトリが無効になって接続 id のレコードを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="50a4d-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="50a4d-149">孤立したこれらのレコードをクリーンアップするには、アプリケーションに関連する指定した期間外で作成されたすべての接続を無効にすることです。</span><span class="sxs-lookup"><span data-stu-id="50a4d-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="50a4d-150">このセクションの例では、接続が作成されたときに追跡するための値を指定するバック グラウンド プロセスとして実行するために、古いレコードをクリーンアップする方法を表示しないが、します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="50a4d-151">データベース</span><span class="sxs-lookup"><span data-stu-id="50a4d-151">Database</span></span>

<span data-ttu-id="50a4d-152">次の例では、データベースに接続し、ユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="50a4d-153">任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="50a4d-154">これらのエンティティ モデルは、データベースのテーブルとフィールドに対応します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="50a4d-155">データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="50a4d-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="50a4d-156">最初の例では、多くの接続のエンティティと関連付けることができるユーザー エンティティを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="50a4d-157">次に、ハブから、次のコードで各接続の状態を追跡できます。</span><span class="sxs-lookup"><span data-stu-id="50a4d-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="50a4d-158">Azure テーブル ストレージ</span><span class="sxs-lookup"><span data-stu-id="50a4d-158">Azure table storage</span></span>

<span data-ttu-id="50a4d-159">次の Azure テーブル ストレージの例では、データベースの例に似ています。</span><span class="sxs-lookup"><span data-stu-id="50a4d-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="50a4d-160">すべての情報は Azure テーブル ストレージ サービスを開始する必要がありますを含みません。</span><span class="sxs-lookup"><span data-stu-id="50a4d-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="50a4d-161">詳細については、次を参照してください。 [.NET からテーブル ストレージの使用方法](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)です。</span><span class="sxs-lookup"><span data-stu-id="50a4d-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="50a4d-162">次の例では、接続情報を格納するためのテーブル エンティティを示します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="50a4d-163">ユーザー名でデータをパーティション分割し、ユーザーはいつでも複数の接続を持つことができますので、接続 id を使用して各エンティティを識別します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="50a4d-164">ハブでは、各ユーザーの接続の状態を追跡します。</span><span class="sxs-lookup"><span data-stu-id="50a4d-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
