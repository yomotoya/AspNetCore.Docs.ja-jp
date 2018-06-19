---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'ハンズ オン ラボ: SignalR でリアルタイムの Web アプリケーション |Microsoft ドキュメント'
author: rick-anderson
description: リアルタイムの Web アプリケーションには、サーバー側のように、リアルタイムで接続しているクライアントにコンテンツをプッシュする機能が機能します。 ASP、ASP.NET 開発者向けしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5a2bc120ded18ad2302fd6c5cde65a5323e86ca8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878049"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>SignalR でリアルタイムの Web アプリケーションをハンズ オン ラボ:
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> リアルタイムの Web アプリケーションには、サーバー側のように、リアルタイムで接続しているクライアントにコンテンツをプッシュする機能が機能します。 ASP.NET 開発者向けの**ASP.NET SignalR**は各自のアプリケーションにリアルタイム web 機能を追加するライブラリ。 これは、自動的にクライアントとサーバーの最適な使用可能なトランスポート最適使用可能なトランスポートを選択するいくつかの配送の活用します。 これを活用**WebSocket**ブラウザーとサーバー間で双方向通信を有効にする HTML5 API です。
> 
> **SignalR**クライアント RPC サーバーを行うためのシンプルで高度な API も用意されています (サーバー側の .NET コードからのクライアントのブラウザーで JavaScript 関数を呼び出す)、ASP.NET アプリケーションだけでなく、接続の管理用に便利ですフックを追加します。などの接続切断イベント、グループ化接続、および承認します。
> 
> **SignalR**は、クライアントとサーバー間のリアルタイムの作業を行うために必要なトランスポートの中に、抽象化します。 A **SignalR**接続は、HTTP、として起動しに昇格し、 **WebSocket**使用可能な場合に接続します。 **WebSocket**の理想的なトランスポートは、 **SignalR**サーバーのメモリを最も効率的に利用するため、最短の待機時間を持ち、最も基本的な機能 (クライアントの間の全二重通信など、サーバー)、最も厳しい要件がありますが、: **WebSocket**サーバーを使用する必要があります**Windows Server 2012**または**Windows 8**、と共に **.NET framework 4.5**です。 これらの要件が満たされない場合**SignalR**がその接続を確立する他のトランスポートを使用するとき (と同様に*Ajax ロング ポーリング*)。
> 
> **SignalR** API には、クライアントとサーバー間で通信するための 2 つのモデルが含まれています:**永続的な接続**と**ハブ**です。 A**接続**グループ化すると、受信者の 1 つの送信、またはブロードキャスト メッセージを単純なエンドポイントを表します。 A**ハブ**を互いに直接メソッドを呼び出すには、クライアントとサーバーをできるようにする接続 API に基づいて構築されておりより高度なパイプラインがします。
> 
> ![SignalR のアーキテクチャ](real-time-web-applications-with-signalr/_static/image1.png)
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- SignalR を使用してクライアントにサーバーからの通知を送信します。
- スケール アウトを使用して SignalR アプリケーションを**SQL Server**です。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。

1. Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダーです。
2. 右クリック**Setup.cmd**選択**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。
3. ユーザー アカウント制御 ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットを使用します。

ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> 各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。 実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の実習が含まれます。

1. [SignalR を使用したリアルタイムのデータの操作](#Exercise1)
2. [SQL Server を使用したスケール アウト](#Exercise2)

この演習を完了する時間を推定: **60 分**

> [!NOTE]
> Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。 このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。 開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>手順 1: SignalR を使用してリアルタイム データの使用

全体を行うことができますチャットは例として使用される多くの場合、中にリアルタイム Web 機能を持つたくさんです。 いつでも、ユーザーが新しいデータまたは Ajax の長いポーリングが、新しいデータを取得するページを実装する web ページを更新 SignalR を使用することができます。

SignalR をサポートしている**サーバー プッシュ**または**ブロードキャスト**機能です。 接続の管理を自動的に処理されます。 クライアント サーバー通信のための従来の HTTP 接続では、接続の要求ごとに再確立が、SignalR は、クライアントとサーバー間で永続的な接続を提供します。 SignalR のサーバー コードを呼び出すクライアント コード、リモート プロシージャ コール (RPC) を使用してブラウザーで、要求/応答モデルではなくお把握します。

この演習では、構成、**マニア Quiz** SignalR を使用して、統計情報を持つダッシュ ボード全体のページを更新することがなく更新されたメトリックを表示するアプリケーション。

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>タスク 1 – マニア クイズの統計情報 ページの表示

このタスクでは、アプリケーションを経由して統計情報 ページを表示する方法を確認し、更新する方法についてを向上する方法します。

1. 開く**Visual Studio Express 2013 for Web**を開くと、 **GeekQuiz.sln**にソリューションがある、 **Source\Ex1 WorkingWithRealTimeData\Begin**フォルダーです。
2. キーを押して**f5 キーを押して**ソリューションを実行します。 **ログイン**ページがブラウザーに表示する必要があります。

    ![ソリューションを実行している](real-time-web-applications-with-signalr/_static/image2.png "ソリューションの実行")

    *ソリューションの実行*
3. をクリックして**登録**アプリケーションに新しいユーザーを作成するページの右上隅にします。

    ![リンクの登録](real-time-web-applications-with-signalr/_static/image3.png "レジスタ リンク")

    *リンクを登録します。*
4. **登録** ページで、入力、**ユーザー名**と**パスワード**、クリックして**登録**です。

    ![ユーザーの登録](real-time-web-applications-with-signalr/_static/image4.png "ユーザーの登録")

    *ユーザーの登録*
5. アプリケーションが、新しいアカウントを登録し、ユーザーが認証され、最初のクイズ問題を示す、ホーム ページにリダイレクトします。
6. 開く、**統計**を新しいウィンドウでページし、配置、**ホーム**ページと**統計**サイド バイ サイドのページです。

    ![サイド バイ サイド windows](real-time-web-applications-with-signalr/_static/image5.png "側 windows 側")

    *サイド バイ サイド windows*
7. **ホーム** ページをクリックすると、オプションのいずれかの質問に回答します。

    ![質問の回答](real-time-web-applications-with-signalr/_static/image6.png "質問に答えること")

    *質問に答えること*
8. 一方のボタンをクリックすると、回答が表示されます。

    ![質問の答えが正しい](real-time-web-applications-with-signalr/_static/image7.png "質問の答えが正しい")

    *正しく回答する質問*
9. 統計情報 ページで提供される情報が古くなっていることを確認します。 更新された結果を確認するために、ページを更新します。

    ![統計情報ページ](real-time-web-applications-with-signalr/_static/image8.png "統計情報 ページ")

    *統計情報 ページ*
10. Visual Studio に戻り、デバッグを停止します。

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>タスク 2: 追加の SignalR マニア クイズをオンラインのグラフを表示するには

このタスクでは、SignalR をソリューションに追加し、新しい応答がサーバーに送信されるときに、クライアントに更新プログラムを自動的に送信します。

1. **ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**、クリックして**パッケージ マネージャー コンソール**です。
2. **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![パッケージがインストールされた SignalR](real-time-web-applications-with-signalr/_static/image9.png "SignalR パッケージのインストール")

    *SignalR のパッケージのインストール*

   > [!NOTE]
   > インストールするときに**SignalR** NuGet パッケージのバージョン 2.0.2 新しい MVC 5 のアプリケーションから、手動で更新する必要が**OWIN** 2.0.1 へのパッケージ (またはそれ以降) SignalR をインストールする前にします。 これを行うに次のスクリプトを実行することができます、 **Package Manager Console**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > SignalR の将来のリリースで OWIN の依存関係が自動的に更新されます。
3. **ソリューション エクスプ ローラー**、展開、**スクリプト**フォルダーとに注意してくださいを SignalR *js*ファイル、ソリューションに追加されました。

    ![SignalR JavaScript 参照](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 参照")

    *SignalR JavaScript 参照*
4. **ソリューション エクスプ ローラー**を右クリックし、 **GeekQuiz**プロジェクトで、**追加** | **新しいフォルダー**、名前を付けます**ハブ**です。
5. 右クリックし、**ハブ**フォルダーと選択**追加 |新しい項目の**します。

    ![新しい項目の追加](real-time-web-applications-with-signalr/_static/image11.png "新しい項目の追加")

    *新しい項目を追加します。*
6. **新しい項目の追加**ダイアログ ボックスで、 **Visual c# |Web |SignalR** 、左ペインで、選択ノード**SignalR ハブ クラス (v2)** 、ファイルの名前を中央のウィンドウから**StatisticsHub.cs**  をクリック**追加**です。

    ![新しい項目 ダイアログ ボックスを追加](real-time-web-applications-with-signalr/_static/image12.png "追加新しい項目 ダイアログ ボックス")

    *新しい項目 ダイアログ ボックスを追加します。*
7. コードで置き換え、 **StatisticsHub**クラスを次のコード。

    (コード スニペットの*RealTimeSignalR - Ex1 - StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. 開いている**Startup.cs**の末尾に次の行を追加し、**構成**メソッドです。

    (コード スニペットの*RealTimeSignalR - Ex1 - MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. 開いている、 **StatisticsService.cs**内のページ、 **Services**フォルダーに次の追加とディレクティブを使用します。

    (コード スニペットの*RealTimeSignalR - Ex1 - UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. 取得する最初の更新プログラムの接続されているクライアントを通知するために、**コンテキスト**現在の接続オブジェクトです。 **ハブ**オブジェクトには、1 つまたは複数の接続をすべてにブロードキャスト クライアントにメッセージを送信するメソッドが含まれています。 次のメソッドを追加、 **StatisticsService**統計データをブロードキャストするクラス。

    (コード スニペットの*RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > 上記のコードでは、クライアントで関数を呼び出すには任意のメソッド名を使っている (つまり: *updateStatistics*)。 指定したメソッド名は、動的なオブジェクトは、IntelliSense またはのコンパイル時の検証がないことを意味として解釈されます。 式は、実行時に評価されます。 メソッドの呼び出しが実行されると、SignalR は、メソッド名とパラメーターの値をクライアントに送信します。 場合は、クライアントは、名前に一致するメソッドを持ち、そのメソッドが呼び出され、パラメーターの値が渡されます。 クライアントで一致するメソッドが見つからない場合、エラーは発生しません。 詳細についてを参照してください[ASP.NET SignalR ハブ API ガイド](../guide-to-the-api/hubs-api-guide-server.md)です。
11. 開く、 **TriviaController.cs**内のページ、**コント ローラー**フォルダーに次の追加とディレクティブを使用します。

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. 次の強調表示されたコードを追加、 **Post**アクション メソッド。

    (コード スニペットの*RealTimeSignalR - Ex1 - NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. 開く、 **Statistics.cshtml**内のページ、**ビュー |ホーム**フォルダーです。 検索、**スクリプト**セクションおよびセクションの先頭に次のスクリプト参照を追加します。

    (コード スニペットの*RealTimeSignalR - Ex1 - SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > SignalR と他のスクリプト ライブラリを Visual Studio プロジェクトに追加するときに、パッケージ マネージャーがここに示すように、バージョンより新しくなっている SignalR スクリプト ファイルのバージョンをインストールできます。 コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。
14. SignalR ハブにクライアントを接続し、ハブから、新しいメッセージが受信したときに、統計データを更新する次の強調表示されたコードを追加します。

    (コード スニペットの*RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    このコードではハブ プロキシの作成され、サーバーから送信されたメッセージをリッスンするイベント ハンドラーを登録します。 この場合、を介して送信されるメッセージをリッスンする、 *updateStatistics*メソッドです。

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>タスク 3: ソリューションの実行

このタスクでは、SignalR を使用して自動的に新しい質問に回答した後、統計情報 ビューを更新することを確認するようにソリューションを実行します。

1. キーを押して**f5 キーを押して**ソリューションを実行します。

    > [!NOTE]
    > アプリケーションにログインしていない場合、は、タスク 1 で作成したユーザーでログインします。
2. 開く、**統計**を新しいウィンドウでページし、配置、**ホーム**ページと**統計**タスク 1 の場合と同様に、サイド バイ サイドをページします。
3. **ホーム** ページをクリックすると、オプションのいずれかの質問に回答します。

    ![別の質問に答えること](real-time-web-applications-with-signalr/_static/image13.png "別の質問に答えること")

    *別の質問に答えること*
4. 一方のボタンをクリックすると、回答が表示されます。 全体のページを更新する必要がない最新の情報を質問に回答した後、ページの統計情報が自動的に更新することに注意してください。

    ![応答の後に統計情報のページが更新される](real-time-web-applications-with-signalr/_static/image14.png "応答後の統計情報 ページの更新")

    *応答の後に更新された統計情報 ページ*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>手順 2: 使用したスケール アウト SQL Server

Web アプリケーションを拡張するときに、通常のできます*スケール アップ*と*スケール アウト*オプション。 *スケール アップ*中に複数のリソース (CPU、RAM など) でより大きなサーバーを使用することを意味*スケール アウト*負荷を処理するサーバーを追加することを意味します。 後者の問題は、クライアントを別のサーバーにルーティング取得ことができます。 1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。

呼ばれるコンポーネントを使用してこれらの問題を解決できる*バック プレーン*サーバー間でメッセージを転送します場合。 有効になっているバック プレーンと各アプリケーション インスタンスが、バック プレーンにメッセージを送信バック プレーンに転送し、その他のアプリケーション インスタンス。

現在は、SignalR のバック プレーンの 3 つの種類があります。

- **Windows Azure Service Bus**です。 Service Bus は、メッセージング インフラストラクチャにより、疎結合されたメッセージを送信するコンポーネントです。
- **SQL Server**です。 SQL Server のバック プレーンでは、SQL テーブルにメッセージを書き込みます。 バック プレーンでは、効率的なメッセージの Service Broker を使用します。 ただし、これは Service Broker が有効になっていない場合にでも動作します。
- **Redis**です。 Redis は、メモリ内キー値ストアです。 Redis は、メッセージを送信する (「パブリッシュ/サブスクライブ」) の公開/定期受信パターンをサポートします。

メッセージ バスを介してすべてのメッセージが送信されます。 メッセージ バスを実装して、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。 既定値に置き換えることで、バック プレーンの作業**IMessageBus**そのバック プレーン用に設計されたバスとします。

各サーバー インスタンスは、バスを介してバック プレーンに接続します。 メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。 サーバーは、バック プレーンからメッセージを受信したときに、そのローカル キャッシュ内で、メッセージを格納します。 サーバーはのローカル キャッシュから、クライアントにメッセージを配信します。

詳細については、SignalR バック プレーンのしくみ、このテキストを読み取る[記事](../performance/scaleout-in-signalr.md)です。

> [!NOTE]
> バック プレーンがボトルネックになることがいくつかのシナリオがあります。 SignalR の一般的なシナリオを次に示します。
> 
> - [サーバーのブロードキャスト](tutorial-server-broadcast-with-signalr.md)(たとえば、株価表示器): サーバーにメッセージが送信される速度を制御するために、バック プレーンがこのシナリオの適切に動作します。
> - [クライアントからクライアントへの](tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでバック プレーン可能性がありますするボトルネックになっている場合は、クライアントの数によるメッセージの数が拡大縮小。 つまり、メッセージの数が増えた場合比例して増えるにつれてクライアントが参加します。
> - [高周波数のリアルタイム](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。


この演習では、使用**SQL Server**間でメッセージを配信、**マニア Quiz**アプリケーションです。 効果を取得するために、構成を設定する方法を学習する 1 つのテスト コンピューターでこれらのタスクを実行するが、SignalR アプリケーションを 2 つまたは複数のサーバーを展開する必要があります。 いずれかのサーバーまたは別の専用サーバーでは、SQL Server をインストールすることも必要があります。

![スケール アウト、SQL Server のダイアグラムの使用](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>タスク 1 のシナリオを理解します。

このタスクでは、2 つのインスタンスを実行して**マニア Quiz**ローカル コンピューターにインスタンスの複数の IIS をシミュレートします。 このシナリオで 1 つのアプリケーションでトリビア質問に答えることと、更新通知が送られません統計情報のページで、2 番目のインスタンス。 このシミュレーションのようになりますアプリケーションが複数のインスタンスに配置した環境に通信するために、ロード バランサーを使用するとします。

1. 開く、 **Begin.sln**にソリューションがある、**ソース/Ex2-ScalingOutWithSQLServer/開始**フォルダーです。 読み込まれるに表示されます、**サーバー エクスプ ローラー**構造体が異なる名前のソリューションには、同一である 2 つのプロジェクトが含まれています。 これにより、ローカル コンピューターに同じアプリケーションの 2 つのインスタンスを実行しているをシミュレートします。

    ![マニア クイズの 2 つのインスタンスをシミュレートするソリューションを開始](real-time-web-applications-with-signalr/_static/image16.png "マニア クイズの 2 つのインスタンスをシミュレートするソリューションの開始")

    *マニア クイズの 2 つのインスタンスをシミュレートするソリューションを開始します。*
2. ソリューション ノードを右クリックし、ソリューションのプロパティ ページを開く**プロパティ**です。 **スタートアップ プロジェクト**を選択**マルチ スタートアップ プロジェクト**を変更して、**アクション**両方のプロジェクトに対して値*開始*です。

    ![複数のプロジェクトを起動](real-time-web-applications-with-signalr/_static/image17.png "複数のプロジェクトの開始")

    *複数のプロジェクトの開始*
3. キーを押して**f5 キーを押して**ソリューションを実行します。 アプリケーションでは、2 つのインスタンスを起動します。**マニア Quiz**で別のポートを同じアプリケーションの複数のインスタンスをシミュレートします。 左と上、画面の右側に、ブラウザーのいずれかをピン留めします。 資格情報でログインするか、新しいユーザーを登録します。 ログインすると、左側のトリビア ページを保持しに移動、**統計**右側のブラウザーでページ。

    ![マニア クイズ サイド バイ サイド](real-time-web-applications-with-signalr/_static/image18.png)

    *マニア クイズ サイド バイ サイド*

    ![別のポートでマニア クイズ](real-time-web-applications-with-signalr/_static/image19.png)

    *別のポートでマニア クイズ*
4. 左側のブラウザーでの質問に回答を起動し、表示になります、**統計**右のブラウザーでページが更新されていません。 これは、ため**SignalR**クライアント間でメッセージを配信は、ローカル キャッシュ、このシナリオでは、複数のインスタンスをシミュレートします。 そのため、キャッシュはそれらの間で共有されません。 確認することができます**SignalR**同じ手順をテストが 1 つのアプリを使用してが動作します。 次のタスクでは、インスタンス間でメッセージをレプリケートするバック プレーンを構成します。
5. Visual Studio に戻り、デバッグを停止します。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>タスク 2 – SQL Server のバック プレーンの作成

このタスクでは、バック プレーンとして使用するデータベースを作成する、**マニア Quiz**アプリケーションです。 使用して**SQL Server オブジェクト エクスプ ローラー**をサーバーを参照して、データベースを初期化します。 さらに、有効にする、 **Service Broker**です。

1. **Visual Studio**、開かれているメニュー**ビュー**選択**SQL Server オブジェクト エクスプ ローラー**です。
2. 右クリックして、LocalDB インスタンスに接続、 **SQL Server**ノードを選択して**SQL サーバーを追加しています.** オプション。

    ![SQL Server インスタンスを追加する](real-time-web-applications-with-signalr/_static/image20.png "SQL Server インスタンスを追加します。")

    *SQL Server オブジェクト エクスプ ローラーに SQL Server インスタンスを追加します。*
3. 設定、**サーバー名**に *(localdb) \v11.0*のままにして**Windows 認証**認証モードとして。 をクリックして**接続**を続行します。

    ![LocalDB への接続](real-time-web-applications-with-signalr/_static/image21.png "LocalDB への接続")

    *LocalDB への接続*
4. これで LocalDB インスタンスに接続している、SignalR の SQL Server のバック プレーンと共に表示されるデータベースを作成する必要があります。 これを行うを右クリックし、**データベース**ノード**新しいデータベースの追加**です。

    ![新しいデータベースを追加する](real-time-web-applications-with-signalr/_static/image22.png "新しいデータベースを追加します。")

    *新しいデータベースを追加します。*
5. データベース名を設定します*SignalR*  をクリック**OK**を作成します。

    ![SignalR のデータベースを作成する](real-time-web-applications-with-signalr/_static/image23.png "SignalR データベースを作成します。")

    *SignalR のデータベースを作成します。*

    > [!NOTE]
    > データベースの任意の名前を選択できます。
6. バック プレーンから更新プログラムをより効率的に受信、データベースの Service Broker を有効にすることをお勧めします。 Service Broker は、メッセージングと SQL Server のキューに対するネイティブ サポートを提供します。 バック プレーンは、Service Broker なしでも動作します。 クリックし、データベースを右クリックして新しいクエリを開く**新しいクエリ**です。

    ![新しいクエリを開く](real-time-web-applications-with-signalr/_static/image24.png "新しいクエリを開く")

    *新しいクエリを開く*
7. Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。 直前に開かれたクエリ ウィンドウで、次のスクリプトを実行します。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![サービス ブローカーの状態を照会する](real-time-web-applications-with-signalr/_static/image25.png "サービス ブローカーの状態を照会します。")

    *サービス ブローカーの状態を照会します。*
8. 場合の値、**は\_broker\_有効になっている**、データベース内の列が&quot;0&quot;、有効にして、次のコマンドを使用します。 置き換える **&lt;、データベース&gt;** データベースを作成するときに設定した名前を持つ (例:: SignalR)。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Service Broker を有効にする](real-time-web-applications-with-signalr/_static/image26.png "Service Broker を有効にします。")

    *Service Broker を有効にします。*

    > [!NOTE]
    > このクエリは、デッドロックを起こすことを確認するが表示された場合、DB に接続されているアプリケーションはありません。

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>タスク 3 – SignalR アプリケーションを構成します。

このタスクでは、構成**マニア Quiz** SQL Server のバック プレーンに接続します。 最初に追加するが、 **SignalR.SqlServer**バック プレーン データベースに NuGet パッケージと接続文字列します。

1. 開く、 **Package Manager Console**から**ツール** | **ライブラリ パッケージ マネージャー**です。 確認して**GeekQuiz**でプロジェクトを選択、**既定のプロジェクト**ドロップダウン リスト。 インストールする次のコマンドを入力、 **Microsoft.AspNet.SignalR.SqlServer** NuGet パッケージです。

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. プロジェクトに対して、前の手順がこの時間を繰り返します**GeekQuiz2**です。
3. SQL Server のバック プレーンを構成するには、開く、 **Startup.cs**のファイル、 **GeekQuiz**プロジェクトし、次のコードを追加、**構成**メソッドです。 置き換える **&lt;、データベース&gt;** を SQL Server のバック プレーンの作成時に使用するデータベース名。 この手順を繰り返します、 **GeekQuiz2**プロジェクト。

    (コード スニペットの*RealTimeSignalR - Ex2 - StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. SQL Server のバック プレーンの使用には、両方のプロジェクトが構成されたら、キーを押します**f5 キーを押して**を同時に実行します。
5. もう一度、 **Visual Studio**の 2 つのインスタンスが起動**マニア Quiz**別のポートにします。 他の画面の右側および左側にピン留め、ブラウザーのいずれかと、資格情報でログインします。 左側のトリビア ページを保持しに移動**統計**ページイン右ブラウザー。
6. 左側のブラウザーで質問に対する回答を開始します。 現時点では、**統計**バック プレーン感謝ページを更新します。 アプリケーション間でのスイッチ (**統計**は現在は、左と**トリビア**は右端に表示) が機能する両方のインスタンスを検証するテストを繰り返すとします。 バック プレーン役割を果たす、*共有キャッシュ*各接続のサーバーと各サーバーのメッセージは、メッセージを保存するローカル キャッシュに接続しているクライアントに配布します。
7. Visual Studio に戻り、デバッグを停止します。
8. SQL Server のバック プレーン コンポーネントは、指定したデータベースで必要なテーブルを自動的に生成します。 **SQL Server オブジェクト エクスプ ローラー**パネルで、バック プレーン用に作成したデータベースを開きます (例:: SignalR) し、そのテーブルを展開します。 次の表が表示されます。

    ![生成されたテーブルのバック プレーン](real-time-web-applications-with-signalr/_static/image27.png)

    *生成されたテーブルのバック プレーン*
9. 右クリックし、 **SignalR.Messages\_0**テーブルを選択して**ビュー データ**です。

    ![SignalR のバック プレーンのメッセージ テーブルな表示](real-time-web-applications-with-signalr/_static/image28.png)

    *SignalR のバック プレーンのメッセージ テーブルな表示*
10. 送信される異なるメッセージを表示、**ハブ**トリビア質問に答えるときです。 バック プレーンでは、接続されている任意のインスタンスにこれらのメッセージを配信します。

    ![バック プレーン メッセージ テーブル](real-time-web-applications-with-signalr/_static/image29.png)

    *バック プレーン メッセージ テーブル*

* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボでは、追加する方法を学習しました**SignalR**を使用して接続されているクライアントにサーバーからアプリケーションおよび送信通知に**ハブ**です。 さらを使用して、アプリケーションを拡張する方法を学習しました、*バック プレーン*アプリケーションが複数の IIS インスタンスに展開されるコンポーネントです。
