---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: 監視とテレメトリ (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: b2c91b26fddc21986bf90957f7e4ef0a493d5837
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396762"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>監視とテレメトリ (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


多くの人は、そのアプリケーションがダウンしてタイミングを把握できるように顧客に依存します。 ない本当にベスト プラクティスどこでも、そして特に、クラウドではありません。 多くの場合に発生した事象に関する最小限または誤解を招くのデータを取得する通知は取得されたらと、簡単な通知の保証はありません。 適切なテレメトリとアプリ、および場合は、何が起こってを認識してありますできますログ記録システムにを使用するトラブルシューティングに役立つ情報をすぐに確認して不適切な移動します。

## <a name="buy-or-rent-a-telemetry-solution"></a>新規購入またはレンタル テレメトリ ソリューション

> [!NOTE]
> この記事では、以前に書かれました[Application Insights](https://azure.microsoft.com/services/application-insights/)リリースされました。 Application Insights は、Azure での製品利用統計情報ソリューションに対して推奨されるアプローチです。 参照してください[ASP.NET web サイトの Application Insights を設定](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net)詳細についてはします。


クラウド環境に関する優れた点の 1 つは、購入またはレンタル勝利に非常に簡単であります。 製品利用統計情報は、例を示します。 多大な労力なし、非常に低コストで、実行中に本当に適切なテレメトリ システムを取得できます。 一連の Azure と統合される優れたパートナーがあり、無料のプランがある一部の何もの基本的なテレメトリを取得できます。 ここで、のほんの一部現在利用 on Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

2015 年 3 月の時点で[for Visual Studio Online、Microsoft Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)まだリリースされていませんが、プレビューを試すが利用可能です。[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#)も監視機能が含まれています。

テレメトリ システムを使用することができます簡単であるかを表示する New Relic の設定を簡単にします。

Azure 管理ポータルのサービスのサインアップします。 クリックして**新規**、 をクリックし、**ストア**します。 **アドオンの選択** ダイアログ ボックスが表示されます。 下へスクロールし、をクリックして**New Relic**します。

![アドオンを選択します。](monitoring-and-telemetry/_static/image1.png)

右矢印をクリックし、必要なサービス レベルを選択します。 このデモは、free レベルを使用します。

![アドオンをカスタマイズします。](monitoring-and-telemetry/_static/image2.png)

右矢印をクリックして、[購入] を確認および New Relic を今すぐポータルでアドオンとして示しています。

![購入を確認します。](monitoring-and-telemetry/_static/image3.png)

![管理ポータルで新しい Relic アドオン](monitoring-and-telemetry/_static/image4.png)

クリックして**接続情報**、ライセンス キーをコピーします。

![接続情報](monitoring-and-telemetry/_static/image5.png)

移動して、**構成**タブでは、ポータルで web アプリ設定**パフォーマンスの監視**に**アドオン**、設定、**アドオンの選択**ドロップ ダウン リスト**New Relic**します。 クリックして**保存**します。

![[構成] タブの new Relic](monitoring-and-telemetry/_static/image6.png)

Visual Studio で、アプリでの New Relic NuGet パッケージをインストールします。

![[構成] タブで開発者分析](monitoring-and-telemetry/_static/image7.png)

アプリを Azure にデプロイし、使用を開始します。 いくつかを監視する New Relic のいくつかのアクティビティを提供するタスク Fix It を作成します。

戻り、 **New Relic**ページで、**アドオン** タブをクリックして、ポータルの**管理**します。 ポータルでは、シングル サインオン認証を使用して、資格情報をもう一度入力する必要はありませんので、New Relic の管理ポータルに送信します。 [概要] ページは、さまざまなパフォーマンスの統計情報を表示します。 (概要ページの完全なサイズを表示する画像をクリックします。)

[![新しい Relic 監視 タブ](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

統計情報を確認できますのほんの一部を次に示します。

- 異なる時間帯で平均応答時間。

    ![応答時間](monitoring-and-telemetry/_static/image10.png)
- 1 日の異なる時間に (1 分あたりの要求) でスループット レート。

    ![スループット](monitoring-and-telemetry/_static/image11.png)
- サーバーの CPU 時間は、別の HTTP 要求の処理に要したします。

    ![Web トランザクション時間](monitoring-and-telemetry/_static/image12.png)
- アプリケーション コードのさまざまな部分に費やされた CPU 時間。

    ![トレースの詳細](monitoring-and-telemetry/_static/image13.png)
- パフォーマンスの履歴の統計情報です。

    ![パフォーマンスの履歴](monitoring-and-telemetry/_static/image14.png)
- Blob service と信頼性の高い方法と、応答性に関する統計情報などの外部サービスへの呼び出し、サービスはされました。

    ![外部サービス](monitoring-and-telemetry/_static/image15.png)

    ![外部サービス](monitoring-and-telemetry/_static/image16.png)

    ![外部サービス](monitoring-and-telemetry/_static/image17.png)
- 世界、米国の web アプリでトラフィックの出所の場所に関する情報。

    ![Geography](monitoring-and-telemetry/_static/image18.png)

レポートおよびイベントを設定することもできます。 たとえば、いつでもエラーが表示を開始すると、問題のアラートをサポート スタッフに電子メールを送信できます。

![レポート](monitoring-and-telemetry/_static/image19.png)

New Relic は、製品利用統計情報システムの 1 つの例このすべては、その他のサービスもから取得できます。 すべてのコードを記述することがなく、クラウドの利点は最小限に抑えるかない経費、突然、アプリケーションの使用方法と、どのようなお客様は実際に発生している多くの詳細についてはします。

<a id="log"></a>
## <a name="log-for-insight"></a>Insight のログ

テレメトリ パッケージは、適切な最初の手順が、独自のコードをインストルメント化する必要があります。 テレメトリ サービスに指示する問題がある場合に通知して、顧客が発生して、、そのできる可能性がありますいない多くのコードで何が起こって洞察。

アプリの実行内容を表示する実稼働サーバーへのリモート接続する必要があるしたくないです。 数百のサーバーにスケールしましたへのリモート接続する必要があるものがわからないときでしょうが、1 つのサーバーを取得したら、実用的なする必要がありますか。 ログ記録を分析およびデバッグの実稼働サーバーへのリモート接続することはありません必要があるのに十分な情報を提供する必要がありますの問題。 ログを通じてのみの問題を特定できるように、現在の十分な情報をログする必要があります。

### <a name="log-in-production"></a>運用環境でのログインします。

多くの人は、問題があるし、デバッグする場合にのみ運用環境でトレースをオンにします。 に関する有用なトラブルシューティング情報を取得する時間と、問題を認識している時間の間で大幅な遅延をもたらします。 取得した情報が役立つ断続的なエラーのない場合があります。

常にしておくこと運用環境でログ記録は、ストレージは安価で、クラウド環境でお勧めします。 これにより、ログに記録してが既にあるし、役立つ履歴データがあるエラーの発生時に時間の経過と共に開発やさまざまなタイミングで定期的に発生する問題を分析します。 以前のログを削除するパージ プロセスを自動化できますが、ログを保管するよりも、このようなプロセスを設定する高価である場合があります。

ログ記録の追加の費用は、時間とコストのすべての問題が発生したときに既に使用可能な必要な情報を保存することができますをトラブルシューティングするのと比べて簡単です。 だれかが指示する、ランダムなエラーしばらく約 8:00、昨夜がエラーを覚えていない、ときにすぐにわかります問題が発生します。

4 ドル未満、1 か月の一方で、50 ギガバイトのログを保持することができ、ログ記録のパフォーマンスに与える影響は簡単に注意してください - パフォーマンスのボトルネックを回避するため、ログ記録ライブラリが非同期かどうかを確認するには 1 つを保持するようです。

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>操作が必要なログから情報を通知するログを区別します。

ログは INFORM (たい何かを知る) または (たい作業を行う) ACT ものです。 だけを心からアクションを実行するには、人や自動化されたプロセスを必要とする問題の ACT ログに書き込むように注意します。 正規の問題を検索していることを取捨選択する作業が多すぎるを必要とする、ノイズが多すぎる ACT ログが作成されます。 場合は、ACT ログは、サポート スタッフに電子メールを送信するなどの何らかのアクションを自動的にトリガーを 1 つの問題によってトリガーされる数千のような操作できるようにすることを回避します。

.NET System.Diagnostics トレースのログをエラー、警告、情報、およびデバッグ/Verbose レベル割り当てることできます。 ACT ログのエラー レベルを予約し、INFORM ログの下位のレベルを使用して、ACT を INFORM ログから区別できます。

![ログ レベル](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>実行時にログ記録レベルを構成します。

運用環境でログは常にも有効ですが、別のベスト プラクティスは、実行時に再デプロイしたり、アプリケーションを再起動せず、ログインしている詳細のレベルを調整することができるログ記録フレームワークを実装するためにです。 たとえばのトレース機能を使用する`System.Diagnostics`エラー、警告、情報を作成して、デバッグ/詳細ログに記録します。 エラー、警告を常にログインして、運用環境で情報がログ記録し、によって個別にトラブルシューティングのためのデバッグ/詳細ログ記録を動的に追加できるたいをお勧めします。

Azure App Service で web アプリは、書き込み用の組み込みサポートを備えた`System.Diagnostics`ファイル システム、Table storage、または Blob storage にログ。 ストレージ送信先ごとに、さまざまなログ記録レベルを選択して、アプリケーションを再起動しなくても、実行時にログ記録レベルを変更することができます。 Blob storage のサポートにより、簡単に実行[HDInsight](https://docs.microsoft.com/azure/hdinsight/) HDInsight が Blob storage を直接操作する方法を知っているため、アプリケーション ログでジョブを分析します。

### <a name="log-exceptions"></a>例外をログ出力

配置しない*例外。ToString()* ログ コードにします。 コンテキスト情報が残ります。 SQL のエラーの場合は SQL エラー数が残ります。 すべての例外のコンテキスト情報、例外自体、およびトラブルシューティングのために必要なすべてのものを提供されているかどうかを確認する内部の例外が含まれます。 たとえば、コンテキスト情報には、サーバー名、トランザクションの識別子とユーザー名 (がいない、パスワードまたはシークレット!) があります。

例外のログ記録で適切な処理を行うには、各開発者に依存する場合は、その一部はされません。 確実に、処理の右側で常に、例外、ロガーのインターフェイスに処理をビルド: ロガー クラスに、例外オブジェクト自体を渡すし、例外データをロガー クラスに正しくログインします。

### <a name="log-calls-to-services"></a>サービスへの呼び出しのログ

記述すること、ログ、アプリ、サービスを呼び出すたびにデータベースや REST API または任意の外部サービスかどうかを強くお勧めします。 成功または失敗が各要求に要した時間を示す値だけでなく、ログが含まれます。 クラウド環境では、完全な停止ではなくスローダウンに関連する問題を多くの場合、表示されます。 突然、1 秒あたりの取得を開始 10 ミリ秒かかること可能性があります。 アプリが低速だれかが指示、New Relic で参照できるようにするか、独自のログが低速な理由の詳細を学習するにはどのような製品利用統計情報サービスがあるし、検証を行う、経験としするを確認できるようします。

### <a name="use-an-ilogger-interface"></a>ILogger インターフェイスを使用して、

単純なを作成する推奨実稼働アプリケーションを作成するときに実行*ILogger*インターフェイスし、その中のいくつかのメソッドを引き続き使用します。 これにより、簡単に後でログ記録の実装を変更し、これを行うすべてのコードを行く必要がありません。 使用すること、 `System.Diagnostics.Trace` Fix It アプリ全体にわたってクラスが、代わりにを使用してログ記録を実装するクラスの内部で*ILogger*、ため*ILogger*全体でメソッドを呼び出しますアプリ。

これにより、機能豊富、ログ記録を作成する場合は置き換えることができます[ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs)する任意のログ メカニズムに使用します。 たとえば、アプリの拡大に応じてことがありますなどのより包括的なロギング パッケージを使用する[NLog](http://nlog-project.org/)または[エンタープライズ ライブラリ Logging Application Block](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx)します。 ([Log4Net](http://logging.apache.org/log4net/)が別の一般的なログ記録フレームワークが実行して非同期のログ記録します)。

NLog などのフレームワークを使用するための 1 つの考えられる理由は、ログ出力を別の高ボリュームと価値の高いデータ ストアに分割を容易にすることです。 大量の ACT データにすばやくアクセスを維持しながら、に対して高速のクエリを実行する必要はありません INFORM データを効率的に格納するために役立ちます。

### <a name="semantic-logging"></a>セマンティック ログ記録

さらに便利な診断情報を生成できるログ記録を行う比較的新しい方法は、次を参照してください。[エンタープライズ ライブラリ セマンティック ログ アプリケーション ブロック (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)します。 スラブを使用して[Windows のイベント トレース](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)(ETW) と[EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)で構造化されたクエリ可能なログを作成するために .NET 4.5 をサポートします。 記述する情報をカスタマイズすることができます、ログに記録するイベントの種類ごとに異なるメソッドを定義します。 たとえばと呼ばれる SQL データベースのエラー ログに記録するため、`LogSQLDatabaseError`メソッド。 このような例外では、メソッド シグネチャでエラー番号のパラメーターを含めるし、エラー番号を記述するログ レコードの個別のフィールドとして記録するでしたので、重要な情報は、エラー番号がわかっています。 数が個別のフィールドのためより簡単かつ確実に取得できますメッセージ文字列にはエラー番号を連結しただけの場合に比べて、SQL エラー番号に基づくレポート。

## <a name="logging-in-the-fix-it-app"></a>この修正プログラムでログ アプリ

### <a name="the-ilogger-interface"></a>ILogger インターフェイス

ここでは、 *ILogger* Fix It アプリでのインターフェイス。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

これらのメソッドを使用するでサポートされている同じ 4 つのレベルでログを書き込む*System.Diagnostics*します。 TraceApi メソッドでは、待機時間に関する情報を外部サービスの呼び出しをログに記録します。 デバッグ/Verbose レベルのメソッドのセットを追加することもできます。

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>ILogger インターフェイスのロガーの実装

インターフェイスの実装は非常に単純です。 基本的に呼び出すだけです、標準に*System.Diagnostics*メソッド。 次のスニペットでは、3 つはメソッドのすべてと、他の 1 つずつを示します。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>ILogger メソッドの呼び出し

呼び出す Fix It アプリでコードが例外をキャッチするたびに、 *ILogger*例外の詳細を記録するメソッド。 データベース、Blob service、または REST API への呼び出しを実行するたびには、呼び出しの前に、ストップウォッチを開始、サービスが返されるときに、stopwatch オブジェクトを停止して、経過時間と成功または失敗に関する情報をログにします。

ログ メッセージには、クラス名とメソッド名が含まれることに注意してください。 ログ メッセージがアプリケーション コードのどの部分を記述したことを特定するかどうかを確認することをお勧めします。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Fix It アプリに SQL Database への呼び出しが行われるたびに、これをそれを呼び出したメソッドの呼び出しを確認でき、正確にどの程度時間がかかりました。

![SQL Database のクエリ ログ](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

ログを参照して移動する場合、データベース呼び出しの所要時間がある変数が確認できます。 便利な情報である可能性があります。 時間の経過と共にデータベース サービスを実行する状況の履歴の傾向を分析できますので、これはすべて、アプリ ログに記録します。 たとえば、サービス高速ほとんどの場合は要求が失敗する可能性がありますもあります日の特定の時間で応答が遅きます。

たびに、アプリが新しいファイルをアップロード、ログがある、および各ファイルのアップロードにかかる時間を正確に確認できます Blob サービスに対して同じ処理を行うことができます。

![Blob のアップロードのログ](monitoring-and-telemetry/_static/image23.png)

たびに、サービスを呼び出すし、今すぐ言われた問題に直面した、たびを正確に知る問題をエラーがあったかどうか、または低速だけ実行されている場合でも作成するコードの 2 つの余分な行になります。 サーバーへのリモート接続しなくても、問題の原因を特定したり、エラーが発生しを再作成することと思います後のログ記録を有効にできます。

## <a name="dependency-injection-in-the-fix-it-app"></a>依存関係の挿入、修正プログラムでそのアプリ

上記の例ではリポジトリのコンス トラクターを取得する方法、ロガー インターフェイスの実装疑問に思うかもしれません。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

接続するため、インターフェイスの実装にアプリを使用して[依存関係の注入](http://en.wikipedia.org/wiki/Dependency_injection)(DI) と[AutoFac](http://autofac.org/)します。 DI では、コード全体でさまざまな場所でインターフェイスに基づくオブジェクトを使用して、インターフェイスがインスタンス化されるときに使用される実装を 1 か所で指定する必要があるだけを行うことができます。 これにより、簡単に実装の変更: NLog ロガーをで System.Diagnostics ロガーを置換するなど。 または、自動テストのため、ロガーのモック版の代わりにする可能性があります。

Fix It アプリケーションは、すべてのリポジトリとそのすべてのコント ローラーで DI を使用します。 コント ローラー クラスのコンス トラクターを取得、 *ITaskRepository*インターフェイス同様、リポジトリがロガー インターフェイスを取得します。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

アプリでは、AutoFac DI ライブラリを使用して、自動的に提供する*TaskRepository*と*ロガー*これらのコンス トラクターのインスタンス。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

任意の場所をコンス トラクターが必要とするこのコードが基本的には、 *ILogger*インターフェイスのインスタンスを渡す、*ロガー*クラス、および必要なときに、 *IFixItTaskRepository*インターフェイスのインスタンスを渡す、 *FixItTaskRepository*クラス。

[AutoFac](http://autofac.org/)使用できる多くの依存関係挿入フレームワークの 1 つです。 もう 1 つの人気のある 1 つは[Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx)、推奨は Microsoft Patterns and Practices でサポートされているとします。

## <a name="built-in-logging-support-in-azure"></a>Azure での組み込みのログ記録のサポート

Azure は、次の種類をサポートの[Azure App Service で Web アプリのログ記録](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics トレース (オンまたはオフにして、サイトを再起動しなくても、その場でレベルを設定)。
- Windows イベント。
- IIS ログ (HTTP/FREB)。

Azure は、次の種類をサポートの[Cloud Services でのログ記録](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics トレースします。
- パフォーマンス カウンターです。
- Windows イベント。
- IIS ログ (HTTP/FREB)。
- カスタム ディレクトリを監視します。

Fix It アプリでは、System.Diagnostics トレースを使用します。 System.Diagnostics は、web アプリでのログ記録を有効にするために必要なは、ポータルで、スイッチを反転または REST API を呼び出します。 ポータルで、をクリックして、**構成**にサイトのタブを下にスクロールしてを参照してください、 **Application Diagnostics**セクション。 ログ記録をオンまたはオフにし、ログ記録レベルを選択できます。 Azure のファイル システムまたはストレージ アカウントにログを書き込むことができます。

![アプリの診断結果と構成 タブで、サイトの診断](monitoring-and-telemetry/_static/image24.png)

Azure でのログ記録を有効にした後が作成されるときに、Visual Studio 出力ウィンドウでログを確認できます。

![ストリーミング ログのメニュー](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![ストリーミング ログのメニュー](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

ストレージ アカウントに書き込まれたログすることもでき、ビューのいずれかでそれらのツールですが、Azure Storage Table サービスをなど、アクセスできる**サーバー エクスプ ローラー** Visual Studio でまたは[Azure ストレージ エクスプ ローラー](https://azure.microsoft.com/features/storage-explorer/)します。

![サーバー エクスプ ローラーでのログ](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>まとめ

ボックスの製品利用統計情報システムを実装し、独自のコードでのログ記録をインストルメントし、Azure でログを構成する非常に簡単です。 運用上の問題がある場合は、テレメトリ システムとカスタム ログの組み合わせに役立つお客様の主要な問題になる前に、すばやく問題を解決します。

[[次へ] の章](transient-fault-handling.md)運用上の問題を調査する必要があるように、一時的なエラーを処理する方法について説明します。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

主に製品利用統計情報に関するドキュメント:

- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)します。 インストルメンテーションとテレメトリのガイダンス、サービスの使用状況測定のガイダンス、正常性エンドポイント監視パターン、およびランタイムの再構成のパターンを参照してください。
- [クラウドでピンチ少額: 新しい Relic のパフォーマンスを Azure Websites での監視を有効にする](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx)します。
- [Azure Cloud Services で大規模なサービスを設計するためのベスト プラクティス](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)します。 Mark Simms、Michael Thomassy、ホワイト ペーパー。 遠隔測定と診断のセクションを参照してください。
- [Application Insights による次世代開発](https://msdn.microsoft.com/magazine/dn683794.aspx)します。 MSDN Magazine の記事。

主にログ記録に関するドキュメント:

- [セマンティック ログ アプリケーション ブロック (SLAB)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)します。 Neil mackenzie 氏は、スラブのセマンティック ログ記録のケースを表示します。
- [セマンティック ログ記録を構造化し、意味のあるログを作成して](https://channel9.msdn.com/Events/Build/2013/3-336)します。 (ビデオ)ユリウス暦 Dominguez は、スラブのセマンティック ログ記録のケースを表示します。
- [EF6 SQL ログ – パート 1: 簡単なログ記録](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/)します。 Arthur Vickers では、EF 6 で Entity Framework によって実行されるクエリ ログに記録する方法を示します。
- [接続復元性と、ASP.NET MVC アプリケーションで Entity Framework とコマンド傍受](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)します。 第 4 に 9 部のチュートリアル シリーズでは、EF 6 コマンド インターセプション機能を使用して Entity Framework によってデータベースに送信された SQL コマンドを記録する方法を示します。
- [C# 5.0 呼び出し元情報属性を使用してログを向上させる](http://www.dotnetcurry.com/showarticle.aspx?ID=972)します。 リテラルにハードコーディングする、またはリフレクションを使用して手動で取得することがなく呼び出し元のメソッドの名前を簡単に記録する方法。

トラブルシューティングについて主にドキュメント:

- [Azure トラブルシューティング&amp;デバッグ ブログ](https://blogs.msdn.com/b/kwill/)します。
- [AzureTools –、診断ユーティリティ、Azure 開発者サポート チームで使用される](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true)します。 紹介し、ダウンロードしてさまざまな診断および監視ツールを実行する Azure VM で使用できるツールのダウンロード リンクを提供します。 特定の VM 上の問題を診断する必要がある場合に役立ちます。
- [Visual Studio を使用して Azure App Service で web アプリのトラブルシューティング](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)します。 System.Diagnostics トレースとリモート デバッグの概要に関するステップ バイ ステップ チュートリアルです。

ビデオ:

- [フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。 Ulrich Homann、Marc Mercuri、Mark Simms、9 部構成シリーズ。 Microsoft Customer ・ Advisory Team (CAT) の実際の顧客使用経験描画ストーリーと非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 エピソード 4 と 9 は、監視とテレメトリの詳細についてはします。 エピソード 9 には、サービス MetricsHub、AppDynamics、New Relic は、PagerDuty の監視の概要が含まれています。
- [構築大きな: Azure のお客様 - パート II から学んだ教訓](https://channel9.msdn.com/Events/Build/2012/3-030)します。 Mark Simms は、エラーのための設計と、すべてのインストルメント化について説明します。 フェール セーフ シリーズが操作方法の詳細になると似ています。

コード サンプル:

- [クラウド サービス Azure の基礎](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)します。 Microsoft Azure Customer Advisory Team で作成したサンプル アプリケーション。 次の記事で説明したように、テレメトリとログ記録のプラクティスの両方を示します。 このサンプルを使用してアプリケーションのログ記録を実装する[NLog](http://nlog-project.org/)します。 関連ドキュメントについては、次を参照してください。、[一連のテレメトリとログ記録に関する 4 つの TechNet wiki の記事](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon)します。

> [!div class="step-by-step"]
> [前へ](design-to-survive-failures.md)
> [次へ](transient-fault-handling.md)
