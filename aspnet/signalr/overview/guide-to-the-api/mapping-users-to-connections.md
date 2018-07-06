---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: SignalR ユーザー接続をマッピング |Microsoft Docs
author: tfitzmac
description: このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。 Patrick Fletcher は、このトピックに記述できました。 ソフトウェアのバージョンがこのトピックで使用しています.
ms.author: aspnetcontent
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: bee743c5b201f4eef04cb80aa860ec67c4afe773
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840438"
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="29383-105">接続に SignalR ユーザーのマッピング</span><span class="sxs-lookup"><span data-stu-id="29383-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="29383-106">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="29383-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="29383-107">このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="29383-108">Patrick Fletcher は、このトピックに記述できました。</span><span class="sxs-lookup"><span data-stu-id="29383-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="29383-109">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="29383-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="29383-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="29383-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="29383-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="29383-111">.NET 4.5</span></span>
> - <span data-ttu-id="29383-112">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="29383-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="29383-113">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="29383-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="29383-114">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="29383-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="29383-115">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="29383-115">Questions and comments</span></span>
> 
> <span data-ttu-id="29383-116">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="29383-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="29383-117">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="29383-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="29383-118">はじめに</span><span class="sxs-lookup"><span data-stu-id="29383-118">Introduction</span></span>

<span data-ttu-id="29383-119">ハブに接続する各クライアントでは、一意の接続 id を渡します。この値を取得することができます、`Context.ConnectionId`ハブ コンテキストのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="29383-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="29383-120">アプリケーションにユーザーの接続 id を割り当てるし、そのマッピングを保持する場合、次のいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="29383-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="29383-121">ユーザーの ID プロバイダー (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="29383-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="29383-122">[メモリ内ストレージ](#inmemory)、ディクショナリなど</span><span class="sxs-lookup"><span data-stu-id="29383-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="29383-123">ユーザーごとに SignalR グループ</span><span class="sxs-lookup"><span data-stu-id="29383-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="29383-124">[永続的な外部の記憶域](#database)など、データベース テーブルまたは Azure テーブル ストレージ</span><span class="sxs-lookup"><span data-stu-id="29383-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="29383-125">このトピックでこれらの各実装に表示されます。</span><span class="sxs-lookup"><span data-stu-id="29383-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="29383-126">使用する、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`のメソッド、`Hub`ユーザー接続の状態を追跡するクラス。</span><span class="sxs-lookup"><span data-stu-id="29383-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="29383-127">アプリケーションに最適な方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="29383-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="29383-128">アプリケーションをホストする web サーバーの数。</span><span class="sxs-lookup"><span data-stu-id="29383-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="29383-129">かどうかは、現在接続しているユーザーの一覧を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29383-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="29383-130">かどうかは、アプリケーションまたはサーバーを再起動すると、グループとユーザーの情報を保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29383-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="29383-131">かどうか、外部のサーバーの呼び出しの待機時間が問題です。</span><span class="sxs-lookup"><span data-stu-id="29383-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="29383-132">どちらのアプローチがこれらの考慮事項の動作を次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="29383-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="29383-133">複数のサーバー</span><span class="sxs-lookup"><span data-stu-id="29383-133">More than one server</span></span> | <span data-ttu-id="29383-134">現在接続しているユーザーの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="29383-134">Get list of currently connected users</span></span> | <span data-ttu-id="29383-135">再起動後の情報を永続化します。</span><span class="sxs-lookup"><span data-stu-id="29383-135">Persist information after restarts</span></span> | <span data-ttu-id="29383-136">最適なパフォーマンス</span><span class="sxs-lookup"><span data-stu-id="29383-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="29383-137">ユーザー Id プロバイダー</span><span class="sxs-lookup"><span data-stu-id="29383-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="29383-138">メモリ内</span><span class="sxs-lookup"><span data-stu-id="29383-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="29383-139">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="29383-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="29383-140">恒久的な外部</span><span class="sxs-lookup"><span data-stu-id="29383-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="29383-141">IUserID プロバイダー</span><span class="sxs-lookup"><span data-stu-id="29383-141">IUserID provider</span></span>

<span data-ttu-id="29383-142">この機能により、ユーザー Id を指定するユーザー IUserIdProvider 新しいインターフェイスを使用して、IRequest に基づいています。</span><span class="sxs-lookup"><span data-stu-id="29383-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="29383-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="29383-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="29383-144">既定があります、ユーザーを使用する実装`IPrincipal.Identity.Name`としてユーザー名。</span><span class="sxs-lookup"><span data-stu-id="29383-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="29383-145">これを変更するには、実装を登録`IUserIdProvider`グローバルのホスト アプリケーションの起動時に使用します。</span><span class="sxs-lookup"><span data-stu-id="29383-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="29383-146">ハブ内でことができます、次の API を使用してこれらのユーザーにメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="29383-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="29383-147">**特定のユーザーにメッセージを送信します。**</span><span class="sxs-lookup"><span data-stu-id="29383-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="29383-148">メモリ内ストレージ</span><span class="sxs-lookup"><span data-stu-id="29383-148">In-memory storage</span></span>

<span data-ttu-id="29383-149">次の例では、メモリに格納されているディクショナリ内の接続とユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="29383-150">ディクショナリを使用して、`HashSet`接続 id を格納します。いつでもユーザーには、SignalR アプリケーションへの接続を 1 つ以上の可能性があります。</span><span class="sxs-lookup"><span data-stu-id="29383-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="29383-151">たとえば、複数のデバイスまたは 1 つ以上のブラウザー タブで接続されているユーザーは、1 つ以上の接続 id があります。</span><span class="sxs-lookup"><span data-stu-id="29383-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="29383-152">場合は、アプリケーションがシャット ダウン、すべての情報が失われたがする再作成ように、ユーザーは、その接続を再確立します。</span><span class="sxs-lookup"><span data-stu-id="29383-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="29383-153">メモリ内ストレージには、各サーバーが接続の別のコレクションをことになるため、環境に 1 つ以上の web サーバーが含まれている場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="29383-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="29383-154">最初の例では、接続に対するユーザーのマッピングを管理するクラスを示します。</span><span class="sxs-lookup"><span data-stu-id="29383-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="29383-155">HashSet のキーは、ユーザーの名前になります。</span><span class="sxs-lookup"><span data-stu-id="29383-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="29383-156">次の例では、ハブからの接続マッピング クラスを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="29383-157">変数名で、クラスのインスタンスが格納されている`_connections`します。</span><span class="sxs-lookup"><span data-stu-id="29383-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="29383-158">シングル ユーザー グループ</span><span class="sxs-lookup"><span data-stu-id="29383-158">Single-user groups</span></span>

<span data-ttu-id="29383-159">ユーザーごとにグループを作成し、そのユーザーだけに接続するときに、そのグループにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="29383-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="29383-160">各グループの名前は、ユーザーの名前です。</span><span class="sxs-lookup"><span data-stu-id="29383-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="29383-161">ユーザーが 1 つ以上の接続を持つ場合は、各接続の id がユーザーのグループに追加されます。</span><span class="sxs-lookup"><span data-stu-id="29383-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="29383-162">必要がありますいない手動で削除するユーザー、グループからユーザーが切断されたとき。</span><span class="sxs-lookup"><span data-stu-id="29383-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="29383-163">この操作は、SignalR フレームワークによって自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="29383-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="29383-164">次の例では、シングル ユーザー グループを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="29383-165">永続的な外部のストレージ</span><span class="sxs-lookup"><span data-stu-id="29383-165">Permanent, external storage</span></span>

<span data-ttu-id="29383-166">このトピックでは、接続情報を格納するため、データベースまたは Azure テーブル ストレージを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="29383-167">このアプローチは、各 web サーバーが、同じデータ リポジトリと対話できるため、複数の web サーバーがある場合は動作します。</span><span class="sxs-lookup"><span data-stu-id="29383-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="29383-168">Web サーバー アプリケーションが再起動されるかの処理を停止する場合、`OnDisconnected`メソッドは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="29383-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="29383-169">したがって、データ リポジトリが有効になっている接続 id のレコードにあることができます。</span><span class="sxs-lookup"><span data-stu-id="29383-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="29383-170">これらの孤立したレコードをクリーンアップするには、アプリケーションに関連する時間枠の外部で作成されたすべての接続を無効にします。</span><span class="sxs-lookup"><span data-stu-id="29383-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="29383-171">このセクションの例には、接続が作成された日時を追跡するための値が含まれているバック グラウンド プロセスとして実行するために古いレコードをクリーンアップする方法を表示しません。</span><span class="sxs-lookup"><span data-stu-id="29383-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="29383-172">データベース</span><span class="sxs-lookup"><span data-stu-id="29383-172">Database</span></span>

<span data-ttu-id="29383-173">次の例では、データベースに接続し、ユーザーの情報を保持する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="29383-174">任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="29383-175">これらのエンティティ モデルは、データベース テーブルとフィールドに対応します。</span><span class="sxs-lookup"><span data-stu-id="29383-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="29383-176">データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="29383-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="29383-177">最初の例では、多くの接続のエンティティに関連付けることができるユーザー エンティティを定義する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="29383-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="29383-178">次に、ハブから、以下に示すコードで各接続の状態を追跡できます。</span><span class="sxs-lookup"><span data-stu-id="29383-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="29383-179">Azure テーブル ストレージ</span><span class="sxs-lookup"><span data-stu-id="29383-179">Azure table storage</span></span>

<span data-ttu-id="29383-180">Azure テーブル ストレージの次の例では、データベースの例に似ています。</span><span class="sxs-lookup"><span data-stu-id="29383-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="29383-181">すべての Azure Table Storage サービスを開始する必要のある情報が含まれません。</span><span class="sxs-lookup"><span data-stu-id="29383-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="29383-182">詳しくは、次を参照してください。 [.NET からテーブル ストレージを使用する方法](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)します。</span><span class="sxs-lookup"><span data-stu-id="29383-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="29383-183">次の例では、接続情報を格納するためのテーブル エンティティを示します。</span><span class="sxs-lookup"><span data-stu-id="29383-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="29383-184">ユーザーの名前で、データをパーティション分割し、ユーザーでは、いつでも複数の接続ができるように、接続 id を使用して各エンティティを識別します。</span><span class="sxs-lookup"><span data-stu-id="29383-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="29383-185">ハブでは、各ユーザーの接続の状態を追跡します。</span><span class="sxs-lookup"><span data-stu-id="29383-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
