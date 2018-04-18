---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR パフォーマンス (SignalR 1.x) |Microsoft ドキュメント
author: pfletcher
description: SignalR パフォーマンス
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance-signalr-1x"></a>SignalR パフォーマンス (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このトピックでは、用にデザイン、メジャー、および SignalR アプリケーションのパフォーマンスを向上させる方法について説明します。


SignalR パフォーマンスとスケーリングの最近のプレゼンテーションを参照してください。 [ASP.NET SignalR でリアルタイム Web をスケーリング](https://channel9.msdn.com/Events/Build/2013/3-502)です。

このトピックは、次のセクションで構成されています。

- [設計に関する考慮事項](#design)
- [パフォーマンスのため、SignalR のサーバーのチューニング](#tuning)
- [パフォーマンスの問題のトラブルシューティング](#troubleshooting)
- [SignalR パフォーマンス カウンターを使用します。](#perfcounters)
- [他のパフォーマンス カウンターを使用します。](#othercounters)
- [その他のリソース](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>設計に関する考慮事項

このセクションでは、パフォーマンスの不必要なネットワーク トラフィックを生成することによって障害されていないことを確認する SignalR アプリケーションのデザイン時に実装できるパターンについて説明します。

### <a name="throttling-message-frequency"></a>メッセージの頻度を調整

(リアルタイムのゲーム アプリケーション) などの高頻度でメッセージを送信して、アプリケーションであっても、1 秒間に複数のメッセージを送信するほとんどのアプリケーションが不要です。 各クライアントを生成するトラフィックの量を減らすためには、メッセージ ループを実装できますキューおよび送信により頻繁に固定の率よりもメッセージないこと (つまり、最大メッセージ数は送信毎秒では、その期間中にメッセージがある場合terval 送信する)。 サンプル アプリケーション (クライアントとサーバーの両方) から特定のレートを調整してメッセージを次を参照してください。 [SignalR と頻度が高いリアルタイム](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)です。

### <a name="reducing-message-size"></a>メッセージのサイズを縮小します。

SignalR メッセージのサイズを小さくには、シリアル化されたオブジェクトのサイズを小さきます。 サーバー コードを送信する必要のないプロパティを格納するオブジェクトを送信する場合は妨げそれらのプロパティを使用してシリアル化される、`JsonIgnore`属性。 プロパティの名前がメッセージにも格納されています。使用して、プロパティの名前を簡略化、`JsonProperty`属性。 次のコード サンプルでは、プロパティ名を短縮する方法と、クライアントに送信されないプロパティを除外する方法を示しています。

**クライアントに送信されるデータを除外する JsonIgnore 属性とメッセージ サイズを縮小する JsonProperty 属性を示す .NET サーバー コード**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

読みやすさを保持するために、クライアント コードで保守性、省略形のプロパティ名は、わかりやすいに再割り当て名、メッセージの受信後に/です。 次のコード サンプルでは、メッセージ コントラクト (マッピング) を定義することで、長いパスワードを簡略化された名前を再マップの方法の 1 つを使用して、`reMap`最適化されたメッセージ クラスに、コントラクトを適用する関数。

**クライアント側の JavaScript コードを再配置を人間が判読できる名前にプロパティ名を短縮します。**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

名前が同じメソッドを使用して同様に、サーバーにクライアントからのメッセージで短縮されることができます。

メモリ使用量 (つまり、メッセージで使用するメモリ量) の削減、メッセージのオブジェクトもパフォーマンスが向上します。 たとえば場合のすべての範囲、`int`は必要ありません、`short`または`byte`代わりに使用されることができます。

メッセージのサイズを小さくする、サーバーのメモリ内メッセージ バス内でメッセージが格納されているためには、サーバー メモリの問題を解決でこともできます。

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>パフォーマンスのため、SignalR のサーバーのチューニング

次の構成設定は、SignalR アプリケーションでパフォーマンスを向上させるため、サーバーのチューニングに使用できます。 ASP.NET アプリケーションのパフォーマンスを向上させる方法の概要については、次を参照してください。 [ASP.NET のパフォーマンスを向上させる](https://msdn.microsoft.com/library/ff647787.aspx)です。

**SignalR の構成設定**

- **DefaultMessageBufferSize**: 既定では、SignalR には接続ごとのハブあたりのメモリ内の 1000 メッセージが保持されます。 サイズの大きいメッセージを使用している場合は、メモリの問題はこの値を減らすことによって軽減されることができますを作成この可能性があります。 この設定で設定することができます、`Application_Start`や ASP.NET アプリケーションでイベント ハンドラー、`Configuration`自己ホスト型アプリケーションでの OWIN スタートアップ クラスのメソッドです。 次の例では、サーバーのメモリ使用量を削減するために、アプリケーションのメモリ使用量を減らすためにこの値を小さく方法を示します。

    **Global.asax で既定のメッセージ バッファーのサイズを減らすための .NET サーバー コード**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS の構成設定**

- **アプリケーションごとの最大同時要求**: IIS の同時実行の数を増やすと、要求で要求処理の使用可能なサーバー リソースは増加します。 既定値は 5000 です。この設定を増やすには、するには、管理者特権のコマンド プロンプトで次のコマンドを実行します。

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**ASP.NET 構成の設定**

このセクションにはで設定できる構成設定が含まれています、`aspnet.config`ファイル。 このファイルにはプラットフォームによっては、2 つの場所のいずれかであります。

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

SignalR パフォーマンスを向上させることがあります ASP.NET の設定を以下に示します。

- **CPU ごとの最大の同時要求**: この設定を増やすとパフォーマンスのボトルネックを軽減する可能性があります。 この設定を増やすには、するには、次の構成設定を追加、`aspnet.config`ファイル。

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **要求キューの制限**: 接続の合計数を超えた場合、`maxConcurrentRequestsPerCPU`設定すると、ASP.NET は開始キューを使用して要求を調整します。 キューのサイズを増やすに増やすことができます、`requestQueueLimit`設定します。 これを行うには、次の構成設定を追加、`processModel`内のノード`config/machine.config`(なく`aspnet.config`)。

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>パフォーマンスの問題のトラブルシューティング

このセクションでは、アプリケーションのパフォーマンスのボトルネックを検索する方法について説明します。

### <a name="verifying-that-websocket-is-being-used"></a>WebSocket が使用されていることを確認しています

SignalR は、さまざまなトランスポートを使用して、クライアントとサーバー間の通信、WebSocket 大幅なパフォーマンスの利点を提供し、クライアントとサーバーがサポートしている場合に使用する必要があります。 調べるには、クライアントとサーバーが、WebSocket の要件を満たすかどうかは、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)です。 調べるにはどのようなトランスポートは、アプリケーションで使用されて、ブラウザーの開発者ツールを使用し、どのようなトランスポートが接続に使用されているログを調べてできます。 Internet Explorer と Chrome ブラウザーの開発ツールの使用方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)です。

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>SignalR パフォーマンス カウンターを使用します。

このセクションを有効にし、SignalR パフォーマンス カウンターを使用する方法について説明で見つかった、`Microsoft.AspNet.SignalR.Utils`パッケージです。

### <a name="installing-signalrexe"></a>Signalr.exe をインストールします。

パフォーマンス カウンターは、という SignalR.exe ユーティリティを使用してサーバーに追加できます。 このユーティリティをインストールするには、次の手順を実行します。

1. Visual Studio アプリケーションで次のように選択します**ツール**、**ライブラリ パッケージ マネージャー**、 **Manage NuGet Packages for Solution しています...**
2. 検索**signalr.utils**インストール を選択します。

    ![](signalr-performance/_static/image1.png)
3. パッケージをインストールするライセンス契約に同意します。
4. インストールされます SignalR.exe`<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`です。

### <a name="installing-performance-counters-with-signalrexe"></a>SignalR.exe でパフォーマンス カウンターをインストールします。

SignalR パフォーマンス カウンターをインストールするには、次のパラメーターを使用して特権のコマンド プロンプトで SignalR.exe を実行します。

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

SignalR パフォーマンス カウンターを削除するには、次のパラメーターを使用して特権のコマンド プロンプトで SignalR.exe を実行します。

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR パフォーマンス カウンター

ユーティリティ パッケージでは、次のパフォーマンス カウンターをインストールします。 "Total"カウンターは、最後のアプリケーション プールまたはサーバーが再起動した後に、イベントの数を計測します。

**接続のメトリック**

次のメトリックは、接続の有効期間発生するイベントを測定します。 詳細については、次を参照してください。[接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)です。

- **接続されている接続**
- **接続の再接続**
- **接続外して**
- **現在の接続**

**メッセージ メトリックス**

次のメトリックは、SignalR によって生成されるメッセージ トラフィックを測定します。

- **接続のメッセージの受信合計**
- **合計送信接続メッセージ**
- **接続を受信メッセージ数/秒**
- **接続が送信メッセージ/秒**

**メッセージ バスのメトリック**

次のメトリックは、内部の SignalR メッセージ バスは、すべての着信および発信の SignalR メッセージが置かれているキューを通過するトラフィックを測定します。 メッセージが**Published**が送信またはブロードキャストします。 A**サブスクライバー**このコンテキストでメッセージ バス上のサブスクリプションはクライアントとサーバー自体の数と等しくなります。 **割り当てられたワーカー** ; のアクティブな接続にデータを送信するコンポーネント、**ビジー状態のワーカー**は、アクティブにメッセージを送信しています。

- **メッセージ バス メッセージを受信した合計**
- **メッセージ バスのメッセージの受信/秒**
- **メッセージ バスのメッセージの合計を発行します。**
- **メッセージ バスのメッセージの発行/秒**
- **メッセージ バスのサブスクライバーの現在**
- **メッセージ バスのサブスクライバーの合計**
- **メッセージ バスのサブスクライバー/秒**
- **メッセージ バスには、ワーカーが割り当てられています。**
- **メッセージ バスのビジー状態のワーカー**
- **メッセージ バスのトピックの現在**

**誤差のメトリック**

次のメトリックは、SignalR メッセージ トラフィックによって生成されたエラーを測定します。 **ハブの解決**ハブまたはハブのメソッドは解決できない場合にエラーが発生します。 **ハブ呼び出し**エラーは、ハブ メソッドの呼び出し中にスローされる例外。 **トランスポート**エラーは、接続エラーの HTTP 要求または応答の中にスローされます。

- **エラー: すべての合計**
- **エラー: 1 秒あたりのすべて**
- **エラー: ハブ解像度の合計**
- **エラー: ハブ解像度/秒**
- **エラー: ハブ呼び出しの合計**
- **エラー: ハブ呼び出し/秒**
- **エラー: トランスポート合計**
- **1 秒あたりのトランスポートのエラー:**

**スケール アウト メトリック**

次のメトリックは、トラフィックおよびスケール アウト プロバイダーによって生成されたエラーを測定します。 A**ストリーム**このコンテキストでは、スケール アウト プロバイダーによって、スケール ユニットを使用。 これは SQL Server が使用される場合は、テーブル、Service Bus を使用する場合のトピックとサブスクリプション Redis を使用する場合。 既定では、1 つだけのストリームを使用するが、これは、SQL Server と Service Bus の構成を増やすことができます。 A**バッファリング**ストリームが途中終了状態になったものです。 ストリームが不要になったエラーが発生するまでにすぐに、ストリームが途中終了状態、バック プレーンに送信されるすべてのメッセージは失敗します。 **Send Queue Length**送信されているが、まだ送信されるメッセージの数です。

- **スケール アウト メッセージ バスのメッセージの受信/秒**
- **スケール アウト ストリームの合計**
- **スケール アウトのストリームを開く**
- **スケール アウト ストリームのバッファリング**
- **スケール アウト エラーの合計**
- **スケール アウト エラー/秒**
- **スケール アウト送信キューの長さ**

これらのカウンターの測定の詳細については、次を参照してください。 [Azure Service Bus での SignalR スケール アウト](scaleout-with-windows-azure-service-bus.md)です。

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>他のパフォーマンス カウンターを使用します。

次のパフォーマンス カウンターは、アプリケーションのパフォーマンスの監視に役立つ場合もあります。

**メモリ**

- .NET CLR メモリ # ヒープ (w3wp) をすべてのバイト数

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

- .NET CLR LocksAndThreads\#論理スレッドの現在の
- .NET CLR LocksAnd スレッド\#物理スレッドの現在の

<a id="otherresources"></a>

## <a name="other-resources"></a>その他の参照情報

ASP.NET のパフォーマンスの監視とチューニングの詳細については、次のトピックを参照してください。

- [ASP.NET のパフォーマンスの概要](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [IIS 7.5、IIS 7.0 および IIS 6.0 に ASP.NET スレッドの使用状況](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt;要素 (Web 設定)](https://msdn.microsoft.com/library/dd560842.aspx)
