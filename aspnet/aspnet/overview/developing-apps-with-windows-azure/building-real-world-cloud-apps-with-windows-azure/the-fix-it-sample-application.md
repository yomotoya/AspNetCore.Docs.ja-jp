---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: '付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築) |Microsoft ドキュメント'
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 9a1fa36b34c4783b101bb27bc6931241e9251e10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876476"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>付録: 修正プログラム、サンプル アプリケーション (Azure での実際のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[プロジェクトに修正プログラムをダウンロードします。](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


この付録の内容、実世界クラウド アプリのビルド Azure 電子書籍をするには、修正、サンプル アプリケーションをダウンロードすることに関する追加情報を提供する次のセクションが含まれています。

- [既知の問題](#knownissues)
- [ベスト プラクティス](#bestpractices)
- [ローカル コンピューターで Visual Studio からアプリを実行する方法](#run-in-vs)
- [Azure App Service Web Apps を Windows PowerShell スクリプトを使用して基本アプリを展開する方法](#deploybase)
- [Windows PowerShell スクリプトのトラブルシューティング](#troubleshooting)
- [Azure App Service Web Apps を Azure クラウド サービスの処理キューにアプリを展開する方法](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>既知の問題

修正、アプリはもともと限りシンプルにこの電子ブックに表示されるパターンを説明するために開発されました。 ただし、電子書籍では、現実世界のアプリの構築方法であるためお対象となる修正コード レビューとテスト プロセスにリリースされたソフトウェアで行うようおに似ています。 問題の数が見つかりましたおよびと同様に、実際のアプリケーションでは、一部の修正は、以降のリリースにそれらのいくつかの遅延はします。

次の一覧には、実稼働アプリケーションでは、修正、サンプル アプリケーションの最初のリリースのアドレスにないことにしました別の理由の 1 つの対処する必要がある問題が含まれています。

### <a name="security"></a>セキュリティ

- 存在しない所有者にタスクを割り当てることはできませんを確認します。
- 実行できますのみを確認してください。 表示および作成するか、自分に割り当てられているタスクを変更します。
- サインイン ページと認証 cookie を HTTPS を使用します。
- 認証 cookie の制限時間を指定します。

### <a name="input-validation"></a>入力の検証

一般に、運用アプリには操作修正アプリよりも多くの入力の検証。 イメージのサイズなど、またはイメージ ファイルのサイズのアップロードを制限する必要があります。

### <a name="administrator-functionality"></a>管理者の機能

管理者は、既存のタスクの所有権を変更できる必要があります。 など、タスクのクリエーターは誰は機関の管理アクセスが有効にしない限り、タスクを維持するために、終了、会社を残すことがあります。

### <a name="queue-message-processing"></a>キュー メッセージの処理

キュー メッセージの処理の修正、アプリでは、最小限のコードでキューを中心とした作業のパターンを説明するために単純にするよう設計されました。 この単純なコードは、実稼働アプリケーションの適切なできません。

- コードでは、各キューのメッセージが最大で 1 回処理されることは保証されません。 キューからメッセージを取得する場合は、によって、メッセージが他のキュー リスナーに表示されていない、タイムアウト期間。 もう一度、メッセージが削除される前にタイムアウトになるには、メッセージが表示されます。 そのため場合は、ワーカー ロール インスタンスは、メッセージの処理時間を費やす、可能であれば理論的には、同じメッセージに 2 回処理に対して、データベース内の重複するタスクの結果として得られます。 この問題の詳細については、次を参照してください。[を使用して Azure ストレージ キュー](https://msdn.microsoft.com/library/ff803365.aspx#sec7)です。
- キューのポーリングのロジックは、メッセージの取得をバッチ処理によってよりコスト効果の高い可能性があります。 呼び出すたびに[CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx)、トランザクション コストが発生します。 代わりに、呼び出せます[CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (複数形に注意してください ')、単一のトランザクションで複数のメッセージを取得します。 Azure ストレージ キューのトランザクションのコストは非常に低いコストへの影響ほとんどのシナリオで大きくします。
- CPU 関係は、マルチコア仮想マシンを効率的に使用しない、キューのメッセージ処理のコードにループが発生します。 優れた設計では、同時に複数の非同期タスクを実行するのにタスクの並列化を使用します。
- キューのメッセージ処理は、のみ基本的な例外処理がします。 たとえば、コードを処理しない[有害メッセージ](https://msdn.microsoft.com/library/ms789028.aspx)です。 (メッセージの処理には、例外が発生とする必要が、エラーを記録してメッセージを削除またはワーカー ロールは再度、プロセスを再試行してください、ループが無期限に続行されます。)

### <a name="sql-queries-are-unbounded"></a>SQL クエリの制限がないです。

現在の修正、コードでないを配置の制限数の行がインデックス ページのクエリが返す可能性があります。 入力される場合は作業の量が多いデータベースに、受信した結果リストのサイズには、パフォーマンスの問題可能性があります。 ソリューションでは、ページングを実装します。 例については、次を参照してください。[並べ替え、フィルター処理、および ASP.NET MVC アプリケーションで Entity Framework とページング](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)です。

### <a name="view-models-recommended"></a>推奨モデルの表示

修正アプリでは、FixItTask エンティティ クラスを使用して、コント ローラーとビューの間で情報を渡します。 モデルの表示を使用することをお勧めします。 データのプレゼンテーションについては、ビュー モデルをデザインするときにデータの永続化に必要なものを軸には、ドメイン モデル (FixItTask エンティティ クラスなど) を設計します。 詳細については、次を参照してください。 [12 の ASP.NET MVC のベスト プラクティス](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx)です。

### <a name="secure-image-blob-recommended"></a>推奨されるセキュリティで保護されたイメージの blob

修正のアプリ ストアでは、イメージをパブリックとして URL を検索する人、イメージにアクセスできることを意味をアップロードします。 パブリックではなく、イメージを保護する可能性があります。

### <a name="no-powershell-automation-scripts-for-queues"></a>キューの PowerShell オートメーション スクリプトではありません。

PowerShell オートメーションのサンプル スクリプトは、修正、Azure App Service Web Apps でまったく実行されているの基本バージョンに対してのみ作成されています。 設定して、web アプリケーションとキューの処理に必要なクラウド サービス環境への展開のおスクリプトを提供していません。

### <a name="special-handling-for-html-codes-in-user-input"></a>ユーザー入力の HTML コードに対して特別な処理

ASP.NET は、さまざまな方法が悪意のあるユーザーがユーザー入力テキスト ボックスにスクリプトを入力することでクロスサイト スクリプティング攻撃を試みる可能性がありますを自動的に防ぎます。 MVC`DisplayFor`タスクを表示するために使用するヘルパー タイトルされ、自動的に HTML エンコード値、ブラウザーに送信します。 運用アプリケーションで別の対策をする可能性があります。 詳細については、次を参照してください。 [ASP.NET での要求の検証](https://msdn.microsoft.com/library/hh882339.aspx)です。

<a id="bestpractices"></a>
## <a name="best-practices"></a>ベスト プラクティス

コード レビューで発見し、修正にアプリの元のバージョンのテストの後に修正された問題のいくつかを次に示します。 いない認識する特定のベスト プラクティスの一部だけで元コードの作成者によっていくつかが発生したため、コードが簡単に作成されており、リリースされたソフトウェアを想定していません。 ここでの問題お一覧表示するしているは、このレビューからわかったためのものを使用する必要がある場合にしをテストすることもを他のユーザーの web アプリを開発しています。

### <a name="dispose-the-database-repository"></a>データベースのリポジトリを破棄します。

`FixItTaskRepository`クラスは、Entity Framework を破棄しなければならない`DbContext`インスタンス。 実装することで、これを行った`IDisposable`で、`FixItTaskRepository`クラス。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

AutoFac が自動的に破棄することに注意してください、`FixItTaskRepository`インスタンス、ためこれを明示的に破棄する必要はありません。

削除することもできます、`DbContext`からメンバー変数`FixItTaskRepository`、ローカルを代わりに作成および`DbContext`各リポジトリ メソッド内で変数内、`using`ステートメントです。 例えば:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>DI でようなシングルトンを登録します。

以降のインスタンスを 1 つだけ、`PhotoService`クラスと`Logger`クラスが必要なこれらのクラスを指定する必要があります[依存関係の挿入の単一インスタンスとして登録されている](https://code.google.com/p/autofac/wiki/InstanceScope)で*DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>セキュリティ: は、ユーザーにエラーの詳細を表示しません。

オリジナルの修正、アプリがありませんでした汎用エラー ページがあると、UI でまでのすべての例外バブルのため、データベース接続エラーなどのいくつかの例外は、ブラウザーに表示されている完全なスタック トレースを可能性があります。 エラーの詳細については、悪意のあるユーザーの攻撃を促進できる場合があります。 ソリューションでは、例外の詳細を記録し、エラーの詳細が含まれていないことをユーザーにエラー ページを表示します。 修正、アプリが既にログ記録、およびエラー ページを表示するために追加されました`<customErrors mode=On>`Web.config ファイルにします。

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

これにより、既定では*Views\Shared\Error.cshtml*エラーを表示します。 カスタマイズできる*Error.cshtml*または独自のエラー ページのビューを作成し、追加、`defaultRedirect`属性。 特定のエラーの別のエラー ページを指定することもできます。

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>セキュリティ: のみ、作成者によって編集するタスクを許可します。

悪意のあるユーザーは別のユーザーのタスクに ID を使用して URL を作成する可能性がありますが、ダッシュ ボードのインデックス ページでは、ユーザーのログオンによって作成されたタスクのみ表示されます。 内のコードを追加しました*DashboardController.cs*をその場合は 404 を返します。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>例外を飲み込むしません。

オリジナルの修正アプリだけが null を返しました SQL クエリから生成される例外をログに記録後。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

ユーザーを検索してクエリが成功しましたが、同じ行が返されませんでしたかのようになります。 ソリューションでは、キャッチとログ記録の後に例外を再スローします。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>ワーカー ロールのすべての例外をキャッチします。

ワーカー ロールでハンドルされない例外、VM がリサイクルされると、try-catch ブロックを実行し、すべての例外を処理するすべてのものをラップするようにします。

### <a name="specify-length-for-string-properties-in-entity-classes"></a>エンティティ クラスのプロパティの文字列の長を指定します。

単純なコードを表示するために、修正、アプリの元のバージョンが FixItTask エンティティのフィールドの長さを指定していないし、その結果、データベース内には、varchar (max) として定義されました。 結果として、UI が、ほぼすべての量の入力を受け入れてください。 両方のユーザーに適用される長さセット制限を指定するは、web ページと、データベース内の列のサイズに入力します。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>変更することが求められていない場合に、プライベート メンバーを読み取り専用としてマークします。

たとえば、`DashboardController`クラスのインスタンス`FixItTaskRepository`が作成され、変更するには想定されていませんとしては、定義したので[readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx)です。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>リストを使用します。一覧ではなく Any() です。Count() &gt; 0

リスト内の 1 つまたは複数の項目が指定した条件に一致するかどうかについて注意する場合を使用して、[任意](https://msdn.microsoft.com/library/bb534972.aspx)メソッドを返すので条件に適合する項目が見つかるとすぐに対し、`Count`メソッドは、常に反復処理するのにはすべての項目。 ダッシュ ボード*Index.cshtml*ファイルでこのコードは最初から。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

これを変更しました。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>MVC ヘルパーを使用して MVC ビューの Url を生成します。

**作成、修正**修正アプリ ハード ホーム ページで、ボタンは、アンカー要素をコード化されました。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

次のように表示/アクションへのリンクは、使用する方がよい、 [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx)例については、HTML ヘルパー。

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>ワーカー ロールで Thread.Sleep ではなく Task.Delay を使用します。

新しいプロジェクト テンプレートの格納`Thread.Sleep`サンプルでは、ワーカー ロールが原因でスレッドをスリープさせることができる、スレッド プールに追加の不要なスレッドを生成します。 使用することを回避する[Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx)代わりにします。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Async void を回避します。

非同期のメソッドが値を返す必要がある場合を返す、`Task`型なく`void`です。

この例は、`FixItQueueManager`クラス。 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

使用する必要があります`async void`トップレベルのイベント ハンドラーに対してのみです。 としてメソッドを定義する場合`async void`、呼び出し元のできません**await**メソッドまたはメソッドをスローするすべての例外をキャッチします。 詳細については、次を参照してください。[非同期プログラミングのベスト プラクティス](https://msdn.microsoft.com/magazine/jj991977.aspx)です。 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>キャンセル トークンを使用して、ワーカー ロールのループを中断するには

通常、**実行**ワーカー ロール上のメソッドには、無限ループが含まれています。 ワーカー ロールを停止すると、 [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx)メソッドが呼び出されます。 このメソッドを使用して内に行われている作業をキャンセルする必要があります、**実行**メソッドと終了適切にします。 それ以外の場合、プロセスは、操作の途中で終了可能性があります。

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>自動 MIME スニッフィング プロシージャからオプトアウトします。

場合によっては、Internet Explorer は web サーバーで指定された型とは異なる MIME の種類を報告します。 たとえば、Internet Explorer が検出されると HTML コンテンツのコンテンツ タイプの HTTP 応答ヘッダーで提供されるファイルで: テキスト/プレーン、Internet Explorer の決定、コンテンツを HTML として表示するか。 残念ながら、この「MIME スニッフィング」も可能性が信頼されていないコンテンツをホストするサーバーのセキュリティの問題にあります。 この問題を解決するには、Internet Explorer 8 MIME の種類の決定コードへの変更の数が確立してにより、アプリケーション開発者[MIME スニッフィングからオプトアウト](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)です。 次のコードに追加された、 *Web.config*ファイル。

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>バンドルと縮小が有効にします。

Visual Studio では、新しい web プロジェクトを作成するときの JavaScript ファイルのバンドルと縮小は既定で有効ありません。 BundleConfig.cs でコードの行が追加されました。

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>認証 cookie の有効期限タイムアウトを設定します。

既定では、認証 cookie は、2 週間以内に切れます。 時間が短い方が安全です。 この設定を変更することができます*StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>ローカル コンピューターで Visual Studio からアプリを実行する方法

これには修正アプリを実行する 2 つの方法があります。

- SQL データベースに直接書き込み、新しいタスク ベースのアプリケーションを実行します。
- キューに加えて、バックエンド サービスを使用してタスクを作成するアプリケーションを実行します。 キュー パターンが、章で説明されている[キュー中心作業パターン](queue-centric-work-pattern.md)です。

<a id="runbase"></a>
### <a name="run-the-base-application"></a>ベースのアプリケーションを実行します。

1. インストール[Visual Studio 2013 または Visual Studio 2013 Express for Web](https://www.visualstudio.com/downloads)です。
2. インストール、 [Azure SDK for Visual Studio 2013 用の .NET です。](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. .Zip ファイルをダウンロード、 [MSDN コード ギャラリー](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)です。
4. ファイル エクスプ ローラーで、.zip ファイルを右クリックしてプロパティ をクリックし、プロパティ ウィンドウで ブロック解除 をクリックします。
5. ファイルを解凍します。
6. Visual Studio を起動するには、ある .sln ファイルをダブルクリックします。
7. ツール] メニューの [ライブラリ パッケージ マネージャー、パッケージ マネージャー コンソールをクリックします。
8. パッケージ マネージャー コンソール (PMC) では、復元をクリックします。
9. Visual Studio を終了します。
10. 開始、 [Azure ストレージ エミュレーター](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)です。
11. 前の手順で閉じられたか、ソリューション ファイルを開いて、Visual Studio を再起動します。
12. FixIt プロジェクトがスタートアップ プロジェクトとして設定を確認し、プロジェクトを実行するには、CTRL + F5 キーを押します。

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>キューの処理で、アプリケーションを実行します。

1. 指示に従い[ベースのアプリケーションを実行](#runbase)ブラウザーを終了して、Visual Studio を閉じます。
2. 管理者特権で Visual Studio を起動します。 (Azure コンピューティング エミュレーターが使用される、管理者特権を必要とする)。
3. アプリケーションで*Web.config*ファイルを*MyFixIt* (web プロジェクト) をプロジェクトでの値を変更`appSettings/UseQueues`"true"にします。 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. 場合、 [Azure ストレージ エミュレーター](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx)がまだ実行中、再び起動します。
5. FixIt web プロジェクトと MyFixItCloudService プロジェクトを同時に実行します。

    Visual Studio 2013 を使用します。

   1. F5 キーを押して FixIt プロジェクトを実行します。
   2. **ソリューション エクスプ ローラー**MyFixItCloudService プロジェクトを右クリックし、クリックして**デバッグ** -- **新しいインスタンスを開始**です。

      Visual Studio 2013 Express for Web の使用。

   3. ソリューション エクスプ ローラーで FixIt ソリューションを右クリックし、**プロパティ**です。
   4. 選択**マルチ スタートアップ プロジェクト**.
   5. **アクション**MyFixIt と MyFixItCloudService、下にあるドロップダウン リストを選択**開始**です。
   6. **[OK]** をクリックします。
   7. 両方のプロジェクトを実行する場合は F5 キーを押します。

      MyFixItCloudService プロジェクトを実行するときに、Visual Studio は、Azure コンピューティング エミュレーターを起動します。 ファイアウォールの構成によっては、エミュレーターがファイアウォールを通過を許可する必要があります。

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Azure App Service Web Apps を Windows PowerShell スクリプトを使用して基本アプリを展開する方法

説明するために、[自動化すべて](automate-everything.md)パターンでは、修正、アプリの azure 環境を設定して、プロジェクトを新しい環境に配置するスクリプトが付属します。 次の手順では、スクリプトを使用する方法について説明します。

キューを使用せずに Azure で実行する、キューでローカルで実行する変更を加えた場合は、元に戻す、次の手順に進む前に UseQueues appSetting 値を設定することを確認します。

これらの手順は、既にダウンロードしていると、修正、ソリューションをローカルで実行し、Azure があるかアカウントまたは Azure サブスクリプションを持っている想定しています。 管理を承認されています。

1. インストール、 **Azure PowerShell**コンソールです。 手順については、次を参照してください。[をインストールして、Azure PowerShell を構成する方法](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)です。

    このカスタマイズされたコンソールは、Azure サブスクリプションを使用するように構成します。 Azure モジュールがインストールされている、 *Program Files* directory、Azure PowerShell コンソールを使用するたびに自動的にインポートしているとします。

    Windows PowerShell ISE などの別のホスト プログラムで作業できるようにする場合を必ず使用して、 [Import-module](https://go.microsoft.com/fwlink/?LinkID=141553)コマンドレットを Azure モジュールをインポートまたは Azure モジュールのコマンドを使用して、モジュールの自動インポートを開始します。
2. Azure PowerShell を起動、**管理者として実行**オプション。
3. 実行、 [Set-executionpolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) Azure PowerShell 実行ポリシーを設定するコマンドレット`RemoteSigned`です。 入力**Y** (Yes) のポリシーの変更を完了します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    この設定では、デジタル署名されていないローカル スクリプトを実行することができます。 (を実行ポリシーを設定することもできます`Unrestricted`後で、ブロックを解除する手順の必要はありませんが、これは、セキュリティ上の理由から推奨されません。)。
4. 実行、`Add-AzureAccount`コマンドレットを PowerShell を設定するアカウントの資格情報を使用します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    これらの資格情報は、一定の時間後に期限切れし、再実行する必要がある、`Add-AzureAccount`コマンドレット。 この電子書籍が書き込まれると資格情報が期限切れ前に時間制限は 12 時間以内にです。
5. 複数のサブスクリプションがある場合は、Select-azuresubscription コマンドレットを使用して、テスト環境を作成するサブスクリプションを指定します。
6. 使用して、同じ Azure サブスクリプションの管理証明書をインポート、`Get-AzurePublishSettingsFile`と`Import-AzurePublishSettingsFile`コマンドレット。 これらのコマンドレットの最初の証明書ファイルをダウンロードして、2 番目の 1 つで、インポートするためにそのファイルの場所を指定します。 > [!IMPORTANT]
   > 安全な場所にダウンロードしたファイルを保持または削除、操作を終了すると、Azure サービスを管理するために使用する証明書が含まれています。

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    証明書が使用される SQL データベース サーバー上のファイアウォール ルールを設定するのには、開発用コンピューターの IP アドレスを検出する REST API の呼び出しです。
7. 実行、 [Set-location](https://go.microsoft.com/fwlink/p/?linkid=293912)コマンドレット (エイリアスは`cd`、`chdir`と`sl`)、スクリプトが含まれているディレクトリに移動します。 (ファイルに配置している、*オートメーション*修正ソリューション フォルダー内のフォルダーです)。ディレクトリ名にスペースが含まれている場合は、引用符で囲まれたパスを配置します。 たとえばに移動するため、`c:\Sample Apps\FixIt\Automation`ディレクトリは、次のコマンドを入力する可能性があります。

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. これらのスクリプトを実行する Windows PowerShell を許可するのには、使用、 [Unblock-file](https://go.microsoft.com/fwlink/p/?linkid=294021)コマンドレット。 (スクリプトがブロックされている、インターネットからダウンロードされたことがあるためです。)

    > [!WARNING]
    > セキュリティ - 実行する前に`Unblock-File`任意のスクリプトまたは実行可能ファイルを使用して、ファイルをメモ帳で開きます、コマンドを確認し、悪意のあるコードが含まれていないことを確認してください。

    たとえば、次のコマンドを実行、`Unblock-File`コマンドレットは、現在のディレクトリ内のすべてのスクリプトにします。

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. (キューの処理) ベースの web アプリを作成するには、アプリを修正、環境の作成スクリプトを実行します。

    必要な`Name`パラメーターが、データベースの名前を指定しはスクリプトを作成する記憶域アカウントにも使用します。 名前は、azurewebsites.net ドメイン内でグローバルに一意にする必要があります。 Fixit またはテストと同様に (または、fixitdemo 例のようにも)、重複する名前を指定する場合、`New-AzureWebsite`コマンドレットは、競合を報告する内部エラーで失敗します。 スクリプトは、web アプリ、ストレージ アカウント、およびデータベースの名前の要件に準拠するすべて小文字に名前を変換します。

    必要な`SqlDatabasePassword`パラメーターは、SQL データベースに作成される管理者アカウントのパスワードを指定します。 パスワードに特殊な XML 文字を含めないでください (&amp; &lt; &gt; ;)。 これは、スクリプト作成された、Azure の制限ではない方法を制限します。

    たとえば、"fixitdemo"をという名前の web アプリを作成して"Passw0rd1"の SQL Server 管理者のパスワードを使用する場合は、次のコマンドを入力します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Azurewebsites.net ドメインで名前が一意である必要があり、パスワードがパスワードの複雑さの SQL データベースの要件を満たす必要があります。 (例 Passw0rd1 が要件を満たしています。)

    コマンドの最初に注意してください"です。\". スクリプトの悪意のある実行を防ぐため、Windows PowerShell では、スクリプトを実行するときに、スクリプト ファイルへの完全修飾パスを指定することが必要です。 現在のディレクトリを示すために、ドットを使用することができます (".\")または、ように、完全修飾パスを提供します。

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    スクリプトの詳細については、使用、`Get-Help`コマンドレット。

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    使用することができます、 `Detailed`、 `Full`、 `Parameters`、および`Examples`返されるヘルプのフィルター処理、Get-help コマンドレットのパラメーターです。

    スクリプトが失敗するか、「New-azurewebsite:: 呼び出す Set-azuresubscription と Select-azuresubscription 最初に、」などのエラーが生成される場合は、Azure PowerShell の構成を完了していない可能性があります。

    示すように作成されたリソースを表示する、Azure 管理ポータルを使用するには、スクリプトの終了後、[自動化すべて](automate-everything.md)章します。
10. 新しい Azure 環境に FixIt プロジェクトを配置するには使用、 *AzureWebsite.ps1*スクリプト。 例えば:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    展開が完了したら、修正、Azure で実行すると、ブラウザーが表示されます。

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Windows PowerShell スクリプトのトラブルシューティング

これらのスクリプトを実行しているときに発生する最も一般的なエラーは、アクセス許可に関連付けられます。 確認して`Add-AzureAccount`と`Import-AzurePublishSettingsFile`が成功したこと、および同じ Azure サブスクリプションに使用します。 場合でも`Add-AzureAccount`が正常に実行する必要があります、再度実行します。 によって追加されたアクセス許可`Add-AzureAccount`12 時間以内に期限切れにします。

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>オブジェクト参照がオブジェクト インスタンスに設定されていません。

「オブジェクト参照がオブジェクトのインスタンスに設定されていない」つまり Windows PowerShell がプロセス (これは、null 参照の例外です) にオブジェクトを検出できないことを実行など、スクリプトがエラーを返した場合、`Add-AzureAccount`コマンドレットとスクリプトをもう一度やり直してください。

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: サーバーには、内部エラーが発生しました。

`New-AzureWebsite`コマンドレットでは、名前は azurewebsites.net ドメイン内で一意ではないときに内部エラーが返されます。 エラーを解決するには、Name パラメーターでは、名前の別の値を使用して*新規 AzureWebsiteEnv.ps1*です。

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>スクリプトを再起動します。

再起動する必要がある場合、*新規 AzureWebsiteEnv.ps1*スクリプト「スクリプトが完了しました」のメッセージを印刷する前に失敗したためをする前に作成したスクリプトが停止しているリソースを削除する場合があります。 たとえば、スクリプトが作成されたは既に ContosoFixItDemo web アプリと同じ名前のスクリプトをもう一度実行する場合場合、名前が使用されているためにのスクリプトでは失敗します。

リソースを決定するには、は、停止、前に作成したスクリプトは、次のコマンドレットを使用します。

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: このコマンドレットを実行するにはパイプを使用するデータベース サーバーの名前`Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

これらのリソースを削除するには、次のコマンドを使用します。 場合、データベース サーバーを削除すると、自動的に削除するサーバーに関連付けられているデータベースに注意してください。

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Azure App Service Web Apps を Azure クラウド サービスの処理キューにアプリを展開する方法

キューを有効にするには、MyFixIt\Web.config ファイルで、次の変更を行います。 `appSettings`の値を変更`UseQueues`"true"にします。 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

その後、MVC アプリケーション展開 Azure App service web アプリへの説明に従って[以前](#deploybase)です。

次に、新しい Azure クラウド サービスを作成します。 修正がアプリに含まれているスクリプトを作成またはしないでこの Azure ポータルを使用する必要がありますので、クラウド サービスを展開します。 ポータルで、をクリックして**新規** -- **コンピューティング**–**クラウド サービス** -- **簡易作成**、URL を入力およびデータ センターの場所。 Web アプリを展開して、同じデータ センターを使用します。

![](the-fix-it-sample-application/_static/image1.png)

クラウド サービスを展開する前に、構成ファイルの一部を更新する必要があります。

MyFixIt.WorkerRoler\app.config の下にある`connectionStrings`の値を置き換える、 `appdb` SQL データベースの実際の接続文字列で接続文字列。 接続文字列は、ポータルから取得できます。 ポータルで、をクリックして**SQL データベース** - **appdb** - **Ado.net、ODBC、PHP、および JDBC のビューの SQL データベース接続文字列**です。 ADO.NET 接続文字列をコピーし、app.config ファイルに値を貼り付けます。 置換"{、\_パスワード\_ここ}"、データベース パスワードを使用します。 (内のデータベース パスワードを指定した MVC アプリの展開にスクリプトを使用したと仮定した場合、`SqlDatabasePassword`パラメーターのスクリプトを作成します)。

結果は、次のようになります。

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

同じ MyFixIt.WorkerRoler\app.config ファイルで下にある`appSettings`、Azure ストレージ アカウントの 2 つのプレース ホルダーの値を置き換えます。

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

アクセス キーは、ポータルから取得できます。 参照してください[ストレージ アカウントを管理する方法](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)です。

MyFixItCloudService\ServiceConfiguration.Cloud.cscfg では、Azure ストレージ アカウントの同じ 2 つのプレース ホルダーの値を置き換えます。

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

クラウド サービスをデプロイする準備が整いました。 ソリューション エクスプ ローラーで MyFixItCloudService プロジェクトを右クリックし **発行**です。 詳細については、次を参照してください。"[を Azure にアプリケーションを配置](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)"、2 部である[このチュートリアル](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36)です。

> [!div class="step-by-step"]
> [前へ](more-patterns-and-guidance.md)
