---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'チュートリアル: SignalR 2 によるサーバー ブロードキャスト |Microsoft Docs'
author: tdykstra
description: このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a243c78c7d552f1c82a88c6083871fcd16538618
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837430"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>チュートリアル: SignalR 2 によるブロードキャスト サーバー

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。 サーバー ブロードキャストでは、サーバーがクライアントに送信される通信を開始することを意味します。

このチュートリアルで作成するアプリケーションでは、株価情報、サーバー ブロードキャストの機能の一般的なシナリオをシミュレートします。 定期的に、サーバーはランダムに株価を更新し、接続されているすべてのクライアントに、更新プログラムをブロードキャストします。 ブラウザー、数字、およびシンボルで、**変更**と**%** 通知をサーバーからの応答で列を動的に変更します。 同じ URL に他のブラウザーを開いた場合、同じデータやデータに同じ変更を同時にすべて表示します。

![Web を作成します。](tutorial-server-broadcast-with-signalr/_static/image1.png)

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * プロジェクトの作成
> * サーバー コードを設定します。
> * サーバー コードを調べる
> * クライアント コードを設定します。
> * クライアント コードを調べる
> * アプリケーションをテストする
> * ログの有効化

> [!IMPORTANT]
> アプリケーションの構築の手順を実行しない場合は、新しい空の ASP.NET Web アプリケーション プロジェクトで SignalR.Sample パッケージをインストールできます。 このチュートリアルの手順を実行せず、NuGet パッケージをインストールした場合の手順に従ってください必要があります、 *readme.txt*ファイル。 OWIN startup を追加する必要があるパッケージを実行するには、クラスの呼び出し、`ConfigureSignalR`インストールされたパッケージ内のメソッド。 OWIN startup クラスを追加しない場合、エラーが表示されます。 参照してください、 [StockTicker のサンプルをインストール](#install-the-stockticker-sample)この記事の「します。


## <a name="prerequisites"></a>必須コンポーネント

 * [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。

## <a name="create-the-project"></a>プロジェクトの作成

このセクションでは、Visual Studio 2017 を使用して、空の ASP.NET Web アプリケーションを作成する方法を示します。

1. Visual Studio で ASP.NET Web アプリケーションを作成します。

    ![Web を作成します。](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. **新しい ASP.NET Web アプリケーション - SignalR.StockTicker**  ウィンドウのままに**空**を選択し、選択**OK**。

## <a name="set-up-the-server-code"></a>サーバー コードを設定します。

このセクションでは、サーバーで実行されるコードを設定します。

### <a name="create-the-stock-class"></a>在庫クラスを作成します。

まずを作成、 *Stock*モデル クラスを格納および転送については、在庫を使用します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **クラス**します。

1. クラスの名前*Stock*し、プロジェクトに追加します。

1. コードに置き換えます、 *Stock.cs*このコード ファイル。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    素材を作成するときに設定する 2 つのプロパティは`Symbol`(たとえば、Microsoft の MSFT) と`Price`します。 その他のプロパティが設定する方法とタイミングに依存`Price`します。 初めて設定する`Price`に、値が伝達される`DayOpen`します。 その後、設定すると`Price`、アプリを計算、`Change`と`PercentChange`プロパティ値の差に基づいて`Price`と`DayOpen`します。

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>StockTickerHub と StockTicker クラスを作成します。

サーバーからクライアントへの対話を処理するために、SignalR Hub API を使用します。 A`StockTickerHub`から派生したクラス、`SignalRHub`クラスはクライアントからの受信接続とメソッドの呼び出しを処理します。 株価データを管理および実行する必要も、`Timer`オブジェクト。 `Timer`オブジェクトが価格の更新プログラムのクライアント接続の独立したを定期的にトリガーされます。 これらの関数を配置することはできません、`Hub`クラス、ハブには、一時的なためです。 アプリを作成、`Hub`接続と、クライアントからサーバーへの呼び出しのように、ハブの各タスクのクラスのインスタンス。 株価データを保持し、価格を更新、価格の更新プログラムにブロードキャスト メカニズムでは、別のクラスで実行します。 クラスは、名前を付けます`StockTicker`します。

![StockTicker からブロードキャスト](tutorial-server-broadcast-with-signalr/_static/image3.png)

1 つのインスタンスが欲しい、`StockTicker`それぞれからの参照を設定する必要がありますので、サーバー上で実行するにはクラス`StockTickerHub`シングルトン インスタンス`StockTicker`インスタンス。 `StockTicker`クラスは、株価データと、更新をトリガーすることから、クライアントにブロードキャストする必要がありますが、`StockTicker`されていない、`Hub`クラス。 `StockTicker`クラスは、SignalR ハブの接続コンテキスト オブジェクトへの参照を取得する必要があります。 クライアントにブロードキャストする SignalR 接続のコンテキスト オブジェクトを使用できます。

#### <a name="create-stocktickerhubcs"></a>StockTickerHub.cs を作成します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。

1. **新しい項目の追加 - SignalR.StockTicker**、**インストール済み** > **Visual C#**   >  **Web** >  **SignalR**選び**SignalR ハブ クラス (v2)** します。

1. クラスの名前*StockTickerHub*し、プロジェクトに追加します。

    この手順で作成、 *StockTickerHub.cs*クラス ファイル。 同時に、プロジェクトに SignalR をサポートする一連のスクリプト ファイルとアセンブリ参照を追加します。

1. コードに置き換えます、 *StockTickerHub.cs*このコード ファイル。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. ファイルを保存します。

アプリでは、[ハブ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)メソッドは、クライアントは、サーバー上で呼び出すことができますを定義するクラス。 1 つのメソッドを定義する:`GetAllStocks()`します。 クライアントは、最初に、サーバーに接続するときは、その現在の価格の株式のすべての一覧を取得するには、このメソッドを呼び出します。 メソッドは同期的に実行して、返す`IEnumerable<Stock>`メモリからデータを返すためです。

指定したかどうか、メソッドがデータベース検索など、web サービス呼び出しの待機を含むは何かの手順を実行してデータを取得する必要がある`Task<IEnumerable<Stock>>`非同期処理を有効にする戻り値として。 詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーの非同期的に実行するタイミング](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)します。

`HubName`属性は、アプリが、クライアントでの JavaScript コードのハブを参照する方法を指定します。 クライアントに既定の名前は、この属性を使用しない場合は、例ではこのクラスの名前のキャメル ケース バージョン`stockTickerHub`します。

後述するように作成するとき、`StockTicker`クラス、アプリは静的でそのクラスのシングルトン インスタンスを作成`Instance`プロパティ。 シングルトン インスタンスが`StockTicker`はクライアントの数が接続または切断に関係なく、メモリ内にします。 そのインスタンスがどのような`GetAllStocks()`メソッドを使用して現在の株価情報を返します。

#### <a name="create-stocktickercs"></a>StockTicker.cs を作成します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **クラス**します。

1. クラスの名前*StockTicker*し、プロジェクトに追加します。

1. コードに置き換えます、 *StockTicker.cs*このコード ファイル。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

すべてのスレッドが StockTicker コードの同じインスタンスを実行するため StockTicker クラスはスレッド セーフであることにあります。

### <a name="examine-the-server-code"></a>サーバー コードを調べる

サーバー コードを確認する場合に役立つアプリの動作を理解します。

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>静的フィールドに、シングルトン インスタンスを格納します。

静的な初期化`_instance`をバックするフィールド、`Instance`クラスのインスタンスのプロパティ。 コンス トラクターはプライベートなので、アプリを作成できるクラスの唯一のインスタンスになります。 アプリは[遅延初期化](/dotnet/framework/performance/lazy-initialization)の`_instance`フィールド。 パフォーマンス上の理由はありません。 インスタンスの作成はスレッド セーフであるかどうかを確認するになります。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

クライアントがサーバーに接続するたびに個別のスレッドで実行される StockTickerHub クラスの新しいインスタンスがから StockTicker のシングルトン インスタンスを取得する、`StockTicker.Instance`の静的プロパティを見た前に、`StockTickerHub`クラス。

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>ConcurrentDictionary の株価データを格納します。

コンス トラクターによって初期化、`_stocks`をサンプルの株価データ コレクションと`GetAllStocks`株式を返します。 既に説明したよう株式のこのコレクションがによって返される`StockTickerHub.GetAllStocks`、メソッドは、サーバーで、`Hub`クライアントが呼び出すことができるクラス。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

Stocks コレクションとは見なさ、 [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)スレッド セーフの型。 別の方法として使用できます、[ディクショナリ](https://msdn.microsoft.com/library/xfhwa508.aspx)オブジェクトし、それを変更するときに明示的にディクショナリをロックします。

このサンプル アプリケーションは [ok] メモリ内でアプリケーション データを保存して、アプリを破棄しますと、データが失われる、`StockTicker`インスタンス。 実際のアプリケーションでは、データベースのようなバックエンド データ ストアと動作します。

#### <a name="periodically-updating-stock-prices"></a>株価を定期的に更新

コンス トラクターは、の起動時、`Timer`を定期的にランダムな単位で株価を更新するメソッドを呼び出すオブジェクト。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` 呼び出し`UpdateStockPrices`、state パラメーターで null を渡します。 価格を更新する前に、アプリは、ロック取得、`_updateStockPricesLock`オブジェクト。 別のスレッドが価格を更新中で既にかどうかと、続いて、コードを確認します`TryUpdateStockPrice`の一覧で、各株にします。 `TryUpdateStockPrice`メソッドは、株価を変更するかどうかを決定し、これを変更する量。 株式の価格が変更された場合、アプリが呼び出す`BroadcastStockPrice`接続されているクライアントをすべてに株価の変更をブロードキャストします。

`_updatingStockPrices`フラグが指定されている[揮発性](https://msdn.microsoft.com/library/x13ttww7.aspx)はスレッド セーフであるかどうかを確認します。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

実際のアプリケーションで、`TryUpdateStockPrice`メソッドは、価格を検索する web サービスを呼び出すとします。 このコードでは、アプリは、ランダムに変更するのに乱数ジェネレーターを使用します。

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>StockTicker クラスは、クライアントにブロードキャストできるように、SignalR のコンテキストを取得します。

料金の変更がここで送信されるため、`StockTicker`オブジェクトを呼び出す必要があるオブジェクトは、`updateStockPrice`接続されているすべてのクライアントのメソッド。 `Hub`クラス、クライアントのメソッドを呼び出すための API は用意したが、`StockTicker`から派生していない、`Hub`クラスし、への参照がありません。`Hub`オブジェクト。 接続されているクライアントは、ブロードキャスト、`StockTicker`クラスの SignalR コンテキスト インスタンスを取得するには、`StockTickerHub`クラスし、クライアントでメソッドの呼び出しに使用します。

コードが、コンス トラクターに渡すを参照する、シングルトン クラス インスタンスを作成するときに、SignalR コンテキストへの参照を取得し、コンス トラクター内に配置、`Clients`プロパティ。

理由は 2 つのコンテキストを 1 回だけ取得する理由: コンテキストの取得は、高価なタスクでありを指定すると、アプリで、クライアントに送信されるメッセージの目的の順序を維持すると、それを取得します。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

取得、`Clients`プロパティのコンテキストと基本的には、`StockTickerClient`プロパティでは、クライアント メソッドを呼び出すコードの場合と同様、同じ外観を記述することができます、`Hub`クラス。 たとえば、すべてのクライアントにブロードキャストすることが書き込み`Clients.All.updateStockPrice(stock)`します。

`updateStockPrice`で呼び出しているメソッド`BroadcastStockPrice`がまだ存在しません。 後で追加するクライアントで実行されるコードを記述するときにします。 参照することができます`updateStockPrice`ここため`Clients.All`は動的で、アプリが実行時に式を評価することを意味します。 このメソッドの呼び出しが実行されると、SignalR は送信メソッド名とパラメーターの値をクライアントにクライアントがという名前のメソッドと`updateStockPrice`アプリはそのメソッドを呼び出すし、パラメーターの値を渡します。

`Clients.All` 意味は、すべてのクライアントに送信します。 SignalR では、その他のオプションのクライアントまたはクライアントに送信するグループを指定できます。 詳細については、次を参照してください。 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)します。

### <a name="register-the-signalr-route"></a>SignalR のルートを登録します。

サーバーは、途中受信および SignalR への直接する URL を把握する必要があります。 そのためには、OWIN startup クラスを追加します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。

1. **新しい項目の追加 - SignalR.StockTicker**選択**インストール済み** > **Visual C#**   >  **Web**と選び**OWIN Startup クラス**します。

1. クラスの名前*スタートアップ*選択と**OK**します。

1. 既定のコードを置き換える、 *Startup.cs*このコード ファイル。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

サーバー コードの設定を今すぐが完了しました。 次のセクションでは、クライアントを設定します。

## <a name="set-up-the-client-code"></a>クライアント コードを設定します。

このセクションでは、クライアントで実行されるコードを設定します。

### <a name="create-the-html-page-and-javascript-file"></a>ページの HTML と JavaScript ファイルを作成します。

データを HTML ページが表示され、JavaScript ファイルは、データを整理します。

#### <a name="create-stocktickerhtml"></a>StockTicker.html を作成します。

まず、HTML クライアントを追加します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **HTML ページ**します。

1. ファイルに名前を*StockTicker*選択と**OK**します。

1. 既定のコードを置き換える、 *StockTicker.html*このコード ファイル。

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    HTML では、5 つの列、ヘッダー行では、5 つの列にまたがる 1 つのセルを持つデータ行とテーブルを作成します。 「読み込み中...」アプリの起動時に一時的にデータ行を示しています。 JavaScript コードはその行を削除して、サーバーから取得した株価データでその場所の行に追加します。

    スクリプト タグを指定します。

    * JQuery スクリプト ファイル。

    * SignalR コア スクリプト ファイル。

    * SignalR プロキシ スクリプト ファイルです。

    * 後で作成する StockTicker スクリプト ファイル。

    アプリは、SignalR プロキシ スクリプト ファイルを動的に生成されます。 「/Signalr ハブ」の URL を指定し、Hub クラスは、ここでは、上のメソッドをプロキシのメソッドを定義します。、`StockTickerHub.GetAllStocks`します。 使用してこの JavaScript ファイルが手動で生成できる場合は、 [SignalR ユーティリティ](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)します。 動的ファイルの作成を無効にすることを忘れないでください、`MapHubs`メソッドの呼び出し。

1. **ソリューション エクスプ ローラー**、展開**スクリプト**します。

    JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。

    > [!IMPORTANT]
    > パッケージ マネージャーは、以降のバージョンの SignalR スクリプトでインストールされます。

1. プロジェクト内のスクリプト ファイルのバージョンに対応するコード ブロック内のスクリプト参照を更新します。

1. **ソリューション エクスプ ローラー**、右クリックして*StockTicker.html*、し、**スタート ページとして設定**します。

#### <a name="create-stocktickerjs"></a>StockTicker.js を作成します。

JavaScript ファイルを作成します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **JavaScript ファイル**します。

1. ファイルに名前を*StockTicker*選択と**OK**します。

1. このコードを追加、 *StockTicker.js*ファイル。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>クライアント コードを調べる

クライアント コードを確認する場合に役立つクライアント コードがアプリケーションを動作させるサーバー コードと対話する方法について説明します。

#### <a name="starting-the-connection"></a>接続の開始

`$.connection` SignalR プロキシを参照します。 コードのプロキシへの参照を取得する、`StockTickerHub`クラスし、内に配置、`ticker`変数。 プロキシの名前が名前によって設定された、`HubName`属性。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

SignalR を呼び出すことによって、ファイル内のコードの最後の行が SignalR 接続を初期化しますすべての変数と関数を定義した後`start`関数。 `start`関数が非同期的に実行し、返します、 [jQuery 遅延オブジェクト](http://api.jquery.com/category/deferred-object/)します。 アプリケーションが非同期のアクションが完了したときに呼び出す関数を指定する元に戻す関数を呼び出すことができます。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>すべての素材を取得します。

`init`関数呼び出し、`getAllStocks`サーバー上の機能を在庫テーブルを更新するサーバーが返す情報を使用しています。 、既定では、あるメソッド名は pascal 形式で表記するサーバーの場合でも、クライアントにキャメル ケースを使用することを確認します。 キャメル ケースのルールは、オブジェクトではなく、メソッドにのみ適用されます。 たとえばを参照してください`stock.Symbol`と`stock.Price`ではなく、`stock.symbol`または`stock.price`します。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

`init`メソッドでは、アプリで、呼び出すことによって、サーバーから受信した各株オブジェクトのテーブル行に HTML が作成されます`formatStock`の書式設定のプロパティを`stock`オブジェクトし、を呼び出した`supplant`内のプレースホルダーを置換するには`rowTemplate`変数で、`stock`オブジェクト プロパティの値。 結果の HTML は、株価のテーブルに追加されます。

> [!NOTE]
> 呼び出す`init`としてそれに渡すことによって、`callback`後非同期に実行される関数`start`関数が完了するとします。 呼び出した場合`init`呼び出した後、別の JavaScript ステートメントとして`start`、完了、接続を確立するのには start 関数を待たずにすぐに実行があるため、関数は失敗します。 その場合は、`init`関数は呼び出しを試みる、`getAllStocks`関数アプリは、サーバー接続を確立する前にします。

#### <a name="getting-updated-stock-prices"></a>更新の株価の取得

呼び出すサーバー株の価格が変更されたとき、`updateStockPrice`接続されているクライアントにします。 アプリのクライアントのプロパティに関数を追加する、`stockTicker`使用できるようにする呼び出しをサーバーからのプロキシ。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

`updateStockPrice`関数形式のテーブルに、サーバーから受信した株価オブジェクトの行と同様、`init`関数。 テーブルに行を追加することではなく、テーブル内の株式の現在の行を検索し、その行を新しいに置き換えます。

## <a name="test-the-application"></a>アプリケーションをテストする

確認するためのアプリをテストすることができますが動作します。 株価の変動をライブの株価のテーブルを表示するすべてのブラウザー ウィンドウが表示されます。

1. ツールバーで、有効にする**スクリプトのデバッグ**しデバッグ モードでアプリを実行する [再生] ボタンを選択します。

    ![ユーザー モードをデバッグし、[再生] を選択のスクリーン ショット。](tutorial-server-broadcast-with-signalr/_static/image4.png)

    表示するブラウザー ウィンドウが開き、**在庫表の Live**します。 ストックの表に、最初に行が示されます「読み込み中...」、その後、しばらくすると、アプリは、初期の株価データを表示し、株価を起動して変更します。

1. ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。

    初期の株価表示は、最初のブラウザーと同じと同時に変更が行われます。

1. すべてのブラウザーを閉じ、新しいブラウザーを開いて、同じ URL に移動します。

    StockTicker シングルトン オブジェクトは、サーバーで実行する継続します。 **在庫表の Live**株式が変更し続けていることを示しています。 図形を変更、初期テーブルのゼロは表示されません。

1. ブラウザーを閉じます。

## <a name="enable-logging"></a>ログの有効化

SignalR では、トラブルシューティングで支援するために、クライアントで有効にできる組み込みのログ記録関数があります。 このセクションでは、ログ記録を有効にし、ログを知る方法を次のトランスポート メソッドの SignalR を使用して表示する例を参照してください。

* [Websocket](http://en.wikipedia.org/wiki/WebSocket)IIS 8 と現在のブラウザーでサポートされていません。

* [サーバー送信イベント](http://en.wikipedia.org/wiki/Server-sent_events)、Internet Explorer 以外のブラウザーでサポートされていません。

* [フレームの永久](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、Internet Explorer でサポートされていません。

* [長いポーリングの Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、すべてのブラウザーでサポートされていません。

特定の接続では、SignalR は、サーバーとクライアントの両方をサポートする最適なトランスポートの方法を選択します。

1. 開いている*StockTicker.js*します。

1. ファイルの最後に、接続を初期化するコードの直前のログ記録を有効にするコードの強調表示された行を追加します。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. **F5** キーを押してプロジェクトを実行します。

1. ブラウザーの開発者ツール ウィンドウを開きし、ログを表示するコンソールを選択します。 新しい接続の転送方法をネゴシエートする SignalR のログを表示するページを更新する必要があります。

    * Windows 8 (IIS 8) で Internet Explorer 10 を実行している場合、トランスポート メソッドが**Websocket**します。

    * Windows 7 (IIS 7.5) で Internet Explorer 10 を実行している場合、トランスポート メソッドが**iframe**します。

    * Windows 8 (IIS 8) で Firefox 19 を実行している場合、トランスポート メソッドが**Websocket**します。

        > [!TIP]
        > Firefox の場合、コンソール ウィンドウを取得する Firebug アドインのインストールします。

    * Windows 7 (IIS 7.5) で Firefox 19 を実行している場合、トランスポート メソッドが**サーバーによって送信される**イベント。

## <a name="install-the-stockticker-sample"></a>StockTicker サンプルをインストールします。

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) StockTicker アプリケーションをインストールします。 NuGet パッケージには、ゼロから作成された簡略化されたバージョンより多くの機能が含まれています。 チュートリアルのこのセクションでは、NuGet パッケージをインストールし、新機能とそれらを実装するコードを確認します。

> [!IMPORTANT]
> このチュートリアルの前の手順を実行せず、パッケージをインストールする場合は、プロジェクトに OWIN startup クラスを追加する必要があります。 NuGet パッケージの場合は、この readme.txt ファイルには、この手順について説明します。

### <a name="install-the-signalrsample-nuget-package"></a>SignalR.Sample NuGet パッケージをインストールします。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[NuGet パッケージの管理]** を選択します。

1. **NuGet パッケージ マネージャー。SignalR.StockTicker**、**参照**します。

1. **パッケージ ソース**、 **nuget.org**します。

1. 入力*SignalR.Sample*検索ボックスを選び**Microsoft.AspNet.SignalR.Sample** > **インストール**します。

1. **ソリューション エクスプ ローラー**、展開、 *SignalR.Sample*フォルダー。

    SignalR.Sample パッケージをインストールすると、フォルダーとその内容が作成されます。

1. *SignalR.Sample*フォルダーを右クリックして*StockTicker.html*、し、**スタート ページとして設定**します。

    > [!NOTE]
    > SignalR.Sample NuGet のインストール パッケージは内にある jQuery のバージョンを変更可能性があります、*スクリプト*フォルダー。 新しい*StockTicker.html*でパッケージをインストールするファイル、 *SignalR.Sample*フォルダーが、元のを実行する場合は、パッケージをインストールするjQueryバージョンとの同期に*StockTicker.html*ファイルを再び、最初にスクリプト タグ内の jQuery 参照を更新する必要があります。

### <a name="run-the-application"></a>アプリケーションの実行

 初めてのアプリで紹介したテーブルには、便利な機能が必要があります。 完全な株価表示器のアプリケーションの新機能を示しています。 株価データおよび増加し、分類の色を変更する株価を表示するウィンドウを水平方向にスクロールします。

1. **F5 キー**を押してアプリを実行します。

     初めてアプリを実行して、「市場」は"closed"静的なテーブルとティッカー ウィンドウ スクロールはありませんを参照してください。

1. 選択**Open Market**します。

    ![ライブ ティッカーのスクリーン ショット。](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * **株式ティッカーの Live**を水平方向にスクロール ボックスを起動し、サーバーがランダムに株価の変更を定期的にブロードキャストする開始します。

    * 株式の価格が変更されるたびに、アプリでは、両方を更新、**在庫表の Live**と**株式ティッカーの Live**します。

    * 株式の価格の変更は、正の値は、アプリには、背景が緑の在庫が表示されます。

    * 変更が負の値と、アプリには、背景が赤の素材が表示されます。

1. 選択**市場を閉じる**します。

    * 表に、更新が停止します。

    * スクロール、ティッカーを停止します。

1. 選択**リセット**します。

    * すべての株価データはリセットされます。

    * アプリでは、料金の変更を開始する前に初期状態を復元します。

1. ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。

1. 各ブラウザーで同時に動的に更新される同じデータを表示します。

1. コントロールのいずれかを選択するとすべてのブラウザーと同じ方法を同時に応答します。

### <a name="live-stock-ticker-display"></a>ライブの株式相場表示

**株式ティッカーの Live**ディスプレイが順不同のリストで、`<div>`要素が 1 行に CSS スタイルで書式設定します。 アプリを初期化し、テーブルと同様、ティッカーの更新: 内のプレース ホルダーを置き換えることで、`<li>`テンプレート文字列と動的に追加する、`<li>`要素を`<ul>`要素。 アプリでは、jQuery を使用してスクロール`animate`関数内で順序付けられていない一覧の左余白を変更するため、`<div>`します。

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

株価情報の HTML コード:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

株式ティッカーの CSS コード:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

JQuery コードがスクロールします。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>クライアントが呼び出すことができる、サーバーで追加のメソッド

アプリに柔軟性を追加するには、追加のメソッドが、アプリが呼び出すことができます。

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

`StockTickerHub`クラスは、クライアントが呼び出すことができる 4 つの追加のメソッドを定義します。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

アプリによる呼び出し`OpenMarket`、 `CloseMarket`、および`Reset`ページの上部にあるボタンに応答します。 これらは、すべてのクライアントに直ちに伝達の状態に変更をトリガーする 1 つのクライアントのパターンを示します。 これらの各メソッドでメソッドを呼び出し、`StockTicker`市場の状態の変更の原因し、新しい状態をブロードキャストするクラス。

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

`StockTicker`クラス、アプリは、市場の状態を保持、`MarketState`を返すプロパティを`MarketState`列挙値。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

ロック ブロック内ではそれぞれの市場の状態を変更する方法、`StockTicker`クラスはスレッド セーフにするには。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

このコードはスレッド セーフかどうかを確認する、`_marketState`をバックするフィールド、`MarketState`プロパティが指定されている`volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

`BroadcastMarketStateChange`と`BroadcastMarketReset`メソッドは、クライアントで定義されている別のメソッドを呼び出す点を除いて、既に説明しましたが BroadcastStockPrice メソッドに似ています。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>サーバーが呼び出すことができるクライアントの追加の関数

`updateStockPrice`関数は、テーブルとティッカーの表示の両方を処理するため、`jQuery.Color`を赤と緑の色をフラッシュします。

新しい関数で*SignalR.StockTicker.js*を有効にして、市場の状態に基づいてボタンを無効にします。 これらも停止または開始、**株式ティッカーの Live**水平方向にスクロールします。 多くの関数に追加されているため`ticker.client`、アプリでは、 [jQuery 関数を拡張する](http://api.jquery.com/jQuery.extend/)に追加します。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>接続の確立後に追加のクライアントのセットアップ

クライアント接続を確立した後、追加の作業を行うがあります。

* 市場が開いているか、適切な呼び出しを閉じてとかどうか`marketOpened`または`marketClosed`関数。

* ボタンにサーバー メソッドの呼び出しをアタッチします。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

サーバーのメソッドは、アプリが接続を確立した後でまでボタンまでがワイヤード (有線) はありません。 これは、コードが利用可能になる前にサーバーのメソッドを呼び出すことはできませんのでです。

## <a name="additional-resources"></a>その他の技術情報

このチュートリアルでは、接続されているすべてのクライアントをサーバーからメッセージをブロードキャストする SignalR アプリケーションをプログラミングする方法を学習できました。 ここで任意のクライアントからの通知に応答して定期的にメッセージをブロードキャストすることができます。 マルチ スレッドのシングルトン インスタンスの概念は、マルチ プレーヤー オンライン ゲーム シナリオでサーバーの状態を維持するために使用できます。 例については、次を参照してください。 [SignalR に基づいて ShootR ゲーム](https://github.com/NTaylorMullen/ShootR)します。

ピア ツー ピア通信シナリオを示すチュートリアルについては、次を参照してください。 [SignalR の概要](introduction-to-signalr.md)と[SignalR によるリアルタイムの更新](tutorial-high-frequency-realtime-with-signalr.md)します。

詳細については、SignalR は、次のリソースを参照してください。

* [ASP.NET SignalR](../../index.md)
* [SignalR プロジェクト](http://signalr.net/)
* [SignalR GitHub とサンプル](https://github.com/SignalR/SignalR)
* [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * プロジェクトの作成
> * サーバー コードを設定します。
> * サーバー コードを調べる
> * クライアント コードを設定します。
> * クライアント コードを調べる
> * アプリケーションのテスト
> * ログ記録を有効になっています。

ASP.NET SignalR 2 を使用するリアルタイムの web アプリケーションを作成する方法については、次の記事に進んでください。
> [!div class="nextstepaction"]
> [SignalR によるリアルタイムの web アプリを作成します。](real-time-web-applications-with-signalr.md)