---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: '付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築) |Microsoft Docs'
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: baf46a87155e6368d9a81c5c5b777a491117d7b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833977"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[プロジェクトに修正プログラムをダウンロードします。](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


Azure 電子書籍で構築実世界クラウド アプリには、この付録には、Fix It サンプル アプリケーションをダウンロードすることに関する追加情報を提供する次のセクションが含まれています。

- [既知の問題](#knownissues)
- [ベスト プラクティス](#bestpractices)
- [ローカル コンピューターに Visual Studio からアプリを実行する方法](#run-in-vs)
- [Windows PowerShell スクリプトを使用して基本アプリを Azure App Service Web Apps にデプロイする方法](#deploybase)
- [Windows PowerShell スクリプトのトラブルシューティング](#troubleshooting)
- [Azure App Service Web Apps と Azure クラウド サービスを処理キューに、アプリをデプロイする方法](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>既知の問題

Fix It アプリは、もともと限りシンプルにこの電子書籍を示したパターンの一部を説明するために開発されました。 ただし、電子書籍では、実際のアプリの構築方法であるため対象となる Fix It コード レビューとテスト プロセスにリリースされたソフトウェアの作業と似ています。 数多くの問題が見つかったと、実際のアプリケーションと同様にその一部を修正しましたされ、今後のリリースにそれらのいくつかの遅延。

次の一覧には、実稼働アプリケーションではアドレス Fix It サンプル アプリケーションの初期リリースでないことにしました別の理由の 1 つの問題に対処する必要がありますが含まれています。

### <a name="security"></a>セキュリティ

- 存在しない所有者にタスクを割り当てることはできませんを確認します。
- しかことを確認します。 表示および作成するか、自分に割り当てられたタスクを変更します。
- サインイン ページと認証 cookie を HTTPS を使用します。
- 認証クッキーの期限を指定します。

### <a name="input-validation"></a>入力の検証

一般に、運用環境のアプリが行う Fix It アプリよりも多くの入力の検証。 たとえば、イメージのサイズ]、[画像のアップロードを制限するか許可されているファイルのサイズ。

### <a name="administrator-functionality"></a>管理者の機能

管理者は、既存のタスクでの所有権を変更できる必要があります。 たとえば、タスクの作成者は、機関の管理アクセスが有効にしない限り、タスクを維持するために、だれを終了、会社を退職可能性があります。

### <a name="queue-message-processing"></a>キュー メッセージの処理

Fix It アプリで処理するキュー メッセージは、最小限のコードでキューを中心とした作業パターンを説明するために簡単に設計されています。 この単純なコードは、実際の運用アプリケーションのための適切なできません。

- コードでは、各キュー メッセージが最大で 1 回処理されることは保証されません。 キューからメッセージを取得するときに、メッセージが他のキュー リスナーに表示されていない、タイムアウト期間があります。 もう一度メッセージが削除される前にタイムアウトになるには、メッセージが表示されます。 そのため、ワーカー ロール インスタンスは、メッセージの処理時間を費やして、ある理論的に、同じメッセージを 2 回、処理可能、データベース内の重複するタスクの結果します。 この問題の詳細については、次を参照してください。[を使用して Azure Storage キュー](https://msdn.microsoft.com/library/ff803365.aspx#sec7)します。
- キューのポーリングのロジックは、メッセージの取得をバッチ処理によってよりコスト効果の高い可能性があります。 呼び出すたびに[CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)トランザクションのコストがあります。 代わりに、呼び出すことができます[CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (、複数形に注意してください ')、単一のトランザクションで複数のメッセージを取得します。 Azure Storage キューのトランザクション コストは、ため、ほとんどのシナリオで大幅なコストへの影響は非常に低いです。
- キュー メッセージの処理コードの緊密なループにより、CPU アフィニティは、複数のコアの Vm を効率的に使用されません。 優れた設計は、いくつかの非同期タスクの並列実行するのにタスクの並列化を使用します。
- キューのメッセージ処理では、のみ初歩的な例外処理があります。 たとえば、コードを処理しない[有害メッセージ](https://msdn.microsoft.com/library/ms789028.aspx)します。 (メッセージの処理には、例外が発生、エラーを記録し、メッセージを削除する必要があるまたはワーカー ロールは、もう一度処理しようし、ループが無期限に続行されます。)

### <a name="sql-queries-are-unbounded"></a>SQL クエリの制限がないです。

現在の修正、コードは、インデックス ページのクエリが返す可能性があります行の数に制限を配置しません。 膨大な量のタスクがデータベースに入力された場合、受信した結果のリストのサイズにはパフォーマンスの問題可能性があります。 ソリューションでは、ページングを実装します。 例については、次を参照してください。[並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework でページング](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)します。

### <a name="view-models-recommended"></a>推奨モデルの表示

Fix It アプリでは、FixItTask エンティティ クラスを使用して、コント ローラーとビューの間で情報を渡します。 ビュー モデルを使用することをお勧めします。 ドメイン モデル (FixItTask エンティティ クラスなど) は、データの表示、ビュー モデルをデザインするときにデータ永続化のために必要なものに基づいて設計されています。 詳細については、次を参照してください。 [12 の ASP.NET MVC のベスト プラクティス](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx)します。

### <a name="secure-image-blob-recommended"></a>セキュリティで保護されたイメージの blob がお勧めします

つまりイメージの URL を見つけた他のユーザー アクセスできる、パブリックとしてイメージを Fix It アプリ ストアにアップロードされます。 パブリックではなく、イメージを保護する可能性があります。

### <a name="no-powershell-automation-scripts-for-queues"></a>キューの PowerShell オートメーション スクリプトはありません。

オートメーションのサンプル PowerShell スクリプトは、のみの修正が完全に Azure App Service Web Apps で実行される基本バージョン用に作成されました。 設定して、web アプリとキューの処理に必要なクラウド サービス環境に展開するためスクリプトを指定していません。

### <a name="special-handling-for-html-codes-in-user-input"></a>ユーザー入力に HTML コードの特別な処理

ASP.NET には、さまざまな方法が悪意のあるユーザーがユーザー入力テキスト ボックスにスクリプトを入力してクロス サイト スクリプティング攻撃を試みることもありますが自動的にできないようにします。 MVC`DisplayFor`タスクを表示するために使用するヘルパーがタイトルし、自動的にブラウザーに送信される HTML エンコードの値をメモします。 ただし、運用アプリでその他の対策をする可能性があります。 詳細については、次を参照してください。 [asp.net 要求の検証](https://msdn.microsoft.com/library/hh882339.aspx)です。

<a id="bestpractices"></a>
## <a name="best-practices"></a>ベスト プラクティス

コード レビューで発見し、修正、アプリの元のバージョンのテストの後に修正されているいくつかの問題を次に示します。 いない認識すること、特定のベスト プラクティスのいくつか単純に元のプログラマによっていくつか発生したため、コードが簡単に作成されており、リリースされたソフトウェアの意図しません。 このレビューから学んだものを使用する必要がある場合、ここでの問題を一覧表示しているし、他の web アプリの開発も、ユーザーに役立つテストにあります。

### <a name="dispose-the-database-repository"></a>データベースのリポジトリを破棄します。

`FixItTaskRepository`クラスは、Entity Framework を破棄する必要があります`DbContext`インスタンス。 実装することで、これを行った`IDisposable`で、`FixItTaskRepository`クラス。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

AutoFac を自動的に破棄するメモ、`FixItTaskRepository`のインスタンスのため、明示的に破棄する必要はありません。

削除することも、`DbContext`からメンバー変数`FixItTaskRepository`、代わりにローカルに作成および`DbContext`各リポジトリ メソッド内で変数内で、`using`ステートメント。 例えば:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>シングルトンに DI よう登録します。

以降のインスタンスを 1 つだけ、`PhotoService`クラスと`Logger`クラスが必要なこれらのクラスを指定する必要があります[依存関係の挿入の 1 つのインスタンスとして登録されている](https://code.google.com/p/autofac/wiki/InstanceScope)で*DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>セキュリティ: ユーザーにエラーの詳細を表示しません。

アプリ Fix It 元は、一般的なエラー ページがあるし、データベース接続エラーなどのいくつかの例外は、ブラウザーに表示されている完全なスタック トレースになる可能性がありますので、UI までのすべての例外バブルに任せてしませんでした。 エラーの詳細については、悪意のあるユーザーからの攻撃を促進できる場合があります。 ソリューションでは、例外の詳細を記録し、エラーの詳細が含まれていないユーザーにエラー ページを表示します。 Fix It アプリが既にログ記録、およびエラー ページを表示するために追加しました`<customErrors mode=On>`Web.config ファイルにします。

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

これにより、既定で*Views\Shared\Error.cshtml*エラーを表示します。 カスタマイズできる*Error.cshtml*または独自のエラー ページのビューを作成し、追加、`defaultRedirect`属性。 特定のエラーの別のエラー ページを指定することもできます。

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>セキュリティ: は、作成者によって編集タスクのみを許可します。

ダッシュ ボードのインデックス ページは、ログオンしているユーザーによって作成されたタスクだけを表示しますが、悪意のあるユーザーは別のユーザーのタスクに ID を持つ URL を作成する可能性があります。 内のコードを追加しました*DashboardController.cs*をその場合は、404 を返します。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>例外を飲み込むはありません。

アプリ Fix It 元だけが null を返しましたログインすると、SQL クエリの結果として生じた例外。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

クエリが成功しましたが、同じ行が返されませんでしたかのように、ユーザーに外観になります。 ソリューションのキャッチとログ記録の後に例外を再スローすることです。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>ワーカー ロールのすべての例外をキャッチします。

VM を再利用すると、ワーカー ロールでハンドルされない例外、try-catch ブロックで実行し、すべての例外を処理するすべてをラップするためです。

### <a name="specify-length-for-string-properties-in-entity-classes"></a>エンティティ クラスのプロパティの文字列長を指定します。

Fix It アプリの元のバージョンの単純なコードを表示するために、FixItTask エンティティのフィールドの長さを指定していないし、その結果、データベースでは、varchar (max) として定義されました。 その結果、UI がほぼすべての量の入力、受け入れていました。 Web ページと、データベース内の列のサイズで両方のユーザーに適用される指定の長さセット制限を入力します。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>変更することが予期されないときに、プライベート メンバーを readonly としてマークします。

たとえば、`DashboardController`クラスのインスタンス`FixItTaskRepository`が作成され、変更するには想定されていませんとして定義したので[readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx)します。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>リストを使用します。一覧ではなく Any() します。Count() &gt; 0

リスト内の 1 つまたは複数の項目が指定した条件に一致するかどうかが重要する場合を使用して、[任意](https://msdn.microsoft.com/library/bb534972.aspx)メソッドを返すため、条件に適合する項目が見つかるとすぐに対し、`Count`メソッドは常に反復処理するにはすべてのアイテム。 ダッシュ ボード*Index.cshtml*ファイルは、このコードを最初から。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

これを変更しました。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>MVC ヘルパーを使用して MVC ビューで Url を生成します。

**作成修正**Fix It アプリ ハード ホーム ページで、ボタンは、アンカー要素をコード化されました。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

このようなビュー/アクション リンクは、使用する方がよい、 [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx)例については、HTML ヘルパー。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Thread.Sleep ではなく Task.Delay を使用して、ワーカー ロール

新しいプロジェクト テンプレートは、`Thread.Sleep`サンプルで worker ロールでは、スレッドをスリープ状態の原因はコード不要な追加のスレッドを生成するスレッド プールが発生することができます。 使用して回避することができます[Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx)代わりにします。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Async void を避ける

非同期メソッドが値を返す必要がある場合を返す、`Task`型なく`void`します。

この例は、`FixItQueueManager`クラス。 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

使用する必要があります`async void`最上位レベルのイベント ハンドラーに対してのみです。 としてメソッドを定義する場合`async void`、呼び出し元ができない**await**メソッドまたはメソッドをスローするすべての例外をキャッチします。 詳細については、次を参照してください。[非同期プログラミングのベスト プラクティス](https://msdn.microsoft.com/magazine/jj991977.aspx)します。 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>キャンセル トークンを使用して、ワーカー ロールのループを中断するには

通常、**実行**をワーカー ロールでのメソッドには、無限ループが含まれています。 Worker ロールが停止しているときに、 [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)メソッドが呼び出されます。 このメソッドは、内部で行われている処理の取り消しを使用する必要があります、**実行**メソッドと終了適切にします。 それ以外の場合、操作の途中でプロセスを終了する可能性があります。

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>自動 MIME スニッフィングのプロシージャをオプトアウトします。

場合によっては、Internet Explorer は web サーバーで指定された型よりもさまざまな MIME の種類を報告します。 たとえば、HTTP 応答ヘッダーのコンテンツ タイプで提供されるファイルで Internet Explorer がコンテンツにある HTML を見つけた場合: テキスト/プレーン コンテンツを HTML として表示すること、Internet Explorer が決定します。 残念ながら、この「MIME スニッフィング」可能性も信頼されていないコンテンツをホストするサーバーのセキュリティの問題にします。 Internet Explorer 8 に、この問題を解決するには、MIME の種類の決定コードへの変更の数が確立およびにより、アプリケーション開発者[MIME スニッフィングのオプトアウト](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)します。 次のコードに追加された、 *Web.config*ファイル。

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>バンドルと縮小が有効にします。

Visual Studio では、新しい web プロジェクトを作成するときの JavaScript ファイルのバンドルと縮小は既定で有効ありません。 BundleConfig.cs でのコード行を追加しました。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>認証クッキーの有効期限のタイムアウトを設定します。

既定では、認証 cookie は、2 週間以内に期限切れ。 短い時間の方が安全です。 この設定を変更する*StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>ローカル コンピューターに Visual Studio からアプリを実行する方法

Fix It アプリを実行する 2 つの方法はあります。

- 新しいタスクを SQL database に直接書き込むベースのアプリケーションを実行します。
- キューに加えて、バックエンド サービスを使用してタスクを作成するアプリケーションを実行します。 キュー パターンが、」の章で説明されている[キューを中心とした作業パターン](queue-centric-work-pattern.md)します。

<a id="runbase"></a>
### <a name="run-the-base-application"></a>ベースのアプリケーションを実行します。

1. インストール[Visual Studio 2013 または Visual Studio 2013 Express for Web](https://www.visualstudio.com/downloads)します。
2. インストール、 [Azure SDK for .NET for Visual Studio 2013。](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. .Zip ファイルをダウンロード、 [MSDN コード ギャラリー](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)します。
4. ファイル エクスプ ローラーで .zip ファイルを右クリックして、プロパティ をクリックし、プロパティ ウィンドウで ブロック解除 をクリックします。
5. ファイルを解凍します。
6. Visual Studio を起動する .sln ファイルをダブルクリックします。
7. [ツール] メニューで、ライブラリ パッケージ マネージャーでは、そのパッケージ マネージャー コンソールをクリックします。
8. パッケージ マネージャー コンソール (PMC) では、復元をクリックします。
9. Visual Studio を終了します。
10. 開始、 [Azure ストレージ エミュレーター](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)します。
11. 前の手順で終了するソリューション ファイルを開き、Visual Studio を再起動します。
12. FixIt プロジェクトがスタートアップ プロジェクトとして設定されていることを確認し、プロジェクトを実行するには、CTRL + F5 キーを押します。

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>キューの処理でアプリケーションを実行します。

1. 指示に従い[ベースのアプリケーションを実行](#runbase)ブラウザーを終了して、Visual Studio を閉じます。
2. 管理者特権で Visual Studio を起動します。 (Azure コンピューティング エミュレーターを使用して管理者特権が必要です)。
3. アプリケーションで*Web.config*ファイル、 *MyFixIt* (web プロジェクト) をプロジェクトでの値を変更、 `appSettings/UseQueues` "true"に。 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. 場合、 [Azure ストレージ エミュレーター](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)がまだ実行中、もう一度開始します。
5. FixIt web プロジェクトと MyFixItCloudService プロジェクトを同時に実行します。

    Visual Studio 2013 を使用します。

   1. F5 キーを押して、FixIt プロジェクトを実行します。
   2. **ソリューション エクスプ ローラー**MyFixItCloudService プロジェクトを右クリックし、クリックして**デバッグ** -- **新しいインスタンスを開始**します。

      Visual Studio 2013 Express for Web の使用。

   3. ソリューション エクスプ ローラーでは、FixIt ソリューションを右クリックして**プロパティ**します。
   4. 選択**マルチ スタートアップ プロジェクト**.
   5. **アクション**MyFixIt と MyFixItCloudService、下のドロップダウン リストで選択**開始**します。
   6. **[OK]** をクリックします。
   7. F5 キーを押して、両方のプロジェクトを実行します。

      MyFixItCloudService プロジェクトを実行すると、Visual Studio は、Azure コンピューティング エミュレーターを起動します。 ファイアウォールの構成によっては、ファイアウォール経由のエミュレーターを許可する必要があります。

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Windows PowerShell スクリプトを使用して基本アプリを Azure App Service Web Apps にデプロイする方法

説明するために、[自動化すべて](automate-everything.md)パターンでは、修正、アプリの Azure 環境を設定し、プロジェクトを新しい環境にデプロイするスクリプトが付属します。 次の手順では、スクリプトを使用する方法について説明します。

キューを使用せずに Azure で実行するローカル キューで実行する変更を加えた場合は、次の手順に進む前に false に UseQueues appSetting 値を設定することを確認します。

これらの手順は、既にダウンロードしてあると Fix It ソリューションをローカルで実行し、Azure があるかアカウントまたは Azure サブスクリプションがある想定しています。 管理が承認されています。

1. インストール、 **Azure PowerShell**コンソール。 手順については、次を参照してください。[をインストールして、Azure PowerShell を構成する方法](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)します。

    このカスタマイズされたコンソールを構成するには、Azure サブスクリプションを使用します。 Azure モジュールがインストールされている、 *Program Files*ディレクトリ、Azure PowerShell コンソールを使用するたびに自動的にインポートされます。

    Windows PowerShell ISE などの別のホスト プログラムで作業する場合に使用することを確認して、 [Import-module](https://go.microsoft.com/fwlink/?LinkID=141553)コマンドレットを Azure モジュールをインポートまたはモジュールの自動インポートをトリガーする、Azure モジュールのコマンドを使用します。
2. Azure PowerShell を起動、**管理者として実行**オプション。
3. 実行、 [Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941)コマンドレットに、Azure PowerShell 実行ポリシーを設定する`RemoteSigned`します。 入力**Y** (Yes) のポリシーの変更を完了します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    この設定では、デジタル署名されていないローカル スクリプトを実行することができます。 (実行ポリシーを設定することもできます`Unrestricted`後で、ブロックを解除する手順の必要はありませんが、これは、セキュリティ上の理由から推奨されません。)。
4. 実行、`Add-AzureAccount`コマンドレット、アカウントの資格情報を PowerShell を使用します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    これらの資格情報は、一定期間後に有効期限が切れるし、再実行する必要が、`Add-AzureAccount`コマンドレット。 この電子書籍が書き込まれると資格情報の有効期限が切れる前に制限時間は 12 時間です。
5. 複数のサブスクリプションがある場合は、テスト環境を作成するサブスクリプションを指定する Select-azuresubscription コマンドレットを使用します。
6. 使用して、同じ Azure サブスクリプションの管理証明書をインポート、`Get-AzurePublishSettingsFile`と`Import-AzurePublishSettingsFile`コマンドレット。 これらのコマンドレットの最初の証明書ファイルをダウンロードして、2 つ目でインポートするためにそのファイルの場所を指定します。 > [!IMPORTANT]
   > 安全な場所にダウンロードしたファイルを保持または削除したら、Azure サービスの管理に使用できる証明書が含まれています。

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    証明書が使用される SQL Database サーバー上のファイアウォール規則を設定するには、開発用コンピューターの IP アドレスを検出する REST API 呼び出し。
7. 実行、 [Set-location](https://go.microsoft.com/fwlink/p/?linkid=293912)コマンドレット (エイリアスは`cd`、 `chdir`、および`sl`) スクリプトを含むディレクトリに移動します。 (ファイルに配置している、 *Automation* Fix It ソリューション フォルダー内のフォルダーです)。ディレクトリ名にスペースが含まれている場合は、引用符で囲まれたパスを配置します。 たとえばに移動するため、`c:\Sample Apps\FixIt\Automation`ディレクトリは、次のコマンドを入力する可能性があります。

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. これらのスクリプトを実行する Windows PowerShell を許可するのには、使用、 [Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021)コマンドレット。 (スクリプトは、インターネットからダウンロードされているためにがブロックされます)

    > [!WARNING]
    > セキュリティ - 実行する前に`Unblock-File`スクリプトまたは実行可能ファイルで、メモ帳で開きのコマンドを確認および悪意のあるコードが含まれていないことを確認します。

    たとえば、次のコマンドを実行、`Unblock-File`コマンドレットは、現在のディレクトリ内のすべてのスクリプトにします。

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. (キューの処理) ベースの web アプリを作成するには、そのアプリを修正、環境の作成スクリプトを実行します。

    必要な`Name`パラメーターは、データベースの名前を指定し、スクリプトを作成するストレージ アカウントにも使用します。 名前は azurewebsites.net ドメイン内でグローバルに一意である必要があります。 Fixit やテストなど (または fixitdemo、例のようにも)、重複する名前を指定する場合、`New-AzureWebsite`コマンドレットは、競合を報告する内部エラーで失敗します。 スクリプトは、web アプリ、ストレージ アカウント、およびデータベースの名前の要件に準拠するすべて小文字に名前を変換します。

    必要な`SqlDatabasePassword`パラメーターは、SQL Database 用に作成される管理者アカウントのパスワードを指定します。 パスワードに特殊な XML 文字を含めないでください (&amp; &lt; &gt; ;)。 これは、スクリプトに書かれて、Azure の制限ではない方法の制限です。

    たとえば、"fixitdemo"という名前の web アプリを作成して SQL Server の管理者パスワード"Passw0rd1"を使用する場合は、次のコマンドを入力します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    名前は azurewebsites.net ドメインで一意である必要があり、パスワードがパスワードの複雑さの SQL データベースの要件を満たす必要があります。 (例 Passw0rd1 が要件を満たしています。)

    コマンドの最初に注意してください"です。\" Windows PowerShell では、悪意のあるスクリプトの実行を防ぐため、スクリプトを実行するときに、スクリプト ファイルへの完全修飾パスを提供することが必要です。 現在のディレクトリを示すために、ドットを使用することができます (".\")または、完全修飾パスを指定します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    スクリプトの詳細については、使用、`Get-Help`コマンドレット。

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    使用することができます、 `Detailed`、 `Full`、 `Parameters`、および`Examples`返されるヘルプをフィルター処理、Get-help コマンドレットのパラメーター。

    スクリプトが失敗したり、「New-azurewebsite:: 呼び出す Set-azuresubscription と Select-azuresubscription 最初に、」などのエラーを生成する場合は、Azure PowerShell の構成を完了していない可能性があります。

    ように作成されたリソースを表示する、Azure 管理ポータルを使用するには、スクリプトの完了後、[自動化すべて](automate-everything.md)」の章。
10. FixIt プロジェクトを新しい Azure 環境に展開するには、使用、 *AzureWebsite.ps1*スクリプト。 例えば:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    デプロイが完了したら、修正、Azure で実行されているとブラウザーが開きます。

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Windows PowerShell スクリプトのトラブルシューティング

これらのスクリプトを実行しているときに発生する最も一般的なエラーに関連するアクセス許可。 確認します`Add-AzureAccount`と`Import-AzurePublishSettingsFile`が正常に実行し、同じ Azure サブスクリプションのためにします。 場合でも`Add-AzureAccount`が成功したが、もう一度実行する場合があります。 によって追加されたアクセス許可`Add-AzureAccount`12 時間以内に期限切れにします。

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>オブジェクト参照がオブジェクト インスタンスに設定されていません。

「オブジェクト参照、オブジェクトのインスタンスに設定されていない」は、Windows PowerShell がプロセス (これは、null 参照の例外です) にオブジェクトを見つけることはできませんが実行されるなど、スクリプトは、エラーを返した場合、`Add-AzureAccount`コマンドレットとスクリプトをもう一度やり直してください。

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: サーバーには、内部エラーが発生しました。

`New-AzureWebsite`コマンドレットでは、azurewebsites.net ドメインで名前が一意でないときに内部エラーが返されます。 エラーを解決するには、Name パラメーターには、名前の別の値を使用して、*新規 AzureWebsiteEnv.ps1*します。

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>スクリプトを再起動します。

再起動する必要がある場合、*新規 AzureWebsiteEnv.ps1*スクリプト「スクリプトが完了しました」のメッセージを印刷する前に、しなかったために、前に作成したスクリプトが停止しているリソースを削除する可能性があります。 たとえば、スクリプトを既に作成して ContosoFixItDemo web アプリと同じ名前のスクリプトをもう一度実行する場合、名前が使用されているためにのスクリプトでは失敗します。

停止するまでに作成したスクリプトを対象のリソースを確認するには、次のコマンドレットを使用します。

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: このコマンドレットを実行するにはデータベース サーバーの名前をパイプ`Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

これらのリソースを削除するには、次のコマンドを使用します。 場合は、データベース サーバーを削除すると、自動的にデータベースを削除するサーバーに関連付けられているに注意してください。

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Azure App Service Web Apps と Azure クラウド サービスを処理キューに、アプリをデプロイする方法

キューを有効にするには、MyFixIt\Web.config ファイルで、次の変更を行います。 `appSettings`の値を変更`UseQueues`"true"にします。 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

説明に従って、Azure App Service で web アプリに、MVC アプリケーションを展開し、[以前](#deploybase)します。

次に、新しい Azure クラウド サービスを作成します。 Fix It アプリに含まれているスクリプトを作成またはしないでこの Azure portal を使用する必要がありますので、クラウド サービスをデプロイします。 ポータルで、次のようにクリックします。**新規** -- **コンピューティング**–**クラウド サービス** -- **簡易作成**、し、URL を入力します。データ センターの場所。 Web アプリを配置した同じデータ センターを使用します。

![](the-fix-it-sample-application/_static/image1.png)

クラウド サービスを展開する前に、構成ファイルの一部を更新する必要があります。

MyFixIt.WorkerRoler\app.config で `connectionStrings`の値を置き換える、 `appdb` SQL データベースの実際の接続文字列で接続文字列。 接続文字列は、ポータルから取得できます。 ポータルで、次のようにクリックします。 **SQL データベース** - **appdb** - **ADO .Net、ODBC、PHP、および JDBC の接続文字列を SQL データベースの表示**します。 ADO.NET 接続文字列をコピーして、app.config ファイルに値を貼り付けます。 置換"{、\_パスワード\_ここ}"をデータベースのパスワード。 (内のデータベース パスワードを指定した MVC アプリをデプロイするスクリプトを使用すると仮定すると、`SqlDatabasePassword`パラメーターのスクリプトを作成します)。

結果は、次のようになります。

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

同じ MyFixIt.WorkerRoler\app.config ファイルで  `appSettings`、Azure ストレージ アカウントの 2 つのプレース ホルダーの値を置き換えます。

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

アクセス キーは、ポータルから取得できます。 参照してください[ストレージ アカウントを管理する方法](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)します。

MyFixItCloudService\ServiceConfiguration.Cloud.cscfg、Azure ストレージ アカウントの同じ 2 つのプレース ホルダー値に置き換えてください。

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

クラウド サービスをデプロイする準備が整いました。 ソリューション エクスプ ローラーでは、MyFixItCloudService プロジェクトを右クリックして**発行**します。 詳細については、次を参照してください。"[アプリケーションを Azure にデプロイ](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)"、第 2 部である[このチュートリアル](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)します。

> [!div class="step-by-step"]
> [前へ](more-patterns-and-guidance.md)
