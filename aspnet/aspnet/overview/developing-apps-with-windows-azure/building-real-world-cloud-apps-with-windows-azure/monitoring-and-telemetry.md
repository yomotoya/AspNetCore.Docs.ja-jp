---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: "監視と遠隔測定 (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント"
author: MikeWasson
description: "Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: 9baddd1836323385239206a3cf49e5938bbaff58
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>監視と遠隔測定 (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


多くの人は、お客様に連絡し、アプリケーションが下向きのときに依存します。 外にある実際のベスト プラクティスをどこにでも、特に、クラウド内にありません。 クイック通知は、の保証はありませんし、多くの場合の変更点について誤解を招くか最小限に抑えるデータを取得する通知は、ときにします。 適切な製品利用統計情報とするでき、何が起こっての対応アプリをときに何かのログ システムは不当をすぐに確認しを使用するトラブルシューティング情報が役立ちます。

## <a name="buy-or-rent-a-telemetry-solution"></a>購入またはレンタル テレメトリ ソリューション

> [!NOTE]
> この記事は、以前に書かれました[Application Insights](https://azure.microsoft.com/services/application-insights/)リリースされました。 Application Insights は、Azure で遠隔測定ソリューションの推奨されるアプローチです。 参照してください[Application Insights を ASP.NET web サイトの設定](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net)詳細についてはします。


クラウド環境に関する素晴らしい機能の 1 つは、本当に楽購入またはレンタル勝利をすることにあることです。 製品利用統計情報は、例を示します。 多くの労力、非常に低コストで、実行中に本当に適切な製品利用統計情報システムを取得できます。 Azure と統合する優れたパートナーの数多くありますいて、一部の無料のプランの何も基本的な遠隔測定を取得できるようにします。 ここでは、もののほんの一部現在使用可能な Azure で。

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

、2015 の年 3 月の時点で[Visual Studio Online の Application Insights を Microsoft](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)まだリリースされていませんが、プレビューを試すに使用します。[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#)も監視機能が含まれます。

製品利用統計情報システムを使用することができますいかに簡単であるかを表示する New Relic の設定を通じて簡単に説明します。

Azure 管理ポータルで、サービスにサインアップしてください。 をクリックして**新規**、クリックして**ストア**です。 **アドオンを選択して** ダイアログ ボックスが表示されます。 をクリックして下へスクロール**New Relic**です。

![アドオンを選択します。](monitoring-and-telemetry/_static/image1.png)

右矢印をクリックし、必要なサービス層を選択します。 このデモは、free レベルを使用します。

![アドオンをカスタマイズします。](monitoring-and-telemetry/_static/image2.png)

右矢印をクリックして、「購買」を確認し、New Relic に表示されます、ポータルでアドオンとしてします。

![注文書を確認します。](monitoring-and-telemetry/_static/image3.png)

![管理ポータルで新しい Relic アドオン](monitoring-and-telemetry/_static/image4.png)

をクリックして**接続情報**、ライセンス キーをコピーします。

![接続情報](monitoring-and-telemetry/_static/image5.png)

移動して、**構成**タブでは、ポータルで web アプリ設定**パフォーマンスの監視**に**アドオン**、設定と、**を選択してアドオン**ドロップ ダウン リスト**New Relic**です。 をクリックして**保存**です。

![[構成] タブで新しい Relic](monitoring-and-telemetry/_static/image6.png)

Visual Studio で、アプリで新しい Relic NuGet パッケージをインストールします。

![[構成] タブで開発者分析](monitoring-and-telemetry/_static/image7.png)

アプリを Azure に展開し、使用を開始します。 タスクがいくつか修正をいくつかのアクティビティを監視する New Relic の提供を作成します。

移動し、 **New Relic**  ページで、**アドオン** タブをクリックして、ポータルの**管理**です。 ポータルでは、シングル サインオン認証を使用して、資格情報をもう一度入力する必要はありませんので、New Relic の管理ポータルに送信します。 [概要] ページでは、さまざまなパフォーマンスの統計情報を表示します。 (概要ページの全体サイズを表示するイメージをクリックします。)

[![新しい Relic 監視 タブ](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

ご覧の統計情報の一部だけを次に示します。

- 異なる時間帯で平均応答時間。

    ![応答時間](monitoring-and-telemetry/_static/image10.png)
- スループット (1 分あたりの要求) で日の異なる時間にします。

    ![スループット](monitoring-and-telemetry/_static/image11.png)
- サーバーの CPU 時間は、さまざまな HTTP 要求の処理に要したです。

    ![Web トランザクションの時刻](monitoring-and-telemetry/_static/image12.png)
- アプリケーション コードのさまざまな部分に費やされた CPU 時間。

    ![トレースの詳細](monitoring-and-telemetry/_static/image13.png)
- 過去のパフォーマンス統計情報です。

    ![パフォーマンスの履歴](monitoring-and-telemetry/_static/image14.png)
- Blob サービスや信頼性の高い方法と応答性の高い方法に関する統計情報などの外部サービスへの呼び出し、サービスはされました。

    ![外部サービス](monitoring-and-telemetry/_static/image15.png)

    ![外部サービス](monitoring-and-telemetry/_static/image16.png)

    ![外部サービス](monitoring-and-telemetry/_static/image17.png)
- 世界またはここで、米国 web app でトラフィックの発生元の場所に関する情報。

    ![Geography](monitoring-and-telemetry/_static/image18.png)

レポートおよびイベントを設定することもできます。 たとえば、いつでもエラーの表示を開始すると、問題のアラートをサポート スタッフに電子メールを送信できます。

![レポート](monitoring-and-telemetry/_static/image19.png)

New Relic テレメトリ システム; の 1 つの例は、します。このすべては、他のサービスもから取得できます。 任意のコードを記述することがなく、クラウドのビューティ ・は最小限に抑えるかいない経費突然ずっと詳細については、アプリケーションの使用方法およびどのようなお客様が実際に発生しています。

<a id="log"></a>
## <a name="log-for-insight"></a>ログについて理解を深める

製品利用統計情報パッケージは、適切な最初の手順が、独自のコードをインストルメント化する必要があります。 製品利用統計情報サービスに指示する問題があるときに何か、顧客が発生していますが得られない場合がする多数のコードで何が起こってを把握します。

アプリが何をして実稼働サーバーにリモートにあるしたくありません。 数百台のサーバーを拡張しましたし、リモートにする必要がありますどれがわからないする際にについても、1 つのサーバーを作成できた場合、実用的でする必要がありますか。 ログ記録はリモート分析およびデバッグする実稼働サーバーにすることはありません必要のある十分な情報を提供する必要がありますの問題です。 ログを通じてのみの問題を特定できるできるようにするには、現在の十分な情報をログする必要があります。

### <a name="log-in-production"></a>実稼働環境でのログします。

多くの人は、問題があるし、デバッグする場合にのみ実稼働環境でトレースをオンにします。 これに関する有用なトラブルシューティング情報を取得する時間と、問題を認識している時間の間で大幅な遅延が生じることです。 取得した情報は、断続的なエラーに役立つできない可能性があります。

常にしておくこと実稼働環境でのログ記録は、記憶域が低コストがあるクラウド環境でお勧めします。 開発時間の経過と共にまたは別の時間で定期的に発生する可能性のある問題を分析するログに記録されることが既にあるし、役立ちます履歴データがあるエラーの発生時にこのようにします。 前のログを削除するパージ プロセスを自動化する可能性がありますが、ログを保持するよりも、このようなプロセスを設定する高価である場合があります。

ログ記録の追加費用は、時間とコストのすべての問題が生じたときに既に使用可能な必要な情報を節約できますのトラブルシューティングの量と比較して簡易です。 だれかがわかりますはランダム エラーしばらく約 8:00 昨日の夜ですが、エラーを覚えていない、ときにすぐにわかります問題が発生します。

4 ドル未満である 50 ギガバイトのログを手元に保つことができますか月とログ記録のパフォーマンスに対する影響は自明留意--パフォーマンスのボトルネックを避けるため、ログ記録ライブラリが非同期かどうかを確認するために 1 つの点を保持する限り、です。

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>操作が必要なログから情報を通知するログを区別します。

INFORM (してもらいたい知識) や (してもらいたい操作を行います) 操作には、ログが意図したものです。 のみアクションを実行するには、人や自動化されたプロセスを本当に必要な問題は、ACT ログを書き込むように注意します。 つかめる正規の問題を検出する作業が多すぎるを必要とする、ノイズが多すぎます ACT ログが作成されます。 場合は、ACT ログでは、サポート担当者に電子メールを送信するなどの何らかのアクションが自動的にトリガーを 1 つの問題によってトリガーされる数千のなどの操作をできるようにすることは避けます。

.NET System.Diagnostics トレースのログが割り当てられますエラー、警告、情報、およびデバッグ/Verbose レベル。 ACT ログのエラー レベルを予約して INFORM ログの下位のレベルを使用して INFORM ログから ACT を区別することができます。

![ログ レベル](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>実行時にログ記録レベルを構成します。

実稼働環境でログは常にすることをお勧め、別のベスト プラクティスを実行時に再配置またはアプリケーションを再起動せず、ログインする詳細のレベルを調整することができます、ログ記録フレームワークを実装するのには。 たとえばで、トレース機能を使用する`System.Diagnostics`エラー、警告、情報を作成して、デバッグ/詳細を記録します。 エラー、警告を常にログインして、実稼働環境で情報がログ ファイルで個別にトラブルシューティングのためデバッグ/詳細ログ記録を動的に追加できるようにしておくことをお勧めします。

Azure App service web アプリは、書き込み用の組み込みサポートを備えた`System.Diagnostics`ファイル システム、テーブル ストレージ、Blob ストレージにログ。 各記憶域の移行先の別のログ記録レベルを選択して、アプリケーションを再起動しなくても、実行時にログ記録レベルを変更することができます。 Blob ストレージのサポートによりを実行しやすく[HDInsight](https://docs.microsoft.com/azure/hdinsight/) HDInsight は、Blob ストレージを直接操作する方法を認識しているため、アプリケーション ログでジョブの分析。

### <a name="log-exceptions"></a>例外をログ出力

だけを含めないでください*例外。ToString()*ログ コードにします。 コンテキスト情報を含まれていません。 SQL のエラーの場合は SQL エラー番号を任せます。 すべての例外のコンテキスト情報自体には、例外、トラブルシューティングのために必要なすべてのものを提供するかどうかを確認する内部の例外が含まれます。 など、コンテキスト情報はサーバー名などトランザクション id とユーザー名 (がいないパスワードも、機密データ!)。

例外をログ記録が正しいことを行うには、各開発者に依存する場合もします。 確保するため、取得実行が、右側で常に、例外、ロガー インターフェイスに処理をビルド: ロガー クラスに、例外オブジェクト自体を渡すし、logger クラスに、例外データを正しくログインします。

### <a name="log-calls-to-services"></a>サービスへの呼び出しのログ

記述すること、ログ サービスにアプリ呼び出すたびに、データベースまたは REST API または任意の外部サービスにあるかどうかを強くお勧めします。 成功または失敗が各要求に要した時間を示す値だけでなく、ログに含まれます。 クラウド環境で多くの場合、完全な停止ではなく、処理が遅くに関連する問題が表示されます。 突然、1 秒あたりの取得を開始 10 ミリ秒かかるもの可能性があります。 だれかがわかりますアプリが遅い、New Relic で参照できるようにするか、独自のログが低速な理由の詳細を調べるには、どのような製品利用統計情報を検証して、体験して、したいサービスを確認できるようです。

### <a name="use-an-ilogger-interface"></a>ILogger インターフェイスを使用してください。

どのようなことをお勧め、実稼働アプリケーションを作成するときは、単純なを作成する*ILogger*インターフェイスし、がいくつかのメソッドを維持します。 これにより、簡単に後でログ記録の実装を変更して、それを実行するすべてのコードにする必要がありません。 私たちを使用している可能性があります、`System.Diagnostics.Trace`修正アプリ全体のクラスが代わりを使用して、ログ記録を実装するクラスでバック グラウンドで*ILogger*、ため*ILogger*全体でメソッドを呼び出しますアプリ。

このように、豊富なのログ記録を作成する場合は、置き換えることができます[ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs)する任意のログ メカニズムに使用します。 たとえば、アプリの増大に応じて拡張することができますより包括的なログ記録のパッケージを使用すること[NLog](http://nlog-project.org/)または[エンタープライズ ライブラリ ログ アプリケーション ブロック](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx)です。 ([Log4Net](http://logging.apache.org/log4net/)別の一般的なログ記録フレームワークが、非同期のログ記録を実行しません)。

NLog などのフレームワークを使用するための 1 つの考えられる理由は、別のデータ量の多いと、価値の高いデータ ストアにログ出力分割することを容易にすることです。 効率的に大量の ACT データにすばやくアクセスを維持しながら、に対して高速のクエリを実行する必要はありません INFORM データを格納することができます。

### <a name="semantic-logging"></a>セマンティックのログ記録

比較的新しい方法をさらに便利な診断情報を生成できるログ記録を行うには、次を参照してください。[エンタープライズ ライブラリ セマンティック ログ アプリケーション ブロック (スラブ)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)です。 スラブを使用して[Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) と[EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx)で複数の構造化データとクエリ可能なログを作成するために .NET 4.5 をサポートします。 記述する情報をカスタマイズすることにより、ログに記録するイベントの種類ごとに異なるメソッドを定義します。 たとえば、呼び出すことができます、SQL データベースのエラー ログに記録するため、`LogSQLDatabaseError`メソッドです。 このような例外がわかって重要な情報はエラー番号のため、メソッド シグネチャ内でエラー番号パラメーターを含み、記述するログ レコードに個別のフィールドとしてはエラー番号を記録する可能性があります。 数値が個別のフィールドがより簡単かつ確実に取得できますだけメッセージ文字列にはエラー番号を連結した場合に比べて、SQL エラー番号に基づくレポート。

## <a name="logging-in-the-fix-it-app"></a>修正プログラムでログ アプリ

### <a name="the-ilogger-interface"></a>ILogger インターフェイス

ここでは、 *ILogger*修正アプリでのインターフェイスです。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

これらのメソッドを使用するでサポートされている同じ 4 つのレベルでログを書き込む*System.Diagnostics*です。 TraceApi メソッドは、待機時間に関する情報を外部サービスの呼び出しをログ記録です。 デバッグ/Verbose レベルのメソッドのセットを追加することもできます。

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>ILogger インターフェイスのロガーの実装

インターフェイスの実装は非常に単純です。 基本的に同じへの呼び出し、標準*System.Diagnostics*メソッドです。 次のスニペットは、情報のメソッドの 3 つすべてと、他の 1 を示します。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>ILogger メソッドの呼び出し

呼び出しの修正、アプリのコードが例外をキャッチするたびに、 *ILogger*例外の詳細を記録するメソッド。 データベース、Blob サービス、または REST API への呼び出しを実行するたびには、呼び出しの前にストップウォッチを開始、サービスから制御が戻るとき、ストップウォッチを停止し、経過時間と成功または失敗に関する情報をログ記録します。

ログ メッセージには、クラス名とメソッド名が含まれることに注意してください。 ログ メッセージを書き込んで、アプリケーション コードのどの部分を識別するかどうかを確認することをお勧めします。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

これで、修正、アプリは、SQL データベースへの呼び出しを実行が毎回の呼び出し、呼び出されるメソッドを表示でき、正確にどのくらい時間がされます。

![ログ SQL データベースのクエリ](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

ログを参照して移動する場合、データベース呼び出しの所要時間が変数であることが確認できます。 情報がある場合に便利です。 時間の経過と共にデータベース サービスを実行する状況の履歴傾向を分析するには、アプリをすべてログに記録するためです。 たとえば、サービス高速ほとんどの時間が、要求が失敗するもあります日の特定の時間で応答が遅くなります。

たびに、アプリが新しいファイルをアップロード、ログがある、および各ファイルをアップロードするに正確にかかる所要時間を参照してください Blob サービスに対して同じ処理を行うことができます。

![Blob のアップロードのログ](monitoring-and-telemetry/_static/image23.png)

たびに、サービスを呼び出すし、今すぐ誰かの質問に問題が発生しましたが、ときにする正確に知る問題、エラーがあるかどうか、またはだけが実行されていたが遅い場合でも作成するコードのいくつかの余分な行です。 リモート サーバーにすることがなく、問題の原因を特定したり、エラーが発生しを再作成することを願って後のログ記録を有効にできます。

## <a name="dependency-injection-in-the-fix-it-app"></a>依存関係の挿入、修正プログラムで、アプリ

前に示した例では、リポジトリ コンス トラクターを取得する方法、ロガー インターフェイスの実装のでしょうか。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

使用するにはネットワーク上でのインターフェイスを実装する、アプリ[依存性の注入](http://en.wikipedia.org/wiki/Dependency_injection)(DI) と[AutoFac](http://autofac.org/)です。 DI では、コード全体で多くの場所でインターフェイスに基づくオブジェクトを使用し、インターフェイスがインスタンス化されるときに使用されるを取得する実装を 1 か所で指定する必要があるだけを行うことができます。 これにより、実装を変更する簡単: NLog ロガーに System.Diagnostics ロガーを置換するなどです。 または、自動テストのロガーのモック版の代わりに使用する場合があります。

修正アプリケーションでは、すべてのリポジトリとすべてのコント ローラーの DI を使用します。 コント ローラー クラスのコンス トラクターを取得、 *ITaskRepository*インターフェイスの同様のリポジトリが、ロガー インターフェイスを取得します。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

アプリが自動的に提供する AutoFac DI ライブラリを使用して*TaskRepository*と*ロガー*これらのコンス トラクターのインスタンス。

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

任意の場所、コンス トラクターが必要とするこのコードが基本的には、 *ILogger*インターフェイスでは、インスタンスを渡す、*ロガー*クラス、および必要なときに、 *IFixItTaskRepository*インターフェイスでは、インスタンスを渡す、 *FixItTaskRepository*クラスです。

[AutoFac](http://autofac.org/)使用できる多くの依存関係の挿入フレームワークの 1 つです。 別の人気のある 1 つは[Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx)、推奨は Microsoft Patterns and Practices でサポートされているとします。

## <a name="built-in-logging-support-in-azure"></a>Azure での組み込みのログ記録のサポート

Azure は、次の種類をサポートの[Azure App service Web アプリのログ記録](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- System.Diagnostics トレース (オンまたはオフにして、サイトを再起動しなくても、実行時にレベルを設定)。
- Windows イベント。
- IIS ログ (HTTP/FREB)。

Azure は、次の種類をサポートの[クラウド サービスでのログ記録](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- System.Diagnostics トレースです。
- パフォーマンス カウンターです。
- Windows イベント。
- IIS ログ (HTTP/FREB)。
- カスタム ディレクトリを監視します。

修正アプリでは、System.Diagnostics トレースを使用します。 System.Diagnostics web アプリでのログ記録を有効にするために必要なは、ポータルでスイッチを反転または REST API を呼び出します。 ポータルで、をクリックして、**構成**、サイトの タブで表示されるまでスクロールし、 **Application Diagnostics**セクションです。 ログ記録をオンまたはオフにし、ログ記録レベルを選択できます。 Azure のファイル システムまたはストレージ アカウントに、ログを書き込むことができます。

![アプリの診断結果と構成 タブで、サイトの診断](monitoring-and-telemetry/_static/image24.png)

Azure でのログ記録を有効にした後、作成するとき、Visual Studio の出力ウィンドウにログを確認できます。

![ストリーミング ログ メニュー](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![ストリーミング ログ メニュー](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

ストレージ アカウントに書き込まれたログすることもでき、ビューのいずれかのツールなど、Azure ストレージ テーブル サービスにアクセス**サーバー エクスプ ローラー** Visual Studio でまたは[Azure ストレージ エクスプ ローラー](https://azure.microsoft.com/features/storage-explorer/)です。

![サーバー エクスプ ローラーでのログ](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>まとめ

ボックスの製品利用統計情報システムを実装し、独自のコードでのログ記録をインストルメントし、Azure でのログの構成に非常に単純であります。 運用の問題がある場合は、製品利用統計情報システムとカスタム ログの組み合わせに役立つ、お客様の重大な問題になる前に問題を迅速に解決します。

[次のチャプター](transient-fault-handling.md)運用の問題を調査する必要のあるしないように、一時的なエラーを処理する方法を紹介します。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

製品利用統計情報は主にドキュメント:

- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)です。 インストルメンテーションと遠隔測定のガイダンス、サービスの使用状況測定のガイダンス、エンドポイント監視して、パターン、およびランタイム再構成パターンを参照してください。
- [クラウドではさむ少額: New Relic パフォーマンスが Azure の web サイトの監視を有効にする](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx)です。
- [Azure クラウド サービスで大規模なサービスのデザインに関するヒント集](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)です。 Mark Simms、Michael Thomassy、ホワイト ペーパー。 遠隔測定と診断のセクションを参照してください。
- [Application Insights による次世代開発](https://msdn.microsoft.com/magazine/dn683794.aspx)です。 MSDN マガジンの記事です。

ログは主にドキュメント:

- [セマンティック ログ アプリケーション ブロック (スラブ)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/)です。 Neil mackenzie 氏は、スラブでログ記録に意味的なケースを表示します。
- [セマンティックのログ記録を構造化し、意味のあるログを作成する](https://channel9.msdn.com/Events/Build/2013/3-336)です。 (ビデオ)ユリウス暦 Dominguez スラブでログ記録に意味的なケースを表示します。
- [EF6 SQL ログ – パート 1: 単純なログ記録](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/)です。 Arthur ヴィッカース EF 6 で Entity Framework によって実行されるクエリを記録する方法を示します。
- [接続の回復と Entity Framework、ASP.NET MVC アプリケーションでのコマンド インターセプト](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)です。 4 番目、9 つの部分から成るチュートリアル シリーズの EF 6 コマンドの途中受信機能を使用して Entity Framework によって、データベースに送信された SQL コマンドを記録する方法を示します。
- [C# 5.0 呼び出し元情報属性を使用してログを向上させる](http://www.dotnetcurry.com/showarticle.aspx?ID=972)です。 リテラルにハードコーディング、またはリフレクションを使用して手動で準備することがなく呼び出し元のメソッドの名前を簡単にログオンする方法。

主に、トラブルシューティングのドキュメント:

- [Azure のトラブルシューティング&amp;デバッグ ブログ](https://blogs.msdn.com/b/kwill/)です。
- [AzureTools –、診断ユーティリティ Azure 開発者サポート チームによって使用される](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true)です。 紹介し、ダウンロードしてさまざまな診断と監視ツールを実行する Azure VM で使用できるツールのダウンロードのリンクを提供します。 特定の VM 上の問題を診断する必要がある場合に役立ちます。
- [Visual Studio を使用して Azure App service web アプリのトラブルシューティングを行う](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)です。 System.Diagnostics トレースやリモート デバッグの概要に関するステップ バイ ステップ チュートリアルです。

ビデオ:

- [フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。 Ulrich Homann、Marc Mercuri、Mark Simms、系列を 9 つの部分で構成します。 実際のお客様と Microsoft Customer ・ Advisory Team (CAT) エクスペリエンスから抽出されたストーリーで非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 エピソード 4 および 9 は監視と遠隔測定します。 エピソード 9 MetricsHub、AppDynamics、New Relic、および PagerDuty のサービスの監視の概要が含まれています。
- [構築 Big: Azure のお客様のパート II から学んだ教訓](https://channel9.msdn.com/Events/Build/2012/3-030)です。 Mark Simms は、エラーのための設計と、すべてのインストルメント化について説明します。 フェール セーフ系列の詳細の操作方法に関する詳細情報になるに似ています。

コード サンプル:

- [クラウド サービスの基礎 azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)です。 Microsoft Azure カスタマー アドバイス チームによって作成されるサンプル アプリケーションです。 次の記事で説明したように、製品利用統計情報とログ記録のプラクティスの両方を示します。 このサンプルを使用してアプリケーションのログ記録を実装する[NLog](http://nlog-project.org/)です。 関連するドキュメントを参照してください、[一連の製品利用統計情報およびログ記録に関する 4 つの TechNet wiki の記事](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon)です。

>[!div class="step-by-step"]
[前へ](design-to-survive-failures.md)
[次へ](transient-fault-handling.md)
