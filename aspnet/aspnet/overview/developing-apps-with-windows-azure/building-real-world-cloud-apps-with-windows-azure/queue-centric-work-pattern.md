---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: キューを中心とした作業のパターン (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 124e673206ecea2eac5efb8c2802a32a690fa104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875436"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>キューを中心とした作業のパターン (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


複数のサービスを使用することになる、アプリの有効な SLA がここでは、「複合」SLA 見た以前、*製品*個々 の Sla のです。 たとえば、修正、アプリは、Web サイト、ストレージ、および SQL データベースを使用します。 これらのサービスのいずれかに失敗した場合、アプリはユーザーにエラーを返します。

キャッシュは、読み取り専用のコンテンツの一時的なエラーを処理することをお勧めします。 アプリが作業を実行する必要がある場合ですか。 たとえば、ユーザーが新しい解決、タスクを送信したときに、アプリことはできませんだけタスク キャッシュに格納します。 アプリは、処理できるように、永続データ ストアに、解決、タスクを書き込む必要があります。

キューを中心とした作業のパターンの出番です。 このパターンは、web 層およびバックエンド サービス間の疎結合を使用できます。

次に、パターンの動作方法を示します。 アプリケーションでは、要求を取得と、が作業項目をキューに配置され、すぐに、応答を返します。 個別のバックエンド プロセス キューから作業項目を取得し、処理します。

キューを中心とした作業のパターンは次に役立ちます。

- 時間のかかる (待機時間が高い) である作業します。
- 常に使用できない外部のサービスが必要な作業です。
- れた作業 (高い cpu 使用率) のリソースを消費します。
- 速度 (負荷の急激なバースト) に従います平準化からメリットを得られる作業します。

## <a name="reduced-latency"></a>待機時間の削減

キューは、時間のかかる作業を実行して、いつでも便利です。 タスクを受け取る場合は、数秒またはそれ以上、ブロックする代わりに、エンドユーザー、作業項目キューに配置します。 「に取り組んでいますが、」に、ユーザーに通知し、キュー リスナーを使用して、バック グラウンドでタスクを実行します。

たとえば、オンラインの小売店舗で商品を購入するときに、web サイト、注文を確認すぐにします。 あなたの情報が配信される、トラック内に既にことを意味します。 タスクが、キューに配置して、バック グラウンドは与信審査、出荷、アイテムの準備を行うなどです。

短い待機時間シナリオでは、エンド ツー エンド時間の合計が長いタスクを同期的に行うと比較して、キューを使用する可能性があります。 その場合でも、その他の利点がありますがその欠点を上回ることができます。

## <a name="increased-reliability"></a>信頼性の向上

バージョンでは、修正、これまで見てきたでこれまでの web フロント エンドは、SQL データベースのバック エンドと密接に関連します。 SQL データベース サービスが利用できない場合、ユーザーはエラーを取得します。 再試行が機能しない場合 (つまり、失敗、複数の一時的な) ことだけを行うことができますが、エラーを表示するようにユーザーが後でもう一度やり直してくださいを依頼してください。

![SQL データベースのバック エンドに失敗すると失敗したフロント エンドのダイアグラムを示す web](queue-centric-work-pattern/_static/image1.png)

ユーザーは、解決、タスクを送信するときに、キューを使用して、アプリは、キューにメッセージを書き込みます。 メッセージのペイロードが、 [JSON](http://json.org/)タスクの表現。 メッセージをキューに書き込むとすぐにアプリを返し、成功メッセージをユーザーにすぐに表示します。

オフライン – SQL データベースや、キュー リスナー--などのバックエンド サービスの場合、ユーザーは新しいの解決、タスクを送信できます。 バックエンド サービスは、再利用するまで、メッセージはキューのみ。 その時点では、バックエンド サービスは、バックログ キャッチします。

![SQL データベースのエラーがある場合に機能する web フロント エンドの続行を示す図](queue-centric-work-pattern/_static/image2.png)

さらに、今すぐフロント エンドの回復性を心配せずより多くのバックエンド ロジックを追加することができます。 たとえばの新しい修正が割り当てられている場合に、所有者に電子メールや SMS メッセージを送信することができます。 できなくなった場合、電子メールや SMS サービスを他のすべてを処理および電子メール/SMS メッセージを送信するための別のキューにメッセージを格納します。

以前は、Web アプリが効果的な SLA&times;ストレージ&times;SQL データベース 99.7% を = です。 (を参照してください[障害にも対処デザイン](design-to-survive-failures.md))。

キューを使用するアプリを変更する場合、web フロント エンドは、複合 99.8% の SLA の Web アプリおよび記憶域、のみに依存します。 (キューは、blob ストレージと同じ SLA に含まれているように、Azure ストレージ サービスの一部に注意してください)。

99.8% よりも必要がある場合は、次の 2 つの異なる地域に 2 つのキューを作成できます。 として、プライマリ、もう 1 つをセカンダリとして指定します。 アプリでは、フェールオーバーをセカンダリ キューにプライマリ キューが使用できない場合。 両方が利用できなくなる、同時に発生する可能性は非常に小さいです。

## <a name="rate-leveling-and-independent-scaling"></a>レートの平準化し、個別に拡張

キューと呼ばれるものに役立ちますも*転送率の平準化*または*負荷平準化*です。

Web アプリは、突発トラフィックを受けやすくなりますが多くの場合です。 自動的に増加した web トラフィックを処理する web サーバーを追加する自動スケーリングを使用できますが、自動スケーリングできないことがありますに反応するため十分速く処理負荷の急激なスパイクです。 Web サーバーは、キューにメッセージを書き込むことによって行う必要がある、作業の一部にオフロードできるより多くのトラフィックを処理できます。 バックエンド サービスは、キューからメッセージを読み取るし、それらを処理します。 キューの深さを拡張または圧縮受信の負荷が変化します。

バックエンド サービスに負荷のかかる作業の多くで、web 層は、トラフィックの急増に対応より簡単にできます。 Web サーバーの数が少ないで指定したトラフィック量を処理できるため、コストの節約します。

ことができます、web 層およびバックエンド サービスを個別に拡張します。 たとえば、1 つだけのサーバーの処理キューのメッセージが、次の 3 つの web サーバーを必要があります。 または、バック グラウンドでコンピューティング集中型のタスクを実行している場合より多くのバックエンド サーバーを必要があります。

![](queue-centric-work-pattern/_static/image3.png)

自動スケーリングは、web 層とバックエンド サービスと共に動作します。 スケール アップまたはバックエンド Vm の CPU 使用率に基づいて、キュー内のタスクを処理している Vm の数をスケール ダウンできます。 または、項目の数は、キュー内に基づく自動スケールすることができます。 たとえば、10 個の項目をキューに保持しようとする自動スケールがわかります。 キューに 10 個より多くの項目がある場合は、自動スケールは Vm を追加します。 見つけられるときに、自動スケールは追加の Vm を終了します。

## <a name="adding-queues-to-the-fix-it-application"></a>追加するキューに修正プログラムにそれをアプリケーション

キュー パターンを実装するのには、修正、アプリに 2 つの変更を加える必要があります。

- ユーザーが新しい解決、タスクを送信したときに、データベースへの書き込みではなく、キューにタスクを配置します。
- キューにメッセージを処理するバックエンド サービスを作成します。

使用して、キュー、 [Azure キュー ストレージ サービス](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/)です。 別のオプションは、使用する[Azure Service Bus](https://docs.microsoft.com/azure/service-bus/)です。

使用するキュー サービスを決定するには、キューにメッセージを送受信する、アプリが必要な方法を考慮してください。

- 協調プロデューサーと競合するコンシューマーがあれば、Azure キュー ストレージ サービスの使用を検討してください。 「協力プロデューサー」では、複数のプロセスは、キューにメッセージを追加することを意味します。 「競合コンシューマー」は複数のプロセスは、それらを処理する、キューからメッセージがプルされますが、特定のメッセージは 1 つ「のコンシューマーです」でしか処理できません。 スループットでは 1 つのキューを使用することも、必要な場合は、追加のキューや追加のストレージ アカウントを使用します。
- 必要がある場合、[パブリッシュ/サブスクライブ モデル](http://en.wikipedia.org/wiki/Publish/subscribe)、Azure Service Bus キューの使用を検討します。

修正がアプリには、協調動作するプロデューサーと競合するコンシューマー モデルが収まるようにします。

他の考慮事項は、アプリケーションの可用性です。 キュー ストレージ サービスは、「使用している blob ストレージのため、これを使用して何も起こりません弊社の SLA で同じサービスの一部です。 Azure Service Bus は、独自の SLA で別のサービスです。 Service Bus のキューを使用した場合は、追加の SLA の割合を考慮する必要がおよび複合 SLA は少なくなります。 キュー サービスを選択する場合は、アプリケーションの可用性の選択の影響を理解して、確認します。 詳細については、次を参照してください。、[リソース](#resources)セクションです。

## <a name="creating-queue-messages"></a>メッセージ キューを作成します。

修正作業をキューに入れますには、web フロント エンドは、次の手順を実行します。

1. 作成、 [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx)インスタンス。 `CloudQueueClient`インスタンスを使用してキュー サービスに対する要求を実行します。
2. まだ存在しない場合は、キューを作成します。
3. 修正タスクのシリアル化します。
4. 呼び出す[CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx)メッセージをキューに配置します。

コンス トラクターでは、この作業をやってと`SendMessageAsync`の新しいメソッド`FixItQueueManager`クラスです。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

使用して次に、 [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) fixit を JSON 形式にシリアル化するライブラリ。 必要に応じてどのようなシリアル化のアプローチを使用することができます。 JSON では、XML よりも、人間が判読できるという利点があります。

実稼働品質のコードは、エラー処理ロジックを追加、一時停止、データベースが使用できなくなった場合、復旧をより正確に処理、アプリケーションの起動時にキューを作成および管理"[有害"メッセージ](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx)です。 (有害メッセージは、何らかの理由は処理できないメッセージです。 たくない場所、ワーカー ロールは継続的にしようと処理が失敗する、もう一度やり直して、失敗するに表示され、キューに留まるように有害なメッセージです。)

フロント エンドの MVC アプリケーションでは、新しいタスクを作成するコードを更新する必要があります。 リポジトリにタスクを設定する代わりに呼び出して、`SendMessageAsync`上記の方法です。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>キュー メッセージの処理

キュー内のメッセージを処理するには、バックエンド サービスが作成されます。 バックエンド サービスは、次の手順を実行する無限ループを実行します。

1. キューから次のメッセージを取得します。
2. 修正タスクにメッセージを逆シリアル化します。
3. データベースに修正作業を記述します。

バックエンド サービスをホストするを含む Azure クラウド サービスを作成、*ワーカー ロール*です。 ワーカー ロールは、バックエンドの処理を実行できる 1 つまたは複数の Vm で構成されます。 これらの Vm で実行されるコードでは、使用可能になったときに、そのキューからメッセージをプルします。 各メッセージのおを JSON ペイロードを逆シリアル化し、web 層で以前使用した同じリポジトリを使用するのには、データベースを修正、タスクのエンティティのインスタンスを記述します。

次の手順では、ロールのプロジェクトを標準的な web プロジェクトを含むソリューションに、ワーカーを追加する方法を示します。 修正したプロジェクトをダウンロードすることができます、これらの手順は既に実行されています。

最初にクラウド サービス プロジェクトを Visual Studio ソリューションに追加します。 ソリューションを右クリックし **追加**、し**新しいプロジェクト**です。 左側のウィンドウで展開**Visual c#** 選択**クラウド**です。

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

**新しい Azure クラウド サービス**ダイアログ ボックスで、展開、 **Visual c#** 左側のウィンドウでノード。 選択**ワーカー ロール**右矢印アイコンをクリックします。

![](queue-centric-work-pattern/_static/image6.png)

(追加することも通知、 *web ロール*です。 修正、Azure Web サイトでを実行することがなく、同じクラウド サービスのフロント エンド実行可能性があります。 フロント エンドとバック エンド間の接続をより簡単に調整する際にいくつかの利点があります。 しかし、このデモをシンプルにするをしている Azure App Service Web アプリにフロント エンドを保持してクラウド サービスのバック エンドを実行しているのみです。)

既定の名前は、ワーカー ロールに割り当てられます。 名前を変更するには、右側のウィンドウで、ワーカー ロール上にマウスを移動し、鉛筆アイコンをクリックします。

![](queue-centric-work-pattern/_static/image7.png)

をクリックして**OK**ダイアログを完了します。 これにより、2 つのプロジェクトが Visual Studio ソリューションに追加されます。

- 構成情報を含む、クラウド サービスを定義する Azure のプロジェクトです。
- ワーカー ロールが定義されているワーカー ロール プロジェクト。

![](queue-centric-work-pattern/_static/image8.png)

詳細については、次を参照してください。 [Visual Studio で Azure プロジェクトを作成します。](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

ワーカー ロールの内部をポーリング メッセージを呼び出すことによって、`ProcessMessageAsync`のメソッド、`FixItQueueManager`先ほど見たクラスです。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync`メソッドでは、メッセージの待機があるかどうかを確認します。 存在する場合、メッセージに逆シリアル化、`FixItTask`エンティティとエンティティがデータベースに保存します。 これは、キューが空になるまでループします。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

キューのメッセージが小さいトランザクションを生じますポーリング充電、処理するを待機しているメッセージが表示されない場合、ワーカー ロールの`RunAsync`メソッド、秒間待ちますポーリングする前にもう一度呼び出すことによって`Task.Delay(1000)`です。

Web プロジェクトでは、非同期コードを追加することができますに自動的にパフォーマンスを向上させる IIS が限られたスレッド プールを管理するためです。 ワーカー ロール プロジェクト内のケースではありません。 ワーカー ロールのスケーラビリティを高めるためには、マルチ スレッド コードを記述または非同期コードを使用して実装する[並列プログラミング](https://msdn.microsoft.com/library/ff963553.aspx)です。 サンプルは、並列プログラミングを実装していませんが、ほどの並列プログラミングを実装するコードを非同期に作成する方法を示します。

## <a name="summary"></a>まとめ

この章では、キューを中心とした作業のパターンを実装することでアプリケーションの応答性、信頼性、およびスケーラビリティを向上させる方法を説明しました。

これは、この電子ブックで説明した 13 パターンの最後のタスクは当然のことながら、他の多くのパターンがあるとするのに役立つプラクティスが成功したクラウド アプリをビルドします。 [最終章](more-patterns-and-guidance.md)これら 13 のパターンで説明されていないトピックのリソースへのリンクを提供します。

<a id="resources"></a>
## <a name="resources"></a>リソース

キューの詳細については、次のリソースを参照してください。

ドキュメント:

- [Microsoft Azure ストレージ キュー パート 1: 作業の開始](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/)です。 ローマ Schacherl による記事です。
- [バック グラウンド タスクを実行して](https://msdn.microsoft.com/library/ff803365.aspx)の第 5 章[クラウド、第 3 版へのアプリケーションの移行](https://msdn.microsoft.com/library/ff728592.aspx)Microsoft Patterns and Practices からです。 (特に、セクション[「を使用して Azure ストレージ キュー」](https://msdn.microsoft.com/library/ff803365.aspx#sec7))。
- [スケーラビリティと Azure のキュー ベースのメッセージング ソリューションのコスト効果を最大化するためのベスト プラクティス](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx)です。 Valery Mizonov、ホワイト ペーパー。
- [Azure キューと Service Bus のキューを比較する](https://msdn.microsoft.com/magazine/jj159884.aspx)です。 MSDN マガジンの記事では、使用するキュー サービスを選択するのに役立つ追加情報を提供します。 アーティクルは、Service Bus は、認証、SB キューが利用できない ACS が使用できないことを意味する ACS に依存することを示しています。 ただし、アーティクルが書き込まれるため、SB が使用するために変更されました[SAS トークン](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx)ACS する代わりにします。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)です。 非同期メッセージング入門、パイプとフィルター パターン、トランザクションの補正パターン、競合コンシューマー パターン、CQRS パターンを参照してください。
- [CQRS の概要](https://msdn.microsoft.com/library/jj554200)です。 Microsoft Patterns and Practices によって CQRS については、電子書籍します。

ビデオ:

- [フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 つの部分で構成します。 実際のお客様と Microsoft Customer ・ Advisory Team (CAT) エクスペリエンスから抽出されたストーリーで非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 Azure ストレージ サービスとキューの概要については、35:13 始まるエピソード 5 を参照してください。

> [!div class="step-by-step"]
> [前へ](distributed-caching.md)
> [次へ](more-patterns-and-guidance.md)
