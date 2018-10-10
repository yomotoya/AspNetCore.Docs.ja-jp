---
uid: signalr/overview/performance/signalr-performance
title: SignalR パフォーマンス |Microsoft Docs
author: pfletcher
description: SignalR パフォーマンス
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 269c10d7a73f181eaceac1c43ad51f3933d6711c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911861"
---
<a name="signalr-performance"></a>SignalR パフォーマンス
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このトピックでは、ための設計、計測、および SignalR アプリケーションのパフォーマンスを向上する方法について説明します。
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
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。


SignalR パフォーマンスとスケーリングに最近行ったプレゼンテーションは、次を参照してください。 [ASP.NET SignalR によるリアルタイムの Web スケール](https://channel9.msdn.com/Events/Build/2013/3-502)します。

このトピックは、次のセクションで構成されています。

- [設計に関する考慮事項](#design)
- [SignalR サーバーのパフォーマンスのチューニング](#tuning)
- [パフォーマンスの問題のトラブルシューティング](#troubleshooting)
- [SignalR パフォーマンス カウンターの使用](#perfcounters)
- [その他のパフォーマンス カウンターの使用](#othercounters)
- [その他のリソース](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>設計に関する考慮事項

このセクションでは、不要なネットワーク トラフィックを生成することによってパフォーマンスが低下しないことを確認するには SignalR アプリケーションのデザイン時に実装できるパターンについて説明します。

### <a name="throttling-message-frequency"></a>メッセージの頻度を調整

を (リアルタイムのゲーム アプリケーションの場合) などの高頻度でメッセージを送信するアプリケーションであっても、ほとんどのアプリケーションは、1 秒間に複数のメッセージを送信する必要はありません。 各クライアントを生成するトラフィックの量を減らすためには、メッセージ ループを実装するキューおよび送信メッセージありませんより固定レートよりも頻繁に (つまり、メッセージ数まで送信されます 1 秒ごとに、その時点でのメッセージがある場合terval 送信する)。 メッセージを (クライアントとサーバーの両方) から、一定のレートを調整するサンプル アプリケーションの場合、次を参照してください。 [SignalR による高頻度リアルタイム メッセージング](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)します。

### <a name="reducing-message-size"></a>メッセージのサイズを縮小します。

SignalR メッセージのサイズを小さくには、シリアル化されたオブジェクトのサイズを小さきます。 サーバー コードでは、転送する必要がないプロパティを格納しているオブジェクトを送信する場合は妨げられるこれらのプロパティを使用してシリアル化される、`JsonIgnore`属性。 プロパティの名前がメッセージにも格納されています。使用してプロパティの名前を短縮できます、`JsonProperty`属性。 次のコード サンプルでは、プロパティ名を短縮する方法と、クライアントに送信されてからプロパティを除外する方法を示しています。

**.NET サーバー コードから、クライアントに送信されるデータを除外する JsonIgnore 属性とメッセージのサイズを小さく JsonProperty 属性を説明します。**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

読みやすさを維持するために、クライアント コードで保守性、プロパティの省略名は、わかりやすいにマップが変更された名、メッセージが受信されるとします。 次のコード サンプルに示します (マッピング) のメッセージ コントラクトを定義することより長いものを使用する簡略名を再マップの方法の 1 つを使用して、`reMap`最適化されたメッセージ クラスに、コントラクトを適用する関数。

**クライアント側の JavaScript コードを再配置するには、人間が判読できる名前のプロパティ名を短縮します。**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

名前は、同じメソッドを使用しても、サーバーに、クライアントからのメッセージで短縮できます。

(つまり、メッセージに使用されるメモリ量) のメモリ使用量の削減、メッセージのオブジェクトもパフォーマンスが向上します。 たとえば場合は、さまざまな、`int`は必要ありません、`short`または`byte`代わりに使用できます。

メッセージがメッセージのサイズを減らし、サーバーのメモリにメッセージ バス内で格納されるためは、サーバー メモリの問題を解決でこともできます。

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>SignalR サーバーのパフォーマンスのチューニング

SignalR アプリケーションでパフォーマンスの向上のため、サーバーをチューニングするは、次の構成設定を使用できます。 ASP.NET アプリケーションのパフォーマンスを向上させる方法の概要については、次を参照してください。 [ASP.NET パフォーマンスの向上](https://msdn.microsoft.com/library/ff647787.aspx)します。

**SignalR の構成設定**

- **DefaultMessageBufferSize**: 既定では、SignalR は接続ごとにハブあたりのメモリに 1000 メッセージを保持します。 サイズの大きいメッセージを使用している場合は、この値を減らすことで緩和できるメモリの問題を作成この可能性があります。 この設定を設定することができます、`Application_Start`または ASP.NET アプリケーションでのイベント ハンドラー、`Configuration`で自己ホスト型アプリケーションの OWIN startup クラスのメソッド。 次の例では、サーバーのメモリ使用量を削減するために、アプリケーションのメモリ使用量を減らすためにこの値を小さく方法を示しています。

    **既定のメッセージ バッファーのサイズを減らすため、Startup.cs で .NET サーバー コード**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS の構成設定**

- **アプリケーションごとの最大同時要求**: IIS の同時実行の数を増やして、要求で要求を提供するために使用可能なサーバー リソースが増加します。 既定値は 5000 です。この設定を向上させるのに管理者特権でコマンド プロンプトで、次のコマンドを実行します。

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: これは、Http.sys がアプリケーション プールのキューを要求の最大数。 キューには、新しい要求は 503「サービスを利用できません」の応答を受信します。 既定値は 1000 です。

    アプリケーションをホストしているアプリケーション プールでワーカー プロセスのキューの長さを短くと、メモリ リソースを節約します。 詳細については、次を参照してください。[管理、調整、およびアプリケーション プールの構成](https://technet.microsoft.com/library/cc745955.aspx)します。

**ASP.NET 構成の設定**

このセクションにはで設定できる構成設定が含まれています、`aspnet.config`ファイル。 このファイルはプラットフォームに応じて、2 つの場所のいずれかであります。

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

SignalR パフォーマンスを向上させることがあります ASP.NET の設定を以下に示します。

- **CPU ごとの同時要求の最大**: この設定を増やすとパフォーマンスのボトルネックを軽減する可能性があります。 この設定を大きくには、次の構成設定を追加、`aspnet.config`ファイル。

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **要求キューの制限**: 接続の合計数を超える場合、`maxConcurrentRequestsPerCPU`設定すると、ASP.NET は開始キューを使用して要求を調整します。 キューのサイズを増やすを増やすことができます、`requestQueueLimit`設定します。 これを行うには、次の構成設定を追加、`processModel`ノード`config/machine.config`(なく`aspnet.config`)。

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>パフォーマンスの問題のトラブルシューティング

このセクションでは、アプリケーションのパフォーマンスのボトルネックを見つける方法について説明します。

### <a name="verifying-that-websocket-is-being-used"></a>WebSocket が使用されていることを確認します。

SignalR は、さまざまなトランスポートを使用して、クライアントとサーバー間の通信、WebSocket は、パフォーマンスに大きなメリットが提供し、クライアントとサーバーがサポートしている場合に使用する必要があります。 調べるには、クライアントとサーバーが WebSocket の要件を満たしているかどうかは、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)します。 どのようなトランスポートは、アプリケーションで使用されているを確認するのには、ブラウザー開発者ツールを使用し、トランスポートが接続に使用されているログを調べてできます。 Internet Explorer と Chrome ブラウザーの開発ツールを使用する方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)します。

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>SignalR パフォーマンス カウンターの使用

有効にして、SignalR パフォーマンス カウンターを使用する方法を説明にある、`Microsoft.AspNet.SignalR.Utils`パッケージ。

### <a name="installing-signalrexe"></a>Signalr.exe をインストールします。

SignalR.exe というユーティリティを使用してサーバーには、パフォーマンス カウンターを追加できます。 このユーティリティをインストールするには、次の手順を実行します。

1. Visual Studio で、次のように選択します**ツール** > **NuGet パッケージ マネージャー** > **ソリューションの NuGet パッケージの管理。**
2. 検索**signalr.utils**インストールを選択します。

    ![](signalr-performance/_static/image1.png)
3. パッケージをインストールするライセンス条項に同意します。
4. SignalR.exe にインストールされる`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`します。

### <a name="installing-performance-counters-with-signalrexe"></a>SignalR.exe でパフォーマンス カウンターをインストールします。

SignalR パフォーマンス カウンターをインストールするには、次のパラメーターを使用して、管理者特権でコマンド プロンプトで SignalR.exe を実行します。

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

SignalR パフォーマンス カウンターを削除するには、次のパラメーターを使用して、管理者特権でコマンド プロンプトで SignalR.exe を実行します。

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR パフォーマンス カウンター

ユーティリティ パッケージは、次のパフォーマンス カウンターをインストールします。 "Total"カウンターは、最後のアプリケーション プールまたはサーバーを再起動するために、イベントの数を計測します。

**接続メトリック**

次のメトリックは、発生する接続の有効期間イベントを測定します。 詳細については、次を参照してください。[接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)します。

- **接続されている接続**
- **接続の再接続**
- **接続の切断**
- **現在の接続**

**メッセージ メトリックス**

次のメトリックは、SignalR によって生成されるメッセージ トラフィックを測定します。

- **接続メッセージの受信した合計**
- **合計送信接続メッセージ**
- **接続を受信メッセージ数/秒**
- **接続が送信メッセージ/秒**

**メッセージ バスのメトリック**

次のメトリックは、内部の SignalR メッセージ バス、すべての受信および送信の SignalR メッセージの配置のキューを通過するトラフィックを測定します。 メッセージが**Published**が送信またはブロードキャストします。 A**サブスクライバー**メッセージ バス上のサブスクリプションは、このコンテキストでは、クライアントとサーバー自体の数と等しくなります。 **割り当てられたワーカー** ; のアクティブな接続にデータを送信するコンポーネントを**ビジー状態の Worker**がアクティブにメッセージを送信する 1 つです。

- **メッセージ バス メッセージの受信の合計数**
- **メッセージ バス メッセージの受信/秒**
- **メッセージ バスのメッセージの合計を発行します。**
- **メッセージ バス メッセージの 1 秒あたりの発行**
- **メッセージ バスのサブスクライバーの現在**
- **メッセージ バスのサブスクライバーの合計**
- **メッセージ バスのサブスクライバー/秒**
- **メッセージ バスには、ワーカーが割り当てられています。**
- **メッセージ バスのビジー状態のワーカー**
- **メッセージ バスのトピックの現在**

**誤差のメトリック**

次のメトリックは、SignalR メッセージ トラフィックによって生成されたエラーを測定します。 **ハブの解決**ハブまたはハブのメソッドは解決できない場合にエラーが発生します。 **ハブ呼び出し**エラーは、ハブ メソッドの呼び出し中にスローされる例外。 **トランスポート**エラーは、接続エラーの HTTP 要求または応答時にスローされます。

- **エラー: すべての合計**
- **エラー: 1 秒あたりのすべて**
- **エラー: ハブの解像度の合計**
- **エラー: ハブの解像度/秒**
- **エラー: ハブ呼び出し合計**
- **エラー: ハブ呼び出し/秒**
- **エラー: トランスポートの合計**
- **1 秒あたりのトランスポート エラー:**

<a id="scaleout_metrics"></a>

**スケール アウトのメトリック**

次のメトリックは、トラフィックと、スケール アウト プロバイダーによって生成されたエラーを測定します。 A **Stream**このコンテキストでは、スケール ユニットはスケール アウト プロバイダーで使用される。 これは SQL Server を使用する場合は、テーブル、Service Bus を使用する場合は、トピックとサブスクリプション Redis を使用する場合。 各ストリームによって順序付けられた読み取りおよび書き込み操作です。1 つのストリームは、潜在的なスケール ボトルネックは、そのボトルネックを減らすためにストリームの数を増やすことができます。 複数のストリームを使用している場合 SignalR は自動的に配布されている順序で特定の接続から送信されたメッセージを保証する方法でこれらのストリーム間でメッセージの (数シャード) を使用します。

[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)設定では、SignalR で保持されているスケール アウト送信キューの長さを制御します。 値に設定する、構成済みのメッセージング バック プレーンに一度に 1 つずつ送信する送信キューのすべてのメッセージを配置は 0 より大きい。 キューのサイズが構成されている長さを超えた場合では送信する後続の呼び出しがすぐに失敗、 [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx)までキュー内のメッセージの数が少ない設定よりももう一度です。 実装されているバック プレーン一般に、独自のキューまたはフロー制御インプレース、キューが既定で無効にします。 場合は、SQL Server は任意の時点で起こっているの送信メッセージの数の制限接続プールを効果的に。

SQL Server と Redis の既定では、1 つのみのストリームが使用される、Service bus の 5 つのストリームが使用される、キューが無効になっているおよびが SQL サーバーと Service Bus の構成でこれらの設定を変更できます。

**SQL Server のバック プレーンのテーブルの数とキューの長さを構成するための .NET サーバー コード**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**バック プレーンの Service Bus のトピックの数とキューの長さを構成するための .NET サーバー コード**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

A**バッファリング**ストリームが途中終了状態に入った 1 つ。 ストリームが不要になったエラーになるまでにすぐにバック プレーンに送信されるすべてのメッセージはエラー ストリームが途中終了状態のときは、にします。 **Send Queue Length**は送信されているが、まだ送信されるメッセージの数です。

- **スケール アウト メッセージ バス メッセージの受信/秒**
- **スケール アウトのストリームの合計**
- **スケール アウトのストリームを開く**
- **スケール アウト ストリームのバッファリング**
- **スケール アウト エラーの合計**
- **スケール アウト エラー/秒**
- **スケール アウト送信キューの長さ**

これらのカウンターの測定の詳細については、次を参照してください。 [Azure Service Bus による SignalR スケール アウト](scaleout-with-windows-azure-service-bus.md)します。

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>その他のパフォーマンス カウンターの使用

次のパフォーマンス カウンターも、アプリケーションのパフォーマンスの監視に役立つ場合があります。

**メモリ**

- .NET CLR Memory\\ヒープ (w3wp) をすべて # bytes

**ASP.NET**

- ASP.NET\Requests Current
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- プロセッサ時間の Information\Processor

**TCP/IP**

- TCPv6/接続の確立
- TCPv4/接続の確立

**Web サービス**

- Web service \current Connections
- Web Service\Maximum 接続

**スレッド化**

- .NET CLR をロックおよびスレッド\\現在の論理スレッド数
- .NET CLR をロックおよびスレッド\\物理的な現在のスレッドの数

<a id="otherresources"></a>

## <a name="other-resources"></a>その他の参照情報

ASP.NET パフォーマンスの監視とチューニングの詳細については、次のトピックを参照してください。

- [ASP.NET のパフォーマンスの概要](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [IIS 7.5、IIS 7.0、IIS 6.0 で ASP.NET のスレッドの使用状況](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt;要素 (Web 設定)](https://msdn.microsoft.com/library/dd560842.aspx)
