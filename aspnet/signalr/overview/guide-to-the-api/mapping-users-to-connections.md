---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "SignalR のユーザー接続にマップ |Microsoft ドキュメント"
author: tfitzmac
description: "このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。 このトピックの内容を書き込む Patrick Fletcher に役に立ちます。 ソフトウェアのバージョンがこのトピックで使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a>接続に SignalR ユーザーのマッピング
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、ユーザーとの接続に関する情報を保持する方法を示します。
> 
> このトピックの内容を書き込む Patrick Fletcher に役に立ちます。
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されているソフトウェア バージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="introduction"></a>はじめに

各クライアントがハブに接続するには、一意の接続 id が渡されます。この値を取得することができます、`Context.ConnectionId`ハブ コンテキストのプロパティです。 アプリケーションは、ユーザーの接続 id を割り当てるし、そのマッピングを保持する必要がある場合、は、次のいずれかを使用できます。

- [ユーザーの ID プロバイダー (SignalR 2)](#IUserIdProvider)
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
| ユーザー Id プロバイダー | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| メモリ内 |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| シングル ユーザー グループ | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| 永続的である、外部 | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>IUserID プロバイダー

この機能により、ユーザー Id を指定するユーザー IUserIdProvider の新しいインターフェイスを使用して、IRequest に基づいています。

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

既定がありますをユーザーを使用して実装`IPrincipal.Identity.Name`ユーザー名とします。 これを変更するには、実装を登録`IUserIdProvider`グローバル ホスト、アプリケーションの起動時に。

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

ハブ内ことができます次の API を使用してこれらのユーザーにメッセージを送信します。

**特定のユーザーにメッセージを送信します。**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>メモリ内ストレージ

次の例では、メモリに格納されているディクショナリ内の接続とユーザーの情報を保持する方法を示します。 ディクショナリを使用して、`HashSet`接続 id を格納します。いつでも、ユーザーには、SignalR アプリケーションへの接続を複数の可能性があります。 たとえば、複数のデバイスまたは複数のブラウザー タブで接続しているユーザーは、1 つ以上の接続 id があります。

アプリケーションはシャット ダウンした場合のすべての情報が失われますそれが再作成されます。 ユーザーからの接続を再確立するとします。 メモリ内ストレージでは、各サーバーが接続の個別のコレクションがあるため、環境に 1 つ以上の web サーバーが含まれる場合は機能しません。

最初の例では、接続するユーザーのマッピングを管理するクラスを示します。 HashSet のキーは、ユーザーの名前になります。

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

次の例では、ハブからの接続のマッピングのクラスを使用する方法を示します。 クラスのインスタンスが変数名に格納されている`_connections`です。

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>シングル ユーザー グループ

各ユーザーのグループを作成し、そのユーザーのみと通信するときに、そのグループにメッセージを送信できます。 各グループの名前は、ユーザーの名前です。 複数の接続をユーザーには、各接続の id が、ユーザーのグループに追加されます。

手動で削除しないでユーザー グループからユーザーを切断するとします。 この操作は、SignalR フレームワークによって自動的に実行します。

次の例では、シングル ユーザー グループを実装する方法を示します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>永続的な外部のストレージ

このトピックでは、接続情報を格納するため、データベースまたは Azure テーブル ストレージを使用する方法を示します。 この方法は、各 web サーバーが同じデータ リポジトリにやり取りできるため、複数の web サーバーがある場合は動作します。 Web サーバーか、アプリケーションの再起動の処理を停止する場合、`OnDisconnected`メソッドは呼び出されません。 したがって、データ リポジトリが無効になって接続 id のレコードを持つことができます。 孤立したこれらのレコードをクリーンアップするには、アプリケーションに関連する指定した期間外で作成されたすべての接続を無効にすることです。 このセクションの例では、接続が作成されたときに追跡するための値を指定するバック グラウンド プロセスとして実行するために、古いレコードをクリーンアップする方法を表示しないが、します。

### <a name="database"></a>データベース

次の例では、データベースに接続し、ユーザーの情報を保持する方法を示します。 任意のデータ アクセス テクノロジを使用することができます。ただし、次の例では、Entity Framework を使用してモデルを定義する方法を示します。 これらのエンティティ モデルは、データベースのテーブルとフィールドに対応します。 データ構造は、アプリケーションの要件に応じて大きく異なる可能性があります。

最初の例では、多くの接続のエンティティと関連付けることができるユーザー エンティティを定義する方法を示します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

次に、ハブから、次のコードで各接続の状態を追跡できます。

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure テーブル ストレージ

次の Azure テーブル ストレージの例では、データベースの例に似ています。 すべての情報は Azure テーブル ストレージ サービスを開始する必要がありますを含みません。 詳細については、次を参照してください。 [.NET からテーブル ストレージの使用方法](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)です。

次の例では、接続情報を格納するためのテーブル エンティティを示します。 ユーザー名でデータをパーティション分割し、ユーザーはいつでも複数の接続を持つことができますので、接続 id を使用して各エンティティを識別します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

ハブでは、各ユーザーの接続の状態を追跡します。

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
