---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web フォームの接続復元性とコマンド傍受 |Microsoft Docs
author: Erikre
description: このチュートリアルでは、接続の回復性とコマンド傍受をサポートするためにサンプル アプリケーションを変更する方法について説明します。
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 039923a91d957765fa8b2c0cfe11abc8790c1e88
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828100"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web フォームの接続復元性とコマンド傍受
====================
によって[Erik Reitan](https://github.com/Erikre)

このチュートリアルでは、接続復元性とコマンド傍受をサポートするために、Wingtip Toys のサンプル アプリケーションを変更します。 接続の回復を有効にすると、Wingtip Toys のサンプル アプリケーションは自動的に再試行データ呼び出し、クラウド環境の一般的な一時的なエラーが発生します。 また、コマンド インターセプションを実装すると、Wingtip Toys のサンプル アプリケーションはキャッチ オール SQL クエリのログまたはそれらを変更するには、データベースに送信します。

> [!NOTE] 
> 
> この Web フォームのチュートリアルが Tom Dykstra の次の MVC のチュートリアルに基づいています。  
> [接続復元性と、ASP.NET MVC アプリケーションで Entity Framework とコマンド傍受](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>学習内容。

- 接続の回復性を提供する方法。
- コマンドのインターセプトを実装する方法。

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、コンピューターにインストールされている、次のソフトウェアがあることを確認します。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。 .NET Framework は、自動的にインストールされます。
- Wingtip Toys では、Wingtip Toys プロジェクト内では、このチュートリアルで説明されている機能を実装できるように、プロジェクトがサンプルです。 次のリンクは、ダウンロードの詳細を提供します。

    - [Getting Started with ASP.NET 4.5.1 Web フォームの Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- このチュートリアルを完了する前に、関連するチュートリアル シリーズの確認を検討する[ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)します。 チュートリアルのシリーズはよく理解する際に役立つ、 **WingtipToys**プロジェクトとコード。

## <a name="connection-resiliency"></a>接続の復元性

考慮すべき 1 つのオプションがデータベースを展開する Windows Azure にアプリケーションの配置を検討するときに**Windows** **Azure SQL Database**、クラウド データベース サービス。 一時的な接続エラーは、ときに、web サーバーとデータベース サーバーに直接接続されてまとめて、同じデータ センターでのクラウド データベース サービスに接続するときに通常より頻繁に。 クラウドの web サーバーとクラウド データベース サービスが同じデータ センターでホストされている場合でも、ロード バランサーなどの問題はそれらの間の以上のネットワーク接続があります。

クラウド サービスを通常で共有されます他のユーザー、つまり、それらにより、応答性の影響を受けることができます。 調整の対象のデータベースにアクセスする場合があります。 手段を調整するデータベース サービスが例外をスローで許可されているより頻繁にアクセスしようとすると、*サービス レベル アグリーメント*(SLA)。

クラウド サービスにアクセスするときに発生する多くのやほとんどの接続の問題は一時的なものには、短時間で解決自体。 したがって、データベース操作をやり直してくださいは通常、一時的エラーの種類を取得すると後、少し待つと、操作が成功する可能性があります、操作を再試行でした。 ここでも、自動的に試行することで一時的なエラーを処理する場合、ユーザーのエクスペリエンスが大幅に改善を行うことができます、顧客に、これらのほとんどを非表示にします。 Entity Framework 6 で、接続の回復性機能は、SQL クエリが再試行の処理に失敗したことを自動化します。

特定のデータベース サービスの接続の回復性機能を適切に構成する必要があります。

1. どの例外が一時的である可能性があります。 ネットワーク接続、一時的な損失によって発生したエラーなどのプログラムのバグによって発生したエラーいないを再試行するには。
2. 失敗した操作の再試行の間隔を適切な時間待機する必要があります。 待機できますになったバッチ処理の再試行間隔よりも、応答をユーザーが待機しているオンラインの web ページのことができます。
3. 再試行回数その前に、適切な数があります。 再試行回以上のオンライン アプリケーションで行う場合、バッチ処理する可能性があります。

これらの設定、Entity Framework プロバイダーでサポートされている任意のデータベース環境を手動で構成できます。

必要な接続の回復を有効にするためには、アセンブリから派生したクラスを作成、`DbConfiguration`クラスし、そのクラスでは、Entity Framework では再試行ポリシーの別の用語ですが、SQL Database 実行戦略を設定します。

### <a name="implementing-connection-resiliency"></a>接続の回復性を実装します。

1. ダウンロードして開きます、 [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409)サンプル Visual Studio で Web フォーム アプリケーションです。
2. *ロジック*のフォルダー、 **WingtipToys**アプリケーション、という名前のクラス ファイルを追加*WingtipToysConfiguration.cs*します。
3. 既存のコードを次のコードで置換します。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework から派生したクラス内で見つかったコードを自動的に実行する`DbConfiguration`します。 使用することができます、`DbConfiguration`で行う場合はコードでの構成タスクを実行するために、 *Web.config*ファイル。 詳細については、次を参照してください。 [EntityFramework コード ベースの構成](https://msdn.microsoft.com/data/jj680699)します。

1. *ロジック*フォルダーを開き、 *AddProducts.cs*ファイル。
2. 追加、`using`ステートメント`System.Data.Entity.Infrastructure`黄色で強調表示されています。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. 追加、`catch`へのブロック、`AddProduct`メソッドように、`RetryLimitExceededException`は黄色で強調表示されているとして記録されます。   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

追加することで、`RetryLimitExceededException`例外、ログ記録より提供したり、プロセスをもう一度やり直してください。 選択できるユーザーにエラー メッセージが表示されます。 キャッチすることにより、`RetryLimitExceededException`例外、エラーのみが一時的である可能性がありますが既にしようとしたされ複数回失敗しました。 返される実際の例外にラップされます、`RetryLimitExceededException`例外。 さらに、汎用の catch ブロックも追加します。 詳細については、 `RetryLimitExceededException` 、例外を参照してください[Entity Framework 接続の回復/再試行ロジック](https://msdn.microsoft.com/data/dn456835)します。

## <a name="command-interception"></a>コマンド傍受

これで再試行ポリシーを有効にした、方法をテストすることが、正しく動作することを確認するでしょうか。 一時的なエラーが発生、特に、ローカルで実行しているし、特に自動化された単体テストに実際の一時的なエラーを統合することは困難を強制するために簡単ではありません。 接続の回復性機能をテストするには、Entity Framework は SQL Server に送信するクエリをインターセプトし、SQL Server の応答を置き換えますが、通常は一時的な例外の種類の方法が必要です。

クラウド アプリケーションのベスト プラクティスを実装するために、クエリのインターセプトを使用することもできます。 ログの待機時間と成功または失敗をすべての呼び出しを外部サービスのデータベース サービスなどです。

このチュートリアルのこのセクションでは、Entity Framework を使用します[*インターセプション機能*](https://msdn.microsoft.com/data/dn469464)のログ記録と一時的なエラーをシミュレートするための両方。

### <a name="create-a-logging-interface-and-class"></a>ログ記録のインターフェイスとクラスを作成します。

ログ記録のためのベスト プラクティスを使用して行う、 [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx)への呼び出しをハード コーディングするのではなく`System.Diagnostics.Trace`またはログ記録クラス。 これによりを実行する必要が生じた場合、後で、ログ記録メカニズムを変更します。 したがって、このセクションで、ログ記録のインターフェイスとそれを実装するクラスを作成します。

上記の手順に基づいて、ダウンロードして開く、 **WingtipToys**サンプル Visual Studio でのアプリケーション。

1. フォルダーを作成、 **WingtipToys**プロジェクトし、名前を付けます*ログ*します。
2. *ログ*フォルダー、という名前のクラス ファイルを作成*ILogger.cs*既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   ログの相対的な重要度を示す 3 つのトレース レベルと 1 つのデータベースのクエリなどの外部サービスの呼び出しの待機時間情報を提供するインターフェイスを提供します。 ログ作成メソッドがあるオーバー ロードを例外で渡すことができます。 これは、スタック トレースと内部例外を含む例外情報は、アプリケーション全体でログ記録メソッド呼び出しのたびに実行されているではなく、インターフェイスを実装するクラスによって確実に記録されるようにします。  
  
   `TraceApi`メソッドでは、SQL Database などの外部サービスへの各呼び出しの待機時間を追跡できます。
3. *ログ*フォルダー、という名前のクラス ファイルを作成*Logger.cs*既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

実装を使用して`System.Diagnostics`トレースを実行します。 これは、簡単に生成して、トレース情報を使用する .NET の組み込み機能です。 たくさん&quot;リスナー&quot;で使用できる`System.Diagnostics`トレース、または Windows azure blob storage に書き込んだりするなど、ファイルにログを書き込む。 いくつかのオプション、およびその他の詳細については、リソースへのリンクを参照してください[Visual Studio で Windows Azure Web サイトをトラブルシューティング](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio)します。 このチュートリアルを見るだけで、Visual Studio でログ**出力**ウィンドウ。

実稼働アプリケーションで他にもトレース フレームワークの使用を検討する可能性があります`System.Diagnostics`、および`ILogger`インターフェイスにより、さまざまなトレース メカニズムに切り替える場合はそのためには比較的簡単です。

### <a name="create-interceptor-classes"></a>インターセプターのクラスを作成します。

次に、これは一時的なエラーをシミュレートしてログ記録を行うのデータベースにクエリを送信するたびに、Entity Framework を呼び出すクラスを作成します。 これらのインターセプター クラスがから派生する必要があります、`DbCommandInterceptor`クラス。 それでは、クエリが実行されるときに自動的に呼び出されるメソッドのオーバーライドを作成します。 これらのメソッドで確認またはログ データベースに送信されるクエリ、およびデータベースに送信される前に、クエリを変更したり、何かを返す Entity Framework に自分でも、データベースにクエリを渡さずにします。

1. データベースに送信される前にすべての SQL クエリをログするインターセプター クラスを作成するには、という名前のクラス ファイルを作成*InterceptorLogging.cs*で、*ロジック*フォルダーと置換、既定のコードで、次のコード:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   成功したクエリまたはコマンドの場合は、このコードは、待機時間情報と情報ログを書き込みます。 例外の場合は、エラー ログが作成されます。
2. 入力すると、ダミーの一時的なエラーを生成するインターセプター クラスを作成する&quot;スロー&quot;で、**名前**という名前のページ上のテキスト ボックス*AdminPage.aspx*クラスを作成という名前のファイル*InterceptorTransientErrors.cs*で、*ロジック*フォルダーと置換、既定のコードを次のコード。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    これは、コードでは唯一の上書き、`ReaderExecuting`メソッドで、複数行のデータを返すことができるクエリと呼びます。 オーバーライドも他の種類のクエリの接続の回復性を確認したい場合、`NonQueryExecuting`と`ScalarExecuting`はログ記録インターセプターとしてメソッド。  
  
   後で、"Admin"としてログインし、選択、**管理者**上部のナビゲーション バーのリンク。 次に、 *AdminPage.aspx*という製品を追加します ページ&quot;スロー&quot;します。 コードは、エラー番号 20、通常は一時的である既知の型のダミー SQL データベースの例外を作成します。 一時的なものとして認識されて、その他のエラー番号は、64、233、10053、10054、10060、10928、10929、40197、40501、および 40613 が、これらは新しいバージョンの SQL データベースで変更されます。 製品の名前が"TransientErrorExample"は、次のコードで実行することができますに変更は、 *InterceptorTransientErrors.cs*ファイル。  
  
   コードでは、Entity framework クエリを実行して、結果の返送を渡すことではなく、例外を返します。 一時的な例外が返されます*4*回後、コードがデータベースにクエリを渡すことの通常のプロシージャに戻されます。

    すべてがログに記録するために成功すると、前に 4 回のクエリを実行しようとする Entity Framework をアプリケーションの唯一の違いはクエリ結果のページを表示するために時間がかかることを確認することができます。  
  
   Entity Framework の再試行回数が構成可能です。コードを 4 回を指定します、SQL Database の実行ポリシーの既定値であるためです。 場合は、実行ポリシーを変更する必要がありましたも変更は、ここに、コードの一時的なエラーが生成される回数を指定します。 Entity Framework がスローされますのでより多くの例外を生成するコードを変更することも、`RetryLimitExceededException`例外。
3. *Global.asax*次を追加するステートメントを使用します。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. 強調表示された行を次に、追加、`Application_Start`メソッド。  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

これらの行のコードでは、Entity Framework がデータベースにクエリを送信するときに実行するインターセプター コードの原因は何です。 一時的なエラーのシミュレーションの個別のインターセプター クラスを作成したログ記録、個別に有効にして無効にするために注意します。   
  
 使用してインターセプターを追加することができます、`DbInterception.Add`メソッドは、コードで任意の場所である必要はありません、`Application_Start`メソッド。 別のオプションでインターセプターを追加しなかった場合、`Application_Start`方法は更新するかという名前のクラスを追加すること*WingtipToysConfiguration.cs*のコンス トラクターの末尾には、上記のコードを配置し、`WingtipToysbConfiguration`クラス。

場所に配置するこのコードは、実行しないように注意する`DbInterception.Add`または 1 回以上同じインターセプターの追加のインターセプター インスタンスが表示されます。 たとえば、ログ記録のインターセプターを 2 回追加すると、すべての SQL クエリの 2 つのログが表示されます。

インターセプターは、登録の順序で実行される (順序、`DbInterception.Add`メソッドが呼び出されます)。 順序は重要でによっては、インターセプターで何をしている可能性があります。 たとえば、インターセプターを変更 SQL コマンドで取得した、`CommandText`プロパティ。 SQL コマンドを変更するは、[次へ] のインターセプターにより、変更された SQL コマンド、元の SQL コマンドではありませんが表示されます。

UI で、別の値を入力して一時的なエラーが発生することができる方法では、一時的なエラーのシミュレーション コードを記述しました。 代わりには、常に特定のパラメーター値をチェックすることがなく一時的な例外のシーケンスを生成するインターセプター コードを記述する可能性があります。 一時的なエラーを生成する場合にのみ、インターセプターを追加できます。 これを行う場合、データベースの初期化が完了した後までインターセプター追加しないでください。 つまり、一時的なエラーの生成を開始する前に、クエリなど、エンティティ セットの 1 つ上の少なくとも 1 つのデータベースの操作を実行します。 Entity Framework がデータベースの初期化中にいくつかのクエリを実行し、ため、初期化中にエラーが発生する一貫性のない状態を取得するコンテキストのトランザクションでは実行されず。

## <a name="test-logging-and-connection-resiliency"></a>テストのログ記録と接続の回復性

1. Visual Studio で、キーを押して**F5**のデバッグ モードでは、インストール"Admin"は"Pa$ $word"を使用して、パスワードとしてとしてログインし、アプリケーションを実行します。
2. 選択**管理者**上部にあるナビゲーション バーから。
3. 適切な説明、価格、およびイメージ ファイルを使用して、新しい製品を「スロー」という名前を入力します。
4. キーを押して、**追加製品**ボタンをクリックします。  
   ブラウザーが Entity Framework は、複数回に、クエリには再試行中の数秒間の停止していることがわかります。 最初の再試行は非常に迅速に発生し、各追加の再試行する前に、待機が増加します。 このプロセスの再試行の間隔が呼び出される前に長時間待つ*指数関数的バックオフ*します。
5. ページが読み込む atttempting でされなくなるまで待ちます。
6. プロジェクトを停止して、Visual Studio を見て**出力**トレース出力を表示するウィンドウ。 検索することができます、**出力**ウィンドウを選択して**デバッグ** - &gt; **Windows**  - &gt; **出力**します。 ロガーによって書き込まれたその他のいくつかのログをスクロールする必要があります。  
  
   データベースに送信される実際の SQL クエリを表示できることに注意してください。 表示いくつかの最初のクエリと Entity Framework が開始するのにがコマンド データベース バージョンと移行履歴テーブルをチェックします。   
    ![[出力] ウィンドウ](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   アプリケーションを停止して再起動した場合を除き、このテストを繰り返すことはできませんに注意してください。 エラー カウンターをリセットするコードを記述して、アプリケーションの 1 つの実行で複数回の接続の回復性をテストできる場合は、`InterceptorTransientErrors`します。
7. 違いを確認する実行戦略 (再試行ポリシー) が、コメント、`SetExecutionStrategy`行*WingtipToysConfiguration.cs*ファイル、*ロジック*実行、フォルダー、**管理者**デバッグ モードでページをもう一度、および名前付きの製品を追加&quot;スロー&quot;もう一度です。  
  
   今回初めてクエリを実行するときにすぐに生成された最初の例外で、デバッガーが停止します。  
    ![デバッグ-詳細の表示](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. コメントを解除、`SetExecutionStrategy`行、 *WingtipToysConfiguration.cs*ファイル。

## <a name="summary"></a>まとめ

このチュートリアルでは、接続の回復性とコマンド傍受をサポートするために Web フォームのサンプル アプリケーションを変更する方法を説明しました。

## <a name="next-steps"></a>次の手順

接続復元性と ASP.NET Web フォームで、コマンド インターセプションを確認してから、ASP.NET Web フォームのトピックを確認してください。 [ASP.NET 4.5 での非同期メソッド](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md)します。 トピックでは、Visual Studio を使用して非同期の ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。
