---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: SignalR 接続の密度がクランクでのテスト |Microsoft ドキュメント
author: tfitzmac
description: SignalR 接続の密度がクランクでテストします。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2017
ms.locfileid: "26535331"
---
<a name="signalr-connection-density-testing-with-crank"></a>SignalR 接続の密度がクランクでテストします。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、クランク ツールを使用して、複数のシミュレートされたクライアント アプリケーションをテストする方法について説明します。


アプリケーションが (いずれか、Azure web ロール、IIS、または自己ホスト型 Owin を使用して) そのホスティング環境で実行されていると、クランク ツールを使用して接続密度の高いレベルへのアプリケーションの応答をテストできます。 ホスティング環境には、インターネット インフォメーション サービス (IIS) サーバー、Owin ホストでは、または Azure web ロールを指定できます。 (注: 接続密度テストからパフォーマンス データを取得することはできませんので、パフォーマンス カウンターを Azure App Service Web Apps で使用できることはできません)。

接続の密度は、サーバー上に確立できる同時 TCP 接続の数を示します。 各 TCP 接続が独自のオーバーヘッドが発生し、多数のアイドル状態の接続を開くと、メモリのボトルネックは最終的に作成します。

[SignalR のコードベース](https://github.com/signalr/signalr)と呼ばれる、ロード テスト ツールが含まれています**Crank**です。 クランクの最新バージョンは含まれて[Dev 分岐](https://github.com/SignalR/signalr/tree/dev)GitHub でします。 Zip アーカイブされた SignalR の Dev 分岐のコードベースをダウンロードする[ここ](https://github.com/SignalR/SignalR/archive/dev.zip)です。

完全サーバーのメモリが飽和状態には、クランクのサーバー ハードウェアに可能なアイドル状態の接続の合計数を計算するために使用できます。 または、可能性がありますも使用するクランクを読み込むテスト一定量のメモリ不足、下にあるサーバー特定数または特定のメモリしきい値に到達するまで、接続を変化させることによってです。

をテストする場合は、リモート クライアントを使用して (つまり、TCP 接続およびメモリ) のリソースの競合を回避する必要があります。 サーバーがすべての容量 (メモリや CPU) に到達するを防ぐことがあります、ボトルネックがヒットいないことを確認するクライアントを監視します。 サーバーを完全に読み込むためにクライアントの数を増やす必要があります。

### <a name="running-a-connection-density-test"></a>接続密度テストを実行します。

このセクションでは、SignalR アプリケーションで接続密度テストを実行するために必要な手順について説明します。

1. ビルドをダウンロードして、 [SignalR の Dev 分岐 codebase](https://github.com/SignalR/SignalR/archive/dev.zip)です。 コマンド プロンプトで、に移動&lt;プロジェクト ディレクトリ&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug です。
2. 目的のホスティング環境にアプリケーションを配置します。 アプリケーションが使用するエンドポイントのメモします。作成したアプリケーションなどで、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)、エンドポイントが`http://<yourhost>:8080/signalr`です。
3. インストール[SignalR パフォーマンス カウンター](signalr-performance.md#perfcounters)サーバーにします。 アプリケーションが Azure で実行されている場合は、次を参照してください。 [Azure Web ロールで SignalR パフォーマンス カウンターを使用して](using-signalr-performance-counters-in-an-azure-web-role.md)です。

ダウンロードし、コードベースを構築し、ホストのパフォーマンス カウンターをインストールしたら、クランク コマンド ライン ツールは含まれて、`src\Microsoft.AspNet.SignalR.Crank\bin\Debug`フォルダーです。

クランク ツールの使用可能なオプションは次のとおりです。

- **/?**: ヘルプ画面を示しています。 場合にも使用可能なオプションが表示されます、 **Url**パラメーターを省略するとします。
- **/Url**: SignalR 接続の URL。 このパラメーターは必須です。 既定のマッピングを使用して SignalR アプリケーションのパスの末尾が"/signalr"です。
- **/トランスポート**: 使用するトランスポートの名前。 既定値は`auto`、最適使用可能なプロトコルを選択するされます。 オプションがあります。 `WebSockets`、 `ServerSentEvents`、および`LongPolling`(`ForeverFrame`オプションがない実際の .NET クライアントから Internet Explorer を使用するのではなく)。 SignalR のトランスポートを選択する方法の詳細については、次を参照してください。[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)です。
- **/BatchSize**: 各バッチで追加されたクライアントの数。 既定値は 50 です。
- **/ConnectInterval**: 接続を追加するまでのミリ秒の間隔。 既定値は 500 です。
- **/接続**: ロード テスト アプリケーションを使用する接続の数。 既定値は 100,000 です。
- **/ConnectTimeout**: テストを中止するまでの秒単位のタイムアウト。 既定値は、300 です。
- **MinServerMBytes**: サーバーの最小メガバイト数に到達します。 既定値は 500 です。
- **SendBytes**: (バイト単位)、サーバーに送信されるペイロードのサイズ。 既定値は 0 です。
- **SendInterval**: サーバーへのメッセージまでのミリ秒の遅延します。 既定値は 500 です。
- **SendTimeout**: サーバーへのメッセージをミリ秒でタイムアウトします。 既定値は、300 です。
- **ControllerUrl**: 1 台のクライアントがハブ コント ローラーをホストする Url。 既定値は null (コント ローラー ハブはありません)。 クランク セッションが開始されるときに、コント ローラーのハブが開始します。それ以上、コント ローラー ハブ クランクと連絡先が行われます。
- **NumClients**: アプリケーションへの接続にシミュレートされたクライアントの数。 既定では 1 つ。
- **Logfile**: テストの実行のログ ファイルのファイル名。 既定値は、`crank.csv` です。
- **SampleInterval**: パフォーマンス カウンターのサンプルの間隔をミリ秒で時間。 既定値は 1000 です。
- **SignalRInstance**: サーバーのパフォーマンス カウンターのインスタンス名。 既定では、クライアント接続の状態を使用します。

### <a name="example"></a>例

次のコマンドはというサイトをテスト`pfsignalr`"ControllerHub"という名前のハブでポート 8080 でアプリケーションをホストする Azure で 100 個の接続を使用します。

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
