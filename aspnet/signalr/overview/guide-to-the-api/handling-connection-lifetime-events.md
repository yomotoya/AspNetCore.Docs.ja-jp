---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: 理解と SignalR で接続の有効期間イベントの処理 |Microsoft Docs
author: pfletcher
description: この記事では、ハブ API によって公開されるイベントを使用する方法について説明します。
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 6a354179a82eba1d4a64184bfdeb302472fabf5f
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287983"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>理解と SignalR で接続の有効期間イベントの処理
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> この記事では、SignalR 接続や再接続、切断イベントを処理することができますとタイムアウトとキープア ライブの設定を構成することができますの概要を示します。
>
> この記事では、SignalR と接続の有効期間イベントの一部に関する知識がある前提としています。 SignalR の概要については、次を参照してください。 [SignalR 入門](../getting-started/introduction-to-signalr.md)します。 接続の有効期間イベントのリストは、次のリソースを参照してください。
>
> - [ハブ クラスでの接続の有効期間イベントを処理する方法](hubs-api-guide-server.md#connectionlifetime)
> - [JavaScript クライアントでの接続の有効期間イベントを処理する方法](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [.NET クライアントでの接続の有効期間イベントを処理する方法](hubs-api-guide-net-client.md#connectionlifetime)
>
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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
> このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。 チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。

## <a name="overview"></a>概要

この記事は、次のセクションで構成されています。

- [接続の有効期間の用語とシナリオ](#terminology)

    - [SignalR 接続、トランスポートの接続、および物理的な接続](#signalrvstransport)
    - [トランスポート切断シナリオ](#transportdisconnect)
    - [クライアントが切断されたときのシナリオ](#clientdisconnect)
    - [サーバーが切断されたときのシナリオ](#serverdisconnect)
- [タイムアウトとキープア ライブ設定](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [タイムアウトとキープア ライブ設定を変更する方法](#changetimeout)
- [接続の切断についてユーザーに通知する方法](#notifydisconnect)
- [継続的に再接続する方法](#continuousreconnect)
- [サーバー コードでクライアントを切断する方法](#disconnectclientfromserver)
- [切断の原因を検出します。](#detectingreasonfordisconnection)

.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。 .NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>接続の有効期間の用語とシナリオ

`OnReconnected` SignalR ハブのイベント ハンドラーは、すぐ後に実行できる`OnConnected`がいない後`OnDisconnected`指定のクライアント。 再接続、切断されることがなくすることが理由では、「接続」という単語が SignalR で使用されているいくつかの方法があることです。

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR 接続、トランスポートの接続、および物理的な接続

この記事では区別*SignalR 接続*、*トランスポート接続*、および*物理接続*:

- **SignalR 接続**クライアントとサーバーの URL、SignalR の API によって管理され、接続の ID によって一意に識別間に論理リレーションシップを参照 このリレーションシップに関するデータは、SignalR が管理し、トランスポート接続を確立するために使用します。 クライアントを呼び出すときのデータのリレーションシップの end と SignalR が破棄、`Stop`メソッドまたはタイムアウト制限に達すると、SignalR が失われるトランスポート接続を再確立しようとしています。
- **トランスポート接続**クライアントと管理 Api の 4 つのトランスポートのいずれかによって、サーバーの間の論理リレーションシップを参照します。Websocket、サーバーによって送信されるイベント、無限のフレームまたはポーリング時間の長い。 SignalR はトランスポート API を使用してトランスポート接続を作成して、トランスポートの API がトランスポート接続を作成する物理ネットワーク接続の存在に依存します。 トランスポート接続は、SignalR でが終了するとき、またはトランスポート API は、物理的な接続が切断されたことを検出したときに終了します。
- **物理的な接続**を指す、物理ネットワークへのリンク - ワイヤ、ワイヤレス信号をルーター、- などをクライアント コンピューターとサーバー コンピューター間の通信を容易にします。 物理的な接続のトランスポート接続を確立するために存在する必要があり、SignalR の接続を確立するためにトランスポート接続を確立する必要があります。 ただし、互換性に影響する物理接続は常に即座に終了トランスポート接続や、SignalR 接続このトピックの後半で説明するようです。

次の図で SignalR 接続は、ハブの API と PersistentConnection API SignalR レイヤーで表されるトランスポート接続は、トランスポート レイヤーで表され、物理接続は、サーバー間の線で表されますクライアントとします。

![SignalR のアーキテクチャ図](handling-connection-lifetime-events/_static/image1.png)

呼び出すと、`Start`メソッド SignalR クライアントで、サーバーへの物理接続を確立するために必要なすべての情報を SignalR クライアント コードが提供されます。 SignalR クライアント コードでは、この情報を使用して、HTTP 要求を作成し、4 つのトランスポートのいずれかを使用する物理接続を確立します。 トランスポート接続が失敗したか、サーバーが失敗した場合、SignalR 接続は離れたはすぐに、クライアントがまだ SignalR のと同じ URL への新しいトランスポート接続を自動的に再確立するために必要な情報。 このシナリオでは、ユーザー アプリケーションによる介入は必要ありませんし、SignalR クライアント コードでは、新しいトランスポート接続を確立するときに新しい SignalR 接続は開始されません。 SignalR 接続の継続性は、ファクトに反映されますを呼び出すときに作成される接続 ID、`Start`メソッドでは変更されません。

`OnReconnected`紛失になった後のトランスポート接続が自動的に再確立されたときに、ハブにイベント ハンドラーが実行されます。 `OnDisconnected` SignalR 接続の最後にイベント ハンドラーを実行します。 SignalR 接続は、次の方法のいずれかで終了できます。

- クライアントを呼び出す場合、`Stop`メソッド、停止メッセージは、サーバーに送信され、クライアントとサーバーの両方、SignalR 接続がすぐに終了します。
- クライアントとサーバー間の接続が失われた後、クライアントが再接続を試行し、サーバーが再接続するクライアントの待機します。 再接続の試行が失敗することと、切断のタイムアウト期間の終了する場合、クライアントとサーバーの両方に SignalR 接続が終了します。 クライアントが再接続するには中止し、サーバーは、SignalR 接続の表現の破棄します。
- クライアントを呼び出す機会をしなくても実行が停止した場合、`Stop`メソッド、再接続するには、クライアントのサーバーが待機し、切断のタイムアウト期間後、SignalR 接続を終了します。
- かどうか、サーバーが実行を停止、クライアントの再接続しよう (再トランスポート接続) を作成し、切断のタイムアウト期間後、SignalR 接続を終了します。

接続の問題がないと、ユーザーのアプリケーションを呼び出して、SignalR 接続の終了、`Stop`メソッド、SignalR 接続とトランスポート接続を開始し、ほぼ同時に終了します。 次のセクションでは、他のシナリオをさらに詳しくについて説明します。

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>トランスポート切断シナリオ

物理的な接続が遅くなるまたは接続の中断がある可能性があります。 中断時間の長さなどの要因によってトランスポート接続が削除される可能性があります。 SignalR はトランスポート接続を再確立を試みます。 トランスポート接続 API が場合があります、中断を検出し、トランスポートの接続が切断し、SignalR がすぐに、接続が失われることです。 他のシナリオでのトランスポート接続 API も、SignalR 認識はすぐになり接続が失われたこと。 SignalR クライアントが呼び出される関数を使用して、ロング ポーリングを除くすべてのトランスポートで*keepalive*トランスポート API が検出できない接続が失われるを確認します。 時間の長いポーリングの接続については、次を参照してください。[タイムアウトとキープア ライブ設定](#timeoutkeepalive)このトピックで後述します。

接続がアクティブでない場合、サーバーはクライアントにキープア ライブ パケットを送信定期的にします。 この記事では、書き込まれる時点で、既定の頻度が 10 秒ごとにできます。 これらのパケットをリッスンして、クライアントは接続の問題があるかどうかを指示することができます。 キープア ライブ パケットが受信されない場合予想される場合、短期間のうちに、クライアントが想定パフォーマンスの低下や中断などの接続に関する問題があること。 長い時間が経過したら、keepalive が受信されないままの場合、クライアントで、接続が切断されてと再接続しようとしていますが開始されます。

次の図は、トランスポート API によって認識されないすぐに物理接続に問題が発生する一般的なシナリオで発生するクライアントとサーバーのイベントを示します。 図は、次の状況に適用されます。

- トランスポートは、Websocket、永久にフレームまたはサーバー送信イベントには。
- 物理ネットワーク接続の中断の期間があります。
- SignalR は、それらを検出するためにキープア ライブ機能に依存するようにはトランスポート API は、中断の認識なりません。

![トランスポートの切断](handling-connection-lifetime-events/_static/image2.png)

クライアントが再接続するモードになる切断のタイムアウト制限内でのトランスポート接続を確立できない場合は、サーバーには、SignalR 接続が終了します。 サーバーが、ハブを実行する場合は、`OnDisconnected`メソッドおよび接続解除の後で接続するクライアントを管理する場合に、クライアントに送信するメッセージをキューします。 接続解除コマンドと呼び出し場合は、クライアント、再接続は、受信、`Stop`メソッド。 このシナリオで`OnReconnected`クライアントが再接続するときは実行されず`OnDisconnected`クライアントを呼び出すときは実行されません`Stop`します。 次の図は、このシナリオを示します。

![トランスポートの中断 - サーバーのタイムアウト](handling-connection-lifetime-events/_static/image3.png)

SignalR 接続の有効期間イベント、クライアントで発生する可能性がありますを次に示します。

- `ConnectionSlow` クライアントのイベントです。

    Keepalive タイムアウト期間のプリセットの比率は、最後のメッセージから経過したか、キープア ライブの ping を受信したときに発生します。 キープア ライブの既定のタイムアウト警告期間は、keepalive タイムアウトの 2/3 です。 Keepalive タイムアウトは、約 13 秒で警告が発生したために、20 秒が。

    既定では、10 秒ごとにキープア ライブの ping を送信するサーバーと、クライアントは keepalive ping (キープア ライブのタイムアウト値と、キープア ライブのタイムアウト警告値の差の 3 分の 1) 2 秒ごとの詳細について確認します。

    API のトランスポートが切断の対応になると、keepalive タイムアウト警告が渡される前に SignalR は切断の通知可能性があります。 その場合は、`ConnectionSlow`イベントは発生しませんが、SignalR に直接移動して、`Reconnecting`イベント。
- `Reconnecting` クライアントのイベントです。

    (A)、トランスポート API 検出を接続が失われた、または最後のメッセージから (b)、キープア ライブのタイムアウト期間が経過または keepalive ping を受信したときに発生します。 SignalR クライアント コードでは、再接続を試行を開始します。 トランスポート接続が失われた場合に、何らかのアクションを実行するアプリケーションの場合は、このイベントを処理することができます。 キープア ライブの既定のタイムアウト期間は、20 秒では現在です。

    クライアント コードでは、SignalR がモードの再接続の間のハブ メソッドを呼び出すしようとして、SignalR は、コマンドを送信しようとします。 ほとんどの場合は、このような試行は失敗しますが、状況によっては成功する可能性があります。 サーバー送信イベント、永久にフレーム、および時間の長いポーリング トランスポート、SignalR は、いずれかのメッセージを受信するために使用して、クライアントを使用してメッセージを送信する 1 つの 2 つの通信チャネルを使用します。 受信するために使用されるチャネルは完全にオープンの 1 つと、物理的な接続が中断されたときに閉じられている 1 つです。 あるため受信チャネルが再確立される前に成功した可能性がありますがクライアントからサーバーへのメソッド呼び出しで物理的な接続が復元された場合に、使用可能なままの送信に使用するチャネル。 戻り値は、SignalR は、受信するために使用されるチャネルを再度開くまで受信できません。
- `Reconnected` クライアントのイベントです。

    トランスポート接続が再度確立されたときに発生します。 `OnReconnected`ハブでイベント ハンドラーを実行します。
- `Closed` クライアント イベント (`disconnected` JavaScript でのイベント)。

    切断のタイムアウト期間は、SignalR クライアント コードがトランスポート接続の切断後に再接続しようとしたときに有効期限が切れるときに発生します。 切断の既定のタイムアウトは 30 秒です。 (接続のため、終了時にこのイベントは発生も、`Stop`メソッドが呼び出されます)。

トランスポート API で検出されない、keepalive ping キープア ライブのタイムアウト警告期間よりも長くサーバーからの受信を延期しないトランスポート接続の中断は、任意の接続が発生します。 有効期間イベントを発生させないことがあります。

一部のネットワーク環境は意図的に、アイドル状態の接続を閉じるし、キープア ライブ パケットの別の関数は、これらのネットワークは、SignalR の接続が使用されているを知ることによってこれを防ぐためにします。 極端なケースで keepalive ping の既定の頻度が十分でない閉じられた接続を防ぐため。 その場合はより多くの場合、送信される ping をキープア ライブを構成できます。 詳細については、次を参照してください。[タイムアウトとキープア ライブ設定](#timeoutkeepalive)このトピックで後述します。

> [!NOTE]
>
> **重要な**:ここで説明するイベントの順序は保証されません。 SignalR は、このスキームに従って、予測可能な方法で接続の有効期間イベントを発生させるすべての試行が、ネットワーク イベントの多くのバリエーションとトランスポート Api などの基になる通信フレームワークがそれらに対応する多くの方法があります。 たとえば、`Reconnected`クライアントが再接続するとき、イベントが発生しないまたは`OnConnected`接続を確立する試行が成功すると、サーバー上のハンドラーが実行可能性があります。 このトピックでは、特定の一般的な状況によって通常生成される効果のみについて説明します。


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>クライアントが切断されたときのシナリオ

ブラウザー クライアントでは、SignalR の接続を維持する SignalR クライアント コードは、web ページの JavaScript のコンテキストで実行されます。 ですが SignalR 接続が終了のいずれかから移動したときに、なぜページ別、およびその 's なぜ複数接続がある複数の接続 Id で複数のブラウザー ウィンドウまたはタブから接続する場合。 SignalR クライアント コードでは、そのブラウザーのイベントを処理して、呼び出しのために、SignalR 接続をすぐに終了、ユーザーは、ブラウザーのウィンドウまたはタブを閉じますまたは新しいページに移動するか、ページを更新、ときに、`Stop`メソッド。 これらのシナリオで、または任意のクライアント プラットフォーム、アプリケーションを呼び出すときに、`Stop`メソッド、`OnDisconnected`イベント ハンドラーは、サーバーですぐに実行して、クライアント、`Closed`イベント (イベントの名前は`disconnected`でJavaScript の場合)。

クライアント アプリケーションまたはで実行されているコンピューターがクラッシュまたはスリープ モードになる (たとえば、ユーザーがラップトップを閉じる)、サーバーが発生した事象に関する通知されません。 サーバーが知っている限り、接続の中断により、クライアントが失われる可能性があり、クライアントが再接続を試行する可能性があります。 クライアントに再接続する機会を提供する、サーバーが待機するこれらのシナリオでそのため、および`OnDisconnected`切断のタイムアウト期間 (既定では約 30 秒) が経過するまでは実行されません。 次の図は、このシナリオを示します。

![クライアント コンピューターの障害](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>サーバーが切断されたときのシナリオ

サーバーがオフラインになると--再起動、失敗した場合、アプリ ドメインのリサイクルなど、結果は、接続が失われましたのような可能性がありますまたはトランスポート API と SignalR 可能性がありますすぐにわかること、サーバーがないためとなしの再接続を試みる SignalR が始まります発生させる、`ConnectionSlow`イベント。 クライアントが再接続するモードに移動し、サーバーの復旧または再起動や、新しいサーバーがオンラインになる切断のタイムアウト期間の有効期限が切れる前に場合は、クライアントは、復元、または新しいサーバーに再接続します。 その場合は、SignalR 接続はクライアントの継続と`Reconnected`イベントが発生します。 最初のサーバーで`OnDisconnected`は決して実行と、新しいサーバー`OnReconnected`が実行される`OnConnected`前に、そのサーバーにクライアントが、実行されなかった。 (効果は同じ場合であるため、サーバーが再起動し、同じサーバーにクライアントが再起動またはアプリのドメイン リサイクルでは後、再接続の前回の接続のアクティビティのメモリがありません)。次の図は、トランスポート API がすぐに、接続が失われましたの対応が前提としています。 そのため、`ConnectionSlow`イベントは発生しません。

![サーバーの障害と再接続](handling-connection-lifetime-events/_static/image5.png)

サーバーは、切断のタイムアウト期間内で使用可能なならない、SignalR 接続は終了します。 このシナリオで、`Closed`イベント (`disconnected` JavaScript クライアントでは) は、クライアントで発生しますが、`OnDisconnected`サーバーで呼び出されることはありません。 次の図は、トランスポート API はならないこと、接続が切断されたに注意してください SignalR keepalive 機能によって検出されたため、前提としています。 および`ConnectionSlow`イベントが発生します。

![サーバーの障害とタイムアウト](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>タイムアウトとキープア ライブ設定

既定の`ConnectionTimeout`、 `DisconnectTimeout`、および`KeepAlive`値は、ほとんどのシナリオに適した、環境内に特別なニーズに変更することができます。 たとえば、ネットワーク環境では、5 秒間アイドル状態になって接続が閉じ、キープア ライブ値を小さく必要があります。

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

この設定は、オープンで終了して、新しい接続を開く前に応答を待機しているトランスポート接続のままにする時間数を表します。 既定値は、110 秒です。

この設定は適用のみ keepalive 機能は無効にした場合、通常、時間の長いにのみ適用されるトランスポートをポーリングします。 次の図は、時間の長いでこの設定の効果のトランスポート接続をポーリングします。

![時間の長いポーリングのトランスポートの接続](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

この設定から発生する前にトランスポート接続が失われるまで待機する時間を表す、`Disconnected`イベント。 既定値は 30 秒です。 設定すると`DisconnectTimeout`、`KeepAlive`の 1/3 に自動的に設定されて、`DisconnectTimeout`値。

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

この設定は、アイドル状態の接続経由でキープア ライブ パケットを送信する前に待機する時間を表します。 既定値は、10 秒です。 この値はの複数の 1/3 をすることはできません、`DisconnectTimeout`値。

両方を設定する場合`DisconnectTimeout`と`KeepAlive`設定`KeepAlive`後`DisconnectTimeout`します。 それ以外の場合、`KeepAlive`設定が上書きされるときに`DisconnectTimeout`が自動的に設定`KeepAlive`タイムアウト値の 1/3 にします。

キープア ライブの機能を無効にする場合は、設定`KeepAlive`を null にします。 Keepalive 機能は使用できませんに自動的に、時間の長いトランスポートをポーリングします。

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>タイムアウトとキープア ライブ設定を変更する方法

設定すると、これらの設定の既定値を変更するには、`Application_Start`で、 *Global.asax*ファイルを次の例に示すようにします。 サンプル コードに表示される値は、既定値の場合と同じです。

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>接続の切断についてユーザーに通知する方法

一部のアプリケーション接続の問題がある場合、ユーザーにメッセージを表示する場合があります。 これを行う方法のいくつかのオプションとがあります。 次のコード サンプルでは、生成されたプロキシを使用して JavaScript クライアント用です。

- 処理、`connectionSlow`再接続モードに入る前に、SignalR は接続の問題を認識するとすぐにメッセージを表示するイベントです。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- 処理、 `reconnecting` SignalR は切断を認識し、再接続モードに入って、ときに、メッセージを表示するイベントです。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- 処理、`disconnected`再接続しようとすると、ときにメッセージを表示するイベントがタイムアウトしました。このシナリオでもう一度サーバーとの接続を再確立する唯一の方法は、呼び出すことによって、SignalR 接続を`Start`メソッドで、新しい接続 ID が作成されます 次のコード サンプルは、SignalR の接続を呼び出すことによって発生する通常の終了後の再接続のタイムアウト後にのみ通知を発行することを確認する、フラグを使用して、`Stop`メソッド。

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>継続的に再接続する方法

一部のアプリケーションに自動的に失われ、再接続を試行がタイムアウトした後に接続を再確立する可能性があります。そのために呼び出すことができます、`Start`からメソッド、`Closed`イベント ハンドラー (`disconnected` JavaScript クライアントでのイベント ハンドラー)。 呼び出しの前に、期間を待機する`Start`もこれを回避するために頻繁に、サーバーまたは物理的な接続が使用できない場合。 次のコード サンプルは、生成されたプロキシを使用して JavaScript クライアントによってです。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

モバイル クライアントで注意すべき潜在的な問題は、サーバーまたは物理接続を利用できない場合、継続的な再接続の試行には、不要なバッテリの消耗可能性があります。

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>サーバー コードでクライアントを切断する方法

SignalR 2 のバージョンのクライアントを切断するための組み込みのサーバー API ではありません。 ある[、今後この機能を追加するためのプラン](https://github.com/SignalR/SignalR/issues/2101)します。 SignalR の現在のリリースで、サーバーからクライアントを切断する最も簡単な方法は、クライアントの切断メソッドを実装し、サーバーからそのメソッドを呼び出すには。 次のコード サンプルでは、生成されたプロキシを使用して JavaScript クライアントを切断メソッドを示します。

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> セキュリティ - クライアントを切断するのには、このメソッドも、提案された組み込みの API はアドレス、クライアントが再接続でした、ハッキングされたコードが削除または悪意のあるコードを実行しているハッキングされたクライアントのシナリオ、`stopClient`メソッドまたは変更動作します。 ステートフルなサービス拒否 (DOS) の保護を実装するために適切な場所は、フレームワークや、サーバー層ではなく、フロント エンドのインフラストラクチャではなくです。


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>切断の原因を検出します。

SignalR 2.1 は、オーバー ロードをサーバーに追加されます。`OnDisconnect`タイムアウトではなく、クライアントが意図的に切断された場合を示すイベントです。`StopCalled`パラメーターが true の場合は、クライアントが接続を明示的に終了します。 JavaScript では、サーバー エラーの原因、クライアントを切断する場合、エラー情報が渡されますとしてクライアントに`$.connection.hub.lastError`します。

**サーバーのコード (C#):`stopCalled`パラメーター**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**JavaScript クライアント コード: にアクセスする`lastError`で、`disconnect`イベント。**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
