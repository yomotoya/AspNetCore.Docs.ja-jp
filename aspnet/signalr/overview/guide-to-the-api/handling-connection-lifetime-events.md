---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: "理解および SignalR で接続の有効期間イベントの処理 |Microsoft ドキュメント"
author: pfletcher
description: "この記事では、ハブ API によって公開されているイベントを使用する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fd9cafd8d7706807998793c3c39377fe9604266
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>理解および SignalR で接続の有効期間イベントの処理
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

> この記事では、SignalR の接続、再接続、切断イベントを処理することができますをおよび構成できるタイムアウトとキープア ライブの設定の概要を示します。
> 
> アーティクルは、SignalR と接続の有効期間イベントの一部に関する知識があると仮定します。 SignalR の概要については、次を参照してください。 [SignalR の概要](../getting-started/introduction-to-signalr.md)です。 接続の有効期間イベントのリストは、次のリソースを参照してください。
> 
> - [ハブ クラスでの接続の有効期間のイベントを処理する方法](hubs-api-guide-server.md#connectionlifetime)
> - [JavaScript クライアントで接続の有効期間のイベントを処理する方法](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [.NET クライアントで接続の有効期間のイベントを処理する方法](hubs-api-guide-net-client.md#connectionlifetime)
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


## <a name="overview"></a>概要

この記事は、次のセクションで構成されています。

- [接続の有効期間の用語とシナリオ](#terminology)

    - [SignalR 接続、トランスポート接続、および物理的な接続](#signalrvstransport)
    - [トランスポートが切断されたときのシナリオ](#transportdisconnect)
    - [クライアント切断のシナリオ](#clientdisconnect)
    - [サーバーが切断されたときのシナリオ](#serverdisconnect)
- [タイムアウトとキープア ライブの設定](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [タイムアウトとキープア ライブの設定を変更する方法](#changetimeout)
- [接続の切断についてユーザーに通知する方法](#notifydisconnect)
- [継続的に再接続する方法](#continuousreconnect)
- [サーバー コードでクライアントを切断する方法](#disconnectclientfromserver)
- [切断の原因を検出します。](#detectingreasonfordisconnection)

API リファレンス トピックへのリンクは、.NET 4.5 のバージョンの API といます。 .NET 4 を使用している場合は、次を参照してください。[の API のトピックは、.NET 4 バージョン](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>接続の有効期間の用語とシナリオ

`OnReconnected` SignalR ハブ内のイベント ハンドラーがのすぐ後に実行できる`OnConnected`がいない後`OnDisconnected`特定のクライアントです。 再接続、切断されることがなくすることが理由は、SignalR で word「接続」を使用しているいくつかの方法があることです。

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR 接続、トランスポート接続、および物理的な接続

この記事は区別*SignalR 接続*、*トランスポート接続*、および*物理接続*:

- **SignalR 接続**クライアントとサーバーの URL、SignalR の API によって維持され、接続 ID によって一意に識別間に論理リレーションシップを参照 このリレーションシップに関するデータは、SignalR が管理し、トランスポート接続を確立するために使用します。 リレーションシップの end と SignalR 破棄することも、データのクライアントを呼び出すと、`Stop`メソッドまたはタイムアウトの制限に達すると、SignalR が失われるトランスポート接続を再確立しようとしています。
- **トランスポート接続**クライアントとサーバー、4 つのトランスポートの Api のいずれかで保持されている間に論理リレーションシップを指す: Websocket、server によって送信されるイベントが永久にフレーム、または長いポーリングします。 SignalR では、トランスポート API を使用して、トランスポート接続を作成し、トランスポート API がトランスポート接続を作成する物理ネットワーク接続の存在に依存します。 SignalR でが終了するとき、またはトランスポート API は、物理的な接続が切断されたことを検出すると、トランスポート接続が終了します。
- **物理的な接続**参照する物理ネットワーク リンク--ワイヤ、ワイヤレス信号をルーターなど、クライアント コンピューターとサーバー コンピューター間の通信を容易にします。 物理的な接続がトランスポート接続を確立するために存在する必要があります、SignalR の接続を確立するためにトランスポート接続を確立する必要があります。 ただし、物理的な接続の重大なしない常に即座に終了トランスポート接続または SignalR 接続にこのトピックの後半で説明するとおりです。

次の図で SignalR 接続は、ハブ API と PersistentConnection API SignalR レイヤーで表される、トランスポートの接続がトランスポート層で表されるおよび物理的な接続は、サーバー間を結ぶ線によって表されますクライアントとします。

![SignalR のアーキテクチャ図](handling-connection-lifetime-events/_static/image1.png)

呼び出すと、 `Start` SignalR クライアントのメソッドは、サーバーに物理的に接続を確立するために必要なすべての情報と SignalR クライアント コードを提供します。 SignalR クライアント コードでは、この情報を使用して、HTTP 要求を送信し、4 つのトランスポートのいずれかを使用する物理接続を確立します。 トランスポート接続が失敗したか、サーバーが失敗する場合、SignalR 接続しない離れたはすぐに、クライアントがまだ同じ SignalR URL に新しいトランスポート接続を自動的に再確立するために必要な情報です。 このシナリオでは、ユーザー アプリケーションによる介入は必要なく、新しい SignalR 接続が開始しない SignalR クライアント コードでは、新しいトランスポート接続を確立するときにします。 SignalR 接続の連続性は、ファクトに反映されますを呼び出すときに作成される接続 ID、`Start`メソッドでは変更されません。

`OnReconnected`トランスポート接続が失われた後に自動的に再確立されたときに、ハブのイベント ハンドラーが実行されます。 `OnDisconnected` SignalR 接続の最後にイベント ハンドラーを実行します。 SignalR 接続は、次の方法のいずれかで終了できます。

- クライアントが呼び出す場合、`Stop`メソッド、停止メッセージは、サーバーに送信され、クライアントとサーバーの両方、SignalR 接続が即座に終了します。
- クライアントとサーバー間の接続が失われた後にクライアントが再接続して、サーバーが再接続するクライアントの待機です。 再接続の試行が成功、切断のタイムアウト期間の終了する場合は、クライアントとサーバーの両方には、SignalR 接続が終了します。 クライアントが再接続するには試行を中止し、サーバーが、形式の SignalR 接続の破棄します。
- 呼び出す可能性をしなくても実行しているクライアントが停止した場合、`Stop`メソッド、サーバーが再接続するには、クライアントの待機し、切断のタイムアウト期間後、SignalR 接続を終了します。
- かどうか、サーバーが実行を停止、クライアントの再接続しよう (再転送接続) を作成し、切断のタイムアウト期間後、SignalR 接続を終了します。

接続の問題がないと、ユーザーのアプリケーションを呼び出して、SignalR 接続の終了、`Stop`メソッド、SignalR 接続のトランスポート接続を開始および同じ時刻についてで終了します。 次のセクションでは、他のシナリオをさらに詳しく説明します。

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>トランスポートが切断されたときのシナリオ

物理的な接続が遅くなる可能性がまたは接続の中断がある可能性があります。 中断時間の長さなどの要因によってトランスポート接続が削除される可能性があります。 SignalR では、トランスポート接続を再確立が試行されます。 トランスポート接続 API が中断を検出し、トランスポート接続が切断場合がありますを SignalR 検索すぐに接続が失われること。 他のシナリオでトランスポート接続 API も SignalR のどちらも認識はすぐになり接続が失われたことです。 SignalR クライアントが呼び出される関数を使用して、ポーリング時間の長いを除くすべてのトランスポートで*keepalive*トランスポート API が検出できない接続が失われるを確認します。 長いポーリングの接続については、次を参照してください。[タイムアウト設定と keepalive 設定](#timeoutkeepalive)このトピックで後述します。

接続がアクティブでないときに、サーバーはクライアントにキープア ライブ パケットを送信定期的にします。 この記事の内容が書き込まれる時点で、既定の間隔は 10 秒ごと これらのパケットをリッスンしてでは、クライアントが、接続の問題があるかどうかを指示することができます。 キープア ライブ パケットが受信されない場合が予想される場合、しばらくしてから、クライアントが想定動作を遅くしたり中断などの接続に関する問題があること。 Keepalive はまだ受け取っていない場合より長い時間が経過したら、クライアントで、接続が切断されてとの再接続を試みるを開始します。

次の図は、トランスポート API によって認識されないすぐに物理接続に問題があるときに、一般的なシナリオで発生するクライアントとサーバーのイベントを示しています。 図は、次の状況に適用されます。

- トランスポートは、Websocket、永久に枠、またはサーバーによって送信されるイベントがします。
- 物理ネットワーク接続の中断の期間があります。
- トランスポート API にはなりません、中断の対応する SignalR 機能に依存、keepalive がそれらを検出するようにします。

![トランスポートの切断](handling-connection-lifetime-events/_static/image2.png)

クライアントが再接続をモードに入る切断タイムアウト制限内でのトランスポート接続を確立できない場合は、サーバーは、SignalR 接続を終了します。 サーバーが、ハブを実行する場合は、`OnDisconnected`メソッドおよび後で接続するクライアントを管理する場合に、クライアントに送信する切断メッセージをキュー。 接続解除コマンドと呼び出しを受け取る場合は、クライアントは再接続して、`Stop`メソッドです。 このシナリオで`OnReconnected`クライアントが再接続された場合は実行されず`OnDisconnected`クライアントが呼び出しては実行されません`Stop`です。 次の図は、このシナリオを示します。

![トランスポートが中断される - サーバーのタイムアウト](handling-connection-lifetime-events/_static/image3.png)

SignalR 接続の有効期間イベントの中で、クライアントで発生する可能性がありますを次に示します。

- `ConnectionSlow`クライアントのイベントです。

    最後のメッセージから keepalive タイムアウト期間のプリセットの比率が経過または keepalive ping を受信したときに発生します。 Keepalive の既定のタイムアウト警告期間とは、2/3 keepalive タイムアウト時間のです。 Keepalive タイムアウトは 20 秒は約 13 秒で警告が発生したためです。

    既定では、サーバー ping を送信 keepalive、10 秒ごと、keepalive ping 2 秒ごと (キープア ライブのタイムアウト値と keepalive タイムアウト警告値の差の 3 分の 1) は、クライアントを確認します。

    API のトランスポートが切断の対応になった場合は、keepalive タイムアウト警告期間が経過する前に、切断の SignalR を通知する可能性があります。 その場合は、`ConnectionSlow`イベントは発生しませんが、および SignalR に直接移動、`Reconnecting`イベント。
- `Reconnecting`クライアントのイベントです。

    接続が失われた、または (b)、keepalive タイムアウト期間が経過後、最後のメッセージかを keepalive ping を受信しました (a)、トランスポート API が検出したときに発生します。 SignalR クライアント コードでは、再接続を試行を開始します。 トランスポート接続が失われたときに何らかのアクションを実行するアプリケーションの場合は、このイベントを処理することができます。 既定のキープア ライブのタイムアウト期間は 20 秒では現在です。

    クライアント コードでは、SignalR が再接続をモードの間のハブ メソッドを呼び出すしようとして、SignalR は、コマンドを送信しようとします。 ほとんどの場合、このような試行失敗しますが、状況によっては成功する可能性があります。 サーバー送信イベント、永久にフレーム、および長いポーリング トランスポート、SignalR は、いずれかのメッセージを送信するクライアントが使用してメッセージの受信に使用されている 1 つの 2 つの通信チャネルを使用します。 受信に使用するチャネルが完全に開いている 1 つと、物理的な接続が中断されたときに閉じられている 1 つであります。 物理的な接続が復元した場合、クライアントからサーバーへのメソッド呼び出しがあります正常に受信チャネルが再確立される前に、使用可能なままの送信に使用するチャネル。 戻り値は、SignalR が再び受信するために使用するチャネルを開くまで受信しません。
- `Reconnected`クライアントのイベントです。

    トランスポート接続が再確立されると発生します。 `OnReconnected`ハブ内のイベント ハンドラーを実行します。
- `Closed`クライアントのイベント (`disconnected` JavaScript でのイベント)。

    切断のタイムアウト期間は、SignalR クライアント コードがトランスポート接続が切断後に再接続しようとしているときに有効期限が切れるときに発生します。 既定値の切断のタイムアウトは 30 秒です。 (ため、接続の終了時に、このイベントは発生も、`Stop`メソッドが呼び出されます)。

トランスポート API で検出されない、keepalive ping keepalive タイムアウト警告期間よりも長いのサーバーからの受信を遅延しないトランスポート接続の中断は、任意の接続が発生する有効期間イベントを発生させない可能性があります。

一部のネットワーク環境が意図的に、アイドル状態の接続を閉じるし、キープア ライブ パケットの別の関数は、これらのネットワークであること、SignalR 接続で使用することでこれを防ぐためにします。 極端なケースで keepalive ping の既定の頻度できない可能性がありますの接続終了を防ぐために十分な数です。 その場合はより多くの場合、送信される keepalive ping を構成できます。 詳細については、次を参照してください。[タイムアウト設定と keepalive 設定](#timeoutkeepalive)このトピックで後述します。

> [!NOTE] 
> 
> **重要な**: ここで説明されているイベントの順序は保証されません。 SignalR は、このスキームによって予測可能な方法で接続の有効期間のイベントを発生させるすべての試行がネットワーク イベントの多くのバリエーションとトランスポート Api など、基になる通信フレームワークがそれらに対応する多くの方法があります。 たとえば、`Reconnected`クライアントが再接続された場合、イベントを発生しない可能性がまたは`OnConnected`接続を確立する試行が成功すると、サーバー上のハンドラーを実行可能性があります。 このトピックでは、特定の標準的な環境によって通常生成される影響のみを説明します。


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>クライアント切断のシナリオ

ブラウザー クライアントで、SignalR 接続を維持する SignalR クライアント コードは、web ページの JavaScript のコンテキストで実行されます。 ですが、SignalR 接続が終了のいずれかから移動したときに、なぜページ別、およびその 's 理由複数接続がある複数の接続 Id で複数のブラウザー ウィンドウまたはタブから接続する場合。 SignalR クライアント コードでは、そのブラウザー イベントを処理して呼び出しのために、SignalR 接続を直ちに終了、ユーザーは、ブラウザー ウィンドウまたはタブを閉じるで新しいページに移動または、ページを更新、ときに、`Stop`メソッドです。 これらのシナリオで、または任意のクライアント プラットフォーム、アプリケーションを呼び出すときに、`Stop`メソッド、`OnDisconnected`サーバー上のイベント ハンドラーをすぐに実行させ、クライアント、`Closed`イベント (イベントの名前は`disconnected`でJavaScript の場合)。

クライアント アプリケーションまたはで実行されているコンピューターがクラッシュまたは (たとえば、ユーザーの終了時に、ラップトップ) スリープ状態に、変更点について、サーバーは通知されません。 サーバーが認識できる限り、接続の中断により、クライアントが失われる可能性があり、クライアントは再接続を試みている可能性がします。 により、クライアント、再接続するには、サーバーが待機するようなシナリオではそのため、および`OnDisconnected`切断タイムアウト期間 (既定では約 30 秒) が経過するまでは実行されません。 次の図は、このシナリオを示します。

![クライアント コンピューターに失敗しました](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>サーバーが切断されたときのシナリオ

サーバーがオフラインになると--、再起動、失敗すると、アプリケーション ドメインは、リサイクルなど、結果が失われた接続のような可能性がありますまたはトランスポート API および SignalR 可能性があることを認識すぐに、サーバーがなくなり、SignalR が可能性がありますを開始せずに再接続しようとしています。させると、`ConnectionSlow`イベント。 クライアントが再接続をモードになる場合と、サーバーを復旧または再起動や、新しいサーバーがオンラインに切断タイムアウト期間の有効期限が切れる前に、クライアントを復元または新しいサーバーに再接続します。 クライアントの SignalR 接続を継続する場合は、および`Reconnected`イベントが発生します。 最初のサーバーで`OnDisconnected`が実行されることと、新しいサーバーで`OnReconnected`が実行される`OnConnected`前にそのサーバーにそのクライアントに対して実行されたことはありません。 (影響は前回の接続のアクティビティのメモリがない場合は、クライアントが再接続する同じサーバーに、再起動またはアプリケーション ドメインのリサイクル後、サーバーが再起動し、同じ)。次の図は、トランスポート API がすぐに、接続が切断された対応が前提としています。 そのため、`ConnectionSlow`イベントは発生しません。

![サーバーの障害と再接続](handling-connection-lifetime-events/_static/image5.png)

サーバーが利用可能にならない切断タイムアウト期間内で、SignalR 接続が終了します。 このシナリオでは、`Closed`イベント (`disconnected` JavaScript クライアントで) がクライアントで発生しますが、`OnDisconnected`サーバーで呼び出されることはありません。 次の図は、トランスポート API はならないこと、接続が切断された対応 SignalR keepalive 機能によって検出されたために前提としています。 および`ConnectionSlow`イベントが発生します。

![サーバーの障害とタイムアウト](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>タイムアウトとキープア ライブの設定

既定値`ConnectionTimeout`、 `DisconnectTimeout`、および`KeepAlive`値はほとんどのシナリオに適していますが、環境内に特別なニーズに応じて変更できます。 たとえば、ネットワーク環境では、5 秒間アイドル状態になって接続が閉じ、keepalive 値を小さく必要があります。

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

この設定では、オープンで終了して、新しい接続を開く前に応答を待機しているトランスポート接続のままにする時間を表します。 既定値は、110 秒です。

この設定では、のみ keepalive 機能は無効にすると、通常、時間の長いにのみ適用されますが適用されます。 トランスポートをポーリングします。 次の図は、時間の長いでこの設定の効果のトランスポート接続をポーリングします。

![長いポーリングのトランスポートの接続](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

この設定は、発生させる前にトランスポート接続が失われた後に待機する時間を表す、`Disconnected`イベント。 既定値は 30 秒です。 設定すると`DisconnectTimeout`、`KeepAlive`の 1/3 に自動的に設定されている、`DisconnectTimeout`値。

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

この設定では、アイドル状態の接続でキープア ライブ パケットを送信する前に待機する時間を表します。 既定値は、10 秒です。 この値が複数の 1/3 のすることはできません、`DisconnectTimeout`値。

両方を設定する場合は、`DisconnectTimeout`と`KeepAlive`設定、`KeepAlive`後`DisconnectTimeout`です。 それ以外の場合、`KeepAlive`設定が上書きされる場合`DisconnectTimeout`が自動的に設定`KeepAlive`タイムアウト値の 1/3 にします。

Keepalive の機能を無効にする場合は、設定`KeepAlive`を null にします。 Keepalive 機能は使用できません自動的に、時間の長いトランスポートをポーリングします。

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>タイムアウトとキープア ライブの設定を変更する方法

これらの設定の既定値を変更するで設定`Application_Start`で、 *Global.asax*ファイルを次の例で示すようにします。 サンプル コードに示すように値は、既定値と同じです。

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>接続の切断についてユーザーに通知する方法

一部のアプリケーションでは、接続の問題がある場合に、ユーザーにメッセージを表示することができます。 これを行う方法に関するいくつかのオプションとがあります。 生成されたプロキシを使用して、JavaScript クライアントは、次のコード サンプルです。

- 処理、`connectionSlow`再接続モードに入る前に、SignalR は接続の問題を認識するとすぐにメッセージを表示するイベントです。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- 処理、 `reconnecting` SignalR が切断を認識しており、再接続をモードに入るはときにメッセージを表示するイベントです。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- 処理、`disconnected`と再接続を試行してメッセージを表示するイベントがタイムアウトしました。呼び出すことによって、SignalR 接続を再起動するサーバーとの接続が再度を再確立する唯一の方法は、このシナリオでは、`Start`メソッドで、新しい接続 ID が作成されます 次のコード例では、フラグを使用して呼び出すことによって発生 SignalR 接続への通常の終了後、再接続するタイムアウトした場合にのみ通知を発行することを確認してください、`Stop`メソッドです。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>継続的に再接続する方法

一部のアプリケーションでは、自動的に失われ、再接続の試行がタイムアウトした後に、接続を再確立することができます。実行するに呼び出せる、`Start`メソッドから、`Closed`イベント ハンドラー (`disconnected` JavaScript クライアントにイベント ハンドラー)。 呼び出す前に期間を待機することができます`Start`もこれを回避するために頻繁に場合、サーバーまたは物理的な接続は使用できません。 次のコード サンプルは、生成されたプロキシを使用して、JavaScript クライアントによってです。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

モバイル クライアントで認識する潜在的な問題は、継続的な再接続試行サーバーまたは物理的な接続を利用できない場合に、不要なバッテリ ドレイン可能性があります。

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>サーバー コードでクライアントを切断する方法

SignalR バージョン 2 のクライアントを切断するための組み込みのサーバー API ではありません。 ある[今後、この機能を追加するためのプラン](https://github.com/SignalR/SignalR/issues/2101)です。 SignalR の現在のリリースでは、サーバーからクライアントを切断する最も簡単な方法は、クライアントで disconnect メソッドを実装し、サーバーからそのメソッドを呼び出すにです。 次のコード サンプルでは、生成されたプロキシを使用して、JavaScript クライアントの disconnect メソッドを示します。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> セキュリティ - も、どちらものクライアントを切断するのには、このメソッドも、提案された組み込み API により対応クライアントが再接続でしたかハッキングされたコードを削除する場合がありますので、悪意のあるコードを実行しているハッキングされたクライアントのシナリオ、`stopClient`メソッドまたは変更動作します。 ステートフルなサービス拒否 (DOS) の保護を実装する適切な場所がではなく、フレームワーク、またはサーバー層のフロント エンド インフラストラクチャではなく、します。


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>切断の原因を検出します。

SignalR 2.1 では、オーバー ロードをサーバーに追加`OnDisconnect`タイムアウトではなく、クライアントが意図的に切断されたかどうかを示すイベントです。`StopCalled`パラメーターが true の場合は、クライアントが接続を明示的に終了します。 JavaScript では、サーバー エラーの原因、クライアントを切断する場合、エラー情報が渡されますとしてクライアントに`$.connection.hub.lastError`です。

**C# サーバー コード:`stopCalled`パラメーター**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**JavaScript クライアント コード: にアクセスする`lastError`で、`disconnect`イベント。**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
