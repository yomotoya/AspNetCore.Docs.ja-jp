---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 接続の回復と Entity Framework、ASP.NET MVC アプリケーションでのコマンド インターセプト |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4a43a9120bf3fa69b00b234d65d0f59d3ce9975b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875345"
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>接続の回復と Entity Framework、ASP.NET MVC アプリケーションでのコマンドの途中受信
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


これまで、アプリケーションが実行されているローカル IIS Express で開発用コンピューター上。 を実際のアプリケーションを他の人にインターネット経由で使用できるようにするには、web ホスティング プロバイダー、を展開して、データベース サーバーにデータベースを配置する必要があります。

このチュートリアルでは、クラウド環境に配置するときに特に有効である Entity Framework 6 の 2 つの機能を使用する方法を学習します接続の回復 (一時的なエラーの自動再試行) とコマンドの途中受信 (すべての SQL クエリ。データベースに送信するログまたはそれらを変更するために)。

この接続の回復性とコマンドの途中受信チュートリアルではオプションです。 このチュートリアルをスキップする場合は、いくつかの細かな調整を以降のチュートリアルで行う必要があります。

## <a name="enable-connection-resiliency"></a>接続の回復を有効にします。

Windows Azure にアプリケーションを展開するときに Windows Azure SQL Database、データベースのクラウド サービスにデータベースを配置します。 一時的な接続エラーは、ときに、web サーバーとデータベース サーバーは直接接続されている一緒に、同じデータ センターでよりクラウド データベース サービスに接続するときに通常より頻繁に発生します。 クラウドの web サーバーおよびデータベースのクラウド サービスが同じデータ センター内にホストされている場合でもはロード バランサーなどの問題があることができますをそれらの間のネットワーク接続の数。

また、クラウド サービスは通常共有他のユーザーによってそれらにより、応答性が低下する可能性がありされます。 データベースへのアクセスが制限されるおそれがあります。 調整の手段データベース サービスが例外をスロー サービス レベル アグリーメント (SLA) では許可されているよりも頻繁にアクセスが詳細しようとするとします。

クラウド サービスにアクセスするときの多くまたはほとんど接続の問題は一時的なもの、クライアントは、短時間で解決自体です。 データベース操作を再試行は通常、一時的エラーの種類を取得すると、短い待機、および、操作が成功する可能性があります後、操作を再試行可能性があります。 自動的に読み込もうとすると、一時的なエラーを処理する場合、ユーザーのエクスペリエンスが大幅に向上を提供できますを顧客にそれらのほとんどを非表示にします。 Entity Framework 6 の接続の復元機能は、SQL クエリが再試行中の処理に失敗したことを自動化します。

特定のデータベースのサービスの接続の復元機能を適切に構成する必要があります。

- 例外は一時的なものである可能性を知っておく必要があります。 ネットワーク接続の一時的に切断によって発生したエラーなどのプログラムのバグによって発生したエラーされませんを再試行するか。
- 失敗した操作の再試行の間の時間の適切な量を待機する必要があります。 待機できますになったバッチ プロセス用の再試行の間隔よりも、ユーザーが応答を待つオンラインの web ページのことができます。
- 適切な回数まで再試行する必要があるとします。 オンライン アプリケーションの場合のバッチ プロセスで複数回を再試行するか可能性があります。

これらの設定、Entity Framework プロバイダーでサポートされている任意のデータベース環境を手動で構成できますが、通常 Windows Azure SQL データベースを使用するオンライン アプリケーションの適切に動作する既定値は、既に構成されているとこれらは、Contoso 大学アプリケーションを実装する設定です。

接続の回復を有効にするために必要なから派生したアセンブリでクラスを作成するが、 [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx)クラス、およびそのクラスには、SQL データベースを設定*実行方法*EF には別の用語*再試行ポリシー*です。

1. DAL フォルダーでという名前のクラス ファイルを追加*SchoolConfiguration.cs*です。
2. テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework がから派生したクラス内で検出されたコードを自動的に実行`DbConfiguration`です。 使用することができます、`DbConfiguration`で行う場合はコードでの構成タスクを実行するクラス、 *Web.config*ファイル。 詳細については、次を参照してください。 [EntityFramework コード ベースの構成](https://msdn.microsoft.com/data/jj680699)です。
3. *StudentController.cs*、追加、`using`の声明`System.Data.Entity.Infrastructure`です。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. すべての変更、`catch`その catch ブロック`DataException`例外見つけられるように`RetryLimitExceededException`例外代わりにします。 例えば:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    使用していた`DataException`にわかりやすい「再試行」メッセージを指定するために一時的なものになる可能性があるエラーを特定しようとしています。 再試行ポリシーを有効にした、これで、一時的なものである可能性のみエラーは既にがされてしようとしたや複数回失敗しました実際に返される例外でラップされます、`RetryLimitExceededException`例外。

詳細については、次を参照してください。 [Entity Framework 接続の回復/再試行ロジック](https://msdn.microsoft.com/data/dn456835)です。

## <a name="enable-command-interception"></a>コマンドの途中受信を有効にします。

これで、再試行ポリシーを有効にした、方法をテストすること期待どおりに機能することを確認するか。 一時的なエラーが発生、特に、ローカルで実行しているし、自動化された単体テストに実際の一時的なエラーを統合することは特に困難を強制するために簡単ではありません。 接続の復元機能をテストするには、Entity Framework は SQL Server に送信されるクエリをインターセプトし、通常、一時的である例外の種類で SQL サーバーの応答を 手段が必要です。

クラウド アプリケーションのためのベスト プラクティスを実装するために、クエリの途中受信を使用することもできます: [、待機時間との成功または失敗の外部サービスへのすべての呼び出しをログ](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)データベース サービスなどです。 EF6 提供、[専用のログ記録 API](https://msdn.microsoft.com/data/dn469464)を容易に、ログ記録を実行できることが、チュートリアルのこのセクションで、Entity Framework の使用方法が学習[インターセプション機能](https://msdn.microsoft.com/data/dn469464)の両方を直接ログ記録と一時的なエラーをシミュレートするためです。

### <a name="create-a-logging-interface-and-class"></a>ログ記録のインターフェイスとクラスを作成します。

A[ベスト プラクティスのログ記録](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)インターフェイスを使用して行うにはハード コーディング System.Diagnostics.Trace または呼び出しをログ記録クラスではなくです。 これによりを実行する必要が生じた場合は、後で、ログ記録メカニズムを変更します。 このセクションでは、ログ記録のインターフェイスと、それ/p を実装するクラスを作成しますように > 

1. プロジェクトにフォルダーを作成し、名前を付けます*ログ*です。
2. *ログ*フォルダー、という名前のクラス ファイルを作成する*ILogger.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    インターフェイスは、ログの相対的な重要度を示すために次の 3 つのトレース レベルおよびデータベース クエリなどの外部サービス呼び出しの待機時間情報を提供する 1 つを提供します。 ログ作成メソッドで例外を渡すことのできるオーバー ロードがあります。 これは、スタック トレースと内部例外を含む例外情報は、アプリケーション全体で各ログ記録のメソッドの呼び出しで実行されていることに依存せずに、インターフェイスを実装するクラスによって確実に記録されるようにします。

    TraceApi メソッドを使用すると、SQL データベースなどの外部サービスへの各呼び出しの待機時間を追跡できます。
3. *ログ*フォルダー、という名前のクラス ファイルを作成する*Logger.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    実装では、System.Diagnostics を使用して、トレースを行います。 これは、簡単に生成し、トレース情報を使用する .NET の組み込み機能です。 たとえば、ファイルにログを書き込む、またはそれを azure blob ストレージに書き込むに System.Diagnostics トレースで使用できる"多"リスナーがあります。 いくつかのオプション、およびその他の詳細については、リソースへのリンクで[Visual Studio での Azure Web サイトのトラブルシューティング](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)です。 Visual Studio でこのチュートリアルに紹介のみをログに記録の**出力**ウィンドウです。

    ILogger インターフェイスを使用する場合はこれを行うには、さまざまなトレース機構に切り替える比較的簡単や実稼働アプリケーションで System.Diagnostics、以外のトレースのパッケージを考慮をする可能性があります。

### <a name="create-interceptor-classes"></a>インターセプターのクラスを作成します。

次に、データベース、一時的なエラーをシミュレートして 1 つをログ記録を行うには、クエリを送信するたびに、Entity Framework が呼び出すクラスを作成します。 これらのインターセプターのクラスがから派生する必要があります、`DbCommandInterceptor`クラスです。 クエリが実行されると自動的に呼び出されるメソッドのオーバーライドを作成します。 これらのメソッドで確認またはログがデータベースに送信されるクエリ、およびデータベースに送信される前に、クエリを変更したり、何かを返す Entity Framework を自分であってもデータベースにクエリを渡さずにします。

1. データベースに送信されるすべての SQL クエリがログに記録するインターセプター クラスを作成するには、という名前のクラス ファイルを作成*SchoolInterceptorLogging.cs*で、 *DAL*フォルダー、およびテンプレート コードでは、置換次のコードでは:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    成功したクエリまたはコマンドの場合は、このコードは、待機時間情報をログ記録する情報を書き込みます。 例外のエラー ログが作成されます。
2. クラスを作成する、インターセプター"の Throw"を入力するときに、ダミーの一時的なエラーを生成するのには**検索**ボックスで、という名前のクラス ファイルを作成する*SchoolInterceptorTransientErrors.cs* で*DAL*フォルダー、および置換されたテンプレート コードを次のコード。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    オーバーライドのみです。 このコード、`ReaderExecuting`複数行のデータを返すことができるクエリに対して呼び出されるメソッド。 接続の回復の他の種類のクエリを確認する場合は、上書きすることでしたも、`NonQueryExecuting`と`ScalarExecuting`はログ記録インターセプターとしてメソッド、します。

    学生ページを実行して、検索文字列として"Throw"を入力して、ときに、このコードはエラー番号 20、通常、一時的である既知の型のダミー SQL データベースの例外を作成します。 現在一時的なものとして認識されているその他のエラー番号は、64、233、10053、10054、10060、10928、10929、40197、40501、および 40613 が、新しいバージョンの SQL データベースで変更される可能性があります。

    コードは、クエリを実行して戻るクエリの結果の代わりに Entity Framework には、例外を返します。 一時的な例外が 4 回返され、後、データベースにクエリを渡すことの通常のプロシージャにコードが戻されます。

    すべてのものが記録されるため、Entity Framework に成功すると、前に 4 回のクエリを実行しようとして、アプリケーションの唯一の違いはクエリ結果のページを表示するために長くかかることを確認することができます。

    Entity Framework は再試行回数が構成可能です。コードは、SQL データベースの実行ポリシーの既定値であるために、4 回を指定します。 実行ポリシーを変更すると、しなければなりませんでしたも変更は、ここに、コードの一時的なエラーが生成される回数を指定します。 Entity Framework をスローするように、例外を生成するコードを変更することも、`RetryLimitExceededException`例外。

    検索ボックスに入力した値になります`command.Parameters[0]`と`command.Parameters[1]`(1 つが、姓と、姓と名のいずれかの使用)。 値「Throw %」が検出されると"Throw"で置き換えられますこれらのパラメーター「、」一部の受講者が検出され、返されるようにします。

    これは、アプリケーションの UI に何らかの入力の変化に応じて接続の回復性をテストする便利な手段だけです。 に関するコメントに後で説明したとおりにすべてのクエリや更新、一時的なエラーを生成するコードを記述することも、 *DbInterception.Add*メソッドです。
3. *Global.asax*、次の追加`using`ステートメント。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. 強調表示された行を追加、`Application_Start`メソッド。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    次のコード行は、Entity Framework は、データベースにクエリを送信するときに実行するコードをインターセプターの原因です。 一時的なエラーのシミュレーションの個別のインターセプターのクラスを作成して、ログ記録を個別に有効または無効にするためすることを確認します。

    使用してインターセプターを追加することができます、`DbInterception.Add`メソッド、コードで任意の場所である必要はありません、`Application_Start`メソッドです。 実行ポリシーを構成する前に作成した DbConfiguration クラスにこのコードを配置することもできます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    場所に配置するこのコードを実行しないように注意をする`DbInterception.Add`同じインターセプターは 1 回または複数追加インターセプター インスタンスが表示されます。 たとえば、2 回ログ記録のインターセプターを追加すると、すべての SQL クエリの 2 つのログが表示されます。

    インターセプターは、登録の順序で実行されます (順序、`DbInterception.Add`メソッドが呼び出されます)。 順序は重要でによっては、インターセプターで何をしている可能性があります。 たとえば、インターセプターを変更、SQL コマンドでは、取得した、`CommandText`プロパティです。 SQL コマンドを変更するは、[次へ] のインターセプターが、変更された SQL コマンドを元の SQL コマンドではなくになります。

    UI で別の値を入力することで一時的なエラーが発生するように一時的なエラー シミュレーション コードを記述しました。 代わりに、常に特定のパラメーター値をチェックすることがなく一時的な例外のシーケンスを生成するインターセプター コードを記述することもできます。 一時的なエラーを生成する場合にのみにし、インターセプターを追加できます。 これを行う場合、データベースの初期化が完了した後までインターセプター追加しないただし、します。 つまり、一時的なエラーの生成を開始する前に、エンティティ セットのいずれかのクエリなど、少なくとも 1 つのデータベース操作を実行します。 Entity Framework データベースの初期化中にいくつかのクエリを実行して、これらはトランザクションでは、ため、初期化中にエラーが発生する不整合な状態を取得するコンテキスト。

## <a name="test-logging-and-connection-resiliency"></a>テストのログ記録と接続の回復性

1. デバッグ モードでアプリケーションを実行し、をクリックし、f5 キーを押して、**受講者**タブです。
2. Visual Studio を見て**出力**トレース出力を表示するウィンドウです。 過去のロガーによって書き込まれたログに取得するには、いくつか JavaScript エラー上にスクロールする必要があります。

    データベースに送信される実際の SQL クエリを表示できることに注意してください。 最初のクエリとデータベースのバージョンを確認しています。 Entity Framework では、作業を開始するコマンドと (について学習します。 移行次のチュートリアルで) 移行履歴テーブルを参照してください。 どのように多くの学生を調べるには、ページングにクエリを参照してくださいし、生徒のデータを取得するクエリを参照してください最後にします。

    ![通常のクエリのログ記録](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. **受講者** ページで、検索文字列として"Throw"を入力してをクリックして**検索**です。

    ![検索文字列をスローします。](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    ブラウザーは Entity Framework でも、クエリを何回か再試行は、数秒のハングことがわかります。 最初の再試行は発生非常に短時間、各追加の再試行まで増加する前に待機します。 このプロセスの再試行の間隔が呼び出される前に長時間待つ*指数バックオフ*です。

    ページを表示するとき、表示された生、名前に「、」を見て、出力ウィンドウと同じクエリが 5 回、最初に 4 回に返す一時的なものを試みましたがわかります例外。 各一時的なエラーで一時的なエラーを生成するときに作成したログが表示されます、`SchoolInterceptorTransientErrors`クラス (「Returning 一時的なエラーをコマンド...」) とするときに書き込まれたログが表示されます`SchoolInterceptorLogging`例外を取得します。

    ![再試行回数を示すログ出力](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    検索文字列を入力したため学生データを返すクエリがパラメーター化します。

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    パラメーターの値を記録しているしないを実行する可能性があります。 パラメーター値を表示する場合からパラメーター値を取得するログ コードを記述することができます、`Parameters`のプロパティ、`DbCommand`インターセプター メソッドで取得するオブジェクト。

    アプリケーションを停止して再起動した場合を除き、このテストを繰り返すことはできませんに注意してください。 エラー カウンターをリセットするためのコードを記述することもできるように、1 つのアプリケーションに複数回接続の回復性をテストする場合は、`SchoolInterceptorTransientErrors`です。
4. 違いを確認する (再試行ポリシー) の実行方法は、コメント、`SetExecutionStrategy`線*SchoolConfiguration.cs*、デバッグ モードで受講者 ページを再度実行し、もう一度"Throw"を検索します。

    今度は、デバッガーを停止最初の生成された例外を初めてクエリを実行しようとするとすぐにします。

    ![ダミー例外](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. コメントを解除、 *SetExecutionStrategy*線*SchoolConfiguration.cs*です。

## <a name="summary"></a>まとめ

このチュートリアルでは、接続の回復を有効にして、Entity Framework は、作成し、データベースに送信する SQL コマンドをログする方法を説明しました。 次のチュートリアルで Code First Migrations を使用して、データベースを展開する、インターネットにアプリケーションを展開します。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
