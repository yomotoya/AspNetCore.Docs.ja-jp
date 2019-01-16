---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリで ef 接続の回復性とコマンド傍受を使用して、'
author: tdykstra
description: このチュートリアルでは、接続の回復性とコマンド傍受を使用する方法を学習します。 これは、Entity Framework 6 の 2 つの重要な機能です。
ms.author: riande
ms.date: 01/14/2018
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fae5c7e1ad1000ed90630c3620b853de3a735d60
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341733"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>チュートリアル: 接続の回復性とコマンド傍受を Entity Framework で ASP.NET MVC アプリで使用します。

これまでに、アプリケーションを実行したローカル IIS Express で開発用コンピューターにします。 で実際のアプリケーションをインターネット経由で使用するには、他のユーザーに使用できるように、web ホスティング プロバイダーにデプロイする必要があるし、データベース サーバーにデータベースを展開します。

このチュートリアルでは、接続の回復性とコマンド傍受を使用する方法を学習します。 Entity Framework 6 のクラウド環境にデプロイするときに特に役に立つできる 2 つの重要な機能は: (一時的なエラーの自動再試行) の接続復元性とコマンド傍受 (catch データベースに送信するすべての SQL クエリまたはするためにログ変更)。

この接続の回復性とコマンド傍受チュートリアルでは、省略可能です。 このチュートリアルをスキップする場合は、若干の調整が以降のチュートリアルで作成する必要があります。

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 接続の回復を有効にします。
> * コマンド傍受を有効にします。
> * 新しい構成をテストします。

## <a name="prerequisites"></a>必須コンポーネント

* [並べ替え、フィルター処理、ページング](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>接続の回復を有効にします。

Windows Azure にアプリケーションを展開するときに、Windows Azure SQL Database のクラウド データベース サービスにデータベースをデプロイします。 一時的な接続エラーは、ときに、web サーバーとデータベース サーバーに直接接続されてまとめて、同じデータ センターでのクラウド データベース サービスに接続するときに通常より頻繁に。 クラウドの web サーバーとクラウド データベース サービスが同じデータ センターでホストされている場合でも、ロード バランサーなどの問題はそれらの間の以上のネットワーク接続があります。

クラウド サービスを通常で共有されます他のユーザー、つまり、それらにより、応答性の影響を受けることができます。 調整の対象のデータベースにアクセスする場合があります。 サービス レベル アグリーメント (SLA) では許可されているよりも頻繁にアクセスが詳細にしようとすると、データベース サービスに例外がスロー手段を調整します。

クラウド サービスにアクセスするときの多くまたはほとんど接続の問題は一時的なものには、短時間で解決自体。 したがって、データベース操作をやり直してくださいは通常、一時的エラーの種類を取得すると後、少し待つと、操作が成功する可能性があります、操作を再試行でした。 ここでも、自動的に試行することで一時的なエラーを処理する場合、ユーザーのエクスペリエンスが大幅に改善を行うことができます、顧客に、これらのほとんどを非表示にします。 Entity Framework 6 で、接続の回復性機能は、SQL クエリが再試行の処理に失敗したことを自動化します。

特定のデータベース サービスの接続の回復性機能を適切に構成する必要があります。

- どの例外が一時的である可能性があります。 ネットワーク接続、一時的な損失によって発生したエラーなどのプログラムのバグによって発生したエラーいないを再試行するには。
- 失敗した操作の再試行の間隔を適切な時間待機する必要があります。 待機できますになったバッチ処理の再試行間隔よりも、応答をユーザーが待機しているオンラインの web ページのことができます。
- 再試行回数その前に、適切な数があります。 再試行回以上のオンライン アプリケーションで行う場合、バッチ処理する可能性があります。

Entity Framework プロバイダーでサポートされている任意のデータベース環境の手動でこれらの設定を構成できますが、通常 Windows Azure SQL Database を使用するオンライン アプリケーションにも適している既定値は、構成済みおよびこれらは、Contoso University アプリケーションの実装を設定します。

接続の回復を有効にするために必要なアセンブリから派生したクラスを作成するが、 [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx)クラスし、そのクラスには、SQL データベースを設定*実行戦略*EF では別の用語の*再試行ポリシー*します。

1. という名前のクラス ファイルを追加、DAL フォルダーで*SchoolConfiguration.cs*します。
2. テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework から派生したクラス内で見つかったコードを自動的に実行する`DbConfiguration`します。 使用することができます、`DbConfiguration`で行う場合はコードでの構成タスクを実行するために、 *Web.config*ファイル。 詳細については、次を参照してください。 [EntityFramework コード ベースの構成](https://msdn.microsoft.com/data/jj680699)します。
3. *StudentController.cs*、追加、`using`ステートメント`System.Data.Entity.Infrastructure`します。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. すべての変更、`catch`その catch ブロック`DataException`例外をキャッチするように`RetryLimitExceededException`例外代わりにします。 例:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    使用していた`DataException`にわかりやすい「もう一度やり直してください」のメッセージを提供するために一時的な可能性のあるエラーを特定しようとしています。 再試行ポリシーを有効にした、これで一過性のエラーの中からのみが既にがされてしようとしたや複数回失敗しましたに返される実際の例外がラップされます、`RetryLimitExceededException`例外。

詳細については、次を参照してください。 [Entity Framework 接続の回復/再試行ロジック](https://msdn.microsoft.com/data/dn456835)します。

## <a name="enable-command-interception"></a>コマンド傍受を有効にします。

これで再試行ポリシーを有効にした、方法をテストすることが、正しく動作することを確認するでしょうか。 一時的なエラーが発生、特に、ローカルで実行しているし、特に自動化された単体テストに実際の一時的なエラーを統合することは困難を強制するために簡単ではありません。 接続の回復性機能をテストするには、Entity Framework は SQL Server に送信するクエリをインターセプトし、SQL Server の応答を置き換えますが、通常は一時的な例外の種類の方法が必要です。

クラウド アプリケーションのベスト プラクティスを実装するために、クエリのインターセプトを使用することもできます:[待機時間と成功または失敗の外部サービスへのすべての呼び出しのログに記録](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)データベース サービスなどです。 EF6 の提供、[専用のログ記録 API](https://msdn.microsoft.com/data/dn469464)を簡単に、ログ記録を行うことが、チュートリアルのこのセクションで、Entity Framework の使用方法が学習[インターセプション機能](https://msdn.microsoft.com/data/dn469464)の両方を直接ログ記録と一時的なエラーをシミュレートするためです。

### <a name="create-a-logging-interface-and-class"></a>ログ記録のインターフェイスとクラスを作成します。

A[ベスト プラクティスのログ記録](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log)インターフェイスを使用して、ハード コーディング System.Diagnostics.Trace または logging クラスの呼び出しではなく。 これによりを実行する必要が生じた場合、後で、ログ記録メカニズムを変更します。 このセクションでは、ログ記録のインターフェイスとその/p を実装するクラスを作成しますように >

1. プロジェクトのフォルダーを作成し、名前*ログ*します。
2. *ログ*フォルダー、という名前のクラス ファイルを作成する*ILogger.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    ログの相対的な重要度を示す 3 つのトレース レベルと 1 つのデータベースのクエリなどの外部サービスの呼び出しの待機時間情報を提供するインターフェイスを提供します。 ログ作成メソッドがあるオーバー ロードを例外で渡すことができます。 これは、スタック トレースと内部例外を含む例外情報は、アプリケーション全体でログ記録メソッド呼び出しのたびに実行されているではなく、インターフェイスを実装するクラスによって確実に記録されるようにします。

    TraceApi メソッドでは、SQL Database などの外部サービスへの各呼び出しの待機時間を追跡することができます。
3. *ログ*フォルダー、という名前のクラス ファイルを作成する*Logger.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    実装では、System.Diagnostics を使用して、トレースを行います。 これは、簡単に生成して、トレース情報を使用する .NET の組み込み機能です。 ファイルは、たとえば、ログを書き込む、または azure blob storage に書き込んだりする System.Diagnostics トレース、使用することができます"多数"リスナーがあります。 いくつかのオプション、およびその他の詳細については、リソースへのリンクを参照してください[Visual Studio で Azure Websites のトラブルシューティング](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)します。 このチュートリアルでについてのみ取り上げますが、Visual Studio でログ**出力**ウィンドウ。

    実稼働アプリケーションで以外、System.Diagnostics トレースのパッケージを検討する可能性があり、ILogger インターフェイスにより、さまざまなトレース メカニズムに切り替える場合はそのためには比較的簡単です。

### <a name="create-interceptor-classes"></a>インターセプターのクラスを作成します。

次に、データベース、一時的なエラーをシミュレートするために 1 つとログ記録を行うクエリを送信するたびに、Entity Framework を呼び出すクラスを作成します。 これらのインターセプター クラスがから派生する必要があります、`DbCommandInterceptor`クラス。 クエリが実行されるときに自動的に呼び出されるメソッドのオーバーライドを作成します。 これらのメソッドで確認またはログ データベースに送信されるクエリ、およびデータベースに送信される前に、クエリを変更したり、何かを返す Entity Framework に自分でも、データベースにクエリを渡さずにします。

1. データベースに送信されるすべての SQL クエリがログに記録するインターセプター クラスを作成するには、という名前のクラス ファイルを作成*SchoolInterceptorLogging.cs*で、 *DAL*フォルダー、およびテンプレートのコードでは、置換次のコードでは:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    成功したクエリまたはコマンドの場合は、このコードは、待機時間情報と情報ログを書き込みます。 例外の場合は、エラー ログが作成されます。
2. "Throw"を入力すると、ダミーの一時的なエラーを生成するインターセプター クラスを作成する、**検索**という名前のクラス ファイルを作成するボックスに、 *SchoolInterceptorTransientErrors.cs* で*DAL*フォルダー、およびテンプレートのコードを次のコードに置き換えます。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    これは、コードでは唯一の上書き、`ReaderExecuting`メソッドで、複数行のデータを返すことができるクエリと呼びます。 オーバーライドも他の種類のクエリの接続の回復性を確認したい場合、`NonQueryExecuting`と`ScalarExecuting`はログ記録インターセプターとしてメソッド。

    学生のページを実行して、検索文字列として「スロー」を入力して、ときに、このコードはエラー番号 20、通常は一時的である既知の型のダミー SQL データベースの例外を作成します。 一時的なものとして認識されて、その他のエラー番号は、64、233、10053、10054、10060、10928、10929、40197、40501、および 40613 が、これらは新しいバージョンの SQL データベースで変更されます。

    コードでは、Entity framework クエリを実行して、バック クエリの結果を渡すことではなく、例外を返します。 一時的な例外が 4 回返され、コードをデータベースにクエリを渡すことの通常の手順を元に戻します。

    すべてがログに記録するために成功すると、前に 4 回のクエリを実行しようとする Entity Framework をアプリケーションの唯一の違いはクエリ結果のページを表示するために時間がかかることを確認することができます。

    Entity Framework の再試行回数が構成可能です。コードを 4 回を指定します、SQL Database の実行ポリシーの既定値であるためです。 場合は、実行ポリシーを変更する必要がありましたも変更は、ここに、コードの一時的なエラーが生成される回数を指定します。 Entity Framework がスローされますのでより多くの例外を生成するコードを変更することも、`RetryLimitExceededException`例外。

    検索ボックスに入力する値に`command.Parameters[0]`と`command.Parameters[1]`(ファースト ネームとラスト ネームに対して 1 つの 1 つ使用されます)。 値「Throw %」が見つかると、「スロー」で置き換えられますこれらのパラメーター「、」一部の受講者が検出し、返されるようにします。

    これは、アプリケーションの UI をいくつかの入力の変更に基づいて接続の回復性をテストする便利な方法だけです。 に関するコメントの後半で説明したとおりに、すべてのクエリや更新プログラムの一時的なエラーを生成するコードを記述することもできます、 *DbInterception.Add*メソッド。
3. *Global.asax*、次の追加`using`ステートメント。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. 強調表示された行を追加、`Application_Start`メソッド。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    これらの行のコードでは、Entity Framework がデータベースにクエリを送信するときに実行するインターセプター コードの原因は何です。 一時的なエラーのシミュレーションの個別のインターセプター クラスを作成したログ記録、個別に有効にして無効にするために注意します。

    使用してインターセプターを追加することができます、`DbInterception.Add`メソッドは、コードで任意の場所である必要はありません、`Application_Start`メソッド。 実行ポリシーを構成する前に作成した DbConfiguration クラスにこのコードを配置するもう 1 つの方法です。

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    場所に配置するこのコードは、実行しないように注意する`DbInterception.Add`または 1 回以上同じインターセプターの追加のインターセプター インスタンスが表示されます。 たとえば、ログ記録のインターセプターを 2 回追加すると、すべての SQL クエリの 2 つのログが表示されます。

    インターセプターは、登録の順序で実行される (順序、`DbInterception.Add`メソッドが呼び出されます)。 順序は重要でによっては、インターセプターで何をしている可能性があります。 たとえば、インターセプターを変更 SQL コマンドで取得した、`CommandText`プロパティ。 SQL コマンドを変更するは、[次へ] のインターセプターにより、変更された SQL コマンド、元の SQL コマンドではありませんが表示されます。

    UI で、別の値を入力して一時的なエラーが発生することができる方法では、一時的なエラーのシミュレーション コードを記述しました。 代わりには、常に特定のパラメーター値をチェックすることがなく一時的な例外のシーケンスを生成するインターセプター コードを記述する可能性があります。 一時的なエラーを生成する場合にのみ、インターセプターを追加できます。 これを行う場合、データベースの初期化が完了した後までインターセプター追加しないでください。 つまり、一時的なエラーの生成を開始する前に、クエリなど、エンティティ セットの 1 つ上の少なくとも 1 つのデータベースの操作を実行します。 Entity Framework がデータベースの初期化中にいくつかのクエリを実行し、ため、初期化中にエラーが発生する一貫性のない状態を取得するコンテキストのトランザクションでは実行されず。

## <a name="test-the-new-configuration"></a>新しい構成をテストします。

1. キーを押して**f5 キーを押して**デバッグ モードでアプリケーションを実行し、**学生**タブ。
2. Visual Studio を見て**出力**トレース出力を表示するウィンドウ。 一部の JavaScript エラー、ロガーによって書き込まれたログを表示するスクロールする必要があります。

    データベースに送信される実際の SQL クエリを表示できることに注意してください。 最初のクエリと Entity Framework で開始するには、データベースのバージョンをチェックするコマンドと (学習の移行に関する次のチュートリアルで) 移行履歴テーブルを参照してください。 クエリ ページング、受講者の数を確認するを参照してくださいし、受講者データを取得するクエリが最後に表示します。

    ![通常のクエリのログ記録](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. **学生**ページで、検索文字列として「スロー」を入力してをクリックして**検索**します。

    ![検索文字列をスローします。](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    ブラウザーが Entity Framework は、複数回に、クエリには再試行中の数秒間の停止していることがわかります。 最初の再試行非常には迅速に発生し、各追加の再試行する前に増加する前に待機します。 このプロセスの再試行の間隔が呼び出される前に長時間待つ*指数関数的バックオフ*します。

    ページが表示されたら、表示されている受講者を持つが名前に「、」の [出力] ウィンドウで、確認し、同じクエリで、最初に 4 回返される一時的な 5 つの時刻が試行されたことを確認します例外。 各一時的なエラーで一時的なエラーを生成するときに作成したログが表示されます、`SchoolInterceptorTransientErrors`クラス (「Returning 一時的なエラー... コマンドの」) とするときに書き込まれたログが表示されます`SchoolInterceptorLogging`例外を取得します。

    ![再試行回数を示すログ出力](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    検索文字列を入力したため、受講者データを返すクエリがパラメーター化します。

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    パラメーターの値を記録しているしないを実行できます。 パラメーターの値を表示する場合からパラメーター値を取得するログ記録コードを記述することができます、`Parameters`のプロパティ、`DbCommand`インターセプターのメソッドで取得するオブジェクト。

    アプリケーションを停止して再起動した場合を除き、このテストを繰り返すことはできませんに注意してください。 エラー カウンターをリセットするコードを記述して、アプリケーションの 1 つの実行で複数回の接続の回復性をテストできる場合は、`SchoolInterceptorTransientErrors`します。
4. 違いを確認する実行戦略 (再試行ポリシー) が、コメント、`SetExecutionStrategy`行*SchoolConfiguration.cs*、デバッグ モードで、[Students] ページを再度実行して、"Throw"再検索します。

    今回初めてクエリを実行するときにすぐに生成された最初の例外で、デバッガーが停止します。

    ![ダミーの例外](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. コメントを解除、 *SetExecutionStrategy*行*SchoolConfiguration.cs*します。

## <a name="additional-resources"></a>その他の技術情報

その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 有効な接続の回復
> * 有効なコマンド傍受
> * 新しい構成のテスト

Code First migrations と Azure の展開の詳細については、次の記事に進んでください。
> [!div class="nextstepaction"]
> [Code First migrations と Azure のデプロイ](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
