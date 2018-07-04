---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: SignalR ユーザー接続をマッピング |Microsoft Docs
author: tfitzmac
description: このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。 Patrick Fletcher は、このトピックに記述できました。 ソフトウェアのバージョンがこのトピックで使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: bd7c0cd9a645ab5b65c5c1446b51ea1646e43799
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391533"
---
<a name="mapping-signalr-users-to-connections"></a>接続に SignalR ユーザーのマッピング
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。
> 
> Patrick Fletcher は、このトピックに記述できました。
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 2 のバージョン
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。


## <a name="introduction"></a>はじめに

ハブに接続する各クライアントでは、一意の接続 id を渡します。この値を取得することができます、`Context.ConnectionId`ハブ コンテキストのプロパティ。 アプリケーションにユーザーの接続 id を割り当てるし、そのマッピングを保持する場合、次のいずれかを使用できます。

- [ユーザーの ID プロバイダー (SignalR 2)](#IUserIdProvider)
- [メモリ内ストレージ](#inmemory)、ディクショナリなど
- [ユーザーごとに SignalR グループ](#groups)
- [永続的な外部の記憶域](#database)など、データベース テーブルまたは Azure テーブル ストレージ

このトピックでこれらの各実装に表示されます。 使用する、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`のメソッド、`Hub`ユーザー接続の状態を追跡するクラス。

アプリケーションに最適な方法によって異なります。

- アプリケーションをホストする web サーバーの数。
- かどうかは、現在接続しているユーザーの一覧を取得する必要があります。
- かどうかは、アプリケーションまたはサーバーを再起動すると、グループとユーザーの情報を保持する必要があります。
- かどうか、外部のサーバーの呼び出しの待機時間が問題です。

どちらのアプローチがこれらの考慮事項の動作を次の表に示します。

|  | 複数のサーバー | 現在接続しているユーザーの一覧を取得します。 | 再起動後の情報を永続化します。 | 最適なパフォーマンス |
| --- | --- | --- | --- | --- |
| ユーザー Id プロバイダー | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| メモリ内 |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| シングル ユーザー グループ | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| 恒久的な外部 | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>IUserID プロバイダー

この機能により、ユーザー Id を指定するユーザー IUserIdProvider 新しいインターフェイスを使用して、IRequest に基づいています。

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

既定があります、ユーザーを使用する実装`IPrincipal.Identity.Name`としてユーザー名。 これを変更するには、実装を登録`IUserIdProvider`グローバルのホスト アプリケーションの起動時に使用します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

ハブ内でことができます、次の API を使用してこれらのユーザーにメッセージを送信します。

**特定のユーザーにメッセージを送信します。**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>メモリ内ストレージ

次の例では、メモリに格納されているディクショナリ内の接続とユーザーの情報を保持する方法を示します。 ディクショナリを使用して、`HashSet`接続 id を格納します。いつでもユーザーには、SignalR アプリケーションへの接続を 1 つ以上の可能性があります。 たとえば、複数のデバイスまたは 1 つ以上のブラウザー タブで接続されているユーザーは、1 つ以上の接続 id があります。

場合は、アプリケーションがシャット ダウン、すべての情報が失われたがする再作成ように、ユーザーは、その接続を再確立します。 メモリ内ストレージには、各サーバーが接続の別のコレクションをことになるため、環境に 1 つ以上の web サーバーが含まれている場合は機能しません。

最初の例では、接続に対するユーザーのマッピングを管理するクラスを示します。 HashSet のキーは、ユーザーの名前になります。

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

次の例では、ハブからの接続マッピング クラスを使用する方法を示します。 変数名で、クラスのインスタンスが格納されている`_connections`します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>シングル ユーザー グループ

ユーザーごとにグループを作成し、そのユーザーだけに接続するときに、そのグループにメッセージを送信できます。 各グループの名前は、ユーザーの名前です。 ユーザーが 1 つ以上の接続を持つ場合は、各接続の id がユーザーのグループに追加されます。

必要がありますいない手動で削除するユーザー、グループからユーザーが切断されたとき。 この操作は、SignalR フレームワークによって自動的に実行されます。

次の例では、シングル ユーザー グループを実装する方法を示します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>永続的な外部のストレージ

このトピックでは、接続情報を格納するため、データベースまたは Azure テーブル ストレージを使用する方法を示します。 このアプローチは、各 web サーバーが、同じデータ リポジトリと対話できるため、複数の web サーバーがある場合は動作します。 Web サーバー アプリケーションが再起動されるかの処理を停止する場合、`OnDisconnected`メソッドは呼び出されません。 したがって、データ リポジトリが有効になっている接続 id のレコードにあることができます。 これらの孤立したレコードをクリーンアップするには、アプリケーションに関連する時間枠の外部で作成されたすべての接続を無効にします。 このセクションの例には、接続が作成された日時を追跡するための値が含まれているバック グラウンド プロセスとして実行するために古いレコードをクリーンアップする方法を表示しません。

### <a name="database"></a>データベース

次の例では、データベースに接続し、ユーザーの情報を保持する方法を示します。 任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。 これらのエンティティ モデルは、データベース テーブルとフィールドに対応します。 データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。

最初の例では、多くの接続のエンティティに関連付けることができるユーザー エンティティを定義する方法を示します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

次に、ハブから、以下に示すコードで各接続の状態を追跡できます。

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure テーブル ストレージ

Azure テーブル ストレージの次の例では、データベースの例に似ています。 すべての Azure Table Storage サービスを開始する必要のある情報が含まれません。 詳しくは、次を参照してください。 [.NET からテーブル ストレージを使用する方法](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)します。

次の例では、接続情報を格納するためのテーブル エンティティを示します。 ユーザーの名前で、データをパーティション分割し、ユーザーでは、いつでも複数の接続ができるように、接続 id を使用して各エンティティを識別します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

ハブでは、各ユーザーの接続の状態を追跡します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
