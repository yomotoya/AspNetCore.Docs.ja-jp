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

このチュートリアルの更新バージョンが利用可能な[ここ](/aspnet/core/tutorials/signalr)Visual Studio の最新バージョンを使用します。 新しいチュートリアルでは使用[ASP.NET Core](/aspnet/core/)、このチュートリアルで多くの機能強化を提供します。

によって[Patrick Fletcher](https://github.com/pfletcher)

> この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。 
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)します。


## <a name="what-is-signalr"></a>SignalR とは何か

ASP.NET SignalR は、リアルタイム web 機能をアプリケーションに追加するプロセスを簡素化する ASP.NET 開発者向けのライブラリです。 リアルタイム web 機能とは、サーバーの新しいデータを要求するクライアントの待機時間を移動するのではなく、サーバー コード プッシュが接続クライアントに対して瞬時にコンテンツが利用可能にする機能です。

SignalR は ASP.NET アプリケーションに何らかの「リアルタイム」web 機能を追加するためにことができます。 チャットの使用が例としては、多くの場合より多くを行うことができます。 いつでも、ユーザーは、新しいデータを表示する web ページを更新します。 または、ページを実装して[ポーリング時間の長い](http://en.wikipedia.org/wiki/Push_technology#Long_polling)の SignalR を使用して、候補では、新しいデータを取得します。 例としては、ダッシュ ボードとアプリケーションを監視するには、コラボレーション アプリケーション (など、同時ドキュメントの編集)、ジョブの進行状況の更新プログラム、およびリアルタイム フォーム。

SignalR ができます。 完全に新しい種類のサーバーから頻度の高い更新プログラムを必要とする web アプリケーションのリアルタイムのゲームなど。

SignalR は、サーバー側の .NET コードからのブラウザー (およびその他のクライアント プラットフォーム) クライアントで JavaScript 関数を呼び出すサーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API を提供します。 SignalR の接続管理の API も含まれます (接続し、切断イベントなど)、および接続をグループ化します。

![SignalR によるメソッドの呼び出し](introduction-to-signalr/_static/image1.png)

SignalR では、自動的に、接続の管理を処理し、チャット ルームなど、接続されているすべてのクライアントへのブロードキャスト メッセージを同時に、できます。 特定のクライアントにメッセージを送信することもできます。 クライアントとサーバー間の接続は、各通信の再確立される従来の HTTP 接続とは異なり、永続的です。

SignalR をサーバー コードを呼び出すクライアント コードの現在を使用して一般的な要求-応答のモデルではなく、リモート プロシージャ コール (RPC)、web ブラウザーで、「サーバー プッシュ」機能をサポートしています。

SignalR アプリケーションを何千もの Service Bus、SQL Server を使用してクライアントをスケール アウトできますまたは[Redis](http://redis.io)します。

SignalR はオープン ソース、使用してアクセス[GitHub](https://github.com/signalr)します。

## <a name="signalr-and-websocket"></a>SignalR と WebSocket

SignalR では、使用可能な場合、新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。 確かに、WebSocket を直接使用して、SignalR を実装する必要がありますが、追加の機能の多くは既に実施することを意味を使用してアプリを作成できますが。 最も重要なは、WebSocket の古いクライアントの別のコード パスの作成について心配することがなく利用するアプリをコーディングすることがこれを意味します。 SignalR も明らかにならないようにする、アプリケーションが WebSocket のバージョン間での一貫性のあるインターフェイスを提供する、基になるトランスポートに変更をサポートするために SignalR が更新されるため、WebSocket への更新について心配する必要がなくなります。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>トランスポートとフォールバック

SignalR では、クライアントとサーバー間のリアルタイムの作業を行うには、必要なトランスポートの中に、抽象化です。 SignalR 接続は、HTTP、として起動し、入手可能になった場合、WebSocket 接続には、昇格します。 サーバーのメモリを最も効率的な使用によりが最も低い待機時間、および、(など、完全な双方向クライアントとサーバー間通信)、最も基本的な機能がいますが、最も厳格なので、WebSocket は、SignalR の理想的なトランスポート要件: WebSocket が Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。 これらの要件が満たされず、SignalR は、接続するために他のトランスポートを使用しようとします。

### <a name="html-5-transports"></a>HTML 5 を転送します。

これらのトランスポートのサポートによって異なります[HTML 5](http://en.wikipedia.org/wiki/HTML5)します。 クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。

- **WebSocket** (場合、サーバーとブラウザーの両方は、Websocket をサポートすることができますを示します)。 WebSocket は、クライアントとサーバー間の場合は true。 永続的な双方向接続を確立するだけのトランスポートです。 ただし、WebSocket でも最も厳しい要件があります。完全に Microsoft Internet Explorer、Google Chrome、Mozilla Firefox、最新のバージョンでのみサポートし、Opera、Safari などの他のブラウザーで部分の実装のみがあります。
- **サーバー送信イベント**EventSource (ブラウザーがサポートする場合は Internet Explorer を除くすべてのブラウザーでは基本的にサーバー送信イベント、.) とも呼ばれます

### <a name="comet-transports"></a>Comet トランスポート

次のトランスポートがに基づいて、 [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) web アプリケーションのモデル、ブラウザーまたは他のクライアントが長時間保持されている HTTP 要求データを具体的には、クライアントを使用せず、クライアントをプッシュするサーバーが使用できるを維持それを要求します。

- **フレームの永久に**(用 Internet Explorer の場合のみ)。 永遠にフレームは、完了しませんサーバー上のエンドポイントへの要求を非表示の IFrame を作成します。 サーバーは、サーバーからクライアントへの一方向のリアルタイム接続を提供することはすぐに実行するクライアントに、スクリプトを継続的に送信します。 クライアントからサーバーへの接続のクライアントの接続をサーバーから別の接続を使用して、送信する必要があるデータの各部分の標準の HTTP 要求のように、新しい接続が作成されます。
- **長いポーリングの Ajax**します。 ロング ポーリングは、永続的な接続は作成されませんが、代わりに、サーバーは、この時点で、接続が閉じ、新しい接続がすぐに要求されるまで開いたままになる要求でサーバーをポーリングします。 接続のリセット中に、いくつかの待機時間があります。

どの構成でサポートされているどのようなトランスポートの詳細については、次を参照してください。[サポートされているプラットフォーム](supported-platforms.md)します。

### <a name="transport-selection-process"></a>トランスポートの選択のプロセス

SignalR を使用して使用するトランスポートを決定する手順を以下に示します。

1. ブラウザーが Internet Explorer 8 またはそれ以前の場合は、長いポーリングが使用されます。
2. JSONP が構成されている場合 (つまり、`jsonp`パラメーターに設定されて`true`接続が開始された場合)、長いポーリングを使用します。
3. (つまり、SignalR エンドポイントが、ホスティング ページと同じドメインにない) 場合、クロス ドメイン接続が確立されている場合、WebSocket が使用されます、次の条件が満たされた場合。

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

アプリケーションは、ハブにログ記録と、コンソール ウィンドウを開き、お使いのブラウザーで有効にして使用してトランスポートを指定できます。

ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。

`$.connection.hub.logging = true;`

- Internet Explorer で、F12 キーを押して開発者ツールを開くし、[コンソール] タブをクリックします。

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- Chrome では、Ctrl + Shift + J キーを押して、コンソールを開きます。

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

コンソールを開くとログが有効では、トランスポートは、SignalR によって使用されているを参照してください。 できるします。

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>トランスポートを指定します。

一定の時間とクライアント/サーバーをリソースには、トランスポートをネゴシエートします。 クライアントの機能がわかっている場合、トランスポートを指定できます、クライアント接続が開始されると。 次のコード スニペットでは、クライアントがその他のプロトコルをサポートしていないことをわかっているが場合に使用されるように、Ajax の長いポーリングのトランスポートを使用して接続の開始を示しています。

`connection.start({ transport: 'longPolling' });`

場合は、クライアントの順序で特定のトランスポートにフォールバックの順序を指定できます。 次のコード スニペットは、しようとしての WebSocket とそれがない場合、長いポーリングを直接を示します。

`connection.start({ transport: ['webSockets','longPolling'] });`

トランスポートを指定する文字列定数の定義は次のとおりです。

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>接続とハブ

SignalR の API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています: 永続的な接続とハブ。

接続では、単一受信者、グループ化、またはブロードキャスト メッセージの送信の単純なエンドポイントを表します。 永続的な接続 API が (PersistentConnection クラスでは、.NET コードで表す) は、開発者は、SignalR を公開する低レベルの通信プロトコルへのアクセスを直接示しています。 接続の通信モデルを使用して Windows Communication Foundation など、接続ベースの Api を使用していた開発者にとって身近になります。

ハブは、クライアントとサーバーの相互に直接メソッドを呼び出すことができる接続 API に基づいて構築されたより高度なパイプラインです。 SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバーでメソッドを呼び出すをできるように、不思議な力とした場合と同じコンピューターの境界を越えてディスパッチを処理します。 ハブの通信モデルを使用してリモート呼び出し .NET リモート処理などの Api を使用していた開発者にとって身近になります。 ハブを使用してではモデル バインドを有効にすると、メソッドに厳密に型指定されたパラメーターを渡すこともできます。

### <a name="architecture-diagram"></a>アーキテクチャ図

次の図は、ハブ、永続的な接続、およびトランスポートに使用される基になるテクノロジ間の関係を示します。

![Api、トランスポート、およびクライアントを示す、SignalR のアーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>ハブのしくみ

パケットが呼び出されるメソッドのパラメーターと名前を含むアクティブなトランスポート経由で送信されるときに、サーバー側コードでは、クライアントでメソッドを呼び出し、(オブジェクトがメソッド パラメーターとして送信されると、シリアル化される JSON を使用して)。 クライアントには、メソッド名をクライアント側のコードで定義されているメソッドを次と一致します。 一致がある場合、逆シリアル化されたパラメーターのデータを使用してクライアント メソッドが実行されます。

などのツールを使用して、メソッドの呼び出しを監視する[Fiddler します。](http://fiddler2.com/) 次の図は、Fiddler の [ログ] ウィンドウで、web ブラウザー クライアントに SignalR サーバーから送信されたメソッドの呼び出しを示します。 メソッドの呼び出しがという名前でハブから送信されて`MoveShapeHub`、呼び出されるメソッドが呼び出されて`updateShape`します。

![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

この例で、ハブの名前が付いて、`H`パラメーターは、メソッド名が付いて、`M`パラメーター、およびメソッドに送信されるデータがで識別される、`A`パラメーター。 このメッセージを生成したアプリケーションは、[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)チュートリアル。

### <a name="choosing-a-communication-model"></a>通信モデルを選択します。

ほとんどのアプリケーションでは、ハブ API を使用する必要があります。 次の状況では、接続 API を使用できます。

- 指定する実際のメッセージ送信のニーズの形式です。
- 開発者は、リモート呼び出しモデルではなく、メッセージおよびディスパッチ モデルを操作しています。
- SignalR を使用してメッセージング モデルを使用する既存のアプリケーションに移植されるは。
