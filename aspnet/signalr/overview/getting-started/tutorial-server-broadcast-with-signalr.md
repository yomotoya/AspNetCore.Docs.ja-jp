---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: "チュートリアル: サーバーは signalr 2 ブロードキャスト |Microsoft ドキュメント"
author: tdykstra
description: "このチュートリアルでは、ASP.NET SignalR 2 を使用して、サーバーのブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。 サーバーのブロードキャストでは、その commun を意味しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 98a7ce4991d58181177cf56976888e9fd1526987
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>チュートリアル: SignalR 2 でサーバーのブロードキャスト
====================
によって[Tom Dykstra](https://github.com/tdykstra)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでは、ASP.NET SignalR 2 を使用して、サーバーのブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。 サーバーのブロードキャストでは、サーバーによってクライアントに送信される通信が開始されたことを意味します。 このシナリオでは、クライアントに送信される通信が 1 つまたは複数のクライアントによって開始された、チャット アプリケーションなどのピア ツー ピア シナリオは異なるプログラミング方法が必要です。
> 
> このチュートリアルで作成するアプリケーションでは、株価情報、サーバー機能をブロードキャストの一般的なシナリオをシミュレートします。
> 
> このトピックは、Patrick Fletcher によって最初書き込まれました。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>このチュートリアルで Visual Studio 2012 を使用します。
> 
> 
> このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。
> 
> - 更新プログラム、 [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。
> - インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)です。
> - Web Platform Installer で検索し、インストール**ASP.NET と Visual Studio 2012 用 Web ツール 2013.1**です。 SignalR クラス用の Visual Studio テンプレートなどのインストールはこの**ハブ**です。
> - 一部のテンプレート (など**OWIN スタートアップ クラス**) できなくなります。 これらの場合には、クラス ファイルを代わりに使用します。
> 
> 
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="overview"></a>概要

このチュートリアルでは定期的に「プッシュ」するリアルタイム アプリケーションの担当者は、株価表示器のアプリケーションを作成したりする、接続されているすべてのクライアントにサーバーからの通知をブロードキャストします。 このチュートリアルの最初の部分では、最初からそのアプリケーションの簡素化されたバージョンを作成します。 チュートリアルの残りの部分を追加の機能を含む NuGet パッケージをインストールし、それらの機能のコードを確認します。

このチュートリアルの最初の部分でビルドしたアプリケーションには、株価データを持つグリッドが表示されます。

![StockTicker 初期バージョン](tutorial-server-broadcast-with-signalr/_static/image1.png)

定期的にサーバーをランダムに株価を更新し、更新プログラムをすべて接続されているクライアントにプッシュします。 ブラウザーの数値および内のシンボルで、**変更**と **%** 列は、サーバーからの通知に応答で動的に変化します。 同じ URL に他のブラウザーを開いた場合、同じデータとデータへの同じ変更を同時に、それらはすべて表示します。

このチュートリアルでは、次のセクションでは、含まれています。

- [前提条件](#prerequisites)
- [プロジェクトを作成します。](#createproject)
- [サーバー コードを設定します。](#server)
- [クライアント コードを設定します。](#client)
- [アプリケーションをテストします。](#test)
- [ログ記録を有効にします。](#enablelogging)
- [インストールし、完全なサンプルを StockTicker を確認](#fullsample)
- [次のステップ](#nextsteps)

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージは、Visual Studio プロジェクトにシミュレートされたサンプルの株価表示器のアプリケーションをインストールします。

> [!NOTE]
> アプリケーションのビルドの手順を実行しない場合は、新しい空の ASP.NET Web アプリケーション プロジェクトで SignalR.Sample パッケージをインストールできます。 このチュートリアルの手順を実行せず、NuGet パッケージをインストールした場合**readme.txt ファイルの指示に従う必要があります**です。 パッケージを実行するには、インストールされているパッケージで ConfigureSignalR メソッドを呼び出す OWIN スタートアップ クラスを追加する必要があります。 OWIN 起動クラスを追加しない場合、エラーが表示されます。


<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、コンピューターにインストールされている Visual Studio 2013 があることを確認します。 Visual Studio をお持ちでない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)、空き Visual Studio 2013 Express を取得します。

<a id="createproject"></a>

## <a name="create-the-project"></a>プロジェクトの作成

1. **ファイル** メニューをクリックして**新しいプロジェクト**です。
2. **新しいプロジェクト**] ダイアログ ボックスで、展開**c#** [**テンプレート**選択と**Web**です。
3. 選択、**空の ASP.NET Web アプリケーション**名では、プロジェクト テンプレートは、 *SignalR.StockTicker*、 をクリック**OK**です。

    ![[新しいプロジェクト] ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. **新しい ASP.NET**プロジェクト ウィンドウのままにして**空**を選択し、をクリックして**プロジェクトの作成**です。

    ![新しい ASP プロジェクト ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>サーバー コードを設定します。

このセクションでは、サーバーで実行されるコードを設定します。

### <a name="create-the-stock-class"></a>ストック クラスを作成します。

保存し、株式に関する情報を送信するときに使用する株価モデル クラスを作成して開始するとします。

1. プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*Stock.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    株式を作成するときに設定する 2 つのプロパティは、シンボル (たとえば、Microsoft の MSFT) 価格。 その他のプロパティは、価格を設定する方法とタイミングに依存します。 初めての価格を設定する値は取得 DayOpen に反映されます。 価格、変更を設定して PercentChange プロパティ値を計算するときに 2 回目以降は、価格と DayOpen の違いに基づいています。

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>StockTicker と StockTickerHub のクラスを作成します。

サーバーからクライアントへの対話を処理するのにには、SignalR ハブの API を使用します。 SignalR ハブ クラスから派生した StockTickerHub クラスでは、クライアントから接続し、メソッド呼び出しの受信を処理します。 また、株価データを維持し、クライアント接続とは無関係に、価格の更新プログラムを定期的にトリガーするタイマー オブジェクトを実行する必要があります。 ハブ インスタンスを一時的なために、ハブ クラスでこれらの関数を配置することはできません。 ハブで、接続と、クライアントからサーバーへの呼び出しなどの操作ごとに、ハブ クラスのインスタンスが作成されます。 株価データを保持、価格を更新し、価格の更新プログラムをブロードキャストするメカニズム StockTicker 名前別のクラスで実行するようにします。

![StockTicker からブロードキャスト](tutorial-server-broadcast-with-signalr/_static/image5.png)

のみを行う各 StockTickerHub インスタンスからシングルトン StockTicker インスタンスへの参照を設定する必要がありますので、サーバー上で実行する StockTicker クラスの 1 つのインスタンス。 StockTicker がハブ クラスではありませんが、株価データを持ちの更新をトリガーするために、クライアントにブロードキャストできる StockTicker クラスにあります。 したがって、SignalR ハブ接続のコンテキストのオブジェクトへの参照を取得する StockTicker クラスがあります。 SignalR 接続コンテキスト オブジェクトを使用して、クライアントにブロードキャストをそのことができます。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、**追加 |SignalR ハブ クラス (v2)**です。
2. 新しいハブの名前を付けます*StockTickerHub.cs*、クリックして**追加**です。 SignalR の NuGet パッケージは、プロジェクトに追加されます。
3. テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    [ハブ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)クライアントがサーバーで呼び出せるメソッドを定義するクラスを使用します。 1 つのメソッドを定義する:`GetAllStocks()`です。 クライアントは、最初に、サーバーに接続するとき、現在の価格の株式のすべての一覧を取得するには、このメソッドが呼び出されます。 メソッドは同期的に実行でき、返す`IEnumerable<Stock>`メモリからデータを返すことがあるためです。 データベースの参照など、web サービス呼び出しの待機も含まれるものの手順を実行してデータを取得するメソッドが持っているかどうかは指定`Task<IEnumerable<Stock>>`非同期処理を有効にする戻り値として。 詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーの非同期的に実行するタイミング](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)です。

    HubName 属性は、クライアントでの JavaScript コードでのハブの参照方法を指定します。 この属性を使用しない場合、クライアントに既定の名前は、ここではある stockTickerHub クラス名の camel 形式のバージョンです。

    StockTicker クラスを作成するときに、後で表示されますと静的インスタンス プロパティにそのクラスのシングルトン インスタンスが作成されます。 StockTicker のシングルトン インスタンスはクライアントの数が接続または切断してに関係なくメモリに残りますをそのインスタンスが現在の在庫情報を返す GetAllStocks メソッドの使用します。
4. プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*StockTicker.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    複数のスレッドが StockTicker コードの同じインスタンスを実行するため、スレッド セーフである StockTicker クラスがあります。

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>静的フィールドに、シングルトン インスタンスを格納します。

    コードは、初期化、静的な\_コンス トラクターがプライベートとしてマークされているために、クラス、およびこれのインスタンスとインスタンスのプロパティをバックするインスタンス フィールドが作成できるクラスの唯一のインスタンス。 [遅延初期化](https://msdn.microsoft.com/library/dd997286.aspx)の使用、\_するインスタンスの作成がスレッド セーフであることを確認しますが、パフォーマンス上の理由は、インスタンス フィールドです。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    クライアントがサーバーに接続するたびに個別のスレッドで実行される StockTickerHub クラスの新しいインスタンスする StockTickerHub クラスで既に説明したとおり StockTicker シングルトン インスタンスが、StockTicker.Instance 静的プロパティから取得します。

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>ConcurrentDictionary の株価データを格納します。

    コンス トラクターは、\_のいくつかのサンプルの株価データと GetAllStocks 株式コレクションには、株式が返されます。 前述の株式のこのコレクションがさらに、サーバー クラスのメソッド、ハブ クライアントが呼び出すことができるは StockTickerHub.GetAllStocks によって返されます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    株式コレクションとは見なさ、 [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)スレッド セーフの型。 代わりに、使用する可能性があります、[ディクショナリ](https://msdn.microsoft.com/library/xfhwa508.aspx)オブジェクトを明示的にそれを変更するときに、ディクショナリをロックします。

    このサンプル アプリケーションは [ok] をメモリ内でアプリケーション データを格納し、StockTicker インスタンスが破棄されるときに、データが失われるです。 実際のアプリケーションでは、データベースなどのバックエンド データ ストアで作業する場合します。

    ### <a name="periodically-updating-stock-prices"></a>株価を定期的に更新

    コンス トラクターを定期的にランダムに株価を更新するメソッドを呼び出すタイマー オブジェクトが開始されます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices は state パラメーターに null を渡すと、タイマーによって呼び出されます。 価格を更新する前に、ロックを取得、 \_updateStockPricesLock オブジェクト。 コードは、別のスレッドが価格を更新中で既にし、一覧には、各銘柄の TryUpdateStockPrice が呼び出すかどうかを確認します。 TryUpdateStockPrice メソッドは、株価を変更するかどうかを決定し、これを変更する量。 株価を変更すると、株式の価格の変更をすべて接続されているクライアントにブロードキャストする BroadcastStockPrice が呼び出されます。

    \_UpdatingStockPrices フラグ マークが付いている[揮発性](https://msdn.microsoft.com/library/x13ttww7.aspx)へのアクセスがスレッド セーフであることを確認します。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    TryUpdateStockPrice メソッドで実際のアプリケーションで、価格を検索する web サービスの呼び出しはこのコードでは、ランダムに変更するのに乱数ジェネレーターを使用します。

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>StockTicker クラスは、クライアントにブロードキャストできるように、SignalR コンテキストを取得します。

    価格の変動は発信 StockTicker オブジェクトで次に、ためこれは、接続されているすべてのクライアントで、updateStockPrice メソッドを呼び出す必要があるオブジェクト。 ハブ クラスで、クライアントのメソッド呼び出しの API があるが、StockTicker ハブ クラスから派生していないと、ハブ オブジェクトへの参照はありません。 したがって、接続しているクライアントにブロードキャストするためには、StockTickerHub クラスの SignalR コンテキストのインスタンスを取得し、クライアントでメソッドの呼び出しに使用するに StockTicker クラスがあります。

    コードが、コンス トラクターにパスを参照する、シングルトン クラス インスタンスを作成するとき、SignalR コンテキストへの参照を取得し、コンス トラクターは、クライアントのプロパティに格納します。

    2 つの理由がコンテキストの 1 回だけ取得する理由がある: コンテキストを取得することはコストのかかる操作、および 1 回取得はによりそのクライアントに送信されたメッセージの目的の順序は維持されます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    コンテキストのクライアントのプロパティを取得して、StockTickerClient プロパティに配置したり、ハブ クラスと同じように見える同じメソッドを呼び出すクライアント コードを記述することができます。 たとえば、すべてのクライアントにブロードキャストする Clients.All.updateStockPrice(stock) を記述できます。

    BroadcastStockPrice で呼び出している updateStockPrice メソッドがまだ存在しません。後で追加するクライアントで実行されるコードを記述するときにします。 Clients.All は動的で、実行時に式が評価されることを意味するので、updateStockPrice ここを参照できます。 このメソッドの呼び出しが実行されると、SignalR はクライアントに送信メソッド名とパラメーターの値、および updateStockPrice をという名前のメソッドで、クライアントは、そのメソッドが呼び出されることに渡されるパラメーターの値。

    Clients.All では、すべてのクライアントに送信を意味します。 SignalR では、その他のオプションのクライアントまたはクライアントに送信するグループを指定できます。 詳細については、次を参照してください。 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)です。

### <a name="register-the-signalr-route"></a>SignalR のルートを登録します。

サーバーを途中受信、および SignalR への直接 URL を知っている必要があります。 追加する操作と OWIN スタートアップ クラスします。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN 起動クラス**です。 クラスの名前を付けます**Startup.cs**です。
2. コードに置き換えます**Startup.cs**以下にします。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

サーバー コードのセットアップが完了しました。 次のセクションでは、クライアントを設定します。

<a id="client"></a>

## <a name="set-up-the-client-code"></a>クライアント コードを設定します。

1. プロジェクト フォルダーに新しい HTML ファイルを作成し、名前*StockTicker.html*です。
2. テンプレート コードを次のコードに置き換えます。

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    HTML では、5 つの列、ヘッダー行では、5 つすべての列にまたがる 1 つのセルを持つデータ行とテーブルを作成します。 データ行は、「読み込み中...」が表示され、アプリケーションの起動時にすぐにのみ表示されます。 JavaScript コードはその行を削除して、サーバーから取得した株価データをその場所の行に追加します。

    スクリプト タグは、jQuery スクリプト ファイル、SignalR core スクリプト ファイル、SignalR プロキシ スクリプト ファイル、および後で作成する StockTicker スクリプト ファイルを指定します。 「/Signalr ハブ」URL を指定するには、SignalR プロキシ スクリプト ファイルは、動的に生成され、StockTickerHub.GetAllStocks をここでは、ハブ クラスのメソッドのプロキシ メソッドを定義します。 使用してこの JavaScript ファイルが手動で生成できる場合は、 [SignalR ユーティリティ](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)MapHubs メソッドの呼び出しでの動的なファイルの作成を無効にするとします。
3. > [!IMPORTANT]
 > JavaScript ファイルを参照するかどうかを確認*StockTicker.html*が正しい。 必ず、スクリプト タグ (1.10.2 の例) で jQuery のバージョンがプロジェクトの jQuery のバージョンと同じ*スクリプト*フォルダー、し、スクリプト タグの SignalR バージョンが、SignalR と同じであるかどうかを確認プロジェクトのバージョン*スクリプト*フォルダーです。 必要な場合は、スクリプト タグのファイル名を変更します。
4. **ソリューション エクスプ ローラー**を右クリックして*StockTicker.html*、クリックして**スタート ページとして設定**です。
5. プロジェクト フォルダーで新しい JavaScript ファイルを作成し、名前*StockTicker.js*.
6. テンプレート コードを次のコードに置き換えます。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection は、SignalR のプロキシを参照します。 コードでは、StockTickerHub クラスのプロキシへの参照を取得し、ティッカー変数に格納します。 プロキシの名前は、[HubName] 属性によって設定された名前を示します。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    すべての変数と関数を定義したら、ファイル内のコードの最後の行 SignalR start 関数を呼び出すことによって、SignalR 接続を初期化します。 Start 関数が非同期的に実行しを返します、 [jQuery 延期オブジェクト](http://api.jquery.com/category/deferred-object/)、することができる非同期操作が完了したときに呼び出す関数を指定する元に戻す関数を呼び出すことができます.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    Init 関数では、サーバーで getAllStocks 関数を呼び出し、在庫テーブルを更新するサーバーが返される情報を使用します。 既定では、ある camel 形式大文字小文字の区別、クライアントで、メソッド名は pascal 形式のサーバーで使用することを確認します。 Camel 形式の大文字と小文字の規則は、メソッド、オブジェクトではなくのみに適用されます。 たとえば、在庫を参照してください。シンボルと在庫です。価格、いない stock.symbol または stock.price です。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    クライアントでは、pascal 文字種を使用するかと同じ方法 HubMethodName 属性を持つハブ メソッドを装飾する可能性がありますが完全に別のメソッド名を使用する場合は、HubName 属性を持つハブ クラス自体が修飾されています。

    ストックのオブジェクトの書式設定のプロパティを呼び出し元 formatStock によって、サーバーから受け取ったストック オブジェクトごとに、初期化メソッドで、テーブルの行の HTML が作成され、置き換えるを呼び出した (の上部に定義されている*StockTicker.js*) オブジェクトのストック プロパティの値を持つ rowTemplate 変数内のプレース ホルダーを置換します。 結果の HTML は、株価のテーブルに追加されます。

    非同期開始関数が完了した後に実行されるコールバック関数として渡すことによって初期化を呼び出します。 Init を呼び出すと、開始の呼び出し後に別の JavaScript ステートメントは start 関数を接続の確立を完了を待たずにすぐに実行があるため、関数は失敗します。 その場合は、初期化関数を関数を呼び出す getAllStocks サーバー接続が確立される前に試行します。

    サーバーには、株式の価格が変更された、ときに、接続しているクライアントで、updateStockPrice を呼び出します。 関数は、使用できるように、呼び出しに、サーバーから stockTicker プロキシのクライアントのプロパティに追加されます。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    UpdateStockPrice 関数は、テーブルの行に初期化関数と同じよう、サーバーから受け取ったストック オブジェクトを書式設定します。 ただし、テーブルに行を追加するのではなく、テーブル内の株式の現在の行を検索し、その行を新しいものに置き換えます。

<a id="test"></a>

## <a name="test-the-application"></a>アプリケーションをテストする

1. F5 キーを押してアプリケーションをデバッグ モードで実行します。

    ストックの表は最初に、「読み込み中...」行、し、初期の株価データを表示すると、短い遅延後に表示し、株式の価格を変更します。

    ![[読み込み中]](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![初期の在庫テーブル](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![サーバーからの変更を受け取るストック テーブル](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. ブラウザーのアドレス バーから URL をコピーし、1 つまたは複数の新しいブラウザー ウィンドウに貼り付けます。

    最初の株価表示は、最初のブラウザーと同じとの変更が同時に発生します。
3. すべてのブラウザーを閉じて新しいブラウザー ウィンドウで、し同じ URL に移動します。

    StockTicker シングルトン オブジェクトは、在庫テーブルが表示された、株式が変更を続けているため、サーバーで実行を続けています。 (図を変更する 0 に初期のテーブルを参照しない)。
4. ブラウザーを閉じます。

<a id="enablelogging"></a>

## <a name="enable-logging"></a>ログの有効化

SignalR には、トラブルシューティングに支援するためにクライアントを有効にできる組み込みのログ記録機能があります。 このセクションでは、ログ記録を有効にして、ログ確認方法を次のトランスポート メソッドの SignalR を使用するかを示す例を参照してください。

- [Websocket](http://en.wikipedia.org/wiki/WebSocket)IIS 8 と現在のブラウザーでサポートされています。
- [サーバー送信イベント](http://en.wikipedia.org/wiki/Server-sent_events)、Internet Explorer 以外のブラウザーでサポートされています。
- [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、Internet Explorer でサポートされていません。
- [Ajax ロング ポーリング](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、すべてのブラウザーでサポートされています。

特定の接続には、SignalR は、サーバーとクライアントの両方をサポートする最適な転送方法を選択します。

1. 開いている*StockTicker.js*し、ファイルの最後の接続を初期化するコードの直前にログ記録を有効にするコードの行を追加します。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. F5 キーを押してプロジェクトを実行します。
3. ブラウザーの開発者ツール ウィンドウを開き、ログを表示するコンソールを選択します。 新しい接続の転送方法をネゴシエートする Signalr のログを表示するページを更新する必要があります。

    Windows 8 (IIS 8) で Internet Explorer 10 を実行している場合は、Websocket はトランスポート メソッドです。

    ![IE 10 IIS 8 をコンソールします。](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Windows 7 (IIS 7.5) で Internet Explorer 10 を実行している場合は、iframe はトランスポート メソッドです。

    ![IE 10 コンソールで、IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    Firefox の場合、Firebug アドインのインストールをコンソール ウィンドウを取得します。 Windows 8 (IIS 8) で Firefox 19 を実行している場合は、Websocket はトランスポート メソッドです。

    ![Firefox 19 IIS 8 Websocket](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Windows 7 (IIS 7.5) で Firefox 19 を実行している場合、トランスポート メソッドは、サーバーによって送信されるイベントです。

    ![Firefox 19 IIS 7.5 コンソールします。](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>インストールし、完全なサンプルを StockTicker を確認

インストールされている StockTicker アプリケーション、 [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージには、最初から作成した簡略化されたバージョンより多くの機能が含まれています。 チュートリアルのこのセクションでは、NuGet パッケージをインストールし、新機能とそれらを実装するコードを確認します。 このチュートリアルの前半の手順を実行せず、パッケージをインストールする場合は、プロジェクトに OWIN スタートアップ クラスを追加する必要があります。 この手順は、NuGet パッケージの readme.txt ファイルで説明します。

### <a name="install-the-signalrsample-nuget-package"></a>SignalR.Sample NuGet パッケージをインストールします。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、 **NuGet パッケージの管理**です。
2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**オンライン**、入力*SignalR.Sample*で、**オンラインで検索**ボックスし、[ ]をクリックして**インストール**で、 **SignalR.Sample**パッケージです。

    ![SignalR.Sample パッケージをインストールします。](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. **ソリューション エクスプ ローラー**、展開、 *SignalR.Sample* SignalR.Sample パッケージをインストールすることによって作成されたフォルダーです。
4. *SignalR.Sample*フォルダーを右クリックして*StockTicker.html*、順にクリック**スタート ページとして設定**です。

    > [!NOTE]
    > SignalR.Sample NuGet をインストールするパッケージは内にある jQuery のバージョンを変更可能性があります、*スクリプト*フォルダーです。 新しい*StockTicker.html*でパッケージをインストールするファイル、 *SignalR.Sample*フォルダーは、元のを実行する場合は、パッケージでインストールされるjQueryのバージョンとの同期になります*StockTicker.html*ファイルを再び、まず、スクリプト タグ内の jQuery 参照を更新する必要があります。

### <a name="run-the-application"></a>アプリケーションの実行

1. F5 キーを押してアプリケーションを実行します。

    先ほど見たグリッド、だけでなく完全株価表示器のアプリケーションは同じ株価データを表示するウィンドウを水平方向にスクロールを表示します。 初めてアプリケーションを実行して「市場」は"closed"静的グリッドとスクロールのないするティッカー ウィンドウを参照してください。

    ![StockTicker 画面の開始](tutorial-server-broadcast-with-signalr/_static/image14.png)

    クリックすると**Open Market**、 **Live ストック ティッカー**を水平方向にスクロール ボックスを起動し、ランダムに株価変更を定期的にブロードキャストするサーバーを起動します。 株価されるたびに変更されると、両方、**在庫表のライブ**グリッドと**Live 株式相場表示**ボックスの一覧を更新します。 背景が緑色の株式の価格の変化が正の値と、在庫は表示し、背景が赤の在庫が示すように変更が負の場合は、します。

    ![StockTicker アプリ市場を開く](tutorial-server-broadcast-with-signalr/_static/image15.png)

    **閉じる市場**ボタンは、変更を停止し、ティッカー スクロールが停止され、**リセット**価格の変更開始する前に、ボタンが初期状態にすべての株価データをリセットします。 複数のブラウザー ウィンドウを開き、同じ URL に移動すると、各ブラウザーで同時に動的に更新される、同じデータを参照してください。 ボタンをクリックすると、すべてのブラウザーは、同時に同じ方法を応答します。

### <a name="live-stock-ticker-display"></a>ライブの株式相場表示

**Live ストック ティッカー**表示は、単一行に CSS スタイルでフォーマットされた div 要素の順序なしのリスト。 ティッカーは初期化され、テーブルと同様の更新: 内のプレース ホルダーを置き換えることで、 &lt;li&gt;テンプレート文字列と動的に追加する、 &lt;li&gt;要素を&lt;ul&gt;要素。 スクロールは、位置の div 内で順序付けられていないリストの左余白を変更するため、jQuery アニメーション関数を使用して実行します。

株価表示器 HTML で:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

株価表示器 CSS で:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

これは、jQuery コードがスクロールします。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>クライアントが呼び出すことができるサーバーの追加のメソッド

StockTickerHub クラスでは、クライアントが呼び出すことができる 4 つの追加方法を定義します。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

OpenMarket、CloseMarket、およびリセットは、ページの上部にあるボタンへの応答で呼び出されます。 これらには、すべてのクライアントにすぐに反映される状態の変更をトリガーする 1 台のクライアントのパターンを示しています。 これらの各メソッドのメソッドを呼び出す StockTicker クラス市場の状態を変更して、新しい状態をブロードキャストし、その効果。

StockTicker クラスでは、市場の状態は MarketState 列挙型値を返す MarketState プロパティで確保されます。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

各市場の状態を変更する方法は、ロック ブロックの内部 StockTicker クラスにはスレッド セーフであるため。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

このコードが、スレッド セーフであることを確認、 \_MarketState プロパティをバックする marketState フィールドが、volatile としてマークされています。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

BroadcastMarketStateChange と BroadcastMarketReset メソッドは、点を除いて、クライアント側で定義されているさまざまなメソッドを呼び出すことが既に説明した BroadcastStockPrice メソッドに似ています。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>サーバーが呼び出すことができるクライアントの追加の関数

UpdateStockPrice 関数は、グリッドと相場表示の両方を処理を使用して jQuery.Color を赤と緑の色をフラッシュします。

新しい関数*SignalR.StockTicker.js*の有効化および無効にするボタンがに基づいて市場の状態や、停止や、ティッカー ウィンドウ水平方向のスクロールを開始します。 複数の関数は ticker.client に追加されているため、 [jQuery 拡張関数](http://api.jquery.com/jQuery.extend/)に追加するために使用します。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>接続を確立した後の他のクライアントのセットアップ

いくつか追加の作業を行うには、クライアント接続を確立した後: 相場が開くか、適切な marketOpened または marketClosed 関数を呼び出すし、ボタンにサーバーのメソッド呼び出しをアタッチするために閉じるかどうかを確認します。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

サーバー メソッドがないワイヤード (有線) までボタンまで接続が確立されると、メソッド呼び出しは、サーバーは使用前にしようとすることはできません、コードをします。

<a id="nextsteps"></a>

## <a name="next-steps"></a>次の手順

このチュートリアルでは、接続されているすべてのクライアント、定期的と任意のクライアントからの通知に応答の両方に、サーバーからメッセージをブロードキャストする SignalR アプリケーションをプログラミングする方法を学びました。 マルチ スレッドのシングルトン インスタンスを使用して、サーバーの状態を保持するパターンも、マルチ プレーヤーのオンライン ゲーム シナリオの使用もできます。 例については、次を参照してください。 [SignalR に基づいている ShootR ゲーム](https://github.com/NTaylorMullen/ShootR)です。

ピア ツー ピア通信シナリオを示すチュートリアルについては、次を参照してください。 [SignalR の概要](introduction-to-signalr.md)と[SignalR でリアルタイムの更新](tutorial-high-frequency-realtime-with-signalr.md)です。

SignalR 開発のより高度な概念については、SignalR のソース コードおよびリソースの次のサイトを参照してください。

- [ASP.NET SignalR](../../index.md)
- [SignalR Project](http://signalr.net/)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

SignalR アプリケーションを Azure にデプロイする方法のチュートリアルについては、次を参照してください。 [Azure App service Web アプリを使用してを使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)です。 Visual Studio web プロジェクトを Windows Azure Web サイトを展開する方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。
