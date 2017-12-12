---
uid: signalr/overview/getting-started/introduction-to-signalr
title: "SignalR の概要 |Microsoft ドキュメント"
author: pfletcher
description: "この記事は、SignalR は、作成するため、デザインされたソリューションの一部について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 27150d314b6861f1098e6ef4a7de94e7b371a78e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-signalr"></a>SignalR の概要
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> この記事は、SignalR は、作成するため、デザインされたソリューションの一部について説明します。 
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="what-is-signalr"></a>SignalR とは何ですか。

ASP.NET SignalR は、ASP.NET 開発者向けのアプリケーションにリアルタイム web 機能を追加するプロセスを簡略化するライブラリです。 リアルタイム web 機能とは、サーバーによって新しいデータを要求するクライアントを待機するのではなく、サーバー コード プッシュのように、使用可能になったら、接続クライアントに対して瞬時にコンテンツを保持する機能です。

SignalR は ASP.NET アプリケーションに何らかの「リアルタイム」web 機能を追加するためにことができます。 チャットは例として使用される多くの場合中よりもずっとを行うことができます。 いつでも、ユーザーに、新しいデータを表示する web ページを更新するか、ページを実装[ポーリング時間の長い](http://en.wikipedia.org/wiki/Push_technology#Long_polling)新しいデータを取得するが、候補 SignalR を使用するためです。 例としては、ダッシュ ボードとアプリケーションの監視、共同作業などのアプリケーション (同時ドキュメントの編集)、ジョブの進行状況の更新プログラム、およびリアルタイム フォーム。

SignalR では、完全に新しい種類の web アプリケーションをサーバーからの頻度の高い更新プログラムを必要とすることもできますリアルタイム ゲームなどです。 これの優れた例については、 [ShootR ゲームです。](http://shootr.signalr.net/)

SignalR には、サーバー側の .NET コードからブラウザー (およびその他のクライアント プラットフォーム) クライアントでの JavaScript 関数を呼び出してサーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API が用意されています。 SignalR には、接続の管理の API にはもが含まれています (接続し、切断イベントなど) との接続をグループ化します。

![SignalR でメソッドを呼び出す](introduction-to-signalr/_static/image1.png)

SignalR 接続の管理を自動的に処理し、ブロードキャスト メッセージをすべて接続されているクライアントに同時に、チャット ルームのようにできます。 特定のクライアントにメッセージを送信することもできます。 クライアントとサーバー間の接続は、各通信の再確立される従来の HTTP 接続とは異なり、永続的です。

SignalR をサーバー コードを呼び出すクライアント コードを使用して一般的な要求/応答モデルではなく、リモート プロシージャ コール (RPC)、web で現在のブラウザーで、「サーバー プッシュ」機能がサポートしています。

SignalR アプリケーションを Service Bus、SQL Server を使用してクライアントの数千にスケール アウトできますまたは[Redis](http://redis.io)です。

SignalR はオープン ソース、経由でアクセスできる[GitHub](https://github.com/signalr)です。

## <a name="signalr-and-websocket"></a>SignalR と WebSocket

SignalR では、使用可能な場合、新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。 WebSocket を直接使用して、多数の追加の機能を実装する必要がありますが、既に SignalR 手段を使用して、アプリケーションを確実に記述することもできますが完了するのです。 最も重要なは、つまり、活用するために WebSocket 古いクライアントの個別のコード パスの作成について心配することがなく、アプリケーションをコーディングすることができます。 SignalR も明らかにならないようにする SignalR は引き続き、基になるトランスポートでの変更をサポートするために更新する、アプリケーションが WebSocket の異なるバージョン間での一貫性のあるインターフェイスを提供するために、WebSocket への更新について心配する必要がなくなります。

単独で WebSocket を使用してソリューションを作成することが確かに、SignalR はすべての他のトランスポートと WebSocket 実装への更新プログラム、アプリケーションへのフォールバックなどを自分で作成する必要があります機能を提供します。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>トランスポートとフォールバック

SignalR は、クライアントとサーバー間のリアルタイムの作業を行うために必要なトランスポートの中に、抽象です。 SignalR 接続では、HTTP、として起動し、使用可能になる場合、WebSocket 接続には、昇格します。 WebSocket は、SignalR の理想的なトランスポートがサーバーのメモリの最も効率的に使用する、最短の待機時間があり、基になる最もなどの機能 (全二重通信クライアントとサーバー間)、いますが、最も厳しい要件: WebSocket が Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。 これらの要件が満たされず、SignalR はその接続を確立する他のトランスポートを使用しようとします。

### <a name="html-5-transports"></a>HTML 5 トランスポートします。

これらのトランスポートのサポートによって異なります[HTML 5](http://en.wikipedia.org/wiki/HTML5)です。 クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。

- **WebSocket** (場合、サーバーとブラウザーの両方は、Websocket をサポートすることを示します)。 WebSocket は、クライアントとサーバー間の場合は true。 永続的で双方向接続を確立するだけのトランスポートです。 ただし、WebSocket も最も厳格な要件です。Microsoft Internet Explorer、Google Chrome、Mozilla Firefox、最新のバージョンでのみ完全にサポートし、のみ実装の一部は、元の Opera、Safari などその他のブラウザーでします。
- **サーバー送信イベント**EventSource (ブラウザーがサポートする場合は Internet Explorer を除くすべてのブラウザーでは基本的にサーバー送信イベント、します) とも呼ばれる、。

### <a name="comet-transports"></a>Comet トランスポート

次のトランスポートがに基づいて、 [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web アプリケーションのモデル、ブラウザーまたは他のクライアントが、長時間保持されている HTTP 要求、具体的には、クライアントを使用せず、クライアントにデータをプッシュするサーバーを使用できるを維持それを要求します。

- **Forever Frame** (の Internet Explorer の場合のみ)。 永久にフレームでは、これは、要求が完了しない、サーバー上のエンドポイントを非表示の IFrame を作成します。 サーバーは、クライアントはすぐに実行するサーバーからクライアントへの一方向のリアルタイム接続を提供することをスクリプトを継続的に送信します。 クライアントからサーバーへの接続がサーバーからクライアントへの個別の接続を使用し、個々 のデータを送信する必要があるため、標準の HTTP 要求と同様に、新しい接続を作成します。
- **Ajax ロング ポーリング**です。 ロング ポーリングは、永続的な接続は作成されませんが、代わりに、サーバーは、この時点では、接続が閉じ、および新しい接続がすぐに要求されるまで開いたままのある要求を使用してサーバーをポーリングします。 接続のリセット中に、ある程度の遅延があります。

どの構成でサポートされているどのようなトランスポートの詳細については、次を参照してください。[サポートされているプラットフォーム](supported-platforms.md)です。

### <a name="transport-selection-process"></a>トランスポート選択のプロセス

次の一覧は、SignalR を使用して使用するトランスポートを決定する手順を示します。

1. ブラウザーが Internet Explorer 8 またはそれ以前の場合は、長いポーリングが使用されます。
2. JSONP が構成されている場合 (つまり、`jsonp`にパラメーターが設定されている`true`接続が開始されたときに)、長いポーリングを使用します。
3. (つまり、SignalR エンドポイントがホストするページと同じドメインではない) 場合、クロス ドメイン接続が確立されている場合、WebSocket が適用されます、次の条件を満たしている場合。

    - クライアントは、CORS (クロス オリジン リソース共有) をサポートします。 詳細については CORS をサポートしているクライアントを次を参照してください。 [caniuse.com で CORS](http://www.caniuse.com/CORS)です。
    - クライアントは、WebSocket をサポートしています。
    - このサーバーは、WebSocket をサポートします。

    これらの条件が満たされない場合、長いポーリングが使用されます。 ドメイン間の接続の詳細については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)です。
4. JSONP が構成されていない接続は、クロス ドメインではない場合は、WebSocket は、クライアントとサーバーの両方がサポートしている場合に使用されます。
5. クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合は、使用可能になる場合にサーバー送信イベントが使用されます。
6. サーバー送信イベントが使用できない場合は、永久にフレームが試行されます。
7. 無限のフレームが失敗すると、長いポーリングが使用されます。

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>トランスポートの監視

ハブで、ログに記録して、ブラウザーで、コンソール ウィンドウを開くようにすることで、アプリケーションが使用するトランスポートを指定できます。

ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。

`$.connection.hub.logging = true;`

- Internet Explorer で、F12 キーを押して開発者ツールを開くし、コンソール タブをクリックします。

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- Chrome では、ctrl キーと shift キーを押しながら J キーを押して、コンソールを開きます。

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

コンソールを開いて、ログを有効に、どのトランスポートは、SignalR によって使用されているを表示することができます。

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>トランスポートを指定します。

一定の時間とクライアント/サーバーをリソースには、トランスポートをネゴシエートします。 クライアントの機能がわかっている場合、トランスポートを指定できます、クライアント接続が開始されたときに。 次のコード スニペットでは、クライアントがその他のプロトコルをサポートしていないことことがわかっている場合に使用されるよう、Ajax の長いポーリング トランスポートを使用して、接続の開始を示しています。

`connection.start({ transport: 'longPolling' });`

クライアントの順序で特定のトランスポートの再試行をする場合は、フォールバックの順序を指定できます。 次のコード スニペットでは、しようとしての WebSocket とそれがない場合、長いポーリングに直接移動を示しています。

`connection.start({ transport: ['webSockets','longPolling'] });`

トランスポートを指定する文字列定数の定義は次のとおりです。

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>接続とハブ

SignalR の API には、クライアントとサーバー間で通信するための 2 つのモデルが含まれています: 永続的な接続とハブ。

接続では、受信者の 1 つ、グループ化、またはブロードキャスト メッセージを送信するための単純なエンドポイントを表します。 開発者は、SignalR を公開する低レベルの通信プロトコルへのアクセスを直接 (PersistentConnection クラスでは、.NET コードで表現) 永続的な接続 API によりします。 接続通信モデルを使用してに使い慣れた Windows Communication Foundation などの接続ベースの Api を使用している開発者になります。

ハブは、相互に直接メソッドを呼び出すには、クライアントとサーバーをできるようにする接続 API に基づいて構築されておりより高度なパイプラインです。 SignalR ディスパッチを処理する、コンピューターの境界を越えてマジック、した場合と同じサーバーで、ローカルのメソッドとして簡単にし、その逆としてメソッドを呼び出すには、クライアントできるようにします。 ハブ通信モデルを使用してにリモート呼び出し .NET リモート処理などの Api を使用している開発者にとって使い慣れたなります。 厳密に型指定されたパラメーターをメソッドに渡す、モデル バインディングを有効にするハブを使用できます。

### <a name="architecture-diagram"></a>アーキテクチャ図

次の図は、ハブ、永続的な接続、および基になるテクノロジは、トランスポートの関係を示しています。

![Api、トランスポート、およびクライアントを示す SignalR アーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>ハブのしくみ

パケットが呼び出されるメソッドのパラメーターと名前を含むアクティブなトランスポート経由で送信されるときに、サーバー側コードでは、クライアントのメソッドを呼び出す、(オブジェクトが、メソッド パラメーターとして送信されると、シリアル化される JSON を使用して)。 クライアントは、クライアント側のコードで定義されたメソッドにメソッド名を一致します。 一致があるの逆シリアル化されたパラメーターのデータを使用して、クライアントのメソッドが実行されます。

などのツールを使用して、メソッドの呼び出しを監視する[Fiddler です。](http://fiddler2.com/) 次の図は、SignalR のサーバーから Fiddler の [ログ] ウィンドウで web ブラウザー クライアントに送信されるメソッドの呼び出しを示しています。 メソッドの呼び出しと呼ばれるハブから送信されて`MoveShapeHub`、呼び出されるメソッドが呼び出されて`updateShape`です。

![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

この例ではハブ名が付いて、`H`パラメーター以外のメソッド名が付いて、`M`パラメーター、およびメソッドに送信されるデータが付いた、`A`パラメーター。 このメッセージを生成したアプリケーションを作成、[頻度が高いリアルタイム](tutorial-high-frequency-realtime-with-signalr.md)チュートリアルです。

### <a name="choosing-a-communication-model"></a>通信モデルを選択します。

ほとんどのアプリケーションでは、ハブ API を使用する必要があります。 次のような状況では、接続 API を使用できます。

- 実際のメッセージを送信する必要がありますを指定する形式。
- 開発者は、リモート起動のモデルではなく、メッセージおよびディスパッチのモデルを使用しています。
- メッセージング モデルを使用する既存のアプリケーションは、SignalR を使用するように移植したされているがします。
