---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: SignalR 接続密度テスト Crank による |Microsoft Docs
author: Rick-Anderson
description: SignalR 接続密度 Crank によるテスト
ms.author: riande
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 556accb1bcc18e9e4d1f813a87fc6f4b67bda088
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021483"
---
<a name="signalr-connection-density-testing-with-crank"></a>SignalR 接続密度 Crank によるテスト
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Crank ツールを使用して、複数のシミュレートされたクライアント アプリケーションをテストする方法について説明します。


(いずれか、Azure web ロール、IIS、またはセルフホステッド Owin を使用して) そのホスティング環境でアプリケーションを実行すると、Crank ツールを使用して接続密度の高いレベルへのアプリケーションの応答をテストできます。 ホスティング環境、インターネット インフォメーション サービス (IIS) サーバー、Owin ホスト、または Azure web ロールを使用できます。 (注: 接続密度テストからパフォーマンス データを取得することはできませんので、パフォーマンス カウンターを Azure App Service の Web Apps で使用できることはできません)。

接続の密度は、サーバー上に確立できる同時 TCP 接続の数を示します。 TCP 接続ごとに独自のオーバーヘッドが発生し、アイドル接続の数が多いを開くと、メモリのボトルネックは最終的に作成します。

[SignalR のコードベース](https://github.com/signalr/signalr)という名前のロード テスト ツールが含まれています**Crank**します。 Crank の最新バージョンが記載されて[Dev 分岐](https://github.com/SignalR/signalr/tree/dev)GitHub でします。 SignalR の開発ブランチのアーカイブのコードベースを Zip をダウンロードする[ここ](https://github.com/SignalR/SignalR/archive/dev.zip)します。

Crank を完全にサーバーのメモリが飽和状態にサーバー ハードウェアに可能なアイドル状態の接続の合計数を計算するためにことができます。 または、使用すること可能性がありますも Crank ロード テストを一定の量、メモリ負荷の下で、サーバーで特定の数または特定のメモリしきい値に達するまで接続知識を深めます。

をテストする場合は、リモート クライアントを使用して、リソース (つまり、TCP 接続とメモリ) の競合を回避する必要があります。 完全な容量 (メモリまたは CPU) に到達できないサーバーを妨げる可能性のあるボトルネック、達していないことを確認するクライアントを監視します。 サーバーを完全に読み込むためにクライアントの数を増やす必要があります。

### <a name="running-a-connection-density-test"></a>接続密度テストを実行します。

このセクションでは、SignalR アプリケーションで接続密度テストを実行するために必要な手順について説明します。

1. ビルドをダウンロードして、 [SignalR の開発ブランチのコードベース](https://github.com/SignalR/SignalR/archive/dev.zip)します。 コマンド プロンプトでに移動します。&lt;プロジェクト ディレクトリ&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug します。
2. その目的のホスティング環境にアプリケーションをデプロイします。 アプリケーションが使用するエンドポイントをメモしてをおきます作成したアプリケーションなどで、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)、エンドポイントが`http://<yourhost>:8080/signalr`します。
3. インストール[SignalR パフォーマンス カウンター](signalr-performance.md#perfcounters)サーバー。 アプリケーションが Azure で実行している場合は、次を参照してください。 [Azure Web ロールで SignalR パフォーマンス カウンターを使用して](using-signalr-performance-counters-in-an-azure-web-role.md)します。

Crank コマンド ライン ツールで見つかるダウンロードして、コードベースのビルドしホストのパフォーマンス カウンターをインストールした後、`src\Microsoft.AspNet.SignalR.Crank\bin\Debug`フォルダー。

Crank ツールの使用可能なオプションは次のとおりです。

- **/?**: ヘルプ画面を示しています。 場合にも使用可能なオプションが表示されます、 **Url**パラメーターを省略するとします。
- **/Url**: SignalR 接続の URL。 このパラメーターは必須です。 SignalR アプリケーションの既定のマッピングを使用する場合、パスの末尾が"/signalr"。
- **/トランスポート**: 使用するトランスポートの名前。 既定値は`auto`、最適使用可能なプロトコルが選択されます。 オプションがあります。 `WebSockets`、 `ServerSentEvents`、および`LongPolling`(`ForeverFrame`オプションではありません、実際の .NET クライアントから Internet Explorer を使用するのではなく)。 SignalR のトランスポートを選択する方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)します。
- **/BatchSize**: 各バッチで追加されたクライアントの数。 既定値には 50 です。
- **/ConnectInterval**: 間隔 (ミリ秒) の接続を追加します。 既定値は 500 です。
- **/接続**: アプリケーションのロード テストに使用される接続の数。 既定値には 100,000 です。
- **/ConnectTimeout**: テストを中止するまでの秒のタイムアウト。 既定では 300 です。
- **MinServerMBytes**: サーバーの最小メガバイト数に到達します。 既定値は 500 です。
- **SendBytes**: (バイト単位) のサーバーに送信されるペイロードのサイズ。 既定値は 0 です。
- **SendInterval**: サーバーへのメッセージ間のミリ秒の遅延。 既定値は 500 です。
- **SendTimeout**: サーバーへのメッセージのミリ秒単位のタイムアウト。 既定では 300 です。
- **ControllerUrl**: 1 台のクライアントがコント ローラーのハブをホストする Url。 既定値は null (コント ローラー ハブはありません)。 Crank セッションが開始される; コント ローラー、ハブが開始しましたそれ以上のコント ローラーのハブと Crank 間の接続が行われます。
- **NumClients**: アプリケーションに接続するシミュレートされたクライアントの数。 既定では、1 つです。
- **ログ ファイル**: テストの実行のログ ファイルのファイル名。 既定値は `crank.csv` です。
- **SampleInterval**: パフォーマンス カウンターのサンプル間のミリ秒単位の時間。 既定値は 1000 です。
- **SignalRInstance**: サーバーのパフォーマンス カウンターのインスタンス名。 既定では、クライアント接続の状態を使用します。

### <a name="example"></a>例

次のコマンドはというサイトをテスト`pfsignalr`"ControllerHub"という名前のハブでポート 8080 でアプリケーションをホストする azure では、100 個の接続を使用します。

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
