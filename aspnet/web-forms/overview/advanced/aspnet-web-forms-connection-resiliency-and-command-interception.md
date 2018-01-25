---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: "ASP.NET Web フォームの接続の回復とコマンド インターセプト |Microsoft ドキュメント"
author: Erikre
description: "このチュートリアルでは、接続の回復とコマンドの途中受信をサポートするために、サンプル アプリケーションを変更する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: e3347657fb5c7bf8c7bb4e51a2e810a1edde826a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web フォームの接続の回復性とコマンドの途中受信
====================
によって[Erik Reitan](https://github.com/Erikre)

このチュートリアルでは、接続の回復とコマンドの途中受信をサポートするために、Wingtip Toys サンプル アプリケーションを変更します。 接続の回復を有効にすると、Wingtip Toys のサンプル アプリケーションは自動的に再試行データ呼び出しクラウド環境の典型的な一時的なエラーが発生したときにします。 また、コマンドの傍受を実装すると、Wingtip Toys のサンプル アプリケーションはすべてキャッチ SQL クエリがログインするか、それらを変更するために、データベースに送信します。

> [!NOTE] 
> 
> この Web フォームのチュートリアルは、Tom Dykstra の次の MVC チュートリアルに基づくされました。  
> [接続の回復と Entity Framework、ASP.NET MVC アプリケーションでのコマンドの途中受信](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>学習する内容。

- 接続の回復を提供する方法です。
- コマンドの傍受を実装する方法です。

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、コンピューターにインストールされている次のソフトウェアがあることを確認します。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。 .NET Framework は、自動的にインストールされます。
- Wingtip Toys Wingtip Toys プロジェクト内では、このチュートリアルで説明した機能を実装できるように、プロジェクトをサンプルします。 次のリンクは、ダウンロードの詳細を提供します。

    - [Getting Started with ASP.NET 4.5.1 Web フォーム、Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- このチュートリアルを終了する前に、関連するチュートリアル シリーズを確認することを検討してください[ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)です。 チュートリアル シリーズのヘルプについて理解するには、 **WingtipToys**プロジェクトとコード。

## <a name="connection-resiliency"></a>接続の復元性

考慮すべき 1 つのオプションがデータベースを展開する Windows Azure にアプリケーションの配置を検討するときに**Windows** **Azure SQL Database**、クラウド データベース サービスです。 一時的な接続エラーは、ときに、web サーバーとデータベース サーバーは直接接続されている一緒に、同じデータ センターでよりクラウド データベース サービスに接続するときに通常より頻繁に発生します。 クラウドの web サーバーおよびデータベースのクラウド サービスが同じデータ センター内にホストされている場合でもはロード バランサーなどの問題があることができますをそれらの間のネットワーク接続の数。

また、クラウド サービスは通常共有他のユーザーによってそれらにより、応答性が低下する可能性がありされます。 データベースへのアクセスが制限されるおそれがあります。 調整の手段データベース サービスが例外をスローで許可されるよりも頻繁にアクセスしようとすると、*サービス レベル アグリーメント*(SLA)。

クラウド サービスにアクセスしているときに発生する多くのやほとんどの接続に関する問題は一時的なもの、クライアントは、短時間で解決自体です。 データベース操作を再試行は通常、一時的エラーの種類を取得すると、短い待機、および、操作が成功する可能性があります後、操作を再試行可能性があります。 自動的に読み込もうとすると、一時的なエラーを処理する場合、ユーザーのエクスペリエンスが大幅に向上を提供できますを顧客にそれらのほとんどを非表示にします。 Entity Framework 6 の接続の復元機能は、SQL クエリが再試行中の処理に失敗したことを自動化します。

特定のデータベースのサービスの接続の復元機能を適切に構成する必要があります。

1. 例外は一時的なものである可能性を知っておく必要があります。 ネットワーク接続の一時的に切断によって発生したエラーなどのプログラムのバグによって発生したエラーされませんを再試行するか。
2. 失敗した操作の再試行の間の時間の適切な量を待機する必要があります。 待機できますになったバッチ プロセス用の再試行の間隔よりも、ユーザーが応答を待つオンラインの web ページのことができます。
3. 適切な回数まで再試行する必要があるとします。 オンライン アプリケーションの場合のバッチ プロセスで複数回を再試行するか可能性があります。

Entity Framework プロバイダーでサポートされている任意のデータベース環境に、手動でこれらの設定を構成することができます。

接続の回復を有効にするために必要なのですから派生したアセンブリでクラスを作成、`DbConfiguration`クラス、およびそのクラスであり、Entity Framework での再試行ポリシーの他の語句は、SQL データベースの実行方法を設定します。

### <a name="implementing-connection-resiliency"></a>接続の回復を実装します。

1. ダウンロードして開きます、 [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409)サンプルの Visual Studio で Web フォーム アプリケーションです。
2. *ロジック*のフォルダー、 **WingtipToys**アプリケーションでは、という名前のクラス ファイルを追加*WingtipToysConfiguration.cs*です。
3. 既存のコードを次のコードで置換します。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework がから派生したクラス内で検出されたコードを自動的に実行`DbConfiguration`です。 使用することができます、`DbConfiguration`で行う場合はコードでの構成タスクを実行するクラス、 *Web.config*ファイル。 詳細については、次を参照してください。 [EntityFramework コード ベースの構成](https://msdn.microsoft.com/data/jj680699)です。

1. *ロジック*フォルダーを開き、 *AddProducts.cs*ファイル。
2. 追加、`using`の声明`System.Data.Entity.Infrastructure`黄色で強調表示されています。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. 追加、`catch`へのブロック、`AddProduct`メソッドできるように、`RetryLimitExceededException`は黄色で強調表示されたとして記録されます。   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

追加することによって、`RetryLimitExceededException`例外、ログ記録よりか場所を選択して、プロセスをもう一度やり直してください。 ユーザーにエラー メッセージを表示します。 キャッチすることにより、`RetryLimitExceededException`例外、エラーのみが一時的なする可能性がありますは既にが試行され、複数回失敗しました。 実際に返される例外でラップされます、`RetryLimitExceededException`例外。 さらに、汎用 catch ブロックも追加します。 詳細については、 `RetryLimitExceededException` 、例外を参照してください[Entity Framework 接続の回復/再試行ロジック](https://msdn.microsoft.com/data/dn456835)です。

## <a name="command-interception"></a>コマンドの途中受信

これで、再試行ポリシーを有効にした、方法をテストすること期待どおりに機能することを確認するか。 一時的なエラーが発生、特に、ローカルで実行しているし、自動化された単体テストに実際の一時的なエラーを統合することは特に困難を強制するために簡単ではありません。 接続の復元機能をテストするには、Entity Framework は SQL Server に送信されるクエリをインターセプトし、通常、一時的である例外の種類で SQL サーバーの応答を 手段が必要です。

クラウド アプリケーションのためのベスト プラクティスを実装するために、クエリの途中受信を使用することもできます: データベース サービスなどのサービスの待機時間、成功または失敗のすべての呼び出しに外部ログ。

Entity Framework のチュートリアルのこのセクションで使用する[*インターセプション機能*](https://msdn.microsoft.com/data/dn469464)のログ記録と一時的なエラーをシミュレートするための両方です。

### <a name="create-a-logging-interface-and-class"></a>ログ記録のインターフェイスとクラスを作成します。

使用して行うには、ログ記録のためのベスト プラクティス、 [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx)ハード コーディングの呼び出しではなく`System.Diagnostics.Trace`またはログ記録クラスです。 これによりを実行する必要が生じた場合は、後で、ログ記録メカニズムを変更します。 したがって、このセクションで、ログ記録のインターフェイスとそれを実装するクラスを作成します。

上記の手順に基づき、ダウンロードして開く、 **WingtipToys**サンプル Visual Studio でのアプリケーションです。

1. フォルダーを作成、 **WingtipToys**プロジェクトおよび名前を付けます*ログ*です。
2. *ログ*フォルダー、という名前のクラス ファイルを作成する*ILogger.cs*し、既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

 インターフェイスは、ログの相対的な重要度を示すために次の 3 つのトレース レベルおよびデータベース クエリなどの外部サービス呼び出しの待機時間情報を提供する 1 つを提供します。 ログ作成メソッドで例外を渡すことのできるオーバー ロードがあります。 これは、スタック トレースと内部例外を含む例外情報は、アプリケーション全体で各ログ記録のメソッドの呼び出しで実行されていることに依存せずに、インターフェイスを実装するクラスによって確実に記録されるようにします。  
  
 `TraceApi`メソッドでは、SQL データベースなどの外部サービスへの各呼び出しの待機時間を追跡できます。
3. *ログ*フォルダー、という名前のクラス ファイルを作成する*Logger.cs*し、既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

実装を使用して`System.Diagnostics`トレースを行う。 これは、簡単に生成し、トレース情報を使用する .NET の組み込み機能です。 多数ある&quot;リスナー&quot;で使用できる`System.Diagnostics`トレース、ログ ファイルを書き込む、たとえば、または、Windows azure blob ストレージに書き込むことです。 いくつかのオプション、およびその他の詳細については、リソースへのリンクで[Visual Studio で Windows Azure Web サイトをトラブルシューティング](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)です。 このチュートリアルでは、だけを見てみましょう Visual Studio でのログ**出力**ウィンドウです。

実稼働アプリケーションでトレース フレームワークを以外の使用を検討する可能性があります`System.Diagnostics`、および`ILogger`インターフェイスを使用、さまざまなトレース機構に切り替える場合はこれを行うには比較的簡単です。

### <a name="create-interceptor-classes"></a>インターセプターのクラスを作成します。

次に、データベース、一時的なエラーをシミュレートして 1 つをログ記録を行うには、クエリを送信するたびに、Entity Framework が呼び出すクラスを作成します。 これらのインターセプターのクラスがから派生する必要があります、`DbCommandInterceptor`クラスです。 クエリが実行されると自動的に呼び出されるメソッドのオーバーライドを作成します。 これらのメソッドで確認またはログがデータベースに送信されるクエリ、およびデータベースに送信される前に、クエリを変更したり、何かを返す Entity Framework を自分であってもデータベースにクエリを渡さずにします。

1. データベースに送信される前にすべての SQL クエリがログオンするインターセプター クラスを作成するには、という名前のクラス ファイルを作成*InterceptorLogging.cs*で、*ロジック*フォルダーと置換、既定のコードを次のコード:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

 成功したクエリまたはコマンドの場合は、このコードは、待機時間情報をログ記録する情報を書き込みます。 例外のエラー ログが作成されます。
2. 入力するときに、ダミーの一時的なエラーを生成するインターセプター クラスを作成する&quot;スロー&quot;で、**名前**という名前のページのテキスト ボックス*AdminPage.aspx*クラスを作成という名前のファイル*InterceptorTransientErrors.cs*で、*ロジック*フォルダーと置換、既定のコードを次のコード。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    オーバーライドのみです。 このコード、`ReaderExecuting`複数行のデータを返すことができるクエリに対して呼び出されるメソッド。 接続の回復の他の種類のクエリを確認する場合は、上書きすることでしたも、`NonQueryExecuting`と`ScalarExecuting`はログ記録インターセプターとしてメソッド、します。  
  
 "Admin"としてログオンした後で、しを選択するか、 **Admin**上部のナビゲーション バーでリンクします。 次に、 *AdminPage.aspx*という名前の製品を追加します ページ&quot;スロー&quot;です。 コードでは、エラー番号 20、通常、一時的である既知の型のダミー SQL データベースの例外を作成します。 現在一時的なものとして認識されているその他のエラー番号は、64、233、10053、10054、10060、10928、10929、40197、40501、および 40613 が、新しいバージョンの SQL データベースで変更される可能性があります。 製品の名前が"TransientErrorExample"は、次のコードで実行することができますに変更は、 *InterceptorTransientErrors.cs*ファイル。  
  
 コードは、クエリを実行して、結果の返送を渡すことではなく Entity Framework に例外を返します。 一時的な例外が返されます*4*時間、データベースにクエリを渡すことの通常のプロシージャにコードが戻されます。

    すべてのものが記録されるため、Entity Framework に成功すると、前に 4 回のクエリを実行しようとして、アプリケーションの唯一の違いはクエリ結果のページを表示するために長くかかることを確認することができます。  
  
 Entity Framework は再試行回数が構成可能です。コードは、SQL データベースの実行ポリシーの既定値であるために、4 回を指定します。 実行ポリシーを変更すると、しなければなりませんでしたも変更は、ここに、コードの一時的なエラーが生成される回数を指定します。 Entity Framework をスローするように、例外を生成するコードを変更することも、`RetryLimitExceededException`例外。
3. *Global.asax*次の追加ステートメントを使用します。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. 強調表示された行を次に、追加、`Application_Start`メソッド。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

次のコード行は、Entity Framework は、データベースにクエリを送信するときに実行するコードをインターセプターの原因です。 一時的なエラーのシミュレーションの個別のインターセプターのクラスを作成して、ログ記録を個別に有効または無効にするためすることを確認します。   
  
 使用してインターセプターを追加することができます、`DbInterception.Add`メソッド、コードで任意の場所である必要はありません、`Application_Start`メソッドです。 別のオプションでインターセプターを追加しなかった場合、`Application_Start`メソッドを更新またはという名前のクラスを追加すること*WingtipToysConfiguration.cs*のコンス トラクターの末尾には、上記のコードを配置し、`WingtipToysbConfiguration`クラスです。

場所に配置するこのコードを実行しないように注意をする`DbInterception.Add`同じインターセプターは 1 回または複数追加インターセプター インスタンスが表示されます。 たとえば、2 回ログ記録のインターセプターを追加すると、すべての SQL クエリの 2 つのログが表示されます。

インターセプターは、登録の順序で実行されます (順序、`DbInterception.Add`メソッドが呼び出されます)。 順序は重要でによっては、インターセプターで何をしている可能性があります。 たとえば、インターセプターを変更、SQL コマンドでは、取得した、`CommandText`プロパティです。 SQL コマンドを変更するは、[次へ] のインターセプターが、変更された SQL コマンドを元の SQL コマンドではなくになります。

UI で別の値を入力することで一時的なエラーが発生するように一時的なエラー シミュレーション コードを記述しました。 代わりに、常に特定のパラメーター値をチェックすることがなく一時的な例外のシーケンスを生成するインターセプター コードを記述することもできます。 一時的なエラーを生成する場合にのみにし、インターセプターを追加できます。 これを行う場合、データベースの初期化が完了した後までインターセプター追加しないただし、します。 つまり、一時的なエラーの生成を開始する前に、エンティティ セットのいずれかのクエリなど、少なくとも 1 つのデータベース操作を実行します。 Entity Framework データベースの初期化中にいくつかのクエリを実行して、これらはトランザクションでは、ため、初期化中にエラーが発生する不整合な状態を取得するコンテキスト。

## <a name="test-logging-and-connection-resiliency"></a>テストのログ記録と接続の回復性

1. Visual Studio で、キーを押して**f5 キーを押して**のデバッグ モードでは、インストールとして"Admin"、パスワードとして"Pa$ $word"を使用してログインし、アプリケーションを実行します。
2. 選択**Admin**のナビゲーション バー上部にあるからです。
3. 適切な説明、価格、およびイメージ ファイルに新しい製品"の Throw"という名前を入力します。
4. キーを押して、**製品の追加**ボタンをクリックします。  
 ブラウザーは Entity Framework でも、クエリを何回か再試行は、数秒のハングことがわかります。 最初の再試行が非常に短時間発生し、各追加の再試行する前に待機時間が増加します。 このプロセスの再試行の間隔が呼び出される前に長時間待つ*指数バックオフ*です。
5. ページが読み込む atttempting ではなくなるまで待機します。
6. プロジェクトを停止し、Visual Studio を見て**出力**トレース出力を表示するウィンドウです。 検索することができます、**出力**ウィンドウを選択して**デバッグ** - &gt; **Windows**  - &gt; **出力**です。 以前のロガーによって書き込まれたその他のいくつかのログをスクロールする必要があります。  
  
 データベースに送信される実際の SQL クエリを表示できることに注意してください。 表示一部の最初のクエリと Entity Framework では、作業を開始するコマンド データベース バージョンと移行の履歴テーブルをチェックします。   
    ![[出力] ウィンドウ](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
 アプリケーションを停止して再起動した場合を除き、このテストを繰り返すことはできませんに注意してください。 エラー カウンターをリセットするためのコードを記述することもできるように、1 つのアプリケーションに複数回接続の回復性をテストする場合は、`InterceptorTransientErrors`です。
7. 違いを確認する (再試行ポリシー) の実行方法は、コメント、`SetExecutionStrategy`線*WingtipToysConfiguration.cs*ファイルで、*ロジック*実行、フォルダー、**管理者**デバッグ モードでページをもう一度、および名前付きの製品を追加&quot;スロー&quot;もう一度です。  
  
 今度は、デバッガーを停止最初の生成された例外を初めてクエリを実行しようとするとすぐにします。  
    ![デバッグの詳細の表示](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. コメントを解除、`SetExecutionStrategy`線、 *WingtipToysConfiguration.cs*ファイル。

## <a name="summary"></a>まとめ

このチュートリアルでは、接続の回復とコマンドの途中受信をサポートするために、Web フォームのサンプル アプリケーションを変更する方法を説明しました。

## <a name="next-steps"></a>次の手順

接続の回復と ASP.NET Web フォームのコマンドの途中受信を確認した後は、ASP.NET Web フォームのトピックを確認[ASP.NET 4.5 での非同期メソッド](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md)です。 このトピックでは、Visual Studio を使用して、非同期の ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。
