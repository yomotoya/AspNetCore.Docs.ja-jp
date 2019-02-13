---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: キューを中心とした作業パターン (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 03b6950104b6f293271d9f9a0feed4071e9b1174
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577913"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>キューを中心とした作業パターン (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson]((https://twitter.com/RickAndMSFT))、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


複数のサービスを使用してにより、アプリの有効な SLA が、複合 SLA 見た前に、*製品*の個々 の Sla。 たとえば、Fix It アプリは、Websites、ストレージ、および SQL Database を使用します。 これらのサービスのいずれかが失敗した場合、アプリは、ユーザーにエラーを返します。

キャッシュは、コンテンツの読み取り専用の一時的なエラーを処理する優れた方法です。 しかし、アプリケーションが作業を実行する必要がある場合でしょうか。 たとえば、ユーザーが新しい Fix It タスクを送信アプリことはできませんだけタスクをキャッシュに格納します。 アプリは、それを処理できるように、永続的なデータ ストアに Fix It タスクを記述する必要があります。

キューを中心とした作業パターンの出番です。 このパターンは、web 層およびバックエンド サービス間の疎結合を使用できます。

次に、パターンの動作方法を示します。 アプリケーションが要求を取得すると、作業項目をキューに配置し、すぐに、応答を返します。 個別のバックエンド プロセスにキューから作業項目を取得し、処理をします。

キューを中心とした作業パターンが適しています。

- 時間がかかる (待機時間が高) は、作業します。
- 常に使用できない外部のサービスが必要な作業です。
- れた作業 (高 CPU) のリソースを消費します。
- 速度 (負荷が突然大量) 平準化からメリットを得られる作業します。

## <a name="reduced-latency"></a>待機時間の短縮

キューは、時間のかかる作業を行うたびに便利です。 タスクは、数秒またはなくなり、エンド ユーザーをブロックする代わりに、キューに作業項目を格納します。 「に取り組んでいるところ、」ユーザーに通知し、キュー リスナーを使用して、バック グラウンドでタスクを処理します。

たとえば、オンラインの小売店舗で商品を購入すると、web サイト注文を確認すぐにします。 あなたの情報が配信される、トラック内に既にあるわけです。 タスクが、キューに配置して、バック グラウンドで、与信審査、出荷、アイテムの準備を実行しているなど。

短い待機時間シナリオでは、エンド ツー エンドの合計の時間が長いタスクを同期的に行うのと比較し、キューを使用してあります。 その他のメリットがその欠点を上回る場合でも。

## <a name="increased-reliability"></a>信頼性の向上

Fix It をこれまでに見てきたのバージョンで、SQL Database バックエンド web フロント エンドは密接に結び付いています。 SQL database のサービスが利用できない場合、ユーザーはエラーを取得します。 再試行が解決しない場合は (これは、エラーがある複数の一時的な) はエラーを表示し、後でもう一度お試しに、ユーザー要求を行うことができますのみです。

![SQL Database のバック エンドに失敗すると失敗したフロント エンドのダイアグラムが表示された web](queue-centric-work-pattern/_static/image1.png)

ユーザーが Fix It タスクを送信するときに、キューを使用して、アプリは、キューにメッセージを書き込みます。 メッセージ ペイロードが、 [JSON](http://json.org/)タスクの表現。 メッセージをキューに書き込むとすぐにアプリを返し、ユーザーにすぐに成功メッセージが表示されます。

オフライン – SQL データベースなど、キュー リスナー - バックエンド サービスのいずれかの場合、ユーザーは新しいの Fix It タスクを送信できます。 バックエンド サービスは、再利用されるまで、メッセージはキューだけです。 その時点では、バックエンド サービスは、バックログ上でキャッチします。

![SQL データベースのエラーがある場合に機能する web フロント エンドの続行を示す図](queue-centric-work-pattern/_static/image2.png)

さらに、これでフロント エンドの回復性について心配することがなくより多くのバックエンド ロジックを追加することができます。 たとえばが割り当てられている、新しい Fix It たびに、所有者に電子メールまたは SMS メッセージを送信します。 電子メールや SMS サービスが使用不可能になった場合、他のすべてを処理し、電子メールまたは SMS メッセージを送信するための別のキューにメッセージを配置できます。

以前は、Web アプリが効果的な SLA&times;ストレージ&times;SQL Database = 99.7% です。 (を参照してください[障害から復旧する設計](design-to-survive-failures.md))。

キューを使用するアプリを変更する場合、web フロント エンドは、複合 99.8% の SLA の Web アプリとストレージのみに依存します。 (キューは、blob ストレージと同じ SLA に含まれるように、Azure storage サービスの一部に注意してください)。

99.8% よりもさらに良いことが必要な場合は、2 つの異なるリージョンに 2 つのキューを作成できます。 として、プライマリ、もう 1 つは、セカンダリとして指定します。 アプリでは、フェールオーバーをセカンダリ キューにプライマリ キューが使用できない場合。 両方利用できないと同時に発生する可能性が非常に小さいです。

## <a name="rate-leveling-and-independent-scaling"></a>レートが平準化して、独立したスケーリング

キューと呼ばれるものの利用も*平準化を評価*または*負荷平準化*します。

Web アプリは、急激なトラフィック増加に影響を受けやすいは多くの場合です。 自動スケールを使用して、高いレベルの web トラフィックを処理する web サーバーを自動的に追加することができます、自動スケールは負荷の突然の急増を処理するために迅速に対処できません可能性があります。 Web サーバーには、これらには、キューにメッセージを記述することで行う必要がある作業の一部をオフロードできるより多くのトラフィックを処理できます。 バックエンド サービスは、キューからメッセージを読み取るし、それらを処理します。 キューの深さを拡張または圧縮受信の負荷が変化します。

バックエンド サービスに負荷が時間のかかる作業の大部分で、web 層はでトラフィックの急増に応答より簡単にできます。 Web サーバーの数が少ないで特定のトラフィック量を処理できるため、コストを削減します。

ことができます、web 層およびバックエンド サービスを個別に拡張します。 たとえば、1 つだけサーバー処理のキュー メッセージが次の 3 つの web サーバーを必要があります。 またはをバック グラウンドでコンピューティング集中型タスクを実行している場合より多くのバックエンド サーバーを必要があります。

![](queue-centric-work-pattern/_static/image3.png)

自動スケールは、web 層およびバックエンド サービスとは動作します。 スケール アップするか、バックエンド Vm の CPU 使用率に基づいて、キュー内のタスクを処理している Vm の数をスケール ダウンします。 または、項目の数は、キュー内に基づく自動スケールすることができます。 たとえば、キューの数が 10 個の項目の状態を保持しようとする自動スケールがわかります。 キューに 10 個を超える項目がある場合は、自動スケールは Vm を追加します。 追いつくまで、自動スケールは、余分な Vm を破棄します。

## <a name="adding-queues-to-the-fix-it-application"></a>追加、修正プログラムをキューに登録アプリケーション

キューのパターンを実装するには、修正がアプリに 2 つの変更を加える必要があります。

- ユーザーは、新しい Fix It タスクを送信するときに、データベースへの書き込みではなく、キューにタスクを配置します。
- キュー内のメッセージを処理するバックエンド サービスを作成します。

キューの使用、 [Azure Queue Storage サービス](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/)します。 別のオプションは、使用する[Azure Service Bus](https://docs.microsoft.com/azure/service-bus/)します。

使用するには、どのキュー サービスを決定するには、アプリで必要なキューにメッセージを送受信する方法を考慮してください。

- 連携をもたらすプロデューサーとコンシューマーが競合があれば、Azure キュー ストレージ サービスの使用を検討してください。 「協力プロデューサー」では、複数のプロセスがキューにメッセージを追加することを意味します。 「競合コンシューマー」は複数のプロセスが、それを処理する、キューからメッセージをプルするが、特定のメッセージは「コンシューマー」には 1 つでしか処理できません。 1 つのキューを取得するよりもより多くのスループットを必要がある場合は、その他のキューや追加のストレージ アカウントを使用します。
- 必要がある場合、[パブリッシュ/サブスクライブ モデル](http://en.wikipedia.org/wiki/Publish/subscribe)、Azure Service Bus キューの使用を検討します。

Fix It アプリでは、連携をもたらすプロデューサーとコンシューマーの競合のモデルに適合させます。

別の考慮事項は、アプリケーションの可用性です。 キュー ストレージ サービスとは、これを使用して SLA に影響があるないため、blob storage を使用している同じサービスの一部です。 Azure Service Bus は、独自の SLA で別のサービスです。 Service Bus キューを使用した場合その他の SLA の割合を考慮する必要があり、複合 SLA は少なくなります。 Queue サービスを選択するときに選択したアプリケーションの可用性の影響を理解することを確認します。 詳細については、次を参照してください。、[リソース](#resources)セクション。

## <a name="creating-queue-messages"></a>キュー メッセージを作成します。

Fix It タスクをキューに配置には、web フロント エンドは、次の手順を実行します。

1. 作成、 [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx)インスタンス。 `CloudQueueClient`インスタンスを使用して、キュー サービスに対する要求を実行します。
2. まだ存在しない場合は、キューを作成します。
3. Fix It タスクをシリアル化します。
4. 呼び出す[CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx)メッセージをキューに配置します。

コンス トラクターでは、この作業をやってと`SendMessageAsync`メソッドの新しい`FixItQueueManager`クラス。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

ここで使用している、 [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) fixit を JSON 形式にシリアル化するライブラリ。 必要に応じて、どのようなシリアル化アプローチを使用することができます。 JSON では、XML よりも簡潔ながら人間が判読できるという利点があります。

実稼働品質のコードはエラー処理ロジックを追加、一時停止、データベースが使用できなくなった場合、復旧の処理をより正確に、アプリケーションの起動時にキューを作成および管理"[有害"メッセージ](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx)します。 (有害メッセージは、いくつかの理由で処理できないメッセージです。 しないワーカー ロールがそれらを処理、失敗、もう一度お試し、失敗、および具合を試す継続的に、キューにとどまるように有害なメッセージです。)

フロント エンドの MVC アプリケーションでは、新しいタスクを作成するコードを更新する必要があります。 タスクを設定する、リポジトリに、代わりに呼び出す、`SendMessageAsync`前に示したメソッド。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>キュー メッセージの処理

キュー内のメッセージを処理するには、バックエンド サービスを作成します。 バックエンド サービスは、次の手順を実行する無限ループを実行します。

1. 次のメッセージをキューから取得します。
2. Fix It タスクにメッセージを逆シリアル化します。
3. Fix It タスクをデータベースに書き込みます。

バックエンド サービスをホストするを含む Azure クラウド サービスを作成します、*ワーカー ロール*します。 ワーカー ロールは、バックエンド処理を実行できる 1 つまたは複数の Vm で構成されます。 これらの Vm で実行されるコードでは、利用可能になったキューからメッセージをプルします。 メッセージごとに、JSON ペイロードを逆シリアル化し、web 層で先ほど使用した同じリポジトリを使用して、データベースに修正がタスク エンティティのインスタンスを書き込むなります。

次の手順では、ロールのプロジェクトには、標準的な web プロジェクトを含むソリューションをワーカーを追加する方法を示します。 Fix It プロジェクトをダウンロードすることで、次の手順は既に行われました。

まず、クラウド サービス プロジェクトを Visual Studio ソリューションに追加します。 ソリューションを右クリックして**追加**、し**新しいプロジェクト**します。 左側のウィンドウで展開**Visual C#** 選択**クラウド**します。

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

**新しい Azure クラウド サービス**ダイアログ ボックスで、展開、 **Visual C#** 左側のウィンドウでノード。 選択**ワーカー ロール**右矢印アイコンをクリックします。

![](queue-centric-work-pattern/_static/image6.png)

(追加することもできますが、 *web ロール*します。 Fix It、Azure Web サイトで実行されているのではなく、同じクラウド サービスのフロント エンドを実行できます。 フロント エンドとバックエンド間の接続をより簡単に調整するいくつかの利点があります。 ただし、このデモを簡潔にするをフロント エンドを Azure App Service Web アプリを保持していますバック エンドをクラウド サービスでのみ実行中します。)

既定の名前は、ワーカー ロールに割り当てられます。 名前を変更するには、右側のウィンドウで、ワーカー ロールの上にマウスを移動し、鉛筆アイコンをクリックします。

![](queue-centric-work-pattern/_static/image7.png)

クリックして**OK**ダイアログを完了します。 これにより、2 つのプロジェクトが Visual Studio ソリューションに追加します。

- 構成情報を含む、クラウド サービスを定義する Azure のプロジェクトです。
- ワーカー ロールを定義するワーカー ロール プロジェクト。

![](queue-centric-work-pattern/_static/image8.png)

詳細については、次を参照してください。 [Visual Studio で Azure プロジェクトを作成します。](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

ワーカー ロール内でメッセージのポーリングを呼び出して、`ProcessMessageAsync`のメソッド、`FixItQueueManager`先ほどお見せするクラス。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync`待ちのメッセージがメソッドを確認します。 いずれかの場合にメッセージを逆シリアル化、`FixItTask`エンティティ、エンティティをデータベースに保存します。 これは、キューが空になるまでループします。

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

ポーリングのメッセージ キューが小規模なトランザクションが発生する請求を処理待ちメッセージが存在しないため、ワーカー ロールの`RunAsync`メソッドは待機少しポーリングする前にもう一度呼び出すことによって`Task.Delay(1000)`します。

Web プロジェクトで非同期コードを追加する自動的にパフォーマンスが向上 IIS が限られたスレッド プールを管理するためです。 ワーカー ロール プロジェクトの場合はありません。 Worker ロールのスケーラビリティを高めるためには、マルチ スレッド コードを記述または非同期コードを使用して実装する[並列プログラミング](https://msdn.microsoft.com/library/ff963553.aspx)します。 サンプルでは、並列プログラミングを実装していないが、並列プログラミングを実装するために、コードを非同期にする方法を示しています。

## <a name="summary"></a>まとめ

この章では、キューを中心とした作業パターンの実装により、アプリケーションの応答性、信頼性、およびスケーラビリティを向上させる方法を説明しました。

13 にこの電子ブックで説明したパターンの最後ですが、もちろんその他の多くのパターンがないし、するのに役立つプラクティスが成功するクラウド アプリを構築します。 [最終章](more-patterns-and-guidance.md)これら 13 のパターンで説明していないトピックのリソースへのリンクを提供します。

<a id="resources"></a>
## <a name="resources"></a>リソース

キューの詳細については、次のリソースを参照してください。

ドキュメント:

- [Microsoft Azure ストレージ キューのパート 1: 作業の開始](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/)します。 ローマ Schacherl による記事です。
- [バック グラウンド タスクを実行して](https://msdn.microsoft.com/library/ff803365.aspx)の第 5 章[移動アプリケーションをクラウド、第 3 版](https://msdn.microsoft.com/library/ff728592.aspx)Microsoft Patterns and Practices から。 (特に、セクション[「を使用して Azure Storage キュー」](https://msdn.microsoft.com/library/ff803365.aspx#sec7))。
- [スケーラビリティと Azure のキュー ベースのメッセージング ソリューションのコスト効果を最大化するためのベスト プラクティス](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx)します。 Valery Mizonov、ホワイト ペーパー。
- [Azure キューと Service Bus キューを比較する](https://msdn.microsoft.com/magazine/jj159884.aspx)します。 MSDN Magazine の記事では、使用するには、どのキュー サービスを選択するのに役立つ追加情報を提供します。 この記事では、Service Bus が認証の場合、SB キューが利用できない ACS が利用できない場合は、ACS に依存することを示しています。 ただし、書かれたものから SB が使用するために変更されました[SAS トークン](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx)ACS する代わりにします。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)します。 非同期メッセージングの基本、パイプとフィルター パターン、補正トランザクション パターン、競合コンシューマー パターン、CQRS パターンを参照してください。
- [CQRS の旅](https://msdn.microsoft.com/library/jj554200)します。 Microsoft Patterns and Practices によって CQRS については、電子書籍。

ビデオ:

- [フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 の部分で構成します。 Microsoft Customer ・ Advisory Team (CAT) の実際の顧客使用経験描画ストーリーと非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 Azure ストレージ サービスとキューの概要については、35:13 始まるエピソード 5 を参照してください。

> [!div class="step-by-step"]
> [前へ](distributed-caching.md)
> [次へ](more-patterns-and-guidance.md)
