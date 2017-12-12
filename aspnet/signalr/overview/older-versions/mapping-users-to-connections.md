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
ms.openlocfilehash: 561c5739c4e8465efeb4b5d1eaf8a196dab8673f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>SignalR の接続 に SignalR ユーザーをマッピング 1.x
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。


## <a name="introduction"></a>はじめに

各クライアントがハブに接続するには、一意の接続 id が渡されます。この値を取得することができます、`Context.ConnectionId`ハブ コンテキストのプロパティです。 アプリケーションは、ユーザーの接続 id を割り当てるし、そのマッピングを保持する必要がある場合、は、次のいずれかを使用できます。

- [メモリ内ストレージ](#inmemory)、ディクショナリなど
- [各ユーザーの SignalR グループ](#groups)
- [永続的な外部の記憶域](#database)など、データベース テーブルまたは Azure テーブル ストレージ

これらの各実装は、このトピックに表示されます。 使用する、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`のメソッド、`Hub`ユーザー接続の状態を追跡するクラス。

アプリケーションの最善の方法によって異なります。

- アプリケーションをホストする web サーバーの数。
- かどうかは、現在接続しているユーザーの一覧を取得する必要があります。
- かどうかは、アプリケーションまたはサーバーが再起動されたときに、グループとユーザーの情報を保存する必要があります。
- かどうか、外部のサーバーの呼び出しの待機時間は、問題です。

どの方法はこれらの考慮事項を次の表に示します。

|  | 複数のサーバー | 現在接続しているユーザーの一覧を取得します。 | 再起動した後の情報を永続化します。 | 最適なパフォーマンス |
| --- | --- | --- | --- | --- |
| メモリ内 |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| シングル ユーザー グループ | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| 永続的である、外部 | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>メモリ内ストレージ

次の例では、メモリに格納されているディクショナリ内の接続とユーザーの情報を保持する方法を示します。 ディクショナリを使用して、`HashSet`接続 id を格納します。いつでも、ユーザーには、SignalR アプリケーションへの接続を複数の可能性があります。 たとえば、複数のデバイスまたは複数のブラウザー タブで接続しているユーザーは、1 つ以上の接続 id があります。

アプリケーションはシャット ダウンした場合のすべての情報が失われますそれが再作成されます。 ユーザーからの接続を再確立するとします。 メモリ内ストレージでは、各サーバーが接続の個別のコレクションがあるため、環境に 1 つ以上の web サーバーが含まれる場合は機能しません。

最初の例では、接続するユーザーのマッピングを管理するクラスを示します。 HashSet のキーは、ユーザーの名前になります。

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

次の例では、ハブからの接続のマッピングのクラスを使用する方法を示します。 クラスのインスタンスが変数名に格納されている`_connections`です。

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>シングル ユーザー グループ

各ユーザーのグループを作成し、そのユーザーのみと通信するときに、そのグループにメッセージを送信できます。 各グループの名前は、ユーザーの名前です。 複数の接続をユーザーには、各接続の id が、ユーザーのグループに追加されます。

手動で削除しないでユーザー グループからユーザーを切断するとします。 この操作は、SignalR フレームワークによって自動的に実行します。

次の例では、シングル ユーザー グループを実装する方法を示します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>永続的な外部のストレージ

このトピックでは、接続情報を格納するため、データベースまたは Azure テーブル ストレージを使用する方法を示します。 この方法は、各 web サーバーが同じデータ リポジトリにやり取りできるため、複数の web サーバーがある場合は動作します。 Web サーバーか、アプリケーションの再起動の処理を停止する場合、`OnDisconnected`メソッドは呼び出されません。 したがって、データ リポジトリが無効になって接続 id のレコードを持つことができます。 孤立したこれらのレコードをクリーンアップするには、アプリケーションに関連する指定した期間外で作成されたすべての接続を無効にすることです。 このセクションの例では、接続が作成されたときに追跡するための値を指定するバック グラウンド プロセスとして実行するために、古いレコードをクリーンアップする方法を表示しないが、します。

### <a name="database"></a>データベース

次の例では、データベースに接続し、ユーザーの情報を保持する方法を示します。 任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。 これらのエンティティ モデルは、データベースのテーブルとフィールドに対応します。 データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。

最初の例では、多くの接続のエンティティと関連付けることができるユーザー エンティティを定義する方法を示します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

次に、ハブから、次のコードで各接続の状態を追跡できます。

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Azure テーブル ストレージ

次の Azure テーブル ストレージの例では、データベースの例に似ています。 すべての情報は Azure テーブル ストレージ サービスを開始する必要がありますを含みません。 詳細については、次を参照してください。 [.NET からテーブル ストレージの使用方法](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/)です。

次の例では、接続情報を格納するためのテーブル エンティティを示します。 ユーザー名でデータをパーティション分割し、ユーザーはいつでも複数の接続を持つことができますので、接続 id を使用して各エンティティを識別します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

ハブでは、各ユーザーの接続の状態を追跡します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
