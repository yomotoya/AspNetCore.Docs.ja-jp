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
ms.openlocfilehash: 0b7e223b6b793d1860797157be6021ffb7f1bc12
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090294"
---
<a name="introduction-to-signalr"></a>SignalR の概要
====================

参照してください[ASP.NET Core SignalR の概要](/aspnet/core/signalr/introduction)最新バージョンの Visual Studio を使用するこのチュートリアルの更新バージョン。 新しいチュートリアルでは使用[ASP.NET Core](/aspnet/core/)、このチュートリアルで多くの機能強化を提供します。

提供者: [Patrick Fletcher](https://github.com/pfletcher)

> この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。 
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。 チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)にて投稿してください。


## <a name="what-is-signalr"></a>SignalR とは何か

ASP.NET SignalR は、リアルタイム Web 機能をアプリケーションに追加するプロセスを簡素化する ASP.NET 開発者向けのライブラリです。 リアルタイム Web 機能とは、サーバーがクライアントからの新しいデータ要求を待つのではなく、サーバー コードが接続クライアントに対して利用可能になったコンテンツを瞬時にプッシュできる機能です。

SignalR は ASP.NET アプリケーションに何らかの「リアルタイム」web 機能を追加するためにことができます。 チャットの使用が例としては、多くの場合より多くを行うことができます。 いつでも、ユーザーは、新しいデータを表示する web ページを更新します。 または、ページを実装して[ポーリング時間の長い](http://en.wikipedia.org/wiki/Push_technology#Long_polling)の SignalR を使用して、候補では、新しいデータを取得します。 例としては、ダッシュ ボードとアプリケーションを監視するには、コラボレーション アプリケーション (など、同時ドキュメントの編集)、ジョブの進行状況の更新プログラム、およびリアルタイム フォーム。

SignalR ができます。 完全に新しい種類のサーバーから頻度の高い更新プログラムを必要とする web アプリケーションのリアルタイムのゲームなど。

SignalR は、サーバー側の .NET コードからクライアント ブラウザー (およびその他のクライアント プラットフォーム) に JavaScript 関数を呼び出す、サーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API を提供します。 SignalR には、接続管理 (イベントの接続や切断など) やグループ接続のための API も含まれます。

![SignalR によるメソッドの呼び出し](introduction-to-signalr/_static/image1.png)

SignalR では、自動的に接続管理を処理し、チャット ルームのように、接続されているすべてのクライアントへのメッセージを同時に配信できます。 特定のクライアントにメッセージを送信することもできます。 クライアントとサーバー間の接続は、通信ごとに再確立される従来の HTTP 接続とは異なり永続的です。

SignalR をサーバー コードを呼び出すクライアント コードの現在を使用して一般的な要求-応答のモデルではなく、リモート プロシージャ コール (RPC)、web ブラウザーで、「サーバー プッシュ」機能をサポートしています。

SignalR アプリケーションは、Service Bus、SQL Server または [Redis](http://redis.io) を使用して何千ものクライアントにスケール アウトできます。

SignalR はオープンソースで、[GitHub](https://github.com/signalr) を通してアクセスできます。

## <a name="signalr-and-websocket"></a>SignalR と WebSocket

SignalR では、使用可能な場合は新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。 WebSocket を直接使用してアプリを作成することはできますが、SignalR を使用するメリットは、実装する必要のある他の機能の多くが既に作成されているという点です。 これにより、最も重要な事ととして、古いクライアントのために個別のコード パスを作成することなく、WebSocket を活用できるようにアプリをコーディングすることができます。 SignalR は基本トランスポートの変更をサポートするために更新され、異なるバージョンの WebSocket でもアプリケーションにおいて一貫したインターフェイスが提供されるため、SignalR は WebSocket の更新に関する心配からユーザーを解放します。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>トランスポートとフォールバック

SignalR では、クライアントとサーバー間のリアルタイムの作業を行うには、必要なトランスポートの中に、抽象化です。 SignalR 接続は、HTTP、として起動し、入手可能になった場合、WebSocket 接続には、昇格します。 サーバーのメモリを最も効率的な使用によりが最も低い待機時間、および、(など、完全な双方向クライアントとサーバー間通信)、最も基本的な機能がいますが、最も厳格なので、WebSocket は、SignalR の理想的なトランスポート要件: WebSocket が Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。 これらの要件が満たされず、SignalR は、接続するために他のトランスポートを使用しようとします。

### <a name="html-5-transports"></a>HTML 5 トランスポート

これらのトランスポートは [HTML 5](http://en.wikipedia.org/wiki/HTML5) のサポートに依存します。 クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。

- **WebSocket** (場合、サーバーとブラウザーの両方は、Websocket をサポートすることができますを示します)。 WebSocket は、クライアントとサーバー間の場合は true。 永続的な双方向接続を確立するだけのトランスポートです。 ただし、WebSocket でも最も厳しい要件があります。完全に Microsoft Internet Explorer、Google Chrome、Mozilla Firefox、最新のバージョンでのみサポートし、Opera、Safari などの他のブラウザーで部分の実装のみがあります。
- **サーバー送信イベント**EventSource (ブラウザーがサポートする場合は Internet Explorer を除くすべてのブラウザーでは基本的にサーバー送信イベント、.) とも呼ばれます

### <a name="comet-transports"></a>Comet トランスポート

次のトランスポートがに基づいて、 [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web アプリケーションのモデル、ブラウザーまたは他のクライアントが長時間保持されている HTTP 要求データを具体的には、クライアントを使用せず、クライアントをプッシュするサーバーが使用できるを維持それを要求します。

- **フレームの永久に**(用 Internet Explorer の場合のみ)。 永遠にフレームは、完了しませんサーバー上のエンドポイントへの要求を非表示の IFrame を作成します。 サーバーは、サーバーからクライアントへの一方向のリアルタイム接続を提供することはすぐに実行するクライアントに、スクリプトを継続的に送信します。 クライアントからサーバーへの接続のクライアントの接続をサーバーから別の接続を使用して、送信する必要があるデータの各部分の標準の HTTP 要求のように、新しい接続が作成されます。
- **長いポーリングの Ajax**します。 ロング ポーリングは、永続的な接続は作成されませんが、代わりに、サーバーは、この時点で、接続が閉じ、新しい接続がすぐに要求されるまで開いたままになる要求でサーバーをポーリングします。 接続のリセット中に、いくつかの待機時間があります。

どの構成でサポートされているどのようなトランスポートの詳細については、次を参照してください。[サポートされているプラットフォーム](supported-platforms.md)します。

### <a name="transport-selection-process"></a>トランスポート選択のプロセス

SignalR が使用するトランスポートを決定する手順を以下に示します。

1. ブラウザーが Internet Explorer 8 またはそれ以前の場合は、Long Polling が使用されます。
2. JSONP が設定されている場合 (つまり、接続が開始されると `jsonp`パラメーターが `true`に設定される)、Long Polling が使用されます。
3. クロス ドメイン接続が確立されている場合 (つまり、SignalR エンドポイントが、ホスティング ページと同じドメインにない)、次の条件が満たされた場合に WebSocket が使用されます。

   - クライアントは、CORS (クロス オリジン リソース共有) をサポートします。 クライアントで CORS がサポートの詳細は、次を参照してください。 [caniuse.com で CORS](http://www.caniuse.com/CORS)します。
   - クライアントは、WebSocket をサポートしています。
   - サーバーは、WebSocket をサポートしています。

     これらの条件のいずれかが満たされていない場合は、長いポーリングが使用されます。 ドメイン間の接続の詳細については、次を参照してください。[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)します。
4. JSONP が構成されていない接続は、クロス ドメインではない場合は、WebSocket は、クライアントとサーバーの両方がサポートしている場合に使用されます。
5. クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合、サーバー送信イベントは、入手可能になった場合に使用されます。
6. サーバー送信イベントが使用できない場合は、永久にフレームが試行されます。
7. フレームを永久に失敗すると、長いポーリングが使用されます。

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>トランスポートの監視

ハブでログを有効にし、ブラウザーでコンソール ウィンドウを開くことによって、アプリケーションで使用されているトランスポートを特定できます。

ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。

`$.connection.hub.logging = true;`

- Internet Explorer で、F12 キーを押して開発者ツールを開き、[コンソール] タブをクリックします。

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- Chrome では、Ctrl + Shift + J キーを押して、コンソールを開きます。

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

コンソールを開くとログが有効では、トランスポートは、SignalR によって使用されているを参照してください。 できるします。

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>トランスポートの指定

トランスポートのネゴシエートには一定の時間を要し、クライアントとサーバーの一定量のリソースを使用します。 クライアントの機能が分かっている場合は、クライアント接続が開始された時点でトランスポートを指定することができます。 次のコード スニペットでは、クラインが他のプロトコルをサポートしていないことが分かっている場合にも使用される、Ajax Long Polling のトランスポートを使用して接続を開始する方法を示しています。

`connection.start({ transport: 'longPolling' });`

クライアントが特定のトランスポートを順番に試行するようにする場合は、フォールバック順序を指定できます。 次のコード スニペットは、WebSocket を試し、失敗した場合、Long Polling に直接移行する例を示します。

`connection.start({ transport: ['webSockets','longPolling'] });`

トランスポートを指定する文字列定数の定義は次のとおりです。

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>接続とハブ

SignalR の API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています: 永続的な接続とハブ。

接続は、単一受信者メッセージ、グループ化メッセージ、またはブロードキャスト メッセージを送信するための単純なエンドポイントを表します。 固定接続 API (PersistentConnection クラスによって .NET コードで表される) は、SignalR が公開している低レベルの通信プロトコルへの直接アクセスを開発者に提供します。 接続の通信モデルは、Windows Communication Foundation などの接続に基づく API を使用したことのある開発者にとって使いやすいものです。

ハブは、クライアントとサーバーが相互に直接メソッドを呼び出すことができるようにする接続 API に基づいて構築されたより高度なパイプラインです。 SignalR は、マシンの境界でのディスパッチを魔法のように行い、クライアントがローカルのメソッドのように簡単にサーバーでメソッドを呼び出せるようにします (またはその逆)。 ハブ通信モデルは、.NET Remoting などのリモート呼び出し API を使用したことのある開発者にとって使いやすいものです。 また、ハブを使用すると、厳密に型指定されたパラメーターをメソッドに渡すことができ、モデルのバインディングが可能になります。

### <a name="architecture-diagram"></a>アーキテクチャ図

次の図は、ハブ、永続的な接続、およびトランスポートに使用される基になるテクノロジ間の関係を示します。

![Api、トランスポート、およびクライアントを示す、SignalR のアーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>ハブのしくみ

サーバー側コードでは、クライアントでメソッドを呼び出すと、呼び出されるメソッドの名前とパラメーターを含むパケットがアクティブなトランスポート経由で送信されます (オブジェクトがメソッド パラメーターとして送信される場合、JSON を使用してシリアル化されます)。 次にクライアントは、メソッド名をクライアント側のコードで定義されているメソッドと一致させます。 一致がある場合、逆シリアル化されたパラメーターのデータを使用してクライアント メソッドが実行されます。

などのツールを使用して、メソッドの呼び出しを監視する[Fiddler します。](http://fiddler2.com/) 次の図は、Fiddler の [ログ] ウィンドウで、web ブラウザー クライアントに SignalR サーバーから送信されたメソッドの呼び出しを示します。 メソッドの呼び出しがという名前でハブから送信されて`MoveShapeHub`、呼び出されるメソッドが呼び出されて`updateShape`します。

![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

この例で、ハブの名前が付いて、`H`パラメーターは、メソッド名が付いて、`M`パラメーター、およびメソッドに送信されるデータがで識別される、`A`パラメーター。 このメッセージを生成したアプリケーションは、[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)チュートリアル。

### <a name="choosing-a-communication-model"></a>通信モデルの選択

ほとんどのアプリケーションでは、ハブ API を使用する必要があります。 接続 API は次の状況で使用できます。

- 実際の送信メッセージの形式を指定する必要がある
- 開発者が、リモート呼び出しモデルではなく、メッセージおよびディスパッチ モデルを利用する事を推奨している
- メッセージングモデルを使用する既存のアプリケーションが SignalR を使用するように移植されている
