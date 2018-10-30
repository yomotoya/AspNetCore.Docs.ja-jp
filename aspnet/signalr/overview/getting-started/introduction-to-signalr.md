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

Visual Studio の最新バージョンを使用するこのチュートリアルの更新バージョンについては、[ここ](/aspnet/core/tutorials/signalr)を参照してください。新しいチュートリアルでは [ASP.NET Core](/aspnet/core/) が使用され、このチュートリアルから多くの点で改善されています。

提供者: [Patrick Fletcher](https://github.com/pfletcher)

> この記事では、SignalR は、いくつかのソリューションを作成するため、デザインされたについて説明します。 
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr)にて投稿してください。


## <a name="what-is-signalr"></a>SignalR とは何か

ASP.NET SignalR は、リアルタイム Web 機能をアプリケーションに追加するプロセスを簡素化する ASP.NET 開発者向けのライブラリです。リアルタイム Web 機能とは、サーバーがクライアントからの新しいデータ要求を待つのではなく、サーバー コードが接続クライアントに対して利用可能になったコンテンツを瞬時にプッシュできる機能です。

SignalR は、ASP.NET アプリケーションに何らかの「リアルタイム」Web 機能を追加するために使用することができます。よくチャットが例として使用されますが、もっと多くのことを行うことができます。ユーザーが新しいデータを表示するために Web ページを更新したり、ページが[ロングポーリング](http://en.wikipedia.org/wiki/Push_technology#Long_polling)を実装して新しいデータを取得したりする場合は、SignalR を使用することをおすすめします。サンプルとしては、ダッシュ ボード アプリケーションや監視アプリケーション、コラボレーション アプリケーション (ドキュメントの同時編集など)、ジョブの進行状況の更新プログラム、およびリアルタイム フォームが含まれます。

SignalR はリアルタイム ゲームなど、サーバーから高頻度で更新を必要とするまったく新しいタイプの Web アプリケーションを有効にできます。

SignalR は、サーバー側の .NET コードからクライアント ブラウザー (およびその他のクライアント プラットフォーム) に JavaScript 関数を呼び出す、サーバーからクライアントへのリモート プロシージャ コール (RPC) を作成するための単純な API を提供します。SignalR には、接続管理 (イベントの接続や切断など) やグループ接続のための API も含まれます。

![SignalR によるメソッドの呼び出し](introduction-to-signalr/_static/image1.png)

SignalR では、自動的に接続管理を処理し、チャット ルームのように、接続されているすべてのクライアントへのメッセージを同時に配信できます。特定のクライアントにメッセージを送信することもできます。クライアントとサーバー間の接続は、通信ごとに再確立される従来の HTTP 接続とは異なり永続的です。

SignalR は「サーバー プッシュ」機能をサポートしています。この機能では、現在 Web において一般的な要求 - 応答モデルではなく、リモート プロシージャ コール (RPC) を使用して、サーバー コードによってブラウザー内のクライアント コードを呼び出すことができます。

SignalR アプリケーションは、Service Bus、SQL Server または [Redis](http://redis.io) を使用して何千ものクライアントにスケール アウトできます。

SignalR はオープン ソースで、[GitHub](https://github.com/signalr)を通してアクセスできます。

## <a name="signalr-and-websocket"></a>SignalR と WebSocket

SignalR では、使用可能な場合は新しい WebSocket トランスポートを使用し、必要に応じて、古いトランスポートにフォールバックします。WebSocket を直接使用してアプリを作成することはできますが、SignalR を使用するメリットは、実装する必要のある他の機能の多くが既に作成されているという点です。これにより、最も重要な事ととして、古いクライアントのために個別のコード パスを作成することなく、WebSocket を活用できるようにアプリをコーディングすることができます。SignalR は基本トランスポートの変更をサポートするために更新され、異なるバージョンの WebSocket でもアプリケーションにおいて一貫したインターフェイスが提供されるため、SignalR は WebSocket の更新に関する心配からユーザーを解放します。

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>トランスポートとフォールバック

SignalR は、クライアントとサーバー間のリアルタイムの作業を行うために必要なトランスポートの一部を抽象化したものです。SignalR 接続は HTTP として起動し、WebSocket 接続が可能な場合は WebSocket 接続となります。WebSocket は、サーバーのメモリを最も効率的に使用し、待機時間が最も短く、最も基本的な機能 (クライアントとサーバー間の全二重通信など) が備わっているため、SignalR の理想的なトランスポートですが、細かい要件があります。WebSocket では、Windows Server 2012 または Windows 8、および .NET Framework 4.5 を使用するサーバーが必要です。これらの要件が満たされない場合、SignalR は、接続するために他のトランスポートの使用を試みます。

### <a name="html-5-transports"></a>HTML 5 トランスポート

これらのトランスポートは [HTML 5](http://en.wikipedia.org/wiki/HTML5) のサポートに依存します。クライアントのブラウザーが HTML 5 の標準をサポートしていない場合は、古いトランスポートが使用されます。

**WebSocket** (サーバーとブラウザーの両方が Websocket をサポートすることを示す場合): WebSocket は、クライアントとサーバー間の通信における真に永続的な双方向接続を確立するためのトランスポートです。ただし、WebSocket には細かい要件もあります。WebSocket は Microsoft Internet Explorer、Google Chrome、Mozilla Firefox の最新のバージョンでのみ完全にサポートされ、Opera や Safari などの他のブラウザーでは部分的にしか実装されません。
- **Server Sent Events**: EventSource とも呼ばれます (ブラウザーがサーバー送信イベントをサポートする場合。基本的に Internet Explorer を除くすべてのブラウザーでサポートされます)。

### <a name="comet-transports"></a>Comet トランスポート

以下のトランスポートは、ブラウザーまたは他のクライアントが長期保持 HTTP リクエストを管理する [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) Web アプリケーションのモデルに基づいています。これは、クライアントからの特別な要求をすることなくサーバーからクライアントへデータをプッシュすることができます。

- **Forever Frame** (Internet Explorer の場合のみ): Forever Frame は、サーバー上のエンドポイントへの要求が完了しない非表示の IFrame を作成します。サーバーは、すぐに実行されるスクリプトを継続的にクライアントに送信し、サーバーからクライアントへの一方向のリアルタイム接続を提供します。クライアントからサーバーへの接続は、サーバーからクライアントとは異なる接続を使用し、標準の HTTP 要求のように、送信する必要のあるデータがあるたびに新しい接続が作成されます。
- **Ajax long polling**: Long Polling では永続的な接続は作成されませんが、サーバーが応答を完了するまで開いたままの要求でサーバーをポーリングします。接続が閉じられたら即座に新しい接続が要求されます。

どの構成でどのようなトランスポートがサポートされているかについての詳細は、[サポートされているプラットフォーム](supported-platforms.md)を参照してください。

### <a name="transport-selection-process"></a>トランスポート選択のプロセス

SignalR が使用するトランスポートを決定する手順を以下に示します。

1. ブラウザーが Internet Explorer 8 またはそれ以前の場合は、Long Polling が使用されます。
2. JSONP が設定されている場合 (つまり、接続が開始されると `jsonp`パラメーターが `true`に設定される)、Long Polling が使用されます。
3. クロス ドメイン接続が確立されている場合 (つまり、SignalR エンドポイントが、ホスティング ページと同じドメインにない)、次の条件が満たされた場合に WebSocket が使用されます。

   - クライアントが CORS (クロス オリジン リソース共有) をサポートしている。CORS をサポートしているクライアントの詳細については、[caniuse.com での CORS](http://www.caniuse.com/CORS) を参照してください。
   - クライアントが WebSocket をサポートしている。
   - サーバーが WebSocket をサポートしている。

     これらの条件のいずれかが満たされていない場合は、Long Polling が使用されます。ドメイン間の接続の詳細については、「[ドメイン間の接続を確立する方法](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)」を参照してください。
4. JSONP が設定されておらず、ドメイン間接続でない場合は WebSocket が使用されます (クライアントとサーバーの両方がサポートしている場合)。
5. クライアントまたはサーバーのいずれかが WebSocket をサポートしていない場合は Server Sent Events が使用されます (利用可能に場合)。
6. Server Sent Events が使用できない場合は、Forever Frame が試されます。
7. Forever Frame が失敗すると、Long Polling が使用されます。

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>トランスポートの監視

ハブでログを有効にし、ブラウザーでコンソール ウィンドウを開くことによって、アプリケーションで使用されているトランスポートを特定できます。

ブラウザーで、ハブのイベントのログ記録を有効にするには、クライアント アプリケーションに、次のコマンドを追加します。

`$.connection.hub.logging = true;`

- Internet Explorer で、F12 キーを押して開発者ツールを開き、[コンソール] タブをクリックします。

    ![Microsoft Internet Explorer でコンソール](introduction-to-signalr/_static/image2.png)
- Chrome では、Ctrl + Shift + J キーを押して、コンソールを開きます。

    ![Google Chrome でコンソール](introduction-to-signalr/_static/image3.png)

コンソールが開いた状態で、ログが有効な場合は、SignalR で利用されているトランスポートを確認することができます。

![Internet explorer が WebSocket トランスポートを示すコンソールします。](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>トランスポートの指定

トランスポートのネゴシエートには一定の時間を要し、クライアントとサーバーの一定量のリソースを使用します。クライアントの機能が分かっている場合は、クライアント接続が開始された時点でトランスポートを指定することができます。次のコード スニペットでは、クラインが他のプロトコルをサポートしていないことが分かっている場合にも使用される、Ajax Long Polling のトランスポートを使用して接続を開始する方法を示しています。

`connection.start({ transport: 'longPolling' });`

クライアントが特定のトランスポートを順番に試行するようにする場合は、フォールバック順序を指定できます。次のコード スニペットは、WebSocket を試し、失敗した場合、Long Polling に直接移行する例を示します。

`connection.start({ transport: ['webSockets','longPolling'] });`

トランスポートを指定する文字列定数の定義は次のとおりです。

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>接続とハブ

SignalR の API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています: 永続的な接続とハブ。

接続は、単一受信者メッセージ、グループ化メッセージ、またはブロードキャスト メッセージを送信するための単純なエンドポイントを表します。固定接続 API (PersistentConnection クラスによって .NET コードで表される) は、SignalR が公開している低レベルの通信プロトコルへの直接アクセスを開発者に提供します。接続の通信モデルは、Windows Communication Foundation などの接続に基づく API を使用したことのある開発者にとって使いやすいものです。

ハブは、クライアントとサーバーが相互に直接メソッドを呼び出すことができるようにする接続 API に基づいて構築されたより高度なパイプラインです。SignalR は、マシンの境界でのディスパッチを魔法のように行い、クライアントがローカルのメソッドのように簡単にサーバーでメソッドを呼び出せるようにします (またはその逆)。ハブ通信モデルは、.NET Remoting などのリモート呼び出し API を使用したことのある開発者にとって使いやすいものです。また、ハブを使用すると、厳密に型指定されたパラメーターをメソッドに渡すことができ、モデルのバインディングが可能になります。

### <a name="architecture-diagram"></a>アーキテクチャ図

次の図は、ハブ、永続的な接続、およびトランスポートに使用される基になるテクノロジ間の関係を示します。

![Api、トランスポート、およびクライアントを示す、SignalR のアーキテクチャ図](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>ハブのしくみ

サーバー側コードでは、クライアントでメソッドを呼び出すと、呼び出されるメソッドの名前とパラメーターを含むパケットがアクティブなトランスポート経由で送信されます (オブジェクトがメソッド パラメーターとして送信される場合、JSON を使用してシリアル化されます)。次にクライアントは、メソッド名をクライアント側のコードで定義されているメソッドと一致させます。一致がある場合、逆シリアル化されたパラメーターのデータを使用してクライアント メソッドが実行されます。

メソッドは、[Fiddler](http://fiddler2.com/) などのツールを使用して監視することができます。次のイメージは、Fiddler の [ログ] ウィンドウで、SignalR サーバーから Web ブラウザー クライアントに送信されたメソッドの呼び出しを示します。メソッドの呼び出しは、`MoveShapeHub` というハブから送信され、呼び出されるメソッドは `updateShape`と呼ばれます。


![SignalR のトラフィックを示す Fiddler ログの表示](introduction-to-signalr/_static/image6.png)

この例では、ハブ名は`H`パラメーターで識別されます。メソッド名は、`M`パラメーターで識別され、メソッドに送信されるデータは`A`パラメーターで識別されます。このメッセージを生成したアプリケーションは、[高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)チュートリアルで作成されます。

### <a name="choosing-a-communication-model"></a>通信モデルの選択

ほとんどのアプリケーションでは、ハブ API を使用する必要があります。接続 API は次の状況で使用できます。

- 実際の送信メッセージの形式を指定する必要がある
- 開発者が、リモート呼び出しモデルではなく、メッセージおよびディスパッチ モデルを利用する事を推奨している
- メッセージングモデルを使用する既存のアプリケーションが SignalR を使用するように移植されている
