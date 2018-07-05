---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'ハンズ オン ラボ: SignalR によるリアルタイムの Web アプリケーション |Microsoft Docs'
author: rick-anderson
description: リアルタイムの Web アプリケーションには、サーバー側の偶然ですが、リアルタイムで接続されているクライアントにコンテンツをプッシュする機能が機能します。 ASP.NET 開発者は、ASP.
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 50ed2bed6b5b20684d00d7887494ee41346b5c3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828910"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>SignalR によるリアルタイムの Web アプリケーションをハンズ オン ラボ:
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> リアルタイムの Web アプリケーションには、サーバー側の偶然ですが、リアルタイムで接続されているクライアントにコンテンツをプッシュする機能が機能します。 ASP.NET 開発者向けの**ASP.NET SignalR**は自分のアプリケーションにリアルタイム web 機能を追加するライブラリです。 これは、クライアントとサーバーの最適な使用可能なトランスポート最適使用可能なトランスポートを自動的に選択すると、いくつかのトランスポート利用します。 活用されて**WebSocket**ブラウザーとサーバー間の双方向通信を実現する HTML5 API。
> 
> **SignalR**クライアント RPC サーバーを行うためのシンプルで高レベルの API も提供します (サーバー側の .NET コードからのクライアントのブラウザーで JavaScript 関数を呼び出す) で、ASP.NET アプリケーションとの接続管理などの便利なフックを追加します。など、接続/切断イベント、接続のグループ化、および承認します。
> 
> **SignalR**クライアントとサーバー間のリアルタイムの作業を行うには、必要なトランスポートの中に、抽象化です。 A **SignalR**接続は、HTTP、として起動しには、昇格を**WebSocket**使用可能な場合に接続します。 **WebSocket**の理想的なトランスポートは、 **SignalR**、サーバー メモリが最も効率的な使用には、最小の待機時間を備え、基になる最も機能があります (クライアント間の全二重通信など、サーバー) が最も厳しい要件があります: **WebSocket** 、サーバーを使用する必要があります**Windows Server 2012**または**Windows 8**、と共に **.NET framework 4.5**します。 これらの要件が満たされない場合**SignalR**他のトランスポートを使用して、その接続の確立を試みます (など*長いポーリングの Ajax*)。
> 
> **SignalR** API には、クライアントとサーバー間の通信に 2 つのモデルが含まれています:**永続的な接続**と**Hubs**します。 A**接続**別にグループ化、またはブロードキャスト メッセージを受信者の 1 つの送信を単純なエンドポイントを表します。 A**ハブ**は、クライアントとサーバーが相互に直接メソッドを呼び出すことができる接続 API に基づいて構築されておりより高度なパイプラインです。
> 
> ![SignalR のアーキテクチャ](real-time-web-applications-with-signalr/_static/image1.png)
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- SignalR を使用してクライアントにサーバーからの通知を送信します。
- スケール アウト、SignalR アプリケーションを使用して**SQL Server**します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。

1. Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダー。
2. 右クリック**Setup.cmd**選択**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。
3. ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットの使用

ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。 演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [SignalR を使用してリアルタイムのデータの使用](#Exercise1)
2. [SQL Server を使用したスケール アウト](#Exercise2)

この演習の所要時間を推定: **60 分**

> [!NOTE]
> Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。 このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。 開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>手順 1: SignalR を使用してリアルタイムのデータの使用

全体を行うことができますチャットの使用が例としては、多くの場合、リアルタイム Web 機能を備えたより。 ユーザーは、新しいデータまたは長いポーリングが新しいデータを取得する Ajax ページの実装を表示する web ページを更新します。 いつでも SignalR を使用することができます。

SignalR をサポートしています**サーバー プッシュ**または**ブロードキャスト**機能; 接続の管理を自動的に処理されます。 クライアント/サーバー通信のクラシックの HTTP 接続では、接続が要求ごとに再確立ですが SignalR クライアントとサーバー間の永続的な接続を提供します。 Signalr は、リモート プロシージャ コール (RPC) を使用してブラウザー内のクライアント コードを呼び出すサーバー コードで要求-応答のモデルではなく今日私たちが知る。

この演習では、構成、**ギーク Quiz** SignalR を使用して、ページ全体を更新する必要はありません、更新されたメトリックと統計情報のダッシュ ボードを表示するアプリケーション。

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>タスク 1 – おたくクイズの統計情報 ページの調査

このタスクでは、アプリケーションを経由して統計情報 ページを表示する方法を確認、方法は、情報をどのように向上できる更新されます。

1. 開く**Visual Studio Express 2013 for Web**を開くと、 **GeekQuiz.sln**ソリューション、 **Source\Ex1 WorkingWithRealTimeData\Begin**フォルダー。
2. キーを押して**F5**ソリューションを実行します。 **ログイン**ページがブラウザーに表示する必要があります。

    ![ソリューションを実行している](real-time-web-applications-with-signalr/_static/image2.png "ソリューションの実行")

    *ソリューションの実行*
3. クリックして**登録**アプリケーションに新しいユーザーを作成するページの右上隅にします。

    ![登録リンク](real-time-web-applications-with-signalr/_static/image3.png "登録リンク")

    *登録リンク*
4. **登録**ページで、入力、**ユーザー名**と**パスワード**、順にクリックします**登録**します。

    ![ユーザーの登録](real-time-web-applications-with-signalr/_static/image4.png "ユーザーの登録")

    *ユーザーの登録*
5. アプリケーションは、新しいアカウントを登録し、ユーザーが認証され、クイズの最初の質問を表示したホーム ページにリダイレクトします。
6. 開く、**統計**新しいウィンドウでページし、配置、**ホーム**ページと**統計**サイド バイ サイドでのページします。

    ![サイド バイ サイドでの windows](real-time-web-applications-with-signalr/_static/image5.png "サイド バイ サイドで側 windows")

    *サイド バイ サイドでの windows*
7. **ホーム** ページで、オプションのいずれかをクリックして、質問に回答します。

    ![質問に答え](real-time-web-applications-with-signalr/_static/image6.png "質問の回答")

    *質問の回答*
8. ボタンのいずれかをクリックした後、応答が表示されます。

    ![質問の答えが正しい](real-time-web-applications-with-signalr/_static/image7.png "質問の答えが正しい")

    *正しく回答する質問*
9. 統計情報 ページで提供される情報が古いことに注意してください。 更新された結果を表示するには、ページを更新します。

    ![統計のページ](real-time-web-applications-with-signalr/_static/image8.png "統計情報 ページ")

    *統計情報 ページ*
10. Visual Studio に戻るし、デバッグを停止します。

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>タスク 2 – 追加の SignalR おたくクイズをオンラインのグラフを表示するには

このタスクでは、SignalR をソリューションに追加し、サーバーに新しい回答が送信されるときに、クライアントに更新プログラムを自動的に送信します。

1. **ツール** メニューの選択 Visual Studio で**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。
2. **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![SignalR パッケージのインストール](real-time-web-applications-with-signalr/_static/image9.png "SignalR パッケージのインストール")

    *SignalR パッケージのインストール*

   > [!NOTE]
   > インストールするときに**SignalR**手動で更新する必要はまったく新しい MVC 5 アプリケーションから NuGet パッケージ バージョン 2.0.2、 **OWIN**パッケージをバージョン 2.0.1 (またはそれ以降) SignalR をインストールする前にします。 これを行うには、以下のスクリプトを実行することができます、**パッケージ マネージャー コンソール**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > SignalR の将来のリリースで OWIN の依存関係は自動的に更新されます。
3. **ソリューション エクスプ ローラー**、展開、**スクリプト**フォルダーに注目してくださいを SignalR *js*ファイル、ソリューションに追加されました。

    ![SignalR JavaScript 参照](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 参照")

    *SignalR JavaScript 参照*
4. **ソリューション エクスプ ローラー**を右クリックし、 **GeekQuiz**プロジェクトで、**追加** | **新しいフォルダー**、名前を付けます**ハブ**します。
5. 右クリックし、 **Hubs**フォルダーと選択**追加 |新しい項目の**します。

    ![新しい項目の追加](real-time-web-applications-with-signalr/_static/image11.png "新しい項目の追加")

    *新しい項目を追加します。*
6. **新しい項目の追加**ダイアログ ボックスで、 **(Visual C#) |Web |SignalR**選択の左側のウィンドウでノード**SignalR ハブ クラス (v2)** ファイルの名前を中央のウィンドウから**StatisticsHub.cs**  をクリック**追加**します。

    ![新しい項目 ダイアログ ボックスを追加](real-time-web-applications-with-signalr/_static/image12.png "追加新しい項目 ダイアログ ボックス")

    *新しい項目 ダイアログ ボックスを追加します。*
7. コードに置き換えます、 **StatisticsHub**クラスを次のコード。

    (コード スニペット - *RealTimeSignalR - Ex1 - StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. 開いている**Startup.cs**の末尾に次の行を追加し、**構成**メソッド。

    (コード スニペット - *RealTimeSignalR - Ex1 - MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. 開く、 **StatisticsService.cs**内でページ、**サービス**フォルダー以下を追加しますディレクティブを使用します。

    (コード スニペット - *RealTimeSignalR - Ex1 - UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. 取得する最初の更新プログラムの接続されているクライアントを通知する、**コンテキスト**現在の接続オブジェクトです。 **ハブ**オブジェクトには、1 つのクライアントまたはブロードキャストに接続されているすべてのクライアントにメッセージを送信するメソッドが含まれています。 次のメソッドを追加、 **StatisticsService**統計データをブロードキャストするクラス。

    (コード スニペット - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > 上記のコードでは、クライアントで関数を呼び出すため、任意のメソッド名を使用します (つまり: *updateStatistics*)。 指定したメソッド名は、IntelliSense またはコンパイル時検証がないことを意味の動的オブジェクトとして解釈されます。 式は、実行時に評価されます。 メソッドの呼び出しが実行されると、SignalR は、メソッド名とパラメーターの値をクライアントに送信します。 クライアントが名前に一致するメソッドの場合、そのメソッドが呼び出され、パラメーターの値が渡されました。 クライアントに一致するメソッドが見つからない場合、エラーは発生しません。 詳細についてを参照してください[ASP.NET SignalR ハブ API ガイド](../guide-to-the-api/hubs-api-guide-server.md)します。
11. 開く、 **TriviaController.cs**内でページ、**コント ローラー**フォルダー以下を追加しますディレクティブを使用します。

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. 次の強調表示されたコードを追加、 **Post**アクション メソッド。

    (コード スニペット - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. 開く、 **Statistics.cshtml**内でページ、**ビュー |ホーム**フォルダー。 検索、**スクリプト**セクションし、セクションの先頭に次のスクリプト参照を追加します。

    (コード スニペット - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > SignalR およびその他のスクリプト ライブラリを Visual Studio プロジェクトに追加すると、パッケージ マネージャーはこのトピックに示すバージョンよりも新しい SignalR スクリプト ファイルのバージョンをインストール可能性があります。 コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。
14. SignalR ハブにクライアントを接続し、新しいメッセージがハブから受信したときに、統計データを更新する次の強調表示されたコードを追加します。

    (コード スニペット - *RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    このコードではハブ プロキシの作成され、サーバーによって送信されたメッセージをリッスンするイベント ハンドラーを登録します。 経由で送信メッセージをリッスンするこの例では、 *updateStatistics*メソッド。

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>タスク 3 – ソリューションの実行

このタスクでは、新しい質問に回答した後は、SignalR を使用して自動的に統計情報 ビューを更新することを確認するソリューションを実行します。

1. キーを押して**F5**ソリューションを実行します。

    > [!NOTE]
    > 場合は、アプリケーションにまだログインして、タスク 1 で作成したユーザーでログインします。
2. 開く、**統計**新しいウィンドウでページし、配置、**ホーム**ページと**統計**タスク 1 で行ったように、サイド バイ サイドをページします。
3. **ホーム** ページで、オプションのいずれかをクリックして、質問に回答します。

    ![別の質問に答える](real-time-web-applications-with-signalr/_static/image13.png "別の質問に答える")

    *別の質問に答える*
4. ボタンのいずれかをクリックした後、応答が表示されます。 ページ全体を更新することがなく最新の情報を質問に回答した後、ページ上の統計情報が自動的に更新されることを確認します。

    ![応答の後に統計情報のページが更新される](real-time-web-applications-with-signalr/_static/image14.png "応答の後に統計情報のページが更新されます。")

    *応答の後に更新された統計情報ページ*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>手順 2: をスケール アウト、SQL Server の使用

選択できます一般に、web アプリケーションをスケーリングするときに*スケール アップ*と*スケール アウト*オプション。 *スケール アップ*中にその他のリソース (CPU、RAM など) で大規模なサーバーを使用することを意味*スケール アウト*負荷を処理するサーバーを追加することを意味します。 後者の場合、問題は、クライアント別のサーバーにルーティングできることです。 1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。

呼ばれるコンポーネントを使用してこれらの問題を解決できる*バック プレーン*サーバー間でメッセージを転送するようにします。 有効になっているバック プレーン、各アプリケーション インスタンスが、バック プレーンにメッセージを送信し、バック プレーンの他のアプリケーション インスタンスに転送します。

現在は、SignalR のバック プレーンの 3 つの種類があります。

- **Windows Azure Service Bus**します。 Service Bus は、メッセージング インフラストラクチャを使用する疎結合のメッセージを送信するコンポーネントです。
- **SQL Server**します。 SQL Server バック プレーンでは、SQL テーブルにメッセージを書き込みます。 バック プレーンでは、効率的なメッセージングを Service Broker を使用します。 ただし、これは Service Broker が有効になっていない場合にも機能します。
- **Redis**します。 Redis はメモリ内のキー値ストアです。 Redis では、メッセージを送信するため、(「パブリッシュ/サブスクライブ」) の発行/サブスクライブ パターンをサポートしています。

すべてのメッセージは、メッセージ バスを通じて送信されます。 メッセージ バス実装、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。 既定値を置き換えることで、バック プレーンの動作**IMessageBus**バス バック プレーンに対するに設計されています。

各サーバー インスタンスは、バスを介してバック プレーンに接続します。 メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。 サーバーは、バック プレーンからメッセージを受信したときに、そのローカル キャッシュ内で、メッセージを格納します。 サーバーはし、そのローカル キャッシュから、クライアントにメッセージを配信します。

SignalR のバック プレーンの動作は、こちらの詳細については[記事](../performance/scaleout-in-signalr.md)します。

> [!NOTE]
> バック プレーンがボトルネックになることがいくつかのシナリオがあります。 SignalR の一般的なシナリオを次に示します。
> 
> - [サーバー ブロードキャスト](tutorial-server-broadcast-with-signalr.md)(たとえば、株価表示器): サーバー メッセージが送信される速度を制御するために、バック プレーンがこのシナリオに動作します。
> - [クライアントで](tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでのバック プレーン場合がありますボトルネックになっているクライアントの数のメッセージの数に合わせて; は、メッセージの数が増えた場合それに比例して増えるクライアントが参加します。
> - [高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。


この演習では使用して**SQL Server**間でメッセージを配布する、**ギーク Quiz**アプリケーション。 完全な効果を取得するために、構成を設定する方法については、1 つのテスト マシンでこれらのタスクを実行するは、SignalR アプリケーションを 2 つまたは複数のサーバーをデプロイする必要があります。 サーバーのいずれか、または別の専用サーバーでは、SQL Server をインストールすることもする必要があります。

![SQL Server の図を使用してスケール](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>タスク 1 - シナリオを理解します。

このタスクでは、2 つのインスタンスを実行する**ギーク Quiz**インスタンスをローカル コンピューターに複数の IIS をシミュレートします。 この場合、1 つのアプリケーションのトリビアの質問に応答するときは、2 番目のインスタンスの統計情報 ページで更新プログラムを通知しません。 このシミュレーションよう、アプリケーションが複数のインスタンスに配置される場所、環境と通信するロード バランサーを使用します。

1. 開く、 **Begin.sln**ソリューション、**ソース/Ex2-ScalingOutWithSQLServer/開始**フォルダー。 読み込まれた後に表示されます、**サーバー エクスプ ローラー**構造体が異なる名前のソリューションには、同一である 2 つのプロジェクトが含まれています。 これは、ローカル コンピューターに、同じアプリケーションの 2 つのインスタンスを実行しているシミュレートします。

    ![コンピューターおたくクイズの 2 つのインスタンスをシミュレートするソリューションの開始](real-time-web-applications-with-signalr/_static/image16.png "おたくクイズの 2 つのインスタンスをシミュレートするソリューションの開始")

    *コンピューターおたくクイズの 2 つのインスタンスをシミュレートするソリューションを開始します。*
2. ソリューション ノードを右クリックして、ソリューションのプロパティ ページを開く**プロパティ**します。 **スタートアップ プロジェクト**を選択します**マルチ スタートアップ プロジェクト**を変更して、**アクション**両方のプロジェクトに対して値*開始*します。

    ![複数のプロジェクトを開始](real-time-web-applications-with-signalr/_static/image17.png "複数のプロジェクトを開始")

    *複数のプロジェクトを開始*
3. キーを押して**F5**ソリューションを実行します。 アプリケーションが 2 つのインスタンスを起動**ギーク Quiz**別々 のポートで同じアプリケーションの複数のインスタンスをシミュレートします。 左辺と他の画面の右側で、ブラウザーのいずれかをピン留めします。 資格情報でログインするか、新しいユーザーを登録します。 ログインすると、左側のトリビアのページを保持しに移動、**統計**右側のブラウザーでページ。

    ![サイド バイ サイドのおたくクイズ](real-time-web-applications-with-signalr/_static/image18.png)

    *サイド バイ サイドのおたくクイズ*

    ![別のポートのおたくのクイズ](real-time-web-applications-with-signalr/_static/image19.png)

    *別のポートのおたくのクイズ*
4. 左側のブラウザーでの質問に回答を起動し、表示になります、**統計**適切なブラウザーでページが更新されません。 これは、ため**SignalR**クライアント間でメッセージを配布する、ローカル キャッシュの使用、このシナリオでは、複数のインスタンスをシミュレートします。 そのため、キャッシュはそれらの間で共有されません。 確認できます**SignalR**で同じ手順をテストが 1 つのアプリを使用して、動作します。 次のタスクでは、インスタンス間でメッセージをレプリケート バック プレーンを構成します。
5. Visual Studio に戻るし、デバッグを停止します。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>タスク 2 – SQL Server のバック プレーンの作成

このタスクでのバック プレーンとして使用するデータベースを作成するが、**ギーク Quiz**アプリケーション。 使用する**SQL Server オブジェクト エクスプ ローラー**をサーバーを参照し、データベースを初期化します。 さらに、有効にする、 **Service Broker**します。

1. **Visual Studio**開き、メニュー**ビュー**選択と**SQL Server オブジェクト エクスプ ローラー**します。
2. 右クリックして、LocalDB インスタンスへの接続、 **SQL Server**ノードと選択**SQL Server の追加.** オプション。

    ![SQL Server のインスタンスを追加する](real-time-web-applications-with-signalr/_static/image20.png "SQL Server のインスタンスを追加します。")

    *SQL Server オブジェクト エクスプ ローラーへの SQL Server インスタンスの追加*
3. 設定、**サーバー名**に *(localdb) \v11.0*して**Windows 認証**認証モードとして。 クリックして**Connect**を続行します。

    ![LocalDB への接続](real-time-web-applications-with-signalr/_static/image21.png "LocalDB への接続")

    *LocalDB への接続*
4. これで、LocalDB インスタンスに接続すると、SignalR の SQL Server バック プレーンを表すデータベースを作成する必要があります。 これを行うを右クリックし、**データベース**ノード**新しいデータベースの追加**します。

    ![新しいデータベースを追加する](real-time-web-applications-with-signalr/_static/image22.png "の新しいデータベースの追加")

    *新しいデータベースを追加します。*
5. データベース名を設定*SignalR*  をクリック**OK**を作成します。

    ![SignalR のデータベースを作成する](real-time-web-applications-with-signalr/_static/image23.png "SignalR データベースを作成します。")

    *SignalR のデータベースを作成します。*

    > [!NOTE]
    > データベースの任意の名前を選択できます。
6. バック プレーンからより効率的に更新プログラムを受信するには、データベースの Service Broker を有効にすることをお勧めします。 Service Broker では、メッセージングと SQL Server でのキューのネイティブ サポートを提供します。 バック プレーンは、Service Broker なくも機能します。 クリックし、データベースを右クリックして新しいクエリを開く**新しいクエリ**します。

    ![新しいクエリを開いて、](real-time-web-applications-with-signalr/_static/image24.png "新しいクエリを開く")

    *新しいクエリを開く*
7. Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。 最近開いたクエリ ウィンドウで、次のスクリプトを実行します。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![サービス ブローカーの状態を照会](real-time-web-applications-with-signalr/_static/image25.png "サービス ブローカーの状態のクエリを実行します。")

    *サービス ブローカーの状態のクエリを実行します。*
8. 場合の値、**は\_broker\_有効になっている**、データベース内の列が&quot;0&quot;、有効にする、次のコマンドを使用します。 置換 **&lt;、データベース&gt;** データベースを作成するときに設定した名前 (例:: SignalR)。

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Service Broker を有効にする](real-time-web-applications-with-signalr/_static/image26.png "Service Broker を有効にします。")

    *Service Broker を有効にします。*

    > [!NOTE]
    > このクエリは、デッドロックを確認が表示された場合、DB に接続されているアプリケーションはありません。

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>タスク 3 – SignalR アプリケーションを構成します。

このタスクでは、構成**ギーク Quiz** SQL Server バック プレーンに接続します。 最初に、追加、 **SignalR.SqlServer**バック プレーン データベースに NuGet パッケージと、接続設定の文字列します。

1. 開く、**パッケージ マネージャー コンソール**から**ツール** | **ライブラリ パッケージ マネージャー**します。 確認します**GeekQuiz**でプロジェクトを選択、**既定のプロジェクト**ドロップダウン リスト。 インストールするには、次のコマンドを入力、 **Microsoft.AspNet.SignalR.SqlServer** NuGet パッケージ。

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. プロジェクトに対してこの時間が、前の手順を繰り返します**GeekQuiz2**します。
3. SQL Server バック プレーンを構成するには、開く、 **Startup.cs**のファイル、 **GeekQuiz**プロジェクトし、次のコードを追加、**構成**メソッド。 置換**&lt;YOUR DATABASE&gt;** を SQL Server バック プレーンを作成するときに使用したデータベース名。 この手順を繰り返します、 **GeekQuiz2**プロジェクト。

    (コード スニペット - *RealTimeSignalR - Ex2 - StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. これで、両方のプロジェクトは、SQL Server バック プレーンを使用する構成は、次のようにキーを押して**F5**それらを同時に実行します。
5. ここでも、 **Visual Studio**の 2 つのインスタンスが起動**ギーク Quiz**で別のポート。 他の画面の右側および左側に、ブラウザーのいずれかをピン留めし、資格情報でログインします。 左側のトリビアのページを保持しに移動**統計**ページイン適切なブラウザー。
6. 左側のブラウザーでの質問に答えることを開始します。 今回は、**統計**バック プレーンに協力してくれたページが更新されます。 アプリケーションを切り替える (**統計**、左側のようになりましたが、**トリビア**が右側) ことが、両方のインスタンスの動作を検証するテストを繰り返すとします。 バック プレーンとして、*共有キャッシュ*、接続されている各サーバーと各サーバーのメッセージは、メッセージを保存する接続されているクライアントに配布するための独自のローカル キャッシュです。
7. Visual Studio に戻るし、デバッグを停止します。
8. SQL Server のバック プレーン コンポーネントには、指定したデータベースで必要なテーブルが自動的に生成されます。 **SQL Server オブジェクト エクスプ ローラー**パネルで、バック プレーン用に作成したデータベースを開きます (例:: SignalR) し、そのテーブルを展開します。 次の表が表示されます。

    ![生成されたテーブルのバック プレーン](real-time-web-applications-with-signalr/_static/image27.png)

    *生成されたテーブルのバック プレーン*
9. 右クリックし、 **SignalR.Messages\_0**テーブルを選択**ビュー データ**します。

    ![SignalR のバック プレーン メッセージ テーブルな表示](real-time-web-applications-with-signalr/_static/image28.png)

    *SignalR のバック プレーン メッセージ テーブルな表示*
10. 送信されたさまざまなメッセージを表示、**ハブ**トリビアの質問に答えるときです。 バック プレーンでは、接続されている任意のインスタンスにこれらのメッセージを配布します。

    ![バック プレーン メッセージ テーブル](real-time-web-applications-with-signalr/_static/image29.png)

    *バック プレーン メッセージ テーブル*

* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボでは、追加する方法を説明しました**SignalR**を使用して、接続されているクライアントをサーバーからアプリケーションおよび送信通知に**Hubs**します。 さらを使用して、アプリケーションをスケーリングする方法について説明しました、*バック プレーン*コンポーネントの複数の IIS インスタンスで、アプリケーションがデプロイされた場合。
