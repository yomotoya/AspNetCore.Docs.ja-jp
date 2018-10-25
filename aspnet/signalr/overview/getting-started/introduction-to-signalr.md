---
uid: signalr/overview/getting-started/introduction-to-signalr
title: SignalR の概要 |Microsoft Docs
author: pfletcher
description: この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: d103573fb31bb3b08d054cbf65ff906bd5d151d3
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912801"
---
<a name="introduction-to-signalr"></a>SignalR の概要
====================

このチュートリアルの更新バージョンが利用可能な[ここ](/aspnet/core/tutorials/signalr)Visual Studio の最新バージョンを使用します。 新しいチュートリアルでは [ASP.NET Core](/aspnet/core/) を使用し、このチュートリアルで多くの改善点があります。

[Patrick Fletcher](https://github.com/pfletcher) による。

> この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。 
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの良いところや、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合は、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr) にて投稿してください。


## <a name="what-is-signalr"></a>SignalR とは何か

ASP.NET SignalR は、リアルタイム web 機能をアプリケーションに追加するプロセスを簡素化する ASP.NET 開発者向けのライブラリです。 リアルタイム web 機能とは、サーバーがクライアントからの要求を待つのではなく、サーバーが接続クライアントに対してコンテンツをプッシュすることが利用可能にする機能です。

SignalR は ASP.NET アプリケーションに何らかの「リアルタイム」web 機能を追加するためにことができます。 よくチャットの使用が例とされますが、もっと多くのことを行うことができます。 よくユーザーは、新しいデータを表示するために web ページを更新したり、ページに[ロングポーリング](http://en.wikipedia.org/wiki/Push_technology#Long_polling)のを実装して新しいデータを取得します。これらは SignalR を使用できる候補。 サンプルとしては、ダッシュ ボードとアプリケーションを監視するには、コラボレーション アプリケーション (同時ドキュメントの編集など)、ジョブの進行状況の更新プログラム、およびリアルタイム フォーム が含まれます。

SignalR はリアルタイムゲームなどのサーバーから高頻度で更新を必要とする完全に新しいタイプの Web アプリケーションを作ることができます。

SignalR は、サーバー側の .NET コードからのブラウザー (およびその他のクライアント プラットフォーム) クライアントの JavaScript 関数を呼び出す server-to-client 型のリモート プロシージャ コール (RPC) を作成するための単純な API を提供します。 SignalR には接続管理(接続し、切断イベントなど)や、および接続をグループ化用の API も含まれます 

![SignalR によるメソッドの呼び出し](introduction-to-signalr/_static/image1.png)

SignalR では、自動的に接続の管理を処理し、チャット ルームなど、接続されているすべてのクライアントへのブロードキャスト メッセージを同時に送信できます。 特定のクライアントにメッセージを送信することもできます。 クライアントとサーバー間の接続は、各通信の再確立される従来の HTTP 接続とは異なり永続的です。

SignalR はサーバー プッシュ機能をサポートしています。この機能は現在の一般的な web のリクエスト-レスポンスモデルではなくリモート　プロシージャ　コール (RPC) を利用して ブラウザのクライアントコードを呼び出すことができます。

SignalR アプリケーションはService Bus、SQL Server または[Redis](http://redis.io) を使用して何千ものクライアントにスケール アウトできます。

SignalR はオープン ソースで、[GitHub](https://github.com/signalr)を通してアクセスできます。

## <a name="signalr-and-websocket"></a>SignalR と WebSocket

SignalR では、使用可能な場合は新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。 WebSocket を直接使用してアプリケーションを作成することはできますが、SignalR を利用する意味は実装する必要がある追加の機能の多くは既に提供されているという点です。 最も重要な事は、WebSocket の古いクライアントのための別のコード パスの作成について心配することがなく利用するアプリをコーディングすることです。 SignalR は WebSocket の更新に関する心配からあなたを守ります。SignalR は基本トランスポートの変更をサポートするための更新や WebSocket のバージョン互換を保つインターフェイスを提供しているからです。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>トランスポートとフォールバック

SignalR は、クライアントとサーバー間のリアルタイムの作業を行うために必要なトランスポートの一部を抽象化したものです。 SignalR 接続は、HTTP として起動し、接続可能になった場合、WebSocket 接続となります。 WebSocket はサーバーのメモリを最も効率的に使用し、最も低い待機時間、および最も基本的な機能(完全な双方向クライアントとサーバー間通信など)があるため、SignalR の理想的なトランスポートですが、WebSocket はWindows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。 これらの要件が満たされない場合、SignalR は、接続するために他のトランスポートを使用を試みます。

### <a name="html-5-transports"></a>HTML 5 トランスポート

これらのトランスポートは [HTML 5](http://en.wikipedia.org/wiki/HTML5) のサポート依存します。 クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。

- **WebSocket** (サーバーとブラウザーの両方はが Websocket をサポートすることが示した場合)。 WebSocket は、クライアントとサーバー間通信の真に永続的な双方向接続を確立するためのトランスポートです。 ただし、WebSocket でも最も厳しい要件があります。完全に Microsoft Internet Explorer、Google Chrome、Mozilla Firefox、が最新のバージョンでのみサポートし、Opera、Safari などの他のブラウザーでは部分実装の場合があります。
- **サーバー送信イベント**EventSource とも呼ばれます。(ブラウザーがサーバー送信イベントサポートする場合。これは基本的に Internet Explorer を除くすべてのブラウザーです)

### <a name="comet-transports"></a>Comet トランスポート

以下のトランスポートは、ブラウザーまたは他のクライアントが長時間保持する HTTP リクエストを管理する [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web アプリケーションのモデルに基づいています。これは、クライアントからの特別な要求をすることなくサーバーからクライアントへデータをプッシュすることができます。

- **永遠フレーム**(Internet Explorer の場合のみ)。 永遠フレームは、サーバー上のエンドポイントへの要求が完了しない非表示の IFrame を作成します。 サーバーは、クライアントにすぐに実行されるスクリプトを継続的に送信し、サーバーからクライアントへの一方向のリアルタイム接続を提供します。 クライアントからサーバーへの接続は、クライアントの接続をサーバーから別の接続を使用して、標準の HTTP 要求のように送信する必要があるデータのがあるたびに新しい接続が作成されます。
- **Ajax のロングポーリング** ロング ポーリングは、永続的な接続は作成されませんが、サーバーが応答を完了するまで開いたままの要求でサーバーをポーリングします。接続が閉じられたら即座に新しい接続が要求されます。

どの構成でどのようなトランスポートがサポートされているかについての詳細は、[サポートされているプラットフォーム](supported-platforms.md)を参照してください。

### <a name="transport-selection-process"></a>トランスポート選択のプロセス

SignalR が使用するトランスポートを決定する手順を以下に示します。

1. ブラウザーが Internet Explorer 8 またはそれ以前の場合は、ロングポーリングが使用されます。
2. JSONP が設定されている場合 (つまり、`jsonp`パラメーターに`true`が設定されて接続が開始された場合)、ロングポーリングを使用します。
3. クロス ドメイン接続が確立されている場合(つまり、SignalR エンドポイントが、ホスティング ページと同じドメインにない)、次の条件が満たされた場合にWebSocket が使用されます。

   - クライアントは、CORS (クロス オリジン リソース共有) をサポートする。 CORS をサポートしているクライアントの詳細は、[caniuse.com で CORS](http://www.caniuse.com/CORS) を参照してください。
   - クライアントが、WebSocket をサポートしている。
   - サーバーが、WebSocket をサポートしている。

     これらの条件のいずれかが満たされていない場合は、ロングポーリングが使用されます。 ドメイン間の接続の詳細については、[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)を参照してください。
4. JSONP が設定されていない接続でかつクロス ドメインではない場合は、クライアントとサーバーの両方がサポートしている場合に WebSocket が使用されます。
5. クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合、利用可能に場合にサーバー送信イベントが使用されます。
6. サーバー送信イベントが使用できない場合は、永遠フレームの使用を試みます。
7. 永遠フレームに失敗すると、ロングポーリングが使用されます。

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>トランスポートの監視

利用しているブラウザーでコンソールウィンドウを開き、ハブのログを有効にすることでアプリケーションが利用しているトランスポートを特定できます。

ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。

`$.connection.hub.logging = true;`

- Internet Explorer で、F12 キーを押して開発者ツールを開き、[コンソール] タブをクリックします。

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- Chrome では、Ctrl + Shift + J キーを押して、コンソールを開きます。

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

コンソールを開き、ログが有効な場合は、SignalR で利用されているトランスポートを見ることができます。

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>トランスポートの指定

トランスポートのネゴシエーションはクライアントとサーバーのリソースを大量に使用します。もしクライアントの性能がわかっている場合、クライアント接続時にトランスポートを指定することができます。次のコード　スニペットでは、クラインとがその他のプロトコルをサポートしていないことをわかっているが場合、Ajax のロングポーリングのトランスポートを使用して接続開始しています。

`connection.start({ transport: 'longPolling' });`

クライアントが特定のトランスポートを順番に試行するようにする場合は、フォールバック順序を指定できます。次のコード スニペットは、WebSocket を試し、失敗した場合、ロングポーリングを例です。

`connection.start({ transport: ['webSockets','longPolling'] });`

トランスポートを指定する文字列定数の定義は次のとおりです。

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>接続とハブ

SignalR の API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています: 永続的な接続とハブ。

接続では、単一受信者、グループ化、またはブロードキャスト メッセージの送信の単純なエンドポイントを表します。 永続的な接続 API (.NET コードでは PersistentConnection クラスで表されます)は、SignalR が公開している低レベルの通信プロトコルへ開発者がアクセスできる事を示しています。 接続の通信モデルを使用して Windows Communication Foundation など、コネクションベースの API を使用していた開発者にとって身近になります。

ハブは、クライアントとサーバーの相互に直接メソッドを呼び出すことができる接続 API に基づいて構築されたより高度なパイプラインです。 SignalR は、マシンをまたいだディスパッチを魔法のように行い、クライアントがローカルのメソッドのように、サーバーでメソッドを呼び出すをできます。Hubs通信モデルを使用することは、.NET Remotingなどのリモート呼び出しAPIを使用した開発者にとっては馴染み深いものです。 また、ハブを使用すると、強く型付けされたパラメーターをメソッドに渡すことができ、モデルのバインディングが可能になります。

### <a name="architecture-diagram"></a>アーキテクチャ図

次の図は、ハブ、永続的な接続、およびトランスポートに使用される基になるテクノロジ間の関係を示します。

![Api、トランスポート、およびクライアントを示す、SignalR のアーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>ハブのしくみ

サーバー側コードでは、クライアントでメソッドを呼び出すと、呼び出されるメソッドのパラメーターと名前を含むパケットがアクティブなトランスポート経由で送信されます(オブジェクトがメソッド パラメーターとして送信される際に、JSON を使用してシリアル化されます)。 次にクライアントには、メソッド名をクライアント側のコードで定義されているメソッドを次と一致します。 一致がある場合、逆シリアル化されたパラメーターのデータを使用してクライアント メソッドが実行されます。

メソッドの呼び出しは[Fiddler](http://fiddler2.com/) などのツールを使用して監視できます。次の図は、FiddlerのLogsペインで、SignalRサーバーからweb ブラウザークライアントに送信されたメソッド呼び出しを示しています。 メソッド呼び出しは、`MoveShapeHub` というハブから送信され、呼び出されるメソッドは `updateShape` と呼ばれます。


![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

この例では、ハブ名は`H`パラメーターで識別されます。メソッド名は、`M`パラメーターで識別され、メソッドに送信されるデータは`A`パラメーターで識別されます。 このメッセージを生成したアプリケーションは、[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)チュートリアルで作成されます。

### <a name="choosing-a-communication-model"></a>通信モデルの選択

ほとんどのアプリケーションでは、ハブ API を使用する必要があります。 接続 API は次の状況で使用できます。

- 実際の送信メッセージの形式を指定する必要がある
- 開発者が、リモート呼び出しモデルではなく、メッセージおよびディスパッチ モデルを利用する事を推奨している
- メッセージングモデルを使用する既存のアプリケーションは、SignalR を使用するように移植されています。
